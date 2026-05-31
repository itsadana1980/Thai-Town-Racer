# ThaiTown Low Poly Glow Racing — MVP Prototype Spec

> Unity MVP prototype สำหรับเกมรถแข่ง Low Poly Glow Open World เมืองท่องเที่ยวไทยขนาดเล็ก  
> ใช้สำหรับส่งให้ Claude Code / Visual Studio / Unity Editor ทำงานเป็น phase

---

## Project Goal

สร้าง prototype เกมรถแข่งแบบ Low Poly Glow Open World ใน Unity โดยเริ่มจากเมืองท่องเที่ยวเล็ก ๆ ในประเทศไทย มีรถ 1 คัน, เมือง blockout ขนาด 800x800m, ระบบแข่ง checkpoint, HUD, save/reward และ build เป็น Windows x86_64 playable session ประมาณ 5–10 นาที

---

## Recommended Unity Setup

### Recommended Template

```text
Unity Template: 3D URP
```

เหตุผล:

- รองรับ Bloom / Glow / Lighting polish ได้ง่าย
- เหมาะกับ low-poly night mood
- เหมาะกับ prototype ที่ต้องการ visual style ชัดเจนตั้งแต่ต้น

ถ้าเริ่มด้วย `3D Core` อยู่แล้ว สามารถทำ Phase 0–7 ได้ก่อน แล้วค่อยปรับเรื่อง lighting/post-processing ใน Phase 8

---

## MVP Scope

### Included

- รถ 1 คัน
- Scene เดียว: `ThaiTownRacer.unity`
- เมือง blockout 800x800m
- Race mission 1 รายการ: `River Loop Race`
- Checkpoint race 1 loop
- HUD แสดง speed / timer / checkpoint / result
- JSON local save
- Coin reward
- Best time
- Low-poly art pass ขั้นต้น
- Bloom / lighting / VFX / audio polish ขั้นต้น
- Windows x86_64 build

### Excluded

- Multiplayer
- Traffic AI
- NPC
- Full open-world progression
- Garage system เต็มรูปแบบ
- Minimap
- Mobile build
- Online leaderboard

---

# Global Folder Structure

สร้าง folder ตามนี้ภายใต้ `Assets/_Game/`

```text
Assets/
  _Game/
    Scenes/
      ThaiTownRacer.unity

    Scripts/
      Vehicle/
        ArcadeVehicleController.cs

      Camera/
        CameraTargetFollow.cs

      Race/
        RaceManager.cs
        Checkpoint.cs
        RaceStartTrigger.cs
        RaceMission.cs

      UI/
        HUDController.cs
        RaceResultPanel.cs

      Save/
        SaveManager.cs
        PlayerSaveData.cs

      Rewards/
        RewardSystem.cs

      Utilities/
        GameConstants.cs

    Prefabs/
      Vehicles/
        Car_Blockout.prefab

      Race/
        Checkpoint.prefab
        RaceStartTrigger.prefab

      Environment/
        Building_Blockout.prefab
        Road_Blockout.prefab
        Bridge_Blockout.prefab

      Props/
        Cone.prefab
        MarketStall.prefab
        StreetLamp.prefab

    ScriptableObjects/
      Missions/
        RiverLoopRace.asset

    Materials/
      Greybox/
      LowPoly/
      Glow/

    Textures/
      palette_32.png

    Audio/
      Engine/
      Race/
      BGM/

    VFX/
      Dust/
      Checkpoint/

    Art/
      LowPoly/
        Vehicles/
        Buildings/
        Props/

    ProBuilder/
      GreyboxTown/
```

---

# Phase 0 — Complete Project Setup

## Objective

เตรียม Unity project ให้พร้อมก่อนเริ่มพัฒนา gameplay

---

## Tasks

### 0.1 Install Required Packages

ติดตั้งหรือ verify packages เหล่านี้ใน Unity Package Manager:

```text
Cinemachine
ProBuilder
TextMeshPro
```

### 0.2 Import TextMeshPro Essentials

ถ้า Unity ขึ้น popup:

```text
Import TMP Essentials
```

ไม่จำเป็นต้อง import Examples & Extras สำหรับ MVP

### 0.3 Create Project Folder Structure

สร้าง folder หลัก:

```text
Assets/_Game/
```

แล้วสร้าง subfolders ตาม Global Folder Structure ด้านบน

### 0.4 Create Main Scene

สร้าง scene:

```text
Assets/_Game/Scenes/ThaiTownRacer.unity
```

### 0.5 Create Base Scene Objects

ใน scene ให้สร้าง object พื้นฐาน:

```text
GameManager
RaceManager
SaveManager
Main Camera
Directional Light
Global Volume
PlayerSpawn
RaceStart
Ground_Test
```

### 0.6 Set Project Unit Convention

```text
1 Unity Unit = 1 Meter
```

### 0.7 Add Scene to Build Settings

เพิ่ม scene นี้เข้า Build Settings:

```text
ThaiTownRacer.unity
```

---

## Acceptance Criteria

- [ ] เปิด Package Manager แล้วเห็น Cinemachine
- [ ] เปิด Package Manager แล้วเห็น ProBuilder
- [ ] เปิด Package Manager แล้วเห็น TextMeshPro
- [ ] สร้าง TMP Text ได้โดยไม่ error
- [ ] มี folder `Assets/_Game/`
- [ ] มี scene `ThaiTownRacer.unity`
- [ ] Scene ถูก add เข้า Build Settings
- [ ] กด Play แล้วไม่มี console error

---

## Notes

ถ้าใช้ Unity 6 หรือ version ใหม่ ชื่อเมนูบางจุดอาจต่างกันเล็กน้อย แต่ concept เหมือนเดิม

---

# Phase 1 — Vehicle Blockout

## Objective

สร้างรถ blockout ที่ขับด้วย W/A/S/D ได้ และแก้ปัญหารถหมุนติ้ว / เลี้ยวไวเกินไปตั้งแต่ต้น

---

## Required Script

```text
Assets/_Game/Scripts/Vehicle/ArcadeVehicleController.cs
```

---

## Required Prefab

```text
Assets/_Game/Prefabs/Vehicles/Car_Blockout.prefab
```

---

## Car GameObject Structure

```text
Car_Blockout
  Body
  FrontMarker
  CameraTarget
```

### Body

ใช้ Cube ทำเป็นตัวถังรถ

```text
Scale: (1.8, 0.6, 4.0)
Position: (0, 0.6, 0)
```

### FrontMarker

ใช้ Cube หรือ small cone วางด้านหน้ารถเพื่อให้รู้ทิศทาง

```text
Position: (0, 0.9, 2.1)
Scale: (0.4, 0.2, 0.2)
```

### CameraTarget

ใช้ Empty GameObject

```text
Local Position: (0, 1.5, 0)
```

---

## Required Components on Car_Blockout

```text
Rigidbody
BoxCollider
ArcadeVehicleController.cs
```

---

## Recommended Rigidbody Settings

```text
Mass: 1000
Drag: 0.2
Angular Drag: 3.0
Use Gravity: true
Interpolate: Interpolate
Collision Detection: Continuous Dynamic

Constraints:
  Freeze Rotation X: true
  Freeze Rotation Y: false
  Freeze Rotation Z: true
```

---

## Recommended BoxCollider Settings

```text
Center: (0, 0.55, 0)
Size:   (1.8, 1.0, 4.0)
```

---

## Recommended ArcadeVehicleController Parameters

```text
Max Speed: 25
Acceleration Force: 9000
Brake Force: 6000
Reverse Force: 4000
Turn Speed: 55
Min Turn Speed Factor: 0.2
Turn Only When Moving: true
Steer Smoothing: 6
Ground Check Distance: 0.7
```

---

## Input Mapping

ใช้ legacy input สำหรับ MVP:

```text
W = Accelerate
S = Brake / Reverse
A = Steer Left
D = Steer Right
Space = Brake
R = Reset Car
```

ถ้า input ไม่ทำงาน:

```text
Edit > Project Settings > Player > Other Settings > Active Input Handling
```

ตั้งเป็น:

```text
Both
```

หรือ

```text
Input Manager (Old)
```

---

## Vehicle Logic Requirements

- ใช้ `FixedUpdate()` สำหรับ physics
- ใช้ Rigidbody เป็นหลัก
- ห้ามขยับรถด้วย `transform.position` ใน gameplay หลัก
- ห้ามหมุนรถด้วย `transform.Rotate()` ใน `Update()`
- จำกัดความเร็วสูงสุด
- เลี้ยวเฉพาะแกน Y
- steering ต้องสัมพันธ์กับความเร็ว
- รถหยุดนิ่งไม่ควรหมุนเร็ว
- มี smoothing input
- มี reset position สำหรับ debug

---

## Acceptance Criteria

- [ ] กด Play แล้วรถไม่ตกทะลุพื้น
- [ ] กด W แล้วรถเดินหน้า
- [ ] กด S แล้วรถเบรกหรือถอยหลัง
- [ ] กด A/D แล้วรถเลี้ยวค่อย ๆ
- [ ] รถไม่หมุนติ้ว
- [ ] รถไม่พลิกง่าย
- [ ] รถวิ่งบนพื้นเรียบได้
- [ ] ความเร็วไม่เกิน max speed
- [ ] กด R reset รถได้
- [ ] Save เป็น prefab `Car_Blockout.prefab`

---

## Common Fix: รถเลี้ยวไว / หมุนติ้ว

ถ้าเจอปัญหารถหมุนไวมาก ให้แก้ตามลำดับนี้:

```text
1. Rigidbody > Angular Drag เพิ่มเป็น 4–6
2. Turn Speed ลดเหลือ 35–45
3. Freeze Rotation X และ Z
4. ตรวจว่า turn คูณ Time.fixedDeltaTime แล้ว
5. ห้ามใช้ transform.Rotate ใน Update
6. ทำให้ turn factor ขึ้นกับ forward speed
7. ถ้าความเร็วต่ำมาก ให้ลด steering ลง
```

---

# Phase 2 — Camera Follow

## Objective

ตั้งกล้อง third-person camera ตามรถแบบนุ่ม ไม่กระตุก ไม่เหวี่ยงแรง

---

## Required Package

```text
Cinemachine
```

---

## Required Object

```text
Car_Blockout
  CameraTarget
```

---

## CameraTarget Settings

```text
Local Position: (0, 1.5, 0)
```

---

## Cinemachine Setup

สร้าง Cinemachine Camera หรือ Virtual Camera แล้วตั้งค่า:

```text
Follow: CameraTarget
Look At: CameraTarget
```

Recommended offset:

```text
Offset: (0, 4, -7)
```

Recommended FOV:

```text
FOV: 55–65
```

Recommended damping:

```text
X Damping: 0.2–0.5
Y Damping: 0.4–0.8
Z Damping: 0.4–0.8
```

---

## Camera Requirements

- กล้องตามหลังรถ
- กล้องไม่สะบัดแรงตอนเลี้ยว
- กล้องไม่ jitter ตอนรถชน
- รถต้องอยู่ใน frame ตลอด
- ขับ 2 นาทีแล้วไม่เวียนหัว

---

## Acceptance Criteria

- [ ] Camera follow รถได้
- [ ] Camera look at รถได้
- [ ] ตอนรถเร่ง กล้องไม่กระตุก
- [ ] ตอนรถเลี้ยว กล้องไม่เหวี่ยงแรง
- [ ] ตอนรถชน กล้องไม่สั่นผิดปกติ
- [ ] รถไม่หลุด frame
- [ ] เล่นต่อเนื่อง 2 นาทีได้

---

## Common Fix: Camera Jitter

```text
1. Rigidbody > Interpolate = Interpolate
2. Camera ต้อง follow CameraTarget ไม่ใช่ Body mesh
3. ใช้ Cinemachine damping
4. ห้ามขยับรถด้วย transform.position ใน Update
5. CameraTarget ควรเป็น child ของรถ
```

---

# Phase 3 — Greybox Town

## Objective

สร้างเมือง blockout ขนาด 800x800m สำหรับทดสอบการขับและ race route

---

## Required Tool

```text
ProBuilder
```

หรือใช้ Cube / Plane ธรรมดาก็ได้ใน MVP

---

## Map Size

```text
Width: 800m
Depth: 800m
Center: (0, 0, 0)
Boundary: -400 to +400
```

---

## Required Landmarks

เมืองต้องมี landmark อย่างน้อย 7 จุด:

```text
1. Start Plaza
2. River Road
3. Night Market
4. Temple
5. Bridge
6. Gas Station
7. Hill Viewpoint
```

---

## Suggested Layout

```text
                 [Hill Viewpoint]
                       /\
                      /  \
       [Temple] ---- Road ---- [Gas Station]
          |                     |
          |                     |
 [Night Market] --- [Bridge] --- River Road
          |
      [Start Plaza]
```

---

## Landmark Details

### Start Plaza

```text
Size: 50m x 50m
Purpose: player spawn + race start
```

Objects:

```text
PlayerSpawn
RaceStartTrigger
Direction Sign
Road Exit
Open Plaza Ground
```

### River Road

```text
Road Width: 10–14m
Purpose: main race route
```

Requirements:

- มีโค้งยาว
- มี checkpoint วางตามทางได้
- มี barrier กันตกน้ำบางจุด
- ขับได้ต่อเนื่อง

### Night Market

```text
Purpose: visual landmark + narrow driving area
```

Objects:

```text
8–12 market stalls
Neon sign placeholders
Street lights
Small props
```

### Temple

```text
Purpose: Thai visual identity landmark
```

Objects:

```text
Temple gate
Main hall block
Wall boundary
Gold roof placeholder
```

### Bridge

```text
Purpose: route crossing river
```

Requirements:

```text
Width: 10–12m
Side barriers
Smooth ramp up/down
```

### Gas Station

```text
Purpose: roadside landmark
```

Objects:

```text
Canopy
Fuel pump blocks
Gas station sign
Small parking area
```

### Hill Viewpoint

```text
Purpose: vertical driving test + scenic endpoint
```

Requirements:

- ใช้ ramp road
- ไม่ชันเกินไป
- มี viewpoint platform
- รถขึ้นได้โดยไม่ค้าง

---

## Road Design Rules

```text
Main road width: 10–14m
Small road width: 7–10m
Minimum turn radius: 12–20m
Barrier height: 0.8–1.2m
Checkpoint spacing: 50–120m
```

---

## Acceptance Criteria

- [ ] เมืองมีขนาดประมาณ 800x800m
- [ ] มี landmark ครบ 7 จุด
- [ ] รถขับจาก Start Plaza ไปทุก landmark ได้
- [ ] ถนนกว้างพอให้เลี้ยว
- [ ] ไม่มีรูใหญ่ที่ทำให้รถตก
- [ ] รถข้าม bridge ได้
- [ ] รถขึ้น hill viewpoint ได้
- [ ] Race loop วิ่งได้ภายใน 60–180 วินาที
- [ ] Landmark อ่านออกแม้ยังเป็น greybox

---

## Performance Notes

- Static object ให้ mark เป็น Static
- ใช้ collider simple
- หลีกเลี่ยง MeshCollider ซับซ้อน
- อย่าเพิ่ม object เล็กจำนวนมากเกินไปใน greybox phase

---

# Phase 4 — Checkpoint Race System

## Objective

สร้างระบบแข่ง checkpoint ที่เริ่มแข่งได้, นับเวลาได้, ผ่าน checkpoint ตามลำดับได้ และจบ race ได้

---

## Required Scripts

```text
Assets/_Game/Scripts/Race/RaceManager.cs
Assets/_Game/Scripts/Race/Checkpoint.cs
Assets/_Game/Scripts/Race/RaceStartTrigger.cs
Assets/_Game/Scripts/Race/RaceMission.cs
```

---

## Required Prefabs

```text
Assets/_Game/Prefabs/Race/Checkpoint.prefab
Assets/_Game/Prefabs/Race/RaceStartTrigger.prefab
```

---

## RaceMission ScriptableObject

File:

```text
Assets/_Game/ScriptableObjects/Missions/RiverLoopRace.asset
```

Fields:

```text
Mission ID
Display Name
Description
Checkpoint List
Lap Count
Countdown Seconds
Gold Time
Silver Time
Bronze Time
Coin Reward
Best Time Save Key
```

---

## Example RaceMission

```text
Mission ID: river_loop_01
Display Name: River Loop Race
Description: Race around the river road and return to town.
Lap Count: 1
Checkpoint Count: 8–12
Countdown Seconds: 3
Gold Time: 75
Silver Time: 95
Bronze Time: 120
Coin Reward: 100
Best Time Save Key: best_river_loop_01
```

---

## RaceManager State Machine

```text
None
WaitingForStart
Countdown
Racing
Finished
Failed
```

---

## Race Flow

```text
1. Player drives into RaceStartTrigger
2. UI shows "Press E to start River Loop Race"
3. Player presses E
4. Countdown starts: 3, 2, 1, GO
5. Timer starts
6. Checkpoint 1 becomes active
7. Player passes checkpoints in order
8. Current checkpoint index updates
9. Final checkpoint triggers finish
10. Timer stops
11. Reward calculated
12. Best time saved
13. Result panel shown
14. Player can restart race
```

---

## Checkpoint Rules

- ใช้ Trigger Collider
- แต่ละ checkpoint มี index
- นับเฉพาะ checkpoint ที่ active
- ขับผ่าน checkpoint ผิดลำดับไม่ควรนับ
- Checkpoint ปัจจุบันควรมี visual ชัดกว่าอันอื่น
- หลังผ่าน checkpoint ให้เปิด checkpoint ถัดไป
- Last checkpoint จบ race

---

## RaceStartTrigger Rules

- แสดง prompt เฉพาะตอน player อยู่ใน trigger
- เริ่ม race เมื่อกด E
- ถ้า player ออกจาก trigger ให้ซ่อน prompt
- ถ้า race กำลังวิ่งอยู่ ห้ามเริ่มซ้ำ

---

## Acceptance Criteria

- [ ] Race ไม่ start เอง
- [ ] เข้า RaceStartTrigger แล้วขึ้น prompt
- [ ] กด E แล้ว countdown ทำงาน
- [ ] Timer เริ่มหลัง countdown
- [ ] Checkpoint active ทีละจุด
- [ ] ผ่าน checkpoint แล้ว index เพิ่ม
- [ ] ข้าม checkpoint ผิดลำดับไม่ count
- [ ] ผ่าน checkpoint สุดท้ายแล้ว race finish
- [ ] Timer หยุดเมื่อ finish
- [ ] Result panel แสดงผล
- [ ] Race restart ได้

---

# Phase 5 — HUD / UI

## Objective

สร้าง HUD สำหรับ gameplay และ race feedback โดยใช้ TextMeshPro

---

## Required Scripts

```text
Assets/_Game/Scripts/UI/HUDController.cs
Assets/_Game/Scripts/UI/RaceResultPanel.cs
```

---

## Required UI Hierarchy

```text
Canvas
  HUDPanel
    SpeedText
    TimerText
    CheckpointText
    CoinText
    StartPromptText

  RaceResultPanel
    TitleText
    TimeText
    BestTimeText
    RewardText
    RankText
    RestartHintText
```

---

## Recommended Canvas Settings

```text
Render Mode: Screen Space - Overlay
Canvas Scaler:
  UI Scale Mode: Scale With Screen Size
  Reference Resolution: 1920 x 1080
  Match: 0.5
```

---

## HUD Display States

### Normal Driving

```text
Speed: 42 km/h
Coins: 0
```

### Near Race Start

```text
Press E to start River Loop Race
```

### Countdown

```text
3
2
1
GO!
```

### During Race

```text
Speed: 64 km/h
Time: 00:42.32
Checkpoint: 5 / 10
Coins: 0
```

### Race Result

```text
Race Complete
Time: 01:18.45
Best: 01:18.45
Rank: Silver
Reward: +100 Coins
Press R to restart race
```

---

## Timer Format

```text
MM:SS.ms
```

Example:

```text
01:18.45
```

---

## Speed Format

ใช้ km/h:

```text
speedKmh = rigidbody.velocity.magnitude * 3.6f
```

Display:

```text
Speed: 64 km/h
```

---

## Acceptance Criteria

- [ ] UI ใช้ TextMeshPro
- [ ] Speed update real-time
- [ ] Timer format อ่านง่าย
- [ ] Checkpoint index update ถูกต้อง
- [ ] Start prompt show/hide ถูกต้อง
- [ ] Result panel hidden ตอนยังไม่จบ race
- [ ] Result panel แสดงหลังจบ race
- [ ] Coin แสดงถูกต้อง
- [ ] ไม่มี missing TMP reference ใน Inspector

---

# Phase 6 — Save / Reward System

## Objective

บันทึก coin และ best time ด้วย local JSON และยังอยู่หลังออก Play Mode / เปิดเกมใหม่

---

## Required Scripts

```text
Assets/_Game/Scripts/Save/SaveManager.cs
Assets/_Game/Scripts/Save/PlayerSaveData.cs
Assets/_Game/Scripts/Rewards/RewardSystem.cs
```

---

## Save File Path

```text
Application.persistentDataPath/thaitown_save.json
```

---

## PlayerSaveData Structure

```text
int coins
List<RaceBestTime> bestTimes
string lastUpdated
```

---

## RaceBestTime Structure

```text
string missionId
float bestTimeSeconds
```

---

## Important JSON Note

ถ้าใช้ Unity `JsonUtility` ไม่ควรใช้:

```text
Dictionary<string, float>
```

เพราะ serialize/deserialize ไม่สะดวกใน Unity JsonUtility

ให้ใช้:

```text
List<RaceBestTime>
```

แล้วเขียน helper method สำหรับ lookup/update best time แทน

---

## Save Flow

```text
1. Game starts
2. SaveManager loads JSON
3. If no save file, create default save data
4. Player finishes race
5. RewardSystem calculates reward
6. SaveManager adds coin
7. SaveManager checks best time
8. If new time is better, update best time
9. SaveManager writes JSON
10. HUD refreshes coin and best time
```

---

## Reward Rules

### Base MVP

```text
Finish Race = +100 coins
```

### Optional Rank Bonus

```text
Gold   = +100 coins
Silver = +75 coins
Bronze = +50 coins
Finish = +25 coins
```

สำหรับ MVP แนะนำใช้ simple rule ก่อน:

```text
Coin Reward จาก RaceMission
```

---

## Best Time Rule

```text
If no best time exists:
  save current time

If current time < saved best time:
  replace best time

If current time >= saved best time:
  keep old best time
```

---

## Debug Tools

ควรมี editor/debug function:

```text
Reset Save
Add Test Coins
Print Save Path
```

---

## Acceptance Criteria

- [ ] Run game ครั้งแรกแล้วสร้าง default save ได้
- [ ] Finish race แล้ว coin เพิ่ม
- [ ] Finish race แล้ว best time ถูกบันทึก
- [ ] Stop Play Mode แล้ว Play ใหม่ ข้อมูลยังอยู่
- [ ] เวลาที่แย่กว่าไม่ทับ best time เดิม
- [ ] Save file ถูกสร้างที่ persistentDataPath
- [ ] Reset save สำหรับ debug ได้
- [ ] ไม่มี JSON parse error

---

# Phase 7 — Low Poly Art Pass

## Objective

เปลี่ยน greybox บางส่วนเป็น low-poly asset เพื่อให้เกมเริ่มมี visual identity

---

## Required Texture

```text
Assets/_Game/Textures/palette_32.png
```

---

## Palette Requirement

Palette ควรมี 32 colors และครอบคลุมสีเหล่านี้:

```text
Asphalt dark grey
Concrete grey
River blue
Neon pink
Neon cyan
Warm market yellow
Temple gold
Roof red/orange
Grass green
Hill brown
Wood brown
Car base color
Car highlight
Window dark blue
Shadow purple
Light cream
```

---

## Recommended Import Settings for Palette

```text
Filter Mode: Point
Compression: None or Low Quality
sRGB: On
```

---

## Required Low Poly Assets

อย่างน้อย 10 assets:

```text
1. Player car
2. Market stall
3. Temple gate
4. Thai-style roof building
5. Bridge
6. Gas station canopy
7. Fuel pump
8. Street lamp
9. Checkpoint arch
10. Road sign
11. Tree / palm / bush
12. Rock / hill prop
```

---

## Replacement Priority

ให้เปลี่ยน greybox เป็น art ตามลำดับนี้:

```text
1. Car
2. Checkpoint
3. Night Market
4. Bridge
5. Temple
6. Gas Station
7. Roadside Props
8. Hill Viewpoint
```

---

## Low Poly Style Rules

- ใช้รูปทรงเรียบ
- ใช้ flat color
- ลด texture detail
- silhouette ต้องอ่านง่าย
- ใช้ material จาก palette เดียวกัน
- ไม่ต้อง high-poly
- collision ใช้ collider simple แยกจาก mesh
- หลีกเลี่ยง mesh ซับซ้อนเกินไป

---

## Car Art Requirements

รถควรมี:

```text
Body
Cabin
Wheels
Front light
Rear light
Window
Simple bumper
```

ยังไม่ต้องมี wheel physics จริงใน MVP

---

## Building Art Requirements

อาคารควรแยก landmark ได้ เช่น:

```text
Temple = roof gold/red + gate
Night Market = neon signs + stalls
Gas Station = canopy + pump
Bridge = side rail + river crossing
```

---

## Acceptance Criteria

- [ ] มี `palette_32.png`
- [ ] มี low-poly assets อย่างน้อย 10 ชิ้น
- [ ] รถไม่ใช่ cube ล้วนแล้ว
- [ ] Checkpoint ดูเด่นชัด
- [ ] Night Market มี visual identity
- [ ] Temple อ่านออกว่าเป็น temple
- [ ] Bridge อ่านออกว่าเป็น bridge
- [ ] Gas Station อ่านออกว่าเป็น gas station
- [ ] ไม่มี material สีชมพู missing
- [ ] Collision ยัง simple และเล่นได้ดี

---

# Phase 8 — Glow / Lighting / Polish

## Objective

เพิ่ม mood แบบ low-poly glow night town และ feedback ตอนขับ/แข่ง

---

## Visual Direction

```text
Theme: Thai small tourist town at night
Mood: Low Poly + Neon Glow
Tone: Fun, readable, arcade
```

---

## Lighting Direction

- ถนนค่อนข้างมืด
- Landmark มีไฟเฉพาะจุด
- Night Market มี neon pink/cyan/yellow
- Checkpoint ต้องเด่นชัด
- Gas Station มีไฟขาว/เหลือง
- Temple ใช้ warm/gold light
- Hill viewpoint มีไฟน้อยกว่าเมือง

---

## Bloom Setup

ถ้าใช้ URP:

```text
1. Create Global Volume
2. Add Bloom override
3. Enable Bloom
4. Adjust intensity carefully
5. Use emissive materials for glow objects
```

---

## Glow Materials

สร้าง material กลุ่มนี้:

```text
MAT_Glow_NeonPink
MAT_Glow_NeonCyan
MAT_Glow_WarmYellow
MAT_Glow_Checkpoint
MAT_Glow_GasStation
```

ใช้กับ:

```text
Market signs
Checkpoint arch/ring
Street lamps
Gas station sign
Car tail light
Race start marker
```

---

## VFX Requirements

### Dust Particle

ติดหลังรถ:

```text
Car_Blockout
  DustVFX
```

Rules:

- แสดงเมื่อรถกำลังวิ่ง
- ลด/หยุดเมื่อรถหยุด
- ไม่บังจอมากเกินไป

### Checkpoint Pass VFX

ตอนผ่าน checkpoint:

```text
Small burst
Short lifetime
Visible at speed
```

### Race Start VFX

ตอน GO:

```text
Flash or small ring pulse
```

---

## Audio Requirements

เพิ่ม audio ขั้นต้น:

```text
Engine loop
Checkpoint pass SFX
Countdown beep
Race start GO SFX
Race finish SFX
Simple BGM loop
```

---

## Audio Rules

- Engine volume ไม่ดังเกิน
- BGM loop ไม่รบกวน
- Checkpoint SFX ต้องได้ยินชัด
- Race finish SFX ต้องต่างจาก checkpoint
- ไม่มี audio clipping

---

## Acceptance Criteria

- [ ] Bloom ทำงาน
- [ ] Glow object มองเห็นชัด
- [ ] Night Market mood ชัดเจน
- [ ] Checkpoint มองเห็นง่ายตอนขับเร็ว
- [ ] Dust VFX ทำงานตอนรถวิ่ง
- [ ] Checkpoint VFX ทำงานตอนผ่าน checkpoint
- [ ] Engine audio เล่นได้
- [ ] Race SFX เล่นถูกจังหวะ
- [ ] BGM เล่นได้
- [ ] FPS ไม่ตกหนักจาก lights/VFX

---

# Phase 9 — Build Prototype

## Objective

เตรียม Windows x86_64 build และตรวจ prototype ให้เล่นได้จริง 5–10 นาที

---

## Build Target

```text
Platform: Windows
Architecture: x86_64
Output: ThaiTownRacer.exe
```

---

## Build Settings

```text
Scene:
  Assets/_Game/Scenes/ThaiTownRacer.unity

Development Build:
  Optional for internal test
  Off for playable prototype

Script Debugging:
  Off for playable prototype

Resolution:
  1920x1080 default
  Windowed optional
```

---

## Pre-Build Checklist

```text
No console errors
No missing scripts
No missing materials
No broken prefab references
Scene added to Build Settings
Player spawn works
Camera follows car
Race can start
Race can finish
Save persists
Result panel appears
Audio assigned
VFX assigned
```

---

## Playtest Script

### Minute 0–1: Start and Basic Drive

```text
1. Launch game
2. Confirm player spawns at Start Plaza
3. Press W/A/S/D
4. Confirm car drives correctly
5. Confirm camera follows car
```

### Minute 1–3: Explore Town

```text
1. Drive to Night Market
2. Drive across Bridge
3. Drive to Gas Station
4. Confirm no major collision problem
5. Confirm roads are wide enough
```

### Minute 3–5: Race Flow

```text
1. Drive to RaceStartTrigger
2. Confirm prompt appears
3. Press E to start race
4. Complete all checkpoints
5. Confirm timer and checkpoint UI
6. Finish race
7. Confirm result panel and reward
```

### Minute 5–10: Persistence and Replay

```text
1. Stop Play Mode / restart build
2. Confirm coin and best time persist
3. Replay race
4. Confirm best time update rule
5. Explore Hill Viewpoint / Temple
```

---

## Acceptance Criteria

- [ ] Windows `.exe` launches
- [ ] Player can drive immediately
- [ ] Camera follow works
- [ ] Town is playable
- [ ] Race can start
- [ ] Race can finish
- [ ] HUD works
- [ ] Save persists
- [ ] Reward works
- [ ] Result panel works
- [ ] No blocking console error
- [ ] No missing script
- [ ] No missing material
- [ ] Playable 5–10 minutes

---

# Final Definition of Done

MVP ถือว่าเสร็จเมื่อผ่านเงื่อนไขทั้งหมดนี้:

```text
1. มี Unity scene ThaiTownRacer.unity
2. มี Car_Blockout.prefab ที่ขับได้
3. มีกล้องตามรถแบบไม่กระตุก
4. มีเมือง blockout 800x800m
5. มี landmark หลักครบ 7 จุด
6. มี RaceManager + Checkpoint flow
7. มี RaceMission River Loop Race
8. มี HUD แสดง speed/timer/checkpoint/result
9. มี SaveManager สำหรับ JSON local save
10. มี RewardSystem สำหรับ coin/best time
11. มี low-poly art assets อย่างน้อย 10 ชิ้น
12. มี palette_32.png
13. มี Bloom / lighting / VFX / audio ขั้นต้น
14. Build Windows x86_64 ได้
15. เล่นได้ 5–10 นาทีโดยไม่มี blocker
```

---

# Suggested Claude Code Prompt

ใช้ prompt นี้กับ Claude Code เพื่อเริ่มทำงาน:

```text
You are helping me build a Unity 3D URP MVP prototype called ThaiTown Low Poly Glow Racing.

Please implement the project phase by phase based on this markdown spec.

Important rules:
- Do not generate everything at once.
- Start from Phase 0 and Phase 1 only.
- Use clean Unity C# scripts.
- Use Rigidbody-based arcade vehicle physics.
- Avoid transform-based physics movement.
- Create clear inspector fields.
- Add comments only where helpful.
- Keep the project simple and MVP-focused.
- After each phase, provide testing steps inside Unity Editor.

Current target:
Phase 0: project setup
Phase 1: vehicle blockout with ArcadeVehicleController.cs

Please create the required scripts and setup instructions for Unity Editor.
```

---

# Recommended Implementation Order

```text
Sprint 1:
  Phase 0
  Phase 1

Sprint 2:
  Phase 2
  Vehicle tuning

Sprint 3:
  Phase 3
  Greybox town

Sprint 4:
  Phase 4
  Race system

Sprint 5:
  Phase 5
  HUD/UI

Sprint 6:
  Phase 6
  Save/reward

Sprint 7:
  Phase 7
  Low-poly art pass

Sprint 8:
  Phase 8
  Glow/lighting/polish

Sprint 9:
  Phase 9
  Build prototype
```

---

# Risk List

## Risk 1 — Car spins too fast

Fix:

```text
Use FixedUpdate
Multiply steering by Time.fixedDeltaTime
Freeze Rotation X/Z
Increase Angular Drag
Reduce Turn Speed
Scale steering by forward speed
```

## Risk 2 — Camera jitter

Fix:

```text
Rigidbody Interpolate = Interpolate
Follow CameraTarget child
Use Cinemachine damping
Avoid transform movement in Update
```

## Risk 3 — JsonUtility cannot save Dictionary

Fix:

```text
Use List<RaceBestTime>
Write helper methods for lookup/update
```

## Risk 4 — Bloom not visible

Fix:

```text
Use URP
Create Global Volume
Add Bloom override
Use emissive materials
Check camera/post-processing settings
```

## Risk 5 — Build missing scene

Fix:

```text
Add ThaiTownRacer.unity to Build Settings
```

## Risk 6 — Claude Code creates too much at once

Fix:

```text
Ask Claude Code to implement only 1–2 phases per prompt
Test in Unity Editor after every phase
Commit/save working state before next phase
```
