# HCSR04BluePill
# STM32 Blue Pill HC-SR04 Ultrasonic Distance Sensor 
*A real-time distance measurement system using STM32F103C8 (Blue Pill) and HC-SR04 sensor.*
![hcsro4](https://github.com/user-attachments/assets/f9e81bb4-f3cb-4cdb-a49d-498baf5ccf6e)




## Key Features
- **Precision Distance Measurement**: Measures 2cm–4m range with HC-SR04 ultrasonic sensor.
- **STM32CubeIDE Project**: Leverages ST’s HAL libraries for GPIO/TIMER configuration.
- **Real-Time Feedback**: Visual output via LED indicators (e.g., LED toggles at threshold distances).

## 🔧 Hardware Setup
### Components
| Component          | Connection to Blue Pill  |
|--------------------|--------------------------|
| HC-SR04 Trig       | PA0 (GPIO_Output)        |
| HC-SR04 Echo       | PA1 (GPIO_Input)         |
| LED Indicator      | PC13 (Onboard LED)       |
| ST-Link V2        | SWDIO (PA13), SWCLK (PA14) |

### Wiring Diagram 
*Trig: PA0, Echo: PA1, VCC: 5V, GND: Common ground.*

## Software Configuration
1. **STM32CubeMX Setup**:
   - Clock: HSE 8MHz → PLL → 72MHz SYSCLK.
   - GPIO: PA0 (Trig), PA1 (Echo), PC13 (LED).
   - Timer: TIM2 for Echo pulse measurement (1µs resolution).
   - SYS: Debug = Serial Wire (for ST-Link).

2. **Code Logic**:
   ```c
   // Trigger pulse (10µs)
   HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_SET);
   delay_us(10);
   HAL_GPIO_WritePin(GPIOA, GPIO_PIN_0, GPIO_PIN_RESET);

   // Measure Echo pulse width
   uint32_t pulse_width = HAL_TIM_ReadCapturedValue(&htim2, TIM_CHANNEL_1);
   float distance_cm = (pulse_width * 0.034) / 2;  // Speed of sound
