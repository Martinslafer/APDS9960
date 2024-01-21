# APDS9960
El sensor puede detectar la información de la velocidad y dirección que lo convierte en una información digital, además, el rango de detección varia entre 10 a 20 centímetros. 
- Comunicación I2C
- 2.4v hasta 3.6v
- Consumo 0.2Ma

## PinOut
<img src="https://github.com/Martinslafer/APDS9960/assets/156008489/ce966da4-c48c-4347-8a06-91eea1e6275e" width="500" />

### Arduino Uno
<img src="https://github.com/Martinslafer/APDS9960/assets/156008489/a11efed6-baa2-4c94-b29b-d8dbbcd8ee5b" width="500" />

| Arduino Pin | APDS-9960 Board | Function   |
|-------------|-----------------|------------|
| None        | VL              | None       |
| GND         | GND             | Ground     |
| 3v3         | VCC             | Power      |
| A4         | SDA             | I2C Data   |
| A5         | SCL             | I2C Clock  |
| 2      | INT             | Interrupt  |

### Arduino Nano
<img src="https://github.com/Martinslafer/APDS9960/assets/156008489/b2d50894-2176-4764-a5fb-7fc265244d3f" width="500" />

| Arduino Pin | APDS-9960 Board | Function   |
|-------------|-----------------|------------|
| None        | VL              | None       |
| GND         | GND             | Ground     |
| 3v3         | VCC             | Power      |
| D18         | SDA             | I2C Data   |
| D19         | SCL             | I2C Clock  |
| D2 PA0      | INT             | Interrupt  |

## Librerias

### SparkFun:
- **Name:** SparkFun APDS9960 RGB and Gesture Sensor
- **Version:** 1.4.3
- **Author:** SparkFun Electronics <techsupport@sparkfun.com>
- **Maintainer:** SparkFun Electronics <sparkfun.com>
- **Sentence:** Library for the Avago APDS-9960 sensor
- **Paragraph:** This library works with the SparkFun Breakout board for the Avago APDS-9960 proximity, light, RGB, and gesture sensor, made by SparkFun Electronics.
- **Category:** Sensors
- **Url:** https://github.com/sparkfun/SparkFun_APDS-9960_Sensor_Arduino_Library
- **Architectures:**

### Adafruit:
- **Name:** Adafruit APDS9960 Library
- **Version:** 1.2.5
- **Author:** Adafruit
- **Maintainer:** Adafruit <info@adafruit.com>
- **Sentence:** This is a library for the Adafruit APDS9960 gesture/proximity/color/light sensor.
- **Paragraph:** This is a library for the Adafruit APDS9960 gesture/proximity/color/light sensor.
- **Category:** Sensors
- **Url:** https://github.com/adafruit/Adafruit_APDS9960
- **Architectures:** *
- **Depends:** Adafruit BusIO

  ### Apreciaciones:
  - ***N1***: Depende mucho de la iluminación del ambiente los valores del sensor, cuando no detecta ningún color detecta negro de por defecto, valores demasiados bajos rondado entre los 20-80 en los tres valores RGB.
  - ***N2***: Además también depende de la proximidad del color del objeto mientrás mas cerca este mejor sera la obtención de los valores RGB.
  - ***N3***: Tuve algunos problemas con la libreria Adafruit, la cual nunca me pudo funcionar correctamente entre los envios de datos. Descarte primero los tipos de driver para el USB y los cables que parecian y estaban en mal estado. Porque algunos se rompieron de tanto uso en un par de horas, por lo que tuve que cambiarlos. A pesar de haberlo revisado completamente en la SerialTerminal me enviaba datos incompletos o se quedaba pausado en el envio de datos RX En conclusión esta libreria no esta funcionando correctamente con este Arduino no Oficial. Por ultimo intente desarmar el codigo para encontrar el problema, este no se congelaba solo cuando mandaba un solo valor (R) pero una vez que queria actualizar se congelaba de vuelta. 
 
    ```
    Codigo:
    #include "Adafruit_APDS9960.h"
    Adafruit_APDS9960 apds;
    
    void setup() {
      Serial.begin(115200);
    
      if(!apds.begin()){
        Serial.println("failed to initialize device! Please check your wiring.");
      }
      else Serial.println("Device initialized!");
    
      //enable color sensign mode
      apds.enableColor(true);
    }
    
    void loop() {
      //create some variables to store the color data in
      uint16_t r, g, b, c;
      
      //wait for color data to be ready
      while(!apds.colorDataReady()){
        delay(5);
      }
    
      //get the data and print the different channels
      apds.getColorData(&r, &g, &b, &c);
      Serial.print("red: ");
      Serial.print(r);
      
      Serial.print(" green: ");
      Serial.print(g);
      
      Serial.print(" blue: ");
      Serial.print(b);
      
      Serial.print(" clear: ");
      Serial.println(c);
      Serial.println();
      
      delay(500);
    }

    
    Serial Terminal:
    r: Value
    g: Value
    b: Value

    r: Value
    **Se congela**

    
    ```
***N4***: Al ya saber todo esto decidi utilizar otra libreria llamada SparkFun que ademas de obtener datos RGB podia obtener la luz ambiente de su alrededor, esta no se congelaba y envia los datos correctamente, incluso hice unos pequeños condicionales para que hagan unos print a la hora detectar los colores rojo y verde. 

**Conclusión**: El sensor requiere esta por lo menos 20cm de distancia al objeto para que tenga una mejor lectura de los datos RGB, depende mucho de la iluminación ambiente, pero con la libreria SparkFun no sufre congelamientos y funciona correctamente. 


