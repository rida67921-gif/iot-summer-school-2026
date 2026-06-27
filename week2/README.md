# Week 2: Arduino Fundamentals

**Questions Covered**: Q11–Q20

This module covers core Arduino programming concepts including digital I/O, serial communication, finite state machines (FSM), PWM control, and timing strategies. You will learn to build responsive, real-world IoT applications using non-blocking timing patterns.

---

## Module Checklist

- [ ] Q11: Arduino IDE & Board Setup
- [ ] Q12: Microcontroller vs Microprocessor
- [ ] Q13: Arduino Pin Types & Functions
- [ ] Q14: Traffic Light FSM with Non-Blocking Timing
- [ ] Q15: Digital Piano with Tone Generation
- [ ] Q16: Serial Command Interface Parser
- [ ] Q17: PWM Breathing LED (Multiple Modes)
- [ ] Q18: Vending Machine FSM
- [ ] Q19: Theory: analogWrite vs analogRead & PWM
- [ ] Q20: Theory: setup()/loop() & Non-Blocking Timing

---

## Key Learnings

- **Q11–Q13**: IDE setup and understanding Arduino hardware capabilities
- **Q14–Q18**: Building FSMs and interactive applications with buttons, LEDs, and serial control
- **Q17–Q20**: PWM, non-blocking timing with `millis()`, and responsive loop design
- **Q19–Q20**: Theory foundations for embedded systems timing and I/O

---

## Folder Structure

```
week2/
├── README.md                           # This file
├── theory_answers.md                   # Q19 & Q20 theory responses
├── traffic_light/
│   ├── traffic_light.ino               # Q14: 3-LED FSM + pedestrian button
│   └── README.md                       # Wiring, timing, millis() explanation
├── digital_piano/
│   ├── digital_piano.ino               # Q15: 4 buttons → tone(), simultaneous 2-button bonus
│   └── README.md                       # Button layout, frequency mapping
├── serial_command_interface/
│   ├── serial_command_interface.ino    # Q16: LED_ON, LED_OFF, BLINK_X, STATUS, RESET
│   └── README.md                       # Command reference, input validation
├── pwm_night_light/
│   ├── pwm_night_light.ino             # Q17: 3 breathing modes + SOS pattern
│   └── README.md                       # Mode descriptions, analogWrite() usage
└── vending_machine_fsm/
    ├── vending_machine_fsm.ino         # Q18: 4-state FSM with state diagram
    └── README.md                       # State transitions, button mapping
```

---

## Core Concepts This Week

### Finite State Machines (FSM)
- Used in Q14 (traffic light), Q18 (vending machine)
- Each state has defined transitions based on inputs
- State transitions print to Serial for debugging

### Non-Blocking Timing with `millis()`
- Essential for responsive systems (required for Q22 bonus mark)
- Avoids `delay()` blocking the main loop
- Allows simultaneous sensor reading and actuator control

### PWM (Pulse-Width Modulation)
- Q17: Breathing LED uses `analogWrite(pin, 0-255)`
- Enables variable brightness and servo/motor speed control
- Foundation for Q25 (servo), Q26 (motor), Q33 (web server)

### Serial Communication
- Q16: Command parsing and response
- Debugging tool throughout all modules
- Input validation crucial for robust code

---

## Tips for Success

1. **Test one feature at a time**: Upload and verify each button/LED/tone works before combining
2. **Use Serial.print() liberally**: Debug state changes, button presses, timing intervals
3. **Comment state transitions**: Make FSM logic easy to follow (especially Q14, Q18)
4. **Prefer `millis()` over `delay()`**: Keeps loop responsive, earns bonus marks
5. **Validate serial input**: Check for malformed BLINK_ commands (Q16 requirement)

---

## Hardware Overview (General)

| Component | Q14 | Q15 | Q16 | Q17 | Q18 |
|-----------|-----|-----|-----|-----|-----|
| LED (×3) | ✓ | — | ✓ | ✓ | ✓ |
| Push Button (×4) | ✓ | ✓ | — | ✓ | ✓ |
| Buzzer/Piezo | — | ✓ | — | — | — |
| Servo | — | — | — | — | — |
| Resistors | ✓ | ✓ | ✓ | ✓ | ✓ |

---

## Next Steps

1. **Q11–Q13**: Document your Arduino IDE setup and pin reference in a personal note
2. **Q14**: Implement traffic light FSM using millis() for non-blocking timing
3. **Q15**: Build a 4-button musical keyboard with `tone()` function
4. **Q16**: Create a serial command parser for LED control (validation required)
5. **Q17**: Implement 3 breathing modes with PWM and button cycling
6. **Q18**: Design a vending machine FSM with 3 buttons and 3 LEDs
7. **Q19–Q20**: Answer theory questions in `theory_answers.md`

---

**Recommended Reading**:
- [Arduino millis() vs delay()](https://www.arduino.cc/reference/en/language/functions/time/millis/)
- [PWM Explanation](https://www.arduino.cc/en/Tutorial/PWM)
- [Finite State Machines](https://en.wikipedia.org/wiki/Finite-state_machine)
- [Arduino Serial Communication](https://www.arduino.cc/reference/en/language/functions/communication/serial/)
