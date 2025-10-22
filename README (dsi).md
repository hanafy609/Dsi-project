# DSI Project — Digital System Interface

## Overview
This project demonstrates the integration of three sensors with a PIC microcontroller that communicates with a C#-based desktop application via RS232 serial communication.  
The system collects real-time data for:
- **Temperature** (LM35)
- **Humidity** (DHT11)
- **Motion detection** (PIR sensor)

The readings are transmitted to a **C# GUI** that displays the sensor information in a user-friendly interface.

---

## Features
- Real-time data acquisition from multiple sensors  
- Serial communication via RS232  
- GUI built with C# and Windows Forms  
- Sensor data visualization (motion detected / not detected, humidity, and temperature)  
- Microcontroller programmed using **MikroC**

---

## Technologies Used
| Component | Technology |
|------------|-------------|
| Microcontroller | PIC (MikroC) |
| Communication | RS232 |
| Desktop App | C# (.NET, Windows Forms) |
| Sensors | LM35 (Temperature), DHT11 (Humidity), PIR (Motion) |

---

## Microcontroller Code (MikroC)

### Main Loop
```c
void main() {
    UART1_Init(9600);
    Delay_ms(100);
    newline();
    while(1) {
        motion();
        humid();
        temp();
    }
}
```

### Temperature Sensor (LM35)
```c
void temp() {
    Temp_in = adc_read(0);
    Dis_temp = (Temp_in * 500) / 1023;
    IntToStr(Dis_temp, txt_temp);
    Uart1_write_text(txt_temp);
    newline();
}
```

### Humidity Sensor (DHT11)
```c
void humid() {
    DHT11_Start();
    DHT11_CheckResponse();
    rhi = DHT11_ReadData();
    DHT11_ReadData(); DHT11_ReadData(); DHT11_ReadData(); DHT11_ReadData();
    delay_ms(18);
    IntToStr(rhi, txt);
    UART1_Write_Text(txt);
    UART1_Write_Text(" %");
    newline();
}
```

### Motion Sensor (PIR)
```c
void motion() {
    if (PORTB.F0) {
        Uart1_write_text("Detected!");
        newline();
    } else {
        Uart1_write_text("Not detected");
        newline();
    }
}
```

---

## C# Desktop Application
The GUI is built using **Microsoft Visual Studio (Windows Forms)**. It reads incoming serial data and updates three text boxes representing motion, humidity, and temperature.

```csharp
SerialPort port = new SerialPort("COM2", 9600, Parity.None, 8, StopBits.One);

private void DataReceivedHandler(object sender, SerialDataReceivedEventArgs e) {
    port = (SerialPort)sender;
    string motion = port.ReadLine();
    string humid = port.ReadLine();
    string temp = port.ReadLine();

    if (!string.IsNullOrEmpty(motion))
        Invoke(new Action(() => richTextBox1.Text = motion));

    if (!string.IsNullOrEmpty(humid))
        Invoke(new Action(() => richTextBox2.Text = humid));

    if (!string.IsNullOrEmpty(temp))
        Invoke(new Action(() => richTextBox3.Text = temp));
}
```

---

## System Design

### Proteus Simulation
A **Proteus** circuit was used to simulate the microcontroller, sensors, and serial connection before deployment.

### GUI States
- **Before Readings:** Displays empty fields for temperature, humidity, and motion.
- **Motion Detected:** Motion indicator turns active.
- **No Motion Detected:** Indicator remains idle.

---

## Project Team
- **Author:** Mohammed Hanafi  
- **Course:** Digital System Interface (CSE 362 - ECE 441)  
- **Institution:** Faculty of Engineering, October University for Modern Sciences and Arts (MSA)  
- **Date:** May 18, 2023  

---



---
