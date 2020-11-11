<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application. Pour cette application, vous allez utiliser la [bibliothèque cliente Microsoft Graph JavaScript](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour passer des appels à Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Récupérer les événements de calendrier à partir d’Outlook

Dans cette section, vous allez étendre la `GraphManager` classe pour ajouter une fonction afin d’obtenir les événements de l’utilisateur pour la semaine en cours et la mise à jour `CalendarScreen` pour utiliser ces nouvelles fonctions.

1. Ouvrez le fichier **GraphTutorial/Graph/GraphManager. TSX** et ajoutez la méthode suivante à la `GraphManager` classe.

    :::code language="typescript" source="../demo/GraphTutorial/graph/GraphManager.ts" id="GetCalendarViewSnippet":::

    > [!NOTE]
    > Examinez le contenu du code `getCalendarView` .
    >
    > - L’URL qui sera appelée est `/v1.0/me/calendarView`.
    > - La `header` fonction ajoute l' `Prefer: outlook.timezone` en-tête à la demande, ce qui entraîne des temps dans la réponse qui se trouve dans le fuseau horaire préféré de l’utilisateur.
    > - La `query` fonction ajoute les `startDateTime` `endDateTime` paramètres et définit la fenêtre de temps pour l’affichage Calendrier.
    > - La `select` fonction limite les champs renvoyés pour chaque événement à seulement ceux que l’application utilisera réellement.
    > - La `orderby` fonction trie les résultats en fonction de l’heure de début.
    > - La `top` fonction limite les résultats aux premiers événements 50.

1. Ouvrez **GraphTutorial/views/CalendarScreen. TSX** et remplacez l’intégralité de son contenu par le code suivant.

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

Vous pouvez maintenant exécuter l’application, se connecter et appuyer sur l’élément de navigation **calendrier** dans le menu. Vous devriez voir un dump JSON des événements dans l’application.

## <a name="display-the-results"></a>Afficher les résultats

À présent, vous pouvez remplacer le vidage JSON par un texte pour afficher les résultats de manière conviviale. Dans cette section, vous allez ajouter un `FlatList` à l’écran calendrier pour afficher les événements.

1. Ajoutez la méthode suivante **au-dessus** de la `CalendarScreen` déclaration de classe.

    :::code language="typescript" source="../demo/GraphTutorial/screens/CalendarScreen.tsx" id="ConvertDateSnippet":::

1. Remplacez le `ScrollView` dans la `CalendarComponent` méthode par ce qui suit.

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

1. Exécutez l’application, connectez-vous, puis appuyez sur l’élément de navigation **calendrier** . La liste des événements doit s’afficher.

    ![Capture d’écran du tableau des événements](./images/calendar-list.png)
