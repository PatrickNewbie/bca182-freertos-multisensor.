# FreeRTOS Multi-sensor Room Monitor Simulated in WOKWI.

* In this laboratory, I attempt to built a room-monitoring node for the STM32F103 "Blue Pill" using native FreeRTOS and the STM32 HAL, avoiding any Arduino libraries. It aims to tracks temperature, humidity, ambient light, and motion, displaying one metric at a time on an OLED screen that I can toggle through using a rotary encoder. I also included an alarm system that triggers a buzzer if the temperature strays outside the comfortable threshold, which automatically silences itself when no motion is detected in the room.


# WHAT I DID IN THIS PROJECT:

* I do this system around six prioritized FreeRTOS tasks—communicating purely through queues, a queue set, an event group, and a mutex without global variables—to handle DHT22, LDR, and PIR sensing, SSD1306 OLED, PWM buzzer, serial logging, and heartbeat LED outputs, alongside rotary encoder input that cycles through measurements or automatically enters a 15-second inactive sleep state when motion ceases, all backed by plain C++ decision logic validated by 20 host unit tests.


# WHAT ARE THE PROJECT'S FEATURES?

* Environmental Data Acquisition: Uses a Cortex-M3 DWT cycle counter inside a critical section for microsecond-level GPIO bit-banging to read temperature/humidity from a DHT22, combined with ADC1 sampling for 0–100% relative light levels and a 100 ms polling loop for PIR motion detection.

* Display Engine & Controls: Features a single-measurement OLED UI powered by a custom 5x7 font engine with integer scaling, navigated seamlessly via a KY-040 rotary encoder that cycles clockwise and counterclockwise with bidirectional edge wrapping.

* Safety & Power Management: Drives a 1 kHz TIM1_CH1 hardware PWM alert when temperatures breach the 18.0–30.0 °C threshold (ALARM LOW/ALARM HIGH), paired with a PIR-driven state machine that enters an inactive power-saving mode after 15 seconds of quiet.

* System Diagnostics & Quality Assurance: Keeps serial communication thread-safe via mutex-guarded 115200-baud USART1 logs, verified by 20 host-side Unity unit tests (pio test -e native) and static analysis via cppcheck.

 # WHAT IS OUR OBJECTIVE THIS THIS LABORATORY?

* Structuring firmware as cooperating RTOS tasks, with priorities justified by timing needs rather than habit.
* Choosing the right IPC primitive for each job: latest-value queues, one queue per consumer, a queue set to block on two sources, an event group for shared status flags, and a mutex with priority inheritance for a shared peripheral.
* Deterministic periodic scheduling with vTaskDelayUntil().
* Timing-critical bit-banging under an RTOS: keeping a critical section short and doing everything else outside it.
* Register-level peripheral work with the STM32 HAL: ADC, I2C, USART, advanced-timer PWM, and the DWT cycle counter.
* Separating hardware-independent logic so it can be unit tested on a PC.
* Engineering hygiene: static analysis with justified resolutions, milestone commits, and a running development log.

# SIMULATION IN WOKWI USING VSCODE:

<img width="1578" height="868" alt="image" src="https://github.com/user-attachments/assets/6e2f1ec2-f79d-4848-9439-1db622b535d7" />

# CONLUSION: 

This laboratory activity demonstrated the design and implementation of a modular, thread-safe embedded system on the STM32F103 "Blue Pill," using a prioritized six-task FreeRTOS architecture that eliminates global variables through RTOS primitives like queues, event groups, and mutexes. The system brings together high-precision microsecond bit-banging via the Cortex-M3 DWT cycle counter, ADC1 light sensing, hardware PWM audio alerts, and custom OLED UI rendering with rotary encoder controls, while utilizing a PIR-driven state machine for power management. Finally, by decoupling decision logic into native C++, the software architecture was systematically validated using 20 host-side Unity unit tests and documented cppcheck static analysis to guarantee embedded code quality and reliability. 








