# Wireless control of a simulated robotic system 🤖📡

Final project of the **Higher Technician programme in Industrial Automation and Robotics** (CFGS), completed at **IES Politécnico Hermenegildo Lanz**, Granada, Spain.

## Why this exists

The visible goal is to move a simulated ABB robotic arm with a **Wii Nunchuk**: joystick, two buttons, a gyroscope and an accelerometer driving each of the six axes, with an HMI showing the state of the simulation and of the communications.

The real subject of the project is the problem underneath. There is a wide variety of devices from different manufacturers, each implementing its own strategies and resolving problems its own way, so integrating them into a single control chain is rarely straightforward. The approach taken here is to draw on what each device is able to contribute on its own and use it to make up for what the others lack, so that all of them take part in the final task.

Hence six systems, **seven links, and a communication protocol written from scratch** to close the one gap the standard protocols did not cover.

## The chain

```
Nunchuk ──I²C──▶ ESP32 #1 ──ESP-NOW──▶ ESP32 #2 ──J2WC──▶ Arduino Nano
                                                              │
                                                             SPI
                                                              ▼
                                                        ENC28J60
                                                              │
                                                        Modbus TCP
                                                              ▼
              RobotStudio ◀──OPC── WinCC RT Pro ◀──S7── PLC (TIA Portal)
```

The Arduino also drives a 16x2 LCD and several LEDs showing link status, latency and cycle time. In TIA Portal a single function block is instantiated six times, one per axis, and handles Modbus errors. RobotStudio's virtual controller returns the tool position and a heartbeat counter used to measure the latency of the whole chain.

**J2WC** (Juan 2 Wire Communication) is the custom protocol, written for this project to carry data reliably between two of the microcontrollers.

## Technologies

**Hardware** — 2x ESP32, Arduino UNO, Wii Nunchuk, 16x2 I²C LCD, ENC28J60 Ethernet module, LiPo battery + booster, LEDs and protoboards.

**Software** — [RobotStudio (ABB)](https://new.abb.com/products/robotics/es/robotstudio), [TIA Portal V16 (Siemens)](https://new.siemens.com/global/en/products/automation/industry-software/automation-software/tia-portal.html), PLCSIM Advanced, WinCC RT Advanced, Arduino IDE + PlatformIO, ABB IRC5 OPC Server.

**Protocols** — I²C, ESP-NOW, J2WC (custom), SPI, Modbus TCP/IP, S7, OPC.

## Repository contents

```
Arduino-ESP32.zip        firmware for both ESP32 boards and the Arduino
TIA Portal.zip           PLC program and WinCC HMI project
Robot Studio.zip         robot station and RAPID program
Estevez_TFC_2022.pdf     full written report (Spanish)
```

Components were chosen to be within anyone's reach, both for their low cost and for how easily they can be implemented.

## Demo

👉 [Watch the demo video](https://youtu.be/hsgEAXDb1Do)

## Full report

📥 [Estevez_TFC_2022.pdf](https://github.com/00Juan/robotstudio-wireless-control/blob/main/Estevez_TFC_2022.pdf) — includes the J2WC frame structure, the wiring diagrams and the budget.

## Acknowledgements

IES Politécnico Hermenegildo Lanz (Granada), and my tutors and professors from the Electricity Department.

## License

MIT. Use it, modify it and adapt it for learning or development. Credit appreciated.

## Contact

Juan Estévez Delgado
📧 juanestevezus@gmail.com
🌐 [00juan.dev](https://00juan.dev) · [LinkedIn](https://www.linkedin.com/in/juanestevezdelgado/)
