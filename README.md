# Sensores
#include "DHT.h"

#define PIN_DHT 2            // Pin del DHT11
#define DHTTYPE DHT11
#define PIN_ANALOGICO A0     // Sensor analógico
#define PIN_INCLINACION 3    // Sensor SW-520D

DHT dht(PIN_DHT, DHTTYPE);

void setup() {
  Serial.begin(9600);
  dht.begin();
  pinMode(PIN_INCLINACION, INPUT);

  // Encabezados para exportación de datos
  Serial.println("LABEL,hora,luminocidad,temperatura,humedad,inclinacion");
}

void loop() {
  delay(2000);  // Tiempo entre mediciones

  int luminocidad = analogRead(PIN_ANALOGICO);
  float temperatura = dht.readTemperature();
  float humedad = dht.readHumidity();
  int inclinacion = digitalRead(PIN_INCLINACION); // 1 = no inclinado, 0 = inclinado

  if (isnan(temperatura) || isnan(humedad)) {
    Serial.println("ERROR,TIME,NaN,NaN,NaN,NaN");
    return;
  }

  // Salida formateada
  Serial.print("DATA,TIME,");
  Serial.print(luminocidad);
  Serial.print(",");
  Serial.print(temperatura);
  Serial.print(",");
  Serial.print(humedad);
  Serial.print(",");
  Serial.println(inclinacion);
}
