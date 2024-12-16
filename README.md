# ESP32-HC-SR04-RELAY-LCD
Monitoreo de Nivel de Tanque con Sensor HC-SR04 y Control de Relevadores

## Objetivo 
El objetivo de esta práctica es medir la distancia en un tanque utilizando el sensor ultrasónico HC-SR04, y a partir de esa medición, determinar el nivel de agua dentro del tanque. A medida que cambia el nivel de agua, se encienden y apagan LEDs (representando relevadores) para indicar diferentes niveles de llenado (porcentaje del tanque), y se visualiza la información en una pantalla LCD.

## Material 
Placa ESP32: Microcontrolador con capacidad de conectividad inalámbrica (Wi-Fi/Bluetooth) y suficientes pines GPIO para controlar varios dispositivos.
Sensor ultrasónico HC-SR04: Sensor de distancia que utiliza ultrasonido para medir la distancia entre el sensor y un objeto (en este caso, el agua en un tanque).
Relevadores (simulados mediante LEDs): Usados para representar el control de encendido y apagado dependiendo del nivel del tanque.
Pantalla LCD 20x4 con I2C: Para mostrar el nivel de tanque y la distancia medida.
Plataforma Wokwi: Para la simulación del circuito sin necesidad de hardware físico.

## Diagrama de conexión
Sensor HC-SR04:
Trigger conectado al GPIO 4.
Echo conectado al GPIO 15.
Relevadores (LEDs):
LED1 (para 90% de llenado) conectado al GPIO 2.
LED2 (para 75% de llenado) conectado al GPIO 5.
LED3 (para 50% de llenado) conectado al GPIO 18.
LED4 (para 35% de llenado) conectado al GPIO 17.
Pantalla LCD: Conectada a la ESP32 mediante comunicación I2C.

## Descripción del código
El código está diseñado para medir la distancia del agua en un tanque utilizando el sensor HC-SR04 y calcular el nivel de llenado del tanque. Con base en la distancia medida, se encienden diferentes LEDs para indicar el porcentaje de llenado del tanque. También se muestran los datos en la pantalla LCD.

### El flujo del código es el siguiente:

Inicialización:

Se configuran los pines de los sensores y los LEDs.
La pantalla LCD se inicializa y se enciende el retroiluminado.
Medición de distancia:

El código envía un pulso de 10 microsegundos al Trigger para generar una onda ultrasónica.
El Echo mide el tiempo que tarda en recibir la señal de vuelta, lo que permite calcular la distancia entre el sensor y el agua.
Cálculo del nivel de llenado:

Según la distancia medida, se determina el porcentaje de llenado del tanque:
90% de llenado: Distancia entre 2 y 5 cm.
75% de llenado: Distancia entre 5 y 10 cm.
50% de llenado: Distancia entre 10 y 45 cm.
35% de llenado: Distancia entre 45 y 70 cm.
5% de llenado o menos: Distancia mayor a 70 cm.
Visualización de datos:

El nivel de llenado del tanque se muestra en la pantalla LCD y también se indican los valores de la distancia y el nivel en el monitor serie.
Dependiendo del nivel de agua, se encienden los LEDs correspondientes para simular los relevadores.
## Código
