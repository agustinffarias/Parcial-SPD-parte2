# Alumno:
FARIAS TOMAS AGUSTIN

# Link del proyecto Parte 3:
[[https://www.tinkercad.com/things/dGNinya2tob-con-sensor-de-temperatura/editel?sharecode=7RmI-q71zYKV9LyrGuB_Ndgmlrxn2aZJloYqPpmNbSg](https://www.tinkercad.com/things/dGNinya2tob-con-sensor-de-temperatura/editel?sharecode=7RmI-q71zYKV9LyrGuB_Ndgmlrxn2aZJloYqPpmNbSg)](https://www.tinkercad.com/things/dGNinya2tob-con-sensor-de-temperatura/editel?sharecode=7RmI-q71zYKV9LyrGuB_Ndgmlrxn2aZJloYqPpmNbSg)

# Diagrama del circuito:
![Tinkercad](/imagenes/Parcialparte3.1.jpg)
![Tinkercad](/imagenes/Parcialparte3.2.jpg)
# Cambios con respecto a la parte2:
Los cambios con respecto a la parte dos son que, se agrego una fotorresistencia, un led y dos resistencias. Esto cumple la funcionalidad de dar aviso de precaucion cuando el programa este trabajando a altas temperaturas, esto aprovechando que, en el parcial parte 2 pude agregar un sensor de temperatura, entonces, cuando el usuario de forma manual controle la temperatura simulando un entorno de trabajo real el sistema pueda detectar que, le podria producir fallas y, tanto por consola con un mensaje, como de forma visual encendiendo el led, el usuario podria tomar conocimiento de dicho dato. 

# Descripcion: 
El siguiente programa cumple la funcion de medir la temperatura con un sensor de temperatura, en el caso de que la temperatura sea mayor a 50°Centigrador, la fotorresistencia entra en accion encendiendo un led para avisar que el sistema podria fallar, asi mismo, printea por consola que la temperatura podria ocacionar problemas o quemar el circuito.
Misma tecitura para cuando el estado del deslizante esta en FAHRENHEITS, pero en este caso, la temperatura debe ser mayor a 122°FAHRENHEIT ya que es el equivalente a 50°Centigrados. La luz LED cumple la funcion de poder dar una alerta visual al operador. 
En cuanto a los 7segmentos siguen cumpliendo la misma funcion de un comienzo, cuando el deslizante se encuentra en 0 el motor girara hacia la izquierda, al mismo tiempo que printea por consola los grados FAHRENHEIT y el 7 segmentos muestra un contador ascendente del 0 al 99 escalando de 1 en 1.
Si el deslizante se encotrara retornando el valor 1, el motor girara hacia la derecha, printeando por consola los grados CELCIUS, el 7 segmentos mostrara de forma ascendente los numeros primos del 0 al 99.

# Descripcion de las funciones:
// DEFINIMOS EN QUE LUGAR ESTARAN CONECTADOS LOS ELEMENTOS
// DENTRO DEL ARDUINO UNO (EN QUE PIN) 
#define LED 4
#define FOTORRESISTENCIA A1
#define A 10
#define B 11
#define C 5
#define D 6
#define E 7
#define F 9
#define G 8
#define DECENA A5
#define UNIDAD A4
#define TIEMPO 10
#define BOTON_DESLIZANTE 3
#define MOTOR_PIN1 13
#define MOTOR_PIN2 12
#define TEMPERATURA A3
// DECLARAMOS EL CONTADOR IGUALADO A 0
int contador = 0;
// SETEAMOS CADA PIN COMO OUTPUT O INPUT SEGUN CORRESPONDA
void setup()
{
  pinMode(FOTORRESISTENCIA,INPUT);
  pinMode(A, OUTPUT);
  pinMode(B, OUTPUT);
  pinMode(C, OUTPUT);
  pinMode(D, OUTPUT);
  pinMode(E, OUTPUT);
  pinMode(F, OUTPUT);
  pinMode(G, OUTPUT);
  pinMode(DECENA, OUTPUT);
  pinMode(UNIDAD, OUTPUT); 
  pinMode(BOTON_DESLIZANTE, INPUT);
  pinMode(MOTOR_PIN1, OUTPUT);
  pinMode(MOTOR_PIN2,OUTPUT);
  pinMode(LED,OUTPUT);
  Serial.begin(9600);
}

// DENTRO DEL LOOP PRIMERO LLAMAMOS A LAS VARIABLES QUE USAREMOS.
// LUEGO, MEDIANTE UN IF PREGUNTAMOS SI LA LECTURA ES = 0 O = 1, 
//DEPENDIENDO DE ESTA LECTURA, EL CONTADOR MOSTRARA LA FUNCION DE
// NUMEROS PRIMOS, O MOSTRARA UN CONTADOR DE LOS SEGUNDOS.
// ASI MISMO, TAMBIEN DEPENDIENDO DEL ESTADO DEL DESLIZANTE, 
// EL MOTOR GIRARA EN UN SENTIDO U EN OTRO, DEPENDIENDO QUE NUMEROS
// ESTE MOSTRANDO.
void loop()
{
  int lectura = analogRead(TEMPERATURA);
  int girar_izquierda = digitalRead(MOTOR_PIN1);
  int estado_deslizante = digitalRead(BOTON_DESLIZANTE);
  float temperatura = analogRead(TEMPERATURA);
  if (estado_deslizante == 0){
  

    while (estado_deslizante == 0)
      {
      estado_deslizante = digitalRead(BOTON_DESLIZANTE);
      float temperatura = analogRead(TEMPERATURA);
      
      MOTOR_IZQUIERDA();
      if (estado_deslizante == 1){
      	break;
      }
      secuencia(contador, estado_deslizante,lectura);
      
    	}
    
  }
  else
  {
    
    for (int i = 2; i <= 99; i++){
      int girar_derecha=digitalRead(MOTOR_PIN2);
      float temperatura = analogRead(TEMPERATURA);
      estado_deslizante = digitalRead(BOTON_DESLIZANTE);
      bool esPrimo = num_primos(i);
      CELCIUS();
      if (estado_deslizante == 0){
      	break;
      }
      if (esPrimo == true){
        mostrar_contador(i);
        MOTOR_DERECHA();
        
        }
      }
   }
}

////////////////////////////////

void secuencia(int contador, int estado_deslizante, int lectura){
  	
  	contador = 0;
  	while (estado_deslizante == 0 && contador < 99){
      
      estado_deslizante = digitalRead(BOTON_DESLIZANTE);
      lectura = analogRead(TEMPERATURA);
  	  FAHRENHEIT();
      
      
      if (estado_deslizante == 1){
      	break;
      }
      contador++;
  	  mostrar_contador(contador);
      
  	   }
   }

////////////////////////////////////////
// FUNCION CON LA UNICA FUNCION DE GIRAR EL MOTOR HACIA EL LADO IZQUIERDO,
// LUEGO, DICHA FUNCION SE LLAMA DENTRO DEL BUCLE PARA DARLE 
// MOVIMIENTO AL MOTOR DEPENDIENDO DE QUE LADO ESTE EL DESLIZANTE
void MOTOR_IZQUIERDA()
{ 
 	
 digitalWrite(MOTOR_PIN1,HIGH);
 digitalWrite(MOTOR_PIN2,LOW);
 delay(1000);
 
} 
  
/////////////////////////////////
// FUNCION CON LA UNICA FUNCION DE GIRAR EL MOTOR HACIA EL LADO DERECHO,
// LUEGO, DICHA FUNCION SE LLAMA DENTRO DEL BUCLE PARA DARLE 
// MOVIMIENTO AL MOTOR DEPENDIENDO DE QUE LADO ESTE EL DESLIZANTE
void MOTOR_DERECHA()
{

    digitalWrite(MOTOR_PIN2,HIGH);
  	digitalWrite(MOTOR_PIN1,LOW);
    delay(1000);

}
int lectura;
float temperatura_fahrenheit;
float temperatura_celcius;
/////////////////////////////
// LA SIGUIENTE FUNCION LO QUE HACE ES QUE CONVIERTE EL NUMERO DE
// EL SENSOR DE  TEMPERATURA A CELCIUS Y LUEGO LO PRINTEA
// POR CONSOLA, TAMBIEN DEPENDIENDO PARA QUE LADO SE ENCUENTRE EL INTERRUPTOR.
// SE LE AGREGA LA FUNCIONALIDAD DE QUE, CUANDO LA TEMPERATURA SEA MAYOR A 50° NOS ENVIE
// UNA SEÑAL VISUAL MEDIANTE EL LED Y LA FOTORRESISTENCIA PARA AVISARNOS DE QUE EL PROGRAMA
// PODRIA TENER DAÑOS POR TRABAJAR A ALTAS TEMPERATURAS, ASI MISMO, LA SEÑAL DE ALERTA ES
// PRINTEADA POR CONSOLA.
void CELCIUS()
{
  int fotor = analogRead(FOTORRESISTENCIA);
  int lectura = analogRead(TEMPERATURA);
  float temperatura_celcius = map(lectura,20,358,-40,125);
  if (temperatura_celcius > 50){
    digitalWrite(LED,HIGH);
  	Serial.print("CUIDADO, LA TEMPERATURA SOBREPASA LOS 50CELCIUS\n");
  	Serial.println("EL SISTEMA PODRIA QUEMARSE");}
    	
  else if (temperatura_celcius < 50){
      digitalWrite(LED,LOW);}
  Serial.print("La temperatura en CELCIUS es: ");
  Serial.println(temperatura_celcius);
}          
///////////////     
// LA SIGUIENTE FUNCION LO QUE HACE ES QUE CONVIERTE EL NUMERO DE
// EL SENSOR DE  TEMPERATURA A FAHRENHEIT Y LUEGO LO PRINTEA
// POR CONSOLA, TAMBIEN DEPENDIENDO PARA QUE LADO SE ENCUENTRE EL INTERRUPTOR
// SE LE AGREGA LA FUNCIONALIDAD DE QUE, CUANDO LA TEMPERATURA SEA MAYOR A 122° NOS ENVIE
// UNA SEÑAL VISUAL MEDIANTE EL LED Y LA FOTORRESISTENCIA PARA AVISARNOS DE QUE EL PROGRAMA
// PODRIA TENER DAÑOS POR TRABAJAR A ALTAS TEMPERATURAS, ASI MISMO, LA SEÑAL DE ALERTA ES
// PRINTEADA POR CONSOLA.
void FAHRENHEIT()
{
  int lectura = analogRead(TEMPERATURA);
  float temperatura_fahrenheit = map(lectura,20,358,-40,125) * 9.0 / 5.0 + 32;
  if (temperatura_fahrenheit >122){
    digitalWrite(LED,HIGH);
    Serial.print("CUIDADO, LA TEMPERATURA SOBREPASA LOS 122 FAHRENHEIT\n");
  	Serial.println("EL SISTEMA PODRIA QUEMARSE");}
  else if (temperatura_fahrenheit <122){
    digitalWrite(LED,LOW);}
  Serial.print("La temperatura en FAHRENHEIT es: ");
  Serial.println(temperatura_fahrenheit);

}
////////////////////////////////////7

//LA SIGUIENTE FUNCION RECORRE EL CONTADOR Y SOLO MUESTRA
// LOS NUMEROS QUE CUMPLAN CON LA CONDICION DE SER PRIMOS
bool num_primos(int contador)
{
  if (contador <= 1) {
    return false;
  }

  for (int i = 2; i <= contador / 2; i++) {
    if (contador % i == 0) {
      return false; 
    }
  }
  return true;
  
}

////////////////////////////////
// LA SIGUIENTE FUNCION RECIBE EL CONTADOR COMO PARAMETRO, 
// LO QUE SE ME OCURRIO FUE QUE, AL MANTENER LA DECENA POR UN DELAY MAS LARGO
// Y LA UNIDAD POR MENOS TIEMPO DA LA SENSACION DE QUE SIEMPRE
//SE ENCUENTRA PRENDIDA Y LO QUE VA CAMBIANDO SON SOLO LAS UNIDADES
// YA QUE 60 MILISEGUNDOS ES UN TIEMPO MUY CORTO.

void mostrar_contador(int contador)
{
   
  prenderLeds(contador - 10 * ((int)contador / 10));
  digitalWrite(UNIDAD,LOW);
  digitalWrite(DECENA,HIGH);
  
  delay(60);
  
  digitalWrite(UNIDAD,HIGH);
  digitalWrite(DECENA,LOW);
  prenderLeds((int)contador / 10);

  delay(500);
  
}

/////////////////////////
// LA SIGUIENTE FUNCION RECIBE COMO PARAMETRO EL CONTADOR Y LUEGO,
// INICIALIZA TODOS LOS LEDS EN APAGADOS, ENTONCES, CUANDO NOSOTROS
// LLAMEMOS AL CONTADOR, SEGUN EL LED SE PRENDERA UN NUMERO. 
void prenderLeds(int contador)
{
  digitalWrite(A, LOW);
  digitalWrite(B, LOW);
  digitalWrite(C, LOW);
  digitalWrite(D, LOW);
  digitalWrite(E, LOW);
  digitalWrite(F, LOW);
  digitalWrite(G, LOW);
  
  switch (contador)
  {
    case 0:
    	digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);
    	break;
    
    case 1:
    	digitalWrite(B, HIGH);
  		digitalWrite(C, HIGH);
  		break;

    case 2:
    	digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(G, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        break;

    case 3:
    	digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 4:
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(G, HIGH);
        digitalWrite(F, HIGH);
        break;

    case 5:
        digitalWrite(A, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 6:
        digitalWrite(A, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 7:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        break;

    case 8:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(D, HIGH);
        digitalWrite(E, HIGH);
        digitalWrite(F, HIGH);
        digitalWrite(G, HIGH);
        break;

    case 9:
        digitalWrite(A, HIGH);
        digitalWrite(B, HIGH);
        digitalWrite(C, HIGH);
        digitalWrite(F, HIGH);
      	digitalWrite(G, HIGH);
        break;

  }
  
}
