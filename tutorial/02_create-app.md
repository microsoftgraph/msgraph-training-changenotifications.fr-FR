<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ccc78-101">Dans cet exercice, vous allez créer une nouvelle application Web Azure AD à l’aide du centre d’administration Azure Active Directory et accorder le consentement de l’administrateur aux étendues d’autorisation requises.</span><span class="sxs-lookup"><span data-stu-id="ccc78-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center and grant administrator consent to the required permission scopes.</span></span>

1. <span data-ttu-id="ccc78-102">Ouvrez un navigateur et accédez au [Centre d’administration Azure Active Directory (https://aad.portal.azure.com)](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ccc78-102">Open a browser and navigate to the [Azure Active Directory admin center (https://aad.portal.azure.com)](https://aad.portal.azure.com).</span></span> <span data-ttu-id="ccc78-103">Connexion à l’aide d’un **compte professionnel ou scolaire**.</span><span class="sxs-lookup"><span data-stu-id="ccc78-103">Login using a **Work or School Account**.</span></span>

1. <span data-ttu-id="ccc78-104">Sélectionnez **Azure Active Directory** dans le volet de navigation de gauche, puis sélectionnez **inscriptions des applications** sous **gérer**.</span><span class="sxs-lookup"><span data-stu-id="ccc78-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![Capture d’écran des inscriptions d’application](./images/aad-portal-home.png)

1. <span data-ttu-id="ccc78-106">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="ccc78-106">Select **New registration**.</span></span> <span data-ttu-id="ccc78-107">Sur la page **inscrire une application**</span><span class="sxs-lookup"><span data-stu-id="ccc78-107">On the **Register an application** page</span></span>

    ![Capture d’écran de la page inscriptions aux applications](./images/aad-portal-newapp.png)

    <span data-ttu-id="ccc78-109">Définissez les valeurs comme suit:</span><span class="sxs-lookup"><span data-stu-id="ccc78-109">Set the values as follows:</span></span>

    - <span data-ttu-id="ccc78-110">**Nom**: GraphNotificationTutorial</span><span class="sxs-lookup"><span data-stu-id="ccc78-110">**Name**: GraphNotificationTutorial</span></span>
    - <span data-ttu-id="ccc78-111">**Types de comptes pris en charge**: comptes dans un annuaire d’organisation et des comptes Microsoft personnels</span><span class="sxs-lookup"><span data-stu-id="ccc78-111">**Supported account types**: Accounts in any organizational directory and personal Microsoft accounts</span></span>
    - <span data-ttu-id="ccc78-112">**URI**de redirection: > Webhttp://localhost</span><span class="sxs-lookup"><span data-stu-id="ccc78-112">**Redirect URI**: Web > http://localhost</span></span>

    ![Capture d’écran de la page inscrire une application](./images/aad-portal-newapp-01.png)

    <span data-ttu-id="ccc78-114">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="ccc78-114">Select **Register**.</span></span>

1. <span data-ttu-id="ccc78-115">Sur la page **GraphNotificationTutorial** , copiez la valeur de l’ID d' **application (client)** et de l' **ID de répertoire (client)** enregistrez-la, vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="ccc78-115">On the **GraphNotificationTutorial** page, copy the value of the **Application (client) ID** and **Directory (tenant) ID** save it, you will need them in the next step.</span></span>

    ![Capture d’écran de l’ID d’application de la nouvelle inscription de l’application](./images/aad-portal-newapp-details.png)

1. <span data-ttu-id="ccc78-117">Sélectionnez **gérer les certificats de > & les secrets**.</span><span class="sxs-lookup"><span data-stu-id="ccc78-117">Select **Manage > Certificates & secrets**.</span></span> 

    <span data-ttu-id="ccc78-118">Sélectionnez **nouvelle clé secrète client**.</span><span class="sxs-lookup"><span data-stu-id="ccc78-118">Select **New client secret**.</span></span>

    <span data-ttu-id="ccc78-119">Entrez une valeur dans **Description** et sélectionnez l’une des options pour \*\*\*\* expirer, puis sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="ccc78-119">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

    ![Capture d’écran de la boîte de dialogue Ajouter une clé secrète client](./images/aad-portal-newapp-secret.png)

    ![Capture d’écran de la boîte de dialogue Ajouter une clé secrète client](./images/aad-portal-newapp-secret-02.png)

1. <span data-ttu-id="ccc78-122">Copiez la valeur du secret client avant de quitter cette page.</span><span class="sxs-lookup"><span data-stu-id="ccc78-122">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="ccc78-123">Vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="ccc78-123">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ccc78-124">Ce secret client n’apparaîtra plus jamais, aussi veillez à le copier maintenant.</span><span class="sxs-lookup"><span data-stu-id="ccc78-124">This client secret is never shown again, so make sure you copy it now.</span></span>

    ![Capture d’écran de la clé secrète client récemment ajoutée](./images/aad-portal-newapp-secret-03.png)

1. <span data-ttu-id="ccc78-126">Sélectionnez **gérer les autorisations d’API de >**.</span><span class="sxs-lookup"><span data-stu-id="ccc78-126">Select **Manage > API Permissions**.</span></span>

    <span data-ttu-id="ccc78-127">Sélectionnez **Ajouter une autorisation** et sélectionnez **Microsoft Graph**.</span><span class="sxs-lookup"><span data-stu-id="ccc78-127">Select **Add a permission** and select **Microsoft Graph**.</span></span>

    ![Capture d’écran de la sélection du service Microsoft Graph](./images/aad-portal-newapp-graphscope.png)

    <span data-ttu-id="ccc78-129">Sélectionnez **autorisation d’application**, développez le groupe d' **utilisateurs** et sélectionnez **utilisateur. ReadWrite. All** .</span><span class="sxs-lookup"><span data-stu-id="ccc78-129">Select **Application Permission**, expand the **User** group and select **User.ReadWrite.All** scope.</span></span>

    <span data-ttu-id="ccc78-130">Sélectionnez **Ajouter des autorisations** pour enregistrer vos modifications.</span><span class="sxs-lookup"><span data-stu-id="ccc78-130">Select **Add permissions** to save your changes.</span></span>

    ![Capture d’écran sélectionnant l’étendue Microsoft Graph](./images/aad-portal-newapp-graphscope-02.png)

    ![Capture d’écran de la clé secrète client récemment ajoutée](./images/aad-portal-newapp-graphscope-03.png)

<span data-ttu-id="ccc78-133">L’application demande une autorisation d’application avec l’étendue **User. ReadWrite. All** .</span><span class="sxs-lookup"><span data-stu-id="ccc78-133">The application requests an application permission with the **User.ReadWrite.All** scope.</span></span> <span data-ttu-id="ccc78-134">Cette autorisation nécessite un consentement administratif.</span><span class="sxs-lookup"><span data-stu-id="ccc78-134">This permission requires administrative consent.</span></span>

<span data-ttu-id="ccc78-135">Sélectionnez **accorder le consentement de l’administrateur pour Contoso**, puis sélectionnez **Oui** pour autoriser cette application et accorder à l’application l’accès à votre client en utilisant les étendues que vous avez spécifiées.</span><span class="sxs-lookup"><span data-stu-id="ccc78-135">Select **Grant admin consent for Contoso**, then select **Yes** to consent this application and grant the application access to your tenant using the scopes you specified.</span></span>

![Capture d’écran consentement de l’administrateur approuvé](./images/aad-portal-newapp-graphscope-04.png)
