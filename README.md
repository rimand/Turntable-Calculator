# Mechanic HOBOT Calculator

Engineering calculator for heavy-duty **turntable** and **linear track** drive systems. Compute motor power, torque, gearbox sizing, braking, and electrical supply from load, speed, and drive parameters.

**ภาษาไทย:** เครื่องมือคำนวณทางวิศวกรรมสำหรับระบบ Turntable และ Linear Track แบบ Heavy-Duty ใช้คำนวณกำลังมอเตอร์ แรงบิด ขนาด Gearbox/VFD และรายงาน Spec สำหรับส่งต่อให้ Supplier ได้ทันที

---

## Overview

Mechanic HOBOT Calculator is a single-page web app for engineers and integrators who need to size motors, gearboxes, and drives for rotating stages (turntables) or linear tracks. It supports two calculation modes (power from load, or load from power), wheel and gear drives, servo and induction motors, and outputs standard motor/VFD recommendations plus a PDF-ready technical report with warnings.

- **Version:** 1.4  
- **Run:** Open `index.html` in a modern browser (Chrome, Firefox, Edge). No server or install required.  
- **Data:** All processing is client-side; project state can be saved/loaded (browser storage and JSON file).

---

## Features

### Application modes

- **Turntable** — Rotating stage; inputs: diameter (m), target speed (RPM), drive radius.
- **Linear Track** — Linear motion; inputs: track length (m), target speed (m/s or m/min).

### Calculation modes

- **Calc Power** — From mass, speed, and kinematics: required motor power (kW), torque, gearbox ratings, total kVA/current.
- **Calc Load** — From installed motor power: maximum permissible load (tons) and limiting factor.

### Drive systems

- **Wheel drive** — Wheel diameter (mm or inch), drive radius; includes traction/slippage warning.
- **Gear drive (turntable)** — Stage and pinion teeth, gearbox ratio, drive radius.
- **Gear drive (linear)** — Rack & pinion: module and teeth (presets or custom), pitch diameter.

### Motor and electrical

- **Motor type:** Servo or Induction (with pole selection for induction).
- **Rated motor speed:** User-editable; for induction, optional auto-fill from poles.
- **Recommended standard motor:** Nearest IEC rating (kW) per motor.
- **Recommended VFD rating:** Standard VFD size (kW) and current (A) for total drive power.
- **Inertia ratio (JL/JM):** Shown for Servo only; load inertia reflected to motor / motor rotor inertia, with warning if > 10:1.

### Kinematics and braking

- **Acceleration time** and **Deceleration time** (sec).
- **Acceleration profile:** Linear (trapezoidal) or S-Curve with configurable peak factor.
- **Braking torque (Nm)** or **Braking force (N)** from deceleration time and load inertia / mass.

### Results and reports

- Torque breakdown: Static (breakaway), Rolling, Acceleration, Design peak (with safety factor), Braking.
- Gearbox: Nominal and peak output torque; minimum recommended peak (with safety factor).
- Electrical: Total power (kW), kVA, current (3-phase 380 V).
- Drive detail: Motor torque (Nm), tangential force (kN), drive element speed (RPM), motor type and rated speed.
- **Technical report** (on-screen) and **PDF export** (via Report / Export button).

### PDF export — specification sheet

The exported PDF is designed as a one-page spec sheet that can be sent directly to motor/gearbox/VFD suppliers:

- **Warnings & Notes** (top of page, red box) — prominently displays:
  - **Motor Speed Exceeded** — target vs actual stage speed (with actual speed in large bold red text), rated motor RPM, and cap notice.
  - **Traction Risk** — minimum wheel downforce required.
  - **High Inertia Ratio** — JL/JM warning for servo sizing.
- **Motor sizing / Load capacity result** — summary box with kW, hp, and operating point.
- **Required Motor** — power per motor, standard motor kW, recommended VFD kW and current, motor speed, total power, kVA, current.
- **Load & Geometry** — load (tons), size (m), speed.
- **Kinematics & Friction** — accel/decel time, profile, friction coefficients.
- **Drive System** — drive type, gearbox ratio, total ratio, efficiency.
- **Torque Analysis** — static, rolling, acceleration, braking, design peak with limiting factor.
- **Gearbox Selection** — nominal/peak torque, minimum recommended, safety factor.
- **Drive Detail** — motor torque, tangential force, drive element speed, rated motor speed, motor type, inertia ratio (servo), min wheel downforce (wheel drive).

If no warnings are active, the warning box is hidden and the PDF remains clean.

### 3D visualization

- **Turntable:** Rotating stage with center pin; motors with IEC-style body (fins, shaft, terminal box, gearbox block), wheel or gear at drive radius.
- **Linear track:** Rails, moving platform, and same motor assemblies along the track.
- Real-time animation of stage and drive elements; orbit camera and color customization.

### Project and export

- **Auto-save** to browser storage (latest state restored on reload).
- **Save File** — Download project as JSON.
- **Load File** — Import project from JSON.

---

## Quick start

1. Open `index.html` in a browser.
2. Choose **Turntable** or **Linear Track** and **Calc Power** or **Calc Load**.
3. Enter geometry (diameter or track length), mass (tons), target speed, acceleration/deceleration times.
4. Select **Wheel** or **Gear** drive and fill in wheel diameter or gear data (and for linear gear: module/teeth).
5. Set number of motors, gearbox ratio, efficiency, and (for Calc Power) rated motor speed and motor type.
6. Read results in the right panel; use **Report** and **Export** for a PDF specification sheet ready for supplier discussion.

---

## Main parameters

| Parameter | Description |
|-----------|-------------|
| Diameter / Track length | Stage diameter (m) or track length (m). |
| Total moving mass | Tons. |
| Target speed | RPM (turntable) or m/s / m/min (linear). |
| Acceleration / Deceleration time | Seconds. |
| Drive radius | Distance from center (turntable) or N/A (linear). |
| Gearbox ratio, Efficiency | Drive reduction and system efficiency (0–1). |
| Safety factor (peak torque) | Multiplier for recommended gearbox peak (e.g. 1.5×). |
| Friction (rolling, static) | μ; presets: Clean, Dusty. |

---

## Calculation outline

- **Torque (turntable):**  
  Static = μ_static × m × g × R_stage; Rolling = μ_rolling × m × g × R_stage;  
  Acceleration = I × α (I = ½ m R², α = ω / t_accel); S-Curve multiplies peak acceleration.  
  Peak = max(Static, Rolling + Acceleration); design peak = peak × safety factor.

- **Force (linear):**  
  Same structure with linear force F = m×a and friction F = μ×m×g; drive torque from force and drive radius.

- **Braking:**  
  Same inertia/mass and speed; α_brake = ω / t_decel (or a_brake = v / t_decel); T_brake = I×α_brake or F_brake = m×a_brake.

- **Motor power:**  
  From required torque/force and gear ratio/efficiency; rated power at rated RPM; total kVA and current for 3-phase 380 V.

- **Standard motor / VFD:**  
  Nearest standard kW from predefined lists. VFD current from total power and voltage.

- **Inertia ratio (servo):**  
  J_load reflected to motor shaft (divided by total ratio²) / typical servo rotor inertia from power; warning if > 10.

---

## Technology

- **HTML5, CSS (Tailwind), JavaScript (ES6 modules)**  
- **Three.js** — 3D scene and animation  
- **html2pdf.js** — PDF report export  
- **Font Awesome** — Icons  

Runs entirely in the browser; no backend.

---

## Warnings and limitations

- **Motor speed:** If required RPM exceeds rated motor speed, a warning is shown (on-screen and in PDF) and speed is capped. The PDF prominently displays the actual achievable stage speed.
- **Traction (wheel drive):** Minimum downforce per wheel is indicated; insufficient downforce can cause slippage.
- **Gearbox:** Select units with nominal torque ≥ calculated nominal and peak torque ≥ recommended minimum (with safety factor).
- **Inertia ratio (servo):** Ratio > 10 may require higher ratio or larger motor for stable response.
- Results are for engineering reference; validate with qualified engineers and equipment data before implementation.

---

## License and contributing

This project is open source. Feedback and contributions (issues, pull requests) are welcome.
