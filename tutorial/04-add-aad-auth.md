<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD. Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph. Pour ce faire, vous allez intégrer la bibliothèque d' [authentification REACT-Native-App à](https://github.com/FormidableLabs/react-native-app-auth) l’application.

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **auth**.
1. Créez un fichier dans le répertoire **GraphTutorial/auth** nommé **authconfig. TS**. Ajoutez le code suivant au fichier.

    :::code language="typescript" source="../demo/GraphTutorial/auth/AuthConfig.example.ts":::

    Remplacez `YOUR_APP_ID_HERE` par l’ID d’application de l’inscription de votre application.

> [!IMPORTANT]
> Si vous utilisez le contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **authconfig. TS** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.

## <a name="implement-sign-in"></a>Implémentation de la connexion

Dans cette section, vous allez créer une classe d’assistance d’authentification et mettre à jour l’application pour se connecter et se déconnecter.

1. Créez un fichier dans le répertoire **GraphTutorial/auth** nommé **AuthManager. TS**. Ajoutez le code suivant au fichier.

    :::code language="typescript" source="../demo/GraphTutorial/auth/AuthManager.ts" id="AuthManagerSnippet":::

1. Ouvrez le fichier **GraphTutorial/App. TSX** et ajoutez l' `import` instruction suivante en haut du fichier.

    ```typescript
    import { AuthManager } from './auth/AuthManager';
    ```

1. Remplacez la `authContext` déclaration existante par la suivante.

    :::code language="typescript" source="../demo/GraphTutorial/App.tsx" id="AuthContextSnippet" highlight="4-6,9":::

1. Ouvrez le fichier **GraphTutorial/menus/DrawerMenu. TSX** et ajoutez l' `import` instruction suivante en haut du fichier.

    ```typescript
    import { AuthManager } from '../auth/AuthManager';
    ```

1. Ajoutez le code suivant à la `componentDidMount` fonction.

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

1. Enregistrez vos modifications et rechargez l’application dans votre émulateur.

Si vous vous connectez à l’application, un jeton d’accès s’affiche sur l’écran d' **Accueil** .

## <a name="get-user-details"></a>Obtenir les détails de l’utilisateur

Dans cette section, vous allez créer un fournisseur d’authentification personnalisé pour la bibliothèque client Graph, créer une classe d’assistance qui contiendra tous les appels à Microsoft Graph et mettre à jour la `DrawerMenuContent` classe pour utiliser cette nouvelle classe afin d’obtenir l’utilisateur connecté.

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **Graph**.
1. Créez un fichier dans le répertoire **GraphTutorial/Graph** nommé **GraphAuthProvider. TS**. Ajoutez le code suivant au fichier.

    :::code language="typescript" source="../demo/GraphTutorial/graph/GraphAuthProvider.ts" id="AuthProviderSnippet":::

1. Créez un fichier dans le répertoire **GraphTutorial/Graph** nommé **GraphManager. TS**. Ajoutez le code suivant au fichier.

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

1. Ouvrez le fichier **GraphTutorial/views/DrawerMenu. TSX** et ajoutez l' `import` instruction suivante en haut du fichier.

    ```typescript
    import { GraphManager } from '../graph/GraphManager';
    ```

1. Remplacez la `componentDidMount` méthode par ce qui suit.

    :::code language="typescript" source="../demo/GraphTutorial/menus/DrawerMenu.tsx" id="ComponentDidMountSnippet":::

Si vous enregistrez vos modifications et rechargez l’application maintenant, une fois connecté, l’interface utilisateur est mise à jour avec le nom d’affichage et l’adresse de messagerie de l’utilisateur.
