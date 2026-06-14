# Appendix 4: Relay & Protection Requirements

## DER Interconnection Protection Requirements by Project Configuration

| Common DER Configurations | Protection Requirements / Applicability |
| --- | --- |
| Level 1 - Export capacity of 25 kW or less and a nameplate capacity of 50 kW or less | Applies to lab-certified, inverter-based generation. No additional protection requirements apply for lab-certified equipment. |
| Level 2 - 5 MW or less, dependent upon line voltage | Applies to lab-certified, inverter-based generation. No additional protection requirements apply for lab-certified equipment. The proposed interconnection must be to a radial distribution circuit or spot network (\< 50 kW) limited to servicing one customer. |
| Level 3a - 50 kVA or less | Applies to non-lab-certified, non-exporting systems. |
| Level 3b - \> 50 kVA to 5 MVA | Applies to non-lab-certified, non-exporting systems. |
| Level 3c - Less than or equal to 50 kW (Lab-Certified Equipment) | Applies to area-network, lab-certified equipment. The system must be non-exporting. It must use reverse power relays or another approved method to prevent export power. The aggregate of all generation must not exceed 5% of area network maximum load or 50 kW. Evaluated on a case-by-case basis. |
| Level 3d - 10 MVA or less | Applies to a radial distribution circuit and to non-exporting systems. Uses reverse power relays to prevent export power[^1]. The system must not be served by a shared transformer. |
| Level 4a - 25 kW or less | Applies to non-lab-certified systems. No additional applicability language is stated in this row. |
| Level 4b - \> 25 kW to 5 MW | Applies to non-lab-certified systems. No additional applicability language is stated in this row. |
| Level 4c - 10 MVA or less | Applies to a radial distribution circuit. The system must not be served by a shared transformer. |
| Part 467 - Total generation is equal to or greater than 10 MVA, up to 20 MVA | Applies where the connected ComEd transmission line is 69 kV or 138 kV, and the configuration is ring bus or T-Taps. |

[^1]: The source table appears to omit the object of “prevent” in this row. “Export power” has been inserted editorially for clarity because it is consistent with adjacent rows and the non-exporting context.

## DER Protective Function Applicability Matrix by Interconnection Category

> **Legend:** ✓ = shown as applicable in the source matrix; — = no checkmark shown

| Configuration | Level 1 | Level 2 | Level 3a | Level 3b | Level 3c | Level 3d | Level 4a | Level 4b | Level 4c | Part 467 | Wholesale |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2 - Timer | — | — | — | — | — | — | — | — | — | — | — |
| 4 - Master Controller | — | — | — | — | — | — | — | — | — | — | — |
| 11 - Multifunction Device/Relay - Required protective functions may be implemented in a single multifunction relay | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 21 - Distance or Impedance - Requirement determined by capacity. Does not apply to inverter-based generation | — | — | — | — | — | — | — | ✓ | ✓ | ✓ | ✓ |
| 21-1 - Impedance Relay: May be required to provide for tripping of DER customer breaker in the event of a phase fault on the ComEd system. Zone 1 impedance relay. | — | — | — | — | — | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| 21-2 - Impedance Relay + Timer: May be required to provide for tripping of DER customer breaker in the event of a phase fault on the ComEd system. This relay is the Zone 2 impedance relay. | — | — | — | — | — | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| 24 - Overexcitation (may be required) | — | — | — | — | — | — | — | ✓ | ✓ | ✓ | ✓ |
| 25 - Synchronizing or Synchronism (DER location): Provide for proper closing of breakers when DER customer generator(s) are to be paralleled to the ComEd System. May be required depending on capacity and DER type. Not required for IBR DER. | — | ✓ | — | ✓ | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| 25 - Synchronizing or Synchronism Check/Backfeed Detection (ComEd substation): May be required depending on capacity. Provide voltage for manual and automatic closing of source station breaker. Breaker will be manually closed for any of the following indicated conditions as requested by Live bus - Live line synchronized, Live bus - Dead line, Dead bus - Live line, Dead bus - Dead line, or Not required. | — | ✓ | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 27 - Undervoltage: Provide tripping of DER customer breaker should the feeder or line voltage not be maintained within acceptable limits. This relay should be a definite-time type or an instantaneous type with a timer. The relay should be capable of providing a trip time in the .5 second to 2 second range. The actual setting is to be determined on a case-by-case basis. Level 1 and Level 2 Lab-Certified Equipment may have on-board protective functions. | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 27 - Deadline Reclosing Supervision - (ComEd Substation) [Synchronous Generators & IBR]: Provide voltage supervision for closing of source station breaker. Breaker may be closed if all 3 phases are dead. | — | ✓ | — | ✓ | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| 32 (32R/32F) - Power Direction (Not suitable for fault protection): May be utilized for Level 3 Interconnections Non-Exporting Systems as an approved method to prevent export as provided in ICC Part 466.75 subsections (c)(1) & (c)(2) (Device 32 may not be used for fault protection). | — | — | ✓ | ✓ | ✓ | ✓ | — | — | — | — | — |
| 37 - Underpower: May be utilized for Level 3 Interconnections Non-Exporting Systems as an approved method to prevent export as provided in ICC Part 466.75 subsections (c)(1) & (c)(2). | — | — | ✓ | ✓ | ✓ | ✓ | — | — | — | — | — |
| 40 - Loss of Field Detection: Protection may be required to detect field loss on synchronous machines. | — | — | — | — | — | — | — | — | — | — | — |
| 46 - Current Balance: May be required for protection against phase unbalance using negative sequence current. | — | — | — | ✓ | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 47 - Voltage Phase Sequence: May be required for protection against phase unbalance by the measurement of negative sequence voltage. | — | — | — | ✓ | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 50 - Phase Instantaneous Overcurrent | — | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 50BF - Breaker Failure Backup Tripping (LBB): Provide for tripping of the DER customer generator breaker in the event that the interface breaker fails to trip. | — | — | — | — | — | — | — | ✓ | ✓ | ✓ | ✓ |
| 50FD - Fault Detector Device | — | — | — | — | — | — | — | — | — | — | — |
| 51 - Time Overcurrent: Provide for tripping of the DER customer breaker in the event of a phase fault on the customer system. | — | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 51G - Ground Time Overcurrent: Provide for tripping of DER customer breaker in the event of a phase fault involving ground on the customer system. | — | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 51N - Neutral Time Overcurrent | — | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 51V - Voltage Restrained/Controlled Time Overcurrent: May be required to provide voltage-adjusted time overcurrent in order to be sensitive to faults close to the DER which cause voltage drops and lowers the short-circuit current. (May be required). | — | — | — | ✓ | — | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| 52 - Interrupting Device: May be a fuse or circuit breaker. Circuit breaker must not be dependent upon A.C. power for tripping. | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 59 - Overvoltage: Provide tripping of DER customer breaker should the feeder or line voltage not be maintained within acceptable limits. This relay should be a definite-time type or an instantaneous type with a timer. The relay should be capable of providing a trip time in the .5 second to 2 second range. The actual setting is to be determined on a case-by-case basis. Level 1 and Level 2 Lab-Certified Equipment may have on-board protective functions. | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 59G - Overvoltage Type Ground Detector: May be required to provide for tripping of DER customer breaker(s) via time delay in the event of a fault of ComEd system involving ground. | — | — | — | ✓ | — | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| 60 - Voltage Balance (May be required) | — | — | — | ✓ | — | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| 67V - Voltage Restrained/Controlled Directional Time Overcurrent: May be required to provide voltage-adjusted directional time overcurrent in order to be sensitive to faults close to the DER which cause voltage drops and lowers the short-circuit current. | — | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 78 - Phase Angle Measuring or Out-of-step | — | — | — | — | — | — | — | — | — | — | — |
| 78VS - Vector Shift: Anti-islanding (May be required for some applications) | — | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 79 - Reclosing: (ComEd Substation) Provide voltage and phase angle supervision for manual and supervisory closing of source station breaker. Breaker will be manually closed for any of the following indicated conditions as requested by Live Bus-Live Line (Sync), Live Bus-Dead Line [source text appears truncated in the source document used for transcription]. | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 81O - Over frequency: Provide tripping of DER customer breaker in the event the frequency fails to be maintained. The actual setting is to be determined on a case-by-case basis. Level 1 and Level 2 Lab-Certified Equipment may have on-board protective functions. | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 81R - ROCOF: Anti-islanding (May be required for some applications) | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 81U - Underfrequency: Provide tripping of DER customer breaker in the event the frequency fails to be maintained. The actual setting is to be determined on a case-by-case basis. Level 1 and Level 2 Lab-Certified Equipment may have on-board protective functions. | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 86 - Lock-Out | — | — | — | ✓ | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| 87 - Current Differential* - May be required based on system configuration (As required on an individual basis). | — | — | — | — | — | — | — | — | — | ✓ | — |
| Power Transformer - As required for system interconnection | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Voltage Transformer - As required for system interconnection | — | — | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 1 - Transfer Trip Receiver: (DTT only as required by system design and configuration) Receive transferred trip signal from source station so as to trip the DER customer breaker. | — | ✓ | — | ✓ | — | ✓ | — | ✓ | ✓ | ✓ | ✓ |
