# Hackathon
Our project is Air Watchtower, an IoT-based environmental monitoring system. The objective of this project is to continuously monitor environmental conditions such as:  Temperature Humidity Air Quality Noise Level  The collected data is displayed on an LCD screen and also sent to the Serial Monitor for real-time observation."
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>

#define DHT_PIN 4
#define MQ135_PIN 34
#define SOUND_PIN 35
#define DHT_TYPE DHT11

DHT dht(DHT_PIN, DHT_TYPE);
LiquidCrystal_I2C lcd(0x27, 16, 2);

// Read MQ135 several times to reduce fluctuations
int readAirQuality() {
  long sum = 0;

  for (int i = 0; i < 20; i++) {
    sum += analogRead(MQ135_PIN);
    delay(5);
  }

  return sum / 20;
}

// Measure sound by finding signal variation
int readSoundLevel() {
  int minValue = 4095;
  int maxValue = 0;

  unsigned long startTime = millis();

  while (millis() - startTime < 100) {
    int sensorValue = analogRead(SOUND_PIN);

    if (sensorValue < minValue)
      minValue = sensorValue;

    if (sensorValue > maxValue)
      maxValue = sensorValue;
  }

  return maxValue - minValue;
}

void setup() {

  Serial.begin(115200);

  dht.begin();

  pinMode(MQ135_PIN, INPUT);
  pinMode(SOUND_PIN, INPUT);

  Wire.begin(21, 22);

  lcd.init();
  lcd.backlight();

  lcd.print("Air Watchtower");
  lcd.setCursor(0, 1);
  lcd.print("Starting...");

  Serial.println("System Starting...");
  delay(5000);   // MQ135 warm-up

  lcd.clear();
}

void loop() {

  float temp = dht.readTemperature();
  float hum = dht.readHumidity();

  if (isnan(temp) || isnan(hum)) {

    Serial.println("Sensor Error");

    lcd.clear();
    lcd.print("DHT Error");
    lcd.setCursor(0, 1);
    lcd.print("Check Sensor");

    delay(2000);
    return;
  }

  int air = readAirQuality();
  int noise = readSoundLevel();

  Serial.println("----------------------");
  Serial.print("Temperature : ");
  Serial.print(temp);
  Serial.println(" C");

  Serial.print("Humidity    : ");
  Serial.print(hum);
  Serial.println(" %");

  Serial.print("Air Value   : ");
  Serial.println(air);

  Serial.print("Noise Value : ");
  Serial.println(noise);

  if (air < 1200)
    Serial.println("Air : GOOD");
  else if (air < 2200)
    Serial.println("Air : MODERATE");
  else
    Serial.println("Air : POOR");

  lcd.clear();

  lcd.setCursor(0, 0);
  lcd.print("T:");
  lcd.print(temp, 1);
  lcd.print(" H:");
  lcd.print(hum, 0);

  lcd.setCursor(0, 1);
  lcd.print("AQ:");
  lcd.print(air);

  lcd.print(" N:");
  lcd.print(noise);

  delay(2000);
}
