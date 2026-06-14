# Appendix 5: Relay Functional Requirements Specifications Form and Preliminary Relay Requirements Diagrams

 NOTE: (The following protective relaying diagrams are for general purposes only and may vary depending on the interconnection request. In addition, these typical protective relaying diagrams only apply to DER that are connected to a radial feeder/line and are also NOT either a Level 3 Non-exporting or Limited Export System)

## Protective Device Numbering

In the following requirements and examples, the terminology and numbering of protective devices adhere to the standards defined in ANSI C37.2. Information provided by the customer should use standard numbering for the more commonly used devices, as shown in the following list:

| Number | Device                                                     |
|--------|------------------------------------------------------------|
| 2      | Timer                                                      |
| 4      | Master Contactor                                           |
| 11     | Multifunction Device                                       |
| 21     | Distance or Impedance                                      |
| 24     | Overexcitation Relay                                       |
| 25     | Synchronizing or Synchronism Check                         |
| 27     | Undervoltage                                               |
| 32     | Power Direction                                            |
| 37     | Underpower                                                 |
| 40     | Loss of Field Detection                                    |
| 46     | Current Balance                                            |
| 47     | Voltage Phase Sequence                                     |
| 50     | Phase Instantaneous Overcurrent                            |
| 51     | Time Overcurrent                                           |
| 51G    | Ground Time Overcurrent                                    |
| 51N    | Neutral Time Overcurrent                                   |
| 51V    | Voltage Restrained/Controlled Time Overcurrent             |
| 59     | Overvoltage                                                |
| 59G    | Overvoltage Type Ground Detector                           |
| 60     | Voltage Balance                                            |
| 67V    | Voltage Restrained/Controlled Directional Time Overcurrent |
| 78     | Phase Angle Measuring or Out-of-Step                       |
| 79     | Reclosing Relay                                            |
| 81O    | Overfrequency                                              |
| 81U    | Underfrequency                                             |
| 86     | Lock-Out                                                   |
| 86 87  | Current Differential                                       |

Plan A: General requirements for generating capacity from 25 kVA to less than 1.0 MVA Preliminary relay requirements for customer-owned generation paralleled with ComEd, for a preliminary reply to service estimate request

This Plan is for generation facilities with the following characteristics:

- Total generation is less than 50% on minimum line load for rotating DER (non-lab certified DER) and less than 100% of minimum line load for lab-certified IBR DER under ICC Part 466 Level 1 and Level 2 and
- Total generation is less than 1.0 MVA

![Plan A relay requirements diagram](../images/_page_41_Figure_7.jpeg)

## Plan A: Notes for relay functional requirements specification (RFRS) form and preliminary relay requirements diagram

This Plan is for generation facilities with the following characteristics:

Total generation is less than 50% of the minimum line load, and

- Total generation is less than 50% on minimum line load for rotating DER (non-lab certified DER) and less than 100% of minimum line load for lab-certified IBR DER under ICC Part 466 Level 1 and Level 2 and
- Total generation is less than 1.0 MVA

Relay requirement/recommendation and installer are dependent upon ComEd/IC property line location. Required relays are to be approved by ComEd.

- W: Protection required by ComEd due to Interconnection Customer's parallel generation; to be installed by ComEd at Interconnection Customer's expense.
- X: Protection required by ComEd due to Interconnection Customer's parallel generation; to be installed by Interconnection Customer at Interconnection Customer's expense.
- Y: Protection recommended by ComEd due to Interconnection Customer's parallel generation; to be installed by Interconnection Customer at Interconnection Customer's expense.
- Z: Protection recommended by ComEd due to Interconnection Customer's parallel generation; to be installed by ComEd at Interconnection Customer's expense.
- EX: Existing relay or equipment
- N/A: Not applicable in this case
- IN: Equipment to be installed

## Additional notes for each device type

- 1 Device Type: Synchrocheck Relay Device #: 25
  - Number Required: As required by the number of generator and transformer breakers needing synchrochecking not needed for most induction type generators.
  - Purpose: Provide for proper closing of breakers when customer-generators are to be paralleled to the ComEd system.
- 2 Device Type: Voltage Transformer
- Number Required: 3 connected grounded-wye/grounded-wye.
- Purpose: Provide voltage for under/over voltage and under/over frequency relays.
- 3 Device Type: Voltage Transformer
- Number Required: As required for synchronizing.
  - Purpose: Provide voltage for synchronizing relays. It may be one connected phase to phase or part of a 3-phase voltage transformer package.
- 4 Device Type: Under/Over Frequency Relay Device #: 81U/O Number Required: 1

Purpose: Provide tripping of customer breaker in the event the frequency fails to be maintained. This relay would be expected to operate if the customer should become isolated on the ComEd line and not maintain the load. The relay should be a definite-time type to provide a trip time in the 0.5 s to 2 s range. The actual setting is to be determined on a caseby-case basis.

- 5 Device Type: Under/Over Voltage Relay Device #: 27/59 Number Required: It depends on the type. Purpose: Provide tripping of customer breaker should the feeder or line voltage not be maintained within acceptable limits. This relay should be a definite-time type or an instantaneous type with a timer. The relay should be capable of providing a trip time in the 0.5 s to 2 s range. The actual setting is to be determined on a case-by-case basis.
- 6 Device Type: Power Transformer Number Required: As needed High voltage connection: Delta
- 7 Device Type: Interrupting Device Number Required: As needed Purpose: It may be a fuse or a circuit breaker. A circuit breaker must not be dependent upon AC power for tripping.

| DEVICE NO. | FUNCTION           | TYPE* | CONN. | C.T. RATIO | P.T. RATIO | PLAN A Note # | LBB | TRIP TRANS C.B. | TRIP GEN C.B. | TRIP LINE C.B. | SUPV. |
|------------|--------------------|-------|-------|------------|------------|---------------|-----|-----------------|---------------|----------------|-------|
| 25-1       | SYNCHRONIZING      |       |       |            |            | 1             |     |                 |               |                |       |
| 25-2       | SYNCHRONIZING      |       |       |            |            | 1             |     |                 |               |                |       |
| 27/59      | UNDER/OVER VOLTAGE |       |       |            |            | 5             |     |                 |               |                |       |
| 81         | UNDER/OVER FREQ.   |       |       |            |            | 4             |     |                 |               |                |       |
