<!-- markdownlint-disable MD002 MD041 -->

Ce didacticiel vous apprend à créer une application .NET Core qui utilise l’API Microsoft Graph pour recevoir des notifications (webhooks) lorsqu’un compte d’utilisateur change dans Azure Active Directory (Azure AD) et exécuter des requêtes à l’aide de l’API de requête Delta pour recevoir toutes les modifications apportées à l’utilisateur comptes depuis la dernière requête.

> [!TIP]
> Si vous préférez télécharger simplement le didacticiel terminé, vous pouvez télécharger ou cloner le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-changenotifications).

## <a name="prerequisites"></a>Conditions préalables

Avant de commencer ce didacticiel, le kit de développement [logiciel (SDK) .net Core 2,2](https://dotnet.microsoft.com/download) et [Visual Studio code](https://code.visualstudio.com/) doivent être installés sur votre ordinateur de développement. Si vous ne les avez pas installés, reportez-vous aux liens précédents sur les options de téléchargement.

> [!NOTE]
> Ce didacticiel a été écrit avec .NET Core version 2,2. Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.

## <a name="feedback"></a>Commentaires

Veuillez fournir des commentaires sur ce didacticiel dans le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-changenotifications).