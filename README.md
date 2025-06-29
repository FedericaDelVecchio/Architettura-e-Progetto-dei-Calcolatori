# STM32F3 Environmental Monitoring System

A comprehensive environmental monitoring system built on STM32F303 microcontroller for real-time temperature and humidity sensing with visual and audio feedback capabilities.

## 📋 Overview

This project implements an embedded environmental monitoring system using the STM32F303VCT6 microcontroller. The system continuously measures temperature and humidity using a DHT11 sensor, provides visual feedback through an LED array, audio alerts via a buzzer, and transmits data over USB CDC (Communication Device Class) for real-time monitoring.

## 🗂️ Project Structure

```
Progetto_APC3/
├── Core/
│   ├── Inc/
│   │   ├── main.h                     # Main application header
│   │   ├── stm32f3xx_hal_conf.h      # HAL configuration
│   │   └── stm32f3xx_it.h            # Interrupt handlers header
│   ├── Src/
│   │   ├── main.c                     # Main application code
│   │   ├── stm32f3xx_hal_msp.c       # MSP initialization
│   │   ├── stm32f3xx_it.c            # Interrupt handlers
│   │   ├── syscalls.c                # System calls implementation
│   │   ├── sysmem.c                  # Memory management
│   │   └── system_stm32f3xx.c        # System initialization
│   └── Startup/
│       └── startup_stm32f303vctx.s   # Startup assembly code
├── USB_DEVICE/
│   ├── App/
│   │   ├── usb_device.c              # USB device configuration
│   │   ├── usbd_cdc_if.c             # CDC interface implementation
│   │   └── usbd_desc.c               # USB descriptors
│   └── Target/
│       └── usbd_conf.c               # USB device configuration
├── Drivers/                          # STM32 HAL drivers and libraries
├── Middlewares/                      # USB device library
├── Debug/                           # Compiled binaries and object files
├── Progetto_APC3.ioc               # STM32CubeMX configuration file
└── STM32F303VCTX_FLASH.ld          # Linker script
```

## 🎯 Key Features

### Hardware Components
- **Microcontroller**: STM32F303VCT6 (ARM Cortex-M4F, 72MHz)
- **Sensor**: DHT11 temperature and humidity sensor
- **Visual Output**: 8-LED array for temperature indication
- **Audio Output**: PWM-controlled buzzer for alarm notifications
- **Communication**: USB CDC for data transmission

### Core Functionality
- **Real-time Monitoring**: Continuous temperature and humidity measurement
- **Visual Feedback**: LED array displays temperature levels (0-8 LEDs based on temperature)
- **Audio Alerts**: Buzzer activation for out-of-range conditions
- **USB Communication**: Real-time data streaming via USB CDC
- **Interrupt-driven**: Timer-based sensor reading for precise timing

### Environmental Thresholds
- **Temperature Range**: 15°C - 32°C (optimal range)
- **Humidity Range**: 40% - 70% RH (optimal range)
- **Alert Conditions**: Values outside optimal ranges trigger buzzer alarm

## 📊 System Architecture

### Timer Configuration
- **TIM6**: Microsecond delay timer for DHT11 communication protocol
- **TIM7**: Periodic interrupt timer for sensor reading cycles
- **TIM2**: PWM generation for buzzer frequency control

### GPIO Configuration
- **PA1**: DHT11 data line (bidirectional communication)
- **PE8-PE15**: LED array output pins
- **TIM2_CH1**: PWM output for buzzer control

### Communication Protocol
- **DHT11 Interface**: Custom bit-banging protocol implementation
- **USB CDC**: Virtual COM port for data transmission
- **Data Format**: "Temperatura: XX.XX; Umidita': XX.XX\n"

## 🛠️ Technical Implementation

### DHT11 Sensor Interface
The system implements a custom DHT11 communication protocol:
- Start signal generation (18ms low pulse)
- Response validation from sensor
- 40-bit data reception (humidity + temperature + checksum)
- Data parsing and conversion to floating-point values

### LED Array Control
Temperature visualization through progressive LED activation:
- Each LED represents approximately 7°C range
- Dynamic LED count calculation: `num_leds = temperature / 7.0`
- Real-time updates synchronized with sensor readings

### Buzzer Alert System
PWM-based audio feedback for environmental alerts:
- Variable frequency generation (20-60Hz range)
- Automatic activation for out-of-range conditions
- Progressive frequency sweep for attention-grabbing alerts

### USB Data Transmission
Real-time environmental data streaming:
- CDC class implementation for cross-platform compatibility
- Formatted string transmission with temperature and humidity values
- Non-blocking transmission for continuous operation

## 🚀 Getting Started

### Prerequisites
- **STM32CubeIDE** (version 1.13.2 or higher)
- **STM32F3 Discovery Board** or compatible STM32F303VCT6 development board
- **DHT11 Sensor** with appropriate pull-up resistor (4.7kΩ recommended)
- **8 LEDs** with current-limiting resistors
- **Buzzer** (active or passive)
- **USB Cable** for programming and communication

### Hardware Connections
```
DHT11 Sensor:
- VCC  → 3.3V
- DATA → PA1
- GND  → GND

LED Array (PE8-PE15):
- Each LED connected through 220Ω resistor to ground

Buzzer:
- Connected to TIM2_CH1 PWM output
- Add appropriate driver circuit if using passive buzzer
```

### Software Setup

1. **Clone/Download the project**
2. **Open in STM32CubeIDE**:
   ```
   File → Import → Existing Projects into Workspace
   Browse to project directory
   ```

3. **Build the project**:
   ```
   Project → Build Project
   ```

4. **Flash to microcontroller**:
   ```
   Run → Debug As → STM32 Cortex-M C/C++ Application
   ```

### Configuration
The system uses STM32CubeMX configuration file (`Progetto_APC3.ioc`) for:
- Clock configuration (72MHz system clock)
- Timer setup (TIM2, TIM6, TIM7)
- GPIO configuration
- USB device setup

## 📈 Usage Examples

### Monitor Environmental Data
Connect to the USB CDC port using any terminal application:
```
$ screen /dev/ttyACM0 115200  # Linux/macOS
$ putty                       # Windows
```

Expected output:
```
Temperatura: 23.45; Umidita': 55.20
Temperatura: 23.50; Umidita': 55.15
Temperatura: 23.48; Umidita': 55.25
```

### Visual Indicators
- **LEDs**: Number of lit LEDs indicates temperature level
  - 0 LEDs: < 7°C
  - 1 LED: 7-14°C
  - 2 LEDs: 14-21°C
  - 3 LEDs: 21-28°C
  - 4+ LEDs: > 28°C

### Audio Alerts
- **Normal Operation**: Silent operation within optimal ranges
- **Alert Mode**: Variable frequency buzzer when:
  - Temperature < 15°C or > 32°C
  - Humidity < 40% or > 70%

## 🔧 Advanced Features

### Timer-Based Sampling
- Configurable sampling rate through TIM7 period adjustment
- Interrupt-driven architecture ensures precise timing
- Non-blocking sensor communication

### Error Handling
- DHT11 communication validation
- Checksum verification (implemented but not actively used)
- Sensor presence detection

### Power Management
- Efficient timer usage minimizes power consumption
- Optimized GPIO switching reduces electromagnetic interference

---

**Author**: Student Project  
**Institution**: Università degli Studi di Napoli "Federico II"  
**Course**: Architettura e Progetto di Calcolatori  
**Year**: 2024
