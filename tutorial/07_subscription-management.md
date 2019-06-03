<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2377e-101">Les abonnements pour les notifications expirent et doivent être renouvelés régulièrement.</span><span class="sxs-lookup"><span data-stu-id="2377e-101">Subscriptions for notifications expire and need to be renewed periodically.</span></span>

<span data-ttu-id="2377e-102">Ouvrez **NotificationsController.cs** et remplacez la `Get()` méthode par le code suivant:</span><span class="sxs-lookup"><span data-stu-id="2377e-102">Open **NotificationsController.cs** and replace the `Get()` method with the following code:</span></span>

```csharp
private static Dictionary<string, Subscription> Subscriptions = new Dictionary<string, Subscription>();
private static Timer subscriptionTimer = null;

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

<span data-ttu-id="2377e-103">Ajoutez l’instruction using suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="2377e-103">Add the following using statement at the top of the file.</span></span>

```csharp
using System.Threading;
```

<span data-ttu-id="2377e-104">Ce code ci-dessus ajoute un minuteur d’arrière-plan qui se déclenche toutes les 15 secondes et vérifie si les abonnements ont expiré.</span><span class="sxs-lookup"><span data-stu-id="2377e-104">This code above adds a background timer that will fire every 15 seconds and check subscriptions to see if they have expired.</span></span>

<span data-ttu-id="2377e-105">Ajoutez les nouvelles méthodes suivantes:</span><span class="sxs-lookup"><span data-stu-id="2377e-105">Add the following new methods:</span></span>

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

<span data-ttu-id="2377e-106">La `CheckSubscriptions` méthode est appelée toutes les 15 secondes par le minuteur.</span><span class="sxs-lookup"><span data-stu-id="2377e-106">The `CheckSubscriptions` method is called every 15 seconds by the timer.</span></span> <span data-ttu-id="2377e-107">Pour l’utilisation de production, cette valeur doit être définie sur une valeur plus raisonnable afin de réduire le nombre d’appels inutiles à Graph.</span><span class="sxs-lookup"><span data-stu-id="2377e-107">For production use this should be set to a more reasonable value to reduce the number of unnecessary calls to Graph.</span></span> <span data-ttu-id="2377e-108">La `RenewSubscription` méthode renouvelle un abonnement et n’est appelée que si un abonnement arrivera à expiration dans les deux minutes à venir.</span><span class="sxs-lookup"><span data-stu-id="2377e-108">The `RenewSubscription` method renews a subscription and is only called if a subscription is going to expire in the next two minutes.</span></span>

<span data-ttu-id="2377e-109">Sélectionnez **débogage > démarrer** le débogage pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="2377e-109">Select **Debug > Start debugging** to run the application.</span></span> <span data-ttu-id="2377e-110">Accédez à l’URL `*http://localhost:5000/api/notifications` suivante pour enregistrer un nouvel abonnement.</span><span class="sxs-lookup"><span data-stu-id="2377e-110">Navigate to the following url `*http://localhost:5000/api/notifications` to register a new subscription.</span></span>

<span data-ttu-id="2377e-111">Vous verrez la sortie suivante dans la `DEBUG OUTPUT` fenêtre de Visual Studio code environ toutes les 15 secondes.</span><span class="sxs-lookup"><span data-stu-id="2377e-111">You will see the following output in the `DEBUG OUTPUT` window of Visual Studio Code approximately every 15 seconds.</span></span>  <span data-ttu-id="2377e-112">Il s’agit du minuteur vérifiant l’expiration de l’abonnement.</span><span class="sxs-lookup"><span data-stu-id="2377e-112">This is the timer checking the subscription for expiry.</span></span>

```shell
Checking subscriptions 12:32:51.882
Current subscription count 1
```

<span data-ttu-id="2377e-113">Patientez quelques minutes et vous verrez ce qui suit lorsque l’abonnement a besoin d’être renouvelé:</span><span class="sxs-lookup"><span data-stu-id="2377e-113">Wait a few minutes and you will see the following when the subscription needs renewing:</span></span>

```shell
Renewed subscription: 07ca62cd-1a1b-453c-be7b-4d196b3c6b5b, New Expiration: 3/10/2019 7:43:22 PM +00:00
```

<span data-ttu-id="2377e-114">Cela indique que l’abonnement a été renouvelé et indique le nouveau délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="2377e-114">This indicates that the subscription was renewed and shows the new expiry time.</span></span>
