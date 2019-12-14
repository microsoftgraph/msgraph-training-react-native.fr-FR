<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f0998-101">Commencez par créer un projet Native REACT.</span><span class="sxs-lookup"><span data-stu-id="f0998-101">Begin by creating a new React Native project.</span></span>

1. <span data-ttu-id="f0998-102">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="f0998-102">Open your command line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="f0998-103">Exécutez la commande suivante pour exécuter l’outil [REACT-Native-CLI](https://github.com/facebook/react-native) et créer un projet natif REACT.</span><span class="sxs-lookup"><span data-stu-id="f0998-103">Run the following command to run the [react-native-cli](https://github.com/facebook/react-native) tool and create a new React Native project.</span></span>

    ```Shell
    npx react-native init GraphTutorial
    ```

1. <span data-ttu-id="f0998-104">**Facultatif :** Vérifiez que votre environnement de développement est correctement configuré en exécutant le projet.</span><span class="sxs-lookup"><span data-stu-id="f0998-104">**Optional:** Verify that your development environment is configured correctly by running the project.</span></span> <span data-ttu-id="f0998-105">Dans votre interface CLI, modifiez le répertoire pour le répertoire **GraphTutorial** que vous venez de créer et exécutez l’une des commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="f0998-105">In your CLI, change the directory to the **GraphTutorial** directory you just created, and run one of the following commands.</span></span>

    - <span data-ttu-id="f0998-106">Pour iOS :`npx react-native run-ios`</span><span class="sxs-lookup"><span data-stu-id="f0998-106">For iOS: `npx react-native run-ios`</span></span>
    - <span data-ttu-id="f0998-107">Pour Android : lancer une instance d’émulateur Android et exécuter`npx react-native run-android`</span><span class="sxs-lookup"><span data-stu-id="f0998-107">For Android: Launch an Android emulator instance and run `npx react-native run-android`</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="f0998-108">Installer les dépendances</span><span class="sxs-lookup"><span data-stu-id="f0998-108">Install dependencies</span></span>

<span data-ttu-id="f0998-109">Avant de poursuivre, installez des dépendances supplémentaires que vous utiliserez plus tard.</span><span class="sxs-lookup"><span data-stu-id="f0998-109">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="f0998-110">[REACT : navigation](https://reactnavigation.org) permettant de gérer la navigation entre les affichages dans l’application.</span><span class="sxs-lookup"><span data-stu-id="f0998-110">[react-navigation](https://reactnavigation.org) to handle navigation between views in the app.</span></span>
- <span data-ttu-id="f0998-111">[REACT-Native-Handler](https://github.com/kmagiera/react-native-gesture-handler) et [REACT-Native-REANIMATION](https://github.com/kmagiera/react-native-reanimated), requis par REACT-navigation.</span><span class="sxs-lookup"><span data-stu-id="f0998-111">[react-native-gesture-handler](https://github.com/kmagiera/react-native-gesture-handler) and [react-native-reanimate](https://github.com/kmagiera/react-native-reanimated), required by react-navigation.</span></span>
- <span data-ttu-id="f0998-112">[REACT-Native-Elements](https://react-native-training.github.io/react-native-elements/docs/getting_started.html) et [REACT-Native-vector-icons](https://github.com/oblador/react-native-vector-icons) pour fournir des icônes pour l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f0998-112">[react-native-elements](https://react-native-training.github.io/react-native-elements/docs/getting_started.html) and [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons) to provide icons for the UI.</span></span>
- <span data-ttu-id="f0998-113">[REACT-Native-App-auth](https://github.com/FormidableLabs/react-native-app-auth) pour gérer l’authentification et la gestion des jetons.</span><span class="sxs-lookup"><span data-stu-id="f0998-113">[react-native-app-auth](https://github.com/FormidableLabs/react-native-app-auth) to handle authentication and token management.</span></span>
- <span data-ttu-id="f0998-114">[moment](https://momentjs.com) à prendre en charge l’analyse et la comparaison des dates et des heures.</span><span class="sxs-lookup"><span data-stu-id="f0998-114">[moment](https://momentjs.com) to handle parsing and comparison of dates and times.</span></span>
- <span data-ttu-id="f0998-115">[Microsoft-Graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f0998-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="f0998-116">Ouvrez la CLI dans le répertoire racine de votre projet natif REACT.</span><span class="sxs-lookup"><span data-stu-id="f0998-116">Open your CLI in the root directory of your React Native project.</span></span>
1. <span data-ttu-id="f0998-117">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="f0998-117">Run the following command.</span></span>

    ```Shell
    npm install react-navigation@3.11.1 react-native-gesture-handler@1.5.2 react-native-reanimated@1.4.0
    npm install react-native-elements@1.2.7 react-native-vector-icons@6.6.0 moment@2.24.0
    npm install react-native-app-auth@4.4.0 @microsoft/microsoft-graph-client@2.0.0
    ```

### <a name="link-and-configure-dependencies-for-ios"></a><span data-ttu-id="f0998-118">Liaison et configuration des dépendances pour iOS</span><span class="sxs-lookup"><span data-stu-id="f0998-118">Link and configure dependencies for iOS</span></span>

> [!NOTE]
> <span data-ttu-id="f0998-119">Si vous ne ciblez pas iOS, vous pouvez ignorer cette section.</span><span class="sxs-lookup"><span data-stu-id="f0998-119">If you are not targeting iOS, you can skip this section.</span></span>

1. <span data-ttu-id="f0998-120">Ouvrez l’interface CLI dans le répertoire **GraphTutorial/iOS** .</span><span class="sxs-lookup"><span data-stu-id="f0998-120">Open your CLI in the **GraphTutorial/ios** directory.</span></span>
1. <span data-ttu-id="f0998-121">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="f0998-121">Run the following command.</span></span>

    ```Shell
    pod install
    ```

1. <span data-ttu-id="f0998-122">Ouvrez le fichier **GraphTutorial/iOS/GraphTutorial/info. plist** dans un éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="f0998-122">Open the **GraphTutorial/ios/GraphTutorial/Info.plist** file in a text editor.</span></span> <span data-ttu-id="f0998-123">Ajoutez le code suivant juste avant la `</dict>` dernière ligne du fichier.</span><span class="sxs-lookup"><span data-stu-id="f0998-123">Add the following just before the last `</dict>` line in the file.</span></span>

    ```xml
    <key>UIAppFonts</key>
    <array>
      <string>AntDesign.ttf</string>
      <string>Entypo.ttf</string>
      <string>EvilIcons.ttf</string>
      <string>Feather.ttf</string>
      <string>FontAwesome.ttf</string>
      <string>FontAwesome5_Brands.ttf</string>
      <string>FontAwesome5_Regular.ttf</string>
      <string>FontAwesome5_Solid.ttf</string>
      <string>Foundation.ttf</string>
      <string>Ionicons.ttf</string>
      <string>MaterialIcons.ttf</string>
      <string>MaterialCommunityIcons.ttf</string>
      <string>SimpleLineIcons.ttf</string>
      <string>Octicons.ttf</string>
      <string>Zocial.ttf</string>
    </array>
    ```

1. <span data-ttu-id="f0998-124">Ouvrez le **GraphTutorial/iOS/GraphTutorial/AppDelegate. h** dans un éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="f0998-124">Open the **GraphTutorial/ios/GraphTutorial/AppDelegate.h** file in a text editor.</span></span> <span data-ttu-id="f0998-125">Remplacez son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="f0998-125">Replace its contents with the following.</span></span>

    ```obj-c
    #import <React/RCTBridgeDelegate.h>
    #import <UIKit/UIKit.h>
    #import "RNAppAuthAuthorizationFlowManager.h"

    @interface AppDelegate : UIResponder <UIApplicationDelegate, RCTBridgeDelegate, RNAppAuthAuthorizationFlowManager>

    @property (nonatomic, strong) UIWindow *window;
    @property (nonatomic, weak) id<RNAppAuthAuthorizationFlowManagerDelegate> authorizationFlowManagerDelegate;

    @end
    ```

### <a name="configure-dependencies-for-android"></a><span data-ttu-id="f0998-126">Configurer les dépendances pour Android</span><span class="sxs-lookup"><span data-stu-id="f0998-126">Configure dependencies for Android</span></span>

> [!NOTE]
> <span data-ttu-id="f0998-127">Si vous ne ciblez pas Android, vous pouvez ignorer cette section.</span><span class="sxs-lookup"><span data-stu-id="f0998-127">If you are not targeting Android, you can skip this section.</span></span>

1. <span data-ttu-id="f0998-128">Ouvrez le fichier **GraphTutorial/Android/App/Build. gradle** dans un éditeur.</span><span class="sxs-lookup"><span data-stu-id="f0998-128">Open the **GraphTutorial/android/app/build.gradle** file in an editor.</span></span>
1. <span data-ttu-id="f0998-129">Recherchez l' `defaultConfig` entrée et ajoutez la propriété suivante dans `defaultConfig`.</span><span class="sxs-lookup"><span data-stu-id="f0998-129">Locate the `defaultConfig` entry and add the following property inside `defaultConfig`.</span></span>

    ```Gradle
    manifestPlaceholders = [
        appAuthRedirectScheme: 'graph-tutorial'
    ]
    ```

    <span data-ttu-id="f0998-130">L' `defaultConfig` entrée doit ressembler à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="f0998-130">The `defaultConfig` entry should look similar to the following.</span></span>

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

1. <span data-ttu-id="f0998-131">Ajoutez la ligne suivante à la fin du fichier.</span><span class="sxs-lookup"><span data-stu-id="f0998-131">Add the following line to the end of the file.</span></span>

    ```Gradle
    apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
    ```

1. <span data-ttu-id="f0998-132">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="f0998-132">Save the file.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="f0998-133">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="f0998-133">Design the app</span></span>

<span data-ttu-id="f0998-134">L’application utilise un [tiroir de navigation](https://reactnavigation.org/docs/drawer-based-navigation.html) pour naviguer entre les différentes vues.</span><span class="sxs-lookup"><span data-stu-id="f0998-134">The application will use a [navigation drawer](https://reactnavigation.org/docs/drawer-based-navigation.html) to navigate between different views.</span></span> <span data-ttu-id="f0998-135">Dans cette étape, vous allez créer les vues de base utilisées par l’application et mettre en œuvre le tiroir de navigation.</span><span class="sxs-lookup"><span data-stu-id="f0998-135">In this step you will create the basic views used by the app and implement the navigation drawer.</span></span>

### <a name="create-views"></a><span data-ttu-id="f0998-136">Créer des vues</span><span class="sxs-lookup"><span data-stu-id="f0998-136">Create views</span></span>

<span data-ttu-id="f0998-137">Dans cette section, vous allez créer les affichages de l’application afin de prendre en charge un [flux d’authentification](https://reactnavigation.org/docs/auth-flow.html).</span><span class="sxs-lookup"><span data-stu-id="f0998-137">In this section you will create the views for the app to support an [authentication flow](https://reactnavigation.org/docs/auth-flow.html).</span></span>

1. <span data-ttu-id="f0998-138">Créez un répertoire dans le répertoire **GraphTutorial** nommé **views**.</span><span class="sxs-lookup"><span data-stu-id="f0998-138">Create a new directory in the **GraphTutorial** directory named **views**.</span></span>
1. <span data-ttu-id="f0998-139">Créez un fichier dans le répertoire **GraphTutorial/views** nommé **homescreen. js**.</span><span class="sxs-lookup"><span data-stu-id="f0998-139">Create a new file in the **GraphTutorial/views** directory named **HomeScreen.js**.</span></span> <span data-ttu-id="f0998-140">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="f0998-140">Add the following code to the file.</span></span>

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

1. <span data-ttu-id="f0998-141">Créez un fichier dans le répertoire **GraphTutorial/views** nommé **CalendarScreen. js**.</span><span class="sxs-lookup"><span data-stu-id="f0998-141">Create a new file in the **GraphTutorial/views** directory named **CalendarScreen.js**.</span></span> <span data-ttu-id="f0998-142">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="f0998-142">Add the following code to the file.</span></span>

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

1. <span data-ttu-id="f0998-143">Créez un fichier dans le répertoire **GraphTutorial/views** nommé **SignInScreen. js**.</span><span class="sxs-lookup"><span data-stu-id="f0998-143">Create a new file in the **GraphTutorial/views** directory named **SignInScreen.js**.</span></span> <span data-ttu-id="f0998-144">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="f0998-144">Add the following code to the file.</span></span>

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

1. <span data-ttu-id="f0998-145">Créez un fichier dans le répertoire **GraphTutorial/views** nommé **AuthLoadingScreen. js**.</span><span class="sxs-lookup"><span data-stu-id="f0998-145">Create a new file in the **GraphTutorial/views** directory named **AuthLoadingScreen.js**.</span></span> <span data-ttu-id="f0998-146">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="f0998-146">Add the following code to the file.</span></span>

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

### <a name="create-a-navigation-drawer"></a><span data-ttu-id="f0998-147">Créer un tiroir de navigation</span><span class="sxs-lookup"><span data-stu-id="f0998-147">Create a navigation drawer</span></span>

<span data-ttu-id="f0998-148">Dans cette section, vous allez créer un menu pour l’application et mettre à jour l’application pour utiliser la navigation REACT pour se déplacer entre les écrans.</span><span class="sxs-lookup"><span data-stu-id="f0998-148">In this section you will create a menu for the application, and update the application to use react-navigation to move between screens.</span></span>

1. <span data-ttu-id="f0998-149">Créez un répertoire dans le répertoire **GraphTutorial** **.**</span><span class="sxs-lookup"><span data-stu-id="f0998-149">Create a new directory in the **GraphTutorial** directory named **menus**.</span></span>
1. <span data-ttu-id="f0998-150">Créez un fichier dans le répertoire **GraphTutorial/menus** nommé **DrawerMenu. js**.</span><span class="sxs-lookup"><span data-stu-id="f0998-150">Create a new file in the **GraphTutorial/menus** directory named **DrawerMenu.js**.</span></span> <span data-ttu-id="f0998-151">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="f0998-151">Add the following code to the file.</span></span>

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

1. <span data-ttu-id="f0998-152">Créez un répertoire dans le répertoire **GraphTutorial** nommé **images**.</span><span class="sxs-lookup"><span data-stu-id="f0998-152">Create a new directory in the **GraphTutorial** directory named **images**.</span></span>
1. <span data-ttu-id="f0998-153">Ajoutez une image de profil par défaut nommée **no-Profile-pic. png** dans ce répertoire.</span><span class="sxs-lookup"><span data-stu-id="f0998-153">Add a default profile image named **no-profile-pic.png** in this directory.</span></span> <span data-ttu-id="f0998-154">Vous pouvez utiliser n’importe quelle image de votre choix ou utiliser [celle de cet exemple](https://github.com/microsoftgraph/msgraph-training-react-native/blob/master/demos/01-create-app/GraphTutorial/images/no-profile-pic.png).</span><span class="sxs-lookup"><span data-stu-id="f0998-154">You can use any image you like, or use [the one from this sample](https://github.com/microsoftgraph/msgraph-training-react-native/blob/master/demos/01-create-app/GraphTutorial/images/no-profile-pic.png).</span></span>

1. <span data-ttu-id="f0998-155">Ouvrez le fichier **GraphTutorial/App. js** et remplacez tout le contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="f0998-155">Open the **GraphTutorial/App.js** file and replace the entire contents with the following.</span></span>

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
    ```

1. <span data-ttu-id="f0998-156">Enregistrez toutes vos modifications.</span><span class="sxs-lookup"><span data-stu-id="f0998-156">Save all of your changes.</span></span>

1. <span data-ttu-id="f0998-157">Rechargez l’application dans votre émulateur.</span><span class="sxs-lookup"><span data-stu-id="f0998-157">Reload the application in your emulator.</span></span>

<span data-ttu-id="f0998-158">Le menu de l’application doit fonctionner pour naviguer entre les deux fragments et changer lorsque vous appuyez sur les boutons **se connecter** ou **se déconnecter** .</span><span class="sxs-lookup"><span data-stu-id="f0998-158">The app's menu should work to navigate between the two fragments and change when you tap the **Sign in** or **Sign out** buttons.</span></span>

![Captures d’écran de l’application sur Android](./images/android-app-screens.png)

![Captures d’écran de l’application sur iOS](./images/ios-app-screens.png)

> [!NOTE]
> <span data-ttu-id="f0998-161">Vous pouvez recevoir des avertissements lors de l’exécution de l’application sur le stockage Async ou componentWillUpdate.</span><span class="sxs-lookup"><span data-stu-id="f0998-161">You may receive warnings when running the app about Async Storage or componentWillUpdate.</span></span> <span data-ttu-id="f0998-162">Pour les besoins de ce didacticiel, vous pouvez ignorer ces avertissements.</span><span class="sxs-lookup"><span data-stu-id="f0998-162">For the purposes of this tutorial, you can dismiss those warnings.</span></span>
