<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez créer une nouvelle application Web Azure AD à l’aide du centre d’administration Azure Active Directory et accorder le consentement de l’administrateur aux étendues d’autorisation requises.

1. Ouvrez un navigateur et accédez au [Centre d’administration Azure Active Directory](https://portal.azure.com). Connexion à l’aide d’un **compte professionnel ou scolaire**.

1. Sélectionnez **Azure Active Directory** dans le volet de navigation gauche, puis sélectionnez **Inscriptions d’applications (préversion)** sous **Gérer**.

    ![Capture d’écran des inscriptions d’application ](./images/01.png)

1. Sélectionnez **Nouvelle inscription**. Sur la page **Inscrire une application**, définissez les valeurs comme suit.

    - Définissez le **Nom** sur `GraphNotificationTutorial`.
    - Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.
    - Sous **URI de redirection**, définissez la première flèche déroulante sur `Web`, et la valeur sur `http://localhost`.

    ![Capture d’écran de la page inscrire une application](./images/02.png)

1. Sélectionnez **Enregistrer**. Sur la page **GraphNotificationTutorial** , copiez la valeur de l’ID d' **application (client)** et de l' **ID de répertoire (client)** enregistrez-la, vous en aurez besoin à l’étape suivante.

    ![Capture d’écran de l’ID d’application de la nouvelle inscription de l’application](./images/03.png)

1. Sélectionnez **Certificats et secrets** sous **Gérer**. Sélectionnez **nouvelle clé secrète client**. Entrez une valeur dans **Description** et sélectionnez l’une des options pour **** expirer, puis sélectionnez **Ajouter**.

    ![Capture d’écran de la boîte de dialogue Ajouter une clé secrète client](./images/04.png)

1. Copiez la valeur du secret client avant de quitter cette page. Vous en aurez besoin à l’étape suivante.

    > [!IMPORTANT]
    > Ce secret client n’apparaîtra plus jamais, aussi veillez à le copier maintenant.

    ![Capture d’écran de la clé secrète client récemment ajoutée](./images/05.png)

1. Sélectionnez **autorisations d’API** sous **gérer**. **Ajoutez une autorisation** et sélectionnez **Microsoft Graph**. Sélectionnez **autorisation d’application** et développez **utilisateur** , puis sélectionnez l’étendue **utilisateur. ReadWrite. All** . Sélectionnez **Ajouter des autorisations** pour enregistrer vos modifications.

    ![Capture d’écran de la clé secrète client récemment ajoutée](./images/06.png)

L’application demande une autorisation d’application avec l’étendue **User. ReadWrite. All** . Cette autorisation nécessite un consentement administratif.

Sélectionnez **accorder le consentement de l’administrateur pour Contoso** , puis sélectionnez **Oui** pour autoriser cette application et accorder à l’application l’accès à votre client en utilisant les étendues que vous avez spécifiées.

![Capture d’écran de la connexion](./images/07.png)
