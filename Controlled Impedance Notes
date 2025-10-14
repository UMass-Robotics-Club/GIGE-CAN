# Ethernet PCB Notes: Controlled Impedance & Magnetics

### Controlled Impedance
**What is controlled impedance?**  
- Controlled impedance in printed circuit boards involves regulating the characteristic resistance (impedance) of signal traces, which ensures signal integrity at high frequencies.  
  [Source](https://www.wevolver.com/article/controlled-impedance)

#


### Why it matters
- Since we are using high speed signals, like Gigabit Ethernet, we have to use differential pasr (two traces carrying opposite signals) into the making of the PCB. Because if the impedance isn't controlled:
  - Signal Reflections would occur
    - Meaning the Ethernets data would be reflected back to the source and wouldn't be able to reach the PHY or anything else.
  - Data errors would increase
  - There would be more Electromagnetic Interference (EMI) would increase

#

### How to implement this to the PCB
- We gotta maintain a fixed distance between the **transmission lines** in parallel, based on the calculations from an [Ethernet Controlled Impedance Calculator](https://www.pcbway.com/pcb_prototype/impedance_calculator.html) 
- Controlled Impedance depends on:
  - Trace Width
  - Trace Spacing
  - PCB stackup and dielectric thickness
  - Copper Thickness
- Rough Rules of Thumb: 
  - Differential Spacing ~ 2x trace width (varies depending on the board parameters). We still gotta use the calc.
  - Ethernet targe: **100 Ω differential impedance** (±10%).  
  - Tools/Resources:
    - [Altium](https://resources.altium.com/p/pcb-stackup-impedance-calculator)

#

### Things to consider for the Ground Layer
- We should place a continous solid ground plane **under** the high speed differential pairs and under the PHY/ 
  - Note:
    - Some designs (honestly if we could do this it would be a little easier to just not worry about it) have a full bottm ground layer, which would maintain the consistent impedance and honestly is simplier.
- If we are going with the first option we gotta avoid splitting the ground plane under the differential pairs because it would ruion the signal integrity.

#

### How this interacts with Magnetics
- Normal Ethernet Chain: MAC -> Phy -> Magnetics (transformers + Common Mode Chokes) -> RJ45 -> Cable.
  - __Note for self:__
    - Common Mode: Noise or voltage that appears equally on both lines of a differntial pair.
    - Transformers (magnetics): They magnetically couple the differential signals while isolating--Galavantic Isolation--DC and common mode voltages between PHY and cable. 
  - Magnetis purpose:
    - Provide **galavanic isolation** between PHY and cable
    - Enable **Common Mode filtering** Which reduces the Electromagnetic interference (EMI)
    - Protect PHY/MCU from voltage surges and ground loops
      - Voltage surges: Sudden spikes in voltage, from lighting, static, or power switching that can damage circuits if not isolated or suppressed properly.
      - Ground loops: unwanted current paths formwed when to connected devices have different ground potentials, causing noise or interfence.
  - Differential pairs should be routed carefully up to the PHY and past it. The worries of sending high speed things are really localized and isolated once past the PHY and across magnetics to the RJ45.
  - Make sure to keep the magnetics + RJ45 close to the PHY for signal integrety and make sure to follow the manufacturer keepout / center-tap guide in the **PHYs Datasheet** -> **Magnetics Datasheet** -> **RJ45 pinout** in that order as they will talk about it in that order.

#


### Best Manufacturering Practices:
- Ensure to tell who ever we choose to manufacture it, that we have controlled impedance and stress the importance of having the differential pairs equally spaced to the difference we calculate.
- Manufacturers will ensure to verify those trace dimensions and even if the overall of the PCB gets a little messed up that our spacing of the transmission lines are equally spaced.
- This will make sure that we have proper signal integrity no matter what.

#

### What we can do in Altium
- What is most common and what we should proabbly do is have a 4-layer stackup when possible:
  1. Top: Signals (Differential Pairs)
  2. Layer 2: Ground Plane (Use the Differential Pairs as a Reference)
  3. Layer 3: Power Plane
  4. Bottom: Routing / Optional Ground layer
- Best practices for routing:
  - Keep PHY near magnetics/RJ45
  - Minamize the amount of vias we use
  - Route differential pairs symmetrically
- We also gotta:
  - Follow all the PHY/Magnetics datasheet notes when it comes to center-tap wiring and local keepout zones.
  - And we gotta avoid sensitive traces underneath the magnetics so that we can reduce the magnetic coupling/noise pickup when the circuit is running.

#

### A Reference if needed

      ┌────────────┐       ┌────────────┐       ┌────────────────┐       ┌──────────┐
      │    MAC     │──────▶│    PHY     │──────▶│   Magnetics    │──────▶│   RJ45  │
      └────────────┘       └────────────┘       └────────────────┘       └──────────┘
             │                   │                      │                     │
             │                   │ Differential Pairs   │                     │
             │                   │ 100 Ω Controlled     │                     │
             │                   │ Impedance Routing    │                     │
             ▼                   ▼                      ▼                     ▼
    ────────────────────────────────────────────────────────────────────────────────
    Continuous Solid Ground Plane (Reference for Differential Pairs & PHY)
    ────────────────────────────────────────────────────────────────────────────────


