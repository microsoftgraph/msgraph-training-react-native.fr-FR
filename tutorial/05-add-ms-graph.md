<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="70f84-101">Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application.</span><span class="sxs-lookup"><span data-stu-id="70f84-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="70f84-102">Pour cette application, vous allez utiliser la [bibliothèque cliente Microsoft Graph JavaScript](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour passer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="70f84-102">For this application, you will use the [Microsoft Graph JavaScript Client Library](https://github.com/microsoftgraph/msgraph-sdk-javascript) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="70f84-103">Récupérer les événements de calendrier à partir d’Outlook</span><span class="sxs-lookup"><span data-stu-id="70f84-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="70f84-104">Dans cette section, vous allez étendre `GraphManager` la classe pour ajouter une fonction afin d’obtenir les événements de l' `CalendarScreen` utilisateur et la mise à jour pour utiliser ces nouvelles fonctions.</span><span class="sxs-lookup"><span data-stu-id="70f84-104">In this section you will extend the `GraphManager` class to add a function to get the user's events and update `CalendarScreen` to use these new functions.</span></span>

1. <span data-ttu-id="70f84-105">Ouvrez le fichier **GraphTutorial/Graph/GraphManager. TSX** et ajoutez la méthode suivante à la `GraphManager` classe.</span><span class="sxs-lookup"><span data-stu-id="70f84-105">Open the **GraphTutorial/graph/GraphManager.tsx** file and add the following method to the `GraphManager` class.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/graph/GraphManager.ts" id="GetEventsSnippet":::

    > [!NOTE]
    > <span data-ttu-id="70f84-106">Examinez le contenu du `getEvents` code.</span><span class="sxs-lookup"><span data-stu-id="70f84-106">Consider what the code in `getEvents` is doing.</span></span>
    >
    > - <span data-ttu-id="70f84-107">L’URL qui sera appelée est `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="70f84-107">The URL that will be called is `/v1.0/me/events`.</span></span>
    > - <span data-ttu-id="70f84-108">La `select` fonction limite les champs renvoyés pour chaque événement à seulement ceux que l’application utilisera réellement.</span><span class="sxs-lookup"><span data-stu-id="70f84-108">The `select` function limits the fields returned for each events to just those the app will actually use.</span></span>
    > - <span data-ttu-id="70f84-109">La fonction `orderby` trie les résultats en fonction de la date et de l’heure de création, avec l’élément le plus récent en premier.</span><span class="sxs-lookup"><span data-stu-id="70f84-109">The `orderby` function sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="70f84-110">Ouvrez **GraphTutorial/views/CalendarScreen. TSX** et remplacez l’intégralité de son contenu par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="70f84-110">Open the **GraphTutorial/views/CalendarScreen.tsx** and replace its entire contents with the following code.</span></span>

    ```typescript
    import React from 'react';
    import {
      ActivityIndicator,
      Alert,
      FlatList,
      Modal,
      ScrollView,
      StyleSheet,
      Text,
      View,
    } from 'react-native';
    import { createStackNavigator } from '@react-navigation/stack';

    import { DrawerToggle, headerOptions } from '../menus/HeaderComponents';
    import { GraphManager } from '../graph/GraphManager';

    const Stack = createStackNavigator();
    const initialState: CalendarScreenState = { loadingEvents: true, events: []};
    const CalendarState = React.createContext(initialState);

    type CalendarScreenState = {
      loadingEvents: boolean;
      events: any[];
    }

    // Temporary JSON view
    const CalendarComponent = () => {
      const calendarState = React.useContext(CalendarState);

      return (
        <View style={styles.container}>
          <Modal visible={calendarState.loadingEvents}>
            <View style={styles.loading}>
              <ActivityIndicator animating={calendarState.loadingEvents} size='large' />
            </View>
          </Modal>
          <ScrollView>
            <Text>{JSON.stringify(calendarState.events, null, 2)}</Text>
          </ScrollView>
        </View>
      );
    }

    export default class CalendarScreen extends React.Component {

      state: CalendarScreenState = {
        loadingEvents: true,
        events: []
      };

      async componentDidMount() {
        try {
          const events = await GraphManager.getEvents();

          this.setState({
            loadingEvents: false,
            events: events.value
          });
        } catch(error) {
          Alert.alert(
            'Error getting events',
            JSON.stringify(error),
            [
              {
                text: 'OK'
              }
            ],
            { cancelable: false }
          );
        }
      }

      render() {
        return (
          <CalendarState.Provider value={this.state}>
            <Stack.Navigator screenOptions={ headerOptions }>
              <Stack.Screen name='Calendar'
                component={ CalendarComponent }
                options={{
                  title: 'Calendar',
                  headerLeft: () => <DrawerToggle/>
                }} />
            </Stack.Navigator>
          </CalendarState.Provider>
        );
      }
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1
      },
      loading: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center'
      },
      eventItem: {
        padding: 10
      },
      eventSubject: {
        fontWeight: '700',
        fontSize: 18
      },
      eventOrganizer: {
        fontWeight: '200'
      },
      eventDuration: {
        fontWeight: '200'
      }
    });
    ```

<span data-ttu-id="70f84-111">Vous pouvez maintenant exécuter l’application, se connecter et appuyer sur l’élément de navigation **calendrier** dans le menu.</span><span class="sxs-lookup"><span data-stu-id="70f84-111">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="70f84-112">Vous devriez voir un dump JSON des événements dans l’application.</span><span class="sxs-lookup"><span data-stu-id="70f84-112">You should see a JSON dump of the events in the app.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="70f84-113">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="70f84-113">Display the results</span></span>

<span data-ttu-id="70f84-114">À présent, vous pouvez remplacer le vidage JSON par un texte pour afficher les résultats de manière conviviale.</span><span class="sxs-lookup"><span data-stu-id="70f84-114">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="70f84-115">Dans cette section, vous allez ajouter un `FlatList` à l’écran calendrier pour afficher les événements.</span><span class="sxs-lookup"><span data-stu-id="70f84-115">In this section, you will add a `FlatList` to the calendar screen to render the events.</span></span>

1. <span data-ttu-id="70f84-116">Ouvrez le fichier **GraphTutorial/Graph/screens/CalendarScreen. TSX** et ajoutez l' `import` instruction suivante en haut du fichier.</span><span class="sxs-lookup"><span data-stu-id="70f84-116">Open the **GraphTutorial/graph/screens/CalendarScreen.tsx** file and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import moment from 'moment';
    ```

1. <span data-ttu-id="70f84-117">Ajoutez la méthode suivante **au-dessus** de la déclaration de `CalendarScreen` classe.</span><span class="sxs-lookup"><span data-stu-id="70f84-117">Add the following method **above** the `CalendarScreen` class declaration.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/screens/CalendarScreen.tsx" id="ConvertDateSnippet":::

1. <span data-ttu-id="70f84-118">Remplacez le `ScrollView` dans la `CalendarComponent` méthode par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="70f84-118">Replace the `ScrollView` in the `CalendarComponent` method with the following.</span></span>

    ```typescript
    <FlatList data={calendarState.events}
      renderItem={({item}) =>
        <View style={styles.eventItem}>
          <Text style={styles.eventSubject}>{item.subject}</Text>
          <Text style={styles.eventOrganizer}>{item.organizer.emailAddress.name}</Text>
          <Text style={styles.eventDuration}>
            {convertDateTime(item.start.dateTime)} - {convertDateTime(item.end.dateTime)}
          </Text>
        </View>
      } />
    ```

1. <span data-ttu-id="70f84-119">Exécutez l’application, connectez-vous, puis appuyez sur l’élément de navigation **calendrier** .</span><span class="sxs-lookup"><span data-stu-id="70f84-119">Run the app, sign in, and tap the **Calendar** navigation item.</span></span> <span data-ttu-id="70f84-120">La liste des événements doit s’afficher.</span><span class="sxs-lookup"><span data-stu-id="70f84-120">You should see the list of events.</span></span>

    ![Capture d’écran du tableau des événements](./images/calendar-list.png)
