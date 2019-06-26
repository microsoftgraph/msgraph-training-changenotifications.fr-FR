<!-- markdownlint-disable MD002 MD041 -->

### <a name="query-for-changes"></a><span data-ttu-id="04255-101">Requête de modifications</span><span class="sxs-lookup"><span data-stu-id="04255-101">Query for changes</span></span>

<span data-ttu-id="04255-102">Microsoft Graph offre la possibilité de rechercher des modifications apportées à une ressource particulière depuis la dernière fois que vous l’avez appelée.</span><span class="sxs-lookup"><span data-stu-id="04255-102">Microsoft Graph offers the ability to query for changes to a particular resource since you last called it.</span></span> <span data-ttu-id="04255-103">L’utilisation de cette option, combinée à des notifications de modification, permet de s’assurer que vous ne manquez pas les modifications apportées aux ressources.</span><span class="sxs-lookup"><span data-stu-id="04255-103">Using this option, combined with Change Notifications, enables a robust pattern for ensuring you don't miss any changes to the resources.</span></span>

<span data-ttu-id="04255-104">Recherchez et ouvrez le contrôleur suivant: **Controllers > NotificationsController.cs**.</span><span class="sxs-lookup"><span data-stu-id="04255-104">Locate and open the following controller: **Controllers > NotificationsController.cs**.</span></span>
<span data-ttu-id="04255-105">Ajoutez le code suivant à la classe `NotificationsController` existante.</span><span class="sxs-lookup"><span data-stu-id="04255-105">Add the following code to the existing `NotificationsController` class.</span></span>

<span data-ttu-id="04255-106">Ce code inclut une nouvelle méthode, `CheckForUpdates()`qui appellera Microsoft Graph à l’aide de l’URL Delta, puis les pages par le biais des résultats jusqu’à `deltalink` ce qu’elle trouve une nouvelle sur la dernière page de résultats.</span><span class="sxs-lookup"><span data-stu-id="04255-106">This code includes a new method, `CheckForUpdates()`, that will call the Microsoft Graph using the delta url and then pages through the results until it finds a new `deltalink` on the final page of results.</span></span> <span data-ttu-id="04255-107">Il stocke l’URL en mémoire jusqu’à ce que le code soit de nouveau notifié lorsqu’une autre notification est déclenchée.</span><span class="sxs-lookup"><span data-stu-id="04255-107">It stores the url in memory until the code is notified again when another notification is triggered.</span></span>

```csharp
private static object DeltaLink = null;

private static IUserDeltaCollectionPage lastPage = null;

private void CheckForUpdates()
{
  var graphClient = GetGraphClient();

  // get a page of users
  var users = GetUsers(graphClient, DeltaLink);

  OutputUsers(users);

  // go through all of the pages so that we can get the delta link on the last page.
  while (users.NextPageRequest != null)
  {
    users = users.NextPageRequest.GetAsync().Result;
    OutputUsers(users);
  }

  object deltaLink;

  if (users.AdditionalData.TryGetValue("@odata.deltaLink", out deltaLink))
  {
    DeltaLink = deltaLink;
  }
}

private void OutputUsers(IUserDeltaCollectionPage users)
{
  foreach(var user in users)
    {
      var message = $"User: {user.Id}, {user.GivenName} {user.Surname}";
      Console.WriteLine(message);
    }
}

private IUserDeltaCollectionPage GetUsers(GraphServiceClient graphClient, object deltaLink)
{
  IUserDeltaCollectionPage page;

  if(lastPage == null)
  {
    page = graphClient
      .Users
      .Delta()
      .Request()
      .GetAsync()
      .Result;

  }
  else
  {
    lastPage.InitializeNextPageRequest(graphClient, deltaLink.ToString());
    page = lastPage.NextPageRequest.GetAsync().Result;
  }

  lastPage = page;
  return page;
}
```

<span data-ttu-id="04255-108">Recherchez la méthode `Post()` existante et remplacez-la par le code suivant:</span><span class="sxs-lookup"><span data-stu-id="04255-108">Locate the existing `Post()` method and replace it with the following code:</span></span>

```csharp
public ActionResult<string> Post([FromQuery]string validationToken = null)
{
  // handle validation
  if(!string.IsNullOrEmpty(validationToken))
  {
    Console.WriteLine($"Received Token: '{validationToken}'");
    return Ok(validationToken);
  }

  // handle notifications
  using (StreamReader reader = new StreamReader(Request.Body))
  {
    string content = reader.ReadToEnd();

    Console.WriteLine(content);

    var notifications = JsonConvert.DeserializeObject<Notifications>(content);

    foreach(var notification in notifications.Items)
    {
      Console.WriteLine($"Received notification: '{notification.Resource}', {notification.ResourceData?.Id}");
    }
  }

  // use deltaquery to query for all updates
  CheckForUpdates();

  return Ok();
}
```

<span data-ttu-id="04255-109">La `Post` méthode appelle `CheckForUpdates` maintenant lorsqu’une notification est reçue.</span><span class="sxs-lookup"><span data-stu-id="04255-109">The `Post` method will now call `CheckForUpdates` when a notification is received.</span></span> <span data-ttu-id="04255-110">En dessous `Post` de la méthode, ajoutez les deux nouvelles méthodes suivantes:</span><span class="sxs-lookup"><span data-stu-id="04255-110">Below the `Post` method add the following two new methods:</span></span>

<span data-ttu-id="04255-111">**Enregistrez** tous les fichiers.</span><span class="sxs-lookup"><span data-stu-id="04255-111">**Save** all files.</span></span>

### <a name="test-your-changes"></a><span data-ttu-id="04255-112">Testez vos modifications:</span><span class="sxs-lookup"><span data-stu-id="04255-112">Test your changes:</span></span>

<span data-ttu-id="04255-113">Dans Visual Studio code, sélectionnez **débogage > démarrer** le débogage pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="04255-113">Within Visual Studio Code, select **Debug > Start debugging** to run the application.</span></span>
<span data-ttu-id="04255-114">Accédez à l’URL suivante: **http://localhost:5000/api/notifications**.</span><span class="sxs-lookup"><span data-stu-id="04255-114">Navigate to the following url: **http://localhost:5000/api/notifications**.</span></span> <span data-ttu-id="04255-115">Cette opération permet d’enregistrer un nouvel abonnement.</span><span class="sxs-lookup"><span data-stu-id="04255-115">This will register a new subscription.</span></span>

<span data-ttu-id="04255-116">Ouvrez un navigateur et accédez au [Centre d’administration Microsoft 365 (https://admin.microsoft.com/AdminPortal)](https://admin.microsoft.com/AdminPortal).</span><span class="sxs-lookup"><span data-stu-id="04255-116">Open a browser and navigate to the [Microsoft 365 admin center (https://admin.microsoft.com/AdminPortal)](https://admin.microsoft.com/AdminPortal).</span></span>

1. <span data-ttu-id="04255-117">Si vous êtes invité à vous connecter, connectez-vous à l’aide d’un compte d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="04255-117">If prompted to login, sign-in using an admin account.</span></span>
1. <span data-ttu-id="04255-118">Sélectionnez **les utilisateurs > utilisateurs actifs**.</span><span class="sxs-lookup"><span data-stu-id="04255-118">Select **Users > Active users**.</span></span> 
1. <span data-ttu-id="04255-119">Sélectionnez un utilisateur actif et sélectionnez **modifier** comme **informations de contact**.</span><span class="sxs-lookup"><span data-stu-id="04255-119">Select an active user and select **Edit** for their **Contact information**.</span></span> 
1. <span data-ttu-id="04255-120">Mettez à jour la valeur de **téléphone mobile** avec un nouveau numéro, puis sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="04255-120">Update the **Mobile phone** value with a new number and Select **Save**.</span></span>

<span data-ttu-id="04255-121">Attendez la réception de la notification comme indiqué dans la **console**de débogage de code Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="04255-121">Wait for the notification to be received as indicated in the Visual Studio Code **Debug Console**:</span></span>

```shell
Received notification: 'Users/7a7fded6-0269-42c2-a0be-512d58da4463', 7a7fded6-0269-42c2-a0be-512d58da4463
```

<span data-ttu-id="04255-122">L’application va maintenant lancer une requête Delta avec le graphique pour obtenir tous les utilisateurs et déconnecter certaines de leurs informations à la sortie de la console.</span><span class="sxs-lookup"><span data-stu-id="04255-122">The application will now initiate a delta query with the graph to get all the users and log out some of their details to the console output.</span></span>

```shell
User: 19e429d2-541a-4e0b-9873-6dff9f48fabe, Allan Deyoung
User: 05501e79-f527-4913-aabf-e535646d7ffa, Christie Cline
User: fecac4be-76e7-48ec-99df-df745854aa9c, Debra Berger
User: 4095c5c4-b960-43b9-ba53-ef806d169f3e, Diego Siciliani
User: b1246157-482f-420c-992c-fc26cbff74a5, Emily Braun
User: c2b510b7-1f76-4f75-a9c1-b3176b68d7ca, Enrico Cattaneo
User: 6ec9bd4b-fc6a-4653-a291-70d3809f2610, Grady Archie
User: b6924afe-cb7f-45a3-a904-c9d5d56e06ea, Henrietta Mueller
User: 0ee8d076-4f13-4e1a-a961-eac2b29c0ef6, Irvin Sayers
User: 31f66f05-ac9b-4723-9b5d-8f381f5a6e25, Isaiah Langer
User: 7ee95e20-247d-43ef-b368-d19d96550c81, Johanna Lorenz
User: b2fa93ac-19a0-499b-b1b6-afa76c44a301, Joni Sherman
User: 01db13c5-74fc-470a-8e45-d6d736f8a35b, Jordan Miller
User: fb0b8363-4126-4c34-8185-c998ff697a60, Lee Gu
User: ee75e249-a4c1-487b-a03a-5a170c2aa33f, Lidia Holloway
User: 5449bd61-cc63-40b9-b0a8-e83720eeefba, Lynne Robbins
User: 7ce295c3-25fa-4d79-8122-9a87d15e2438, Miriam Graham
User: 737fe0a7-0b67-47dc-b7a6-9cfc07870705, Nestor Wilke
User: a1572b58-35cd-41a0-804a-732bd978df3e, Patti Fernandez
User: 7275e1c4-5698-446c-8d1d-fa8b0503c78a, Pradeep Gupta
User: 96ab25eb-6b69-4481-9d28-7b01cf367170, Megan Bowen
User: 846327fa-e6d6-4a82-89ad-5fd313bff0cc, Alex Wilber
User: 200e4c7a-b778-436c-8690-7a6398e5fe6e, MOD Administrator
User: 7a7fded6-0269-42c2-a0be-512d58da4463, Adele Vance
User: 752f0102-90f2-4b8d-ae98-79dee995e35e,   Removed?:deleted
User: 4887248a-6b48-4ba5-bdd5-fed89d8ea6a0,   Removed?:deleted
User: e538b2d5-6481-4a90-a20a-21ad55ce4c1d,   Removed?:deleted
User: bc5994d9-4404-4a14-8fb0-46b8dccca0ad,   Removed?:deleted
User: d4e3a3e0-72e9-41a6-9538-c23e10a16122,   Removed?:deleted
Got deltalink
```

<span data-ttu-id="04255-123">Dans le portail d’administration 365 de Microsoft, répétez le processus de modification d’un utilisateur et **Enregistrez** -le à nouveau.</span><span class="sxs-lookup"><span data-stu-id="04255-123">In the Microsoft 365 Admin Portal, repeat the process of editing a user and **Save** again.</span></span>

<span data-ttu-id="04255-124">L’application reçoit une autre notification et interroge à nouveau le graphique à l’aide du dernier lien Delta qu’il a reçu.</span><span class="sxs-lookup"><span data-stu-id="04255-124">The application will receive another notification and will query the graph again using the last delta link it received.</span></span> <span data-ttu-id="04255-125">Toutefois, cette fois-ci, vous remarquerez que seul l’utilisateur modifié a été renvoyé dans les résultats.</span><span class="sxs-lookup"><span data-stu-id="04255-125">However, this time you will notice that only the modified user was returned in the results.</span></span>

```shell
User: 7a7fded6-0269-42c2-a0be-512d58da4463, Adele Vance
```

<span data-ttu-id="04255-126">À l’aide de cette combinaison de notifications avec la requête Delta, vous pouvez vous assurer que vous ne manquez pas les mises à jour d’une ressource.</span><span class="sxs-lookup"><span data-stu-id="04255-126">Using this combination of notifications with delta query you can be assured you wont miss any updates to a resource.</span></span> <span data-ttu-id="04255-127">Les notifications peuvent être manquées en raison de problèmes de connexion temporaires, mais la prochaine fois que votre application reçoit une notification, elle récupère toutes les modifications apportées depuis la dernière requête réussie.</span><span class="sxs-lookup"><span data-stu-id="04255-127">Notifications may be missed due to transient connection issues, however the next time your application gets a notification it will pick up all the changes since the last successful query.</span></span>
