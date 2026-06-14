# Protection Schemes

## Protection Scheme Overview

This section describes the most commonly used interconnection. Here you will find general guidelines for protection schemes. Final requirements are established during the technical review process. The protection schemes in this section are intended to be cumulative, in that concepts and requirements described for Level 1 are carried forward to subsequent Levels. For example, information discussed in Level 1 applies to Level 2 and so forth.

Protection requirements for individual projects may be greater than those listed in the Guidelines, based on specific system conditions (e.g., other existing or previously-queued DER on the same circuit), and are considered on a case-by-case basis. Under certain circumstances, an interconnection classified under one Level, may require protection typically associated with a higher Level due to safety and reliability considerations.

## DER Interconnection Protection Scheme Configurations

ComEd will determine the bus and line configurations and protection requirements that are necessary to connect the DER proposed in the Interconnection Customer's application. To perform such an analysis, ComEd will require technical information on the Interconnection Customer's proposed DER facility, as well as the proposed point of interconnection ("POI") to the ComEd distribution system.

Interconnection configurations are site-specific. The following section provides protection scheme guidelines on the most commonly used configurations for interconnection by ComEd. Specifications may be found in [Appendix 4](16-appendix-4-relay-protection.md) of this document.

## General Overview of Protection Scheme Requirements

Direct Transfer Trip (DTT) to either ComEd or customer equipment may be required for any of the following conditions:

- An island condition exists with a mix of rotating-machine generation and inverter-based generation
- Generation capacity greater than 5.0 MVA
- Capacity of rotating generation is greater than 1/3 of minimum line load
- Generation has the potential to back-feed onto the transmission system

Transformer Configurations

ComEd requires all customer-owned DER facility transformers to have a Delta High-Side connection on the utility (ComEd) side of the transformer.

Transient Overvoltage (TOV) Limits

Interconnection Customer's inverters shall not, by their design or application, while interconnected to the ComEd system, cause transient overvoltage (TOV) which exceed ComEd line or equipment ratings during fault or switching operations. If the Interconnection Customer's inverters cause overvoltages that exceed the ratings of the ComEd lines or equipment, then ComEd may require that the Interconnection Customer, at its expense, mitigate these issues to a level below the equipment design ratings.

Short-Circuit Current Ratings

- Interconnection Customer's equipment shall be rated for fault currents provided by ComEd and in accordance with all applicable codes and standards.

Protective Relay

- All ComEd required protective functions must be implemented in a ComEd-approved utilitygrade protective relay.
- A Behind the Meter application with capacity of 1MW or greater, served by a 12 kV ComEd circuit, will require a Ground Fault Protection Scheme. The scheme consists of potential transformers ("PTs") upstream of the transformer, wired into a customer-owned relay. The customer is also responsible for installing secondary conduit and wire between the PT cabinet and the relay. Applications that fail both a Level 2 Expedited Review and a Supplemental Review may also require a Ground Fault Protection Scheme.

Grounded Conductor Connection

- The existing ComEd 34.5 kV distribution system in many areas operates as a three-wire grounded-wye system. Customer equipment connected to these lines should be rated for three-wire operation. ComEd recommends that the equipment be rated for 34.5 kV to ground to protect against line imbalance during ground faults.
- Newer 34.5 kV distribution lines may be built for four-wire grounded-wye operation. To reduce the risk of transient overvoltage, and to ensure swift clearing of line faults, customer equipment should be designed to provide a neutral-to-ground connection and the customer should extend a grounded service conductor to the point of interconnection when the line is this type.
- Information about the type of line and requirements for alteration of the type of line should be directed to the ComEd Interconnection Engineer.

Basic Insulation Levels (BIL) Rating

- Three-wire 34.5 kV lines are designed for a 200 kV BIL, and four-wire 34 kV lines are designed for a 150 kV BIL. Both kinds of line are equipped with lightning arresters to limit surge voltages accordingly. It is recommended that customer equipment rated for 150 kV BIL be connected only to four-wire services, or additional arrester protection may be necessary. Note that three-wire lines are subject to occasional sustained line-to-ground voltages exceeding 30 kV; arrester protection on three-wire services should be designed to withstand this, and a surge arrester MCOV rating of at least 29 kV is recommended.
- ComEd's 12.5 kV and lower voltage lines are normally designed for a 95 kV BIL in accordance with IEEE recommended practices.
