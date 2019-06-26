<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="87a10-101">Ouvrez votre invite de commandes, accédez à un répertoire où vous avez le droit de créer votre projet, puis exécutez la commande suivante pour créer une nouvelle application .NET Core WebApi:</span><span class="sxs-lookup"><span data-stu-id="87a10-101">Open your command prompt, navigate to a directory where you have rights to create your project, and run the following command to create a new .NET Core WebApi app:</span></span>

```shell
dotnet new webapi -o msgraphapp
```

<span data-ttu-id="87a10-102">Après avoir créé l’application, exécutez les commandes suivantes pour vérifier que votre nouveau projet s’exécute correctement.</span><span class="sxs-lookup"><span data-stu-id="87a10-102">After creating the application, run the following commands to ensure your new project runs correctly.</span></span>

  ```shell
  cd msgraphapp
  dotnet add package Microsoft.Identity.Client
  dotnet add package Microsoft.Graph
  dotnet run
  ```

  <span data-ttu-id="87a10-103">L’application démarre et génère les résultats suivants:</span><span class="sxs-lookup"><span data-stu-id="87a10-103">The application will start and output the following:</span></span>

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

<span data-ttu-id="87a10-104">Arrêtez l’application en appuyant sur <kbd>CTRL</kbd>++<kbd>C</kbd>.</span><span class="sxs-lookup"><span data-stu-id="87a10-104">Stop the application running by pressing <kbd>CTRL</kbd>+<kbd>C</kbd>.</span></span>

<span data-ttu-id="87a10-105">Ouvrez l’application dans Visual Studio code à l’aide de la commande suivante:</span><span class="sxs-lookup"><span data-stu-id="87a10-105">Open the application in Visual Studio Code using the following command:</span></span>

```shell
code .
```

<span data-ttu-id="87a10-106">Si Visual Studio code affiche une boîte de dialogue vous demandant si vous souhaitez ajouter des composants requis au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="87a10-106">If Visual Studio code displays a dialog box asking if you want to add required assets to the project, select **Yes**.</span></span>
