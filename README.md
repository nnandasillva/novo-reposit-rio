//Alteração blablabla
import React from 'react';
import { Text, View, StyleSheet, Image } from 'react-native';
import * as Location from 'expo-location';
import moment from 'moment'; 

export default class App extends React.Component {
  constructor() {
    super();
    this.state = {
      api: '',
        dateTime: moment().format('YYYY-MM-DD HH:mm:ss'), 
      location: null,
      errorMessage: null,
    
    };
  }

  async componentDidMount() {
    await this.getLocation();
  }

  getLocation = async () => {
    let { status } = await Location.requestForegroundPermissionsAsync();
    if (status !== 'granted') {
      this.setState({ errorMessage: 'Permissão negada' });
      return;
    }
    let location = await Location.getCurrentPositionAsync({});
    this.setState({ location }, () => {
      this.pegarDados();
      this.pegarHora();
    });
  };

  pegarDados = async () => {
    const { latitude, longitude } = this.state.location.coords;
    const link = ``;

   return await fetch(link)
  .then((resposta) => resposta.json())
  .then((respostaJson) => {
    this.setState({ api: respostaJson });
  })
  .catch((erro) => console.error(erro));
};

pegarHora = async () => {
  const {time} = this.state.location.coords;
  var link = ``;

  return await fetch(link)
  .then((resposta) => resposta.json())
  .then((respostaJson) => {
    this.setState({ api: respostaJson });
  })
  .catch((erro) => console.error(erro));
};


  render() {
    const { api, errorMessage, dateTime } = this.state;

    if (errorMessage) {
      return (
        <View style={styles.container}>
          <Text>{errorMessage}</Text>
        </View>
      );
    }

    if (!api || !api.main || !dateTime) {
      return (
        <View style={styles.container}>
          <Text>Carregando...</Text>
        </View>
      );
    }

    return (
      <View style={styles.container}>
        <Text style={styles.title}>Previsão do Tempo</Text>
        <Image
          source={require('./assets/10587825-removebg-preview.png')} 
          style={styles.imagem}
        />
        <Text style={styles.texto}>{api.name} - {api.main.temp}ºC</Text>
        <Text style={styles.dateTime}> {dateTime}</Text> 
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#eb34e5',
    padding: 20, 
  },
  title: {
    marginTop: 50,
    fontSize: 30,
    fontWeight: 'bold',
    textAlign: 'center',
    color: 'pink'
  },
  imagem: {
    width: 150,
    height: 150,
    marginTop: 30,
    alignSelf: 'center',
    marginVertical: 30,
  },
  texto: {
    fontSize: 20,
    textAlign: 'center',
    color: 'pink'
  },
  dateTime: {
    fontSize: 14,
    textAlign: 'center',
    marginTop: 10,
    color: 'pink'
  },
});
