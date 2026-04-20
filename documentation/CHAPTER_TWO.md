# CHAPTER TWO

# LITERATURE REVIEW

## 2.1 Conceptual Framework

### 2.1.1 Overview of the Conceptual Framework

The conceptual framework for this study explains how measurement, billing, communication, persistence, and control interact inside an IoT-based smart prepaid energy meter. In the literature, smart metering is described as a shift from passive consumption recording to intelligent sensing and actuation, where the meter becomes part of the wider smart grid communication ecosystem [1], [2], [4]. This makes the meter a cyber-physical device rather than a simple measuring instrument.

The study is built around five linked concepts: energy measurement, prepaid billing, token-based recharge, load control, and IoT visibility. These concepts are interconnected because measured energy must be converted into credit deduction, the remaining credit must determine relay status, and system state must be communicated reliably to the user and the monitoring platform [2], [3], [5]. In other words, the meter is only useful when the measurement layer, the decision layer, the storage layer, and the communication layer all work together without conflict.

This framework also reflects the practical needs of prepaid utility systems. The consumer must see what is happening, the controller must know when to disconnect the load, the storage layer must preserve credit after reset, and the communication layer must make the meter visible to the outside world. For that reason, the conceptual design is not limited to energy sensing alone but extends to the full consumption-to-payment workflow [4], [5], [6].

### 2.1.2 Concept of Smart Metering

Smart metering refers to digital metering systems that support interval measurement, event logging, and two-way communication between the meter and external systems [1], [2]. Unlike conventional meters, smart meters are expected to supply not only cumulative energy readings but also operational data that can be used for billing, monitoring, and demand-side management [4], [5].

The literature also shows that smart metering contributes to better grid visibility, improved load monitoring, and more efficient utility operations [1], [4]. In practice, that means smart meters help utilities understand how energy is being used, when demand peaks occur, and how consumer behaviour changes over time. A meter that only stores total kWh is useful for billing, but a smart meter that also exposes state and status data can support broader utility planning and user engagement.

In this project, smart metering is implemented through the ESP32 controller and the PZEM-004T module, which together form the measurement and control core of the prototype. The ESP32 handles logic and communication while the PZEM-004T provides the measured electrical quantities, allowing the prototype to focus on billing enforcement and status feedback rather than raw analog signal processing.

### 2.1.3 Concept of Prepaid Energy Metering

Prepaid metering is a billing architecture in which credit is purchased before electricity is consumed. The main advantage of this approach is that it changes the billing relationship from post-consumption debt recovery to real-time credit enforcement [2], [5], [6]. In practical terms, the meter continuously compares measured usage with available credit and disconnects the load when the balance reaches zero.

This model has strong relevance for consumer fairness and utility revenue protection because it reduces debt accumulation and provides clearer consumption-cost feedback [2], [6]. It also changes user behaviour by making the cost of consumption visible at the moment energy is used rather than after a delayed invoice arrives. That immediate feedback is one reason prepaid systems are often easier to understand and easier to manage.

The logic used in this study follows that same principle: when credit is available, the load remains connected; when credit is exhausted, the relay opens the circuit until recharge occurs. The prepaid meter therefore acts as both a measuring device and a payment gatekeeper, ensuring that electricity delivery is tied directly to available balance.

### 2.1.4 Concept of IoT-Enabled Metering

IoT-enabled metering extends the meter into a connected data node. IoT literature describes this as a distributed architecture where devices sense, transmit, receive, and act with minimal human intervention [7], [8], [9]. For utility systems, this means that readings can be published to a dashboard, alerts can be generated automatically, and remote configuration can be supported where communication infrastructure is available [7], [9], [12].

The IoT concept is important here because modern metering is no longer only about local display. It is also about remote visibility, system integration, and the possibility of using live data for decisions. A meter that reports to a dashboard can support better monitoring, faster fault detection, and more transparent consumer communication. In a prepaid environment, that visibility becomes even more important because users want to know when their balance changes and when the meter state changes.

This study adopts that conceptual model by using ESP32 Wi-Fi connectivity for remote reporting and simulated command reception. The value of this design is not only communication convenience but also the possibility of monitoring consumption trends and recharge events in near real time [8], [12]. The remote layer is therefore treated as an extension of the meter's control and information function, not as a separate optional feature.

### 2.1.5 Concept of Measurement, Billing Variables, and State Persistence

Metering accuracy depends on how well electrical values are transformed into billing variables. The literature on smart metering and smart grid communication shows that accurate and structured data handling is essential because billing logic depends on trustworthy measurement input [2], [3], [4]. If the measurement input is unstable or badly interpreted, the resulting bill loses credibility even when the hardware appears to be functioning.

For a prepaid system, the critical state variables are voltage, current, active power, cumulative energy, tariff, remaining credit, relay state, and used token history. If these variables are not preserved across resets, the user may lose trust in the billing process and the utility may lose auditability [2], [11]. This is why persistence is treated in this study as part of the conceptual design rather than as an optional feature. The study therefore assumes that balance storage is not a back-end detail but a core part of meter reliability.

State persistence also supports recovery after interruption. If power fails in the middle of use, the meter must return with a believable state, not a random or default state that breaks the billing record. That is especially important for prepaid meters because the consumer expects continuity between the last valid balance and the restored meter state.

### 2.1.6 Concept of User Interface and Consumer Engagement

Smart prepaid systems are consumer-facing devices, so the user interface matters. Research on technology adoption shows that people are more likely to accept a system when it is useful, understandable, and easy to use [13], [14], [15]. In metering applications, this means the display should clearly show balance, usage, and system status so that the consumer can interpret the meter's behaviour without specialist knowledge.

The interface has a direct effect on trust. If a user can see the remaining credit, current consumption, and disconnect status, the meter becomes easier to understand and less likely to be seen as arbitrary. For a prepaid system, that clarity matters because the user needs immediate confirmation when recharge is successful and immediate warning when balance is low.

For this reason, the local display in this project is treated as a functional control layer, not just an accessory. It supports trust, visibility, and faster recharge decisions. The display also bridges the gap between technical measurement output and the simple information that ordinary consumers actually need.

## 2.2 Theoretical Framework

### 2.2.1 Systems Theory

Systems Theory views the meter as a set of interacting subsystems that must function together as one unit [1], [4]. The measurement module, billing logic, storage module, relay controller, display, and communication service each have separate roles, but the system succeeds only when their interactions are consistent.

Applied to this study, Systems Theory explains why modularity is important. If measurement works but storage fails, the balance is unreliable. If billing works but relay control fails, payment enforcement is broken. If communication fails but local control works, the system is still partly useful but loses IoT visibility. The literature supports this kind of layered design because smart grid components are most effective when they are connected through defined interfaces [3], [4], [9].

The theory also helps explain fault tolerance. A modular meter can continue operating when one subsystem needs adjustment, provided the interfaces remain stable. This is especially useful in embedded development because measurement, display, and communication often evolve at different speeds, but they still need to appear as one coherent product to the user.

### 2.2.2 Cyber-Physical Systems Theory

Cyber-Physical Systems theory describes systems that combine computation, communication, and physical actuation [3], [7], [9]. A smart prepaid meter is a clear CPS example because it senses electrical behaviour, computes billing decisions, communicates state, and controls a physical relay.

This theory is relevant to the present work because it highlights timing and state consistency. Billing updates must be synchronized with measurement intervals, and relay actions must reflect the latest credit state. The literature on smart grid communication and IoT architecture shows that such systems depend on predictable control flow and reliable data handling [3], [7], [12].

The CPS perspective also shows that the meter is not only software or only hardware. It is a coordinated loop where physical consumption affects the software state, and the software state determines the physical response. That is why this project treats sensing, decision making, communication, and switching as a single operational pipeline.

### 2.2.3 Control Theory and Feedback Regulation

Control Theory provides a useful way to explain prepaid credit enforcement. The meter can be seen as a discrete feedback system in which energy use is the disturbance input, credit is the controlled state, and relay switching is the control output [5], [6].

The feedback loop is straightforward: measure energy, compute cost, reduce credit, compare the balance against a threshold, and disconnect the load when the threshold is crossed. This approach is consistent with the literature on demand-side management and smart loads, where automated control is used to regulate energy use according to system rules [5], [6].

In a practical sense, the control theory view justifies the use of thresholds and state checks. The low balance warning is an early control signal, while the zero credit disconnection is the hard enforcement signal. That structure makes the meter easier to understand and helps avoid sudden behaviour that the user may interpret as a fault.

### 2.2.4 Information Reliability and Smart Grid Communication

Communication reliability is a major concern in smart grid systems because data must be transmitted without corrupting the local control state [3], [7], [12]. Smart grid communication studies show that the system design must separate operational control from external communication so that a network interruption does not damage local billing or relay logic [3], [11].

This separation matters because utility communication is often vulnerable to delay, packet loss, or temporary unavailability. A meter that depends completely on the network would become unusable whenever the link fails, which is not acceptable for a prepaid control device. The communication function therefore needs to be supportive rather than controlling.

This principle guides the current study. The meter should continue to measure and enforce credit even if cloud communication is interrupted, while still publishing data when the channel becomes available again. In that way, the IoT layer adds visibility without weakening the local meter logic.

### 2.2.5 Technology Acceptance Perspective

Technology Acceptance research explains why users adopt some digital systems more readily than others. The classic model focuses on perceived usefulness and perceived ease of use [13], while later extensions emphasize performance expectancy, effort expectancy, social influence, and facilitating conditions [14], [15].

For prepaid metering, the implication is clear: if the consumer sees transparent balance updates, clear recharge feedback, and simple interaction steps, the system is more likely to be accepted. This is why the study includes a local display and straightforward recharge flow rather than relying only on hidden backend logic [13], [15].

The theory is important because technical correctness alone does not guarantee adoption. A consumer may reject a meter that is accurate but hard to understand, or distrust a meter that hides its billing state. The design therefore needs to be not only functional but also understandable, visible, and easy to operate.

## 2.3 Empirical Studies

### 2.3.1 Global Evidence on Smart Metering

Early and later smart grid literature consistently shows that smart metering improves grid visibility, supports remote reading, and reduces dependence on manual processes [1], [2], [4]. Depuru et al. [2] emphasize that smart meters bring operational advantages but also require careful attention to reliability, interoperability, and consumer trust. Fang et al. [4] extend this by showing that smart metering is part of a broader grid modernization pathway rather than a standalone upgrade.

The empirical direction from these studies is that smart metering is most effective when it combines communication, data handling, and user-facing transparency. This supports the architecture used in the present study. It also shows that smart meter projects should be evaluated at the system level, not just by the accuracy of the measuring component alone.

Another important lesson from the global literature is that meter modernization is linked to grid modernization. A smart meter becomes useful when it can be integrated into broader management practices such as remote reading, demand analysis, and consumer communication. This is why the current work treats the meter as both a local device and a networked utility asset.

### 2.3.2 Empirical Evidence on Prepaid Billing and Demand Management

The literature on demand-side management and smart loads shows that systems can regulate consumption more effectively when control is tied to a measurable state variable such as credit or demand threshold [5], [6]. In prepaid metering, this is translated into automatic enforcement of payment status rather than delayed billing recovery.

The practical implication is that prepaid logic reduces the billing lag present in postpaid systems and gives the consumer immediate feedback on the cost of use [2], [5]. That is why prepaid metering remains a strong design choice for low-trust or revenue-sensitive utility contexts.

The empirical significance here is that prepaid billing works not only because it collects money earlier, but because it changes the relationship between consumption and accountability. When the user can see the balance dropping, consumption decisions become more deliberate. That visible feedback is part of the behavioural value of prepaid systems, not just their financial value.

### 2.3.3 Empirical Evidence on IoT Metering Architectures

IoT survey literature shows that connected devices are now commonly used for telemetry, automation, and remote interaction across many domains [7], [8], [9], [10]. In smart metering, this means that meter data can be published to dashboards or integrated into broader monitoring systems [8], [12].

Atzori et al. [10] and Gubbi et al. [9] describe the IoT as an ecosystem of sensing, communication, and service layers. That view aligns closely with the meter prototype in this study, where the ESP32 provides communication capability and the firmware coordinates local logic with external reporting.

The literature also suggests that IoT value is strongest when the data produced by a device can be acted on. A consumption reading becomes more valuable when it supports alerts, history tracking, remote visibility, or control decisions. That is why the communication layer in this study is tied to status publishing and recharge reporting rather than being treated as a decorative feature.

### 2.3.4 Empirical Evidence on Industrial and Utility IoT

Xu, He, and Li [12] show that industrial IoT applications depend on reliable sensing, communication, and system integration. Their findings are relevant to metering because an energy meter is effectively an industrial monitoring device operating at utility edge level.

This evidence supports the decision to treat communication and control as part of one firmware architecture. A meter that sends data but cannot preserve state is not sufficiently robust for practical use. The industrial IoT literature suggests that the design must protect local operation even when connectivity is intermittent [12].

For this project, that means the meter should be able to continue its main billing and control tasks even when the remote reporting layer is unavailable. This is a practical engineering requirement because utility systems often operate in environments where network conditions are not guaranteed at all times.

### 2.3.5 Empirical Evidence on Security, Privacy, and Trust

Security and privacy are repeatedly identified as central issues in smart grid and smart metering systems [11]. McDaniel and McLaughlin [11] note that connected energy systems must defend both data integrity and operational reliability. While the present project is not a full cryptographic implementation, the literature still justifies replay prevention, token validation, and controlled access to recharge functions.

This is important because prepaid systems can fail operationally if tokens are reused, invalid credit is accepted, or state records are manipulated. The system therefore benefits from a basic trust model even in simulation.

Trust in a smart prepaid meter is not only about preventing attack. It is also about preventing accidental inconsistency after reset, communication loss, or user error. The security dimension therefore overlaps with state integrity, and both are necessary if the meter is to be perceived as dependable.

### 2.3.6 Empirical Evidence on User Acceptance and Consumer Trust

Technology acceptance studies consistently show that perceived usefulness and ease of use are strong predictors of adoption [13], [14], [15]. In a metering context, consumers are more likely to trust the system when deduction is visible, recharge is simple, and warnings are clear.

Davis [13] established the core usefulness-ease-of-use relationship, while Venkatesh et al. [14], [15] extended the model by showing that adoption is also shaped by performance expectancy and facilitating conditions. These findings support the inclusion of a local display and simple recharge workflow in the present design.

The empirical implication is that a technically sound meter can still fail socially if users do not understand it. This is one reason consumer-facing feedback is given a central role in the prototype. When the meter explains itself clearly, the likelihood of acceptance increases.

### 2.3.7 Empirical Synthesis of Research Gaps

The reviewed literature reveals several gaps that this study addresses:

1. Many smart metering studies focus on communication or measurement alone, but not on the full prepaid workflow [2], [4], [7].
2. Several IoT studies explain connectivity well but give less attention to local control and persistence [9], [10], [12].
3. Security discussions often stress vulnerability without showing a simple prototype-level recharge protection mechanism [11].
4. Adoption studies show the importance of usability, but many metering prototypes still neglect the consumer display and feedback layer [13], [15].

The present project responds to these gaps by integrating measurement, billing, token handling, relay control, persistence, and IoT communication into one implementation-oriented prototype. It also aims to show how the same device can serve three roles at once: a measurement instrument, a payment enforcement mechanism, and a consumer information tool.

By combining these functions in one embedded design, the study moves beyond narrow single-function prototypes and presents a more realistic view of what a practical prepaid smart meter should do. That broader integration is the main contribution of the work.

### 2.3.8 Chapter Summary

This chapter has reviewed the conceptual, theoretical, and empirical literature relevant to the design of an IoT-based smart prepaid energy meter. The conceptual review established the links between smart metering, prepaid billing, IoT communication, measurement variables, and consumer interaction. The theoretical review showed that Systems Theory, Cyber-Physical Systems Theory, Control Theory, and Technology Acceptance Theory all support the project design. The empirical review demonstrated that smart metering, prepaid control, and IoT connectivity are well-supported in the literature, while also revealing practical gaps in persistence, user feedback, and prototype-level integration.

The literature therefore supports the central argument of this study: a modular ESP32-based prepaid energy meter with firmware validation and bench testing is both technically justified and academically relevant. It also shows that the value of the project lies not just in measurement accuracy, but in the complete chain from sensing to billing to enforcement to user feedback.

## References

[1] Amin, M., & Wollenberg, B. F. (2005). Toward a smart grid: Power delivery for the 21st century. IEEE Power and Energy Magazine, 3(5), 34-41. https://doi.org/10.1109/MPAE.2005.1507024

[2] Depuru, S. S. S. R., Wang, L., & Devabhaktuni, V. (2011). Smart meters for power grid: Challenges, issues, advantages and status. Renewable and Sustainable Energy Reviews, 15(6), 2736-2742. https://doi.org/10.1016/j.rser.2011.02.039

[3] Gungor, V. C., Sahin, D., Kocak, T., Ergut, S., Buccella, C., Cecati, C., & Hancke, G. P. (2011). Smart grid technologies: Communication technologies and standards. IEEE Transactions on Industrial Informatics, 7(4), 529-539. https://ieeexplore.ieee.org/document/6011696

[4] Fang, X., Misra, S., Xue, G., & Yang, D. (2012). Smart grid - The new and improved power grid: A survey. IEEE Communications Surveys & Tutorials, 14(4), 944-980. https://ieeexplore.ieee.org/document/6099519

[5] Palensky, P., & Dietrich, D. (2011). Demand side management: Demand response, intelligent energy systems, and smart loads. IEEE Transactions on Industrial Informatics, 7(3), 381-388. https://doi.org/10.1109/TII.2011.2143734

[6] Siano, P. (2014). Demand response and smart grids - A survey. Renewable and Sustainable Energy Reviews, 30, 461-478. https://doi.org/10.1016/j.rser.2013.10.022

[7] Al-Fuqaha, A., Guizani, M., Mohammadi, M., Aledhari, M., & Ayyash, M. (2015). Internet of Things: A survey on enabling technologies, protocols, and applications. IEEE Communications Surveys & Tutorials, 17(4), 2347-2376. https://doi.org/10.1109/COMST.2015.2444095

[8] Zanella, A., Bui, N., Castellani, A., Vangelista, L., & Zorzi, M. (2014). Internet of Things for smart cities. IEEE Internet of Things Journal, 1(1), 22-32. https://doi.org/10.1109/JIOT.2014.2306328

[9] Gubbi, J., Buyya, R., Marusic, S., & Palaniswami, M. (2013). Internet of Things (IoT): A vision, architectural elements, and future directions. Future Generation Computer Systems, 29(7), 1645-1660. https://doi.org/10.1016/j.future.2013.01.010

[10] Atzori, L., Iera, A., & Morabito, G. (2010). The Internet of Things: A survey. Computer Networks, 54(15), 2787-2805. https://doi.org/10.1016/j.comnet.2010.05.010

[11] McDaniel, P., & McLaughlin, S. (2009). Security and privacy challenges in the smart grid. IEEE Security & Privacy, 7(3), 75-77. https://doi.org/10.1109/MSP.2009.76

[12] Xu, L. D., He, W., & Li, S. (2014). Internet of Things in industries: A survey. IEEE Transactions on Industrial Informatics, 10(4), 2233-2243. https://doi.org/10.1109/TII.2014.2300753

[13] Davis, F. D. (1989). Perceived usefulness, perceived ease of use, and user acceptance of information technology. MIS Quarterly, 13(3), 319-340. https://www.jstor.org/stable/249008

[14] Venkatesh, V., Morris, M. G., Davis, G. B., & Davis, F. D. (2003). User acceptance of information technology: Toward a unified view. MIS Quarterly, 27(3), 425-478. https://www.jstor.org/stable/30036540

[15] Venkatesh, V., Thong, J. Y. L., & Xu, X. (2012). Consumer acceptance and use of information technology: Extending the unified theory of acceptance and use of technology. MIS Quarterly, 36(1), 157-178. https://www.jstor.org/stable/41410412