# IngeteamAltsvista107
Monitorización del inversor conectado a red de Altavista 107

Consta de dos partes: un arduino nano y un Nodemcu
El nano lee los datos del inversor por modbus y los envía por softserial al nodemcu
El Nodemcu lee los datos del nano y los envía a emoncms

Se ha hecho así ya que no he conseguido que funcionase la librería modbus en el nodemcu

Se utiliza un Max485 para la lectura del inversor por el nano por modbus rtu
/*
#define D0 16
#define D1 5 // I2C Bus SCL (clock)
#define D2 4 // I2C Bus SDA (data)
#define D3 0
#define D4 2 // Same as "LED_BUILTIN", but inverted logic
#define D5 14 // SPI Bus SCK (clock)
#define D6 12 // SPI Bus MISO 
#define D7 13 // SPI Bus MOSI
#define D8 15 // SPI Bus SS (CS)
#define D9 3 // RX0 (Serial console)
#define D10 1 // TX0 (Serial console)

Si se usa sensor irradiancia TSL2561
TSL2561 -> Nodemcu
vcc -> 3V
gng -> G
SDA -> D2
SCL -> D1

Conexiones del arduino nano
Nano ->Nodemcu
GND -> GND
D3(Tx) -> D4(Rx)  la salida nano D3(TX)al pin D4 (el mismo que el LED interno) del Nodemcu
D2(Rx) -> D6(Tx)  No hace falta conectarlo ya que el Nodemcu sólo lee datos

El Led interno de Nodemcu parpadea cada vez que recibe datos del Nano

Usa wifimanager para conectarse
1.la primera vez o después de un reset wifi, conectarse con el movil o el ordenador a la red Nodemcu
(introducir la clave A12345678)
2.después entrar en el explorador a la dirección http://192.168.4.1
3.seleccionar la wifi disponible y conectar introduciendo su clave correspondiente
4.desconectar el movil u ordenador de la nodemcu y conectar a la wifi local
5. por puerto serie se puede ver la ip local para acceder, suele ser http://192.168.1.43
NOTA: Cuando se conecta uno con la ip local a veces se queda "colgado", hay que resetear apagando y volviendo a encender el Nodemcu
 * */
 * /**
 *  Ingeteam altavista 107  Arduino nano
 *  arduino nano lee los datos
 *  los envía por software serial al Nodemcu 
 *  Se utiliza un max485
 *Adaptado Miguel Alonso 2016.
  
         MAX485EE
          ____
 1(Rx)-- 1|    |8 --- +5
 8 ----- 2|    |7 --- RS-485 B- Naranja al inversor
 8 ----- 3|    |6 --- RS-485 A+ Marron al inversor
 0(Tx) - 4|____|5 --- GND

 *  Nano    --->   Nodemcu o Max485
 *  0       --->   4(Tx) del Max485
 *  1       --->   1(Rx) del Max485
 *  2       --->   D6 del Nodemcu (softwareserial) No necesario
 *  3       --->   D4 del Nodemcu (softwareserial) 
 *  8       --->   2 y 3 del MAX485 
 * 	5V      --->   8 del del MAX485 
 * 	GND     --->   GND del Nodemcu y del MAX485 
 *		
 */
 //https://github.com/madsci1016/Arduino-EasyTransfer para enviar datos por puerto serie a Nodemcu
 // Modificar la librería, comentar io.h para que sea compatible con Nodemcu (funciona igual)
 //usar softwareserial para pines 2 y 3.
