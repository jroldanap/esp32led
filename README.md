# Practica ESP32 con Node-Red.
Este repositorio muestra como podemos programar una ESP32 con un relevador para prende el led atraves de un boton en node-red.

## Introducción

### Descripción

La Esp32 la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos un relevador para prender un led con un boton que se vera reflejado en Node-Red ; Cabe aclarar que esta practica se usara un simulador llamado [WOKWI](https://https://wokwi.com/).


## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- Relevador con led.
- Programa Node-Red (previamente instalado en https://github.com/DiegoJm10/Node-red-instalacion)



## Instrucciones

### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).


### Instrucciones de preparación de entorno 

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "52.29.234.128";
const int mqttPort = 1883;
const char* mqttUser = "jorgera";
const char* mqttPassword = "1234";
const char* mqttTopic = "jorgeled";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }
}
```

2. Instalar las librerias de *PubSubClient* como se muestra en la siguente imagen.

![](https://github.com/jroldanap/esp32led/blob/main/libe.png?raw=true)

3. Hacer la conexion del *relevador*, con la *ESP32* como se muestra en la siguente imagen.

![](https://github.com/jroldanap/esp32led/blob/main/relay.png?raw=true)

4. Poner el bloque switch en el programa Node-Red y cambiar el topic a *encender led*.

![](https://github.com/jroldanap/esp32led/blob/main/switch.png?raw=true)



5. Añadir el bloque *mqtt out* y el server a  *52.29.234.128* y modificar el topic a *jorgeled*.

![](https://github.com/jroldanap/esp32led/blob/main/mqtt.png?raw=true)


7. Añadir la configuracion en dashboard y dar click en flecha en rojo para ver el boton, como se muestra en la imagen.

![](https://github.com/jroldanap/esp32led/blob/main/boton.png?raw=true)




### Instrucciónes de operación

1. Iniciar simulador dando click en el boton verde de play.
2. Visualizar los datos en el monitor serial.
3. Presionar en el boton de Node-Red.
4. Visualizar como prende y apaga el led.

## Resultados

Cuando haya funcionado, verás el boton en  Node-Red y como prende el led en programa wokwi.

![](https://github.com/jroldanap/esp32led/blob/main/led%201.png?raw=true)

![](https://github.com/jroldanap/esp32led/blob/main/led2.png?raw=true)




## Evidencias de programa corriendo

![](https://github.com/jroldanap/esp32led/blob/main/led%201.png?raw=true)

![](https://github.com/jroldanap/esp32led/blob/main/led2.png?raw=true)


# Créditos

Desarrollado por Jorge Alberto Roldan Aponte

- [GitHub](https://github.com/jroldanap)