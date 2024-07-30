# forest-fire-detection-and-alert-system-using-lora
Receiver information
#include <SPI.h>
#include <LoRa.h>

int val = 0;
int nss = 10;
int dio0 = 2;
String full_string;
String inString = "";
int rssi;

void setup()
{
  LoRa.setPins(nss,dio0);
  Serial.begin(9600); 
  while (!Serial);
  Serial.println("LoRa Receiver");
  if (!LoRa.begin(868E6)) {
    Serial.println("Starting LoRa failed!");
    while (1);
  }
  LoRa.setSpreadingFactor(12);
  LoRa.setSignalBandwidth(62.5E3);
}

void loop() {
  // try to parse packet
  int packetSize = LoRa.parsePacket();
  full_string = "";

  if (packetSize)
  {
    // received a packet
    Serial.println("");
    Serial.println("Received packet: ");

    // read packet
    while (LoRa.available()) {
      char incoming = (char)LoRa.read();
      full_string += incoming;
      if (incoming == 'F') {
        digitalWrite(LED_BUILTIN, HIGH);  
        delay(2000);
        digitalWrite(LED_BUILTIN, LOW);// Set the built-in LED high
        Serial.println("Flame detected");
      } else { 
        digitalWrite(LED_BUILTIN, LOW);
      } 
      rssi = LoRa.packetRssi();
    }
    Serial.print("  RSSI ");
    Serial.print(rssi);
  }
}
