# Plan C - Notes for Relay Functional Requirement Specification (RFRS) Form and Preliminary Relay Requirements Diagram

This plan is for Generation Facilities with Generation less than 10 MVA connected to 69 kV or 138

kV -----OR-----

Generation Greater than 5.0 MVA connected to the Distribution System

-----OR-----

Special System Constraints Warrant Using this Plan

![Plan C relay requirements diagram](../images/_page_51_Figure_14.jpeg)

## Additional Notes Pertaining to Each Device Type

1 Device Type: Potential Transformer

Number Required: 3 connected grounded-wye/grounded-wye

Purpose: To provide voltage for manual and automatic closing of source station breaker

2 Device Type: Undervoltage Relay Device #: 27

Number Required: 3 (may be part of auto-reclosing relay device #79)

Purpose: To provide voltage supervision for closing of source station breaker. Breaker may be closed if all 3 phases are dead.

3 Device Type: Synchrocheck Relay Device #: 25

Number Required: As required (depends on type)

Purpose: To provide voltage and phase angle supervision for manual and supervisory closing of source station breaker. Breaker will be manually closed for any of the following indicated conditions as requested

Live bus - Live line synchronized

Live bus - Dead line

Dead bus - Live line

Dead bus - Dead line

Not required

4 Device Type: Potential Transformer

Number Required: Depends on type of synchrocheck relay

Purpose: To provide bus voltage for synchrocheck relay

5 Device Type: Power Transformer

Number Required: As needed

High Voltage Connection: Delta for 12 kV system. Delta or grounded-wye for higher voltage systems; exact connection determined on a case-by-case basis.

6 Device Type: Transferred Trip Transmitter

Number Required: 1

Purpose: To provide for tripping of customer breaker in the event that the source station breaker should open. May be keyed by line relaying, bus relaying or 52b contact.

7 Device Type: Transferred Trip Receiver

Number Required: 1

Purpose: To receive transferred trip signal from source station so as to trip the customer breaker

8 Device Type: Breaker Failure Back-up Tripping (i.e., LBB)

Number Required: 1 (may consist of several breakers)

Purpose: To provide for tripping of the customer generator breaker in the event that the interface breaker fails to trip.

This relay to be initiated by any customer line fault relay.

9 Device Type: Potential Transformer

Number Required: 3 connected grounded-wye line side and either broken-delta or groundedwye on secondary side.

Purpose: To provide voltage to 59G or 27 relay for faults involving ground on the ComEd system. Preferred connection is broken-delta but if feeder loading is unbalanced to the point that the 3 times zero sequence voltage is normally significant, then the secondary side should be connected grounded-wye. If this PT is to provide voltage for impedance relays or directional relays and is to be used for a 59G relay, then the PT may be 3 winding type with the third winding connected grounded-wye. Using an aux. PT (grounded-wye/broken-delta) in conjunction with PT provided by note 22 may also provide the broken-delta connection.

10 Device Type: Over/Undervoltage Relays Device #: 59G or 27

Number Required: 1 if 59G relay or 3 if 27 relay

Purpose: To provide for tripping of customer breaker(s) via time delay in the event of a fault of ComEd system involving ground. If the PTs are connected broken-delta, then the relays used is the 59G overvoltage type. If the PTs are connected grounded-wye on the secondary, then the relays used are the 27 undervoltage relays. Redundancy of protection is required such that no single protection system component failure can cause a fault to remain uncleared from a system.

11 Device Type: Fault Detector Device #: 50FD

Number Required: 1 or 3, depending on type. CT ratio is \_\_\_\_.

Purpose: Fault Detector for 21-1 and 21-2 relays (notes 12 and 13)

12 Device Type: Impedance Relay Device #: 21-1

Number Required: 1

Purpose: To provide for tripping of customer breaker in the event of a phase fault on the ComEd system. This relay is the Zone 1 impedance relay. This relay may be part of a stepped zone phase and ground distance relay package. If CTs are located on the low voltage side of the transformer and the PT is on the high voltage side, then this relay must receive delta current. See note 22 for required PT location. This relay may not be suitable for all locations.

13 Device Type: Impedance Relay plus Timer Device #: 21-2/2

Number Required: 1 impedance relay plus 1 timer

Purpose: Provide for tripping of customer breaker in the event of a phase fault on the ComEd system. This relay is the Zone 2 impedance relay. This relay may be part of a stepped zone phase and ground distance relay package. If CTs are located on the low voltage side of the

transformer and the PT is on the high voltage side, then this relay must receive delta current. See note 22 for required PT location. This relay may be on low voltage side (using CTs and PTs located on the low voltage side) if no 21-1 relay is used. The time should be capable of providing a trip time in the .5-second to 2-second range. Redundancy of protection is required such that no single protection system component failure can cause a fault to remain uncleared from a system.

14 Device Type: Ground Overcurrent Relay Device #: 51G

Number Required: 1

Purpose: To provide for tripping of customer breaker in the event of a phase fault involving ground on the customer system. This relay may be in the customer neutral.

15 Device Type: Overcurrent Relay Device #: 51

Number Required: 1

Purpose: To provide for tripping of the customer breaker in the event of a phase fault on the customer system

16 Device Type: Directional Power Relay Device #: 32

Number Required: 1 or 3 depending on type.

Purpose: To provide for tripping of the customer breaker if transformer size is smaller than generator, to limit power out if necessary to prevent damage to other customers or to limit power out because of ComEd system constraints. This relay is not used for fault detection. This relay may be located in a CT located on the transformer secondary.

17 Device Type: Synchronism Check Relay Device #: 25

Number Required: As required by the number of generator and transformer breakers needing synchrochecking. Not needed for most induction-type generators. Purpose: To provide for proper closing of breakers when customer generator(s) are to be paralleled to the ComEd system.

18 Device Type: Potential Transformer

Number Required: As required for synchronism check

Purpose: To provide voltage for synchronism check relays. May be one connected phase to phase or may be part of a 3-phase potential transformer package.

19 Device Type: Potential Transformer

Number Required: 3 connected grounded-wye/grounded-wye

Purpose: To provide voltage for under/overvoltage and under/ overfrequency relays.

20 Device Type: Under/Overfrequency Relay Device #: 81U/O

Number Required: 1

Purpose: To provide tripping of customer breaker in the event the frequency fails to be maintained. This relay would be expected to operate if the customer should become isolated

on the ComEd line and not be able to maintain the load. The relay should be a definite-time type with a capability of providing a trip time in the .5-second to 2-second range. A solidstate definite time type relay is recommended. The actual setting is to be determined on a case-by-case basis.

21 Device Type: Under/Overvoltage relay Device #: 27/59

Number Required: Depends on type

Purpose: To provide tripping of customer breaker should the feeder or line voltage not be maintained within acceptable limits. This relay should be a definite-time type or an instantaneous type with a timer. The relay should be capable of providing a trip time in the .5-second to 2-second range. The actual setting is to be determined on a case-by-case basis.

22 Device Type: Potential Transformer

Number Required: 3 connected grounded-wye/grounded-wye

Purpose: To provide voltage for impedance relays and power directional relays. May be part of same transformer package as described in note 19.

23 Device Type: Interrupting Device

Number Required: As needed

Purpose: May be a fuse or circuit breaker. Circuit breaker must not be dependent upon AC power for tripping

## Relay Functional Requirements Specifications Plan C

| DEVICE NO. | FUNCTION            | TYPE* | CONN. | C.T. RATIO | P.T. RATIO | NOTE # | LBB | TRIP TRANS C.B. | TRIP GEN C.B. | TRIP LINE C.B. | SUPV. CLOSE |
|------------|---------------------|-------|-------|------------|------------|--------|-----|-----------------|---------------|----------------|-------------|
| 25-1       | SYNCHRONIZING       |       |       |            |            | 17     |     |                 |               |                |             |
| 25-2       | SYNCHRONIZING       |       |       |            |            | 17     |     |                 |               |                |             |
| 27/59      | UNDER/OVER VOLTAGE  |       |       |            |            | 21     |     |                 |               |                |             |
|            | LBB                 |       |       |            |            | 8      |     |                 |               |                |             |
| 27         | UNDERVOLTAGE        |       |       |            |            | 10     |     |                 |               |                |             |
| 59G        | OVERVOLTAGE         |       |       |            |            | 10     |     |                 |               |                |             |
| 21-1       | IMPEDANCE           |       |       |            |            | 12     |     |                 |               |                |             |
| 21-2/2     | IMPEDANCE & TIMER   |       |       |            |            | 13     |     |                 |               |                |             |
| 50FD       | FAULT DETECTOR      |       |       |            |            | 11     |     |                 |               |                |             |
| 32         | DIRECTIONAL POWER   |       |       |            |            | 16     |     |                 |               |                |             |
| 51         | OVERCURRENT         |       |       |            |            | 15     |     |                 |               |                |             |
| 51G        | GROUND OVERCURRENT  |       |       |            |            | 14     |     |                 |               |                |             |
| 81         | UNDER/OVER FREQ.    |       |       |            |            | 20     |     |                 |               |                |             |
|            | TRANSFER TRIP RCVR. |       |       |            |            | 7      |     |                 |               |                |             |
