<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="8e25e-101">Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8e25e-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="8e25e-102">Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="8e25e-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="8e25e-103">Pour ce faire, vous allez intégrer la bibliothèque d' [authentification REACT-Native-App à](https://github.com/FormidableLabs/react-native-app-auth) l’application.</span><span class="sxs-lookup"><span data-stu-id="8e25e-103">To do this, you will integrate the [react-native-app-auth](https://github.com/FormidableLabs/react-native-app-auth) library into the application.</span></span>

1. <span data-ttu-id="8e25e-104">Créez un répertoire dans le répertoire **GraphTutorial** nommé **auth**.</span><span class="sxs-lookup"><span data-stu-id="8e25e-104">Create a new directory in the **GraphTutorial** directory named **auth**.</span></span>
1. <span data-ttu-id="8e25e-105">Créez un fichier dans le répertoire **GraphTutorial/auth** nommé **authconfig. TS**.</span><span class="sxs-lookup"><span data-stu-id="8e25e-105">Create a new file in the **GraphTutorial/auth** directory named **AuthConfig.ts**.</span></span> <span data-ttu-id="8e25e-106">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="8e25e-106">Add the following code to the file.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/auth/AuthConfig.example.ts":::

    <span data-ttu-id="8e25e-107">Remplacez `YOUR_APP_ID_HERE` par l’ID d’application de l’inscription de votre application.</span><span class="sxs-lookup"><span data-stu-id="8e25e-107">Replace `YOUR_APP_ID_HERE` with the app ID from your app registration.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8e25e-108">Si vous utilisez le contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **authconfig. TS** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.</span><span class="sxs-lookup"><span data-stu-id="8e25e-108">If you're using source control such as git, now would be a good time to exclude the **AuthConfig.ts** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="8e25e-109">Implémentation de la connexion</span><span class="sxs-lookup"><span data-stu-id="8e25e-109">Implement sign-in</span></span>

<span data-ttu-id="8e25e-110">Dans cette section, vous allez créer une classe d’assistance d’authentification et mettre à jour l’application pour se connecter et se déconnecter.</span><span class="sxs-lookup"><span data-stu-id="8e25e-110">In this section you will create an authentication helper class, and update the app to sign in and sign out.</span></span>

1. <span data-ttu-id="8e25e-111">Créez un fichier dans le répertoire **GraphTutorial/auth** nommé **AuthManager. TS**.</span><span class="sxs-lookup"><span data-stu-id="8e25e-111">Create a new file in the **GraphTutorial/auth** directory named **AuthManager.ts**.</span></span> <span data-ttu-id="8e25e-112">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="8e25e-112">Add the following code to the file.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/auth/AuthManager.ts" id="AuthManagerSnippet":::

1. <span data-ttu-id="8e25e-113">Ouvrez le fichier **GraphTutorial/App. TSX** et ajoutez l' `import` instruction suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="8e25e-113">Open the **GraphTutorial/App.tsx** file and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import { AuthManager } from './auth/AuthManager';
    ```

1. <span data-ttu-id="8e25e-114">Remplacez la `authContext` déclaration existante par la suivante.</span><span class="sxs-lookup"><span data-stu-id="8e25e-114">Replace the existing `authContext` declaration with the following.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/App.tsx" id="AuthContextSnippet" highlight="4-6,9":::

1. <span data-ttu-id="8e25e-115">Ouvrez le fichier **GraphTutorial/menus/DrawerMenu. TSX** et ajoutez l' `import` instruction suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="8e25e-115">Open the **GraphTutorial/menus/DrawerMenu.tsx** file and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import { AuthManager } from '../auth/AuthManager';
    ```

1. <span data-ttu-id="8e25e-116">Ajoutez le code suivant à la `componentDidMount` fonction.</span><span class="sxs-lookup"><span data-stu-id="8e25e-116">Add the following code to the `componentDidMount` function.</span></span>

    ```typescript
    try {
      const accessToken = await AuthManager.getAccessTokenAsync();

      // TEMPORARY
      this.setState({userFirstName: accessToken, userLoading: false});
    } catch (error) {
      Alert.alert(
        'Error getting token',
        JSON.stringify(error),
        [
          {
            text: 'OK'
          }
        ],
        { cancelable: false }
      );
    }
    ```

1. <span data-ttu-id="8e25e-117">Enregistrez vos modifications et rechargez l’application dans votre émulateur.</span><span class="sxs-lookup"><span data-stu-id="8e25e-117">Save your changes and reload the application in your emulator.</span></span>

<span data-ttu-id="8e25e-118">Si vous vous connectez à l’application, un jeton d’accès s’affiche sur l’écran d' **Accueil** .</span><span class="sxs-lookup"><span data-stu-id="8e25e-118">If you sign in to the app, you should see an access token displayed on the **Welcome** screen.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="8e25e-119">Obtenir les détails de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="8e25e-119">Get user details</span></span>

<span data-ttu-id="8e25e-120">Dans cette section, vous allez créer un fournisseur d’authentification personnalisé pour la bibliothèque client Graph, créer une classe d’assistance qui contiendra tous les appels à Microsoft Graph et mettre à jour la `DrawerMenuContent` classe pour utiliser cette nouvelle classe afin d’obtenir l’utilisateur connecté.</span><span class="sxs-lookup"><span data-stu-id="8e25e-120">In this section you will create a custom authentication provider for the Graph client library, create a helper class to hold all of the calls to Microsoft Graph and update the `DrawerMenuContent` class to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="8e25e-121">Créez un répertoire dans le répertoire **GraphTutorial** nommé **Graph**.</span><span class="sxs-lookup"><span data-stu-id="8e25e-121">Create a new directory in the **GraphTutorial** directory named **graph**.</span></span>
1. <span data-ttu-id="8e25e-122">Créez un fichier dans le répertoire **GraphTutorial/Graph** nommé **GraphAuthProvider. TS**.</span><span class="sxs-lookup"><span data-stu-id="8e25e-122">Create a new file in the **GraphTutorial/graph** directory named **GraphAuthProvider.ts**.</span></span> <span data-ttu-id="8e25e-123">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="8e25e-123">Add the following code to the file.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/graph/GraphAuthProvider.ts" id="AuthProviderSnippet":::

1. <span data-ttu-id="8e25e-124">Créez un fichier dans le répertoire **GraphTutorial/Graph** nommé **GraphManager. TS**.</span><span class="sxs-lookup"><span data-stu-id="8e25e-124">Create a new file in the **GraphTutorial/graph** directory named **GraphManager.ts**.</span></span> <span data-ttu-id="8e25e-125">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="8e25e-125">Add the following code to the file.</span></span>

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import { GraphAuthProvider } from './GraphAuthProvider';

    // Set the authProvider to an instance
    // of GraphAuthProvider
    const clientOptions = {
      authProvider: new GraphAuthProvider()
    };

    // Initialize the client
    const graphClient = Client.initWithMiddleware(clientOptions);

    export class GraphManager {
      static getUserAsync = async() => {
        // GET /me
        return await graphClient
          .api('/me')
          .select('displayName,givenName,mail,mailboxSettings,userPrincipalName')
          .get();
      }
    }
    ```

1. <span data-ttu-id="8e25e-126">Ouvrez le fichier **GraphTutorial/views/DrawerMenu. TSX** et ajoutez l' `import` instruction suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="8e25e-126">Open the **GraphTutorial/views/DrawerMenu.tsx** file and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import { GraphManager } from '../graph/GraphManager';
    ```

1. <span data-ttu-id="8e25e-127">Remplacez la `componentDidMount` méthode par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="8e25e-127">Replace the `componentDidMount` method with the following.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/menus/DrawerMenu.tsx" id="ComponentDidMountSnippet":::

<span data-ttu-id="8e25e-128">Si vous enregistrez vos modifications et rechargez l’application maintenant, une fois connecté, l’interface utilisateur est mise à jour avec le nom d’affichage et l’adresse de messagerie de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8e25e-128">If you save your changes and reload the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
