# <a name="using-change-notifications-and-track-changes-with-microsoft-graph"></a>Utilisation des notifications de modification et suivi des modifications avec Microsoft Graph

Dans cet atelier, vous allez créer une application de console .NET principale qui reçoit les notifications de modification de Microsoft Graph lorsqu’une mise à jour est effectuée sur un compte d’utilisateur dans Azure Active Directory (Azure AD). L’application gère l’abonnement aux notifications de modification et utilise les modifications apportées au suivi dans Microsoft Graph pour s’assurer qu’aucune modification n’est manquée.

## <a name="in-this-lab"></a>Dans cet atelier

1. [Introduction à l’atelier](./tutorial/01_intro.md)
1. [Inscrire et accorder le consentement à l’application dans Microsoft Graph](./tutorial/02_create-app.md)
1. [Installer ngrok](./tutorial/03_ngrok.md)
1. [Créer le projet .NET Core](./tutorial/04_create-project.md)
1. [Code de l’API HTTP](./tutorial/05_add-code.md)
1. [Exécuter l’application](./tutorial/06_run.md)
1. [Gérer les abonnements aux notifications](./tutorial/07_subbscription-management.md)
1. [Requête de modifications](./tutorial/08_deltaquery.md)
1. [Atelier terminé](./tutorial/09_completed.md)

## <a name="prerequisites"></a>Conditions préalables

Avant de commencer ce didacticiel, le kit de développement [logiciel (SDK) .net Core 2,2](https://dotnet.microsoft.com/download) et [Visual Studio code](https://code.visualstudio.com/) doivent être installés sur votre ordinateur de développement. 

## <a name="completed-exercises"></a>Exercices terminés

Les solutions terminées sont fournies [](./Demos) dans le dossier Demos si vous êtes bloqué.