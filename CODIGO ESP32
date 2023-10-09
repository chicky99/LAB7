/*****************************************************************************
// Universidad del Valle de Guatemala
// BE3015 - Electrónica Digital 2
// Jose Luis Monterroso - 20142
// Laboratorio 5
//*****************************************************************************
//*****************************************************************************
// Librerías
//*****************************************************************************
#include <Arduino.h>
#include <LiquidCrystal.h>
#include <Wire.h>
//*****************************************************************************
// Definición de pines
//*****************************************************************************
#define d4 18
#define d5 19
#define d6 21
#define d7 22
#define en 2
#define rs 15
#define pot1 26
#define pot2 36


//*****************************************************************************
// Prototipos de función
//*****************************************************************************
void voltajeloop();

//*****************************************************************************
// Variables Globales
//*****************************************************************************
LiquidCrystal LCD(rs, en, d4, d5, d6, d7);
uint8_t decenas, unidades, decimal;
int adcRaw;
float voltaje;
//*****************************************************************************
// ISR-Interrupciones
//*****************************************************************************

//*****************************************************************************
// Funciones Principales
//*****************************************************************************
void setup(){
  Serial2.begin(115200);
  LCD.begin(16,2);
}

void loop(){
  voltajeloop();
}
//*****************************************************************************
// Funciones Secundarias
//*****************************************************************************
void voltajeloop() {
  while (Serial2.available() > 0) {
    char c = Serial2.read();
    if (c == 'C') {
      // Se detectó un mensaje de la TIVA C
      int cpuValue = Serial2.parseInt(); // Leer el valor del contador
      // Leer el encabezado "POTC "
      while (Serial2.read() != 'P') {} // Esperar a que llegue la 'P'
      Serial2.read(); // Leer la 'O'
      Serial2.read(); // Leer la 'T'
      Serial2.read(); // Leer la 'C'
      float potcValue = Serial2.parseFloat(); // Leer el valor del potenciómetro
      LCD.setCursor(6, 0);
      LCD.print("Potc");
      LCD.setCursor(6, 1);
      LCD.print(potcValue);
      LCD.setCursor(11, 0);
      LCD.print("CPU");
      LCD.setCursor(11, 1);
      LCD.print(cpuValue);
    }
  }
  voltaje = analogRead(pot1) / 10.0;
  int temp = voltaje;
  decenas = temp / 100.0;
  temp = temp - decenas * 100.0;
  unidades = temp / 10.0;
  temp = temp - unidades * 10.0;
  decimal = temp;

  // Enviar los valores al microcontrolador TIVA C a través de la comunicación USART
  Serial2.print("ESP32 ");
  Serial2.print("POTE ");
  Serial2.print(decenas);
  Serial2.print('.');
  Serial2.print(unidades);
  Serial2.print(decimal);
  Serial2.println(" V");

  LCD.setCursor(0, 1);
  LCD.print("    ");
  
  LCD.setCursor(0, 0);
  LCD.print("Poti");
  LCD.setCursor(0, 1);
  LCD.print(decenas);
  LCD.print('.');
  LCD.print(unidades);
  LCD.print(decimal);

  delay(250);
}





