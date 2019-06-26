<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7da5e-101">Les abonnements pour les notifications expirent et doivent être renouvelés régulièrement.</span><span class="sxs-lookup"><span data-stu-id="7da5e-101">Subscriptions for notifications expire and need to be renewed periodically.</span></span> <span data-ttu-id="7da5e-102">Les étapes suivantes montrent comment renouveler les notifications.</span><span class="sxs-lookup"><span data-stu-id="7da5e-102">The following steps will demonstrate how to renew notifications</span></span>

<span data-ttu-id="7da5e-103">Les **contrôleurs ouverts > fichier NotificationsController.cs**</span><span class="sxs-lookup"><span data-stu-id="7da5e-103">Open **Controllers > NotificationsController.cs** file</span></span>

<span data-ttu-id="7da5e-104">Ajoutez les deux déclarations de membre suivantes à `NotificationsController` la classe:</span><span class="sxs-lookup"><span data-stu-id="7da5e-104">Add the following two member declarations to the `NotificationsController` class:</span></span>

```csharp
private static Dictionary<string, Subscription> Subscriptions = new Dictionary<string, Subscription>();
private static Timer subscriptionTimer = null;
```

<span data-ttu-id="7da5e-105">Ajoutez les nouvelles méthodes suivantes.</span><span class="sxs-lookup"><span data-stu-id="7da5e-105">Add the following new methods.</span></span> <span data-ttu-id="7da5e-106">Ces éléments implémenteront un minuteur d’arrière-plan qui s’exécutera toutes les 15 secondes pour vérifier si les abonnements ont expiré.</span><span class="sxs-lookup"><span data-stu-id="7da5e-106">These will implement a background timer that will run every 15 seconds to check if subscriptions have expired.</span></span> <span data-ttu-id="7da5e-107">Si c’est le cas, ils seront renouvelés.</span><span class="sxs-lookup"><span data-stu-id="7da5e-107">If they have, they will be renewed.</span></span>

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

<span data-ttu-id="7da5e-108">La `CheckSubscriptions` méthode est appelée toutes les 15 secondes par le minuteur.</span><span class="sxs-lookup"><span data-stu-id="7da5e-108">The `CheckSubscriptions` method is called every 15 seconds by the timer.</span></span> <span data-ttu-id="7da5e-109">Pour l’utilisation de la production, cette valeur doit être définie sur une valeur plus raisonnable afin de réduire le nombre d’appels inutiles à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="7da5e-109">For production use this should be set to a more reasonable value to reduce the number of unnecessary calls to Microsoft Graph.</span></span>

<span data-ttu-id="7da5e-110">La `RenewSubscription` méthode renouvelle un abonnement et n’est appelée que si un abonnement arrivera à expiration dans les deux minutes à venir.</span><span class="sxs-lookup"><span data-stu-id="7da5e-110">The `RenewSubscription` method renews a subscription and is only called if a subscription is going to expire in the next two minutes.</span></span>

<span data-ttu-id="7da5e-111">Recherchez la méthode `Get()` et remplacez-la par le code suivant:</span><span class="sxs-lookup"><span data-stu-id="7da5e-111">Locate the method `Get()` and replace it with the following code:</span></span>

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

<span data-ttu-id="7da5e-112">Ajoutez l’instruction suivante après les instructions `using` existantes en haut du fichier:</span><span class="sxs-lookup"><span data-stu-id="7da5e-112">Add the following statement after the existing `using` statements at the top of the file:</span></span>

```csharp
using System.Threading;
```

### <a name="test-the-changes"></a><span data-ttu-id="7da5e-113">Testez les modifications:</span><span class="sxs-lookup"><span data-stu-id="7da5e-113">Test the changes:</span></span>

<span data-ttu-id="7da5e-114">Dans Visual Studio code, sélectionnez **débogage > démarrer** le débogage pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="7da5e-114">Within Visual Studio Code, select **Debug > Start debugging** to run the application.</span></span>
<span data-ttu-id="7da5e-115">Accédez à l’URL suivante: **http://localhost:5000/api/notifications**.</span><span class="sxs-lookup"><span data-stu-id="7da5e-115">Navigate to the following url: **http://localhost:5000/api/notifications**.</span></span> <span data-ttu-id="7da5e-116">Cette opération permet d’enregistrer un nouvel abonnement.</span><span class="sxs-lookup"><span data-stu-id="7da5e-116">This will register a new subscription.</span></span>

<span data-ttu-id="7da5e-117">Dans la fenêtre de la **console** de débogage de code Visual Studio, toutes les 15 secondes, Notez que le minuteur vérifiant que l’abonnement est expiré:</span><span class="sxs-lookup"><span data-stu-id="7da5e-117">In the Visual Studio Code **Debug Console** window, approximately every 15 seconds, notice the timer checking the subscription for expiration:</span></span>

```shell
Checking subscriptions 12:32:51.882
Current subscription count 1
```

<span data-ttu-id="7da5e-118">Patientez quelques minutes et vous verrez ce qui suit lorsque l’abonnement a besoin d’être renouvelé:</span><span class="sxs-lookup"><span data-stu-id="7da5e-118">Wait a few minutes and you will see the following when the subscription needs renewing:</span></span>

```shell
Renewed subscription: 07ca62cd-1a1b-453c-be7b-4d196b3c6b5b, New Expiration: 3/10/2019 7:43:22 PM +00:00
```

<span data-ttu-id="7da5e-119">Cela indique que l’abonnement a été renouvelé et indique le nouveau délai d’expiration.</span><span class="sxs-lookup"><span data-stu-id="7da5e-119">This indicates that the subscription was renewed and shows the new expiry time.</span></span>