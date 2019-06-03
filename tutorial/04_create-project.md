<!-- markdownlint-disable MD002 MD041 -->

Ouvrez votre invite de commandes, accédez à un répertoire où vous avez le droit de créer des fichiers, puis exécutez les commandes suivantes pour créer une nouvelle application .NET Core WebApi.

```shell
dotnet new webapi -o msgraphapp
```

Une fois la commande terminée, exécutez les commandes suivantes pour vérifier que votre nouveau projet s’exécute correctement.

```shell
cd msgraphapp
dotnet add package Microsoft.Identity.Client
dotnet add package Microsoft.Graph
dotnet run
```

L’application démarre et génère les résultats suivants:

```shell
Now listening on: http://localhost:5000
```

Si vous ne voyez pas cette sortie ou que vous voyez des messages d’erreur, il existe probablement un problème avec le kit de développement [logiciel .net Core 2,2](https://dotnet.microsoft.com/download) installé sur votre ordinateur de développement qui doit être résolu avant de continuer.

Arrêtez l’application en appuyant sur <kbd>CTRL</kbd>++<kbd>C</kbd>.

Ouvrez l’application dans Visual Studio code à l’aide de la commande suivante:

```shell
code .
```

Lorsqu’une boîte de dialogue vous demande si vous souhaitez ajouter des ressources requises au projet, sélectionnez **Oui**.
