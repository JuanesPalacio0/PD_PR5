# Monitor de Temperatura y Humedad con ESP32, Sensor AHT10/AHT20 y Pantalla OLED

Este programa utiliza un ESP32 para medir la temperatura y la humedad utilizando un sensor AHT10 o AHT20 y muestra los datos en una pantalla OLED. A continuación se presenta el código y una explicación detallada de su funcionamiento.

## Código

```cpp
#include <Adafruit_AHTX0.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <Wire.h>

#define SCREEN_WIDTH 128 // Ancho de la pantalla OLED, en píxeles
#define SCREEN_HEIGHT 32 // Alto de la pantalla OLED, en píxeles

#define OLED_RESET -1 // Pin de reset (o -1 si se comparte el pin de reset del Arduino)
#define SCREEN_ADDRESS 0x3C // Dirección de la pantalla OLED; 0x3D para 128x64, 0x3C para 128x32

Adafruit_AHTX0 aht;
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

void setup() {
    Wire.begin();
    Serial.begin(115200);
    Serial.println("Adafruit AHT10/AHT20 demo!");

    if (!aht.begin()) {
        Serial.println("¡No se pudo encontrar el AHT! ¡Verifique la conexión!");
        while (1) delay(10);
    }
    Serial.println("AHT10 o AHT20 encontrado");

    if (!display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS)) {
        Serial.println(F("¡Error al asignar memoria para SSD1306!"));
        for (;;);
    }

    display.display();
    delay(2000);
    display.clearDisplay();
}

void loop() {
    sensors_event_t humidity, temp;
    aht.getEvent(&humidity, &temp); // Obtener los objetos de temperatura y humedad con datos actualizados

    display.clearDisplay();

    display.setTextSize(1);
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(0, 0);
    display.print("Temperatura: ");
    display.println(temp.temperature);
    display.print("Humedad: ");
    display.println(humidity.relative_humidity);

    display.display();
    Serial.print("Temperatura: ");
    Serial.print(temp.temperature);
    Serial.println(" grados C");
    Serial.print("Humedad: ");
    Serial.print(humidity.relative_humidity);
    Serial.println("% HR");

    delay(500);
}

### Objetivo
El objetivo de este programa es medir y mostrar la temperatura y la humedad ambiente utilizando un sensor AHT10/AHT20 y una pantalla OLED conectada a un ESP32.

 ## Funcionamiento

1. Configuración del Hardware:

- Se incluye la biblioteca Adafruit_AHTX0 para manejar el sensor AHT10/AHT20.
- Se incluyen las bibliotecas Adafruit_GFX y Adafruit_SSD1306 para manejar la pantalla OLED.
- Se definen las dimensiones de la pantalla OLED y su dirección I2C.

2. Inicialización en setup():

- Se inicializa el bus I2C con Wire.begin().
- Se inicia la comunicación serial para el monitoreo a través del puerto serie.
- Se inicializa el sensor AHT10/AHT20 y se verifica su conexión. Si no se encuentra el sensor, el programa se detiene.
- Se inicializa la pantalla OLED y se verifica su correcto funcionamiento. Si hay un error, el programa se detiene.
- Se muestra una pantalla de bienvenida durante 2 segundos y luego se limpia la pantalla.

3. Medición y Visualización en loop():

- Se obtienen los datos de temperatura y humedad del sensor.
- Se limpia la pantalla OLED.
- Se configuran los parámetros de texto y se muestran los valores de temperatura y humedad en la pantalla.
- Se envían los mismos datos al monitor serie para su visualización.
- Se espera medio segundo antes de realizar una nueva medición.


***Explicación de las Funciones Clave***
- aht.begin(): Inicializa el sensor AHT10/AHT20.
- display.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS): Inicializa la pantalla OLED.
- aht.getEvent(&humidity, &temp): Obtiene las lecturas de temperatura y humedad del sensor.
- display.clearDisplay(): Limpia la pantalla OLED.
- display.setTextSize(1): Configura el tamaño del texto.
- display.setTextColor(SSD1306_WHITE): Configura el color del texto.
- display.setCursor(0, 0): Establece la posición del cursor en la pantalla.
- display.print() y display.println(): Muestran texto en la pantalla OLED.
- display.display(): Actualiza la pantalla con el contenido actual.