<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7ece1-101">Ce didacticiel vous apprend à créer une application .NET Core qui utilise l’API Microsoft Graph pour recevoir des notifications (webhooks) lorsqu’un compte d’utilisateur change dans Azure Active Directory (Azure AD) et exécuter des requêtes à l’aide de l’API de requête Delta pour recevoir toutes les modifications apportées à l’utilisateur comptes depuis la dernière requête.</span><span class="sxs-lookup"><span data-stu-id="7ece1-101">This tutorial teaches you how to build a .NET Core app that uses the Microsoft Graph API to receive notifications (webhooks) when a user account changes in Azure Active Directory (Azure AD) and perform queries using the delta query API to receive all changes to user accounts since the last query was made.</span></span>

> [!TIP]
> <span data-ttu-id="7ece1-102">Si vous préférez télécharger simplement le didacticiel terminé, vous pouvez télécharger ou cloner le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-changenotifications).</span><span class="sxs-lookup"><span data-stu-id="7ece1-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-changenotifications).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ece1-103">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="7ece1-103">Prerequisites</span></span>

<span data-ttu-id="7ece1-104">Avant de commencer ce didacticiel, le kit de développement [logiciel (SDK) .net Core 2,2](https://dotnet.microsoft.com/download) et [Visual Studio code](https://code.visualstudio.com/) doivent être installés sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="7ece1-104">Before you start this tutorial, you should have [.NET Core 2.2 SDK](https://dotnet.microsoft.com/download) and [Visual Studio Code](https://code.visualstudio.com/) installed on your development machine.</span></span>

> [!NOTE]
> <span data-ttu-id="7ece1-105">Ce didacticiel a été écrit avec .NET Core version 2,2.</span><span class="sxs-lookup"><span data-stu-id="7ece1-105">This tutorial was written with .NET Core version 2.2.</span></span> <span data-ttu-id="7ece1-106">Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.</span><span class="sxs-lookup"><span data-stu-id="7ece1-106">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="watch-the-tutorial"></a><span data-ttu-id="7ece1-107">Regarder le didacticiel</span><span class="sxs-lookup"><span data-stu-id="7ece1-107">Watch the tutorial</span></span>

<span data-ttu-id="7ece1-108">Ce module a été enregistré et est disponible dans le canal YouTube de développement Office.</span><span class="sxs-lookup"><span data-stu-id="7ece1-108">This module has been recorded and is available in the Office Development YouTube channel.</span></span>

<!-- markdownlint-disable MD033 MD034 -->
<br/>

> [!VIDEO https://www.youtube-nocookie.com/embed/fThiCZmIcMQ]
<!-- markdownlint-enable MD033 MD034 -->

## <a name="feedback"></a><span data-ttu-id="7ece1-109">Commentaires</span><span class="sxs-lookup"><span data-stu-id="7ece1-109">Feedback</span></span>

<span data-ttu-id="7ece1-110">Veuillez fournir des commentaires sur ce didacticiel dans le [référentiel GitHub](https://github.com/microsoftgraph/msgraph-training-changenotifications).</span><span class="sxs-lookup"><span data-stu-id="7ece1-110">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-changenotifications).</span></span>
