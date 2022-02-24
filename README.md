# Arduino
**
const int nBotones = 7;
boolean notaioff[nBotones];
int contador[nBotones];
byte nota[] = {60,62,64,65,67,69,71};
const int nPots=2;
int lecturas[nPots]; 
int lecturasAnteriores[nPots];
void setup() {

 Serial.begin(115200);
  for (int i=0;i<nBotones;i++)
  {
   pinMode(i+2, INPUT_PULLUP);
  }
}

void loop() {
  for (int i=0; i<nBotones; i++) 
      if (digitalRead(i+2) == LOW)
      {
        if (contador[i]==0)
        {
          if (notaioff[i]== 1)
          {
            contador[i]=calibracion; 
            midi(144,notas[i],100); 
            notaioff[i] = 0; 
          }
        }
        
      }
      else 
      {
        if (contador[i]==0) 
        {
          if (notaioff[i] ==0)
          {
            contador[i]=calibracion; 
            midi(128,notas[i],0);
            notaioff[i] = 1;
          }
        }
      }
  }
for (int i=0; i<nBotones;i++)

{
if (contador[i]>0) contador[i]--;
}

for (int k=0; k<nPots; k++) 
 {
  lecturas[k] = map(analogRead(k),0,1023,0,255);
 }

for (int k=0; k<nPots;k++)
{    
    if (lecturas[k] > (lecturasAnteriores[k]+1) || lecturas[k] < (lecturasAnteriores[k]-1) )
      {                   
          midi(176,k+30,lecturas[k]/2);
          lecturasAnteriores[k] = lecturas[k];
      } 
}
  
}

void midi(unsigned char command, unsigned char note,unsigned char vel)
{
  Serial.write(command);
  Serial.write(note);
  Serial.write(vel);
}
**
