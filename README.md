# IoT-Final-Project-of-Proshenjit-and-Sanjida
# IoT-based Flood Alert System using ESP32, Ultrasonic Sensor, LEDs, and Blynk.

# Step 1: Components Required
  1.ESP32 – Main microcontroller with Wi-Fi
  2.Ultrasonic Sensor (HC-SR04) – Measures water level
  3.LEDs (Green, Yellow, Red) – Indicates different water levels
  4.Resistors (220Ω or 330Ω) – For LEDs
  5.Power Bank & USB Cable – Power supply for ESP32
  6.Jumper Wires & Breadboard – For connections

# Step 2: Understanding Water Level Alerts
  Water Level	LED Indication	Alert Type
  Low (Safe)	Green LED ON	Normal condition
  Medium (Warning)	Yellow LED ON	Caution alert
  High (Danger/Flood Alert)	Red LED ON & Blynk Notification	Flood warning!

# Step 3: Circuit Connections
  Component	ESP32 Pin
  Trigger (HC-SR04)	GPIO 5
  Echo (HC-SR04)	GPIO 18
  Green LED	GPIO 23
  Yellow LED	GPIO 22
  Red LED	GPIO 21
  Buzzer (Optional)	GPIO 19

# Step 4: Setting Up Blynk
  1.Download Blynk App (Android/iOS)
  2.Create New Project → Choose ESP32
  3.Add a "Gauge" & "Notification" Widget
  4.Copy the "Auth Token" (Needed for the code)

# Step 5: ESP32 Code (Arduino IDE)
  1.Install Required Libraries
  ○Install ESP32 Board Package in Arduino IDE
  ○Install Blynk
  ○Install NewPing (for Ultrasonic sensor)

# 2.Upload this Code:
    #include <WiFi.h>
    #include <BlynkSimpleEsp32.h>
    #include <NewPing.h>
    
    // WiFi & Blynk Credentials
    char auth[] = "YOUR_BLYNK_AUTH_TOKEN";
    char ssid[] = "YOUR_WIFI_SSID";
    char pass[] = "YOUR_WIFI_PASSWORD";
    
    // Ultrasonic Sensor Pins
    #define TRIG_PIN 5
    #define ECHO_PIN 18
    #define MAX_DISTANCE 200 // Max depth in cm
    NewPing sonar(TRIG_PIN, ECHO_PIN, MAX_DISTANCE);
    
    // LED Pins
    #define GREEN_LED 23
    #define YELLOW_LED 22
    #define RED_LED 21
    
    
    void setup() {
      Serial.begin(115200);
      
      // Initialize Wi-Fi & Blynk
      WiFi.begin(ssid, pass);
      Blynk.begin(auth, ssid, pass);
      
      // Set LED and Buzzer as Output
      pinMode(GREEN_LED, OUTPUT);
      pinMode(YELLOW_LED, OUTPUT);
      pinMode(RED_LED, OUTPUT);
    }
    
    void loop() {
      Blynk.run(); // Run Blynk
      
      int distance = sonar.ping_cm(); // Measure Water Level
      int waterLevel = 100 - distance; // Assuming 100cm as full tank
      
      Serial.print("Water Level: ");
      Serial.print(waterLevel);
      Serial.println("%");
    
      Blynk.virtualWrite(V1, waterLevel); // Send data to Blynk
    
      // Alert Conditions
      if (waterLevel < 30) { // Low Level (Safe)
        digitalWrite(GREEN_LED, HIGH);
        digitalWrite(YELLOW_LED, LOW);
        digitalWrite(RED_LED, LOW);
      }
      else if (waterLevel >= 30 && waterLevel < 70) { // Medium Level (Warning)
        digitalWrite(GREEN_LED, LOW);
        digitalWrite(YELLOW_LED, HIGH);
        digitalWrite(RED_LED, LOW);
      }
      else { // High Level (Flood Alert)
        digitalWrite(GREEN_LED, LOW);
        digitalWrite(YELLOW_LED, LOW);
        digitalWrite(RED_LED, HIGH);    
        Blynk.notify("🚨 Flood Alert! Water Level Critical! 🚨");
      }
    
      delay(2000); // Wait before next reading
    }

# Step 6: Powering the Project
  ●Use a Power Bank (5V USB output) to power the ESP32 via micro-USB.
  ●Alternative: Use a 3.7V Li-ion battery with a boost converter (5V output).

# Step 7: Testing & Deployment
  1.Upload the code to ESP32.
  2.Open Blynk App and check if it receives water level updates.
  3.Test LED indicators by adjusting the sensor height.
  4.Pour water to test Blynk notifications.
  5.Mount the sensor properly for real-world use.
  Final Outcome
  ✅ Real-time Water Level Monitoring
  ✅ LED Indicators (Green, Yellow, Red)
  ✅ Blynk App Alerts on your phone 📲
✅ Powered by Power Bank for portability
