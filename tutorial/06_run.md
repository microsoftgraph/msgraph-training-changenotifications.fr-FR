<!-- markdownlint-disable MD002 MD041 -->

Sélectionnez **débogage > démarrer** le débogage pour exécuter l’application. Après la création de l’application, une fenêtre de navigateur s’ouvre sur une page 404. Ceci est correct puisque notre application est une API et non une page Web.

Pour vous abonner aux notifications de modifications pour les utilisateurs `http://localhost:5000/api/notifications`, accédez à l’URL suivante. Si l’opération réussit, vous verrez une sortie qui inclut un ID d’abonnement comme celui ci-dessous:

```shell
Subscribed. Id: e2dbfbe1-160b-42b0-9b9f-8ab79bf8dfed
```

Votre application est maintenant abonnée pour recevoir des notifications de Microsoft Graph lorsqu’une mise à jour est effectuée sur les utilisateurs du client Office 365.

Ouvrez un navigateur et accédez au [Centre d’administration Microsoft 365](https://admin.microsoft.com/AdminPortal). Connectez-vous à l’aide d’un compte d’administrateur. Sélectionnez **les utilisateurs > utilisateurs actifs**. Sélectionnez un utilisateur actif et sélectionnez **modifier** comme **informations de contact**. Mettez à jour la valeur de **téléphone mobile** avec un nouveau numéro, puis sélectionnez **Enregistrer**.

![Capture d’écran des détails de l’utilisateur](./images/03.png)

Dans la **console** de débogage de Visual Studio, une notification a été reçue. Parfois, cette opération peut prendre quelques minutes. Voici un exemple de la sortie:

```shell
Received notification: 'Users/7a7fded6-0269-42c2-a0be-512d58da4463', 7a7fded6-0269-42c2-a0be-512d58da4463
```

Cela indique que l’application a réussi à recevoir la notification de Microsoft Graph pour l’utilisateur spécifié dans la sortie. Vous pouvez ensuite utiliser ces informations pour interroger le graphique pour les détails complets des utilisateurs si vous souhaitez synchroniser leurs détails dans votre application.