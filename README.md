# PRACTICA 5 :  Buses de comunicación I (introducción y I2c)
Alumna: **Africa Abad**
## Ejercicio de subida de nota  ( muy valorado) 

* Parte  1.- Realizar utilizando el display y el sensor anterior un dispositivo que muestre en display la frecuencia cardiaca  y el contenido de oxigeno .
![](im1.png)

![](im2.jpg)

## Explicación del código

1. Inicialización:
- Se configuran las comunicaciones y se inicializan el sensor y la pantalla OLED.
- Se muestra un mensaje de inicio en la pantalla. 

2. Bucle principal (`loop()`): 
- Se obtiene la lectura del sensor de pulso.

        long irValue = particleSensor.getIR();

- Se verifica si hay un latido cardíaco.

        if (checkForBeat(irValue) == true) {

- Se calcula la frecuencia cardíaca y se actualiza el promedio de las frecuencias cardíacas.

        long delta = millis() - lastBeat;
        lastBeat = millis();

        beatsPerMinute = 60 / (delta / 1000.0);

        if (beatsPerMinute < 255 && beatsPerMinute > 20) {
        rates[rateSpot++] = (byte)beatsPerMinute;
        rateSpot %= RATE_SIZE;

        beatAvg = 0;
        for (byte x = 0; x < RATE_SIZE; x++)
            beatAvg += rates[x];
        beatAvg /= RATE_SIZE;
        }

- Se muestra la frecuencia cardíaca y el valor del sensor IR en la pantalla OLED.


        display.clearDisplay();
        display.setTextSize(1);
        display.setTextColor(SSD1306_WHITE);
        display.setCursor(0, 0);
        display.print("Heart Rate: ");
        display.println(beatAvg);
        display.print("IR: ");
        display.println(irValue);
        display.display();

- Se espera un segundo antes de la próxima lectura.

        delay(1000); // Update every second

