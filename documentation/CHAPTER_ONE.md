# CHAPTER ONE

# INTRODUCTION

## 1.1 Background to the Study

Electricity metering has moved from a passive recording activity to an intelligent control function within the smart grid environment. Smart metering supports real-time sensing, automated billing, communication, and remote control, making it an important part of modern utility systems [1], [2], [4]. In a prepaid metering context, the meter does not only measure usage; it also enforces payment status and gives immediate feedback to the consumer [2], [5].

This change is important because electricity supply is no longer judged only by how much energy is delivered, but also by how clearly consumption is measured, how fairly it is billed, and how quickly the user can understand what is happening. A smart prepaid meter therefore sits at the intersection of metrology, billing logic, consumer interaction, and utility revenue assurance [2], [5]. When the meter is designed well, it reduces disputes, improves trust, and gives both the consumer and the utility a transparent view of energy use.

The relevance of this issue becomes clearer in environments where billing disputes, delayed payments, and weak consumption visibility are common. In such settings, a prepaid meter offers a more disciplined and auditable approach than delayed postpaid billing because payment and use are linked directly at the point of consumption. This creates a practical incentive for efficient usage while reducing the burden of debt accumulation on the utility side [2], [6].

The Internet of Things extends this idea further by making the meter a connected data node. IoT-enabled metering allows readings to be published, alerts to be generated, and status information to be monitored remotely [6], [7]. In a utility context, that connectivity matters because it makes the meter visible to both the consumer and the operator, not just locally but across a wider network of monitoring and decision-making tools [3], [6].

This study focuses on an IoT-based smart prepaid energy meter built around the ESP32, the PZEM-004T energy meter module, a relay-based load controller, a display interface, and Wi-Fi connectivity for remote monitoring. The design reflects the wider smart grid direction in the literature, where communication-enabled metering improves transparency, user awareness, and utility visibility [1], [4], [6]. Firmware behaviour is first verified in Wokwi and then confirmed on a physical prototype so that the interaction between measurement, billing, user feedback, and relay switching can be validated before deployment.

## 1.2 Statement of the Problem

The study is driven by the following problems:

1. Conventional postpaid metering allows electricity to be consumed before payment is made, creating debt risk and weak revenue assurance for utilities. In practice, the utility carries the risk of non-payment after energy has already been delivered, while the consumer may receive a bill long after the actual usage event. The delay makes enforcement expensive and often leads to disputes when estimated charges do not match actual consumption.
2. Many consumers do not have real-time visibility of their energy use or remaining credit, which makes planning and dispute prevention difficult. Without immediate feedback, users cannot tell whether a heavy appliance is consuming too much power or whether their available balance is approaching zero, so they are forced to react only after a cutoff or a bill arrives.
3. Manual meter reading and field disconnection are slow, costly, and prone to human error. Field agents must be deployed repeatedly, which increases labour cost, exposes the process to transcription mistakes, and makes service enforcement dependent on physical access to premises.
4. Some low-cost prototypes measure energy but do not integrate billing, load control, persistence, and IoT visibility into one working system. A system that only displays readings is incomplete for prepaid use because the meter must also compute charges, store state, react to credit depletion, and communicate status to the user or utility platform.
5. Recharge and control processes are often not protected by reliable state storage, so power interruptions can lead to lost balance information. When credit values or relay state are lost after a reset, the meter becomes unreliable and may either disconnect unfairly or restore supply without valid credit, both of which reduce trust in the system.
6. Many embedded prototypes still stop at measurement or demonstration level and do not show a complete consumption-to-payment workflow. For a prepaid meter to be useful, it must connect the measured load, the remaining credit, the relay state, the recharge process, and the user-facing feedback loop as one consistent operation.

## 1.3 Objectives of the Study

The general objective of this study is to design and implement an IoT-based smart prepaid energy meter that measures electricity consumption in real time, deducts prepaid credit accurately, controls the load automatically, and publishes meter status for remote monitoring.

The specific objectives are:

1. To design a microcontroller-based metering circuit that reads voltage, current, power, and energy values from the PZEM-004T module so that billing is based on measured consumption rather than estimated usage.
2. To implement a prepaid billing engine that deducts credit in proportion to measured energy usage so that the consumer's balance changes in line with actual load demand.
3. To develop an automatic relay control function that disconnects the load when credit reaches zero and restores supply after recharge so that payment status is enforced without manual intervention.
4. To implement a token-based recharge workflow that supports secure credit loading and rejects invalid or reused tokens so that recharge transactions remain meter-specific and resistant to replay.
5. To publish meter readings and status data through the ESP32 Wi-Fi interface for IoT monitoring so that users and utility operators can observe system behaviour remotely.
6. To persist key meter state variables such as credit balance, cumulative energy, relay status, and used token records across power cycles so that the meter remains consistent after interruption or reset.
7. To validate the complete firmware and hardware workflow in Wokwi and on bench hardware before physical deployment so that design errors are detected early before field resources are committed.

## 1.4 Research Questions

In order to guide the study, the following research questions are proposed:

1. How can a microcontroller-based embedded system accurately measure real-time AC voltage, current, active power, and cumulative energy using a dedicated energy meter module?
2. What billing logic best ensures accurate and tamper-resistant deduction of prepaid credit in a smart prepaid meter?
3. How can a token-based recharge workflow be implemented so that valid credit is accepted and invalid or reused tokens are rejected?
4. How can the relay control, display, state storage, and IoT communication modules operate together without breaking the prepaid enforcement logic?
5. To what extent can Wokwi-based firmware validation and bench testing confirm the interaction of the firmware, measurement module, display, relay, and communication functions before physical deployment?

## 1.5 Methodology

The project adopts an iterative prototyping methodology supported by modular object-oriented firmware design. This approach suits embedded system development because it allows the work to progress in stages, with each stage tested before the next one begins [3], [6]. The choice of methodology is appropriate because the system combines sensing, billing, display output, relay switching, persistence, and communication, all of which need to be verified individually before they can be trusted as a complete system.

Iterative prototyping also fits the implementation-driven nature of the project. Instead of waiting until the full system is complete before testing, each module can be integrated and checked as soon as it is implemented. That reduces the risk of building a large firmware base with hidden faults, and it makes it easier to trace errors when a change in one module affects another. For an embedded prepaid meter, this is valuable because logic errors in billing or relay control can directly affect the correctness of the prototype.

The methodology follows these steps:

1. Requirements analysis: identify the functional needs of prepaid billing, measurement, relay control, display, persistence, and IoT communication.
2. System design: divide the solution into measurement, billing, recharge, communication, storage, and control modules.
3. Hardware selection: choose the ESP32, PZEM-004T, TFT display, relay, push buttons, and supporting power circuitry.
4. Firmware development: implement each function as a separate module using the Arduino framework and object-oriented C++ structure.
5. Firmware validation: verify the modules in Wokwi and then confirm signal flow, control logic, and user interaction on the physical prototype.
6. Functional testing: test normal operation, recharge events, zero-credit disconnection, and power-cycle recovery.
7. Refinement: adjust the logic where needed until the prototype behaves consistently and accurately.

This lifecycle is appropriate for the study because it supports continuous verification of the system while preserving the modular structure required for an embedded IoT prototype [4], [7]. It also supports a practical embedded development style where user feedback, timing behaviour, and data persistence can be checked early instead of being discovered only at the end of implementation.

## 1.6 Materials Used in the Study

The project uses both hardware and software materials.

The hardware materials provide the physical sensing, control, display, and power functions, while the software materials provide the logic, validation, communication, and state management required to make the prototype operational. Together, they form the complete development stack for the prepaid meter.

### 1.6.1 Hardware Materials

1. ESP32 microcontroller board, which serves as the control center for measurement processing, billing logic, display updates, and Wi-Fi communication.
2. PZEM-004T energy measurement module, which provides voltage, current, power, and energy data through serial communication.
3. 1.8-inch TFT LCD display, which gives the consumer immediate visual feedback on balance, usage, and system status.
4. Single-channel relay module, which acts as the electrical switching interface for disconnecting or restoring the load.
5. Push buttons for user input, which are used for local interaction such as navigation and recharge confirmation.
6. Regulated 5V power supply, which provides stable operating voltage for the control circuit and peripheral modules.
7. AC test load and wiring components used for bench validation, which make it possible to validate the load control behaviour before deployment.

### 1.6.2 Software Materials

1. Wokwi, which is used to simulate the ESP32 firmware, test peripheral interactions, and verify basic control logic before hardware testing.
2. Arduino IDE, which is used to write, compile, and upload the embedded firmware.
3. Arduino ESP32 core, which provides the board support package, libraries, and build tools for ESP32 development.
4. Preferences library for non-volatile storage, which keeps balance and configuration data available after power interruption.
5. TFT display library, which handles drawing text and status indicators on the screen.
6. Serial communication and Modbus handling libraries, which support data exchange with the PZEM-004T module.
7. IoT dashboard tools for data publication and monitoring, which are used to represent the remote visibility component of the system.

## 1.7 Expected Result

The expected result of the study is a working prototype and firmware-validated test model that:

1. Measures AC energy parameters in real time.
2. Deducts prepaid balance accurately from measured usage.
3. Disconnects the load automatically when the balance is exhausted.
4. Accepts valid recharge tokens and restores supply.
5. Stores critical state safely across resets and power interruptions.
6. Displays useful information clearly on the local screen.
7. Publishes meter data for remote IoT monitoring.

In practical terms, the prototype should behave like a functional prepaid meter rather than a simple display unit. It should show current readings, update the available balance continuously, issue warnings before credit reaches zero, and disconnect the load only when the credit limit is actually exhausted. After recharge, it should restore service without losing prior records or requiring manual recalibration.

## 1.8 Scope of the Study

The study is limited to a single-phase prepaid energy meter prototype designed and tested through firmware validation and bench integration. It covers measurement, billing, token handling, relay control, display feedback, and IoT communication logic. The system is targeted at a typical low-voltage single-phase residential or small commercial load environment, where accurate consumption tracking and automatic credit enforcement are most useful.

The scope also includes the use of the ESP32 as the central processing unit, the PZEM-004T as the measurement source, and the TFT display as the consumer interface. The Wi-Fi function is treated as an IoT reporting layer for status visibility, while the firmware logic is verified in Wokwi and the network behaviour is confirmed during bench testing and live platform integration.

It does not include three-phase billing, full commercial token server deployment, enclosure fabrication, or field installation. It also does not attempt to model every field condition such as power quality disturbances, RF interference, or long-term hardware ageing, because those aspects are outside the firmware-validation focus of this project.

## 1.9 Limitations of the Study

The conduct of this study is subject to the following constraints and limitations:

1. Firmware validation environment constraints. Wokwi does not reproduce every physical phenomenon present in real hardware. AC mains behaviour, sensor tolerances, relay contact wear, electromagnetic interference, and supply voltage variation are handled later during bench testing, so the firmware may appear more ideal in the validation environment than in the final prototype.
2. Wi-Fi and cloud integration constraints. Wokwi is useful for checking control flow, serial output, and network-request logic, but the final Wi-Fi and cloud behaviour still needs confirmation on the real ESP32 hardware with the chosen IoT platform.
3. Token handling simplification. A simplified token model is used to validate recharge flow, meter identity checking, and replay rejection without implementing a full cryptographic token generation system.
4. Single-phase operation only. The design addresses single-phase residential and light commercial loads. Industrial and large commercial consumers operating on three-phase supply are outside the scope of this study.
5. Resource constraints of the ESP32. Although the ESP32 has sufficient memory for the prototype, the concurrent execution of Wi-Fi communication, display rendering, serial communication, and billing computation still requires careful memory and timing management.

## 1.10 Significance of the Study

The study is significant because it demonstrates how low-cost embedded components can be combined into a practical prepaid metering solution. It also shows how smart metering can support billing transparency, consumer awareness, and better operational control in line with the broader smart grid direction described in the literature [1], [2], [4].

For the electricity sector, the work shows a prototype path toward reducing estimated billing, improving revenue assurance, and enabling clearer consumption tracking. For consumers, it demonstrates a mechanism for seeing and controlling energy spending in real time, which strengthens trust in the billing process and encourages more disciplined usage [2], [5].

For academic and technical development, the study contributes a modular embedded systems design that combines measurement, state persistence, user interface, and IoT communication in one prototype. It also provides a firmware-validation and bench-testing model that can be reused or extended by future researchers working on smart metering, prepaid energy systems, or related IoT applications [4], [6], [7].

## 1.11 Definition of Terms

The following terms are defined as used within the context of this study:

ESP32: A low-power dual-core microcontroller system-on-chip used as the main control unit for sensing, billing, switching, and Wi-Fi communication.

PZEM-004T: A dedicated energy measurement module that provides voltage, current, power, and energy data through serial communication.

Prepaid Meter: A meter that deducts payment before or during consumption, and disconnects the load when credit is exhausted.

Smart Meter: An electronic meter that records consumption and supports communication, monitoring, and automated control.

IoT: A system in which devices sense, communicate, and act over a network with minimal human intervention.

Relay: A switching device used to connect or disconnect the consumer load based on control signals from the microcontroller.

Wokwi: Browser-based ESP32 simulation and firmware validation platform used to test logic, peripheral interactions, and control flow before physical implementation.

Preferences Library: A non-volatile storage library used on the ESP32 to retain critical meter state values after power loss.

Token-Based Recharge: A recharge method in which valid credit is entered into the meter using a token or code.

## 1.5 Methodology

## 1.12 References

[1] Amin, M., & Wollenberg, B. F. (2005). Toward a smart grid: Power delivery for the 21st century. IEEE Power and Energy Magazine, 3(5), 34-41. https://doi.org/10.1109/MPAE.2005.1507024

[2] Depuru, S. S. S. R., Wang, L., & Devabhaktuni, V. (2011). Smart meters for power grid: Challenges, issues, advantages and status. Renewable and Sustainable Energy Reviews, 15(6), 2736-2742. https://doi.org/10.1016/j.rser.2011.02.039

[3] Gungor, V. C., Sahin, D., Kocak, T., Ergut, S., Buccella, C., Cecati, C., & Hancke, G. P. (2011). Smart grid technologies: Communication technologies and standards. IEEE Transactions on Industrial Informatics, 7(4), 529-539. https://ieeexplore.ieee.org/document/6011696

[4] Fang, X., Misra, S., Xue, G., & Yang, D. (2012). Smart grid - The new and improved power grid: A survey. IEEE Communications Surveys & Tutorials, 14(4), 944-980. https://ieeexplore.ieee.org/document/6099519

[5] Palensky, P., & Dietrich, D. (2011). Demand side management: Demand response, intelligent energy systems, and smart loads. IEEE Transactions on Industrial Informatics, 7(3), 381-388. https://doi.org/10.1109/TII.2011.2143734

[6] Al-Fuqaha, A., Guizani, M., Mohammadi, M., Aledhari, M., & Ayyash, M. (2015). Internet of Things: A survey on enabling technologies, protocols, and applications. IEEE Communications Surveys & Tutorials, 17(4), 2347-2376. https://doi.org/10.1109/COMST.2015.2444095

[7] Davis, F. D. (1989). Perceived usefulness, perceived ease of use, and user acceptance of information technology. MIS Quarterly, 13(3), 319-340. https://www.jstor.org/stable/249008