[⬅ 回目錄](../README.md) | [⬅ 上一週：第 7 週](./week-07.md) | [下一週：第 9 週 — 停車場雷達 ➡](./week-09.md)

---

# 第 8 週：期中專題 — 智慧密室綜合監控站
## 主題專案：把七週所學全部串在一起

> 🎯 **本週定位**：這一週**沒有新元件、沒有新語法**。密室老闆要驗收成果了——你要把前 7 週學過的所有技巧（LED、按鈕、RGB、旋鈕、蜂鳴器、光敏電阻、溫度感測、伺服馬達）全部整合到同一個完整專案中，打造一套「智慧密室綜合監控站」。這是檢驗這一個半月學習成果最重要的里程碑，也是這學期第一次體驗「系統整合」的完整開發流程：**硬體佈局 → 個別測試 → 核心邏輯撰寫 → 邊界測試 → 展示優化**。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [系統架構總覽](#系統架構總覽)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道整合關卡](#後端程式設計五道整合關卡)
  - [關卡 1：硬體總體檢 — 逐一驗證每個元件](#關卡-1硬體總體檢--逐一驗證每個元件)
  - [關卡 2：核心邏輯骨架 — 建立模組化函式架構](#關卡-2核心邏輯骨架--建立模組化函式架構)
  - [關卡 3：環境監控子系統 — 光線與溫度整合](#關卡-3環境監控子系統--光線與溫度整合)
  - [關卡 4：互動控制子系統 — 按鈕、旋鈕、馬達整合](#關卡-4互動控制子系統--按鈕旋鈕馬達整合)
  - [關卡 5：安全優先權設計 — 緊急停止與連鎖邏輯](#關卡-5安全優先權設計--緊急停止與連鎖邏輯)
  - [🏆 BOSS 關：智慧密室綜合監控站完整系統](#-boss-關智慧密室綜合監控站完整系統)
- [QA 品質保證：邊界測試清單](#qa-品質保證邊界測試清單)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎨 期中專題加碼挑戰](#-期中專題加碼挑戰)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [期中專題檢核清單與自評表](#期中專題檢核清單與自評表)

---

## 學習地圖

### 這一個半月你已經會的所有技能
| 週次 | 核心技能 |
|---|---|
| 第 1 週 | `digitalWrite`、`for` 迴圈、陣列、自訂函式 |
| 第 2 週 | `digitalRead`、`if-else`、防彈跳、狀態變數、`&&`/`\|\|` |
| 第 3 週 | `analogWrite`（PWM）、RGB 混色、帶參數函式、二維陣列 |
| 第 4 週 | `analogRead`、`map()`、`constrain()` |
| 第 5 週 | `tone()`/`noTone()`、光敏電阻、狀態轉變偵測 |
| 第 6 週 | TMP36 溫度換算、`float`、警報鎖定與人工復歸 |
| 第 7 週 | `<Servo.h>`、物件導向雛形、安全連鎖 |

### 這週不學新語法，而是學這件事
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| 系統架構設計 | 先規劃整體架構，再逐步填入細節 | 任何專案開發的第一步都是架構設計，而非直接寫程式 |
| 模組化開發 | 把大系統拆成獨立的子函式，各自開發、各自測試 | 大幅降低除錯難度，方便團隊分工合作 |
| 邊界測試（QA） | 刻意用極端數值測試系統穩定性 | 產品上市前的必經流程，工程師的專業素養 |
| 優先權設計 | 決定哪些邏輯（如安全條件）必須優先於其他邏輯執行 | 避免系統在多重條件衝突時做出錯誤反應 |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室老闆下週要帶投資人來參觀，要求你在這週把之前分散在各個房間的機關技術，整合成一套「總控室監控站」——集中顯示所有房間的環境狀態（光線、溫度）、控制核心機關（伺服馬達閘門）、並具備緊急應變能力（安全連鎖、警報系統）。這是你這學期第一次面對「不是學一個新東西，而是把很多舊東西組裝成一個更大的系統」的挑戰，這正是真實軟體工程師最常做的工作型態。
>
> 這週結束時，你會做出一套完整的「智慧密室綜合監控站」，具備以下功能：
> 1. 環境光線自動照明
> 2. 溫度異常警報（含鎖定與人工復歸）
> 3. 旋鈕手動測試與參數調整
> 4. 伺服馬達閘門控制（含容納量管制）
> 5. 緊急停止機制（最高優先權）
> 6. 完整的聲光反饋系統

---

## 系統架構總覽

在動手接線與寫程式之前，先花時間理解整個系統的架構藍圖，這是專業工程師開發任何系統前必做的第一步。

```
┌─────────────────────────────────────────────────┐
│              智慧密室綜合監控站                    │
├─────────────────────────────────────────────────┤
│                                                   │
│  輸入層（感測與操作）      輸出層（反饋與動作）       │
│  ─────────────────      ─────────────────       │
│  • 光敏電阻 (A1)          • RGB LED 狀態燈 (~9~11) │
│  • TMP36 溫度 (A2)        • 蜂鳴器警報 (Pin 8)     │
│  • 旋鈕 (A0)              • 伺服馬達閘門 (Pin 6)   │
│  • 進場按鈕 (Pin 7)        • 照明 LED (Pin 13)     │
│  • 緊急停止鈕 (Pin 4)                              │
│                                                   │
├─────────────────────────────────────────────────┤
│                邏輯判斷核心（loop()）                │
│  優先權順序：                                       │
│  ① 緊急停止（最高優先權，強制關閉所有輸出）             │
│  ② 溫度安全連鎖（異常時鎖定閘門功能）                  │
│  ③ 環境自動照明                                    │
│  ④ 閘門容納量管制                                   │
│  ⑤ 一般狀態顯示                                     │
└─────────────────────────────────────────────────┘
```

這張架構圖說明了兩件事：**「資料如何流動」**（感測器讀值 → 邏輯判斷 → 驅動輸出裝置）與**「優先權如何排序」**（安全相關的判斷永遠排在最前面）。這學期後半段的所有專題，都會沿用這種「先畫架構圖、再寫程式」的開發習慣。

---

## 模擬設計專案配置

### 元件總表（本週不新增任何元件，全部沿用前 7 週）
| 元件 | 數量 | 對應週次 | 腳位 |
|---|---|---|---|
| LED（照明燈） | 1 | 第 1 週 | Pin 13 |
| Pushbutton（進場按鈕） | 1 | 第 2 週 | Pin 7 |
| Pushbutton（緊急停止鈕） | 1 | 第 2 週 | Pin 4 |
| 10kΩ 電阻 ×2 | 2 | 第 2 週 | 搭配上述按鈕 |
| RGB LED（共陰極） | 1 | 第 3 週 | ~9 / ~10 / ~11 |
| 220Ω 電阻 ×3 | 3 | 第 3 週 | 搭配 RGB LED |
| Potentiometer | 1 | 第 4 週 | A0 |
| Buzzer | 1 | 第 5 週 | Pin 8 |
| Photoresistor | 1 | 第 5 週 | A1 |
| 10kΩ 電阻（LDR 分壓用） | 1 | 第 5 週 | 搭配 LDR |
| TMP36 | 1 | 第 6 週 | A2 |
| Micro Servo | 1 | 第 7 週 | Pin 6 |

### 佈線建議
麵包板空間有限，建議依照以下分區規劃，讓接線更有條理：
- **左半部**：所有輸入類元件（兩顆按鈕、旋鈕、光敏電阻、TMP36）
- **右半部**：所有輸出類元件（LED、RGB LED、蜂鳴器、伺服馬達）
- **電源軌**：正負電源軌貫穿整塊麵包板共用，避免每個元件都各自接一條線回 Arduino

### 完整腳位對照表
| 元件 | 腳位 |
|---|---|
| 照明 LED | Pin 13 |
| 進場按鈕 | Pin 7 |
| 緊急停止鈕 | Pin 4 |
| RGB LED - R / G / B | ~9 / ~10 / ~11 |
| 旋鈕 | A0 |
| 光敏電阻 | A1 |
| TMP36 | A2 |
| 蜂鳴器 | Pin 8 |
| 伺服馬達 | Pin 6 |

> 💡 **配置檢查清單**
> - [ ] 12 個元件全部正確接線（可對照上表逐一勾選）
> - [ ] 所有負極/接地線都確實接回同一個 GND 系統
> - [ ] 沒有兩個元件誤用同一個腳位

---

## 後端程式設計：五道整合關卡

### 關卡 1：硬體總體檢 — 逐一驗證每個元件

**機關故事**：在寫任何複雜邏輯之前，專業工程師的第一步永遠是「先確認每個元件個別都能正常運作」，這比一次寫完整個系統再除錯有效率得多。

#### 範例 1-1：逐一測試所有輸出裝置
```cpp
// ============================================
// 範例 1-1：依序測試每個輸出裝置是否正常運作
// 測試原則：一次只測一個裝置，方便定位問題
// ============================================

#include <Servo.h>

int pinLed = 13;
int pinR = 9, pinG = 10, pinB = 11;
int pinBuzzer = 8;
Servo gateServo;

void setup() {
  pinMode(pinLed, OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  gateServo.attach(6);
  Serial.begin(9600);
}

void loop() {
  Serial.println("測試：照明 LED");
  digitalWrite(pinLed, HIGH);
  delay(500);
  digitalWrite(pinLed, LOW);
  delay(500);

  Serial.println("測試：RGB LED 紅色");
  analogWrite(pinR, 255); analogWrite(pinG, 0); analogWrite(pinB, 0);
  delay(500);
  analogWrite(pinR, 0);

  Serial.println("測試：蜂鳴器");
  tone(pinBuzzer, 1000, 300);
  delay(500);

  Serial.println("測試：伺服馬達");
  gateServo.write(90);
  delay(500);
  gateServo.write(0);
  delay(1000);
}
```

#### 範例 1-2：逐一測試所有輸入裝置
```cpp
// ============================================
// 範例 1-2：確認所有感測器與按鈕都能正確讀值
// ============================================

int pinButton = 7;
int pinEmergency = 4;
int pinPot = A0;
int pinLdr = A1;
int pinTemp = A2;

void setup() {
  pinMode(pinButton, INPUT);
  pinMode(pinEmergency, INPUT);
  Serial.begin(9600);
}

void loop() {
  Serial.print("進場鈕: "); Serial.print(digitalRead(pinButton));
  Serial.print("  緊急鈕: "); Serial.print(digitalRead(pinEmergency));
  Serial.print("  旋鈕: "); Serial.print(analogRead(pinPot));
  Serial.print("  光線: "); Serial.print(analogRead(pinLdr));
  Serial.print("  溫度原始值: "); Serial.println(analogRead(pinTemp));
  delay(300);
}
```
**教學重點**：這種「把所有感測器讀值印在同一行」的除錯手法，是這學期第一次真正意義上的「系統健檢」，往後每次開發新專題，第一步都應該先做類似的總體檢，確認所有硬體都連接正確，再開始撰寫邏輯功能。

---

### 關卡 2：核心邏輯骨架 — 建立模組化函式架構

**機關故事**：確認硬體都正常後，接下來要規劃程式碼的「骨架」——把整個系統拆成幾個獨立的函式，各自負責一部分功能，這是專業軟體開發的標準做法。

#### 範例 2-1：建立空白的函式骨架
```cpp
// ============================================
// 範例 2-1：先寫出所有函式的「空殼」，確立整體架構
// 好處：可以先確認架構合理，再逐一填入細節，不會迷失在細節中
// ============================================

#include <Servo.h>

// --- 腳位宣告區 ---
int pinLed = 13;
int pinButton = 7;
int pinEmergency = 4;
int pinR = 9, pinG = 10, pinB = 11;
int pinPot = A0;
int pinLdr = A1;
int pinTemp = A2;
int pinBuzzer = 8;
Servo gateServo;

// --- 函式宣告區（先寫空殼） ---
void checkEmergency() {
  // 之後會在這裡填入緊急停止邏輯
}

void updateLighting() {
  // 之後會在這裡填入自動照明邏輯
}

void checkTemperature() {
  // 之後會在這裡填入溫度監控邏輯
}

void handleGate() {
  // 之後會在這裡填入閘門控制邏輯
}

void setup() {
  pinMode(pinLed, OUTPUT);
  pinMode(pinButton, INPUT);
  pinMode(pinEmergency, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  gateServo.attach(6);
  Serial.begin(9600);
}

void loop() {
  checkEmergency();
  updateLighting();
  checkTemperature();
  handleGate();
}
```
**教學重點**：這是這學期最重要的架構設計示範——`loop()` 現在讀起來就像一份「劇本大綱」，清楚列出系統要做的四件事，完全看不到任何實作細節。這種寫法讓程式碼的「可讀性」大幅提升，也讓後續每個函式可以獨立開發、獨立測試，互不干擾。

#### 範例 2-2：填入第一個函式 — 自動照明
```cpp
// ============================================
// 範例 2-2：先完成 updateLighting()，驗證架構可行
// ============================================

int pinLdr = A1;
int pinLed = 13;

void updateLighting() {
  int lightLevel = analogRead(pinLdr);
  if (lightLevel < 300) {
    digitalWrite(pinLed, HIGH);
  } else {
    digitalWrite(pinLed, LOW);
  }
}

void setup() {
  pinMode(pinLed, OUTPUT);
}

void loop() {
  updateLighting();
}
```

---

### 關卡 3：環境監控子系統 — 光線與溫度整合

**機關故事**：把第 5 週與第 6 週學過的光線與溫度監控邏輯，整合進骨架中的對應函式。

#### 範例 3-1：完整的溫度監控函式（含警報鎖定與人工復歸）
```cpp
// ============================================
// 範例 3-1：把第 6 週學過的完整溫度邏輯，封裝成一個函式
// ============================================

int pinTemp = A2;
int pinBuzzer = 8;
int pinEmergency = 4;   // 這裡借用緊急停止鈕，模擬復歸鈕的功能
float tempThreshold = 32.0;
bool tempAlarmLatched = false;

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void checkTemperature() {
  float celsius = readTemperature();

  if (celsius > tempThreshold) {
    tempAlarmLatched = true;
  }
  if (digitalRead(pinEmergency) == HIGH && celsius <= tempThreshold) {
    tempAlarmLatched = false;
  }

  if (tempAlarmLatched) {
    tone(pinBuzzer, 2000);
  } else {
    noTone(pinBuzzer);
  }
}

void setup() {
  pinMode(pinEmergency, INPUT);
}

void loop() {
  checkTemperature();
}
```

#### 範例 3-2：把光線與溫度整合進 RGB 燈號顯示
```cpp
// ============================================
// 範例 3-2：用 RGB LED 同時呈現光線與溫度的綜合狀態
// ============================================

int pinLdr = A1;
int pinTemp = A2;
int pinR = 9, pinG = 10, pinB = 11;
float tempThreshold = 32.0;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void updateStatusLight() {
  float celsius = readTemperature();
  int lightLevel = analogRead(pinLdr);

  if (celsius > tempThreshold) {
    setColor(255, 0, 0);       // 溫度異常：紅色優先顯示
  } else if (lightLevel < 300) {
    setColor(0, 0, 255);       // 夜間模式：藍色
  } else {
    setColor(0, 255, 0);       // 一切正常：綠色
  }
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  updateStatusLight();
}
```
**教學重點**：注意這裡的 `if-else if-else` 判斷順序——溫度異常的判斷寫在最前面，代表它擁有最高的顯示優先權，即使當下同時是夜間、光線又暗，只要溫度異常，燈號一定優先顯示紅色。這種「多重條件下決定顯示優先順序」的思考方式，是這週期中專題最重要的訓練重點之一。

---

### 關卡 4：互動控制子系統 — 按鈕、旋鈕、馬達整合

**機關故事**：把第 4 週的旋鈕、第 2 週的按鈕與第 7 週的伺服馬達整合成完整的閘門控制子系統。

#### 範例 4-1：閘門控制函式（含容納量管制）
```cpp
// ============================================
// 範例 4-1：把第 7 週學過的閘門邏輯封裝成函式
// ============================================

#include <Servo.h>

Servo gateServo;
int pinButton = 7;
int lastButtonState = LOW;
int occupantCount = 0;
int maxCapacity = 5;

void handleGate() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    if (occupantCount < maxCapacity) {
      occupantCount++;
      gateServo.write(90);
      delay(1500);
      gateServo.write(0);
    }
  }
  lastButtonState = currentState;
}

void setup() {
  gateServo.attach(6);
  pinMode(pinButton, INPUT);
}

void loop() {
  handleGate();
}
```

#### 範例 4-2：旋鈕作為現場手動測試工具
```cpp
// ============================================
// 範例 4-2：旋鈕不是用來控制正式功能，而是提供「現場手動測試模式」
// 情境：維修人員可以隨時切換到手動模式，直接用旋鈕測試馬達角度
// ============================================

#include <Servo.h>

Servo gateServo;
int pinPot = A0;
int pinButton = 7;
int lastButtonState = LOW;
bool manualTestMode = false;

void setup() {
  gateServo.attach(6);
  pinMode(pinButton, INPUT);
}

void checkManualMode() {
  // 這裡示範一種簡化判斷：如果旋鈕被轉到接近最大值，視為進入手動測試模式
  int potValue = analogRead(pinPot);
  manualTestMode = potValue > 1000;
}

void loop() {
  checkManualMode();

  if (manualTestMode) {
    int potValue = analogRead(pinPot);
    int angle = map(potValue, 0, 1023, 0, 180);
    gateServo.write(angle);
  } else {
    // 一般模式：維持正常關閉狀態，等待按鈕觸發
    gateServo.write(0);
  }
}
```

---

### 關卡 5：安全優先權設計 — 緊急停止與連鎖邏輯

**機關故事**：整個系統最重要的一環——緊急停止機制必須擁有絕對最高的優先權，不管系統當下處於什麼狀態，只要緊急停止被觸發，一切都要立刻停止。

#### 範例 5-1：緊急停止函式
```cpp
// ============================================
// 範例 5-1：緊急停止邏輯 - 強制關閉所有輸出
// ============================================

#include <Servo.h>

int pinEmergency = 4;
int pinLed = 13;
int pinR = 9, pinG = 10, pinB = 11;
int pinBuzzer = 8;
Servo gateServo;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

bool checkEmergency() {
  bool emergencyActive = digitalRead(pinEmergency) == HIGH;

  if (emergencyActive) {
    digitalWrite(pinLed, LOW);
    setColor(0, 0, 0);
    noTone(pinBuzzer);
    gateServo.write(0);
  }

  return emergencyActive;   // 回傳結果，讓 loop() 知道是否要跳過後續邏輯
}

void setup() {
  pinMode(pinEmergency, INPUT);
  pinMode(pinLed, OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  gateServo.attach(6);
}

void loop() {
  if (checkEmergency()) {
    return;   // 緊急停止觸發，立刻結束本輪迴圈，跳過所有其他邏輯
  }

  // 這裡才會執行一般的系統邏輯（本範例先省略，BOSS 關會完整呈現）
}
```
**教學重點**：`checkEmergency()` 這個函式回傳一個 `bool` 值，讓 `loop()` 能夠判斷「這一輪要不要繼續往下執行」。這種「函式回傳布林值，決定後續流程」的寫法，是這學期第一次接觸的重要設計模式，你會發現它比單純用一堆巢狀 `if-else` 更清晰易讀。

---

### 🏆 BOSS 關：智慧密室綜合監控站完整系統

**機關故事**：整合本週所有子系統，打造完整的智慧密室綜合監控站——這是這學期目前為止規模最大、功能最完整的專案。

```cpp
// ============================================
// BOSS 關：智慧密室綜合監控站 - 期中專題完整系統
// 架構：緊急停止（最高優先權）→ 溫度安全連鎖 → 環境監控 → 閘門控制
// ============================================

#include <Servo.h>

// --- 腳位宣告區 ---
int pinLed = 13;
int pinButton = 7;
int pinEmergency = 4;
int pinR = 9, pinG = 10, pinB = 11;
int pinPot = A0;
int pinLdr = A1;
int pinTemp = A2;
int pinBuzzer = 8;
Servo gateServo;

// --- 系統參數區 ---
float tempThreshold = 32.0;
int lightThreshold = 300;
int maxCapacity = 5;

// --- 狀態變數區 ---
bool tempAlarmLatched = false;
int lastButtonState = LOW;
int occupantCount = 0;

// --- 工具函式區 ---
void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

// --- 子系統函式區 ---

// ① 緊急停止（最高優先權）
bool checkEmergency() {
  bool emergencyActive = digitalRead(pinEmergency) == HIGH;
  if (emergencyActive) {
    digitalWrite(pinLed, LOW);
    setColor(0, 0, 0);
    noTone(pinBuzzer);
    gateServo.write(0);
    Serial.println("!!! 緊急停止已觸發，系統全面暫停 !!!");
  }
  return emergencyActive;
}

// ② 溫度安全連鎖
bool checkTemperatureSafety() {
  float celsius = readTemperature();

  if (celsius > tempThreshold) {
    tempAlarmLatched = true;
  }
  // 溫度恢復正常後，警報會在下方環境監控函式中視情況解除（此處僅負責判斷是否鎖定）

  if (tempAlarmLatched) {
    setColor(255, 0, 0);
    tone(pinBuzzer, 2000);
    delay(80);
    noTone(pinBuzzer);
    delay(80);
  }

  return tempAlarmLatched;   // true 代表系統應鎖定閘門功能
}

// ③ 環境監控（照明 + 狀態燈）
void updateEnvironment() {
  int lightLevel = analogRead(pinLdr);
  digitalWrite(pinLed, lightLevel < lightThreshold ? HIGH : LOW);

  if (!tempAlarmLatched) {
    if (lightLevel < lightThreshold) {
      setColor(0, 0, 255);   // 夜間模式：藍色
    } else {
      setColor(0, 255, 0);   // 正常：綠色
    }
  }
}

// ④ 閘門控制（含容納量管制）
void handleGate() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    if (occupantCount < maxCapacity) {
      occupantCount++;
      Serial.print("玩家進場，目前人數：");
      Serial.println(occupantCount);
      tone(pinBuzzer, 1200, 150);
      gateServo.write(90);
      delay(1500);
      gateServo.write(0);
    } else {
      Serial.println("已達容納上限，閘門拒絕開啟！");
      tone(pinBuzzer, 400, 150);
    }
  }
  lastButtonState = currentState;
}

void setup() {
  pinMode(pinLed, OUTPUT);
  pinMode(pinButton, INPUT);
  pinMode(pinEmergency, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  gateServo.attach(6);
  gateServo.write(0);
  Serial.begin(9600);
  Serial.println("=== 智慧密室綜合監控站啟動 ===");
}

void loop() {
  // 優先權 ①：緊急停止，最高優先權，一旦觸發立刻結束本輪
  if (checkEmergency()) {
    return;
  }

  // 優先權 ②：溫度安全連鎖，異常時鎖定閘門功能
  bool isLocked = checkTemperatureSafety();

  // 優先權 ③：環境監控（在溫度正常時才更新一般狀態顯示）
  updateEnvironment();

  // 優先權 ④：閘門控制（溫度異常鎖定時，跳過此功能）
  if (!isLocked) {
    handleGate();
  }
}
```
**教學重點**：這是這學期目前為止最完整的「多層優先權」系統設計——`loop()` 的四行呼叫，清楚呈現了整個系統的執行順序與優先權邏輯。這種寫法完全體現了關卡 2 提到的「模組化開發」精神：每個函式各自獨立、職責單一，整合起來卻能完成複雜的系統行為。**這正是這學期到目前為止，最接近真實軟體工程專案的一次完整演練。**

---

## QA 品質保證：邊界測試清單

完成 BOSS 關程式碼後，請依照以下清單逐一進行「刁難測試」，這是專業軟體開發流程中不可或缺的一環：

| 測試項目 | 測試方式 | 預期結果 |
|---|---|---|
| 緊急停止優先權 | 同時觸發溫度異常與緊急停止 | 系統應完全靜止，不受溫度異常影響 |
| 溫度鎖定閘門 | 溫度異常時嘗試按下進場按鈕 | 閘門不應有任何反應 |
| 容納量上限 | 連續按壓進場按鈕超過 5 次 | 第 6 次起應顯示拒絕訊息，馬達不動作 |
| 夜間模式與溫度警報衝突 | 在光線昏暗的同時觸發溫度異常 | 應優先顯示紅色（溫度警報），而非藍色（夜間模式） |
| 快速連續操作 | 快速連續按壓進場按鈕與緊急停止鈕 | 系統不應出現邏輯錯亂或當機 |
| 旋鈕極端值 | 旋鈕轉到最左與最右極限 | 手動測試模式應正常切換，不應出現異常角度 |

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| 緊急停止觸發後，系統似乎「卡住」無法恢復 | `return;` 導致每輪迴圈都提前結束，這其實是正確的設計行為 | 這不是錯誤！只要放開緊急停止鈕，下一輪迴圈就會恢復正常，這正是安全連鎖該有的行為 |
| 溫度異常鎖定後，閘門仍然可以開啟 | `handleGate()` 沒有被正確包在 `if (!isLocked)` 判斷內 | 檢查 `loop()` 中呼叫 `handleGate()` 的位置是否正確加上條件判斷 |
| 多個函式之間互相影響、出現無法預期的行為 | 函式之間共用的全域變數被不同函式意外修改 | 檢查每個函式修改的變數是否符合設計預期，必要時加上 Serial 除錯輸出追蹤 |
| 系統邏輯順序調換後行為完全不同 | `loop()` 中呼叫函式的順序，直接決定了優先權高低 | 這正是這週的教學重點：呼叫順序即優先權順序，務必謹慎安排 |
| Serial Monitor 訊息量太大看不清楚 | 太多函式都在做 `Serial.println()` | 可以暫時註解掉不需要的除錯輸出，只保留當下關注的部分 |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改 BOSS 關程式碼的 `tempThreshold`，從 32.0 改成 30.0，並測試效果。
2. 修改 `maxCapacity`，從 5 改成 3，測試容納量管制是否正確運作。
3. 在 `checkEmergency()` 函式中，新增一行 Serial 訊息，印出「所有裝置已強制關閉」。
4. 修改 `updateEnvironment()`，把光線門檻從 300 改成 400。
5. 幫 `handleGate()` 新增功能：每次成功開門，額外閃爍一次照明 LED 作為視覺提示。

### 🟡 中等題
6. 新增一個「離場按鈕」（可沿用第 7 週練習題 7 的邏輯），讓 `occupantCount` 可以遞減。
7. 設計一個「溫度警戒」（尚未達到鎖定門檻但已偏高）的中間狀態，此時 RGB 顯示黃色，但閘門仍可正常使用。
8. 幫緊急停止機制新增「解除確認」邏輯：緊急停止鈕放開後，還需要額外按一次進場按鈕才能真正恢復系統運作（模擬「二次確認」的安全設計）。
9. 統計「系統啟動至今，緊急停止總共被觸發幾次」，並在每次觸發時印出累計次數。
10. 設計一個「維護模式」：當旋鈕轉到最小值（接近 0）時，系統進入維護模式，此時所有一般功能暫停，只顯示各感測器的即時原始讀值方便除錯。

### 🔴 挑戰題
11. **多重異常優先權排序**：目前系統只有「緊急停止」與「溫度異常」兩種會鎖定系統的條件。如果新增第三種異常（例如自行定義「光線感測器讀值異常波動」），你會把它排在優先權順序的哪個位置？為什麼？
12. **狀態機重構挑戰**：嘗試把 BOSS 關程式碼中「系統目前是否被鎖定」的邏輯，改用第 6 週提過的 `enum` 概念（可自行查詢語法），定義一個 `SystemState` 列舉型別（例如 NORMAL、TEMP_LOCKED、EMERGENCY），取代目前用多個布林變數判斷的寫法。
13. **事件日誌系統**：設計一個簡易的「事件記錄」機制，把系統運作過程中發生的重要事件（開門、溫度異常、緊急停止觸發）依序記錄在一個字串陣列中，並能在 Serial Monitor 印出「最近 5 筆事件」。
14. **多重感測器投票機制**：想像系統加裝了第二顆 TMP36（需自行擴充接線），設計一個「兩個溫度感測器同時異常才觸發鎖定」的判斷邏輯，模擬工業上常見的「多重感測器交叉驗證」設計，避免單一感測器故障造成誤判。
15. **完整系統重新設計**：不修改 BOSS 關程式碼，而是完全重新設計一套「你認為更合理的密室監控站架構」，至少要包含：4 個獨立子函式、明確的優先權順序、至少一種安全連鎖機制。並寫一段文字（5-6 句話）說明你的架構設計理念與 BOSS 關版本的差異。

---

## 🎨 期中專題加碼挑戰

如果你已經熟練掌握 BOSS 關的完整系統，這裡提供更具開放性的加碼挑戰，作為期中專題的加分項目：

> **加碼挑戰 A：訪客體驗優化**
> 設計一套「開機迎賓」流程：系統啟動時，先讓 RGB LED 依序播放一輪彩虹效果（可複習第 3 週的技巧），搭配蜂鳴器播放一小段歡迎旋律（複習第 5 週的技巧），最後才進入正常監控模式。

> **加碼挑戰 B：容納量視覺化**
> 想像新增 5 顆額外的 LED（需自行擴充接線），設計一個「容納量長條圖」——目前有幾位訪客在場內，就點亮對應數量的燈（複習第 4 週的長條圖技巧）。

> **加碼挑戰 C：自動化壓力測試腳本**
> 設計一段「自我測試模式」程式碼：系統開機後自動依序模擬觸發各種異常情境（不需要真的操作感測器，而是暫時用固定數值取代真實讀值），驗證系統各項安全機制是否正常運作，這模擬了真實軟體工程中的「自動化測試」概念。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| 系統架構（System Architecture） | 系統架構 | 描述系統各部件如何組織、互相關聯的整體藍圖 |
| 模組化（Modularization） | 模組化 | 把大系統拆解成多個獨立、職責單一的小單元 |
| 優先權（Priority） | 優先權 | 決定多個條件同時成立時，何者應優先被處理 |
| 安全連鎖（Safety Interlock） | 安全連鎖 | 確保安全條件優先滿足，否則鎖定其他功能的設計原則 |
| QA（Quality Assurance） | 品質保證 | 透過系統化測試，確保產品在各種情境下都穩定可靠 |
| 邊界測試（Boundary Testing） | 邊界測試 | 刻意用極端或臨界數值測試系統穩定性 |
| 回傳布林值決策 | 回傳布林值決策 | 函式回傳 true/false，讓呼叫端決定後續流程走向 |

---

## 反思與延伸討論

1. 這週的核心精神是「模組化」——把系統拆成多個各自獨立的函式。請討論：如果沒有採用模組化設計，把所有邏輯擠在同一個 `loop()` 裡面寫成一長串程式碼，對於「除錯」與「日後修改」會造成什麼樣的困難？
2. BOSS 關程式碼的 `loop()` 中，四個子系統函式的呼叫順序就是優先權順序。請討論：如果今天把「環境監控」的呼叫順序調到「緊急停止」判斷之前，會發生什麼問題？這說明了什麼重要的程式設計原則？
3. QA 品質保證測試清單裡的「邊界測試」，故意用各種極端情境（同時觸發多個條件、快速連續操作）去「刁難」系統。請討論：為什麼工程師需要主動尋找系統的弱點，而不是等到使用者發現問題才修正？這種心態對於未來你們在職場上開發任何系統（不限於程式），會有什麼樣的啟發？

---

## 期中專題檢核清單與自評表

### 功能完整性檢核
- [ ] 緊急停止機制正常運作，且具備最高優先權
- [ ] 溫度監控具備警報鎖定與人工復歸功能
- [ ] 環境光線自動照明正常運作
- [ ] 閘門控制具備容納量管制
- [ ] 系統各子函式模組化清楚，`loop()` 保持精簡易讀

### 自我評分表（1~5 分，5 分為最高）
| 評分項目 | 自評分數 |
|---|---|
| 硬體接線正確性（是否 12 個元件都正確接線） | ___ / 5 |
| 程式碼可讀性（變數命名、註解是否清楚） | ___ / 5 |
| 邏輯完整性（是否涵蓋所有優先權情境） | ___ / 5 |
| 邊界測試表現（QA 測試清單通過情況） | ___ / 5 |
| 個人挑戰題完成度（完成多少中等/挑戰題） | ___ / 5 |

完成自評後，建議找同組或鄰座同學互相測試彼此的系統，用 QA 品質保證測試清單互相「刁難」對方的作品，這是這學期第一次進行同儕互評，能幫助你們發現自己沒注意到的設計死角。

---

## 下週預告

期中專題告一段落，第 9 週我們要進入第三階段——工業物聯網基礎元件。密室老闆想打造一間「停車場雷達」主題房，你將學會使用超音波測距模組，讓機關真正具備「感知空間距離」的能力，這也是後續幾週動力控制與自動避障邏輯的重要基礎。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 7 週](./week-07.md) | [下一週：第 9 週 — 停車場雷達 ➡](./week-09.md)
