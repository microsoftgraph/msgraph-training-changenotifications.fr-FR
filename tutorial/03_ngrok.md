<!-- markdownlint-disable MD002 MD041 -->

Pour que Microsoft Graph puisse envoyer des notifications à votre application en cours d’exécution sur votre ordinateur de développement, vous devez utiliser un outil tel que ngrok pour effectuer un tunnel des appels depuis Internet vers votre ordinateur. Ngrok permet de diriger les appels depuis Internet vers votre application exécutée localement sans avoir à créer de règles de pare-feu.

Avant de continuer, vous devez avoir installé [ngrok](https://ngrok.com) sur votre ordinateur de développement. Si vous n’avez pas ngrok, visitez le lien précédent pour obtenir les options et les instructions de téléchargement.

Une fois installé, exécutez ngrok.

```shell
ngrok http 5000
```

Cette opération démarre ngrok et achemine les demandes d’une URL de ngrok externe vers votre ordinateur de développement sur le port 5000.

Copiez l’adresse de transfert HTTPS. Dans l’exemple ci-dessous, `https://787b8292.ngrok.io`il s’agit de. Vous en aurez besoin plus tard.

> [!IMPORTANT]
> Chaque fois que ngrok est redémarré, une nouvelle adresse est générée et vous devez la recopier.

```shell
ngrok by @inconshreveable

Session Status                online
Account                       Basic
Version                       2.3.15
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://787b8292.ngrok.io -> http://localhost:5000
Forwarding                    https://787b8292.ngrok.io -> http://localhost:5000

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```
