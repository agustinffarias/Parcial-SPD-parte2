# COLABORADOR:
Santiago DECIBE - Tomas Agustin FARIAS

# Link de tinkercad parte 2:
https://www.tinkercad.com/things/dGNinya2tob-con-sensor-de-temperatura/editel?sharecode=7RmI-q71zYKV9LyrGuB_Ndgmlrxn2aZJloYqPpmNbSg
# MAPA:
![Tinkercad] (imagenes/imagen_proyecto2.pdf)
# Descripcion del programa:
El siguiente programa tiene la funcion de, que cuando el deslizante se encuentre con el valor 0, el motor girara hacia el lado izquierdo, asi mismo, por consola se printearan los grados representados en CELCIUS, y en el 7
segmentos se mostraran los numeros primos del 0 al 99.
En el caso de que el deslizante se encuentre retornando el valor 1, el motor girara hacia la derecha, el 7 segmentos mostrara como un contador ascendente los numeros del 1 al 99 y en consola se printearan los grados
extraidos del sensor de temperatura, pero, esta vez, en FAHRENHEIT.

# Componentes adicionales con respecto a la parte 1:
Uno de los componentes adicionales es un MOTOR DE CC: el mismo tiene dos funciones, puede girar hacia la derecha o hacia la izquierda, para poder realizar dicha actividad el motor de CC necesita tambien
un controlador de motor de puente, ya que, de otra manera, solo giraria en un sentido, y nuestra idea era que gire para diferentes lados segun lo que se este mostrando por consola y por el 7 segmentos para darle
un efecto extra visual.
El segundo componente que elegimos agregar, por una manera de practicidad, fue el SENSOR DE TEMPERATURA, el mismo en nuestro codigo cumple una funcion que nosotros la dividimos en dos con el deslizante.
El sensor de temperatura se puede manejar de manera manual, subiendo y bajandole la temperatura, aunque tambien se podria utilizar con un bucle en el que creariamos un entorno controlado. Dicho sensor, tiene
la capacidad de medir la temperatura desde -40 hasta 125°

# Codigo:
// DEFINIMOS EN QUE LUGAR ESTARAN CONECTADOS LOS ELEMENTOS
// DENTRO DEL ARDUINO UNO (EN QUE PIN) 

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
// FUNCION CON LA UNICA FUNCION DE GIRAR HACIA EL LADO IZQUIERDO,
// LUEGO, DICHA FUNCION SE LLAMA DENTRO DEL BUCLE PARA DARLE 
// MOVIMIENTO AL MOTOR DEPENDIENDO DE QUE LADO ESTE EL DESLIZANTE
void MOTOR_IZQUIERDA()
{ 
 	
 digitalWrite(MOTOR_PIN1,HIGH);
 digitalWrite(MOTOR_PIN2,LOW);
 delay(1000);
 
} 
  
/////////////////////////////////
// FUNCION CON LA UNICA FUNCION DE GIRAR HACIA EL LADO DERECHO,
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
// POR CONSOLA, TAMBIEN DEPENDIENDO PARA QUE LADO SE ENCUENTRE EL INTERRUPTOR
void CELCIUS()
{
  int lectura = analogRead(TEMPERATURA);
  float temperatura_celcius = map(lectura,20,358,-40,125);
  
  Serial.print("La temperatura en CELCIUS es: ");
  Serial.println(temperatura_celcius);
}          
///////////////     
// LA SIGUIENTE FUNCION LO QUE HACE ES QUE CONVIERTE EL NUMERO DE
// EL SENSOR DE  TEMPERATURA A FAHRENHEIT Y LUEGO LO PRINTEA
// POR CONSOLA, TAMBIEN DEPENDIENDO PARA QUE LADO SE ENCUENTRE EL INTERRUPTOR
void FAHRENHEIT()
{
  int lectura = analogRead(TEMPERATURA);
  float temperatura_fahrenheit = map(lectura,20,358,-40,125) * 9.0 / 5.0 + 32;
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
