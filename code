#include <SPI.h>
#include <Ethernet.h>
int a,b;
byte mac[] = { 0x00, 0xAA, 0xBB, 0xCC, 0xDE, 0x19 };   
char DEVID1[] = "v79D475D829DBCE2 ";  
char DEVID2[] = "v93F7AAD05B50254 "; 
uint8_t pinDevid1 = 3;  
uint8_t pinDevid2 = 7;
boolean DEBUG = true;
char serverName[] = "api.pushingbox.com";
boolean pinDevid1State = false;
boolean pinDevid2State = false;
boolean lastConnected = false;               
EthernetClient client;
void setup() {
Serial.begin(9600);
pinMode(pinDevid1, INPUT);
pinMode(pinDevid2,INPUT);
if (Ethernet.begin(mac) == 0) {
Serial.println("Failed to configure Ethernet using DHCP");  
while(true);}
else{
Serial.println("Ethernet ready");
Serial.print("My IP address: ");
Serial.println(Ethernet.localIP(); }
delay(1000);}
void loop()
{       a = digitalRead(pinDevid1);
       b = digitalRead(pinDevid2);
       Serial.println("Value on doorbell:");
       Serial.println(a);
       Serial.println("Value on motion sesnor:");
       Serial.println(b);
       delay(3000);
       if (digitalRead(pinDevid1) == HIGH && pinDevid1State == false)      {
       if(DEBUG){Serial.println("pinDevid1 is HIGH");}
       pinDevid1State = true;
       sendToPushingBox(DEVID1);}
       if (digitalRead(pinDevid1) == LOW && pinDevid1State == true)      {
       if(DEBUG){Serial.println("pinDevid1 is LOW");}
       pinDevid1State = false; }
       if (b == 1 )  {
       if(DEBUG){Serial.println("pinDevid2 is HIGH");}
       pinDevid2State = true;
       digitalWrite(13,HIGH);
       delay(1000);
       digitalWrite(13,LOW);
       delay(5000);
       sendToPushingBox(DEVID2); }
       if (b == 0 && a == 0){
       if(DEBUG){Serial.println("pinDevid2 is LOW");}
       delay(2000); }
       if (client.available()) {
       char c = client.read();
     if(DEBUG){Serial.print(c);}
     }
     if (!client.connected() && lastConnected) {
     if(DEBUG){Serial.println();}
     if(DEBUG){Serial.println("disconnecting.");}
     client.stop();}
     lastConnected = client.connected();}
  void sendToPushingBox(char devid[]){
   client.stop();
   if(DEBUG){Serial.println("connecting...");}
   if (client.connect(serverName, 80)) {
   if(DEBUG){Serial.println("connected");}
   if(DEBUG){Serial.println("sendind request");}
   client.print("GET /pushingbox?devid=");
   client.print(devid);
   client.println(" HTTP/1.1");
   client.print("Host: ");
   client.println(serverName);
   client.println("User-Agent: Arduino");
   client.println();} 
  else {
   if(DEBUG){Serial.println("connection failed");
        }
}
}
