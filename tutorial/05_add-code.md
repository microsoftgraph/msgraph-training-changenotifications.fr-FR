<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="122ba-101">Ouvrez le fichier **Startup.cs** et commentez la ligne suivante pour désactiver la redirection SSL.</span><span class="sxs-lookup"><span data-stu-id="122ba-101">Open the **Startup.cs** file and comment out the following line to disable ssl redirection.</span></span>

```csharp
//app.UseHttpsRedirection();
```

### <a name="add-model-classes"></a><span data-ttu-id="122ba-102">Ajouter des classes de modèles</span><span class="sxs-lookup"><span data-stu-id="122ba-102">Add model classes</span></span>

<span data-ttu-id="122ba-103">L’application utilise plusieurs nouvelles classes de modèles pour (de) la sérialisation de messages vers ou à partir de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="122ba-103">The application uses several new model classes for (de)serialization of messages to/from the Microsoft Graph.</span></span>

<span data-ttu-id="122ba-104">Cliquez avec le bouton droit dans l’arborescence de fichiers du projet et sélectionnez **nouveau dossier**.</span><span class="sxs-lookup"><span data-stu-id="122ba-104">Right-click in the project file tree and select **New Folder**.</span></span> <span data-ttu-id="122ba-105">Nommer les **modèles**</span><span class="sxs-lookup"><span data-stu-id="122ba-105">Name it **Models**</span></span>

<span data-ttu-id="122ba-106">Cliquez avec le bouton \*\*\*\* droit sur le dossier Models et ajoutez trois nouveaux fichiers:</span><span class="sxs-lookup"><span data-stu-id="122ba-106">Right-click the **Models** folder and add three new files:</span></span>

- <span data-ttu-id="122ba-107">**Notification.cs**</span><span class="sxs-lookup"><span data-stu-id="122ba-107">**Notification.cs**</span></span>
- <span data-ttu-id="122ba-108">**ResourceData.cs**</span><span class="sxs-lookup"><span data-stu-id="122ba-108">**ResourceData.cs**</span></span>
- <span data-ttu-id="122ba-109">**MyConfig.cs**</span><span class="sxs-lookup"><span data-stu-id="122ba-109">**MyConfig.cs**</span></span>

<span data-ttu-id="122ba-110">Remplacez le contenu de **notification.cs** par ce qui suit:</span><span class="sxs-lookup"><span data-stu-id="122ba-110">Replace the contents of **Notification.cs** with the following:</span></span>

```csharp
using Newtonsoft.Json;
using System;

namespace msgraphapp.Models
{
  public class Notifications
  {
    [JsonProperty(PropertyName = "value")]
    public Notification[] Items { get; set; }
  }

  // A change notification.
  public class Notification
  {
    // The type of change.
    [JsonProperty(PropertyName = "changeType")]
    public string ChangeType { get; set; }

    // The client state used to verify that the notification is from Microsoft Graph. Compare the value received with the notification to the value you sent with the subscription request.
    [JsonProperty(PropertyName = "clientState")]
    public string ClientState { get; set; }

    // The endpoint of the resource that changed. For example, a message uses the format ../Users/{user-id}/Messages/{message-id}
    [JsonProperty(PropertyName = "resource")]
    public string Resource { get; set; }

    // The UTC date and time when the webhooks subscription expires.
    [JsonProperty(PropertyName = "subscriptionExpirationDateTime")]
    public DateTimeOffset SubscriptionExpirationDateTime { get; set; }

    // The unique identifier for the webhooks subscription.
    [JsonProperty(PropertyName = "subscriptionId")]
    public string SubscriptionId { get; set; }

    // Properties of the changed resource.
    [JsonProperty(PropertyName = "resourceData")]
    public ResourceData ResourceData { get; set; }
  }
}
```

<span data-ttu-id="122ba-111">Remplacez le contenu de **ResourceData.cs** par ce qui suit:</span><span class="sxs-lookup"><span data-stu-id="122ba-111">Replace the contents of **ResourceData.cs** with the following:</span></span>

```csharp
using Newtonsoft.Json;

namespace msgraphapp.Models
{
  public class ResourceData
  {
    // The ID of the resource.
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }

    // The OData etag property.
    [JsonProperty(PropertyName = "@odata.etag")]
    public string ODataEtag { get; set; }

    // The OData ID of the resource. This is the same value as the resource property.
    [JsonProperty(PropertyName = "@odata.id")]
    public string ODataId { get; set; }

    // The OData type of the resource: "#Microsoft.Graph.Message", "#Microsoft.Graph.Event", or "#Microsoft.Graph.Contact".
    [JsonProperty(PropertyName = "@odata.type")]
    public string ODataType { get; set; }
  }
}
```

<span data-ttu-id="122ba-112">Remplacez le contenu de **MyConfig.cs** par ce qui suit:</span><span class="sxs-lookup"><span data-stu-id="122ba-112">Replace the contents of **MyConfig.cs** with the following:</span></span>

```csharp
namespace msgraphapp
{
    public class MyConfig
    {
        public string AppId { get; set; }
        public string AppSecret { get; set; }
        public string TenantId { get; set; }
        public string Ngrok { get; set; }
    }
}
```

<span data-ttu-id="122ba-113">Ouvrez le fichier **Startup.cs** .</span><span class="sxs-lookup"><span data-stu-id="122ba-113">Open the **Startup.cs** file.</span></span> <span data-ttu-id="122ba-114">Recherchez la méthode `ConfigureServices()` de méthode & remplacez-la par le code suivant:</span><span class="sxs-lookup"><span data-stu-id="122ba-114">Locate the method `ConfigureServices()` method & replace it with the following code:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
    var config = new MyConfig();
    Configuration.Bind("MyConfig", config);
    services.AddSingleton(config);
}
```

<span data-ttu-id="122ba-115">Ouvrez le fichier **appSettings. JSON** et remplacez le contenu ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="122ba-115">Open the **appsettings.json** file and replace the content the following.</span></span>

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "MyConfig":
  {
    "AppId": "<APP ID>",
    "AppSecret": "<APP SECRET>",
    "TenantId": "<TENANT ID>",
    "Ngrok": "<NGROK URL>"
  }
}
```

<span data-ttu-id="122ba-116">Remplacez les variables suivantes par les valeurs que vous avez copiées précédemment:</span><span class="sxs-lookup"><span data-stu-id="122ba-116">Replace the following variables with the values you copied earlier:</span></span>

- <span data-ttu-id="122ba-117">`<NGROK URL>`doit être défini sur l’URL HTTPS ngrok que vous avez copiée précédemment.</span><span class="sxs-lookup"><span data-stu-id="122ba-117">`<NGROK URL>` should be set to the https ngrok url you copied earlier.</span></span>
- <span data-ttu-id="122ba-118">`<TENANT ID>`doit être votre ID de client Office 365, par exemple: **contoso.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="122ba-118">`<TENANT ID>` should be your Office 365 tenant id, for example: **contoso.onmicrosoft.com**.</span></span>
- <span data-ttu-id="122ba-119">`<APP ID>``<APP SECRET>` il doit s’agir de l’ID et de la clé secrète que vous avez copiés précédemment lors de la création de l’enregistrement de l’application.</span><span class="sxs-lookup"><span data-stu-id="122ba-119">`<APP ID>` and `<APP SECRET>` should be the application id and secret you copied earlier when you created the application registration.</span></span>

### <a name="add-notification-controller"></a><span data-ttu-id="122ba-120">Ajouter un contrôleur de notification</span><span class="sxs-lookup"><span data-stu-id="122ba-120">Add notification controller</span></span>

<span data-ttu-id="122ba-121">L’application requiert un nouveau contrôleur pour traiter l’abonnement et la notification.</span><span class="sxs-lookup"><span data-stu-id="122ba-121">The application requires a new controller to process the subscription and notification.</span></span>

<span data-ttu-id="122ba-122">Cliquez avec le bouton `Controllers` droit sur le dossier, sélectionnez **nouveau fichier**, puis nommez le contrôleur **NotificationsController.cs**.</span><span class="sxs-lookup"><span data-stu-id="122ba-122">Right-click the `Controllers` folder, select **New File**, and name the controller **NotificationsController.cs**.</span></span>

<span data-ttu-id="122ba-123">Remplacez le contenu de **NotificationController.cs** par le code suivant:</span><span class="sxs-lookup"><span data-stu-id="122ba-123">Replace the contents of **NotificationController.cs** with the following code:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using msgraphapp.Models;
using Newtonsoft.Json;
using System.Net;
using System.Net.Http.Formatting;
using System.Threading;
using Microsoft.Graph;
using Microsoft.Identity.Client;
using System.Net.Http.Headers;

namespace msgraphapp.Controllers
{
  [Route("api/[controller]")]
  [ApiController]
  public class NotificationsController : ControllerBase
  {
    private readonly MyConfig config;

    public NotificationsController(MyConfig config)
    {
      this.config = config;
    }

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

      return $"Subscribed. Id: {newSubscription.Id}, Expiration: {newSubscription.ExpirationDateTime}";
    }

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

      return Ok();
    }

    private GraphServiceClient GetGraphClient()
    {
      var graphClient = new GraphServiceClient(new DelegateAuthenticationProvider((requestMessage) => {

          // get an access token for Graph
          var accessToken = GetAccessToken().Result;

          requestMessage
              .Headers
              .Authorization = new AuthenticationHeaderValue("bearer", accessToken);

          return Task.FromResult(0);
      }));

      return graphClient;
    }

    private async Task<string> GetAccessToken()
    {
      IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(config.AppId)
        .WithClientSecret(config.AppSecret)
        .WithAuthority($"https://login.microsoftonline.com/{config.TenantId}")
        .WithRedirectUri("https://daemon")
        .Build();

      string[] scopes = new string[] { "https://graph.microsoft.com/.default" };

      var result = await app.AcquireTokenForClient(scopes).ExecuteAsync();

      return result.AccessToken;
    }

  }
}
```

<span data-ttu-id="122ba-124">**Enregistrez** tous les fichiers.</span><span class="sxs-lookup"><span data-stu-id="122ba-124">**Save** all files.</span></span>