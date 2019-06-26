<!-- markdownlint-disable MD002 MD041 -->

Ouvrez le fichier **Startup.cs** et commentez la ligne suivante pour désactiver la redirection SSL.

```csharp
//app.UseHttpsRedirection();
```

### <a name="add-model-classes"></a>Ajouter des classes de modèles

L’application utilise plusieurs nouvelles classes de modèles pour (de) la sérialisation de messages vers ou à partir de Microsoft Graph.

Cliquez avec le bouton droit dans l’arborescence de fichiers du projet et sélectionnez **nouveau dossier**. Nommer les **modèles**

Cliquez avec le bouton **** droit sur le dossier Models et ajoutez trois nouveaux fichiers:

- **Notification.cs**
- **ResourceData.cs**
- **MyConfig.cs**

Remplacez le contenu de **notification.cs** par ce qui suit:

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

Remplacez le contenu de **ResourceData.cs** par ce qui suit:

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

Remplacez le contenu de **MyConfig.cs** par ce qui suit:

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

Ouvrez le fichier **Startup.cs** . Recherchez la méthode `ConfigureServices()` de méthode & remplacez-la par le code suivant:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
    var config = new MyConfig();
    Configuration.Bind("MyConfig", config);
    services.AddSingleton(config);
}
```

Ouvrez le fichier **appSettings. JSON** et remplacez le contenu ci-dessous.

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

Remplacez les variables suivantes par les valeurs que vous avez copiées précédemment:

- `<NGROK URL>`doit être défini sur l’URL HTTPS ngrok que vous avez copiée précédemment.
- `<TENANT ID>`doit être votre ID de client Office 365, par exemple: **contoso.onmicrosoft.com**.
- `<APP ID>``<APP SECRET>` il doit s’agir de l’ID et de la clé secrète que vous avez copiés précédemment lors de la création de l’enregistrement de l’application.

### <a name="add-notification-controller"></a>Ajouter un contrôleur de notification

L’application requiert un nouveau contrôleur pour traiter l’abonnement et la notification.

Cliquez avec le bouton `Controllers` droit sur le dossier, sélectionnez **nouveau fichier**, puis nommez le contrôleur **NotificationsController.cs**.

Remplacez le contenu de **NotificationController.cs** par le code suivant:

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

**Enregistrez** tous les fichiers.