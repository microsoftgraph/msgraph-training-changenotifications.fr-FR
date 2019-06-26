<!-- markdownlint-disable MD002 MD041 -->

Les abonnements pour les notifications expirent et doivent être renouvelés régulièrement. Les étapes suivantes montrent comment renouveler les notifications.

Les **contrôleurs ouverts > fichier NotificationsController.cs**

Ajoutez les deux déclarations de membre suivantes à `NotificationsController` la classe:

```csharp
private static Dictionary<string, Subscription> Subscriptions = new Dictionary<string, Subscription>();
private static Timer subscriptionTimer = null;
```

Ajoutez les nouvelles méthodes suivantes. Ces éléments implémenteront un minuteur d’arrière-plan qui s’exécutera toutes les 15 secondes pour vérifier si les abonnements ont expiré. Si c’est le cas, ils seront renouvelés.

```csharp
private void CheckSubscriptions(Object stateInfo)
{
  AutoResetEvent autoEvent = (AutoResetEvent)stateInfo;

  Console.WriteLine($"Checking subscriptions {DateTime.Now.ToString("h:mm:ss.fff")}");
  Console.WriteLine($"Current subscription count {Subscriptions.Count()}");

  foreach(var subscription in Subscriptions)
  {
    // if the subscription expires in the next 2 min, renew it
    if(subscription.Value.ExpirationDateTime < DateTime.UtcNow.AddMinutes(2))
    {
      RenewSubscription(subscription.Value);
    }
  }
}

private void RenewSubscription(Subscription subscription)
{
  Console.WriteLine($"Current subscription: {subscription.Id}, Expiration: {subscription.ExpirationDateTime}");

  var graphServiceClient = GetGraphClient();

  subscription.ExpirationDateTime = DateTime.UtcNow.AddMinutes(5);

  var foo = graphServiceClient
    .Subscriptions[subscription.Id]
    .Request()
    .UpdateAsync(subscription).Result;

  Console.WriteLine($"Renewed subscription: {subscription.Id}, New Expiration: {subscription.ExpirationDateTime}");
}
```

La `CheckSubscriptions` méthode est appelée toutes les 15 secondes par le minuteur. Pour l’utilisation de la production, cette valeur doit être définie sur une valeur plus raisonnable afin de réduire le nombre d’appels inutiles à Microsoft Graph.

La `RenewSubscription` méthode renouvelle un abonnement et n’est appelée que si un abonnement arrivera à expiration dans les deux minutes à venir.

Recherchez la méthode `Get()` et remplacez-la par le code suivant:

```csharp
[HttpGet]
public ActionResult<string> Get()
{
    var graphServiceClient = GetGraphClient();

    var sub = new Microsoft.Graph.Subscription();
    sub.ChangeType = "updated";
    sub.NotificationUrl = config.Ngrok + "/api/notifications";
    sub.Resource = "/users";
    sub.ExpirationDateTime = DateTime.UtcNow.AddMinutes(5);
    sub.ClientState = "SecretClientState";

    var newSubscription = graphServiceClient
      .Subscriptions
      .Request()
      .AddAsync(sub).Result;

    Subscriptions[newSubscription.Id] = newSubscription;

    if(subscriptionTimer == null)
    {
        subscriptionTimer = new Timer(CheckSubscriptions, null, 5000, 15000);
    }

    return $"Subscribed. Id: {newSubscription.Id}, Expiration: {newSubscription.ExpirationDateTime}";
}
```

Ajoutez l’instruction suivante après les instructions `using` existantes en haut du fichier:

```csharp
using System.Threading;
```

### <a name="test-the-changes"></a>Testez les modifications:

Dans Visual Studio code, sélectionnez **débogage > démarrer** le débogage pour exécuter l’application.
Accédez à l’URL suivante: **http://localhost:5000/api/notifications**. Cette opération permet d’enregistrer un nouvel abonnement.

Dans la fenêtre de la **console** de débogage de code Visual Studio, toutes les 15 secondes, Notez que le minuteur vérifiant que l’abonnement est expiré:

```shell
Checking subscriptions 12:32:51.882
Current subscription count 1
```

Patientez quelques minutes et vous verrez ce qui suit lorsque l’abonnement a besoin d’être renouvelé:

```shell
Renewed subscription: 07ca62cd-1a1b-453c-be7b-4d196b3c6b5b, New Expiration: 3/10/2019 7:43:22 PM +00:00
```

Cela indique que l’abonnement a été renouvelé et indique le nouveau délai d’expiration.