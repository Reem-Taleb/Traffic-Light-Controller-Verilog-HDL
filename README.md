# Smart Traffic Light Controller with Pedestrian Request
# FSM Design & Verification (Verilog)

## Overview
This project implements a Traffic Light Controller using a Finite State Machine (FSM) in Verilog.
The system controls traffic for:
- Main Road
- Side Road
- Pedestrian Crossing
The controller is designed based on a formal specification, then verified using a self-checking testbench and a behavioral reference model.

## Key Features
✅ FSM-based synchronous design
✅ Moore Machine (outputs depend only on state)
✅ Binary state encoding
✅ Timer-controlled state transitions
✅ Pedestrian priority handling
✅ Request queuing mechanism
✅ Self-checking verification system
✅ Cycle-accurate comparison with reference model

## System Behavior
Default State:
- Main Road: 🟢 GREEN
- Side Road: 🔴 RED

 ## Transition Logic
1. If Vehicle Sensor (VS) or Pedestrian Button (PB) is activated:
   - Main → Yellow → All Red
   - Then serve request
2. Priority Handling:
   - Pedestrian request → served first
   - Then side road request
3. Request Queuing:
   - Requests during active states are stored and served later

## FSM States
| State | Description                       |
| ----- | --------------------------------- |
| MG    | Main Green (default, reset state) |
| MY    | Main Yellow                       |
| AR    | All Red (decision state)          |
| SG    | Side Green                        |
| SY    | Side Yellow                       |
| PG    | Pedestrian Green                  |

## Timing Constraints
| State            | Duration               |
| ---------------- | ---------------------- |
| Main Green       | ≥ 60s                  |
| Side Green       | 40s                    |
| Pedestrian Green | 40s                    |
| Yellow           | 5s                     |
| All Red          | Short transition state |

## Design Methodology
- FSM implemented using binary encoding
- All transitions occur on posedge clk (synchronous design)
- Outputs depend only on current state → Moore Machine
- A shared 8-bit timer controls all state durations
- Timer resets automatically on each state transition

## Input Signals
| Signal | Description                |
| ------ | -------------------------- |
| clk    | Clock signal               |
| rst    | Reset                      |
| VS     | Vehicle sensor (side road) |
| PB     | Pedestrian button          |

## Output Signals
| Signal     | Description                      |
| ---------- | -------------------------------- |
| light_main | Main road lights {RED, YEL, GRN} |
| light_side | Side road lights {RED, YEL, GRN} |
| walk       | Pedestrian signal                |

## Internal Design Concepts
- **Request Queuing**
- **Uses internal flags:**
- Ped_request
- Side_request
- **Ensures:**
- No request is lost
- Ordered execution
- Safe transitions

- **Priority Handling**
- Pedestrian requests have higher priority
- Evaluated first in **All Red state**

- **Verification State**
- verf_state signal added for debugging and verification
- Does NOT affect functionality

- **Verification Strategy**
- **Reference Model**
A behavioral module (traffic_ctrl_ref) is used to:
- Generate expected outputs
- Validate RTL correctness

- **Self-Checking Testbench**
The testbench:
Instantiates both:
- RTL design
- Reference model
- Automatically compares outputs every clock cycle

- **Tested Scenarios**
✅ Reset behavior
✅ Normal operation (no requests)
✅ Pedestrian-only request
✅ Side-road-only request
✅ Simultaneous requests (priority validation)

- **Error Detection**
Any mismatch:
- Prints error details
- Stops simulation immediately

- **Simulation & Results**
- Waveforms match testbench outputs exactly
- All transitions verified successfully
- Timing behavior is correct

- **Example Observation:**
**At t = 655:**
- Timer = 60 → MG state
- Main: GREEN (001)
- Side: RED (100)
- Walk: OFF
**At t = 775:**
- Transition MG → MY
- Caused by PB request
**At t = 845:**
- Pedestrian walk activated
- Timer reset for 40s duration

## Project Deliverables
- FSM State Diagram
- Verilog RTL Code
- Testbench
- Simulation Waveforms
- Report

## Conclusion
This project demonstrates a complete **digital system design** flow:
1. Specification → FSM design
2.  RTL implementation
3. Verification using reference model

The system ensures:
- Correct functionality
- Stable synchronous behavior
- Accurate timing control
- Reliable request handling

## Author
[Reem Taleb](https://github.com/Reem-Taleb)

## Notes
This project was developed as part of a digital design / computer engineering course focusing on:
- FSM design
- Verilog HDL
- Verification methodologies

