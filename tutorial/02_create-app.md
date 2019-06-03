<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="9d224-101">Dans cet exercice, vous allez créer une nouvelle application Web Azure AD à l’aide du centre d’administration Azure Active Directory et accorder le consentement de l’administrateur aux étendues d’autorisation requises.</span><span class="sxs-lookup"><span data-stu-id="9d224-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center and grant administrator consent to the required permission scopes.</span></span>

1. <span data-ttu-id="9d224-102">Ouvrez un navigateur et accédez au [Centre d’administration Azure Active Directory](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9d224-102">Open a browser and navigate to the [Azure Active Directory admin center](https://portal.azure.com).</span></span> <span data-ttu-id="9d224-103">Connexion à l’aide d’un **compte professionnel ou scolaire**.</span><span class="sxs-lookup"><span data-stu-id="9d224-103">Login using a **Work or School Account**.</span></span>

1. <span data-ttu-id="9d224-104">Sélectionnez **Azure Active Directory** dans le volet de navigation gauche, puis sélectionnez **Inscriptions d’applications (préversion)** sous **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="9d224-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="9d224-105">Capture d’écran des inscriptions d’application</span><span class="sxs-lookup"><span data-stu-id="9d224-105">A screenshot of the App registrations</span></span> ](./images/01.png)

1. <span data-ttu-id="9d224-106">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="9d224-106">Select **New registration**.</span></span> <span data-ttu-id="9d224-107">Sur la page **Inscrire une application**, définissez les valeurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="9d224-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="9d224-108">Définissez le **Nom** sur `GraphNotificationTutorial`.</span><span class="sxs-lookup"><span data-stu-id="9d224-108">Set **Name** to `GraphNotificationTutorial`.</span></span>
    - <span data-ttu-id="9d224-109">Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="9d224-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="9d224-110">Sous **URI de redirection**, définissez la première flèche déroulante sur `Web`, et la valeur sur `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="9d224-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost`.</span></span>

    ![Capture d’écran de la page inscrire une application](./images/02.png)

1. <span data-ttu-id="9d224-112">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="9d224-112">Select **Register**.</span></span> <span data-ttu-id="9d224-113">Sur la page **GraphNotificationTutorial** , copiez la valeur de l’ID d' **application (client)** et de l' **ID de répertoire (client)** enregistrez-la, vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="9d224-113">On the **GraphNotificationTutorial** page, copy the value of the **Application (client) ID** and **Directory (tenant) ID** save it, you will need them in the next step.</span></span>

    ![Capture d’écran de l’ID d’application de la nouvelle inscription de l’application](./images/03.png)

1. <span data-ttu-id="9d224-115">Sélectionnez **Certificats et secrets** sous **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="9d224-115">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="9d224-116">Sélectionnez **nouvelle clé secrète client**.</span><span class="sxs-lookup"><span data-stu-id="9d224-116">Select **New client secret**.</span></span> <span data-ttu-id="9d224-117">Entrez une valeur dans **Description** et sélectionnez l’une des options pour \*\*\*\* expirer, puis sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9d224-117">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

    ![Capture d’écran de la boîte de dialogue Ajouter une clé secrète client](./images/04.png)

1. <span data-ttu-id="9d224-119">Copiez la valeur du secret client avant de quitter cette page.</span><span class="sxs-lookup"><span data-stu-id="9d224-119">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="9d224-120">Vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="9d224-120">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="9d224-121">Ce secret client n’apparaîtra plus jamais, aussi veillez à le copier maintenant.</span><span class="sxs-lookup"><span data-stu-id="9d224-121">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Capture d’écran de la clé secrète client récemment ajoutée](./images/05.png)

1. <span data-ttu-id="9d224-123">Sélectionnez **autorisations d’API** sous **gérer**.</span><span class="sxs-lookup"><span data-stu-id="9d224-123">Select **API Permissions** under **Manage**.</span></span> <span data-ttu-id="9d224-124">**Ajoutez une autorisation** et sélectionnez **Microsoft Graph**.</span><span class="sxs-lookup"><span data-stu-id="9d224-124">**Add a permission** and select **Microsoft Graph**.</span></span> <span data-ttu-id="9d224-125">Sélectionnez **autorisation d’application** et développez **utilisateur** , puis sélectionnez l’étendue **utilisateur. ReadWrite. All** .</span><span class="sxs-lookup"><span data-stu-id="9d224-125">Select **Application Permission** and expand **User** and select the **User.ReadWrite.All** scope.</span></span> <span data-ttu-id="9d224-126">Sélectionnez **Ajouter des autorisations** pour enregistrer vos modifications.</span><span class="sxs-lookup"><span data-stu-id="9d224-126">Select **Add permissions** to save your changes.</span></span>

    ![Capture d’écran de la clé secrète client récemment ajoutée](./images/06.png)

<span data-ttu-id="9d224-128">L’application demande une autorisation d’application avec l’étendue **User. ReadWrite. All** .</span><span class="sxs-lookup"><span data-stu-id="9d224-128">The application requests an application permission with the **User.ReadWrite.All** scope.</span></span> <span data-ttu-id="9d224-129">Cette autorisation nécessite un consentement administratif.</span><span class="sxs-lookup"><span data-stu-id="9d224-129">This permission requires administrative consent.</span></span>

<span data-ttu-id="9d224-130">Sélectionnez **accorder le consentement de l’administrateur pour Contoso** , puis sélectionnez **Oui** pour autoriser cette application et accorder à l’application l’accès à votre client en utilisant les étendues que vous avez spécifiées.</span><span class="sxs-lookup"><span data-stu-id="9d224-130">Select **Grant admin consent for Contoso** and then select **Yes** to consent this application and grant the application access to your tenant using the scopes you specified.</span></span>

![Capture d’écran de la connexion](./images/07.png)
