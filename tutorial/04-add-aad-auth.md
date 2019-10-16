<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD. Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph. Pour ce faire, vous allez intégrer la bibliothèque d' [authentification REACT-Native-App à](https://github.com/FormidableLabs/react-native-app-auth) l’application.

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **auth**.
1. Créez un fichier dans le répertoire **GraphTutorial/auth** nommé **authconfig. js**. Ajoutez le code suivant au fichier.

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

    Remplacez `YOUR_APP_ID_HERE` par l’ID d’application de l’inscription de votre application.

> [!IMPORTANT]
> Si vous utilisez le contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **authconfig. js** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.

## <a name="implement-sign-in"></a>Mettre en œuvre la connexion

Dans cette section, vous allez créer une classe d’assistance d’authentification et mettre à jour l’application pour se connecter et se déconnecter.

1. Créez un fichier dans le répertoire **GraphTutorial/auth** nommé **AuthManager. js**. Ajoutez le code suivant au fichier.

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

1. Ouvrez le fichier **GraphTutorial/views/SignInScreen. js** et ajoutez l’instruction `import` suivante en haut du fichier.

    ```js
    import { AuthManager } from '../auth/AuthManager';
    ```

1. Remplacez la méthode `_signInAsync` existante par ce qui suit.

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

1. Ajoutez la méthode suivante à la classe `HomeScreen`.

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

1. Ouvrez le fichier **GraphTutorial/menus/DrawerMenu. js** et ajoutez l’instruction `import` suivante en haut du fichier.

    ```js
    import { AuthManager } from '../auth/AuthManager';
    ```

1. Dans `_onItemPressed`, remplacez la `// TEMPORARY` ligne par ce qui suit.

    ```js
    await AuthManager.signOutAsync();
    ```

1. Enregistrez vos modifications et rechargez l’application dans votre émulateur.

Si vous vous connectez à l’application, un jeton d’accès s’affiche sur l’écran d' **Accueil** .

## <a name="get-user-details"></a>Obtenir les détails de l’utilisateur

Dans cette section, vous allez créer un fournisseur d’authentification personnalisé pour la bibliothèque client Graph, créer une classe d’assistance qui contiendra tous les appels à Microsoft Graph et `HomeScreen` mettre `DrawerMenuContent` à jour les classes et pour utiliser cette nouvelle classe pour obtenir l’utilisateur connecté.

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **Graph**.
1. Créez un fichier dans le répertoire **GraphTutorial/Graph** nommé **GraphAuthProvider. js**. Ajoutez le code suivant au fichier.

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

1. Créez un fichier dans le répertoire **GraphTutorial/Graph** nommé **GraphManager. js**. Ajoutez le code suivant au fichier.

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

1. Ouvrez le fichier **GraphTutorial/views/homescreen. js** et ajoutez l’instruction `import` suivante en haut du fichier.

    ```js
    import { GraphManager } from '../graph/GraphManager';
    ```

1. Remplacez la `componentDidMount` méthode par ce qui suit.

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

1. Ouvrez le fichier **GraphTutorial/views/DrawerMenu. js** et ajoutez l’instruction `import` suivante en haut du fichier.

    ```js
    import { GraphManager } from '../graph/GraphManager';
    ```

1. Ajoutez la méthode `componentDidMount` suivante à la `DrawerMenuContent` classe.

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

Si vous enregistrez vos modifications et rechargez l’application maintenant, une fois connecté, l’interface utilisateur est mise à jour avec le nom d’affichage et l’adresse de messagerie de l’utilisateur.
