# Thai Town Racer MVP — Unity + Claude Code Development Spec

## 1. Project Overview

### Project Name
`ThaiTownRacer`

### Game Concept
เกมรถแข่งแบบ **Low Poly / Stylized / Glow / Open World ขนาดเล็ก** โดยเริ่มจากเมืองท่องเที่ยวไทยขนาดเล็ก ผู้เล่นขับรถ 1 คัน สำรวจเมือง เล่น checkpoint race และ time attack ผ่าน Unity Editor

### MVP Theme
**ตลาดริมน้ำซิ่ง**  
เมืองท่องเที่ยวริมแม่น้ำ มีตลาดกลางคืน วัด สะพาน ถนนเลียบแม่น้ำ ปั๊มน้ำมัน และจุดชมวิวขนาดเล็ก

### Target Platform
- Phase MVP: Windows PC
- Input เริ่มต้น: Keyboard
- ดูผล: Unity Editor Play Mode
- Build ภายหลัง: Windows `.exe`

---

## 2. Core Objective

สร้าง prototype ที่เล่นได้จริงภายใน Unity Editor โดยมีองค์ประกอบหลักดังนี้:

1. รถ blockout 1 คัน ขับได้
2. กล้องตามรถ
3. เมือง greybox ขนาดเล็ก
4. ระบบ checkpoint race
5. Timer / UI พื้นฐาน
6. Reward / best time แบบง่าย
7. Art direction แบบ low-poly + color palette + glow
8. พร้อมต่อยอดเป็น open world racing game

---

## 3. Technology Stack

| Area | Technology |
|---|---|
| Game Engine | Unity 6 `6000.4.9f1` |
| Unity Template | Universal 3D |
| Render Pipeline | URP |
| Language | C# |
| IDE | Visual Studio |
| AI Assistant | Claude Code |
| 3D Modeling | Blender |
| Version Control | Git + Git LFS |
| Asset Style | Low Poly / Toon / Palette Texture |
| Target Scene | `ThaiTownRacer.unity` |

---

## 4. Hard Constraints

Claude Code ต้องทำตาม constraint เหล่านี้:

- ห้ามทำ multiplayer ใน MVP
- ห้ามใช้รถแบรนด์จริง / logo จริง
- ห้ามทำเมืองใหญ่เกิน scope
- ห้ามใช้ paid asset โดยไม่ระบุ
- ห้ามสร้างระบบซับซ้อนก่อนระบบขับรถนิ่ง
- ต้องทำงานแบบ phase-by-phase
- ทุก phase ต้อง test ผ่าน Unity Editor Play Mode
- ทุก script ต้องอยู่ใน `Assets/_Game/Scripts/`
- ทุก prefab ต้องอยู่ใน `Assets/_Game/Prefabs/`
- ทุก scene ต้องอยู่ใน `Assets/_Game/Scenes/`

---

## 5. Repository / Folder Structure

ให้จัด Unity Project ดังนี้:

```text
Assets/
  _Game/
    Art/
      Cars/
      Environment/
      Props/
      Materials/
      Palette/
    Audio/
      Music/
      SFX/
    Prefabs/
      Cars/
      Checkpoints/
      Environment/
      UI/
    Scenes/
      MainMenu.unity
      ThaiTownRacer.unity
    Scripts/
      Core/
      Vehicle/
      Camera/
      Race/
      UI/
      Save/
      Utility/
    ScriptableObjects/
      Missions/
      CarConfigs/
    Shaders/
    VFX/
  ThirdParty/
  Settings/
```

---

## 6. MVP Gameplay Loop

```text
Start Game
  ↓
Spawn รถในเมือง
  ↓
ขับสำรวจเมือง
  ↓
เข้า Race Start Point
  ↓
วิ่งผ่าน Checkpoints
  ↓
จบ Race
  ↓
แสดงเวลา / คะแนน / Reward
  ↓
บันทึก Best Time
  ↓
กลับไป Free Roam
```

---

## 7. Scene Spec

### Main Scene
`Assets/_Game/Scenes/ThaiTownRacer.unity`

### Required GameObjects

```text
ThaiTownRacer
├── World
│   ├── Ground
│   ├── Road_Blockout
│   ├── Buildings_Blockout
│   ├── Landmarks
│   └── Props
├── Player
│   └── Car_Blockout
├── Camera
│   └── FollowCamera
├── Race
│   ├── RaceManager
│   ├── CheckpointGroup_RiverLoop
│   └── RaceStartPoint
├── UI
│   ├── Canvas_HUD
│   ├── TimerText
│   ├── SpeedText
│   ├── CheckpointText
│   └── ResultPanel
└── Systems
    ├── GameManager
    ├── InputManager
    └── SaveManager
```

---

## 8. Phase Plan

---

# Phase 0 — Project Setup

## Goal
ตั้งค่า Unity project ให้พร้อมสำหรับพัฒนา MVP

## Tasks
- Create Unity project ด้วย template `Universal 3D`
- ตั้งชื่อ project: `ThaiTownRacer`
- สร้าง folder structure ตาม spec
- สร้าง scene `ThaiTownRacer.unity`
- ติดตั้ง package พื้นฐาน:
  - Input System
  - Cinemachine
  - ProBuilder
  - TextMeshPro
- ตั้งค่า Git repository
- ตั้งค่า Git LFS สำหรับไฟล์ asset ใหญ่

## Claude Code Instruction
Claude Code ช่วยสร้าง folder structure และไฟล์ placeholder ได้ แต่ห้ามแก้ setting Unity ที่เสี่ยงโดยไม่อธิบาย

## Acceptance Criteria
- เปิด Unity Editor ได้
- Scene `ThaiTownRacer.unity` เปิดได้
- ไม่มี console error
- Folder structure ถูกต้อง

---

# Phase 1 — Vehicle Blockout

## Goal
สร้างรถ blockout 1 คันที่ใช้ทดสอบ gameplay

## Required Prefab
`Assets/_Game/Prefabs/Cars/Car_Blockout.prefab`

## Car Hierarchy

```text
Car_Blockout
├── Visual
│   ├── Body
│   ├── Cabin
│   ├── FrontGlass
│   ├── RearGlass
│   ├── Wheel_FL
│   ├── Wheel_FR
│   ├── Wheel_RL
│   └── Wheel_RR
├── Colliders
│   └── MainBoxCollider
├── CameraTarget
└── GroundCheck
```

## Recommended Scale

```text
Body Scale:      X=1.8, Y=0.6, Z=3.8
Cabin Scale:     X=1.4, Y=0.6, Z=1.4
Wheel Scale:     X=0.45, Y=0.2, Z=0.45
Car Length:      ~4 meters
Car Width:       ~1.8 meters
```

## Required Components
On root `Car_Blockout`:

- `Rigidbody`
- `BoxCollider`
- `ArcadeVehicleController.cs`

## Required Script
`Assets/_Game/Scripts/Vehicle/ArcadeVehicleController.cs`

## Vehicle Parameters
```text
accelerationForce
brakeForce
steeringPower
maxSpeed
driftFactor
traction
turnSpeed
groundCheckDistance
```

## Acceptance Criteria
- กด Play แล้วรถอยู่บนพื้น
- กด W แล้วรถเดินหน้า
- กด S แล้วเบรก/ถอยหลัง
- กด A/D แล้วเลี้ยว
- รถไม่ตกทะลุพื้น
- ไม่มี console error

---

# Phase 2 — Camera Follow

## Goal
สร้างกล้อง third-person ที่ตามรถได้ดี

## Recommended Tool
ใช้ Cinemachine ถ้ามี package พร้อม

## Required Behavior
- กล้องตาม `CameraTarget`
- กล้องอยู่หลังรถ
- กล้องไม่สั่นมาก
- มองเห็นถนนข้างหน้า
- รองรับความเร็วรถ

## Camera Position Guideline

```text
Follow Offset:
X = 0
Y = 4
Z = -7
```

## Required Script Optional
`Assets/_Game/Scripts/Camera/VehicleCameraTarget.cs`

## Acceptance Criteria
- ขับรถแล้วกล้องตามได้
- เวลาเลี้ยว กล้องยังอ่านทิศทางรถได้
- ไม่เกิด motion sickness มาก
- ไม่มี console error

---

# Phase 3 — Greybox Town

## Goal
สร้างเมืองท่องเที่ยวเล็กแบบ blockout ก่อนลง art จริง

## Map Size
เริ่มจากประมาณ:

```text
800m x 800m
```

## Required Areas
```text
1. River Road
2. Night Market
3. Temple Area
4. Bridge
5. Gas Station
6. Hill Viewpoint
7. Start Plaza
```

## Required Road Types
- ถนน loop หลักรอบเมือง
- ถนนเลียบแม่น้ำ
- ถนนขึ้นเนินเล็ก
- ทางลัด 1 เส้น
- ลานกว้างสำหรับ start race

## Greybox Assets
ใช้ Cube / Plane / ProBuilder ก่อน:

```text
Building_Blockout_A
Building_Blockout_B
Market_Tent_Blockout
Temple_Blockout
Bridge_Blockout
GasStation_Blockout
Viewpoint_Blockout
Road_Barrier_Blockout
```

## Acceptance Criteria
- รถขับวนรอบเมืองได้
- ถนนกว้างพอสำหรับรถ
- มี landmark เห็นชัดอย่างน้อย 3 จุด
- รถไม่ติด collider ผิดปกติ
- ไม่มี console error

---

# Phase 4 — Checkpoint Race System

## Goal
สร้าง race system สำหรับ checkpoint race 1 สนาม

## Required Race
`River Loop Race`

## Required Scripts

```text
Assets/_Game/Scripts/Race/RaceManager.cs
Assets/_Game/Scripts/Race/Checkpoint.cs
Assets/_Game/Scripts/Race/RaceStartTrigger.cs
Assets/_Game/Scripts/Race/RaceMission.cs
```

## Race Flow

```text
Player enters RaceStartTrigger
  ↓
Show prompt: Press E to Start Race
  ↓
Start timer
  ↓
Activate checkpoint 1
  ↓
Player passes each checkpoint in order
  ↓
Finish race
  ↓
Show result
  ↓
Save best time
```

## RaceMission Data
ใช้ `ScriptableObject`

```text
missionId
missionName
timeLimit
rewardCoin
checkpointList
goldTime
silverTime
bronzeTime
```

## Acceptance Criteria
- เริ่ม race ได้
- checkpoint แสดงทีละจุด
- ผ่าน checkpoint แล้วไปจุดถัดไป
- จบ race แล้วแสดงเวลา
- ไม่มี console error

---

# Phase 5 — HUD / UI

## Goal
ทำ UI พื้นฐานสำหรับ gameplay

## Required UI
```text
SpeedText
TimerText
CheckpointText
RacePromptText
ResultPanel
BestTimeText
```

## Required Scripts
```text
Assets/_Game/Scripts/UI/HUDController.cs
Assets/_Game/Scripts/UI/RaceResultPanel.cs
```

## UI Behavior
- แสดงความเร็วรถ
- แสดงเวลา race
- แสดง checkpoint ปัจจุบัน
- แสดง prompt ตอนอยู่ใกล้จุดเริ่ม race
- แสดง result ตอนจบ race

## Acceptance Criteria
- UI อ่านง่าย
- UI ไม่บังกลางจอมากเกินไป
- ข้อมูลเวลา/checkpoint ถูกต้อง
- ไม่มี console error

---

# Phase 6 — Save / Reward System

## Goal
บันทึก best time และ reward พื้นฐาน

## Save Data
ใช้ JSON local save

```text
coin
bestTimeByMission
unlockedPaint
selectedPaint
```

## Required Scripts
```text
Assets/_Game/Scripts/Save/SaveManager.cs
Assets/_Game/Scripts/Save/PlayerSaveData.cs
Assets/_Game/Scripts/Core/RewardSystem.cs
```

## Reward Rule
```text
Finish Race = coin
Gold Time = extra coin
New Best Time = bonus coin
```

## Acceptance Criteria
- เล่นจบ race แล้วได้ coin
- best time ถูกบันทึก
- ปิด/เปิด Play Mode แล้วยังโหลดข้อมูลได้
- ไม่มี console error

---

# Phase 7 — Low Poly Art Pass

## Goal
แทน greybox บางส่วนด้วย asset low-poly

## Blender Asset List
เริ่มจาก asset ชุดเล็ก:

```text
SiamPickup_LowPoly
House_Wood_A
House_Wood_B
Market_Tent_A
Temple_Small_A
Bridge_River_A
Tree_Tropical_A
StreetLamp_A
RoadSign_A
GasStation_Small_A
```

## Art Direction
```text
Low Poly
Flat Color
Palette Texture
Toon Lighting
Subtle Glow
Thai Tourist Town Mood
```

## Color Palette Texture
สร้าง texture:

```text
Assets/_Game/Art/Palette/palette_32.png
```

## Palette Groups
```text
Car Colors
Building Colors
Road Colors
Nature Colors
Water Colors
Glow Colors
UI Debug Colors
```

## Unity Import Setting
```text
Texture Type: Default
sRGB: On
Wrap Mode: Clamp
Filter Mode: Point
Compression: None
```

## Acceptance Criteria
- รถ low-poly แทน blockout ได้
- asset อย่างน้อย 5 ชิ้นอยู่ในเมือง
- สี asset ไปในทิศทางเดียวกัน
- performance ยังลื่นใน Unity Editor
- ไม่มี console error

---

# Phase 8 — Glow / Lighting / Polish

## Goal
เพิ่ม mood ของเมืองท่องเที่ยวกลางคืน/เย็น

## Required Effects
- Bloom สำหรับไฟท้ายรถ / ป้ายร้าน / ไฟตลาด
- Directional Light แบบเย็น
- Ambient light แบบ sunset/night market
- ไฟถนนบางจุด
- particle ฝุ่นหลังรถแบบง่าย
- skid mark optional

## Required Polish
```text
Car engine sound
Checkpoint sound
Race start sound
Race finish sound
Simple background music
```

## Acceptance Criteria
- ภาพเริ่มมี mood ชัดเจน
- glow ไม่แรงจนแสบตา
- ขับแล้วรู้สึกมี feedback
- ไม่มี console error

---

# Phase 9 — Build Prototype

## Goal
ทำ build สำหรับส่งให้คนอื่นลองเล่น

## Build Target
```text
Windows x86_64
```

## Required Checks
- Main scene อยู่ใน Build Settings
- ไม่มี missing script
- ไม่มี missing material
- ไม่มี critical console error
- กดเริ่มเกมแล้วเล่นได้ทันที

## Acceptance Criteria
- ได้ไฟล์ build `.exe`
- เล่นได้อย่างน้อย 5–10 นาที
- มี race อย่างน้อย 1 สนาม
- คนเล่นเข้าใจเป้าหมายโดยไม่ต้องอธิบายเยอะ

---

## 9. Script Architecture

## Core Scripts

```text
GameManager
- manage game state
- free roam / racing / result

ArcadeVehicleController
- handle acceleration, brake, steering
- expose speed

RaceManager
- manage current mission
- start / update / finish race

Checkpoint
- detect player trigger
- notify RaceManager

HUDController
- show speed, timer, checkpoint

SaveManager
- save/load JSON

RewardSystem
- calculate coin and bonus
```

---

## 10. Coding Rules for Claude Code

Claude Code ต้องทำตาม coding rules:

1. ใช้ namespace:
```csharp
namespace ThaiTownRacer
```

2. ห้าม hardcode scene object ถ้าเลี่ยงได้
3. ใช้ `[SerializeField]` สำหรับ field ที่ต้องปรับใน Inspector
4. ห้ามใช้ public field โดยไม่จำเป็น
5. ต้อง null-check reference สำคัญ
6. ต้องแยก responsibility ชัดเจน
7. ห้ามสร้าง monolithic script
8. ทุก script ต้อง compile ได้ทันที
9. หลังแก้ code ต้องบอกว่าแก้ไฟล์ไหน
10. ถ้าต้องสร้าง prefab/scene object ให้บอก manual step ชัดเจน

---

## 11. Claude Code Working Prompt

ใช้ prompt นี้ใน Claude Code ตอนเริ่มงาน:

```text
You are helping me build a Unity 6 URP low-poly open-world arcade racing MVP called ThaiTownRacer.

Use this GAME_SPEC.md as the source of truth.

Important rules:
- Work phase by phase.
- Do not jump ahead.
- Keep the MVP small.
- Use C# scripts under Assets/_Game/Scripts/.
- Use namespace ThaiTownRacer.
- Prefer simple, readable Unity code.
- After each change, explain what files were created or modified.
- Also explain what I need to check in Unity Editor Play Mode.

Current phase: Phase 1 — Vehicle Blockout.

Please implement only the required scripts for this phase first.
```

---

## 12. Per-Phase Claude Code Prompts

## Phase 1 Prompt

```text
Implement Phase 1 — Vehicle Blockout.

Create ArcadeVehicleController.cs for Unity 6.

Requirements:
- Keyboard input W/S/A/D
- Rigidbody-based movement
- Arcade feel, not simulation
- Expose acceleration, brake, steering, maxSpeed, driftFactor in Inspector
- Provide current speed property for UI later
- Code must compile
- Namespace ThaiTownRacer
- Do not implement race system yet

After implementation, tell me the Unity Editor setup steps.
```

## Phase 4 Prompt

```text
Implement Phase 4 — Checkpoint Race System.

Create:
- RaceManager.cs
- Checkpoint.cs
- RaceStartTrigger.cs
- RaceMission.cs as ScriptableObject

Requirements:
- Player can start a race by entering trigger and pressing E
- Checkpoints must be completed in order
- Race timer starts and stops correctly
- Race result exposes finish time
- No reward system yet unless absolutely necessary
- Namespace ThaiTownRacer

After implementation, tell me how to create checkpoint objects in Unity Editor.
```

## Phase 5 Prompt

```text
Implement Phase 5 — HUD / UI.

Create:
- HUDController.cs
- RaceResultPanel.cs

Requirements:
- Show speed
- Show race timer
- Show current checkpoint index
- Show start prompt
- Show result panel after race finish
- Use TextMeshPro
- Namespace ThaiTownRacer

After implementation, tell me how to wire UI references in Unity Inspector.
```

---

## 13. Unity Editor Test Checklist

ทุกครั้งหลัง Claude Code แก้ script ให้ตรวจดังนี้:

```text
1. Unity compile ผ่านหรือไม่
2. Console มี error หรือไม่
3. กด Play ได้หรือไม่
4. Object reference ครบหรือไม่
5. Prefab missing script หรือไม่
6. รถยังขับได้หรือไม่
7. Scene save แล้วหรือยัง
```

---

## 14. MVP Definition of Done

MVP ถือว่าสำเร็จเมื่อ:

- เปิด Unity Editor แล้ว Play ได้
- รถ 1 คันขับได้
- มีกล้องตามรถ
- มีเมือง greybox ขนาดเล็ก
- มี checkpoint race อย่างน้อย 1 สนาม
- มี timer / result / best time
- มี reward coin แบบง่าย
- มี low-poly asset บางส่วน
- มี glow / lighting mood เบื้องต้น
- ไม่มี console error สำคัญ
- สามารถ build เป็น Windows `.exe` ได้

---

## 15. Future Scope หลัง MVP

หลัง MVP สำเร็จ ค่อยพิจารณา:

```text
- รถหลายคัน
- แต่งรถ
- Drift score
- NPC traffic
- Mini map
- More missions
- More towns
- Mobile build
- Controller support
- Steam page prototype
```
