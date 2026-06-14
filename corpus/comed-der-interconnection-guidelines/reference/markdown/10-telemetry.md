# Telemetry

## Inverter Based DER

Any inverter-based DER projects that meet the following criteria will be required to install communications to ensure real-time SCADA telemetry. These include:

- Any project 1.0MW to 5MW:
  - Installation of a Distribution Automation (DA) device or equivalent approved system
  - All SCADA points listed below except relay failure status
  - Polling Rate of 5-minute maximum

- Any project that is greater than 5MW
  - Installation of communication equipment to support required polling rate
  - All SCADA points listed below
  - Polling Rates of 30 seconds (analog) and 2-4 seconds (status)

- Any project that is participating in the PJM Wholesale Market
  - Installation of communication equipment to support required polling rate
  - All SCADA points listed below
  - Polling Rates of 10 seconds (analog) and 2-4 seconds (status)

- Net-metered C&I projects are not required to provide SCADA information.

Basis for Real-time SCADA requirements:

- Monitor impact of larger installations on ComEd system
- Monitor performance during transmission and distribution faults
- Monitor feeder loading and performance (voltage and frequency)
- Verify islanding performance

Inverter Communication Specifications to be determined based on approved tariff requirements.

The following is a preliminary list of SCADA points required. This represents the minimum list of data points required.

- 3 Phase kV (Voltage)
- 3 Phase Amps
- 3 Phase MVA
- 3 Phase MW
- 3 Phase MVAR
- 3 Phase MWh
- Relay Failure Status (as required)
- Communication Failures (as required)

## Other Information

- Each project is evaluated on a case-by-case basis and may result in additional SCADA communication requirements.
- For T1 (TDM) connections There are a limited number of ports capable of a T1 (TDM) connections within the Exelon environment. For this type of connection, the preferred method would be an MPLS connection from Exelon approved vendors through private backhaul. The Interconnection Customer is responsible for the monthly cost of such circuits.. The developers/installers need to coordinate network configuration with Exelon Engineering.
- Communication backhaul will be determined by Exelon and ComEd Engineering to provide interconnection customers with the least-cost technical feasible options that meet project requirements. Refer to [Appendix 3](15-appendix-3-telemetry.md) for varies options, not all options are suitable based on technical requirements.

Proposed DER facilities that meet the following criteria will be required to install communications to ensure real-time SCADA telemetry.

| DER Capacity                             | Real-time SCADA Communications Required       | SCADA Telemetry:  Note:                                                             |
|------------------------------------------|-----------------------------------------------|-------------------------------------------------------------------------------------|
| 1MW to 5MW                               |                                               | Installation of a Distribution Automation (DA) device or equivalent approved system |
|                                          | Yes                                           | All SCADA points - except relay failure status                                      |
|                                          |                                               | Polling Rate of 5-minute maximum                                                    |
| Greater than 5MW                         | Yes                                           | Installation of communication equipment to support required polling rate            |
|                                          | Yes                                           | All SCADA points listed                                                             |
|                                          |                                               | Polling Rates of 30 seconds (analog) and 2-4 seconds (status)                       |
| Net-metered C&I projects at any sizes    | No                                            | not required to provide SCADA information.                                          |

See [Appendix 3](15-appendix-3-telemetry.md) for options regarding Projects greater than 5.0MW.

## Cybersecurity Requirements

Interconnection Customers' facilities or solutions which require interconnection with ComEd are required to go through the Exelon Architecture Review Process, in which our IT and Security Architecture teams in conjunction with other stakeholders evaluate the proposed solution against Exelon/ComEd cybersecurity policies, processes and controls required for: a.) connectivity to Exelon/ComEd systems; b.) access to Exelon/ComEd data; or c.) significant work affecting Exelon/ComEd systems. During the evaluation period, Exelon's Third Party Security Team evaluates the internal cybersecurity policies, processes, and controls and overall security posture of the customer as part of the Security Risk Assessment process. The customer is required to submit its cybersecurity plan, implementation strategy, and detailed architecture of the DER facility to Exelon/ComEd for review and approval. The Architecture Review Process verifies and ensures that risk is accounted for and monitored, risk reducing measures are consistently in place, adequate security controls and practices are deployed during the project and are continuously implemented and monitored throughout the lifecycle of the interconnectivity to Exelon/ComEd systems. The cybersecurity plan should address vendor's supply chain security, securing of devices and systems, such as secure communications paths, networks and protocols, secure configurations, application security, backup/recovery, patching, vulnerability management, change management, physical and cyber safety precautions and mitigations, and lifecycle management of the DER Facility, and should include any associated hardware or software applications that directly or indirectly communicate with Exelon/ComEd systems.

## Rotating Generators (Synchronous and Induction)

Some generators will require continuous telemetry to ComEd's operation facilities. These will typically be large generators, generators involved in wholesale transactions, or generators which are dispatchable by ComEd. Telemetry may be required for one or more of the following reasons:

- System Control. ComEd has an obligation to maintain frequency and generation/load balance within its service territory. Changes in the status of large amounts of generation, without real-time telemetry, are detrimental to system control.
- Monitor Wholesale Power Transaction. ComEd must be able to monitor wholesale power transactions taking place between generators and third parties through the ComEd System.
- Transmission and Distribution System Operation. The status of large generators significantly impacts operating decisions. Operators need to know the status of these large generators before performing routine or emergency switching.
- Public Safety. Generators can potentially keep a portion of the electrical grid energized while isolated from the ComEd System. It is critical to detect these situations as soon as they occur so that corrective action can be taken, since the safety of the public and of ComEd workers is at stake.

Generators that meet the following criteria require implementing telemetry to ComEd's control center and dial-up telephone communication to the revenue meter. Required telemetry is listed below each criterion. If more than one criterion applies to a generator, the telemetry requirements of each criterion must be met. If PJM's metering requirements are stricter than ComEd's, than PJM's metering requirements take precedence.

1. If the aggregate generation at a site is greater than 5.0MW and greater than 50% of the peak load at the site.
   - Continuous telemetry required.
   - Instantaneous MW and MVAR of each generator.
   - Instantaneous revenue grade MW and MVAR; and cumulative revenue grade MWhr and MVARhr at all points of interconnection with ComEd.
   - Status of all circuit breaker(s) which can disconnect a generator from the ComEd System.
   - Status of bus tie circuit breaker(s).
   - At least one bus kV measurement.
2. If the generation is involved in sales transactions through the ComEd System.
   - Continuous telemetry required.
   - Instantaneous revenue grade MW and MVAR; and cumulative revenue grade MWhr and MVARhr at all points of service from ComEd.
   - Aggregate instantaneous MW and cumulative MWhr of all third-party loads inside ComEd's control area.
3. If the generation is involved in a Power Purchase Agreement (PPA) or participating in the PJM capacity markets which contains unit specific performance or a unit specific payment structure.
   - Continuous telemetry required.
   - Instantaneous revenue grade MW and MVAR; and cumulative revenue grade MWhr and MVARhr at the generator's step-up transformer high side (or equivalent net output) for each unit.
   - Instantaneous revenue grade MW and MVAR; and cumulative revenue grade MWhr and MVARhr at all points of interconnection with ComEd all points of service from ComEd.
4. If the generation will be remotely turned on/off by ComEd.
   - Continuous telemetry required.
   - Instantaneous revenue grade MW and MVAR; and cumulative revenue grade MWhr and MVARhr at all points of service from ComEd.
   - Supervisory control for generator's (or generators') on/off from ComEd.
5. If multiple generators over a large area with an aggregate generation greater than 40 MW are being centrally controlled.
   - Continuous telemetry required.
   - Aggregate instantaneous MW of all generators.
6. If the generation, for protection, requires transfer trip communication, then generation site transfer trip communication status shall be telemetered. ComEd may also require Generation Commercial Management (GCM) or similar communication software. GCM helps to facilitate communication between ComEd's operation center and the generating unit. GCM will ensure the timely and effective communication of:
   - changes in unit status
   - changes in dispatch
   - switching orders
   - emergency conditions

   The implementation of GCM will be developed during the interconnection engineering and design phase. Interconnection Customers supplying ancillary services will require additional telemetry. [Appendix 3](15-appendix-3-telemetry.md) provides ComEd's telemetry standards. PJM may have additional standards. Details of the specific telemetry requirements will be provided at the initial project meeting with ComEd. The Interconnection Customers will be responsible for the installation cost and monthly communication costs of the required telemetry.

7. Generators that do not participate as capacity resources must provide instantaneous real power data only if:
   - they are 10 MW or larger
   - they are greater than 1 MW and connected at a bus operating at 34 kV and above

Refer to PJM Manuals `M-01` and `M-14D` for further data requirements.

Manufacturer specifications for frequency and voltage protection schemes must be submitted to ComEd for review. If this protection is not an integral part of a listed, manufactured power source interconnection system, ComEd shall have the right to require testing of the protection device systems at the Interconnection Customer's expense.
