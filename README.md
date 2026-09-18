# DIY Benchtop Power Supply

This is a project of designing and assembling a custom laboratory benchtop power supply built inside a repurposed Corsair ATX power supply enclosure[cite: 1]. It features an adjustable CNC 0–50V / 5A output stage[cite: 1], auxiliary 5V logic rails[cite: 1], dual USB ports[cite: 1], and an autonomous MOSFET temperature-controlled cooling system[cite: 1].

This is a very fun project in many ways, and it is intended for:
* Electronics and robotics hobbyists looking for a reliable workbench power unit without spending a fortune.
* Makers wanting to learn how to safely bridge high-voltage AC mains to isolated low-voltage DC rails[cite: 1].
* Everyone with an old PC power supply unit (PSU) and the motivation to create something cool :D[cite: 1].

<p align="center">
  <img src="docs/images/banner.jpg" width="750">
</p>

---

## Readme Structure

* [What are the necessary components?](#what-are-the-necessary-components)
  * [3D Printed & Mechanical Parts](#3d-printed--mechanical-parts)
  * [Hardware & Tools](#hardware--tools)
  * [Electrical Components](#electrical-components)
* [Connecting All the Components](#connecting-all-the-components)
  * [1. Mains AC Input & Primary Stage](#1-mains-ac-input--primary-stage)
  * [2. Primary Regulated Output (0–50V)](#2-primary-regulated-output-050v)
  * [3. Intermediate 12V Bus](#3-intermediate-12v-bus)
  * [4. Temperature-Controlled Fan Circuit](#4-temperature-controlled-fan-circuit)
  * [5. Fixed 5V Rail & USB Outputs](#5-fixed-5v-rail--usb-outputs)
* [Safety Guidelines](#safety-guidelines)
* [License](#license)

---

## What are the necessary components?

Due to the modular approach and the reuse of standard off-the-shelf step-down modules, this is one of the cleanest hardware setups for a DIY bench supply.

### 3D Printed & Mechanical Parts
* **Enclosure:** Old Corsair ATX power supply case[cite: 1]. The original steel chassis provides rigid physical protection, integrated ventilation cutouts, and chassis grounding[cite: 1, 2].
* **Front & Rear Panels:** 3D-printed faceplates (or CNC cut plywood) designed to mount into the original cable-exit opening of the PSU[cite: 1].
  * *Filament choice:* PETG or ABS/ASA is recommended for heat resistance near internal heatsinks, though PLA works fine if airflow is kept decoupled.
  * *Print settings:* 4 perimeters/walls, 30–40% infill to firmly support tightening binding post nuts.

### Hardware & Tools
* **Soldering Station:** Soldering iron and rosin-core solder wire[cite: 1].
* **Fastening:** Hot glue gun (for wire securing / module standoffs)[cite: 1], heat-shrink tubing, and M3 hardware/standoffs.
* **Prototyping:** Perfboard / stripboard for soldering discrete components (fan circuit & 5V regulator)[cite: 1].
* **Wiring:** 
  * `14 AWG`: High-current DC path (36V input / 50V output)[cite: 1].
  * `20 AWG`: Auxiliary power distribution (12V & 5V rails)[cite: 1].
  * `32 AWG`: Low-power signals, LED indicators, and thermistor sense lines[cite: 1].

### Electrical Components

#### Power Modules
* **Primary SMPS:** 180W AC-to-DC step-down power supply board (`100–240V AC` input, `36V 5A DC` output)[cite: 1].
* **Main Adjustable DC-DC Module:** Numerical Control Step-Down module (`6–55V` input to `0–50V 5A` output) with digital display and keypad/encoder[cite: 1].
* **Intermediate Step-Down:** LM2596 DC-DC buck converter module (steps down `36V` to `12V`)[cite: 1].
* **USB Output Module:** 5V 3A dual USB step-down converter board[cite: 1].
* **Linear Regulator:** LM7805 5V linear voltage regulator in TO-220 package[cite: 1].

#### Cooling & Temperature Control
* **12V Fan:** Stock 12V brushless exhaust fan extracted directly from the Corsair PSU[cite: 1].
* **IRFZ44N N-Channel MOSFET:** Acts as a low-side ground switch for the 12V fan[cite: 1].
* **10k NTC Thermistor:** Attached to the primary SMPS heatsink to monitor operational temperature[cite: 1].
* **10k Variable Resistor:** Trimpot/potentiometer to fine-tune the gate trigger voltage and temperature threshold[cite: 1].

#### Switches, Passives & Connectors
* **Switches:** 
  * `1x` Mains rocker switch (220V AC input)[cite: 2].
  * `2x` Dual-position lever toggle switches (one for the 0–50V output rail, one for the 5V rail)[cite: 1].
* **Indicators:** 
  * Green LED (indicates active 36V intermediate power)[cite: 1, 2].
  * White LED (indicates active 5V output)[cite: 1, 2].
* **Capacitors:** `0.33 µF` (input) and `0.1 µF` (output) ceramic/film capacitors for LM7805 stabilization[cite: 1].
* **Terminals:** 4mm banana binding post pairs / DC output jacks[cite: 1].

---

## Connecting All the Components

The system architecture cleanly separates high-voltage input, variable high-power regulation, intermediate power distribution, and thermal management.

<p align="center">
  <img src="docs/schematics.jpg" width="750">
</p>

### 1. Mains AC Input & Primary Stage
* The `220V AC` input enters through an IEC socket and passes through an AC-rated toggle/rocker switch on the Line (L) lead[cite: 2].
* **Protective Earth (PE):** Must be bolted directly to the bare metal of the Corsair chassis with a star washer for shock safety[cite: 2].
* The switched AC lines feed into the **180W AC-DC Step-Down** module, producing an isolated `36V DC` rail[cite: 1, 2].
* A **Green LED** is connected across the 36V output (with an appropriate current-limiting resistor) to serve as the master DC power indicator[cite: 1, 2].

### 2. Primary Regulated Output (0–50V)
* The `36V` positive rail runs through the first dual-position toggle switch before reaching the input of the **Numerical Control 0–50V 5A module**[cite: 1, 2].
* This setup allows isolating and setting up your desired output parameters on the screen before physically enabling power to the front **0–50V Banana Terminals**[cite: 2].

### 3. Intermediate 12V Bus
* In parallel with the main output, the `36V` rail feeds the input of an **LM2596 buck converter**[cite: 2].
* Adjust the onboard trimpot of the LM2596 until its output reads exactly `12.0V DC`[cite: 2]. This creates the internal auxiliary power bus[cite: 2].

### 4. Temperature-Controlled Fan Circuit
* To keep the bench supply dead-silent during light loads, the Corsair 12V fan is not driven continuously[cite: 1].
* The **10k NTC thermistor** and the **10k variable resistor** form a voltage divider biasing the gate pin of the **IRFZ44N MOSFET**[cite: 1, 2]:
  * Drain (D) connects to the negative wire of the 12V fan[cite: 2].
  * Source (S) connects to Ground (GND)[cite: 2].
  * The positive wire of the 12V fan connects directly to the `+12V` intermediate rail[cite: 2].
* As temperatures inside the chassis rise, thermistor resistance drops, pulling the gate voltage past $V_{GS(th)}$ and kicking on the fan automatically[cite: 1, 2].

### 5. Fixed 5V Rail & USB Outputs
* The `+12V` intermediate bus supplies two independent 5V stages[cite: 2]:
  1. **Dual USB Board:** Stepped down directly from 12V to 5V (3A max) for powering microcontrollers or phone charging[cite: 1, 2].
  2. **LM7805 Linear Stage:** Stepped down from 12V with a `0.33 µF` cap across Pin 1 (IN) and GND, and a `0.1 µF` cap across Pin 3 (OUT) and GND to prevent high-frequency oscillations[cite: 1].
* The linear 5V line passes through the second **toggle switch** out to dedicated **5V binding posts**, accompanied by a **White LED** indicating rail activity[cite: 1, 2].

---

## Safety Guidelines

> ⚠️ **DANGER: HIGH VOLTAGE**  
> Mains AC voltage (`220V`) can cause severe electrical shock or death.
> * Always double-check that the AC power cord is unplugged before touching internal components.
> * Insulate all AC switch lugs and IEC terminals thoroughly with adhesive-lined heat-shrink tubing.
> * Ensure the metal enclosure is reliably grounded to the mains Earth wire (PE)[cite: 2].

---

## License

Distributed under the [MIT License](LICENSE). Feel free to build, remix, and use it for your laboratory bench!
