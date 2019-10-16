# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="ff125-101">Exécution du projet terminé</span><span class="sxs-lookup"><span data-stu-id="ff125-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff125-102">Conditions préalables</span><span class="sxs-lookup"><span data-stu-id="ff125-102">Prerequisites</span></span>

<span data-ttu-id="ff125-103">Pour exécuter le projet terminé dans ce dossier, vous avez besoin des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ff125-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="ff125-104">Au moins l’un des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="ff125-104">At least one of the following:</span></span>
  - <span data-ttu-id="ff125-105">Kit de développement [Android Studio](https://developer.android.com/studio/) **et** [java](https://jdk.java.net) (requis pour exécuter l’exemple sur Android)</span><span class="sxs-lookup"><span data-stu-id="ff125-105">[Android Studio](https://developer.android.com/studio/) **and** [Java Development Kit](https://jdk.java.net) (required to run the sample on Android)</span></span>
  - <span data-ttu-id="ff125-106">[Xcode](https://developer.apple.com/xcode/) (requis pour exécuter l’exemple sur iOS)</span><span class="sxs-lookup"><span data-stu-id="ff125-106">[Xcode](https://developer.apple.com/xcode/) (required to run the sample on iOS)</span></span>
- [<span data-ttu-id="ff125-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="ff125-107">Node.js</span></span>](https://nodejs.org)
- <span data-ttu-id="ff125-108">Soit un compte Microsoft personnel avec une boîte aux lettres sur Outlook.com, soit un compte professionnel ou scolaire Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ff125-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

> <span data-ttu-id="ff125-109">**Remarque :** Ce didacticiel a été écrit à l’aide de l’interface CLI native de REACT, qui présente des conditions préalables spécifiques en fonction de votre système d’exploitation et des plateformes cibles.</span><span class="sxs-lookup"><span data-stu-id="ff125-109">**Note:** This tutorial was written using the React Native CLI, which has specific prerequisites depending on your operating system and target platforms.</span></span> <span data-ttu-id="ff125-110">Pour obtenir des instructions sur la configuration de votre ordinateur de développement, voir [REACT Native Getting Started](https://facebook.github.io/react-native/docs/getting-started) .</span><span class="sxs-lookup"><span data-stu-id="ff125-110">See [React Native Getting Started](https://facebook.github.io/react-native/docs/getting-started) for instructions on configuring your development machine.</span></span> <span data-ttu-id="ff125-111">Les versions utilisées pour ce didacticiel sont répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ff125-111">The versions used for this tutorial are listed below.</span></span> <span data-ttu-id="ff125-112">Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.</span><span class="sxs-lookup"><span data-stu-id="ff125-112">The steps in this guide may work with other versions, but that has not been tested.</span></span>
>
> - <span data-ttu-id="ff125-113">Android Studio version 3.4.2 avec 1.8.0 JRE et le kit de développement logiciel (SDK) Android 9,0</span><span class="sxs-lookup"><span data-stu-id="ff125-113">Android Studio version 3.4.2 with the 1.8.0 JRE and the Android 9.0 SDK</span></span>
> - <span data-ttu-id="ff125-114">Kit de développement Java version 12.0.2</span><span class="sxs-lookup"><span data-stu-id="ff125-114">Java Development Kit version 12.0.2</span></span>
> - <span data-ttu-id="ff125-115">Xcode version 10,3</span><span class="sxs-lookup"><span data-stu-id="ff125-115">Xcode version 10.3</span></span>
> - <span data-ttu-id="ff125-116">Node. js version 10.16.0</span><span class="sxs-lookup"><span data-stu-id="ff125-116">Node.js version 10.16.0</span></span>

<span data-ttu-id="ff125-117">Si vous n’avez pas de compte Microsoft, vous disposez de deux options pour obtenir un compte gratuit :</span><span class="sxs-lookup"><span data-stu-id="ff125-117">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="ff125-118">Vous pouvez vous [inscrire pour obtenir un nouveau compte Microsoft personnel](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="ff125-118">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="ff125-119">Vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.</span><span class="sxs-lookup"><span data-stu-id="ff125-119">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-an-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="ff125-120">Enregistrer une application avec le centre d’administration Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff125-120">Register an application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="ff125-121">Ouvrez un navigateur, accédez au [Centre d’administration Azure Active Directory ](https://aad.portal.azure.com) et connectez-vous à l’aide d’un **compte personnel** (ou compte Microsoft) ou d’un **compte professionnel ou scolaire**.</span><span class="sxs-lookup"><span data-stu-id="ff125-121">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="ff125-122">Sélectionnez **Azure Active Directory** dans le volet de navigation de gauche, puis sélectionnez **inscriptions des applications** sous **gérer**.</span><span class="sxs-lookup"><span data-stu-id="ff125-122">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="ff125-123">Capture d’écran des inscriptions d’application</span><span class="sxs-lookup"><span data-stu-id="ff125-123">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="ff125-124">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="ff125-124">Select **New registration**.</span></span> <span data-ttu-id="ff125-125">Sur la page **Inscrire une application**, définissez les valeurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="ff125-125">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="ff125-126">Définissez le **Nom** sur `React Native Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="ff125-126">Set **Name** to `React Native Graph Tutorial`.</span></span>
    - <span data-ttu-id="ff125-127">Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="ff125-127">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="ff125-128">Sous **URI de redirection**, remplacez la liste déroulante par **client Public (mobile & Desktop)** et définissez `graph-tutorial://react-native-auth`la valeur sur.</span><span class="sxs-lookup"><span data-stu-id="ff125-128">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `graph-tutorial://react-native-auth`.</span></span>

    ![Capture d’écran de la page inscrire une application](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="ff125-130">Sélectionnez **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="ff125-130">Select **Register**.</span></span> <span data-ttu-id="ff125-131">Sur la page **didacticiel de graphique Native REACT** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="ff125-131">On the **React Native Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Capture d’écran de l’ID d’application de la nouvelle inscription de l’application](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="ff125-133">Sous **gérer**, sélectionnez **authentification**.</span><span class="sxs-lookup"><span data-stu-id="ff125-133">Under **Manage**, select **Authentication**.</span></span> <span data-ttu-id="ff125-134">Sur la page **URI de redirection** , ajoutez un autre URI de redirection de type **public client (mobile & Desktop)**, `urn:ietf:wg:oauth:2.0:oob`avec l’URI.</span><span class="sxs-lookup"><span data-stu-id="ff125-134">On the **Redirect URIs** page, add another redirect URI of type **Public client (mobile & desktop)**, with the URI `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="ff125-135">Cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="ff125-135">Select **Save**.</span></span>

    ![Capture d’écran de la page des URI de redirection](/tutorial/images/aad-redirect-uris.png)

## <a name="configure-the-sample"></a><span data-ttu-id="ff125-137">Configurer l’exemple</span><span class="sxs-lookup"><span data-stu-id="ff125-137">Configure the sample</span></span>

1. <span data-ttu-id="ff125-138">Renommez le fichier **GraphTutorial/auth/authconfig. js. example** en **authconfig. js**.</span><span class="sxs-lookup"><span data-stu-id="ff125-138">Rename the **GraphTutorial/auth/AuthConfig.js.example** file to **AuthConfig.js**.</span></span>
1. <span data-ttu-id="ff125-139">Modifiez le fichier **authconfig. js** et effectuez les modifications suivantes.</span><span class="sxs-lookup"><span data-stu-id="ff125-139">Edit the **AuthConfig.js** file and make the following changes.</span></span>
    1. <span data-ttu-id="ff125-140">Remplacez `YOUR_APP_ID_HERE` par l' **ID d’application** que vous avez obtenu à partir du portail d’inscription des applications.</span><span class="sxs-lookup"><span data-stu-id="ff125-140">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

1. <span data-ttu-id="ff125-141">Dans votre interface de ligne de commande (CLI), accédez au répertoire **GraphTutorial** et exécutez la commande suivante pour installer les conditions requises.</span><span class="sxs-lookup"><span data-stu-id="ff125-141">In your command-line interface (CLI), navigate to the **GraphTutorial** directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="ff125-142">Exécution de l’exemple</span><span class="sxs-lookup"><span data-stu-id="ff125-142">Run the sample</span></span>

### <a name="run-on-ios-simulator"></a><span data-ttu-id="ff125-143">Exécuter sur le simulateur iOS</span><span class="sxs-lookup"><span data-stu-id="ff125-143">Run on iOS Simulator</span></span>

<span data-ttu-id="ff125-144">Exécutez la commande suivante dans votre interface CLI pour démarrer l’application dans un simulateur iOS.</span><span class="sxs-lookup"><span data-stu-id="ff125-144">Run the following command in your CLI to start the application in an iOS Simulator.</span></span>

```Shell
react-native run-ios
```

### <a name="run-on-android-emulator"></a><span data-ttu-id="ff125-145">Exécuter sur l’émulateur Android</span><span class="sxs-lookup"><span data-stu-id="ff125-145">Run on Android emulator</span></span>

<span data-ttu-id="ff125-146">Instance de lancement et d’émulateur Android et exécutez la commande suivante dans votre interface CLI pour démarrer l’application dans un émulateur Android.</span><span class="sxs-lookup"><span data-stu-id="ff125-146">Launch and Android emulator instance and run the following command in your CLI to start the application in an Android emulator.</span></span>

```Shell
react-native run-android
```
