<!-- markdownlint-disable MD002 MD041 -->

Commencez par créer un projet Native REACT.

1. Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet. Exécutez la commande suivante pour installer l’outil [REACT-Native-CLI](https://github.com/facebook/react-native) et créer un projet Native REACT.

    ```Shell
    npm install -g react-native-cli
    react-native init GraphTutorial
    ```

1. **Facultatif :** Vérifiez que votre environnement de développement est correctement configuré en exécutant le projet. Dans votre interface CLI, modifiez le répertoire pour le répertoire **GraphTutorial** que vous venez de créer et exécutez l’une des commandes suivantes.

    - Pour iOS :`react-native run-ios`
    - Pour Android : lancer une instance d’émulateur Android et exécuter`react-native run-android`

## <a name="install-dependencies"></a>Installer les dépendances

Avant de poursuivre, installez des dépendances supplémentaires que vous utiliserez plus tard.

- [REACT : navigation](https://reactnavigation.org) permettant de gérer la navigation entre les affichages dans l’application.
- [REACT-Native-Handler](https://github.com/kmagiera/react-native-gesture-handler) et [REACT-Native-REANIMATION](https://github.com/kmagiera/react-native-reanimated), requis par REACT-navigation.
- [REACT-Native-Elements](https://react-native-training.github.io/react-native-elements/docs/getting_started.html) et [REACT-Native-vector-icons](https://github.com/oblador/react-native-vector-icons) pour fournir des icônes pour l’interface utilisateur.
- [REACT-Native-App-auth](https://github.com/FormidableLabs/react-native-app-auth) pour gérer l’authentification et la gestion des jetons.
- [moment](https://momentjs.com) à prendre en charge l’analyse et la comparaison des dates et des heures.
- [Microsoft-Graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour effectuer des appels à Microsoft Graph.

1. Ouvrez la CLI dans le répertoire racine de votre projet natif REACT.
1. Exécutez la commande suivante.

    ```Shell
    npm install react-navigation@3.11.1 react-native-gesture-handler@1.3.0 react-native-reanimated@1.1.0
    npm install react-native-elements@1.1.0 react-native-vector-icons@6.6.0 moment@2.24.0
    npm install react-native-app-auth@4.4.0 @microsoft/microsoft-graph-client@1.7.0
    react-native link react-native-vector-icons
    ```

### <a name="link-and-configure-dependencies-for-ios"></a>Liaison et configuration des dépendances pour iOS

> [!NOTE]
> Si vous ne ciblez pas iOS, vous pouvez ignorer cette section.

1. Ouvrez l’interface CLI dans le répertoire **GraphTutorial/iOS** .
1. Exécutez la commande suivante.

    ```Shell
    pod install
    ```

1. Ouvrez le **GraphTutorial/iOS/GraphTutorial/AppDelegate. h** dans un éditeur de texte. Remplacez son contenu par ce qui suit.

    ```obj-c
    #import <React/RCTBridgeDelegate.h>
    #import <UIKit/UIKit.h>
    #import "RNAppAuthAuthorizationFlowManager.h"

    @interface AppDelegate : UIResponder <UIApplicationDelegate, RCTBridgeDelegate, RNAppAuthAuthorizationFlowManager>

    @property (nonatomic, strong) UIWindow *window;
    @property (nonatomic, weak) id<RNAppAuthAuthorizationFlowManagerDelegate> authorizationFlowManagerDelegate;

    @end
    ```

### <a name="configure-dependencies-for-android"></a>Configurer les dépendances pour Android

> [!NOTE]
> Si vous ne ciblez pas Android, vous pouvez ignorer cette section.

1. Ouvrez le fichier **GraphTutorial/Android/App/Build. gradle** dans un éditeur.
1. Recherchez l' `defaultConfig` entrée et ajoutez la propriété suivante dans `defaultConfig`.

    ```Gradle
    manifestPlaceholders = [
        appAuthRedirectScheme: 'graph-tutorial'
    ]
    ```

1. Enregistrez le fichier. L' `defaultConfig` entrée doit ressembler à ce qui suit.

    ```Gradle
    defaultConfig {
        applicationId "com.graphtutorial"
        minSdkVersion rootProject.ext.minSdkVersion
        targetSdkVersion rootProject.ext.targetSdkVersion
        versionCode 1
        versionName "1.0"
        manifestPlaceholders = [
            appAuthRedirectScheme: 'graphtutorial'
        ]
    }
    ```

## <a name="design-the-app"></a>Concevoir l’application

L’application utilise un [tiroir de navigation](https://reactnavigation.org/docs/drawer-based-navigation.html) pour naviguer entre les différentes vues. Dans cette étape, vous allez créer les vues de base utilisées par l’application et mettre en œuvre le tiroir de navigation.

### <a name="create-views"></a>Créer des vues

Dans cette section, vous allez créer les affichages de l’application afin de prendre en charge un [flux d’authentification](https://reactnavigation.org/docs/auth-flow.html).

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **views**.
1. Créez un fichier dans le répertoire **GraphTutorial/views** nommé **homescreen. js**. Ajoutez le code suivant au fichier.

    ```JSX
    import React from 'react';
    import {
      ActivityIndicator,
      Text,
      StyleSheet,
      View,
    } from 'react-native';
    import { Icon } from 'react-native-elements';

    export default class HomeScreen extends React.Component {
      static navigationOptions = ({navigation}) => {
        return {
          title: 'Welcome',
          headerLeft: <Icon iconStyle={{ marginLeft: 10, color: 'white' }} size={30} name="menu" onPress={navigation.toggleDrawer} />
        };
      }

      state = {
        userLoading: true,
        userName: ''
      };

      render() {
        return (
          <View style={styles.container}>
            <ActivityIndicator animating={this.state.userLoading} size='large' />
            {this.state.userLoading ? null: <Text>Hello {this.state.userName}!</Text>}
          </View>
        );
      }
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        alignItems: 'center',
        justifyContent: 'center'
      }
    });
    ```

1. Créez un fichier dans le répertoire **GraphTutorial/views** nommé **CalendarScreen. js**. Ajoutez le code suivant au fichier.

    ```JSX
    import React from 'react';
    import {
      Text,
      StyleSheet,
      View,
    } from 'react-native';
    import { Icon } from 'react-native-elements';

    export default class CalendarScreen extends React.Component {
      static navigationOptions = ({navigation}) => {
        return {
          title: 'Calendar',
          headerLeft: <Icon iconStyle={{ marginLeft: 10, color: 'white' }} size={30} name="menu" onPress={navigation.toggleDrawer} />
        };
      }

      // Temporary placeholder view
      render() {
        return (
          <View style={styles.container}>
            <Text>Calendar</Text>
          </View>
        );
      }
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        alignItems: 'center',
        justifyContent: 'center'
      }
    });
    ```

1. Créez un fichier dans le répertoire **GraphTutorial/views** nommé **SignInScreen. js**. Ajoutez le code suivant au fichier.

    ```JSX
    // Adapted from https://reactnavigation.org/docs/auth-flow.html
    import React from 'react';
    import {
      Button,
      StyleSheet,
      View,
    } from 'react-native';

    export default class SignInScreen extends React.Component {
      static navigationOptions = {
        title: 'Please sign in',
      };

      _signInAsync = async () => {
        // TEMPORARY
        this.props.navigation.navigate('App');
      };

      render() {
        return (
          <View style={styles.container}>
            <Button title="Sign In" onPress={this._signInAsync}/>
          </View>
        );
      }
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        alignItems: 'center',
        justifyContent: 'center'
      }
    });
    ```

1. Créez un fichier dans le répertoire **GraphTutorial/views** nommé **AuthLoadingScreen. js**. Ajoutez le code suivant au fichier.

    ```JSX
    // Adapted from https://reactnavigation.org/docs/auth-flow.html
    import React from 'react';
    import {
      ActivityIndicator,
      AsyncStorage,
      Text,
      StyleSheet,
      View,
    } from 'react-native';

    export default class AuthLoadingScreen extends React.Component {

      componentDidMount() {
        this._bootstrapAsync();
      }

      // Fetch the token from storage then navigate to our appropriate place
      // NOTE: Production apps should protect user tokens in secure storage.
      _bootstrapAsync = async () => {
        const userToken = await AsyncStorage.getItem('userToken');

        // This will switch to the App screen or Auth screen and this loading
        // screen will be unmounted and thrown away.
        this.props.navigation.navigate(userToken ? 'App' : 'Auth');
      };

      render() {
        return (
          <View style={styles.container}>
            <ActivityIndicator size='large' />
            <Text style={styles.statusText}>Logging in...</Text>
          </View>
        );
      }
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        alignItems: 'center',
        justifyContent: 'center'
      },
      statusText: {
        marginTop: 10
      }
    });
    ```

### <a name="create-a-navigation-drawer"></a>Créer un tiroir de navigation

Dans cette section, vous allez créer un menu pour l’application et mettre à jour l’application pour utiliser la navigation REACT pour se déplacer entre les écrans.

1. Créez un répertoire dans le répertoire **GraphTutorial** **.**
1. Créez un fichier dans le répertoire **GraphTutorial/menus** nommé **DrawerMenu. js**. Ajoutez le code suivant au fichier.

    ```JSX
    import React from 'react';
    import {
      Image,
      SafeAreaView,
      ScrollView,
      StyleSheet,
      Text,
      View
    } from 'react-native';
    import { DrawerItems } from 'react-navigation';

    export default class DrawerMenuContent extends React.Component {

      state = {
        // TEMPORARY
        userName: 'Adele Vance',
        userEmail: 'adelev@contoso.com',
        userPhoto: require('../images/no-profile-pic.png')
      }

      // Check if user tapped on the Sign Out button and
      // sign the user out
      _onItemPressed = async (route) => {
        if (route.route.routeName !== 'Sign Out') {
          this.props.onItemPress(route);
          return;
        }

        // Sign out
        // TEMPORARY
        this.props.navigation.navigate('Auth');
      }

      render() {
        return (
          <ScrollView>
            <SafeAreaView style={styles.container}
              forceInset={{ top: 'always', horizontal: 'never' }}>
              <View style={styles.profileView}>
                <Image source={this.state.userPhoto}
                  resizeMode='contain'
                  style={styles.profilePhoto} />
                <Text style={styles.profileUserName}>{this.state.userName}</Text>
                <Text style={styles.profileEmail}>{this.state.userEmail}</Text>
              </View>
              <DrawerItems {...this.props}
                onItemPress={this._onItemPressed}/>
            </SafeAreaView>
          </ScrollView>
        );
      }
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1
      },
      profileView: {
        alignItems: 'center',
        padding: 10
      },
      profilePhoto: {
        width: 80,
        height: 80,
        borderRadius: 40
      },
      profileUserName: {
        fontWeight: '700'
      },
      profileEmail: {
        fontWeight: '200',
        fontSize: 10
      }
    });
    ```

1. Ouvrez le fichier **GraphTutorial/App. js** et remplacez tout le contenu par ce qui suit.

    ```JSX
    // Adapted from https://reactnavigation.org/docs/auth-flow.html
    import {
      createSwitchNavigator,
      createStackNavigator,
      createDrawerNavigator,
      createAppContainer } from "react-navigation";

    import AuthLoadingScreen from './views/AuthLoadingScreen';
    import SignInScreen from './views/SignInScreen';
    import HomeScreen from './views/HomeScreen';
    import CalendarScreen from './views/CalendarScreen';
    import DrawerMenuContent from './menus/DrawerMenu';

    const headerOptions = {
      defaultNavigationOptions: {
        headerStyle: {
          backgroundColor: '#276b80'
        },
        headerTintColor: 'white'
      }
    };

    const AuthStack = createStackNavigator(
      {
        SignIn: SignInScreen
      },
      headerOptions
    );

    const AppDrawer = createDrawerNavigator(
      {
        Home: createStackNavigator({ Home: HomeScreen }, headerOptions),
        Calendar: createStackNavigator({ Calendar: CalendarScreen }, headerOptions),
        'Sign Out': 'SignOut'
      },
      {
        hideStatusBar: false,
        contentComponent: DrawerMenuContent
      }
    );

    export default createAppContainer(createSwitchNavigator(
      {
        AuthLoading: AuthLoadingScreen,
        App: AppDrawer,
        Auth: AuthStack
      },
      {
        initialRouteName: 'AuthLoading'
      }
    ));

1. Save all of your changes.

1. Reload the application in your emulator.

The app's menu should work to navigate between the two fragments and change when you tap the **Sign in** or **Sign out** buttons.

![Screenshots of the application on Android](./images/android-app-screens.png)

![Screenshots of the application on iOS](./images/ios-app-screens.png)

> [!NOTE]
> You may receive warnings when running the app about Async Storage or componentWillUpdate. For the purposes of this tutorial, you can dismiss those warnings.
