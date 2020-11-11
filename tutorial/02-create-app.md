<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="063c8-101">Commencez par créer un projet Native REACT.</span><span class="sxs-lookup"><span data-stu-id="063c8-101">Begin by creating a new React Native project.</span></span>

1. <span data-ttu-id="063c8-102">Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet.</span><span class="sxs-lookup"><span data-stu-id="063c8-102">Open your command line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="063c8-103">Exécutez la commande suivante pour exécuter l’outil [REACT-Native-CLI](https://github.com/facebook/react-native) et créer un projet natif REACT.</span><span class="sxs-lookup"><span data-stu-id="063c8-103">Run the following command to run the [react-native-cli](https://github.com/facebook/react-native) tool and create a new React Native project.</span></span>

    ```Shell
    npx react-native init GraphTutorial --template react-native-template-typescript
    ```

1. <span data-ttu-id="063c8-104">**Facultatif :** Vérifiez que votre environnement de développement est correctement configuré en exécutant le projet.</span><span class="sxs-lookup"><span data-stu-id="063c8-104">**Optional:** Verify that your development environment is configured correctly by running the project.</span></span> <span data-ttu-id="063c8-105">Dans votre interface CLI, modifiez le répertoire pour le répertoire **GraphTutorial** que vous venez de créer et exécutez l’une des commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="063c8-105">In your CLI, change the directory to the **GraphTutorial** directory you just created, and run one of the following commands.</span></span>

    - <span data-ttu-id="063c8-106">Pour iOS : `npx react-native run-ios`</span><span class="sxs-lookup"><span data-stu-id="063c8-106">For iOS: `npx react-native run-ios`</span></span>
    - <span data-ttu-id="063c8-107">Pour Android : lancer une instance d’émulateur Android et exécuter `npx react-native run-android`</span><span class="sxs-lookup"><span data-stu-id="063c8-107">For Android: Launch an Android emulator instance and run `npx react-native run-android`</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="063c8-108">Installer les dépendances</span><span class="sxs-lookup"><span data-stu-id="063c8-108">Install dependencies</span></span>

<span data-ttu-id="063c8-109">Avant de poursuivre, installez des dépendances supplémentaires que vous utiliserez plus tard.</span><span class="sxs-lookup"><span data-stu-id="063c8-109">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="063c8-110">[REACT : navigation](https://reactnavigation.org) permettant de gérer la navigation entre les affichages dans l’application.</span><span class="sxs-lookup"><span data-stu-id="063c8-110">[react-navigation](https://reactnavigation.org) to handle navigation between views in the app.</span></span>
- <span data-ttu-id="063c8-111">[REACT-Native-descripteur](https://github.com/kmagiera/react-native-gesture-handler), [REACT-Native-Safe-Context](https://github.com/th3rdwave/react-native-safe-area-context), [REACT-Native-screens](https://github.com/kmagiera/react-native-screens), [REACT-Native-REANIMATION](https://github.com/kmagiera/react-native-reanimated)et [Masked-View](https://github.com/react-native-community/react-native-masked-view) requis par REACT-navigation.</span><span class="sxs-lookup"><span data-stu-id="063c8-111">[react-native-gesture-handler](https://github.com/kmagiera/react-native-gesture-handler), [react-native-safe-area-context](https://github.com/th3rdwave/react-native-safe-area-context), [react-native-screens](https://github.com/kmagiera/react-native-screens), [react-native-reanimate](https://github.com/kmagiera/react-native-reanimated), and [masked-view](https://github.com/react-native-community/react-native-masked-view) required by react-navigation.</span></span>
- <span data-ttu-id="063c8-112">[REACT-Native-Elements](https://reactnativeelements.com/docs/) et [REACT-Native-vector-icons](https://github.com/oblador/react-native-vector-icons) pour fournir des icônes pour l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="063c8-112">[react-native-elements](https://reactnativeelements.com/docs/) and [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons) to provide icons for the UI.</span></span>
- <span data-ttu-id="063c8-113">[REACT-Native-App-auth](https://github.com/FormidableLabs/react-native-app-auth) pour gérer l’authentification et la gestion des jetons.</span><span class="sxs-lookup"><span data-stu-id="063c8-113">[react-native-app-auth](https://github.com/FormidableLabs/react-native-app-auth) to handle authentication and token management.</span></span>
- <span data-ttu-id="063c8-114">[Async : stockage pour le](https://react-native-async-storage.github.io/async-storage/docs/install) stockage des jetons.</span><span class="sxs-lookup"><span data-stu-id="063c8-114">[async-storage](https://react-native-async-storage.github.io/async-storage/docs/install) to provide storage for tokens.</span></span>
- <span data-ttu-id="063c8-115">[DateTimePicker](https://github.com/react-native-datetimepicker/datetimepicker) pour ajouter des sélecteurs de date et heure à l’interface utilisateur.</span><span class="sxs-lookup"><span data-stu-id="063c8-115">[datetimepicker](https://github.com/react-native-datetimepicker/datetimepicker) to add date and time pickers to the UI.</span></span>
- <span data-ttu-id="063c8-116">[moment](https://momentjs.com) à prendre en charge l’analyse et la comparaison des dates et des heures.</span><span class="sxs-lookup"><span data-stu-id="063c8-116">[moment](https://momentjs.com) to handle parsing and comparison of dates and times.</span></span>
- <span data-ttu-id="063c8-117">[Windows-IANA](https://github.com/rubenillodo/windows-iana) pour traduire les fuseaux horaires Windows au format IANA.</span><span class="sxs-lookup"><span data-stu-id="063c8-117">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zones to IANA format.</span></span>
- <span data-ttu-id="063c8-118">[Microsoft-Graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour effectuer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="063c8-118">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="063c8-119">Ouvrez la CLI dans le répertoire racine de votre projet natif REACT.</span><span class="sxs-lookup"><span data-stu-id="063c8-119">Open your CLI in the root directory of your React Native project.</span></span>
1. <span data-ttu-id="063c8-120">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="063c8-120">Run the following command.</span></span>

    ```Shell
    npm install @react-navigation/native@5.8.8 @react-navigation/drawer@5.11.1 @react-navigation/stack@5.12.5
    npm install @react-native-community/masked-view@0.1.10 react-native-safe-area-context@3.1.8 windows-iana
    npm install react-native-reanimated@1.13.1 react-native-screens@2.14.0 @react-native-async-storage/async-storage@1.13.2
    npm install react-native-elements@2.3.2 react-native-vector-icons@7.1.0 react-native-gesture-handler@1.8.0
    npm install react-native-app-auth@6.0.1 moment@2.29.1 moment-timezone @microsoft/microsoft-graph-client@2.1.1
    npm install @react-native-community/datetimepicker@3.0.4
    npm install @microsoft/microsoft-graph-types --save-dev
    ```

### <a name="link-and-configure-dependencies-for-ios"></a><span data-ttu-id="063c8-121">Liaison et configuration des dépendances pour iOS</span><span class="sxs-lookup"><span data-stu-id="063c8-121">Link and configure dependencies for iOS</span></span>

> [!NOTE]
> <span data-ttu-id="063c8-122">Si vous ne ciblez pas iOS, vous pouvez ignorer cette section.</span><span class="sxs-lookup"><span data-stu-id="063c8-122">If you are not targeting iOS, you can skip this section.</span></span>

1. <span data-ttu-id="063c8-123">Ouvrez l’interface CLI dans le répertoire **GraphTutorial/iOS** .</span><span class="sxs-lookup"><span data-stu-id="063c8-123">Open your CLI in the **GraphTutorial/ios** directory.</span></span>
1. <span data-ttu-id="063c8-124">Exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="063c8-124">Run the following command.</span></span>

    ```Shell
    pod install
    ```

1. <span data-ttu-id="063c8-125">Ouvrez le fichier **GraphTutorial/iOS/GraphTutorial/info. plist** dans un éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="063c8-125">Open the **GraphTutorial/ios/GraphTutorial/Info.plist** file in a text editor.</span></span> <span data-ttu-id="063c8-126">Ajoutez le code suivant juste avant la dernière `</dict>` ligne du fichier.</span><span class="sxs-lookup"><span data-stu-id="063c8-126">Add the following just before the last `</dict>` line in the file.</span></span>

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

1. <span data-ttu-id="063c8-127">Ouvrez le **GraphTutorial/iOS/GraphTutorial/AppDelegate. h** dans un éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="063c8-127">Open the **GraphTutorial/ios/GraphTutorial/AppDelegate.h** file in a text editor.</span></span> <span data-ttu-id="063c8-128">Remplacez son contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="063c8-128">Replace its contents with the following.</span></span>

    :::code language="objc" source="../demo/GraphTutorial/ios/GraphTutorial/AppDelegate.h":::

### <a name="configure-dependencies-for-android"></a><span data-ttu-id="063c8-129">Configurer les dépendances pour Android</span><span class="sxs-lookup"><span data-stu-id="063c8-129">Configure dependencies for Android</span></span>

> [!NOTE]
> <span data-ttu-id="063c8-130">Si vous ne ciblez pas Android, vous pouvez ignorer cette section.</span><span class="sxs-lookup"><span data-stu-id="063c8-130">If you are not targeting Android, you can skip this section.</span></span>

1. <span data-ttu-id="063c8-131">Ouvrez le fichier **GraphTutorial/Android/App/Build. gradle** dans un éditeur.</span><span class="sxs-lookup"><span data-stu-id="063c8-131">Open the **GraphTutorial/android/app/build.gradle** file in an editor.</span></span>
1. <span data-ttu-id="063c8-132">Recherchez l' `defaultConfig` entrée et ajoutez la propriété suivante dans `defaultConfig` .</span><span class="sxs-lookup"><span data-stu-id="063c8-132">Locate the `defaultConfig` entry and add the following property inside `defaultConfig`.</span></span>

    ```Gradle
    manifestPlaceholders = [
        appAuthRedirectScheme: 'graph-tutorial'
    ]
    ```

    <span data-ttu-id="063c8-133">L' `defaultConfig` entrée doit ressembler à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="063c8-133">The `defaultConfig` entry should look similar to the following.</span></span>

    :::code language="gradle" source="../demo/GraphTutorial/android/app/build.gradle" id="DefaultConfigSnippet":::

1. <span data-ttu-id="063c8-134">Ajoutez la ligne suivante à la fin du fichier.</span><span class="sxs-lookup"><span data-stu-id="063c8-134">Add the following line to the end of the file.</span></span>

    ```Gradle
    apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
    ```

1. <span data-ttu-id="063c8-135">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="063c8-135">Save the file.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="063c8-136">Concevoir l’application</span><span class="sxs-lookup"><span data-stu-id="063c8-136">Design the app</span></span>

<span data-ttu-id="063c8-137">L’application utilise un [tiroir de navigation](https://reactnavigation.org/docs/drawer-based-navigation.html) pour naviguer entre les différentes vues.</span><span class="sxs-lookup"><span data-stu-id="063c8-137">The application will use a [navigation drawer](https://reactnavigation.org/docs/drawer-based-navigation.html) to navigate between different views.</span></span> <span data-ttu-id="063c8-138">Dans cette étape, vous allez créer les vues de base utilisées par l’application et mettre en œuvre le tiroir de navigation.</span><span class="sxs-lookup"><span data-stu-id="063c8-138">In this step you will create the basic views used by the app and implement the navigation drawer.</span></span>

### <a name="create-views"></a><span data-ttu-id="063c8-139">Créer des vues</span><span class="sxs-lookup"><span data-stu-id="063c8-139">Create views</span></span>

<span data-ttu-id="063c8-140">Dans cette section, vous allez créer les affichages de l’application afin de prendre en charge un [flux d’authentification](https://reactnavigation.org/docs/auth-flow).</span><span class="sxs-lookup"><span data-stu-id="063c8-140">In this section you will create the views for the app to support an [authentication flow](https://reactnavigation.org/docs/auth-flow).</span></span>

1. <span data-ttu-id="063c8-141">Ouvrez **GraphTutorial/index.js** et ajoutez la commande suivante en haut du fichier, avant toute autre `import` instruction.</span><span class="sxs-lookup"><span data-stu-id="063c8-141">Open **GraphTutorial/index.js** and add the following to the top of the file, before any other `import` statements.</span></span>

    ```javascript
    import 'react-native-gesture-handler';
    ```

1. <span data-ttu-id="063c8-142">Créez un fichier dans le répertoire **GraphTutorial** nommé **AuthContext. TSX** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="063c8-142">Create a new file in the **GraphTutorial** directory named **AuthContext.tsx** and add the following code.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/AuthContext.tsx" id="AuthContextSnippet":::

1. <span data-ttu-id="063c8-143">Créez un fichier dans le répertoire **GraphTutorial** nommé **UserContext. TSX** et ajoutez le code suivant.</span><span class="sxs-lookup"><span data-stu-id="063c8-143">Create a new file in the **GraphTutorial** directory named **UserContext.tsx** and add the following code.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/UserContext.tsx" id="UserContextSnippet":::

1. <span data-ttu-id="063c8-144">Créez un répertoire dans le répertoire **GraphTutorial** nommé **écrans**.</span><span class="sxs-lookup"><span data-stu-id="063c8-144">Create a new directory in the **GraphTutorial** directory named **screens**.</span></span>
1. <span data-ttu-id="063c8-145">Créez un fichier dans le répertoire **GraphTutorial/screens** nommé **homescreen. TSX**.</span><span class="sxs-lookup"><span data-stu-id="063c8-145">Create a new file in the **GraphTutorial/screens** directory named **HomeScreen.tsx**.</span></span> <span data-ttu-id="063c8-146">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="063c8-146">Add the following code to the file.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/screens/HomeScreen.tsx" id="HomeScreenSnippet":::

1. <span data-ttu-id="063c8-147">Créez un fichier dans le répertoire **GraphTutorial/screens** nommé **CalendarScreen. TSX**.</span><span class="sxs-lookup"><span data-stu-id="063c8-147">Create a new file in the **GraphTutorial/screens** directory named **CalendarScreen.tsx**.</span></span> <span data-ttu-id="063c8-148">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="063c8-148">Add the following code to the file.</span></span>

    ```typescript
    import React from 'react';
    import {
      Text,
      StyleSheet,
      View,
    } from 'react-native';
    import { createStackNavigator } from '@react-navigation/stack';

    const Stack = createStackNavigator();

    // Temporary placeholder view
    const CalendarComponent = () => (
      <View style={styles.container}>
        <Text>Calendar</Text>
      </View>
    );

    export default class CalendarScreen extends React.Component {

      render() {
        return (
          <Stack.Navigator>
            <Stack.Screen name='Calendar'
              component={ CalendarComponent }
              options={{
                headerShown: false
              }} />
          </Stack.Navigator>
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

1. <span data-ttu-id="063c8-149">Créez un fichier dans le répertoire **GraphTutorial/screens** nommé **SignInScreen. TSX**.</span><span class="sxs-lookup"><span data-stu-id="063c8-149">Create a new file in the **GraphTutorial/screens** directory named **SignInScreen.tsx**.</span></span> <span data-ttu-id="063c8-150">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="063c8-150">Add the following code to the file.</span></span>

    ```typescript
    // Adapted from https://reactnavigation.org/docs/auth-flow
    import React from 'react';
    import {
      Alert,
      Button,
      StyleSheet,
      View,
    } from 'react-native';
    import { ParamListBase } from '@react-navigation/native';
    import { StackNavigationProp } from '@react-navigation/stack'

    import { AuthContext } from '../AuthContext';

    type SignInProps = {
      navigation: StackNavigationProp<ParamListBase>;
    };

    export default class SignInScreen extends React.Component<SignInProps> {
      static contextType = AuthContext;

      _signInAsync = async () => {
        await this.context.signIn();
      };

      componentDidMount() {
        this.props.navigation.setOptions({
          title: 'Please sign in',
          headerShown: true
        });
      }

      render() {
        return (
          <View style={styles.container}>
            <Button title='Sign In' onPress={this._signInAsync}/>
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

1. <span data-ttu-id="063c8-151">Créez un fichier dans le répertoire **GraphTutorial/screens** nommé **AuthLoadingScreen. TSX**.</span><span class="sxs-lookup"><span data-stu-id="063c8-151">Create a new file in the **GraphTutorial/screens** directory named **AuthLoadingScreen.tsx**.</span></span> <span data-ttu-id="063c8-152">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="063c8-152">Add the following code to the file.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/screens/AuthLoadingScreen.tsx" id="AuthLoadingScreenSnippet":::

### <a name="create-a-navigation-drawer"></a><span data-ttu-id="063c8-153">Créer un tiroir de navigation</span><span class="sxs-lookup"><span data-stu-id="063c8-153">Create a navigation drawer</span></span>

<span data-ttu-id="063c8-154">Dans cette section, vous allez créer un menu pour l’application et mettre à jour l’application pour utiliser la navigation REACT pour se déplacer entre les écrans.</span><span class="sxs-lookup"><span data-stu-id="063c8-154">In this section you will create a menu for the application, and update the application to use react-navigation to move between screens.</span></span>

1. <span data-ttu-id="063c8-155">Créez un répertoire dans le répertoire **GraphTutorial** **.**</span><span class="sxs-lookup"><span data-stu-id="063c8-155">Create a new directory in the **GraphTutorial** directory named **menus**.</span></span>

1. <span data-ttu-id="063c8-156">Créez un fichier dans le répertoire **GraphTutorial/menus** nommé **DrawerMenu. TSX**.</span><span class="sxs-lookup"><span data-stu-id="063c8-156">Create a new file in the **GraphTutorial/menus** directory named **DrawerMenu.tsx**.</span></span> <span data-ttu-id="063c8-157">Ajoutez le code suivant au fichier.</span><span class="sxs-lookup"><span data-stu-id="063c8-157">Add the following code to the file.</span></span>

    ```typescript
    import React, { FC } from 'react';
    import {
      Alert,
      Image,
      StyleSheet,
      Text,
      View,
      ImageSourcePropType
    } from 'react-native';
    import {
      createDrawerNavigator,
      DrawerContentScrollView,
      DrawerItem,
      DrawerItemList,
      DrawerContentComponentProps
    } from '@react-navigation/drawer';
    import { ParamListBase } from '@react-navigation/native';
    import { StackNavigationProp } from '@react-navigation/stack'

    import { AuthContext } from '../AuthContext';
    import { UserContext } from '../UserContext';
    import HomeScreen from '../screens/HomeScreen';
    import CalendarScreen from '../screens/CalendarScreen';

    const Drawer = createDrawerNavigator();

    type CustomDrawerContentProps = DrawerContentComponentProps & {
      userName: string;
      userEmail: string;
      userPhoto: ImageSourcePropType;
      signOut: () => void;
    }

    type DrawerMenuProps = {
      navigation: StackNavigationProp<ParamListBase>;
    }

    const CustomDrawerContent: FC<CustomDrawerContentProps> = props => (
      <DrawerContentScrollView {...props}>
          <View style={styles.profileView}>
            <Image source={props.userPhoto}
              resizeMode='contain'
              style={styles.profilePhoto} />
            <Text style={styles.profileUserName}>{props.userName}</Text>
            <Text style={styles.profileEmail}>{props.userEmail}</Text>
          </View>
          <DrawerItemList {...props} />
          <DrawerItem label='Sign Out' onPress={props.signOut}/>
      </DrawerContentScrollView>
    );

    export default class DrawerMenuContent extends React.Component<DrawerMenuProps> {
      static contextType = AuthContext;

      state = {
        // TEMPORARY
        userLoading: true,
        userFirstName: 'Adele',
        userFullName: 'Adele Vance',
        userEmail: 'adelev@contoso.com',
        userTimeZone: 'UTC',
        userPhoto: require('../images/no-profile-pic.png')
      }

      _signOut = async () => {
        this.context.signOut();
      }

      async componentDidMount() {
        this.props.navigation.setOptions({
          headerShown: false,
        });
      }

      render() {
        const userLoaded = !this.state.userLoading;

        return (
          <UserContext.Provider value={this.state}>
            <Drawer.Navigator
              drawerType='front'
              screenOptions={{
                headerStyle: {
                  backgroundColor: '#276b80'
                },
                headerTintColor: 'white'
              }}
              drawerContent={props => (
                <CustomDrawerContent {...props}
                  userName={this.state.userFullName}
                  userEmail={this.state.userEmail}
                  userPhoto={this.state.userPhoto}
                  signOut={this._signOut} />
              )}>
              <Drawer.Screen name='Home'
                component={HomeScreen}
                options={{drawerLabel: 'Home', headerTitle: 'Welcome'}} />
              { userLoaded &&
                <Drawer.Screen name='Calendar'
                  component={CalendarScreen}
                  options={{drawerLabel: 'Calendar'}} />
              }
            </Drawer.Navigator>
          </UserContext.Provider>
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

1. <span data-ttu-id="063c8-158">Créez un répertoire dans le répertoire **GraphTutorial** nommé **images**.</span><span class="sxs-lookup"><span data-stu-id="063c8-158">Create a new directory in the **GraphTutorial** directory named **images**.</span></span>
1. <span data-ttu-id="063c8-159">Ajoutez une image de profil par défaut nommée **no-profile-pic.png** dans ce répertoire.</span><span class="sxs-lookup"><span data-stu-id="063c8-159">Add a default profile image named **no-profile-pic.png** in this directory.</span></span> <span data-ttu-id="063c8-160">Vous pouvez utiliser n’importe quelle image de votre choix ou utiliser [celle de cet exemple](https://github.com/microsoftgraph/msgraph-training-react-native/blob/master/demo/GraphTutorial/images/no-profile-pic.png).</span><span class="sxs-lookup"><span data-stu-id="063c8-160">You can use any image you like, or use [the one from this sample](https://github.com/microsoftgraph/msgraph-training-react-native/blob/master/demo/GraphTutorial/images/no-profile-pic.png).</span></span>

1. <span data-ttu-id="063c8-161">Ouvrez le fichier **GraphTutorial/App. TSX** et remplacez tout le contenu par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="063c8-161">Open the **GraphTutorial/App.tsx** file and replace the entire contents with the following.</span></span>

    ```typescript
    // Adapted from https://reactnavigation.org/docs/auth-flow
    import * as React from 'react';
    import { NavigationContainer, ParamListBase } from '@react-navigation/native';
    import { createStackNavigator, StackNavigationProp } from '@react-navigation/stack'

    import { AuthContext } from './AuthContext';
    import SignInScreen from './screens/SignInScreen';
    import DrawerMenuContent from './menus/DrawerMenu'
    import AuthLoadingScreen from './screens/AuthLoadingScreen';

    const Stack = createStackNavigator();

    type Props = {
      navigation: StackNavigationProp<ParamListBase>;
    };

    export default function App({ navigation }: Props) {
      const [state, dispatch] = React.useReducer(
        (prevState: any, action: any) => {
          switch (action.type) {
            case 'RESTORE_TOKEN':
              return {
                ...prevState,
                userToken: action.token,
                isLoading: false
              };
            case 'SIGN_IN':
              return {
                ...prevState,
                isSignOut: false,
                userToken: action.token
              }
            case 'SIGN_OUT':
              return {
                ...prevState,
                isSignOut: true,
                userToken: null
              }
          }
        },
        {
          isLoading: true,
          isSignOut: false,
          userToken: null
        }
      );

      React.useEffect(() => {
        const bootstrapAsync = async () => {
          let userToken = null;
          // TEMPORARY
          dispatch({ type: 'RESTORE_TOKEN', token: userToken });
        };

        bootstrapAsync();
      }, []);

      const authContext = React.useMemo(
        () => ({
          signIn: async () => {
            dispatch({ type: 'SIGN_IN', token: 'placeholder-token' });
          },
          signOut: async () => {
            dispatch({ type: 'SIGN_OUT' });
          }
        }),
        []
      );

      return (
        <AuthContext.Provider value={authContext}>
          <NavigationContainer>
            <Stack.Navigator>
              {state.isLoading ? (
                <Stack.Screen name="Loading" component={AuthLoadingScreen} />
              ) : state.userToken == null ? (
                <Stack.Screen name="SignIn" component={SignInScreen} />
              ) : (
                <Stack.Screen name="Main" component={DrawerMenuContent} />
              )}
            </Stack.Navigator>
          </NavigationContainer>
        </AuthContext.Provider>
      );
    }
    ```

1. <span data-ttu-id="063c8-162">Enregistrez toutes vos modifications.</span><span class="sxs-lookup"><span data-stu-id="063c8-162">Save all of your changes.</span></span>

1. <span data-ttu-id="063c8-163">Rechargez l’application dans votre émulateur.</span><span class="sxs-lookup"><span data-stu-id="063c8-163">Reload the application in your emulator.</span></span>

<span data-ttu-id="063c8-164">Le menu de l’application doit fonctionner pour naviguer entre les deux fragments et changer lorsque vous appuyez sur les boutons **se connecter** ou **se déconnecter** .</span><span class="sxs-lookup"><span data-stu-id="063c8-164">The app's menu should work to navigate between the two fragments and change when you tap the **Sign in** or **Sign out** buttons.</span></span>

![Captures d’écran de l’application sur Android](./images/android-app-screens.png)

![Captures d’écran de l’application sur iOS](./images/ios-app-screens.png)
