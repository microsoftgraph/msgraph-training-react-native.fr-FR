<!-- markdownlint-disable MD002 MD041 -->

Commencez par créer un projet Native REACT.

1. Ouvrez votre interface de ligne de commande (CLI) dans un répertoire où vous souhaitez créer le projet. Exécutez la commande suivante pour exécuter l’outil [REACT-Native-CLI](https://github.com/facebook/react-native) et créer un projet natif REACT.

    ```Shell
    npx react-native init GraphTutorial --template react-native-template-typescript
    ```

1. **Facultatif :** Vérifiez que votre environnement de développement est correctement configuré en exécutant le projet. Dans votre interface CLI, modifiez le répertoire pour le répertoire **GraphTutorial** que vous venez de créer et exécutez l’une des commandes suivantes.

    - Pour iOS :`npx react-native run-ios`
    - Pour Android : lancer une instance d’émulateur Android et exécuter`npx react-native run-android`

## <a name="install-dependencies"></a>Installer les dépendances

Avant de poursuivre, installez des dépendances supplémentaires que vous utiliserez plus tard.

- [REACT : navigation](https://reactnavigation.org) permettant de gérer la navigation entre les affichages dans l’application.
- [REACT-Native-descripteur](https://github.com/kmagiera/react-native-gesture-handler), [REACT-Native-Safe-Context](https://github.com/th3rdwave/react-native-safe-area-context), [REACT-Native-screens](https://github.com/kmagiera/react-native-screens), [REACT-Native-REANIMATION](https://github.com/kmagiera/react-native-reanimated)et [Masked-View](https://github.com/react-native-community/react-native-masked-view) requis par REACT-navigation.
- [REACT-Native-Elements](https://react-native-training.github.io/react-native-elements/docs/getting_started.html) et [REACT-Native-vector-icons](https://github.com/oblador/react-native-vector-icons) pour fournir des icônes pour l’interface utilisateur.
- [REACT-Native-App-auth](https://github.com/FormidableLabs/react-native-app-auth) pour gérer l’authentification et la gestion des jetons.
- [Async : stockage pour le](https://github.com/react-native-community/react-native-async-storage) stockage des jetons.
- [moment](https://momentjs.com) à prendre en charge l’analyse et la comparaison des dates et des heures.
- [Microsoft-Graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour effectuer des appels à Microsoft Graph.

1. Ouvrez la CLI dans le répertoire racine de votre projet natif REACT.
1. Exécutez la commande suivante.

    ```Shell
    npm install @react-navigation/native@5.1.5 @react-navigation/drawer@5.4.1 @react-navigation/stack@5.2.10
    npm install @react-native-community/masked-view@0.1.7 react-native-safe-area-context@0.7.3
    npm install react-native-reanimated@1.8.0 react-native-screens@2.4.0 @react-native-community/async-storage@1.9.0
    npm install react-native-elements@1.2.7 react-native-vector-icons@6.6.0 react-native-gesture-handler@1.6.1
    npm install react-native-app-auth@5.1.1 moment@2.24.0 @microsoft/microsoft-graph-client@2.0.0
    ```

### <a name="link-and-configure-dependencies-for-ios"></a>Liaison et configuration des dépendances pour iOS

> [!NOTE]
> Si vous ne ciblez pas iOS, vous pouvez ignorer cette section.

1. Ouvrez l’interface CLI dans le répertoire **GraphTutorial/iOS** .
1. Exécutez la commande suivante.

    ```Shell
    pod install
    ```

1. Ouvrez le fichier **GraphTutorial/iOS/GraphTutorial/info. plist** dans un éditeur de texte. Ajoutez le code suivant juste avant la `</dict>` dernière ligne du fichier.

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

1. Ouvrez le **GraphTutorial/iOS/GraphTutorial/AppDelegate. h** dans un éditeur de texte. Remplacez son contenu par ce qui suit.

    :::code language="objc" source="../demo/GraphTutorial/ios/GraphTutorial/AppDelegate.h":::

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

    L' `defaultConfig` entrée doit ressembler à ce qui suit.

    :::code language="gradle" source="../demo/GraphTutorial/android/app/build.gradle" id="DefaultConfigSnippet":::

1. Ajoutez la ligne suivante à la fin du fichier.

    ```Gradle
    apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
    ```

1. Enregistrez le fichier.

## <a name="design-the-app"></a>Concevoir l’application

L’application utilise un [tiroir de navigation](https://reactnavigation.org/docs/drawer-based-navigation.html) pour naviguer entre les différentes vues. Dans cette étape, vous allez créer les vues de base utilisées par l’application et mettre en œuvre le tiroir de navigation.

### <a name="create-views"></a>Créer des vues

Dans cette section, vous allez créer les affichages de l’application afin de prendre en charge un [flux d’authentification](https://reactnavigation.org/docs/auth-flow).

1. Ouvrez **GraphTutorial/index. js** et ajoutez le code suivant en haut du fichier, avant toutes les autres `import` instructions.

    ```javascript
    import 'react-native-gesture-handler';
    ```

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **écrans**.
1. Créez un fichier dans le répertoire **GraphTutorial/screens** nommé **homescreen. TSX**. Ajoutez le code suivant au fichier.

    ```typescript
    import React from 'react';
    import {
      ActivityIndicator,
      Alert,
      StyleSheet,
      Text,
      View,
    } from 'react-native';
    import { createStackNavigator } from '@react-navigation/stack';
    import { DrawerToggle, headerOptions } from '../menus/HeaderComponents';

    const Stack = createStackNavigator();
    const UserState = React.createContext({userLoading: true, userName: ''});

    type HomeScreenState = {
      userLoading: boolean;
      userName: string;
    }

    const HomeComponent = () => {
      const userState = React.useContext(UserState);

      return (
        <View style={styles.container}>
          <ActivityIndicator animating={userState.userLoading} size='large' />
          {userState.userLoading ? null: <Text>Hello {userState.userName}!</Text>}
        </View>
      );
    }

    export default class HomeScreen extends React.Component {

      state: HomeScreenState = {
        userLoading: true,
        userName: ''
      };

      render() {
        return (
          <UserState.Provider value={this.state}>
              <Stack.Navigator screenOptions={headerOptions}>
                <Stack.Screen name='Home'
                  component={HomeComponent}
                  options={{
                    title: 'Welcome',
                    headerLeft: () => <DrawerToggle/>
                  }} />
              </Stack.Navigator>
          </UserState.Provider>
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

1. Créez un fichier dans le répertoire **GraphTutorial/screens** nommé **CalendarScreen. TSX**. Ajoutez le code suivant au fichier.

    ```typescript
    import React from 'react';
    import {
      Text,
      StyleSheet,
      View,
    } from 'react-native';
    import { createStackNavigator } from '@react-navigation/stack';
    import { DrawerToggle, headerOptions } from '../menus/HeaderComponents';

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
          <Stack.Navigator screenOptions={ headerOptions }>
            <Stack.Screen name='Calendar'
              component={ CalendarComponent }
              options={{
                title: 'Calendar',
                headerLeft: () => <DrawerToggle/>
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

1. Créez un fichier dans le répertoire **GraphTutorial/screens** nommé **SignInScreen. TSX**. Ajoutez le code suivant au fichier.

    ```typescript
    // Adapted from https://reactnavigation.org/docs/auth-flow
    import React from 'react';
    import {
      Alert,
      Button,
      StyleSheet,
      View,
    } from 'react-native';
    import { NavigationContext } from '@react-navigation/native';

    export default class SignInScreen extends React.Component {
      static contextType = NavigationContext;

      static navigationOptions = {
        title: 'Please sign in',
      };

      _signInAsync = async () => {
        const navigation = this.context;

        // TEMPORARY
        navigation.reset({
          index: 0,
          routes: [ { name: 'Main' } ]
        });
      };

      render() {
        const navigation = this.context;
        navigation.setOptions({
          title: 'Please sign in',
          headerShown: true
        });

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

1. Créez un fichier dans le répertoire **GraphTutorial/screens** nommé **AuthLoadingScreen. TSX**. Ajoutez le code suivant au fichier.

    :::code language="typescript" source="../demo/GraphTutorial/screens/AuthLoadingScreen.tsx" id="AuthLoadingScreenSnippet":::

### <a name="create-a-navigation-drawer"></a>Créer un tiroir de navigation

Dans cette section, vous allez créer un menu pour l’application et mettre à jour l’application pour utiliser la navigation REACT pour se déplacer entre les écrans.

1. Créez un répertoire dans le répertoire **GraphTutorial** **.**
1. Créez un fichier dans le répertoire **GraphTutorial/menus** nommé **HeaderComponents. TSX**. Ajoutez le code suivant au fichier.

    :::code language="typescript" source="../demo/GraphTutorial/menus/HeaderComponents.tsx" id="HeaderComponentSnippet":::

1. Créez un fichier dans le répertoire **GraphTutorial/menus** nommé **DrawerMenu. TSX**. Ajoutez le code suivant au fichier.

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
    import { NavigationContext } from '@react-navigation/native';

    import HomeScreen from '../screens/HomeScreen';
    import CalendarScreen from '../screens/CalendarScreen';

    const Drawer = createDrawerNavigator();

    type CustomDrawerContentProps = DrawerContentComponentProps & {
      userName: string;
      userEmail: string;
      userPhoto: ImageSourcePropType;
      signOut: () => void;
    }

    type DrawerMenuState = {
      userName: string;
      userEmail: string;
      userPhoto: ImageSourcePropType;
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

    export default class DrawerMenuContent extends React.Component {
      static contextType = NavigationContext;

      state: DrawerMenuState = {
        // TEMPORARY
        userName: 'Adele Vance',
        userEmail: 'adelev@contoso.com',
        userPhoto: require('../images/no-profile-pic.png')
      }

      _signOut = async () => {
        const navigation = this.context;
        // Sign out
        // TEMPORARY
        navigation.reset({
          index: 0,
          routes: [{ name: 'SignIn' }]
        });
      }

      render() {
        const navigation = this.context;
        navigation.setOptions({
          headerShown: false,
        });

        return (
          <Drawer.Navigator
            drawerType='front'
            drawerContent={props => (
              <CustomDrawerContent {...props}
                userName={this.state.userName}
                userEmail={this.state.userEmail}
                userPhoto={this.state.userPhoto}
                signOut={this._signOut} />
            )}>
            <Drawer.Screen name='Home'
              component={HomeScreen}
              options={{drawerLabel: 'Home'}} />
            <Drawer.Screen name='Calendar'
              component={CalendarScreen}
              options={{drawerLabel: 'Calendar'}} />
          </Drawer.Navigator>
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

1. Créez un répertoire dans le répertoire **GraphTutorial** nommé **images**.
1. Ajoutez une image de profil par défaut nommée **no-Profile-pic. png** dans ce répertoire. Vous pouvez utiliser n’importe quelle image de votre choix ou utiliser [celle de cet exemple](https://github.com/microsoftgraph/msgraph-training-react-native/blob/master/demo/GraphTutorial/images/no-profile-pic.png).

1. Ouvrez le fichier **GraphTutorial/App. TSX** et remplacez tout le contenu par ce qui suit.

    :::code language="typescript" source="../demo/GraphTutorial/App.tsx" id="AppSnippet":::

1. Enregistrez toutes vos modifications.

1. Rechargez l’application dans votre émulateur.

Le menu de l’application doit fonctionner pour naviguer entre les deux fragments et changer lorsque vous appuyez sur les boutons **se connecter** ou **se déconnecter** .

![Captures d’écran de l’application sur Android](./images/android-app-screens.png)

![Captures d’écran de l’application sur iOS](./images/ios-app-screens.png)
