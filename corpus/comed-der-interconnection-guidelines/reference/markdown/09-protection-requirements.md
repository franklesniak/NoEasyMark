# Protection Requirements

## General Need for System Protection in the Presence of Parallel Generation

The components of the ComEd transmission and distribution (T&D) systems are subject to a variety of natural and man-made hazards, such as lightning, wind, wildlife, and vandalism. Damaged or short-circuited equipment should be switched out of service as soon as possible to minimize safety hazards and to avoid additional equipment damage. DER operated in parallel with the T&D system provides an additional source of energy that must also be disconnected in case of an emergency. Because interconnected DER facilities may interfere with the operation of protective devices normally used on the T&D system, it is essential that a suitable system of protection be used to minimize hazards and to prevent the reduction of quality of service to other ComEd customers.

## General Effects of Parallel Generation on System Protection Requirements

The interconnection of DER shall not introduce a hazard or adversely affect the quality of service to ComEd customers. Protective equipment must be added to standard ComEd facilities to provide adequate protection of the T&D system. Requirements for additional protective equipment due to parallel operation of DER will vary depending on the capacity (MW) and type of the DER facility and on the configuration of the local ComEd System.

The ComEd System Protection Department will specify all necessary protective relaying, communication and SCADA requirements for DER interconnection. Communication may include, but is not limited to, microwave communication systems, single mode fiber optic systems, and power line carrier communications systems. For information on Telemetry see [Appendix 3](15-appendix-3-telemetry.md). Examples of general relay requirements for various types of installations are discussed in [Appendix 4](16-appendix-4-relay-protection.md).

## Reclosing of ComEd Supply Lines

Most faults that occur on overhead lines are transient. That is, if the line is de-energized promptly, it can often be reclosed and returned to service. Examples of such transient faults include momentary tree contact due to wind, and insulator flashover due to lightning. Automatic reclosing of overhead lines is standard industry practice to improve system reliability. In many cases, an overhead line can be de-energized and reclosed within one second, with minimum disruption of service to the customers connected to the line.

## Effects of Parallel Generation on Automatic Reclosing

Automatic reclosing on ComEd's lines can potentially damage not only customer-owned generating equipment operated in parallel with the T&D system, but also adversely impact other ComEd customers and the ComEd system. Severe mechanical stress on the rotating generation equipment may occur if the line is reclosed while the rotating generator is still connected to the ComEd System. With synchronous generators, damage may occur when they are out of synchronism when the supply is restored; with induction generators, damage may occur if they are operating at a speed higher or lower than normal when reclosed to the system. Other types of DER may also cause damage to other ComEd customer equipment and ComEd equipment if still online during reclosing. As a general policy, ComEd does not eliminate automatic reclosing of overhead supply lines to interconnect DER, because that would risk reducing the reliability of service to other customers. In order to protect the safety and reliability of the ComEd distribution, sub-transmission and transmission systems, ComEd may require either dead-line reclosing or DTT (Direct Transfer Trip) for both inverter-based DER and rotating generators. Addition of DER to a line shall not alter standard auto- restoration schemes at transmission substations, distribution centers, or other distribution loads. Some configurations may require direct tripping of connected DER for faults on the line.

## Possible Reclosing Scenarios and Interconnection Customer Responsibilities

The Interconnection Customer is responsible for protecting the DER facility's equipment so that automatic or manual reclosing, faults, or other disturbances on the ComEd System do not cause damage to the equipment.

If automatic reclosing may result in or results in equipment damage or a safety hazard, either to the ComEd System or customer facilities, ComEd will require that additional protective equipment be installed. This will usually consist of communication and/or control equipment to disconnect the customer-owned DER (or to confirm that it is disconnected) before the ComEd supply line is reclosed.

## Requirements to Maintain Existing Levels of Reliability

Often when parallel generation is brought online, existing utility-owned protection devices on the feeder/line will need to be changed out or removed to allow continued operation of the protection elements of the circuit. If this is the case, it is required that any protection device being removed or reconfigured must be replaced with a device or logic that will maintain existing levels of reliability. This may be in the form of new advanced device(s) being installed, existing devices being upgraded, or a simple reprogramming of existing devices.

The following design requirements shall apply where DA schemes (FLISR- fault location, isolation, and service restoration) exist on distribution facilities where interconnection applications are being reviewed:

- Proposed DER facilities located within Distribution Automation zones shall not interfere with the proper operation of the scheme.
- If existing levels of reliability cannot be maintained, mitigation shall be required at customer expense.
- Proposed DER facilities located within existing protection and automation schemes must be integrated to maintain existing levels of reliability.

The ensuing ComEd Relay Protection Requirements shall be enforced when adjustments or enhancements to existing protection schemes and equipment become necessary as a consequence of interconnecting customer DER facilities. These measures aim to uphold the present safety and reliability standards of the ComEd system.

- ComEd upgrade of substation equipment essential for the implementation of dead-line reclosing or DTT in response to the interconnection of DER customer facilities.

## Other Equipment and Protection Requirements

Behind-the-meter DER refers to DER interconnected in parallel with the customer's load, sharing a common metering service and drawing power from a shared service transformer. Ground faults can create voltage rise of non-faulted phases on the primary, delta connected service transformers. Protection for DER behind the meter will only be required when there is reverse power flow, or when the generation exceeds the daytime minimum load. For the cases where the service or customer interconnection transformer has a delta connection at the high side (ComEd side), it will be necessary to install either of the following two options:

- 27/59 Under/Over-voltage protection is the preferred methodology for ground fault detection. It is to be installed using Wye-Grounded/wye-grounded Yg-ygPTs at the high side of the interconnection transformer, and on each individual phase. PTs will be owned and installed by ComEd, at customer's expense, and connected to the customer owned protective relay on secondary side of interconnection transformer to isolate customer's DER for faults on the Area EPS. This protection scheme is shown in Figure 1.
- Neutral Over-Voltage function scheme (59N), at the ComEd side of the service transformer. In this case, the 59N relay must activate a circuit breaker to isolate the Interconnection Customer DER. This protection scheme is shown in Figure 2, and is meant to detect overvoltage conditions when the utility grounding source is lost. A loading resistor connected in parallel with the 59N relay will be necessary to mitigate the potential for a ferroresonant condition.

![Under/Over voltage detection with Yg-yg PTs](../images/_page_20_Picture_3.jpeg)

*Figure 1. Under/Over voltage detection with Yg-yg PTs*

![Ground fault detection with broken delta PTs](../images/_page_20_Picture_5.jpeg)

*Figure 2. Ground fault detection with broken delta PTs*
