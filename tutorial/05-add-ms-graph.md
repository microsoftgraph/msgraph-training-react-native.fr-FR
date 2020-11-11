<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f956d-101">Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application.</span><span class="sxs-lookup"><span data-stu-id="f956d-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="f956d-102">Pour cette application, vous allez utiliser la [bibliothèque cliente Microsoft Graph JavaScript](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour passer des appels à Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="f956d-102">For this application, you will use the [Microsoft Graph JavaScript Client Library](https://github.com/microsoftgraph/msgraph-sdk-javascript) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="f956d-103">Récupérer les événements de calendrier à partir d’Outlook</span><span class="sxs-lookup"><span data-stu-id="f956d-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="f956d-104">Dans cette section, vous allez étendre la `GraphManager` classe pour ajouter une fonction afin d’obtenir les événements de l’utilisateur pour la semaine en cours et la mise à jour `CalendarScreen` pour utiliser ces nouvelles fonctions.</span><span class="sxs-lookup"><span data-stu-id="f956d-104">In this section you will extend the `GraphManager` class to add a function to get the user's events for the current week and update `CalendarScreen` to use these new functions.</span></span>

1. <span data-ttu-id="f956d-105">Ouvrez le fichier **GraphTutorial/Graph/GraphManager. TSX** et ajoutez la méthode suivante à la `GraphManager` classe.</span><span class="sxs-lookup"><span data-stu-id="f956d-105">Open the **GraphTutorial/graph/GraphManager.tsx** file and add the following method to the `GraphManager` class.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/graph/GraphManager.ts" id="GetCalendarViewSnippet":::

    > [!NOTE]
    > <span data-ttu-id="f956d-106">Examinez le contenu du code `getCalendarView` .</span><span class="sxs-lookup"><span data-stu-id="f956d-106">Consider what the code in `getCalendarView` is doing.</span></span>
    >
    > - <span data-ttu-id="f956d-107">L’URL qui sera appelée est `/v1.0/me/calendarView`.</span><span class="sxs-lookup"><span data-stu-id="f956d-107">The URL that will be called is `/v1.0/me/calendarView`.</span></span>
    > - <span data-ttu-id="f956d-108">La `header` fonction ajoute l' `Prefer: outlook.timezone` en-tête à la demande, ce qui entraîne des temps dans la réponse qui se trouve dans le fuseau horaire préféré de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f956d-108">The `header` function adds the `Prefer: outlook.timezone` header to the request, causing the times in the response to be in the user's preferred time zone.</span></span>
    > - <span data-ttu-id="f956d-109">La `query` fonction ajoute les `startDateTime` `endDateTime` paramètres et définit la fenêtre de temps pour l’affichage Calendrier.</span><span class="sxs-lookup"><span data-stu-id="f956d-109">The `query` function adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
    > - <span data-ttu-id="f956d-110">La `select` fonction limite les champs renvoyés pour chaque événement à seulement ceux que l’application utilisera réellement.</span><span class="sxs-lookup"><span data-stu-id="f956d-110">The `select` function limits the fields returned for each events to just those the app will actually use.</span></span>
    > - <span data-ttu-id="f956d-111">La `orderby` fonction trie les résultats en fonction de l’heure de début.</span><span class="sxs-lookup"><span data-stu-id="f956d-111">The `orderby` function sorts the results by the start time.</span></span>
    > - <span data-ttu-id="f956d-112">La `top` fonction limite les résultats aux premiers événements 50.</span><span class="sxs-lookup"><span data-stu-id="f956d-112">The `top` function limits the results to the first 50 events.</span></span>

1. <span data-ttu-id="f956d-113">Ouvrez **GraphTutorial/views/CalendarScreen. TSX** et remplacez l’intégralité de son contenu par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="f956d-113">Open the **GraphTutorial/views/CalendarScreen.tsx** and replace its entire contents with the following code.</span></span>

    ```typescript
    import React from 'react';
    import {
      ActivityIndicator,
      Alert,
      FlatList,
      Modal,
      Platform,
      ScrollView,
      StyleSheet,
      Text,
      View,
    } from 'react-native';
    import { createStackNavigator } from '@react-navigation/stack';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    import moment from 'moment-timezone';
    import { findOneIana } from 'windows-iana';

    import { UserContext } from '../UserContext';
    import { GraphManager } from '../graph/GraphManager';

    const Stack = createStackNavigator();
    const CalendarState = React.createContext<CalendarScreenState>({
      loadingEvents: true,
      events: []
    });

    type CalendarScreenState = {
      loadingEvents: boolean;
      events: MicrosoftGraph.Event[];
    }

    // Temporary JSON view
    const CalendarComponent = () => {
      const calendarState = React.useContext(CalendarState);

      return (
        <View style={styles.container}>
          <Modal visible={calendarState.loadingEvents}>
            <View style={styles.loading}>
              <ActivityIndicator
                color={Platform.OS === 'android' ? '#276b80' : undefined}
                animating={calendarState.loadingEvents}
                size='large' />
            </View>
          </Modal>
          <ScrollView>
            <Text>{JSON.stringify(calendarState.events, null, 2)}</Text>
          </ScrollView>
        </View>
      );
    }

    export default class CalendarScreen extends React.Component {
      static contextType = UserContext;

      state: CalendarScreenState = {
        loadingEvents: true,
        events: []
      };

      async componentDidMount() {
        try {
          const tz = this.context.userTimeZone || 'UTC';
          // Convert user's Windows time zone ("Pacific Standard Time")
          // to IANA format ("America/Los_Angeles")
          // Moment.js needs IANA format
          const ianaTimeZone = findOneIana(tz);

          // Get midnight on the start of the current week in the user's
          // time zone, but in UTC. For example, for PST, the time value
          // would be 07:00:00Z
          const startOfWeek = moment
            .tz(ianaTimeZone!.valueOf())
            .startOf('week')
            .utc();

          const endOfWeek = moment(startOfWeek)
            .add(7, 'day');

          const events = await GraphManager.getCalendarView(
            startOfWeek.format(),
            endOfWeek.format(),
            tz);

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
            <Stack.Navigator>
              <Stack.Screen name='Calendar'
                component={ CalendarComponent }
                options={{
                  headerShown: false
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

<span data-ttu-id="f956d-114">Vous pouvez maintenant exécuter l’application, se connecter et appuyer sur l’élément de navigation **calendrier** dans le menu.</span><span class="sxs-lookup"><span data-stu-id="f956d-114">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="f956d-115">Vous devriez voir un dump JSON des événements dans l’application.</span><span class="sxs-lookup"><span data-stu-id="f956d-115">You should see a JSON dump of the events in the app.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="f956d-116">Afficher les résultats</span><span class="sxs-lookup"><span data-stu-id="f956d-116">Display the results</span></span>

<span data-ttu-id="f956d-117">À présent, vous pouvez remplacer le vidage JSON par un texte pour afficher les résultats de manière conviviale.</span><span class="sxs-lookup"><span data-stu-id="f956d-117">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="f956d-118">Dans cette section, vous allez ajouter un `FlatList` à l’écran calendrier pour afficher les événements.</span><span class="sxs-lookup"><span data-stu-id="f956d-118">In this section, you will add a `FlatList` to the calendar screen to render the events.</span></span>

1. <span data-ttu-id="f956d-119">Ajoutez la méthode suivante **au-dessus** de la `CalendarScreen` déclaration de classe.</span><span class="sxs-lookup"><span data-stu-id="f956d-119">Add the following method **above** the `CalendarScreen` class declaration.</span></span>

    :::code language="typescript" source="../demo/GraphTutorial/screens/CalendarScreen.tsx" id="ConvertDateSnippet":::

1. <span data-ttu-id="f956d-120">Remplacez le `ScrollView` dans la `CalendarComponent` méthode par ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="f956d-120">Replace the `ScrollView` in the `CalendarComponent` method with the following.</span></span>

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

1. <span data-ttu-id="f956d-121">Exécutez l’application, connectez-vous, puis appuyez sur l’élément de navigation **calendrier** .</span><span class="sxs-lookup"><span data-stu-id="f956d-121">Run the app, sign in, and tap the **Calendar** navigation item.</span></span> <span data-ttu-id="f956d-122">La liste des événements doit s’afficher.</span><span class="sxs-lookup"><span data-stu-id="f956d-122">You should see the list of events.</span></span>

    ![Capture d’écran du tableau des événements](./images/calendar-list.png)
