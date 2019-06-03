<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="cea2d-101">Pour que Microsoft Graph puisse envoyer des notifications à votre application en cours d’exécution sur votre ordinateur de développement, vous devez utiliser un outil tel que ngrok pour effectuer un tunnel des appels depuis Internet vers votre ordinateur.</span><span class="sxs-lookup"><span data-stu-id="cea2d-101">In order for the Microsoft Graph to send notifications to your application running on your development machine you need to use a tool such as ngrok to tunnel calls from the internet to your machine.</span></span> <span data-ttu-id="cea2d-102">Ngrok permet de diriger les appels depuis Internet vers votre application exécutée localement sans avoir à créer de règles de pare-feu.</span><span class="sxs-lookup"><span data-stu-id="cea2d-102">Ngrok allows calls from the internet to be directed to your application running locally without needing to create firewall rules.</span></span>

<span data-ttu-id="cea2d-103">Avant de continuer, vous devez avoir installé [ngrok](https://ngrok.com) sur votre ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="cea2d-103">Before you continue you should have [ngrok](https://ngrok.com) installed on your development machine.</span></span> <span data-ttu-id="cea2d-104">Si vous n’avez pas ngrok, visitez le lien précédent pour obtenir les options et les instructions de téléchargement.</span><span class="sxs-lookup"><span data-stu-id="cea2d-104">If you do not have ngrok, visit the previous link for download options and instructions.</span></span>

<span data-ttu-id="cea2d-105">Une fois installé, exécutez ngrok.</span><span class="sxs-lookup"><span data-stu-id="cea2d-105">Once installed, run ngrok.</span></span>

```shell
ngrok http 5000
```

<span data-ttu-id="cea2d-106">Cette opération démarre ngrok et achemine les demandes d’une URL de ngrok externe vers votre ordinateur de développement sur le port 5000.</span><span class="sxs-lookup"><span data-stu-id="cea2d-106">This will start ngrok and will tunnel requests from an external ngrok url to your development machine on port 5000.</span></span>

<span data-ttu-id="cea2d-107">Copiez l’adresse de transfert HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cea2d-107">Copy the https forwarding address.</span></span> <span data-ttu-id="cea2d-108">Dans l’exemple ci-dessous, `https://787b8292.ngrok.io`il s’agit de.</span><span class="sxs-lookup"><span data-stu-id="cea2d-108">In the example below that would be `https://787b8292.ngrok.io`.</span></span> <span data-ttu-id="cea2d-109">Vous en aurez besoin plus tard.</span><span class="sxs-lookup"><span data-stu-id="cea2d-109">You will need this later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cea2d-110">Chaque fois que ngrok est redémarré, une nouvelle adresse est générée et vous devez la recopier.</span><span class="sxs-lookup"><span data-stu-id="cea2d-110">Each time ngrok is restarted a new address will be generated and you will need to copy it again.</span></span>

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
