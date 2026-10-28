# Kicad_project4_On-delay-timer
This Project gives a On-Delay Timer 2 layer PCB in KiCad using an NE555 timer in monostable mode to process low-voltage DC signals and create a designated time delay. The circuit uses a transistor to safely drive an relay for switching high-power AC/DC loads, protected by a flyback diode against spikes, with LED for visual timing status.

### Features:
Timing & Logic Architecture: NE555 timer IC configured in monostable multivibrator mode, utilizing an RC network (R1, C1) to generate precise on-delay pulse timing.
Transistor Driver & Inductive Protection: NPN transistor (BC547) low-side switch for relay coil actuation, paired with a anti-parallel flyback diode (D2) across the coil to clamp inductive back-EMF voltage spikes (V = L*(di/dt)).

Galvanic Load Switching: Onboard SPDT relay (K1) coupled to a 3-pin high-voltage screw terminal block (J2: NC, C, NO) for total electrical isolation between control circuitry and external AC/DC loads.

Visual Status Indication: Through-Hole (THT) 5mm dome LED (D1) with a 330ohm resistor with series current-limiting resistor (R2) for clear, wide-angle visual timing status.
Dual-Layer Copper Pour Strategy: Continuous Bottom Copper (B.Cu) Ground (GND) plane for low-impedance current return paths and noise immunity, alongside a Top Copper (F.Cu) +5V power zone for low-resistance power distribution.Enclosure-Ready Mechanical Layout: Four corner M3 mounting holes with copper/via keepouts and smooth rounded corner radii via PCB edge fillets.

### Process:
Phase 1: Schematic Capture, Footprints & Draft BOM
Schematic & Netlist Setup: Captured the NE555 monostable circuit, transistor switch, protection diode, and terminal blocks, verifying pin connections and running Electrical Rules Check (ERC).

IPC-7351 Footprint Assignment: Assigned standardized footprints across all components (SOIC-8 for U1, TO-92 for Q1, LED_THT:LED_D5.0mm for D1, standard SMD passive packages for resistors/capacitors, and heavy-duty THT footprints for K1/J2).

Phase 2: PCB Layout & Physical RoutingNetlist Transfer & Placement: 
Pushed schematic data to PCB Editor (F8). Rotated (R) and positioned components (R1–R4, U1, Q1) to untangle ratsnest lines—placing timing components directly adjacent to U1 pins 2 (TRIG) and 7 (DISCH).

Edge Geometry & Fillets: Defined the PCB boundary on the Edge.Cuts layer and applied the Fillet Lines tool (Shape Modification -> Fillet Lines) to convert sharp 90 degree corners into smooth rounded edges. 

Trace Routing & Zone Fills: Routed all signal and power connections with 45 degree trace bends. Flooded B.Cu with a GND zone and F.Cu with a +5V zone, pressing B to calculate zone fills.

### Extras:
Why Use a Transistor Driver (BC547) and Flyback Diode (D_2) for the Relay?
Relay Coil Drive: Driving a relay coil directly from the 555 output can cause current loading and thermal stress on the IC. The BC547 acts as an efficient low-side switch to turn the coil on and off reliably. 

Inductive Spike Clamping: When the relay turns off, the magnetic field in the coil collapses instantly, generating a massive reverse voltage spike. The flyback diode (D2) provides a safe recirculating path for this energy, protecting Q1 and U1 from high-voltage breakdown.

### Design Problems Encountered & Practical Rectifications
During the design and layout of the NE555-based On-Delay Timer module, several electrical, layout, and software verification challenges were identified and systematically resolved.

1. Component Reference Designator Mismatch (R1 vs R3)
Problem: Visual layout discrepancy between your schematic design and the instructor's reference PCB, where R1 and R3 appeared swapped.
Theory: KiCad routes based on Netlist continuity, not visual refdes strings. The netlist tracks electrical nodes regardless of component labels.
Rectification: Verified ratsnest connection paths in the PCB Editor (R3 Pad 2 connected to U1 Pin 2 TRIG; R1 Pad 2 connected to Pin 7 DISCH). Rotated (R) and repositioned the footprints to align with netlist paths without breaking electrical logic.

2. Hidden Copper Traces in 3D Viewer Render
Problem: Copper tracks rendered clearly in earlier projects appeared "missing" or blended into the board surface in 3D mode.
Theory: Adding a continuous Bottom Copper Ground Pour (B.Cu) floods copper across the entire layer. When covered by green solder mask epoxy, routed traces and ground fills merge into a uniform surface.
Rectification: Verified trace geometries by turning off the front solder mask (F.Mask) in the 3D Viewer Appearance Panel and pressing B in the PCB editor to recalculate zone fills.

3. Thermal Relief Spoke Count DRC Violation (R3 Pad 1)
Problem: DRC flagged Error: Thermal relief connection to zone in... F.Cu; zone min spoke count 2; actual 1 at Pad 1 of R3.
Theory: Heavy copper pours absorb heat rapidly during soldering. Thermal relief spokes restrict heat flow to prevent cold solder joints. If surrounding signal traces crowd a pad, KiCad cannot route the default minimum of 2 copper spokes.
Rectification: Manually routed a direct 45 degree copper trace (X) from R3 Pad 1 to the main +5V rail and nudged adjacent traces (D) to grant the zone space for a second spoke.

4. Unconnected Net and Silkscreen Clearance Warnings
Problem: DRC reported 1 unconnected net between J1 Pin 1 (+5V) and R1/R3, alongside 3 silkscreen clearance warnings (J2, R1, R2 text touching footprint boundary lines).
Theory: Overlapping silkscreen ink over solder pads or outer mechanical boundaries causes ink flaking during assembly and violates fab clearance rules.
Rectification: Completed the missing +5V trace route and used the move command (M) to position all reference designator text clearly outside component outlines (K1 relay box and J1 connector outline).

5. SMD to THT LED Package Conversion
Problem: Upgraded output status indicator from an SMD 0805 LED to a Through-Hole (THT) 5mm LED.
Theory: SMD LEDs emit light planar to the PCB surface. A THT 5mm dome LED focuses light forward, making it visible through enclosure cutouts.
Rectification: Reassigned the schematic symbol footprint to LED_THT:LED_D5.0mm while keeping the electrical current-limiting network identical (R2 = 330ohms).

### Outputs: 
<img width="1600" height="836" alt="image" src="https://github.com/user-attachments/assets/6b74d7ca-12c8-4b09-a0a9-26bffb940e79" />
<img width="1600" height="840" alt="image" src="https://github.com/user-attachments/assets/4b392055-466f-4e54-867d-25e56a4546c4" />
<img width="1600" height="781" alt="image" src="https://github.com/user-attachments/assets/a5764817-86b6-4300-ad87-8479d6f9614f" />


