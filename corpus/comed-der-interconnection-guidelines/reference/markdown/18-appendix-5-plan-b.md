# Plan B: General requirements for generating capacity greater than 1.0 MVA up to and including 5.0 MVA

Preliminary relay requirements for customer-owned generation paralleled with ComEd, for a reply to a service estimate request

This plan is for generation facilities with the following characteristics:

- Total generation is greater than or equal to 50% of the minimum line load, or
- Total generation is greater than or equal to 50% of the minimum line load for rotating DER (non-lab certified) DER or greater than 100% of the minimum line load for lab-certified IBR DER under Level 2
- Total generation is greater than or equal to 1.0 MVA up to and including 5.0 MVA
- Special system constraints warrant using this plan

![Plan B relay requirements diagram](../images/_page_45_Figure_10.jpeg)

## Plan B: Notes for relay functional requirements specification (RFRS) form and preliminary relay requirements diagram

This plan is for generation facilities with the following characteristics:

- Total generation is greater than or equal to 50% of the minimum line load for rotating DER (non-lab certified) DER or greater than 100% of the minimum line load for lab-certified IBR DER under Level 2 or
- Total generation is greater than or equal to 1.0 MVA up to and including 5.0 MVA
- Special system constraints warrant using this plan
- W: Protection required by ComEd due to Interconnection Customer's parallel generation; to be installed by ComEd at Interconnection Customer's expense.
- X: Protection required by ComEd due to Interconnection Customer's parallel generation; to be installed by Interconnection Customer at Interconnection Customer's expense.
- Y: Protection recommended by ComEd due to Interconnection Customer's parallel generation; to be installed by Interconnection Customer at Interconnection Customer's expense.
- Z: Protection recommended by ComEd due to Interconnection Customer's parallel generation; to be installed by ComEd at Interconnection Customer's expense.
- EX: Existing relay or equipment
- N/A: Not applicable in this case
- IN: Equipment to be installed

## Additional notes for each device type

1 Device Type: Voltage transformer

Number Required: 3 connected grounded-wye/grounded-wye.

Purpose: Provide voltage for manual and automatic closing of the source station breaker.

2 Device Type: Undervoltage relay device #: 27

Number Required: 3 (It may be part of auto-reclosing relay-device type 79)

Purpose: Provide voltage supervision for the closing of the source station breaker. The breaker may be closed if all 3 phases are dead.

| 3   | Device Type: Synchrocheck Relay Device #: 25                                          |
| --- | ------------------------------------------------------------------------------------- |
|     | Number Required: As required (it depends on type).                                    |
|     | Purpose: To provide voltage and phase angle supervision for manual and supervisory    |
|     | closing of source station breaker. The breaker will be manually closed for any of the |
|     | following indicated conditions, as requested by:                                      |
|     | Live bus Live line synchronized                                                       |
|     | Live bus Dead line                                                                    |
|     | Dead bus Live line                                                                    |

| Dead bus Dead line |
| ------------------ |
| Not required       |
|                    |

4 Device Type: Voltage Transformer

Number Required: It depends on the type of synchrocheck relay.

Purpose: To provide bus voltage for synchrocheck relay.

5 Device Type: Power transformer Number Required: As needed High voltage connection: Delta

6 Device Type: Breaker failure backup tripping (i.e., LBB)

Number Required: 1 (it may consist of several relays).

Purpose: To provide for tripping of customer-generator breaker if the interface breaker fails to trip. This relay is to be initiated by any Interconnection Customer line fault relay.

7 Device Type: Voltage Transformer

Number Required: 3 connected grounded-wye line side, and either broken delta or grounded-wye on the secondary side.

Purpose: Provide voltage to 59G or 27 relays for faults involving ground in the ComEd system. The preferred connection is a broken delta, but if feeder loading is unbalanced to the point that three times the zero-sequence voltage is normally significant, then the secondary side should be connected in grounded-wye. If this VT is to provide voltage also for impedance relays or directional relays and is to be used for a 59G relay, then the VT may be a three-winding type, with the third winding connected in grounded-wye, or the broken delta connection may be provided by using an aux. VT (grounded-wye/broken delta) if the main VT is grounded-wye/grounded-wye.

8 Device Type: Under/Over Voltage Relay Device #: 59G or 27

Number Required: 1 if 59G or 3 if 27 relay

Purpose: To provide tripping of customer breakers in the event of a fault in the ComEd system involving ground. If the VTs are connected in a broken delta, then the relay used is the 59G overvoltage type. If the VTs are connected in grounded-wye on the secondary, then the relays used are the 27 undervoltage relays.

9 Device Type: Impedance relay plus timer device #: 21-2/2

Number Required: 1 impedance relay plus 1 timer

Purpose: To provide for tripping of customer breaker in the event of a phase fault in the ComEd. This relay is used in a Zone 2 mode. The timer should be capable of providing a trip time in the 0.5 s to 2 s range.

10 Device Type: Voltage transformer

Number Required: 3

Purpose: To provide voltage for impedance relays and power directional relays.

11 Device Type: Ground overcurrent relay device #: 51G

Number Required: 1

Purpose: To provide for tripping of customer breaker in the event of a fault involving ground on the Interconnection Customer system. This relay may be in the transformer neutral.

12 Device Type: Overcurrent relay device #: 51

Number Required: 3 or 1 3-phase

Purpose: To provide for tripping of customer breaker in the event of a phase fault on the Interconnection Customer system.

13 Device Type: Directional power relay device #: 32

Number Required: 1 or 3, depending on the type

Purpose: To provide for tripping of Interconnection Customer breaker if the transformer size is smaller than that of the generator, to limit power out if necessary to prevent damage to other customers, or to limit power out because of ComEd system constraints. This relay is not used for fault detection.

14 Device Type: Synchronizing relay device #: 25

Number Required: As required by the number of generators and transformer breakers needing synchrochecking. It is not needed for most induction type machines.

Purpose: To provide for proper closing of breakers when the Interconnection Customer generators are to be paralleled to the ComEd system.

15 Device Type: Voltage transformer

Number Required: As required for synchronizing

Purpose: To provide voltage for synchronizing relays. It may be one connected phase-tophase, or it may be a part of a 3phase voltage transformer package.

16 Device Type: Voltage transformer

Number Required: 3 connected grounded-wye/grounded-wye

Purpose: To provide for under/over voltage and under/over frequency relays.

17 Device Type: Under/overfrequency relay device #: 81U/O

Number Required: 1

Purpose: To provide tripping for Interconnection Customer breaker in the event frequency fails to be maintained. This relay would be expected to operate if the Interconnection

Customer should become isolated on the ComEd line and not maintain the load. The relay should be a definite-time type to provide a trip time in the 0.5 s to 2 s range. A solid-state definite time type relay is recommended. The actual setting is to be determined on a caseby-case basis.

18 Device Type: Under/overvoltage relay device #: 27/59

Number Required: It depends on the type.

Purpose: To provide tripping for Interconnection Customer breaker should the feeder or line voltage not be maintained within acceptable limits. The relay should be a definite-time type or an instantaneous type with a timer. The relay should provide a trip time in the 0.5 s to 2 s range. The actual setting is to be determined on a case-by-case basis.

19 Device Type: Interrupting device

Number Required: As needed

Purpose: It may be a fuse or a circuit breaker. The circuit breaker must not be dependent upon AC power for tripping.

| DEVICE NO. | FUNCTION           | TYPE* | CONN. | C.T. RATIO | P.T. RATIO | PLAN B Note # | LBB | TRIP TRANS C.B. | TRIP GEN C.B. | TRIP LINE C.B. | SUPV. CLOSE |
|------------|--------------------|-------|-------|------------|------------|---------------|-----|-----------------|---------------|----------------|-------------|
| 25-1       | SYNCHRONIZING      |       |       |            |            | 14            |     |                 |               |                |             |
| 25-2       | SYNCHRONIZING      |       |       |            |            | 14            |     |                 |               |                |             |
| 27/59      | UNDER/OVER VOLTAGE |       |       |            |            | 18            |     |                 |               |                |             |
|            | LBB                |       |       |            |            | 6             |     |                 |               |                |             |
| 27         | UNDERVOLTAGE       |       |       |            |            | 8             |     |                 |               |                |             |
| 59G        | OVERVOLTAGE        |       |       |            |            | 8             |     |                 |               |                |             |
| 21-2/2     | IMPEDANCE & TIMER  |       |       |            |            | 9             |     |                 |               |                |             |
| 51         | OVERCURRENT        |       |       |            |            | 12            |     |                 |               |                |             |
| 32         | DIRECTIONAL POWER  |       |       |            |            | 13            |     |                 |               |                |             |
| 51G        | GROUND OVERCURRENT |       |       |            |            | 11            |     |                 |               |                |             |
| 81         | UNDER/OVER FREQ.   |       |       |            |            | 17            |     |                 |               |                |             |
