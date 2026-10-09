# Reporte de Práctica: LED con Bluetooth

**Universidad Iberoamericana Puebla**
**Materia:** Introducción a la Mecatrónica
**Tema:** Bluetooth — LED con Bluetooth
**Periodo:** Otoño 2026

---

## 1. Metas y propósitos de la práctica

### Objetivo

El objetivo de esta práctica fue utilizar la conexión Bluetooth del ESP32 para recibir comandos desde un teléfono Android y controlar el encendido y apagado de un LED.

También se buscó comprender el funcionamiento de la comunicación inalámbrica y observar cómo un retraso puede afectar el tiempo de respuesta del sistema.

## 2. Componentes y materiales utilizados

| Cantidad | Componente |
|----------|------------|
| 1 | ESP32 |
| 1 | LED |
| 1 | Protoboard |
| Varios | Cables jumper |
| 1 | Cable USB |
| 1 | Teléfono Android |
| 1 | Aplicación de terminal Bluetooth |
| 1 | Computadora con Arduino IDE |

!!! note "Importante"
    Para realizar esta práctica se utilizó un teléfono con sistema operativo Android, ya que la comunicación Bluetooth empleada en la actividad y la aplicación utilizada estaban destinadas a este sistema.

## 3. Desarrollo de la práctica

Para comenzar, se preparó el ESP32 y se utilizó el GPIO 23 para controlar el LED. Después se conectó la tarjeta a la computadora y se cargó el programa mediante Arduino IDE.

En el programa se utilizó la librería `BluetoothSerial.h` para establecer la comunicación Bluetooth. El ESP32 se configuró con el nombre `ESP32_LED`, permitiendo identificarlo al momento de buscar dispositivos disponibles.

Para esta parte fue necesario utilizar un teléfono Android. Desde el teléfono se realizó el emparejamiento con el ESP32 y posteriormente se utilizó una aplicación de terminal Bluetooth para enviar los comandos.

Una vez establecida la conexión, se enviaron los comandos `ON` y `OFF`. Al recibir `ON`, el ESP32 colocaba el GPIO 23 en estado HIGH y encendía el LED. Al recibir `OFF`, cambiaba el estado a LOW y el LED se apagaba.

El ESP32 también enviaba un mensaje de regreso al teléfono para confirmar la acción realizada. Si se enviaba un comando diferente, aparecía un mensaje indicando que el comando no era reconocido.

También se utilizó `trim()` para eliminar espacios o caracteres adicionales de los mensajes. Finalmente, se agregó una opción para introducir un retraso de un segundo y observar cómo este afectaba el tiempo de respuesta.

## 4. Código utilizado

```cpp
#include "BluetoothSerial.h"

BluetoothSerial bluetooth;

// Pin utilizado para el LED
#define pinLed 23

// Variable para activar la prueba de retraso
bool pruebaRetraso = false;

void setup() {
  Serial.begin(115200);

  // Iniciar Bluetooth
  bluetooth.begin("ESP32_LED");
  bluetooth.setTimeout(20);

  pinMode(pinLed, OUTPUT);
  digitalWrite(pinLed, LOW);

  Serial.println("Bluetooth listo para conectarse.");
}

void loop() {

  // Revisar si llegó un mensaje por Bluetooth
  if (bluetooth.available()) {

    String comando = bluetooth.readStringUntil('\n');

    // Eliminar espacios y caracteres adicionales
    comando.trim();

    Serial.print("Mensaje recibido: ");
    Serial.println(comando);

    if (comando == "ON") {
      digitalWrite(pinLed, HIGH);
      bluetooth.println("LED encendido");
    }

    else if (comando == "OFF") {
      digitalWrite(pinLed, LOW);
      bluetooth.println("LED apagado");
    }

    else {
      bluetooth.println("Comando incorrecto");
    }
  }

  // Prueba de retraso
  if (pruebaRetraso) {
    delay(1000);
  }
}
```

## 5. Resultados y observaciones

Durante la práctica se logró establecer la comunicación Bluetooth entre el teléfono Android y el ESP32. Después de emparejar ambos dispositivos, fue posible enviar instrucciones desde la aplicación de terminal Bluetooth.

Al enviar `ON`, el LED conectado al GPIO 23 se encendía y al enviar `OFF`, se apagaba. El ESP32 también enviaba una confirmación al teléfono después de realizar cada acción.

Un punto importante de la práctica fue el uso de un dispositivo Android, ya que fue el sistema utilizado para realizar el emparejamiento y trabajar con la aplicación de terminal Bluetooth requerida para la actividad.

Al activar el retraso de un segundo, se pudo observar una respuesta más lenta del sistema, permitiendo identificar de manera sencilla el efecto de la latencia.

## 6. Conclusión

En esta práctica aprendimos a utilizar la comunicación Bluetooth del ESP32 para controlar un LED desde un teléfono Android. Mediante comandos sencillos fue posible encender y apagar el LED conectado al GPIO 23 y recibir una confirmación de cada acción.

También comprendimos la importancia de establecer correctamente la conexión entre el teléfono y el ESP32, además de observar cómo un retraso puede afectar el tiempo de respuesta. Esta práctica permitió conocer de manera sencilla una forma de controlar componentes de manera inalámbrica.
