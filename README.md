# Excerise 8
The exercise demonstrates how to eliminate resource contention selectively by using a semaphore to protect critical code sections in a multitasking environment. It builds on concepts learned in previous exercises to ensure safe access to shared resources.
The program uses the STM32F1 board and FreeRTOS to simulate multitasking and resource management while operating LEDs.

## Prequisites
### Hardware: 
STM32F103C8T6 board. Ensure the board is operational and has the required drivers installed.
### Software:
1. STM32CubeIDE for code generation and peripheral configuration.
2. STM32Cube HAL (Hardware Abstraction Layer).
3. FreeRTOS, integrated with STM32CubeMX.
### Programming Environment: 
Familiarity with C programming, embedded systems, and FreeRTOS APIs.

## Program Description
### Objective
The program demonstrates how a semaphore can protect access to shared resources in a multitasking environment, preventing resource contention and improving system safety.
### Components
#### Tasks:
1. redLEDTask: Toggles the red LED and accesses a shared resource.
2. greenLEDTask: Toggles the green LED and accesses the same shared resource.
3. orangeLEDTask: Toggles the orange LED independently without accessing shared resources.
#### Semaphore:
A binary semaphore (CriticalResourceSemaphoreHandle) is used to control access to the shared resource (AccessSharedData).
#### Shared Resource Management:
Simulated using the AccessSharedData function, which models contention by toggling a blue LED when access conflicts occur.
#### Delay Mechanism:
Tasks include delays (osDelay) to create a realistic multitasking environment.

## How to Run
### Hardware Setup:
- Connect every LED to the STM32F1 board. You can use the available pins on stm32f1. Here I use pin PA11 for RED LED, PA10 for BLUE LED, PA9 for GREEN LED, and PA8 for ORANGE LED, where the pins are connected to the positive side of the LED. While the negative side of the LED is connected to Ground (GND). And then connect the STM32F1 board to your computer via USB.
### Code Generation:
- Open STM32CubeIDE and load the provided .ioc file.
- Ensure the GPIO pins for LEDs and middleware (FreeRTOS) are configured as follows:
  #### SYS
  <img width="578" alt="sys" src="https://github.com/user-attachments/assets/566d21cc-be8c-4369-aa2d-467de1df3624">
  
  #### Pin Configuration
  <img width="395" alt="pin" src="https://github.com/user-attachments/assets/1beb6c9d-9fa8-4004-827a-bf3ef38c87f4">
  
  #### FreeRTOS
  <img width="928" alt="freertos" src="https://github.com/user-attachments/assets/c44efd36-a3ea-4e1c-897c-7d2aa88d6ee7">

- Generate the project files for your IDE.
### Build and Deploy:
- Import the generated project into your IDE.
- Add the application code (main program provided above).
- Compile the project and flash it to the STM32 board.
### Observe Behavior:
- LEDs will toggle according to their tasks.

## Features
### Semaphore Implementation:
- A binary semaphore ensures that only one task accesses the shared resource at a time.
- Prevents simultaneous access to AccessSharedData by multiple tasks.
### Multitasking with FreeRTOS:
- Demonstrates task scheduling and priority management.
- Independent task (orangeLEDTask) runs concurrently without affecting shared resource management.
### LED Control:
- Red, green, and orange LEDs toggle based on task execution.
- Blue LED indicates contention if the semaphore is not properly utilized.

## Project Files
- main.c: Application code with task definitions, semaphore management, and shared resource access logic.
- STM32CubeIDE.ioc: Configuration file for STM32CubeIDE.
- FreeRTOSConfig.h: Configuration for FreeRTOS parameters.

## How It Works?
### redLEDTask
1. Turns on the red LED:
   - HAL_GPIO_WritePin(GPIOA, GPIO_PIN_11, GPIO_PIN_SET);
2. Waits for the semaphore:
   - osSemaphoreWait(CriticalResourceSemaphoreHandle, WaitTimeMilliseconds);
   - If the semaphore is available, the task proceeds. Otherwise, it waits for the specified timeout (WaitTimeMilliseconds).
3. Accesses the shared resource:
   - Calls AccessSharedData(), which simulates resource usage and potential contention.
4. Releases the semaphore:
   - osSemaphoreRelease(CriticalResourceSemaphoreHandle);
   - Allows other tasks to access the shared resource.
5. Turns off the red LED:
   - HAL_GPIO_WritePin(GPIOA, GPIO_PIN_11, GPIO_PIN_RESET);
6. Delays for 550 ms:
   - osDelay(550);
   - Repeats the process.
### greenLEDTask
1. Turns on the red LED:
   - HAL_GPIO_WritePin(GPIOA, GPIO_PIN_9, GPIO_PIN_SET);
2. Waits for the semaphore:
   - osSemaphoreWait(CriticalResourceSemaphoreHandle, WaitTimeMilliseconds);
   - The task waits for the semaphore, similar to redLEDTask.
3. Accesses the shared resource:
   - Calls AccessSharedData(), which simulates resource usage and potential contention.
4. Releases the semaphore:
   - osSemaphoreRelease(CriticalResourceSemaphoreHandle);
5. Turns off the red LED:
   - HAL_GPIO_WritePin(GPIOA, GPIO_PIN_9, GPIO_PIN_RESET);
6. Delays for 550 ms:
   - osDelay(200);
   - Repeats the process.
### orangeLEDTask
1. Toggles the orange LED (HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_8)).
2. Waits for 50 milliseconds (osDelay(50)) before toggling again.

## Demo Video
[Exercise 8](https://github.com/user-attachments/assets/106d13f0-09b4-41d8-b4c5-b24b548eb492)

