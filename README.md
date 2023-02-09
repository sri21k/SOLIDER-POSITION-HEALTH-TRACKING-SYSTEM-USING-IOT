# SOLIDER-POSITION-HEALTH-TRACKING-SYSTEM-USING-IOT
The soldier Health and Position Tracking System allows military to track the current GPS position of soldier and also checks the health status including body temperature and heartbeats of soldier.
The nation’s security is monitored and kept by army, navy and air-force. The important and vital role is of soldiers who sacrifice their life for their country. There are many concerns regarding the safety of the soldier. Soldiers entering the enemy lines often lose their lives due to lack of connectivity, it is very vital for the army base station to known the location as well as health status of all soldiers. India has already lost so many soldiers in war-fields as there was no proper health backup and connectivity between the soldiers on the war-fields and the officials at the army base stations.
                                                                                                           Soldier’s tracking is done using IOT is used to provide wireless communication system. For monitoring the health parameters of soldier we are using bio medical sensors such as temperature sensor and heart beat sensor. An oxygen level sensor is used to monitor atmospheric oxygen so if there are any climatic changes the soldiers will be equipped accordingly.
So this paper focuses on tracking the location of soldier from location, which is useful for control room station to know the exact location of soldier and accordingly they will guide them and it is for High-speed, short range, soldier-to-soldier wireless communications to relay information on situational awareness with Bio-medical sensors, position navigation.


CODE (I): Code for communication
#include <SPI.h>
#include <Ethernet.h>
#include <BlynkSimpleEthernet.h>

// You should get Auth Token in the Blynk App.
// Go to the Project Settings (nut icon).
char auth[] = "YourAuthToken";

BLYNK_WRITE(V1) {
  GpsParam gps(param);

  // Print 6 decimal places for Lat, Lon
  Serial. Print ("Lat: ");
  Serial.println(gps.getLat(), 7);

  Serial.print("Lon: ");
  Serial.println(gps.getLon(), 7);

  // Print 2 decimal places for Alt, Speed
  Serial.print("Altitute: ");
  Serial.println(gps.getAltitude(), 2);

  Serial.print("Speed: ");
  Serial.println(gps.getSpeed(), 2);

  Serial.println();
}

void setup()
{
  // Debug console
  Serial. begin (9600);

  Blynk.begin(auth);
}

void loop()
{
#define BLYNK_PRINT Serial    // Comment this out to disable prints and save space
#include <SPI.h>
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <SimpleTimer.h>
#include <DHT.h>


char auth[] = "TB6KiXEXl----------qFo3Bp"; //you can add yout auth token 
char ssid[] = "sneha123";     // your Wifi name
char pass[] = "asdfghjkl";    // your wifi password









CODE (II):  Code for the tracking the health Parameters.
#define DHTPIN D1          // Digital pin D1
float moisture; 
#define DHTTYPE DHT11     // DHT 11
int temp, Humid;
DHT dht(DHTPIN, DHTTYPE);
SimpleTimer timer;

WidgetTerminal terminal(V1);

BLYNK_WRITE (V1)
{
    terminal.write (param.getBuffer(), param.getLength());
    terminal.println();

  // Ensure everything is sent
  terminal.flush();
}

void sendSensor()
{
  float h = dht.readHumidity();
  float t = dht.readTemperature(); // or dht.readTemperature(true) for Fahrenheit

  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }
  // You can send any value at any time.
  // Please don't send more that 10 values per second.
  Blynk.virtualWrite (V5, h);  //V5 is for Humidity
  Blynk.virtualWrite (V6, t);  //V6 is for Temperature

}

void setup()
{
  Serial.begin(9600); // See the connection status in Serial Monitor
  Blynk.begin(auth, ssid, pass);

  dht.begin();
  timer.setInterval (1000L, sendSensor);
  terminal.flush ();
}

void loop()
{  
    temp = dht.readTemperature(); // or dht.readTemperature(true) for Fahrenheit
  Humid = dht.readHumidity();

  Serial.print("temp: ");
  Serial.print(temp);
  Serial.print(" c");  
  terminal.print("temp: ");
  terminal.print(temp);
  terminal.print(" c");
  Serial.print("    Humidity: ");
  Serial.print(Humid);
  Serial.println(" %");  
  terminal.print("  Humidity: ");
  terminal.print(Humid);
  terminal.println(" %");
  delay(300);

    

   terminal.flush();
     
  Blynk.run(); // Initiates Blynk
  timer.run(); // Initiates Simple Timer
}	
