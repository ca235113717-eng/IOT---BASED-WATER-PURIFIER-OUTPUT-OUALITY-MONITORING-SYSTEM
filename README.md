#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

#define TDS_PIN 34       // Analog input from TDS sensor
#define BUZZER_PIN 25    // Active buzzer pin

float tdsValue = 0.0;

void setup() {
Serial.begin(115200);

pinMode(BUZZER_PIN, OUTPUT);
digitalWrite(BUZZER_PIN, LOW);

if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
Serial.println(F("SSD1306 allocation failed"));
for(;;);
  }

display.clearDisplay();
display.setTextSize(1);
display.setTextColor(SSD1306_WHITE);
display.setCursor(0,0);
display.println("Water Quality Monitor");
display.display();
delay(2000);
}

void loop() {
  int analogValue = analogRead(TDS_PIN);

  // Convert analog value to TDS (ppm)
tdsValue = analogValue * (3.3 / 4095.0) * 1000; // Example conversion

display.clearDisplay();
display.setCursor(0,0);
display.println("Water Quality Monitor");
display.setCursor(0,20);
display.print("TDS: ");
display.print(tdsValue);
display.println(" ppm");

  // Trigger buzzer if water quality is bad (example threshold)
if(tdsValue> 500) {
digitalWrite(BUZZER_PIN, HIGH);
  } else {
digitalWrite(BUZZER_PIN, LOW);
  }

display.display();
delay(1000);
}

