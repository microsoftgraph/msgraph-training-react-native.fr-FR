<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ae012-101">Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae012-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="ae012-102">Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="ae012-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="ae012-103">Pour ce faire, vous allez intégrer la bibliothèque d' [authentification REACT-Native-App à](https://github.com/FormidableLabs/react-native-app-auth) l’application.</span><span class="sxs-lookup"><span data-stu-id="ae012-103">To do this, you will integrate the [react-native-app-auth](https://github.com/FormidableLabs/react-native-app-auth) library into the application.</span></span>

1. <span data-ttu-id="ae012-104">Créez un répertoire dans le répertoire **GraphTutorial** nommé **auth**.</span><span class="sxs-lookup"><span data-stu-id="ae012-104">Create a new directory in the **GraphTutorial** directory named **auth**.</span></span>
1. <span data-ttu-id="ae012-105">Créez un fichier dans le répertoire **GraphTutorial/auth** nommé **authconfig. js**.</span><span class="sxs-lookup"><span data-stu-id="ae012-105">Create a new file in the **GraphTutorial/auth** directory named **AuthConfig.js**.</span></span> <span data-ttu-id="ae012-106">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="ae012-106">Add the following code to the file.</span></span>

    ```js
    export const AuthConfig = {
      appId = 'YOUR_APP_ID_HERE',
      appScopes = [
        'openid',
        'offline_access',
        'profile',
        'User.Read',
        'Calendars.Read'
      ]
    };
    ```

    <span data-ttu-id="ae012-107">Remplacez `YOUR_APP_ID_HERE` par l’ID d’application de l’inscription de votre application.</span><span class="sxs-lookup"><span data-stu-id="ae012-107">Replace `YOUR_APP_ID_HERE` with the app ID from your app registration.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ae012-108">Si vous utilisez le contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **authconfig. js** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.</span><span class="sxs-lookup"><span data-stu-id="ae012-108">If you're using source control such as git, now would be a good time to exclude the **AuthConfig.js** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="ae012-109">Mettre en œuvre la connexion</span><span class="sxs-lookup"><span data-stu-id="ae012-109">Implement sign-in</span></span>

<span data-ttu-id="ae012-110">Dans cette section, vous allez créer une classe d’assistance d’authentification et mettre à jour l’application pour se connecter et se déconnecter.</span><span class="sxs-lookup"><span data-stu-id="ae012-110">In this section you will create an authentication helper class, and update the app to sign in and sign out.</span></span>

1. <span data-ttu-id="ae012-111">Créez un fichier dans le répertoire **GraphTutorial/auth** nommé **AuthManager. js**.</span><span class="sxs-lookup"><span data-stu-id="ae012-111">Create a new file in the **GraphTutorial/auth** directory named **AuthManager.js**.</span></span> <span data-ttu-id="ae012-112">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="ae012-112">Add the following code to the file.</span></span>

    ```js
    import { AuthConfig } from './AuthConfig';
    import { AsyncStorage } from 'react-native';
    import { authorize, refresh } from 'react-native-app-auth';
    import moment from 'moment';

    const config = {
      clientId: AuthConfig.appId,
      redirectUrl: Platform.OS === 'ios' ? 'urn:ietf:wg:oauth:2.0:oob' : 'graph-tutorial://react-native-auth',
      scopes: AuthConfig.appScopes,
      additionalParameters: { prompt: 'select_account' },
      serviceConfiguration: {
        authorizationEndpoint: 'https://login.microsoftonline.com/common/oauth2/v2.0/authorize',
        tokenEndpoint: 'https://login.microsoftonline.com/common/oauth2/v2.0/token',
      }
    };

    export class AuthManager {

      static signInAsync = async () => {
        const result = await authorize(config);

        // Store the access token, refresh token, and expiration time in storage
        await AsyncStorage.setItem('userToken', result.accessToken);
        await AsyncStorage.setItem('refreshToken', result.refreshToken);
        await AsyncStorage.setItem('expireTime', result.accessTokenExpirationDate);
      }

      static signOutAsync = async () => {
        // Clear storage
        await AsyncStorage.removeItem('userToken');
        await AsyncStorage.removeItem('refreshToken');
        await AsyncStorage.removeItem('expireTime');
      }

      static getAccessTokenAsync = async() => {
        const expireTime = await AsyncStorage.getItem('expireTime');

        if (expireTime !== null) {
          // Get expiration time - 5 minutes
          // If it's <= 5 minutes before expiration, then refresh
          const expire = moment(expireTime).subtract(5, 'minutes');
          const now = moment();

          if (now.isSameOrAfter(expire)) {
            // Expired, refresh
            const refreshToken = await AsyncStorage.getItem('refreshToken');

            const result = await refresh(config, {refreshToken: refreshToken});

            // Store the new access token, refresh token, and expiration time in storage
            await AsyncStorage.setItem('userToken', result.accessToken);
            await AsyncStorage.setItem('refreshToken', result.refreshToken);
            await AsyncStorage.setItem('expireTime', result.accessTokenExpirationDate);

            return result.accessToken;
          }

          // Not expired, just return saved access token
          const accessToken = await AsyncStorage.getItem('userToken');
          return accessToken;
        }

        return null;
      }
    }
    ```

1. <span data-ttu-id="ae012-113">Ouvrez le fichier **GraphTutorial/views/SignInScreen. js** et ajoutez l’instruction `import` suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="ae012-113">Open the **GraphTutorial/views/SignInScreen.js** file and add the following `import` statement to the top of the file.</span></span>

    ```js
    import { AuthManager } from '../auth/AuthManager';
    ```

1. <span data-ttu-id="ae012-114">Remplacez la méthode `_signInAsync` existante par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="ae012-114">Replace the existing `_signInAsync` method with the following.</span></span>

    ```js
    _signInAsync = async () => {
      try {
        await AuthManager.signInAsync();
        this.props.navigation.navigate('App');
      } catch (error) {
        alert(error);
      }
    };

1. Open the **GraphTutorial/views/HomeScreen.js** file and add the following `import` statement to the top of the file.

    ```js
    import { AuthManager } from '../auth/AuthManager';
    ```

1. <span data-ttu-id="ae012-115">Ajoutez la méthode suivante à la classe `HomeScreen`.</span><span class="sxs-lookup"><span data-stu-id="ae012-115">Add the following method to the `HomeScreen` class.</span></span>

    ```js
    async componentDidMount() {
      try {
        const accessToken = await AuthManager.getAccessTokenAsync();

        // TEMPORARY
        this.setState({userName: accessToken, userLoading: false});
      } catch (error) {
        alert(error);
      }
    }
    ```

1. <span data-ttu-id="ae012-116">Ouvrez le fichier **GraphTutorial/menus/DrawerMenu. js** et ajoutez l’instruction `import` suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="ae012-116">Open the **GraphTutorial/menus/DrawerMenu.js** file and add the following `import` statement to the top of the file.</span></span>

    ```js
    import { AuthManager } from '../auth/AuthManager';
    ```

1. <span data-ttu-id="ae012-117">Dans `_onItemPressed`, remplacez la `// TEMPORARY` ligne par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="ae012-117">In `_onItemPressed`, replace the `// TEMPORARY` line with the following.</span></span>

    ```js
    await AuthManager.signOutAsync();
    ```

1. <span data-ttu-id="ae012-118">Enregistrez vos modifications et rechargez l’application dans votre émulateur.</span><span class="sxs-lookup"><span data-stu-id="ae012-118">Save your changes and reload the application in your emulator.</span></span>

<span data-ttu-id="ae012-119">Si vous vous connectez à l’application, un jeton d’accès s’affiche sur l’écran d' **Accueil** .</span><span class="sxs-lookup"><span data-stu-id="ae012-119">If you sign in to the app, you should see an access token displayed on the **Welcome** screen.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="ae012-120">Obtenir les détails de l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="ae012-120">Get user details</span></span>

<span data-ttu-id="ae012-121">Dans cette section, vous allez créer un fournisseur d’authentification personnalisé pour la bibliothèque client Graph, créer une classe d’assistance qui contiendra tous les appels à Microsoft Graph et `HomeScreen` mettre `DrawerMenuContent` à jour les classes et pour utiliser cette nouvelle classe pour obtenir l’utilisateur connecté.</span><span class="sxs-lookup"><span data-stu-id="ae012-121">In this section you will create a custom authentication provider for the Graph client library, create a helper class to hold all of the calls to Microsoft Graph and update the `HomeScreen` and `DrawerMenuContent` classes to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="ae012-122">Créez un répertoire dans le répertoire **GraphTutorial** nommé **Graph**.</span><span class="sxs-lookup"><span data-stu-id="ae012-122">Create a new directory in the **GraphTutorial** directory named **graph**.</span></span>
1. <span data-ttu-id="ae012-123">Créez un fichier dans le répertoire **GraphTutorial/Graph** nommé **GraphAuthProvider. js**.</span><span class="sxs-lookup"><span data-stu-id="ae012-123">Create a new file in the **GraphTutorial/graph** directory named **GraphAuthProvider.js**.</span></span> <span data-ttu-id="ae012-124">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="ae012-124">Add the following code to the file.</span></span>

    ```js
    import { AuthManager } from '../auth/AuthManager';

    // Used by Graph client to get access tokens
    // See https://github.com/microsoftgraph/msgraph-sdk-javascript/blob/dev/docs/CustomAuthenticationProvider.md
    export class GraphAuthProvider {
      getAccessToken = async() => {
        return await AuthManager.getAccessTokenAsync();
      }
    }
    ```

1. <span data-ttu-id="ae012-125">Créez un fichier dans le répertoire **GraphTutorial/Graph** nommé **GraphManager. js**.</span><span class="sxs-lookup"><span data-stu-id="ae012-125">Create a new file in the **GraphTutorial/graph** directory named **GraphManager.js**.</span></span> <span data-ttu-id="ae012-126">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="ae012-126">Add the following code to the file.</span></span>

    ```js
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
        return graphClient.api('/me').get();
      }
    }
    ```

1. <span data-ttu-id="ae012-127">Ouvrez le fichier **GraphTutorial/views/homescreen. js** et ajoutez l’instruction `import` suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="ae012-127">Open the **GraphTutorial/views/HomeScreen.js** file and add the following `import` statement to the top of the file.</span></span>

    ```js
    import { GraphManager } from '../graph/GraphManager';
    ```

1. <span data-ttu-id="ae012-128">Remplacez la `componentDidMount` méthode par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="ae012-128">Replace the `componentDidMount` method with the following.</span></span>

    ```js
    async componentDidMount() {
      try {
        // Get the signed-in user from Graph
        const user = await GraphManager.getUserAsync();
        // Set the user name to the user's given name
        this.setState({userName: user.givenName, userLoading: false});
      } catch (error) {
        alert(error);
      }
    }
    ```

1. <span data-ttu-id="ae012-129">Ouvrez le fichier **GraphTutorial/views/DrawerMenu. js** et ajoutez l’instruction `import` suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="ae012-129">Open the **GraphTutorial/views/DrawerMenu.js** file and add the following `import` statement to the top of the file.</span></span>

    ```js
    import { GraphManager } from '../graph/GraphManager';
    ```

1. <span data-ttu-id="ae012-130">Ajoutez la méthode `componentDidMount` suivante à la `DrawerMenuContent` classe.</span><span class="sxs-lookup"><span data-stu-id="ae012-130">Add the following `componentDidMount` method to the `DrawerMenuContent` class.</span></span>

    ```js
    async componentDidMount() {
      try {
        // Get the signed-in user from Graph
        const user = await GraphManager.getUserAsync();

        // Update UI with display name and email
        this.setState({
          userName: user.displayName,
          // Work/School accounts have email address in mail attribute
          // Personal accounts have it in userPrincipalName
          userEmail: user.mail !== null ? user.mail : user.userPrincipalName,
        });
      } catch(error) {
        alert(error);
      }
    }
    ```

<span data-ttu-id="ae012-131">Si vous enregistrez vos modifications et rechargez l’application maintenant, une fois connecté, l’interface utilisateur est mise à jour avec le nom d’affichage et l’adresse de messagerie de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ae012-131">If you save your changes and reload the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
