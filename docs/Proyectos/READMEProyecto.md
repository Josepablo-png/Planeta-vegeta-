# Carrito de Fútbol con Control Xbox (ESP32)

Carrito de tracción diferencial controlado con un control inalámbrico de Xbox por Bluetooth Low Energy (BLE). Cuenta con un mecanismo de tiro accionado por un servo SG90.

## Configuración de librerías para el Control Inalámbrico

Para usar el control de Xbox con el ESP32 se usaron las siguientes librerías (instalar desde **Herramientas → Administrar bibliotecas** en el IDE de Arduino):

| Librería | Uso en el proyecto | Enlace |
|---|---|---|
| `BLE-Gamepad-Client` (tbekas) | Conexión y lectura del control Xbox por BLE (`XboxController`, `XboxControlsState`)
| `ESP32Servo` (madhephaestus) | Manejo del servo SG90 del mecanismo de tiro (`Servo`, `ESP32PWM`)

Notas:

* En PlatformIO, `BLE-Gamepad-Client` se agrega en `lib_deps` como `tbekas/BLE-Gamepad-Client@^0.12.1`.
* `BLE-Gamepad-Client` está construida sobre NimBLE-Arduino, que se instala como dependencia.
* No se usa Bluepad32, por lo que **no** hacen falta URLs adicionales de gestor de tarjetas. Basta con el paquete oficial de ESP32 para Arduino.
* El código usa `ledcAttachChannel`, que pertenece a la versión 3.x del paquete ESP32 para Arduino. Con versiones 2.x no compila.
* El control Xbox debe tener el firmware 5.x. Se puede actualizar desde la app *Accesorios Xbox*.

## Emparejamiento del control

1. Cargar el programa en el ESP32 y abrir el monitor serial a **115200 baudios**.
2. Encender el control presionando el botón Xbox.
3. Mantener presionado el botón de emparejamiento del control durante 3 segundos y soltarlo.
4. El LED integrado del ESP32 deja de parpadear y queda **fijo** cuando el control está conectado.

## Protocolo de Comandos

* **Palanca izquierda (Eje Y):** controla avance y retroceso.
* **Palanca izquierda (Eje X):** controla el giro. Se aplica una mezcla tipo arcade: motor izquierdo = Y + X, motor derecho = Y − X.
  * Solo adelante o atrás: ambas ruedas igual.
  * Adelante + lado: curva, la rueda interior va más lenta.
  * Solo lado: el carro gira sobre su propio eje.
* **Zona muerta:** 0.12 sobre el recorrido de la palanca (de 0 a 1). Dentro de esa zona los motores se detienen.
* **Botones LB / RB:** bajan o suben el nivel de velocidad (3 niveles).
  * Nivel 1 (bajo): PWM máximo 150
  * Nivel 2 (medio): PWM máximo 210 (nivel inicial)
  * Nivel 3 (alto): PWM máximo 255
  * El PWM mínimo de arranque es 60 para vencer la fricción de la caja reductora.
* **Botón A o Gatillo derecho (RT, más de la mitad del recorrido):** tiro con el servo. Pasa de 0° a 90° durante 120 ms y regresa a 0°. Hay 300 ms de recarga antes de poder tirar de nuevo.
* **Botón B:** paro de emergencia. Mientras esté presionado, los motores se mantienen en 0.

## Comportamiento y seguridad

* **Aceleración suave:** la velocidad sube como máximo 25 unidades de PWM cada 10 ms. Frenar o bajar la velocidad es inmediato. Al invertir el sentido, el motor pasa primero por cero.
* **Control desconectado:** los motores se detienen y el LED integrado parpadea mientras se busca el control. En el monitor serial aparece "Control no conectado...".
* **Freno:** con velocidad cero ambos pines IN del driver se ponen en alto (freno corto).
* **Watchdog opcional (`USAR_WATCHDOG`):** desactivado por defecto. Si se activa, detiene los motores cuando la palanca no cambia durante 1000 ms (`TIEMPO_FAILSAFE`).
* **Servo:** se desacopla (`detach`) cuando no está tirando para evitar vibración y consumo.

## Conexiones

Alimentación: portapilas de 4 pilas AA con switch hacia 5V del ESP32, VM del TB6612FNG y VCC del servo. Todos comparten GND. El VCC lógico del TB6612FNG se conecta al 3V3 del ESP32.

| Función | Pin del ESP32 | Pin del componente |
|---|---|---|
| PWM motor A (izquierdo) | GPIO14 | PWMA (TB6612FNG) |
| Dirección motor A | GPIO27 / GPIO26 | AIN1 / AIN2 |
| Dirección motor B | GPIO25 / GPIO33 | BIN1 / BIN2 |
| PWM motor B (derecho) | GPIO32 | PWMB |
| Standby del driver | GPIO4 | STBY |
| Señal del servo | GPIO13 | Señal (SG90) |
| LED indicador | GPIO2 | LED integrado |

Los motores TT se conectan a AO1/AO2 (izquierdo) y BO1/BO2 (derecho).

## Ajustes en el código

Constantes al inicio del programa:

| Constante | Valor por defecto | Para qué sirve |
|---|---|---|
| `INVERTIR_Y` | `false` | Poner `true` si al empujar la palanca arriba el carro va hacia atrás |
| `INVERTIR_X` | `false` | Poner `true` si el carro gira al lado contrario |
| `INVERTIR_MOTOR_A` | `true` | Invierte el sentido del motor izquierdo |
| `INVERTIR_MOTOR_B` | `false` | Invierte el sentido del motor derecho |
| `ZONA_MUERTA` | `0.12` | Zona muerta de la palanca |
| `PWM_MINIMO` | `60` | PWM mínimo al moverse |
| `NIVELES` | `{150, 210, 255}` | PWM máximo de cada nivel de velocidad |
| `PASO_ACEL` | `25` | Aumento máximo de PWM por ciclo de 10 ms |
| `POS_REPOSO` / `POS_GOLPE` | `0` / `90` | Ángulos del servo |
| `TIEMPO_GOLPE` / `TIEMPO_RECARGA` | `120` / `300` ms | Duración del golpe y de la recarga |
| `USAR_WATCHDOG` | `false` | Activa el paro por falta de cambios en la palanca |
| `DEBUG` | `true` | Imprime datos por el monitor serial |

## Depuración

Con `DEBUG` en `true`, cada 250 ms el monitor serial (115200 baudios) muestra:

X:  0.00 Y:  0.85 | A: 210 B: 210 | nivel:2 | rep/s:120


* `X`, `Y`: posición de la palanca izquierda.
* `A`, `B`: PWM actual del motor izquierdo y derecho (negativo = reversa).
* `nivel`: nivel de velocidad seleccionado (1 a 3).
* `rep/s`: reportes por segundo que llegan del control, útil para detectar mala conexión.

## Problemas comunes

* **Un motor gira al revés:** cambiar `INVERTIR_MOTOR_A` o `INVERTIR_MOTOR_B`, o invertir sus dos cables en el driver.
* **El carro va hacia atrás al empujar arriba:** poner `INVERTIR_Y` en `true`.
* **El ESP32 se reinicia al arrancar los motores:** agregar un capacitor de 470 a 1000 µF entre +V y GND cerca del TB6612FNG.
* **Los motores no se mueven:** revisar que STBY esté conectado a GPIO4 y que el VCC lógico del TB6612FNG esté en 3V3.
* **No conecta el control:** verificar el firmware 5.x y repetir el emparejamiento.
* **Al programar por USB:** apagar el switch de las pilas para no unir las dos fuentes de alimentación.

## Código completo (Arduino IDE)

```cpp
#include <Arduino.h>
#include <BLEGamepadClient.h>
#include <ESP32Servo.h>

XboxController controller;
Servo brazoServo;

// Pines TB6612FNG
const int pinPWMA = 14;
const int pinAIN1 = 27;
const int pinAIN2 = 26;
const int pinBIN1 = 25;
const int pinBIN2 = 33;
const int pinPWMB = 32;
const int pinSTBY = 4;

// Servo SG90 (mecanismo de tiro)
const int pinSERVO = 13;
const int POS_REPOSO = 0;
const int POS_GOLPE  = 90;

// LED integrado: parpadea buscando control, fijo si esta conectado
const int pinLED = 2;

// ================== AJUSTES ==================
const bool INVERTIR_Y = false;       // si empujas arriba y va hacia atras, pon true
const bool INVERTIR_X = false;       // si gira al lado contrario, pon true
const bool INVERTIR_MOTOR_A = true; // motor izquierdo (si gira al reves, pon true)
const bool INVERTIR_MOTOR_B = false; // motor derecho

const float ZONA_MUERTA = 0.12;
const int   PWM_MINIMO  = 60;
const int   NIVELES[3]  = {150, 210, 255};  // R2: 3 niveles (baja, media, alta)
int nivel = 1;                              // arranca en media

const int PASO_ACEL = 25;                   // aceleracion limitada por tick
const unsigned long PERIODO_CONTROL = 10;

// Tiro
const unsigned long TIEMPO_GOLPE   = 120;
const unsigned long TIEMPO_RECARGA = 300;

// R4: se detiene siempre que el control se desconecta.
// Opcional: detener si la palanca no cambia en este tiempo
const bool USAR_WATCHDOG = false;
const unsigned long TIEMPO_FAILSAFE = 1000;

const bool DEBUG = true;
// =============================================

const int PWM_FREQ = 20000;
const int PWM_RES  = 8;

// Estado de motores (con signo: positivo adelante, negativo atras)
int actualA = 0;
int actualB = 0;

// Estado del tiro
enum EstadoTiro { TIRO_LIBRE, TIRO_EXTENDIDO, TIRO_RECARGA };
EstadoTiro estadoTiro = TIRO_LIBRE;
unsigned long inicioTiro = 0;

// Estado de botones y reportes
bool prevLB = false, prevRB = false, prevTiro = false;
float ultX = 0, ultY = 0;
unsigned long ultimoCambio = 0;
unsigned long ultimoControl = 0;
unsigned long ultimoDebug = 0;
unsigned long reportes = 0;

// ---------- Motores ----------
void direccionMotor(int in1, int in2, int valor) {
  if (valor > 0) {
    digitalWrite(in1, HIGH); digitalWrite(in2, LOW);
  } else if (valor < 0) {
    digitalWrite(in1, LOW);  digitalWrite(in2, HIGH);
  } else {
    digitalWrite(in1, HIGH); digitalWrite(in2, HIGH);  // freno corto
  }
}

void escribirMotores(int a, int b) {
  if (INVERTIR_MOTOR_A) a = -a;
  if (INVERTIR_MOTOR_B) b = -b;
  direccionMotor(pinAIN1, pinAIN2, a);
  direccionMotor(pinBIN1, pinBIN2, b);
  ledcWrite(pinPWMA, abs(a));
  ledcWrite(pinPWMB, abs(b));
}

void detenerTodo() {
  actualA = 0;
  actualB = 0;
  escribirMotores(0, 0);
}

// Aceleracion limitada; frenar o bajar velocidad es inmediato
int rampa(int actual, int objetivo) {
  if ((actual > 0 && objetivo < 0) || (actual < 0 && objetivo > 0)) actual = 0;
  if (abs(objetivo) <= abs(actual)) return objetivo;
  if (objetivo > 0) return min(actual + PASO_ACEL, objetivo);
  return max(actual - PASO_ACEL, objetivo);
}

int valorPWM(float v) {
  float a = fabsf(v);
  if (a < 0.03) return 0;
  int pwm = PWM_MINIMO + (int)(a * (NIVELES[nivel] - PWM_MINIMO));
  return v > 0 ? pwm : -pwm;
}

// Mezcla tipo "arcade": Y = avance, X = giro
void calcularObjetivo(float x, float y, int &objA, int &objB) {
  float mag = sqrtf(x * x + y * y);
  if (mag < ZONA_MUERTA) { objA = 0; objB = 0; return; }
  if (mag > 1.0f) { x /= mag; y /= mag; mag = 1.0f; }
  float escala = (mag - ZONA_MUERTA) / (1.0f - ZONA_MUERTA);
  x = x / mag * escala;
  y = y / mag * escala;

  float izq = y + x;  // motor A = izquierdo
  float der = y - x;  // motor B = derecho
  float mx = fmaxf(fabsf(izq), fabsf(der));
  if (mx > 1.0f) { izq /= mx; der /= mx; }

  objA = valorPWM(izq);
  objB = valorPWM(der);
}

// ---------- Tiro (servo) ----------
void iniciarTiro() {
  if (estadoTiro != TIRO_LIBRE) return;
  if (!brazoServo.attached()) brazoServo.attach(pinSERVO, 500, 2400);
  brazoServo.write(POS_GOLPE);
  inicioTiro = millis();
  estadoTiro = TIRO_EXTENDIDO;
}

void actualizarTiro(unsigned long ahora) {
  switch (estadoTiro) {
    case TIRO_EXTENDIDO:
      if (ahora - inicioTiro >= TIEMPO_GOLPE) {
        brazoServo.write(POS_REPOSO);
        estadoTiro = TIRO_RECARGA;
      }
      break;
    case TIRO_RECARGA:
      if (ahora - inicioTiro >= TIEMPO_GOLPE + TIEMPO_RECARGA) {
        brazoServo.detach();
        estadoTiro = TIRO_LIBRE;
      }
      break;
    default:
      break;
  }
}

void setup() {
  Serial.begin(115200);
  delay(500);

  Serial.println();
  Serial.println("========================================");
  Serial.println("   CARRITO FUTBOL - CONTROL XBOX");
  Serial.println("========================================");

  ESP32PWM::allocateTimer(0);
  brazoServo.setPeriodHertz(50);
  brazoServo.attach(pinSERVO, 500, 2400);
  brazoServo.write(POS_REPOSO);
  delay(400);
  brazoServo.detach();

  pinMode(pinAIN1, OUTPUT);
  pinMode(pinAIN2, OUTPUT);
  pinMode(pinBIN1, OUTPUT);
  pinMode(pinBIN2, OUTPUT);
  pinMode(pinSTBY, OUTPUT);
  digitalWrite(pinSTBY, HIGH);
  pinMode(pinLED, OUTPUT);

  ledcAttachChannel(pinPWMA, PWM_FREQ, PWM_RES, 4);
  ledcAttachChannel(pinPWMB, PWM_FREQ, PWM_RES, 5);

  detenerTodo();

  controller.begin();
  Serial.println("Esperando control Xbox...");
  ultimoCambio = millis();
}

void loop() {
  unsigned long ahora = millis();
  actualizarTiro(ahora);

  if (!controller.isConnected()) {
    detenerTodo();
    prevLB = prevRB = prevTiro = false;
    ultimoCambio = ahora;
    digitalWrite(pinLED, (ahora / 250) % 2);

    static unsigned long ultimoMensaje = 0;
    if (ahora - ultimoMensaje > 1000) {
      Serial.println("Control no conectado...");
      ultimoMensaje = ahora;
    }
    delay(10);
    return;
  }

  digitalWrite(pinLED, HIGH);

  XboxControlsState s;
  controller.read(&s);

  bool lb = s.leftBumper, rb = s.rightBumper;
  if (rb && !prevRB && nivel < 2) { nivel++; Serial.printf("Nivel %d\n", nivel + 1); }
  if (lb && !prevLB && nivel > 0) { nivel--; Serial.printf("Nivel %d\n", nivel + 1); }
  prevLB = lb;
  prevRB = rb;

  bool pedirTiro = s.buttonA || (s.rightTrigger > 0.5);
  if (pedirTiro && !prevTiro) iniciarTiro();
  prevTiro = pedirTiro;

  if (s.leftStickX != ultX || s.leftStickY != ultY) {
    ultX = s.leftStickX;
    ultY = s.leftStickY;
    ultimoCambio = ahora;
    reportes++;
  }

  if (ahora - ultimoControl >= PERIODO_CONTROL) {
    ultimoControl = ahora;

    float x = s.leftStickX;
    float y = s.leftStickY;
    if (INVERTIR_Y) y = -y;
    if (INVERTIR_X) x = -x;

    int objA, objB;
    calcularObjetivo(x, y, objA, objB);

    if (s.buttonB) { objA = 0; objB = 0; }

    if (USAR_WATCHDOG && (objA != 0 || objB != 0) && (ahora - ultimoCambio >= TIEMPO_FAILSAFE)) {
      objA = 0;
      objB = 0;
    }

    actualA = rampa(actualA, objA);
    actualB = rampa(actualB, objB);
    escribirMotores(actualA, actualB);

    if (DEBUG && ahora - ultimoDebug >= 250) {
      Serial.printf("X:%6.2f Y:%6.2f | A:%4d B:%4d | nivel:%d | rep/s:%lu\n",
                    x, y, actualA, actualB, nivel + 1, reportes * 4);
      reportes = 0;
      ultimoDebug = ahora;
    }
  }
}