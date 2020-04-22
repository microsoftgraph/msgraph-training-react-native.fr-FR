<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez étendre l’application de l’exercice précédent pour prendre en charge l’authentification avec Azure AD. Cela est nécessaire pour obtenir le jeton d’accès OAuth nécessaire pour appeler Microsoft Graph. Pour ce faire, vous allez intégrer la bibliothèque d' [authentification REACT-Native-App à](https://github.com/FormidableLabs/react-native-app-auth) l’application.

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **auth**.
1. Créez un fichier dans le répertoire **GraphTutorial/auth** nommé **authconfig. TS**. Ajoutez le code suivant au fichier.

    :::code language="typescript" source="../demo/GraphTutorial/auth/AuthConfig.ts.example":::

    Remplacez `YOUR_APP_ID_HERE` par l’ID d’application de l’inscription de votre application.

> [!IMPORTANT]
> Si vous utilisez le contrôle de code source tel que git, il est maintenant recommandé d’exclure le fichier **authconfig. TS** du contrôle de code source afin d’éviter une fuite accidentelle de votre ID d’application.

## <a name="implement-sign-in"></a>Implémentation de la connexion

Dans cette section, vous allez créer une classe d’assistance d’authentification et mettre à jour l’application pour se connecter et se déconnecter.

1. Créez un fichier dans le répertoire **GraphTutorial/auth** nommé **AuthManager. TS**. Ajoutez le code suivant au fichier.

    :::code language="typescript" source="../demo/GraphTutorial/auth/AuthManager.ts" id="AuthManagerSnippet":::

1. Ouvrez le fichier **GraphTutorial/views/SignInScreen. TSX** et ajoutez l’instruction `import` suivante en haut du fichier.

    ```typescript
    import { AuthManager } from '../auth/AuthManager';
    ```

1. Remplacez la méthode `_signInAsync` existante par ce qui suit.

    :::code language="typescript" source="../demo/GraphTutorial/screens/SignInScreen.tsx" id="SignInAsyncSnippet":::

1. Ouvrez le fichier **GraphTutorial/views/homescreen. TSX** et ajoutez l’instruction `import` suivante en haut du fichier.

    ```typescript
    import { AuthManager } from '../auth/AuthManager';
    ```

1. Ajoutez la méthode suivante à la classe `HomeScreen`.

    ```typescript
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

1. Ouvrez le fichier **GraphTutorial/menus/DrawerMenu. TSX** et ajoutez l’instruction `import` suivante en haut du fichier.

    ```typescript
    import { AuthManager } from '../auth/AuthManager';
    ```

1. Remplacez la méthode `_signOut` existante par ce qui suit.

    :::code language="typescript" source="../demo/GraphTutorial/menus/DrawerMenu.tsx" id="SignOutSnippet" highlight="5":::

1. Enregistrez vos modifications et rechargez l’application dans votre émulateur.

Si vous vous connectez à l’application, un jeton d’accès s’affiche sur l’écran d' **Accueil** .

## <a name="get-user-details"></a>Obtenir les détails de l’utilisateur

Dans cette section, vous allez créer un fournisseur d’authentification personnalisé pour la bibliothèque client Graph, créer une classe d’assistance qui contiendra tous les appels à Microsoft Graph et `HomeScreen` mettre `DrawerMenuContent` à jour les classes et pour utiliser cette nouvelle classe pour obtenir l’utilisateur connecté.

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
        return graphClient.api('/me').get();
      }
    }
    ```

1. Ouvrez le fichier **GraphTutorial/views/homescreen. TSX** et ajoutez l’instruction `import` suivante en haut du fichier.

    ```typescript
    import { GraphManager } from '../graph/GraphManager';
    ```

1. Remplacez la `componentDidMount` méthode par ce qui suit.

    :::code language="typescript" source="../demo/GraphTutorial/screens/HomeScreen.tsx" id="ComponentDidMountSnippet" highlight="3-6,9":::

1. Ouvrez le fichier **GraphTutorial/views/DrawerMenu. TSX** et ajoutez l’instruction `import` suivante en haut du fichier.

    ```typescript
    import { GraphManager } from '../graph/GraphManager';
    ```

1. Ajoutez la méthode `componentDidMount` suivante à la classe `DrawerMenuContent`.

    :::code language="typescript" source="../demo/GraphTutorial/menus/DrawerMenu.tsx" id="ComponentDidMountSnippet":::

Si vous enregistrez vos modifications et rechargez l’application maintenant, une fois connecté, l’interface utilisateur est mise à jour avec le nom d’affichage et l’adresse de messagerie de l’utilisateur.
