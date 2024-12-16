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

// defines pins numbers
const int trigPin = 4;
const int echoPin = 15;
const int led1 = 2;
const int led2 = 5;
const int led3 = 18;
const int led4 = 17;

#include <LiquidCrystal_I2C.h> //Libreria de LCD
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

// defines variables
long duration;
int distance;
int distancia;
int safetyDistance;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
pinMode(echoPin, INPUT); // Sets the echoPin as an Input
pinMode(led1, OUTPUT);
pinMode(led2, OUTPUT);
pinMode(led3, OUTPUT);
pinMode(led4, OUTPUT);
Serial.begin(9600); // Starts the serial communication
  lcd.init();
  lcd.backlight();
}

void loop() {
// Clears the trigPin
digitalWrite(trigPin, LOW);
delayMicroseconds(2);

// Sets the trigPin on HIGH state for 10 micro seconds
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

// Reads the echoPin, returns the sound wave travel time in microseconds
duration = pulseIn(echoPin, HIGH);

// Calculo de la distancia
distancia= int(0.01716*duration);

safetyDistance = distancia;
if (safetyDistance>=2 && safetyDistance<=5) //90% TANQUE
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
  lcd.clear(); 
  lcd.setCursor(4, 0);
  lcd.print("MODULO V");
  lcd.setCursor(6, 1);
  lcd.print("AIyM");
 delay(1000);

lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("diego bahena");
  lcd.setCursor(6, 1);
  lcd.print("I.E.E");
  delay(1000);

lcd.clear(); 
  lcd.setCursor(0, 0);
  lcd.print(" Distancia: " + String(distancia)+"cm");
  lcd.setCursor(2, 1);
  lcd.print("Tanque: 90%");
  delay(2000);
}
else if(safetyDistance>=5 && safetyDistance<=10) //75% TANQUE
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
lcd.clear(); 
  lcd.setCursor(4, 0);
  lcd.print("MODULO V");
  lcd.setCursor(6, 1);
  lcd.print("AIyM");
 delay(1000);

lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("diego bahena");
  lcd.setCursor(6, 1);
  lcd.print("I.E.E");
  delay(1000);

lcd.clear(); 
  lcd.setCursor(0, 0);
  lcd.print(" Distancia: " + String(distancia)+"cm");
  lcd.setCursor(2, 1);
  lcd.print("Tanque: 75%");
  delay(2000);
}
else if(safetyDistance>=10 && safetyDistance<=45) //50% TANQUE
{
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, LOW);
lcd.clear(); 
  lcd.setCursor(4, 0);
  lcd.print("MODULO V");
  lcd.setCursor(6, 1);
  lcd.print("AIyM");
 delay(1000);

lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("diego bahena");
  lcd.setCursor(6, 1);
  lcd.print("I.E.E");
  delay(1000);

lcd.clear(); 
  lcd.setCursor(0, 0);
  lcd.print(" Distancia: " + String(distancia)+"cm");
  lcd.setCursor(2, 1);
  lcd.print("Tanque: 50%");
  delay(2000);
}
else if(safetyDistance>=45 && safetyDistance<=70) //35% TANQUE
{
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, HIGH);
lcd.clear(); 
  lcd.setCursor(4, 0);
  lcd.print("MODULO V");
  lcd.setCursor(6, 1);
  lcd.print("AIyM");
 delay(1000);

lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("diego bahena");
  lcd.setCursor(6, 1);
  lcd.print("I.E.E");
  delay(1000);

lcd.clear(); 
  lcd.setCursor(0, 0);
  lcd.print(" Distancia: " + String(distancia)+"cm");
  lcd.setCursor(2, 1);
  lcd.print("Tanque: 35%");
  delay(2000);
}
else  //5% TANQUE O MENOS
{
 digitalWrite(led1,  LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
lcd.clear(); 
  lcd.setCursor(4, 0);
  lcd.print("MODULO V");
  lcd.setCursor(6, 1);
  lcd.print("AIyM");
 delay(1000);

lcd.clear();
  lcd.setCursor(2, 0);
  lcd.print("Zuriel Osio");
  lcd.setCursor(6, 1);
  lcd.print("I.E.E");
  delay(1000);

lcd.clear(); 
  lcd.setCursor(0, 0);
  lcd.print("Distancia: " + String(distancia)+"cm");
  lcd.setCursor(2, 1);
  lcd.print("Tanque: 5%");
  delay(2000);
}

// Prints the distance on the Serial Monitor
Serial.print("Distancia: "  );
Serial.println(distancia);
delay (2000); 
}

## Resultados 
La distancia medida por el sensor HC-SR04 se calcula y se muestra en la pantalla LCD.
Los LEDs (que simulan los relevadores) se encienden según el porcentaje de llenado del tanque, de acuerdo con la distancia medida por el sensor.
Se muestran mensajes en la pantalla LCD para indicar el nivel de llenado y la distancia.


![Texto alternativo](https://github.com/ZurielO/ESP32-HC-SR04-RELAY-LCD/blob/main/imagen_2024-12-15_190859867.png).


## Conclusión
Esta práctica permite simular un sistema de monitoreo de nivel de agua en un tanque utilizando la tecnología de sensores ultrasónicos, actuadores (LEDs) y visualización en pantalla LCD, todo controlado por la placa ESP32. El código proporciona una implementación sencilla para medir la distancia y convertirla en información útil para el monitoreo.






