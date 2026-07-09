#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <Servo.h>

// ── PINES ADAPTADOS PARA NANO ─────────────────────────────────────────
LiquidCrystal_I2C lcd(0x27, 16, 2);
Servo servoClasif;

const int SERVO_PIN = 9; // El Nano tiene pocos pines PWM, usa el 9
const int TCS_S0 = 2, TCS_S1 = 3, TCS_S2 = 4, TCS_S3 = 5, TCS_OUT = 6;
const int B2_IN1 = 7, B2_IN2 = 8, B2_EN = 10; // B2 en pines adaptados
const int IR_B2 = A0; 

// --- (El resto de las variables globales como antes) ---
enum EstadoClasif { ESPERA, DETECTANDO, CLASIFICANDO, LIBERANDO };
EstadoClasif estadoB2 = ESPERA;
bool b2_on = false;
uint8_t ultimoColor = 0;
unsigned long timerClasif = 0;

void setup() {
  Serial.begin(9600); // Bluetooth conectado a pines 0 y 1
  servoClasif.attach(SERVO_PIN);
  
  pinMode(TCS_S0, OUTPUT); pinMode(TCS_S1, OUTPUT);
  pinMode(TCS_S2, OUTPUT); pinMode(TCS_S3, OUTPUT);
  pinMode(TCS_OUT, INPUT);
  
  pinMode(B2_IN1, OUTPUT); pinMode(B2_IN2, OUTPUT); pinMode(B2_EN, OUTPUT);
  pinMode(IR_B2, INPUT);
  
  // Configuración TCS3200
  digitalWrite(TCS_S0, HIGH);
  digitalWrite(TCS_S1, LOW);
}

void loop() {
  // Simplificado para Nano
  logicaClasificacion();
}

void logicaClasificacion() {
  // Lógica igual que la anterior, asegurando el control de los pines del Nano
  switch(estadoB2) {
    case ESPERA:
      if(digitalRead(IR_B2) == LOW) { // IR detecta objeto
        analogWrite(B2_EN, 0); // Detener banda
        timerClasif = millis();
        estadoB2 = DETECTANDO;
      }
      break;
    // ... (continúa con la lógica de DETECTANDO, CLASIFICANDO, LIBERANDO)
  }
}
