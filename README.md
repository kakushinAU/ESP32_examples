# ESP32_examples

## REST_API_CLIENT
```shell
#include <WiFi.h>
#include <HTTPClient.h>
#include <ArduinoJson.h>

const char* ssid = "MY_SSID";
const char* password = "MY_PASSWORD";
String response = "";

//JSON document
DynamicJsonDocument doc(2048);

void setup(void) {
  Serial.begin(9600);
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  Serial.println("");

  // Wait for connection
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.print("WiFi connected with IP: ");
  Serial.println(WiFi.localIP());
}

void loop(void) {
  HTTPClient http;
  String request = "https://api.chucknorris.io/jokes/random";
  http.begin(request);
  http.GET();
  response = http.getString();
  DeserializationError error = deserializeJson(doc, response);
  if (error) {
    Serial.print(F("deserializeJson() failed: "));
    Serial.println(error.f_str());
    return;
  }
  Serial.println(doc["value"].as<char*>());

  http.end();
  delay(2000);
}
```

## PIR_Sensor
```shell
#define PIR_OUT (18) /* PIR OUT pin*/

void setup(void)
{
  pinMode(19, OUTPUT);     //use pin 19 to power the sensor
  digitalWrite(19, HIGH);  //use pin 19 to power the sensor
  int i = 10;
  Serial.begin(9600);
  delay(100);
  Serial.println("SimplyTronics PIR Sensor(ST-00031/ST-000081)");
  Serial.println("===========================================");
  Serial.println("Please wait 10 seconds the PIR Sensor to warm up...");
  while (i-- > 0)
  {
    Serial.print(i);
    Serial.println("  seconds left");
    delay(1000);
  }
  Serial.println("Start detect the state of PIR Sensor.");
  pinMode(PIR_OUT, INPUT);
}

int pir_output = 0;
void loop(void)
{
  pir_output = digitalRead(PIR_OUT);
  Serial.print("IN0 = ");
  Serial.println(pir_output, BIN);
  delay(200);
}

```

## WIFI_timeserver
```shell
#include <WiFi.h>
#include <Wire.h> 
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <NTPClient.h>

const char* ssid     = "MY_SSID";
const char* password = "MY_PASSWORD";

#define NTP_OFFSET  18000 // In seconds. +5 = 18000
#define NTP_INTERVAL 60 * 1000 // In miliseconds
#define NTP_ADDRESS  "1.asia.pool.ntp.org"

WiFiUDP ntpUDP;
NTPClient timeClient(ntpUDP, NTP_ADDRESS, NTP_OFFSET, NTP_INTERVAL);

Adafruit_SSD1306 display(128,64,&Wire,4);

void setup()
{
Serial.begin(9600);
Serial.println();
Serial.print("Connecting to ");
Serial.println(ssid);
WiFi.begin(ssid, password);
while (WiFi.status() != WL_CONNECTED)
{
delay(500);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected.");
Serial.println("IP address: ");
Serial.println(WiFi.localIP());

timeClient.begin();
display.begin(SSD1306_SWITCHCAPVCC, 0x3C); 
display.clearDisplay();
display.setTextColor(WHITE);
}

void loop()
{
timeClient.update();
String formattedTime = timeClient.getFormattedTime();
display.clearDisplay();
display.setTextSize(1);
display.setCursor(0, 0);
display.println(formattedTime);
display.display();   // write the buffer to the display
delay(100);
}
```

## SERVO
```shell
#include <ESP32Servo.h>

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;    // variable to store the servo position

void setup() {
  myservo.attach(13);  // attaches the servo on pin 13 to the servo object
}

void loop() {
  myservo.write(35);
  delay(1000);
  myservo.write(90);
  delay(1000);
  myservo.write(135);
  delay(1000);
  myservo.write(90);
  delay(1000);
}
```
# libraries
* https://dl.espressif.com/dl/package_esp32_index.json
* ESP32Servo
* Adafruit_GFX
* Adafruit_SSD1306
* NTPClient
* WiFi
* HTTPClient
* ArduinoJson
* (Silicon Labs CP210x USB to UART Bridge)

