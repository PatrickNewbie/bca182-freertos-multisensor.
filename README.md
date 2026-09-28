# FreeRTOS Multi-sensor Room Monitor Simulated in WOKWI.

*In this laboratory, I attempt to built a room-monitoring node for the STM32F103 "Blue Pill" using native FreeRTOS and the STM32 HAL, avoiding any Arduino libraries. It aims to tracks temperature, humidity, ambient light, and motion, displaying one metric at a time on an OLED screen that I can toggle through using a rotary encoder. I also included an alarm system that triggers a buzzer if the temperature strays outside the comfortable threshold, which automatically silences itself when no motion is detected in the room.
