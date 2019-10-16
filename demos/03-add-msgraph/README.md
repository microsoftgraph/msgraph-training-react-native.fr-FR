# <a name="how-to-run-the-completed-project"></a>Exécution du projet terminé

## <a name="prerequisites"></a>Conditions préalables

Pour exécuter le projet terminé dans ce dossier, vous avez besoin des éléments suivants :

- Au moins l’un des éléments suivants :
  - Kit de développement [Android Studio](https://developer.android.com/studio/) **et** [java](https://jdk.java.net) (requis pour exécuter l’exemple sur Android)
  - [Xcode](https://developer.apple.com/xcode/) (requis pour exécuter l’exemple sur iOS)
- [Node.js](https://nodejs.org)
- Soit un compte Microsoft personnel avec une boîte aux lettres sur Outlook.com, soit un compte professionnel ou scolaire Microsoft.

> **Remarque :** Ce didacticiel a été écrit à l’aide de l’interface CLI native de REACT, qui présente des conditions préalables spécifiques en fonction de votre système d’exploitation et des plateformes cibles. Pour obtenir des instructions sur la configuration de votre ordinateur de développement, voir [REACT Native Getting Started](https://facebook.github.io/react-native/docs/getting-started) . Les versions utilisées pour ce didacticiel sont répertoriées ci-dessous. Les étapes de ce guide peuvent fonctionner avec d’autres versions, mais cela n’a pas été testé.
>
> - Android Studio version 3.4.2 avec 1.8.0 JRE et le kit de développement logiciel (SDK) Android 9,0
> - Kit de développement Java version 12.0.2
> - Xcode version 10,3
> - Node. js version 10.16.0

Si vous n’avez pas de compte Microsoft, vous disposez de deux options pour obtenir un compte gratuit :

- Vous pouvez vous [inscrire pour obtenir un nouveau compte Microsoft personnel](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Vous pouvez vous [inscrire au programme pour les développeurs office 365](https://developer.microsoft.com/office/dev-program) pour obtenir un abonnement gratuit à Office 365.

## <a name="register-an-application-with-the-azure-active-directory-admin-center"></a>Enregistrer une application avec le centre d’administration Azure Active Directory

1. Ouvrez un navigateur, accédez au [Centre d’administration Azure Active Directory ](https://aad.portal.azure.com) et connectez-vous à l’aide d’un **compte personnel** (ou compte Microsoft) ou d’un **compte professionnel ou scolaire**.

1. Sélectionnez **Azure Active Directory** dans le volet de navigation de gauche, puis sélectionnez **inscriptions des applications** sous **gérer**.

    ![Capture d’écran des inscriptions d’application ](/tutorial/images/aad-portal-app-registrations.png)

1. Sélectionnez **Nouvelle inscription**. Sur la page **Inscrire une application**, définissez les valeurs comme suit.

    - Définissez le **Nom** sur `React Native Graph Tutorial`.
    - Définissez les **Types de comptes pris en charge** sur **Comptes dans un annuaire organisationnel et comptes personnels Microsoft**.
    - Sous **URI de redirection**, remplacez la liste déroulante par **client Public (mobile & Desktop)** et définissez `graph-tutorial://react-native-auth`la valeur sur.

    ![Capture d’écran de la page inscrire une application](/tutorial/images/aad-register-an-app.png)

1. Sélectionnez **Enregistrer**. Sur la page **didacticiel de graphique Native REACT** , copiez la valeur de l' **ID d’application (client)** et enregistrez-la, vous en aurez besoin à l’étape suivante.

    ![Capture d’écran de l’ID d’application de la nouvelle inscription de l’application](/tutorial/images/aad-application-id.png)

1. Sous **gérer**, sélectionnez **authentification**. Sur la page **URI de redirection** , ajoutez un autre URI de redirection de type **public client (mobile & Desktop)**, `urn:ietf:wg:oauth:2.0:oob`avec l’URI. Cliquez sur **Enregistrer**.

    ![Capture d’écran de la page des URI de redirection](/tutorial/images/aad-redirect-uris.png)

## <a name="configure-the-sample"></a>Configurer l’exemple

1. Renommez le fichier **GraphTutorial/auth/authconfig. js. example** en **authconfig. js**.
1. Modifiez le fichier **authconfig. js** et effectuez les modifications suivantes.
    1. Remplacez `YOUR_APP_ID_HERE` par l' **ID d’application** que vous avez obtenu à partir du portail d’inscription des applications.

1. Dans votre interface de ligne de commande (CLI), accédez au répertoire **GraphTutorial** et exécutez la commande suivante pour installer les conditions requises.

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a>Exécution de l’exemple

### <a name="run-on-ios-simulator"></a>Exécuter sur le simulateur iOS

Exécutez la commande suivante dans votre interface CLI pour démarrer l’application dans un simulateur iOS.

```Shell
react-native run-ios
```

### <a name="run-on-android-emulator"></a>Exécuter sur l’émulateur Android

Instance de lancement et d’émulateur Android et exécutez la commande suivante dans votre interface CLI pour démarrer l’application dans un émulateur Android.

```Shell
react-native run-android
```
