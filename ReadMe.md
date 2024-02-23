# LPTIM-Tick-U5

This repository is a port of the [LPTIM-Tick-U5](https://github.com/jefftenney/LPTIM-Tick-U5)
repository to the NUCLEO-U575ZI-Q development board.

-----

*FreeRTOS Tick/Tickless via LPTIM in STM32U5*

Use LPTIM for the FreeRTOS tick instead of the SysTick Timer for ultra-low-power applications.

- No drift or slippage in kernel time
- Use STOP modes even while FreeRTOS timers are running or delays are underway
- For any STM32 with LPTIM (see [LPTIM-Tick](https://github.com/jefftenney/LPTIM-Tick))

This repository demonstrates adaptation of the [lptimTick.c gist](https://gist.github.com/jefftenney/02b313fe649a14b4c75237f925872d72) to the STM32U5 series MCU.  The specific target platform is the ST [NUCLEO-U575ZI-Q](https://www.st.com/en/evaluation-tools/nucleo-u575zi-q.html) (STM32U575).  The project uses a Makefile and the STM32CubeMX code-generation tool.  However, lptimTick.c is compatible with any toolchain supported by FreeRTOS.

For a thorough evaluation, this project can be built without tickless idle, with the default tickless idle, or with the custom tickless idle provided by lptimTick.c.  See branches for additional evaluation options.

---

## NUCLEO-U575ZI-Q Demo

Press the blue button to cycle between tests:
1. Maintain kernel time only.  LED blinks every 5 seconds.
2. Validate tick timing.  LED blinks every 2 seconds.
3. Stress test tick timing.  LED blinks every second.

Tests 2 and 3 display live test results to a serial terminal.  Connect to the STLink Virtual COM Port at 115200 8N1.  Additionally, the LED blinks twice (instead of just once) in case of test failure.

## Test Results
*Current readings shown are averages, *not* including the LED*

Please note: The following values are carried over from the original repository,
and have not been validated on the NUCLEO-U575ZI-Q board.

__With lptimTick.c (`configUSE_TICKLESS_IDLE 2`)__

- Test 1: 9μA, no drift
- Test 2: 35μA, no drift
- Test 3: 57μA, no drift

__Default tickless idle (`configUSE_TICKLESS_IDLE 1`)__

- Test 1: 0.49mA, trivial drift
- Test 2: 0.50mA, trivial drift
- Test 3: 0.51mA, trivial drift (with kernel v10.5.1 or newer)

__Tickless disabled (`configUSE_TICKLESS_IDLE 0`)__

- Test 1: 1.21mA, no drift
- Test 2: 1.21mA, no drift
- Test 3: 1.21mA, no drift
