<!-- markdownlint-disable MD002 MD041 -->

Dans cet exercice, vous allez incorporer Microsoft Graph dans l’application. Pour cette application, vous allez utiliser la [bibliothèque cliente Microsoft Graph JavaScript](https://github.com/microsoftgraph/msgraph-sdk-javascript) pour passer des appels à Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtenir des événements de calendrier à partir d’Outlook

Dans cette section, vous allez étendre `GraphManager` la classe pour ajouter une fonction afin d’obtenir les événements de l' `CalendarScreen` utilisateur et la mise à jour pour utiliser ces nouvelles fonctions.

1. Ouvrez le fichier **GraphTutorial/Graph/GraphManager. js** et ajoutez la méthode suivante à la `GraphManager` classe.

    ```js
    static getEvents = async() => {
      // GET /me/events
      return graphClient.api('/me/events')
        // $select='subject,organizer,start,end'
        // Only return these fields in results
        .select('subject,organizer,start,end')
        // $orderby=createdDateTime DESC
        // Sort results by when they were created, newest first
        .orderby('createdDateTime DESC')
        .get();
    }
    ```

    > [!NOTE]
    > Examinez le contenu du `getEvents` code.
    >
    > - L’URL qui sera appelée est `/v1.0/me/events`.
    > - La `select` fonction limite les champs renvoyés pour chaque événement à seulement ceux que l’application utilisera réellement.
    > - La `orderby` fonction trie les résultats en fonction de la date et de l’heure de leur création, avec l’élément le plus récent en premier.

1. Ouvrez **GraphTutorial/views/CalendarScreen. js** et remplacez l’intégralité de son contenu par le code suivant.

    ```JSX
    import React from 'react';
    import {
      ActivityIndicator,
      FlatList,
      Modal,
      ScrollView,
      StyleSheet,
      Text,
      View,
    } from 'react-native';
    import { Icon } from 'react-native-elements';
    import { GraphManager } from '../graph/GraphManager';

    export default class CalendarScreen extends React.Component {
      static navigationOptions = ({navigation}) => {
        return {
          title: 'Calendar',
          headerLeft: <Icon iconStyle={{ marginLeft: 10, color: 'white' }} size={30} name="menu" onPress={navigation.toggleDrawer} />
        };
      }

      state = {
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
          alert(error);
        }
      }

      // Temporary JSON view
      render() {
        return (
          <View style={styles.container}>
            <Modal visible={this.state.loadingEvents}>
              <View style={styles.loading}>
                <ActivityIndicator animating={this.state.loadingEvents} size='large' />
              </View>
            </Modal>
            <ScrollView>
              <Text>{JSON.stringify(this.state.events, null, 2)}</Text>
            </ScrollView>
          </View>
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

1. Ouvrez le fichier **GraphTutorial/Graph/GraphManager. js** et ajoutez l’instruction `import` suivante en haut du fichier.

    ```js
    import moment from 'moment';
    ```

1. Ajoutez la méthode suivante **au-dessus** de la déclaration de `CalendarScreen` classe.

    ```js
    convertDateTime = (dateTime) => {
      const utcTime = moment.utc(dateTime);
      return utcTime.local().format('MMM Do H:mm a');
    };
    ```

1. Remplacez le `ScrollView` dans la `render` méthode par ce qui suit.

    ```JSX
    <FlatList data={this.state.events}
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
