# ESP32 Smart Home
A Wi-Fi enabled smart home monitoring and control system built on the ESP32. It hosts a web dashboard directly from the microcontroller — no cloud, no app, no external server required. Connect to the ESP32's access point from any browser and get live sensor readings, motion alerts, gas detection, and light control across three rooms.

Features

Live sensor dashboard — temperature, humidity, ultrasonic distance, and gas levels updated every 2 seconds
3-room coverage — Living Room, Bedroom, and Kitchen each have independent sensors and alert controls
Safety alerts — motion/proximity detection, high temperature warning, and kitchen gas leak detection
Hardware buzzer — physical alarm fires when any armed room exceeds a threshold
Web Audio siren — browser plays a two-tone siren alongside the visual alarm flash
Light control — toggle all three room LEDs simultaneously from the dashboard
Per-room alert arming — arm or disarm alerts for each room independently via toggle switches
Offline demo mode — the dashboard simulates live data when not connected to the ESP32, useful for testing
No dependencies — the entire frontend (HTML/CSS/JS) is served from flash memory (PROGMEM)


Hardware
Components
ComponentModelQtyPurposeMicrocontrollerESP32 (any 38-pin variant)1Brain + Wi-Fi AP + web serverTemp/Humidity sensorDHT223One per roomUltrasonic distance sensorHC-SR043Motion / proximity detectionGas sensorMQ-21Kitchen gas / smoke detectionWhite LED5 mm, any3Room indicator lightsBuzzerActive buzzer, 5 V1Audible alarmResistor10 kΩ3DHT22 pull-up (one per sensor)Resistor220 Ω3LED current limiting (one per LED)
Pin Map
GPIODirectionConnected to4InputDHT22 — Living Room5InputDHT22 — Bedroom18InputDHT22 — Kitchen12OutputHC-SR04 TRIG — Living Room14InputHC-SR04 ECHO — Living Room27OutputHC-SR04 TRIG — Bedroom35Input (only)HC-SR04 ECHO — Bedroom25OutputHC-SR04 TRIG — Kitchen33InputHC-SR04 ECHO — Kitchen34Input (only)MQ-2 analog out — Kitchen19OutputWhite LED — Living Room23OutputWhite LED — Bedroom15OutputWhite LED — Kitchen26OutputActive Buzzer

⚠️ GPIO 34 and 35 are input-only pins — no internal pull-up/pull-down resistors available.
⚠️ HC-SR04 Echo outputs 5 V logic. Use a voltage divider or level shifter before connecting to ESP32 input pins.
⚠️ MQ-2 heater draws ~150 mA — power from the 5 V rail, not from the ESP32's 3.3 V pin.
⚠️ DHT22 sensors require a 10 kΩ pull-up resistor between the data pin and 3.3 V.


Software
Requirements

Arduino IDE 2.x or later
ESP32 board package by Espressif — install via Boards Manager
Libraries (install via Library Manager):

DHT sensor library by Adafruit
Adafruit Unified Sensor (dependency of DHT library)
