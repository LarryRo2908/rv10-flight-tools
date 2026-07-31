# AGENTS.md - RV-10 Flight Tools Workspace Guidelines

This document defines the architectural specifications, cockpit ergonomic rules, aircraft constraints, and user preferences for the **RV-10 Flight Tools** Progressive Web App (PWA). 

Any AI agent starting a new conversation in this workspace must read and strictly adhere to these instructions.

---

## 1. Aircraft & Target Hardware Profile

- **Aircraft Model:** Vans RV-10 (Registration: N205EN, Engine: Lycoming IO-540)
- **Total Fuel Capacity:** 60.0 GAL usable (30.0 GAL Left tank, 30.0 GAL Right tank)
- **Protocol:** Single engine feeding from one tank at a time. Pilot sits in Left seat (180 lbs offset), flight protocols start feeding on **LEFT TANK** to burn climb fuel and offset lateral weight & balance.
- **Typical Fuel Burn:** ~10.0 GPH (cruise), 15.0 – 20.0 GPH (climb).
- **Target Primary Device:** **iPad Mini (6th Generation)** mounted in single-pilot cockpit during in-flight operations (including turbulence).

---

## 2. Technical Architecture & Constraints

- **Single-File PWA (`index.html`):** The entire application (HTML structure, Tailwind CSS via CDN, JavaScript logic, inline Web Manifest Data URI, and inline Blob Service Worker) MUST remain self-contained in `index.html` with zero external npm or build steps.
- **Offline-First:** All state (Left/Right tank quantities, active tank selection, EMS totalizer reading, flight takeoff timestamp, and switch log) MUST persist to `localStorage` on every tap. The app must function 100% offline in flight.
- **Git & GitHub Pages Deployment:** Main branch (`main`) is pushed to GitHub (`rv10-flight-tools`) and deployed via free GitHub Pages for native iOS PWA Home Screen installation.
- **Safety First:** NEVER make code edits that delete, bypass, or weaken user confirmation safeguards.

---

## 3. Cockpit Ergonomics & Navigation Rules (FlyQ Style)

1. **Auto-Hiding FlyQ-Style Bottom Navigation Bar**:
   - Deep black background with white icons & text.
   - **Active Module Tab:** Highlighted in **Cyan background** (`bg-cyan-500 text-black font-black`).
   - **Auto-Hide Timer (10 Seconds):** Appears when the pilot touches the screen, and automatically fades out/hides after 10 seconds of inactivity to keep flight screens 100% unobstructed.
   - **Screen Visibility Rule:** Present on all main module screens (`FUEL`, `KNEEBOARD`, `CHECKLISTS`). MUST NOT appear on the `Tank MGMT` full-screen modal/overlay. Pilot must tap `Back / Close` to exit Tank MGMT before changing modules.

2. **Non-Interactive Top Status Bar**:
   - 100% status-only (`GPS: OK / N/A`, `TOTAL FUEL: XX.X GAL`, `ACTIVE FEED: LEFT/RIGHT TANK`). Zero clickable elements on top.

3. **Zero-Scroll Main Cockpit Dashboard**:
   - Massive numerical displays for Left Tank (Blue) & Right Tank (Green) with active feed glow highlights.
   - ONE primary action button: `[ TANK MGMT ]`.
   - Flight Switch Log displaying 24-hr UTC timestamps (`HH:MMZ`) and segment duration subtext (`Δ HH:MM`).

4. **No-Glasses Readability**:
   - Primary numbers (**New EMS Reading**, **Delta Burned**, **Segment GPH**) MUST be rendered in large, bold font (30px+ `font-mono font-black`) readable without reading glasses.

5. **Massive Stepper Touch Targets**:
   - Primary deduction steppers `[-1.0 GAL]` and `[-0.1 GAL]` MUST be **80px tall (`h-20`)** and span 50% of the screen width each.
   - Secondary nudge buttons (`+0.1`, `+1.0`) are kept in a smaller secondary row for over-tap corrections.

6. **Safety Friction Design for Destructive Actions**:
   - **Undo Last Entry**: Requires a modal where the primary default button is a massive green `CANCEL (KEEP LOG ENTRY)`.
   - **Reset Flight / Fuel Tanks**: Located on the Tank Management page, separated from the `Confirm Switch` button by a large vertical scroll spacer (`pt-48 pb-16`). Requires a 2-step confirmation modal where `CANCEL` is always the primary top button.
   - **Unconfirmed Edits**: Tapping `Back / Close` on the Tank Management screen MUST discard any unconfirmed keypad/stepper edits and restore the EMS input to the clean auto-predicted value based on elapsed time.

---

## 4. Multi-Module Roadmap

1. **Module 1 (Implemented):** Fuel Tracker & Tank Switch Engine.
2. **Module 2 (Implemented):** Kneeboard Cockpit Reference Cards & Specs (integrated from `Flight_Data/N205EN_Cockpit_Card.html`).
3. **Module 3 (Planned):** Performance & GAMI Lean Tables / Operating Checklists.

---

## 5. Local Reference Data

- Reference files and cockpit cards are accessible via the relative directory: `../Flight_Data/reference/cockpit_cards/`
