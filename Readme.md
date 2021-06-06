#Practica 5.2: Texto en movimiento en patalla OLED con Arduino

##Codigo

```
#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
 
// Definir constantes
#define ANCHO_PANTALLA 128 // ancho pantalla OLED
#define ALTO_PANTALLA 64 // alto pantalla OLED
 
// Objeto de la clase Adafruit_SSD1306
Adafruit_SSD1306 display(ANCHO_PANTALLA, ALTO_PANTALLA, &Wire, -1);
 
void setup() {
#ifdef __DEBUG__
  Serial.begin(9600);
  delay(100);
  Serial.println("Iniciando pantalla OLED");
#endif
 
  // Iniciar pantalla OLED en la dirección 0x3C
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
#ifdef __DEBUG__
    Serial.println("No se encuentra la pantalla OLED");
#endif
    while (true);
  }
 
  // Limpir buffer
  display.clearDisplay();
 
  // Tamaño del texto
  display.setTextSize(1);
  // Color del texto
  display.setTextColor(SSD1306_WHITE);
  // Posición del texto
  display.setCursor(10, 32);
  //Activar página de código 437
  display.cp437(true);
  // Escribir el carácter ¡ dos veces
  display.write(173);
  display.write(173);
  // Escribir texto
  display.println("Hola mundo!!");
  
  // Enviar a pantalla
  display.display();
  delay(2000);
 
  // Mover texto de izquierda a derecha
  display.startscrollright(0x00, 0x0F);
  delay(5000);
  display.stopscroll();
  
  // Mover texto de derecha a izquierda
  display.startscrollleft(0x00, 0x0F);
  delay(5000);
  display.stopscroll();
 
  // Mover texto en diagonal hacia la derecha
  display.startscrolldiagright(0x00, 0x07);
  delay(5000);
  display.stopscroll();
 
  // Mover texto en diagonal hacia la izquierda
  display.startscrolldiagleft(0x00, 0x07);
  delay(5000);
  display.stopscroll();
}
 
void loop() {}
````

##Funcionamiento

Con este programa crearemos un texto en movimiento que aparecera por nuestra pantalla oled. 

Primero de todo definiremos las constantes que en este caso serian:
 #define ANCHO_PANTALLA 128 // ancho pantalla OLED
 #define ALTO_PANTALLA 64 // alto pantalla OLED

Seguidamente Creamos el objeto de la clase Adafruit_SSD1306:

`Adafruit_SSD1306 display(ANCHO_PANTALLA, ALTO_PANTALLA, &Wire, -1);`

Dentro del voidsetup() iniciamos la pantalla OLED:
```
void setup() {
#ifdef __DEBUG__
  Serial.begin(9600);
  delay(100);
  Serial.println("Iniciando pantalla OLED");
#endif
```
Despues iniciamos la pantalla OLED en la direccion 0x3C:
```
if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
#ifdef __DEBUG__
    Serial.println("No se encuentra la pantalla OLED");
#endif
    while (true);
  }
  ```
  Una vez iniciada la pantalla ajustamos el tamaño del texto:
  ```
    // Tamaño del texto
  display.setTextSize(1);
  // Color del texto
  display.setTextColor(SSD1306_WHITE);
  // Posición del texto
  display.setCursor(10, 32);
  //Activar página de código 437
  display.cp437(true);
  // Escribir el carácter ¡ dos veces
  display.write(173);
  display.write(173);
  // Escribir texto
  display.println("Hola mundo!!");
  ```
  Una vez establecido el tamaño, lo enviamos a la pantalla:
  ```
   // Enviar a pantalla
  display.display();
  delay(2000);
  ```


En estre caso existen 4 movimientos del texto:
* Movimiento de izquierda a derecha.
* Movimiento de derecha a izquierda.
* Movimiento diagonal hacia la izquierda.

Primero empezaremos con el movimiento de izquierda a derecha. Este movimiento mueve un texto de izquierda a derecha. Cuando sale por la derecha aparece de nuevo por la izquierda. Para este movimiento utilizamos esta función:

`display.startscrollright(inicio, fin);`

Donde el:

* inicio: es la primera fila que se moverá.
* fin: es la última fila que se moverá.
```
  // Mover texto de izquierda a derecha
  display.startscrollright(0x00, 0x0F);
  delay(5000);
  display.stopscroll();
  ```

Para mover todas las filas de la pantalla inicio tiene que ser igual a 0x00 y fin tiene que ser igual a 0x0F (son números en hexadecimal).

Para mover de derecha a izquierda funciona exactamente igual pero se utiliza la siguiente función:

`display.startscrollleft(inicio, fin);`

Donde el:

* inicio: es la primera fila que se moverá.
* fin: es la última fila que se moverá.

O puedes mover de derecha a izquierda:

`display.startscrollleft(inicio, fin);`

Donde el:

* inicio: es la primera fila que se moverá.
* fin: es la última fila que se moverá.
```
// Mover texto de derecha a izquierda
  display.startscrollleft(0x00, 0x0F);
  delay(5000);
  display.stopscroll();
```


Para mover todas las filas en diagonal tienes que pasar como parámetro inicio 0x00 y como fin 0x07.
```
 // Mover texto en diagonal hacia la derecha
  display.startscrolldiagright(0x00, 0x07);
  delay(5000);
  display.stopscroll()
```
 
```
   // Mover texto en diagonal hacia la izquierda
  display.startscrolldiagleft(0x00, 0x07);
  delay(5000);
  display.stopscroll();
````



