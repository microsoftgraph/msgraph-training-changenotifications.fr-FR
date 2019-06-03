<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="86048-101">Ouvrez votre invite de commandes, accédez à un répertoire où vous avez le droit de créer des fichiers, puis exécutez les commandes suivantes pour créer une nouvelle application .NET Core WebApi.</span><span class="sxs-lookup"><span data-stu-id="86048-101">Open your command prompt, navigate to a directory where you have rights to create files, and run the following commands to create a new .NET Core WebApi app.</span></span>

```shell
dotnet new webapi -o msgraphapp
```

<span data-ttu-id="86048-102">Une fois la commande terminée, exécutez les commandes suivantes pour vérifier que votre nouveau projet s’exécute correctement.</span><span class="sxs-lookup"><span data-stu-id="86048-102">Once the command finishes, run the following commands to ensure your new project runs correctly.</span></span>

```shell
cd msgraphapp
dotnet add package Microsoft.Identity.Client
dotnet add package Microsoft.Graph
dotnet run
```

<span data-ttu-id="86048-103">L’application démarre et génère les résultats suivants:</span><span class="sxs-lookup"><span data-stu-id="86048-103">The application will start and output:</span></span>

```shell
Now listening on: http://localhost:5000
```

<span data-ttu-id="86048-104">Si vous ne voyez pas cette sortie ou que vous voyez des messages d’erreur, il existe probablement un problème avec le kit de développement [logiciel .net Core 2,2](https://dotnet.microsoft.com/download) installé sur votre ordinateur de développement qui doit être résolu avant de continuer.</span><span class="sxs-lookup"><span data-stu-id="86048-104">If you don't see this output, or you see error messages, there is likely a problem with the [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download) installed on your development machine that needs to be fixed before continuing.</span></span>

<span data-ttu-id="86048-105">Arrêtez l’application en appuyant sur <kbd>CTRL</kbd>++<kbd>C</kbd>.</span><span class="sxs-lookup"><span data-stu-id="86048-105">Stop the application running by pressing <kbd>CTRL</kbd>+<kbd>C</kbd>.</span></span>

<span data-ttu-id="86048-106">Ouvrez l’application dans Visual Studio code à l’aide de la commande suivante:</span><span class="sxs-lookup"><span data-stu-id="86048-106">Open the application in Visual Studio Code using the following command:</span></span>

```shell
code .
```

<span data-ttu-id="86048-107">Lorsqu’une boîte de dialogue vous demande si vous souhaitez ajouter des ressources requises au projet, sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="86048-107">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>
