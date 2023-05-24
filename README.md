#Primer parcial spd

En el codigo del proyecto de spd como primer paso lo que hago es definir todas las conexiones poniendo un nombre el cual las diferencia de cada una 
```
#define ledVerde 12
#define ledRojo 13
#define SEG_B 7
#define SEG_A 8
#define SEG_F 6
#define SEG_G 5
#define SEG_C A0
#define SEG_D A2
#define SEG_E A1
#define BotonSube A3
#define BotonBaja A4
#define BotonFrenar A5
#define trayecto 3000
```
luego declaro las variables me serviran para cada parte del codigo algunas de ellas son:
```
int estado;
```
la cual utilizare para guardar el estado del boton y otra puede ser:
```
bool detenido = true;
```
Esta es un valor booleano que utilizare para saber en si el montacargas esta detenido o no.

Luego en el void setup declaro todos los pines que utilizare y que tipo de respuesta va a tener interna o externa.Adema del monitor serial para mostrar los mensajes.
```
Serial.begin(9600);
  pinMode(ledVerde, OUTPUT);
  pinMode(ledRojo, OUTPUT);
  pinMode(SEG_A, OUTPUT);
  pinMode(SEG_B, OUTPUT);
  pinMode(SEG_C, OUTPUT);
  pinMode(SEG_D, OUTPUT);
  pinMode(SEG_E, OUTPUT);
  pinMode(SEG_F, OUTPUT);
  pinMode(SEG_G, OUTPUT);
  pinMode(BotonSube, INPUT_PULLUP);
  pinMode(BotonBaja, INPUT_PULLUP);
  pinMode(BotonFrenar, INPUT_PULLUP);
```

Lo que realize fue que el montacargas realize un recorrido de tres pisos. Por eso hice tres funciones diferentes para cada piso las cuales prende las luces del 7 segmentos formando el numero de cada piso. 
ejemplo de la funcion para el numero tres
```
void pisoTres()
{
  digitalWrite(SEG_A, HIGH);	
  digitalWrite(SEG_B, HIGH);
  digitalWrite(SEG_G, HIGH);	
  digitalWrite(SEG_C, HIGH);
  digitalWrite(SEG_D, HIGH);
  digitalWrite(SEG_A, LOW);	
  digitalWrite(SEG_B, LOW);
  digitalWrite(SEG_G, LOW);	
  digitalWrite(SEG_C, LOW);
  digitalWrite(SEG_D, LOW);
}
```
Ademas tambien hice tres funciones para los botones que como dije antes eran input-pullup
estas tiene una direccion que es hacia la cual va ir el monta cargas (0 para frenar,1 para subir y -1 para descender).
tambien tiene la asignacion de la variable booleano detenido en la cual uno sera true y e los otros false.
Dentro de cada una de ellas preguntare el estado de su boton y si esta detenido si esta funcion se cumple prenderan la luz que deben prender y ademas imprimiran en el monitor serian cada uno de los mensajes correspondientes. Por ultimo dentro de ellas se llamara a la funcion mostrar piso la cual es un switch que preguntara el piso actual y prendera el numero en el 7 segmentos

funcion mostrar piso
```
void mostrarPiso()
{
  switch(pisoActual)
  {
    case 1:
        pisoUno();
        break;
    case 2:
        pisodos();
        break;
    case 3:
        pisoTres();
        break; 
  }
```


funcion boton ascenso 
```
void botonSubir(int botonsubir, int leduno, int leddos,int direccion)
{
  direccion = 1;
  detenido = false;
  estado = digitalRead(botonsubir);
  if (estado == 0 && detenido==false)
  {
    pisoActual += direccion;
    Serial.print("Piso: ");
    Serial.println(pisoActual);
    digitalWrite(leduno, HIGH);
    digitalWrite(leddos, LOW);
    Serial.println("Montacargas en movimiento (subiendo).");
    delay(trayecto);
  }
  mostrarPiso();
}
```

funcion boton descenso
```
void botonBajar(int botonbajar, int leduno, int leddos,int direccion)
{
  direccion = -1;
  detenido = false;
  estado = digitalRead(botonbajar);
  if (estado == 0 && detenido==false)
  {
    pisoActual += direccion;
    Serial.print("Piso: ");
    Serial.println(pisoActual);
    digitalWrite(leduno, HIGH);
    digitalWrite(leddos, LOW);
    Serial.println("Montacargas en movimiento (bajando).");
    delay(trayecto);
  }
  mostrarPiso();
}
```
funcion boton frenar
```
void botonFrenar(int botonfrenar, int leduno, int leddos,int direccion)
{
  direccion = 0;
  detenido = true;
  estado = digitalRead(botonfrenar);
  if (estado == 0 && detenido==true)
  {
    pisoActual += direccion;
    Serial.print("Piso: ");
    Serial.println(pisoActual);
    digitalWrite(leduno, HIGH);
    digitalWrite(leddos, LOW);
    Serial.println("Montacargas detenido.");
    delay(trayecto);
  }
  mostrarPiso();
}
```

Por ultimo llamo dentro del loop a cada una de estas tres funciones para que se ejecuten siempre y cuando le pase los parametros adecuados.
```
void loop()
{  
  botonSubir(BotonSube, ledVerde, ledRojo,direccion);
  botonBajar(BotonBaja, ledVerde, ledRojo,direccion);
  botonFrenar(BotonFrenar, ledRojo, ledVerde,direccion);
}
```

> **Formiga Thiago 1B**
