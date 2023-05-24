/*Formiga Thiago1b
primerparcialPractico*/
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
int estado;
int direccion =0;
int pisoActual = 0;
int pisos;
bool detenido = true;
/*direccion 0 para frenar, 1 para subir y -1para bajar*/


void setup()
{
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
}

void loop()
{  
  botonSubir(BotonSube, ledVerde, ledRojo,direccion);
  botonBajar(BotonBaja, ledVerde, ledRojo,direccion);
  botonFrenar(BotonFrenar, ledRojo, ledVerde,direccion);
}

void pisoUno()
{
  digitalWrite(SEG_C, HIGH);	
  digitalWrite(SEG_B, HIGH);
  digitalWrite(SEG_B, LOW);	
  digitalWrite(SEG_C, LOW);
}

void pisodos()
{
  digitalWrite(SEG_A, HIGH);	
  digitalWrite(SEG_B, HIGH);
  digitalWrite(SEG_G, HIGH);	
  digitalWrite(SEG_E, HIGH);
  digitalWrite(SEG_D, HIGH);
  digitalWrite(SEG_A, LOW);	
  digitalWrite(SEG_B, LOW);
  digitalWrite(SEG_G, LOW);	
  digitalWrite(SEG_E, LOW);
  digitalWrite(SEG_D, LOW);
}
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

}
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
    delay(3000);
  }
  mostrarPiso();
}

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
    delay(3000);
  }
  mostrarPiso();
}
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
    delay(3000);
  }
  mostrarPiso();
}