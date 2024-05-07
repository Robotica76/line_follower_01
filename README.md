# line_follower_01
Line follower robot -  line sensing


# Arduino Uno/Nano Mapa de conexiones IO
El mapa de entradas/salidas del Arduino Uno consiste en 14 pines digitales, 6 entradas analogicas, una entrada de alimentacion de voltage, conexion USB y un conector ICSP. Esta versitalidad de entradas/salidas provee versitalidad en las conexiones de controladores de motor, LED's, lectura de sensores y mas. A continuacion se muestra el mapa de conexiones del arduino uno:

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/ce4b906a-5fd1-4897-98cd-3c6cb44f1126)

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/e6ec4819-4319-4663-8af3-9be2228282e1)

Arduino nano:

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/9521210e-03d8-4817-974c-11148ffea3b9)


# Robot seguidor de lionea con Arduino

## Detector de líneas con Arduino y sensor TCRT5000L
###  ¿Qué es un TCRT5000L?

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

## Controlar motores de corriente continua con Arduino y L298N

### Que es un L298N?

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/3a272c1e-622b-44f6-9c3e-7086c3334960)

El L298N es un controlador (driver) de motores, que permite encender y controlar dos motores de corriente continua desde Arduino, variando tanto la dirección como la velocidad de giro.

Como comentamos frecuentemente Arduino, y en general todos los autómatas, no disponen de potencia suficiente para mover actuadores. De hecho, la función de un procesador no debe ser ejecutar acciones si no mandar ejecutar acciones a drivers que realicen el “trabajo pesado”.

El L298N también puede controlar un único motor de paso a paso aunque, en general, preferiremos usar dispositivos específicamente diseñados para motores paso a paso.

La corriente máxima que el L298N puede suministrar a los motores es, en teoría, 2A por salida (hasta 3A de pico) y una tensión de alimentación de 3V a 35V.

Sin embargo, el L298N tiene una eficiencia baja. La electrónica supone una caída de tensión de unos 3V, es decir, la tensión que recibe el motor es unos 3V inferior a la tensión de alimentación.

Estas pérdidas se disipan en forma de calor lo que se traduce en que, a efectos prácticos, es difícil que podamos obtener más de 0.8-1A por fase sin exceder el rango de temperatura de funcionamiento.

El L298N incorpora protecciones contra efectos que pueden producirse al manejar motores de corriente continua. Dispone de protecciones contra sobre intensidad, sobre temperatura, y diodos de protección contra corrientes inducidas (flyback).

El controlador L298N es ampliamente usado en proyectos electrónico y robótica, por su sencillez de uso, bajo coste, y buena calidad precio.

### Cómo funciona un L298N?

Básicamente un L298N consiste en dos puentes-H, uno para la salida A y otro para la salida B.

Un puente-H es un componente ampliamente utilizado en electrónica para alimentar una carga de forma que podemos invertir el sentido de la corriente que le atraviesa.

Internamente un puente-H es una formación de 4 transistores, conectados entre Vcc y GND, con la carga a alimentar entre ellos. Dibujado en esquema el conjunto tiene forma de “H”, de la que recibe su nombre su nombre.

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/34326c52-76e2-4e66-97d4-49a47e260d92)

Actuando sobre los 4 transistores, activando los transistores opuestos en diagonal de cada rama, podemos variar el sentido en el que la corriente atraviesa la carga

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/ead88a3e-d96b-410e-ae33-e0a9708aad7f)

Conectando simultáneamente los transistores superiores o inferiores, podemos poner la carga Vcc o Gnd respectivamente, configuración que usaremos como freno.

Por último, nunca debemos encender ambos transistores de un mismo ramal (izquierda o derecha), ya que estaremos provocando un cortocircuito entre Vcc y GND.

La placa L298N incorpora electrónica que simplifica la conexión al puente H, agrupando las conexiones en 3 pines accesibles (por cada salida) y eliminando la posibilidad de generar un cortocircuito.

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/43913e17-5dd4-4202-9ee3-e4ae681a122b)

Dos de estos pines, IN1 y IN2 (IN3 y IN4 para la salida B), controlan el encendido de los transistores de cada una de las dos ramas, encendiendo el ramal superior o inferior de la misma.

El tercer pin (IEA/IEB) desactiva simultáneamente todos los transistores del puente-H, desconectando la carga por completo.

### Esquema montaje

La placa de conexión del L298N incorpora una entrada de voltaje, una serie de jumpers para configurar el módulo, dos salidas A y B, y los pines de entrada que regulan la velocidad y el sentido de giro.

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/d3c78c1f-97a5-478d-9bd8-c904db18b4ad)

La entrada de tensión proporciona el voltaje que alimentará a los motores. El rango de entrada admisible es de 3V a 35V y se suministra mediante los 2 terminales izquierdos de la clema de conexión entrada.

El tercer terminal de la clema de conexión, Vlógico, está conectado con la electrónica del L298N y es necesario que tenga un valor entre 4.5 y 5.5V para que la placa funcione correctamente.

Para ello el módulo incorpora un regulador de voltaje que suministra la tensión necesaria en Vlógico. Este regulador puede desactivarse quitando el jumper de la placa. Desactivaremos el regulador cuando la tensión de alimentación sea inferior a 5V o superior a 15V.

Por tanto:

Si el regulador está activado (jumper cerrado) Vlógico es una salida de 5V que podemos emplear para alimentar otros dispositivos.
Si el regulador está desactivado (jumper abierto), Vlógico es una entrada a la que tendremos que proporcionar un voltaje de 4.5 a 5.5V.


** No debemos introducir corriente en Vlógico con el regulador activado (jumper conectado) o podemos dañar el módulo.**


Por otro lado, tenemos las dos clemas de conexión A y B que suministran la salida a los motores.

Por último, tenemos los pines de entrada que controlan la dirección y velocidad de giro.

Los pines IEA, IN1 e IN2 controlan la salida A.
Los pines IEB, IN3 e IN4 controlan la salida B.
Los pines IN1, IN2, y IN3 e IN4, controlan la dirección de giro, respectivamente, de la salida A y B.

Los pines IEA y IEB desactivan la salida. Podemos conectarlos permanentemente mediante el uso de un jumper, o conectar una señal PWM para controlar la velocidad de giro.

En el caso de querer usar ambas fases, y poder elegir tanto el sentido de giro como la velocidad, y alimentar desde una fuente de 12V, el esquema de conexión a Arduino sería el siguiente.

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/86e7e458-5e8e-40c3-8045-622258336874)

Mientras que la conexión, vista desde el lado de Arduino, sería la siguiente.

![image](https://github.com/Robotica76/line_follower_01/assets/57429237/c1c11733-75da-434d-b12a-85a9bf784c84)

La alimentación de Arduino en este caso podría realizarse desde la fuente de 12V al pin Vin de Arduino (usando el regulador de voltaje de Arduino), o desde el pin Vlogico del L298N al pin 5V de Arduino (usando el regulador del L298N).

** Siempre que uséis más de una fuente de tensión recordar poner en común todos los GND incluido el de Arduino. De lo contrario podéis dañar un componente.**
