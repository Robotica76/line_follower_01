# line_follower_01
Line follower robot -  line sensing


# Arduino Uno pin out guide
El mapa de entradas/salidas del Arduino Uno consiste en 14 pines digitales, 6 entradas analogicas, una entrada de alimentacion de voltage, conexion USB y un conector ICSP. Esta versitalidad de entradas/salidas provee versitalidad en las conexiones de controladores de motor, LED's, lectura de sensores y mas. A continuacion se muestra el mapa de conexiones del arduino uno:

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/ce4b906a-5fd1-4897-98cd-3c6cb44f1126)

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/e6ec4819-4319-4663-8af3-9be2228282e1)

Arduino nano:

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/9521210e-03d8-4817-974c-11148ffea3b9)


# Line follower with Arduino and sensor TCRT5000L

## ¿Qué es un TCRT5000L?

Un TCRT5000L es un tipo de sensor óptico reflectivo que detecta la diferencia de color en un objeto mediante la reflexión de la luz en el mismo.

El TCRT5000L es un sensor sencillo. Dispone de un LED emisor de luz infrarroja, y de un fototransitor que recibe la luz reflejada por un posible obstáculo. La cantidad de luz recibida depende del color y reflectividad del objeto, por lo que podemos distinguir entre zonas y oscuras de un objeto.

Estos sensores suelen proporcionarse con una placa de medición estándar con el comparador LM393, que permite obtener la lectura como un valor digital cuando se supera un cierto umbral, que se regula a través de un potenciómetro ubicado en la placa. Podemos capturar esta señal con las entradas digitales de Arduino.

El rango de medición del sensor varía entre 0.2 a 15mm, siendo la distancia óptima 2.5mm. Por tanto es un sensor de muy corta distancia.

La cantidad de luz infrarroja tiene una fuerte dependencia con el color, material, forma y posición del obstáculo, por lo que no disponen de precisión suficiente para proporcionar una estimación de la distancia a un objeto, simplemente es capaz de su detección.

Los sensores TCRT5000L son ampliamente utilizados para hacer robots seguidores de líneas, aunque también pueden emplearse para detectar cualquier otro tipo de objeto. Por ejemplo, son empleados en impresoras para saber cuando se ha agotado el papel.

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/3723e7dc-9069-47eb-8358-2dc538e384db)

## Esquema de montaje
Si usas una placa comercial, que como hemos dicho en general es recomendable, el montaje de un TCRT5000L a Arduino es realmente sencillo. Alimentamos el módulo a través de Vcc y GND conectándolos, respectivamente, a la salida de 5V y GND en Arduino.

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/5de0f0d3-3d47-4314-98e2-988772e069ad)

Por otro lado conectamos la salida digital del sensor (DO) a una entrada digital para leer el estado del sensor.

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/b95c7ccb-1af0-4c34-af51-5df951c9d93b)

Ejemplos de código
Para detectar cuando el TCRT5000L pasa por encima de una zona oscura simplemente leemos el estado de la entrada digital, tal y como vimos en la entrada Entradas digitales en Arduino.

Cuando el sensor se dispara tomaremos las acciones oportunas, como detener o variar la dirección de un robot.

``` c
const int sensorPin = 9;

void setup() {
  Serial.begin(9600);   //iniciar puerto serie
  pinMode(sensorPin, INPUT);  //definir pin como entrada
}
 
void loop(){
  int value = 0;
  value = digitalRead(sensorPin );  //lectura digital de pin
 
  if (value == LOW) {
      Serial.println("TCRT5000L activado");  //zona oscura
  }
  delay(1000);
}
```

Otra forma de atender al TCRT5000L es emplear interrupciones, lo que nos simplificará el código. Sin embargo, en un robot seguidor de líneas frecuentemente usaremos de tres a cinco detectores de líneas, mientras que Arduino UNO y Nano solo tenemos dos interrupciones externas.

