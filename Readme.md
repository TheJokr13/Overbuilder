# Overbuilder
A 3D sandbox and building game built with **Unreal Engine 5**, featuring interactive physics, real-time object placement, dynamic scaling, rotation, destruction mechanics, blockout environment design, and an interactive lootbox/crate item economy.

---

## Game Purpose & Features

The primary objective of this project is to provide a physics-based sandbox environment where players can freely construct, modify, and destroy structures using various modular building elements (cubes, ramps, cylinders, etc.), while exploring blockout environments, opening lootboxes/crates, and managing acquired items via a dynamic hotbar.

### Key Gameplay Mechanics
- **First-Person Character Controls**: Full movement system including walking, sprinting, jumping, and camera controls.
- **Dynamic Building System**: 
  - Toggle Build Mode on/off.
  - Cycle through available building assets via mouse wheel or dedicated Hotbar UI.
  - Preview objects with dynamic hologram materials (Green = Valid, Red = Invalid/Colliding).
  - Rotate objects on the fly before placing.
  - Scale objects dynamically up or down.
- **Lootbox & Crate System**: Interactive world crates that contain randomized or preset items for the player to collect.
- **Dynamic Hotbar Item Management**: Real-time hotbar updates when adding, placing, or emptying items across slots.
- **Destruction Mechanics**: Destroy placed or existing objects in real time using a raycast-based deletion tool.
- **Blockout Environment & Map**: Clean blockout world setup for prototyping and testing, featuring dedicated map assets (prepared for future mission slot integration).
- **Interactive UI / HUD**: 
  - Contextual control hints dynamically toggled based on the active mode.
  - Interactive Hotbar with active slot visual highlighting and live slot item states.

---

## Short Technical Documentation

### 1. Overall Project Architecture
The project follows a modular Component-Oriented and Event-Driven architecture inside Unreal Engine 5 using Blueprints:
- **`BP_PlayerCharacter`**: Serves as the central actor managing inputs, movement, physics trace queries, item inventory logic, and UI references.
- **`BP_HUD` / `WBP_HUD`**: Manages user interface rendering, dynamic hotbar updates (add/place/empty slot transitions), and contextual keybind overlays.
- **`BP_Crate` / Lootbox Blueprints**: Dedicated actors handling player interaction, loot container state, and item distribution.
- **Building / Placement Logic**: Utilizes line traces (raycasting) from the player camera to calculate surface normals, ground offsets, and transform data for previewing and spawning mesh actors.

### 2. Main Classes, Blueprints & Components
- **`BP_PlayerCharacter`**:
  - `CameraComponent` & `SpringArmComponent`: First-person view handling.
  - `EnhancedInputComponent`: Processes inputs for movement, building toggles, scaling, rotation, lootbox interactions, and object placement.
  - `HUD_Ref`: Reference variable storing the active `WBP_HUD` instance.
- **`BP_Crate` (Lootbox Blueprint)**:
  - Manages container interaction logic, randomized/preset item drop tables, and dispatching acquired items to the player's hotbar.
- **`WBP_HUD` (Widget Blueprint)**:
  - `UpdateActiveSlot(CurrentIndex)`: Function that compares slot indices to highlight the active Hotbar slot dynamically.
  - `UpdateSlotState(SlotIndex, ItemData)`: Updates hotbar slots dynamically when items are added, placed (consumed), or emptied.
  - `UpdateControlsDisplay(bIsBuildMode)`: Function updating context-sensitive HUD elements.
- **Building Assets, Map & Blockout Level**:
  - Modular static meshes with precise collision channels (Complex as Simple or Box Collisions) to support stable stacking and physics interaction.
  - Level map asset configured in blockout style, structured for future updates featuring dedicated mission slots.

### 3. Interaction System & Item Economy
- **Lootbox Interaction**: Players interact with `BP_Crate` instances in the world to extract items/building components.
- **Dynamic Hotbar State Management**: 
  - **Adding Items**: Automatically populates empty hotbar slots or stacks items upon looting.
  - **Placing/Consuming Items**: Placing an item decrements slot counts or removes the asset from the hotbar slot.
  - **Emptying Slots**: Hotbar updates visually and structurally when slots are emptied or cleared.
- **Raycasting (Line Trace by Channel)**: Executes every frame while in Build Mode from the center of the viewport to detect surface locations and normals.
- **Hologram Preview**: A temporary static mesh component attached to the player or updated dynamically at the trace hit point. Its material color switches based on overlapping state checks (`IsOverlapping` queries).
- **Spawn & Transform Logic**: Calculates placement locations by combining hit impact point + surface normal offset + player rotation + scale modifier before spawning the definitive Actor.

### 4. Physics Systems Developed
- **Rigid Body Dynamics**: Placed or destroyed structures can simulate physics upon placement or destruction for realistic collapse and interaction.
- **Collision Presets**: Custom collision handling ensuring preview meshes ignore player collision while active structures properly block player movement and physics actors.

### 5. Main Technical Choices
- **Blueprint-First Approach**: Chosen for rapid iteration, seamless visual debugging, and efficient UI integration with UMG.
- **Decoupled UI & Container Logic**: UI functions (`UpdateActiveSlot`, `UpdateSlotState`, `UpdateControlsDisplay`) are isolated within `WBP_HUD`, while item collection logic is modularized inside `BP_Crate`.
- **Enhanced Input System**: Utilized for clean mapping of multi-key actions (e.g., mouse wheel scrolling, rotation inputs, scaling shortcuts, and interaction keys).

---

## AI Statement of Intent

### Overview
During the development of this project, Artificial Intelligence tools (specifically Large Language Models) were utilized as a collaborative assistant to accelerate problem-solving, clarify technical concepts, and optimize blueprint architecture.

> **Important Statement**: AI was used purely as a technical advisor and brainstorming facilitator to streamline implementation. The AI did **not** perform the work in my place; every system, blueprint node, logic flow, lootbox/hotbar interaction, and asset configuration was manually implemented, tested, debugged, and fully comprehended by myself.

---

### 1. Tools Used
- **ChatGPT / Gemini (Large Language Models)**: Used for technical guidance, Blueprint architecture planning, mathematical formula verification (location/rotation offsets), inventory/hotbar array management, and UI workflow optimization within Unreal Engine 5.

---

### 2. Where AI Was Used
AI assistance was integrated across the following areas:
- **UI Architecture & Hotbar Management**: Designing the logic for dynamic hotbar slot highlighting (`UpdateActiveSlot`), state updates for adding/placing/emptying slots (`UpdateSlotState`), and context-based control toggles (`UpdateControlsDisplay`).
- **Lootbox & Crate Blueprint Architecture**: Planning modular interaction systems and data flow between `BP_Crate`, player inventory arrays, and HUD elements.
- **Math & Line Trace Logic**: Verifying surface normal calculations for placing objects flush against walls/floors and computing dynamic object scaling.
- **Troubleshooting & Debugging**: Resolving Blueprint execution flow issues, variable reference passing, loot item state sync, and material opacity/color setting issues.
- **Documentation**: Structuring and refining technical explanations for this README file.

---

### 3. Why AI Was Used
- To speed up technical research regarding Unreal Engine 5 API, UMG best practices, and actor interaction mechanics.
- To serve as a sounding board for architectural decisions (e.g., deciding whether to store container inventory data inside individual `BP_Crate` instances or a central manager).
- To overcome technical roadblocks quickly without spending hours browsing outdated forum threads.

---

### 4. How AI Was Used
- **Prompt-Based Technical Inquiries**: Asking specific questions such as *"How do I highlight a specific slot in an Horizontal Box in UMG based on an integer index?"*, *"How do I update hotbar slot icons when an item is added or placed?"*, or *"How to toggle UI visibility based on a boolean state?"*.
- **Code & Logic Review**: Describing planned Blueprint node chains to verify logic correctness before implementation.
- **Iterative Refinement**: Using AI feedback to break complex features down into step-by-step implementation phases.

---

### 5. Benefits Obtained
- **Accelerated Learning**: Gained a deeper understanding of Unreal Engine 5 UMG functions, variable promotion, event-driven communication between Character, Lootbox Crates, and HUD.
- **Cleaner Blueprint Architecture**: Avoided redundant code by implementing modular functions rather than cluttering the Event Graph.
- **Improved UI/UX & Gameplay Mechanics**: Developed a cleaner and more responsive HUD through structured UI design guidelines and seamless hotbar state updates.

---

### 6. Limitations Encountered
- **Visual Blueprint Context**: AI models operate on text and cannot directly "see" Unreal Engine Blueprint graphs or editor windows. All node setups had to be translated verbally or via screenshots, requiring precise technical communication.
- **Outdated API References**: Occasionally, AI tools suggested legacy Unreal Engine 4 methods (e.g., legacy input bindings) that had to be manually adapted to Unreal Engine 5's Enhanced Input System.
- **No Direct Execution**: The AI could only explain *how* to build something; the manual execution, node wiring, testing, and debugging remained 100% my responsibility.

### Verification & Understanding Statement
All logic, Blueprint nodes, functions, and systems present in this project have been built, thoroughly tested, verified, and fully understood. I am completely capable of explaining, modifying, or extending any part of the codebase independently.
