<!-- markdownlint-disable MD002 MD041 -->

Ouvrez votre invite de commandes, accédez à un répertoire où vous avez le droit de créer votre projet, puis exécutez la commande suivante pour créer une nouvelle application .NET Core WebApi:

```shell
dotnet new webapi -o msgraphapp
```

Après avoir créé l’application, exécutez les commandes suivantes pour vérifier que votre nouveau projet s’exécute correctement.

  ```shell
  cd msgraphapp
  dotnet add package Microsoft.Identity.Client
  dotnet add package Microsoft.Graph
  dotnet run
  ```

  L’application démarre et génère les résultats suivants:

  ```shell
  info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0] ...
  info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[58] ...
  warn: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[35] ...
  info: Microsoft.AspNetCore.DataProtection.Repositories.FileSystemXmlRepository[39] ...
  Hosting environment: Development
  Content root path: /Users/ac/_play/graphchangenotifications/msgraphapp
  Now listening on: https://localhost:5001
  Now listening on: http://localhost:5000
  Application started. Press Ctrl+C to shut down.
  ```

Arrêtez l’application en appuyant sur <kbd>CTRL</kbd>++<kbd>C</kbd>.

Ouvrez l’application dans Visual Studio code à l’aide de la commande suivante:

```shell
code .
```

Si Visual Studio code affiche une boîte de dialogue vous demandant si vous souhaitez ajouter des composants requis au projet, sélectionnez **Oui**.
