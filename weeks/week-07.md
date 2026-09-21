[⬅ 回目錄](../README.md) | [⬅ 上一週：第 6 週](./week-06.md) | [下一週：第 8 週 — 期中專題 ➡](./week-08.md)

---

# 第 7 週：機械保全閘門
## 主題專案：從「顯示訊息」到「真正動起來」

> 🎯 **本週定位**：前六週我們做的所有機關，輸出裝置都是燈光或聲音——本質上都是「顯示狀態給人看／聽」。這一週你會第一次讓 Arduino **驅動真正的機械動作**——伺服馬達。密室老闆想打造一間「機械保全閘門」主題房，玩家解開謎題後，實體閘門會真的轉動開啟。這是這學期第一次接觸「精準角度控制」，也是為後續更複雜的動力控制（第 10 週的直流馬達、第 14 週的繼電器）打下最重要的基礎。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [電路體檢站（3 分鐘快速複習）](#電路體檢站3-分鐘快速複習)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道機關關卡](#後端程式設計五道機關關卡)
  - [關卡 1：馬達初體驗 — 認識 Servo.h 函式庫](#關卡-1馬達初體驗--認識-servoh-函式庫)
  - [關卡 2：平滑掃描 — 連續角度變化](#關卡-2平滑掃描--連續角度變化)
  - [關卡 3：按鈕觸發閘門開關](#關卡-3按鈕觸發閘門開關)
  - [關卡 4：感測連動 — 旋鈕與溫度控制馬達角度](#關卡-4感測連動--旋鈕與溫度控制馬達角度)
  - [關卡 5：自動化閘門邏輯 — 延遲自動關閉與流量管制](#關卡-5自動化閘門邏輯--延遲自動關閉與流量管制)
  - [🏆 BOSS 關：機械保全閘門主控系統](#-boss-關機械保全閘門主控系統)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎨 額外主題卡（給想多做的人）](#-額外主題卡給想多做的人)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～6 週）
- ✅ 數位/類比輸出入、`map()` 數值轉換、`float` 運算
- ✅ 陣列、迴圈、自訂函式（含有回傳值的函式）
- ✅ 狀態變數、防彈跳、狀態轉變偵測、警報鎖定
- ✅ 蜂鳴器、光敏電阻、TMP36 溫度感測器

### 這週要新學的
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| 伺服馬達（Servo Motor） | 能精準定位到指定角度（通常 0°~180°）的馬達 | 機械手臂、閘門、方向控制等精密定位機構的核心 |
| `<Servo.h>` 函式庫 | Arduino 內建的伺服馬達控制函式庫 | 學會如何使用別人寫好的函式庫，是重要的工程能力 |
| `.attach()` | 把伺服馬達物件與實體腳位綁定 | 使用函式庫元件的標準流程 |
| `.write()` | 命令伺服馬達轉到指定角度 | 精準角度控制的核心指令 |
| 物件（Object） | 像 `Servo myServo` 這種「打包資料與功能」的程式概念 | 這學期第一次接觸物件導向程式設計的雛形 |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室老闆這次要打造終極機關——一道會真正「轉動開啟」的機械保全閘門。玩家必須完成一連串謎題，觸發閘門機構讓真實的門真的動起來，而不再只是燈光暗示「這裡有門」。你被指派設計整套閘門控制系統。
>
> 這週結束時，你會做出：
> 1. 一個能讓馬達轉到指定角度的基礎測試機關（暖身）
> 2. 一段馬達平滑來回掃描的動畫效果
> 3. 一個按鈕觸發閘門開關的互動機關
> 4. 一套用旋鈕或溫度即時控制馬達角度的感測連動系統
> 5. 一套具備自動延遲關閉與流量管制的閘門邏輯
> 6. 一個整合以上所有技巧的「機械保全閘門主控系統」

---

## 電路體檢站（3 分鐘快速複習）

**體檢題 1**：這週的伺服馬達跟前幾週學過的 LED、蜂鳴器一樣，都是「輸出裝置」——Arduino 主動送出指令控制它。差別在於，伺服馬達內部其實包含了一個小型的回授控制電路，能自動判斷「目前轉到哪裡」並修正到你指定的角度，這比單純的 `digitalWrite` 或 `analogWrite` 複雜得多，所以需要額外引入專門的函式庫（`<Servo.h>`）協助處理底層細節。

**體檢題 2**：回想第 3 週學到「PWM 用時間比例模擬類比效果」的概念，伺服馬達內部其實也是靠類似的脈衝訊號原理來判斷該轉到哪個角度，只是這些複雜的訊號時序細節，已經被 `<Servo.h>` 函式庫幫你處理好了，你只需要呼叫簡單的 `.write(角度)` 就能控制，不需要自己手動產生精確的脈衝訊號。

**體檢題 3**：伺服馬達需要額外接電阻保護嗎？
> 答案：不需要。伺服馬達內部已經有自己的驅動電路，只需要正確接上電源（5V）、接地（GND）、訊號線三條線即可，這跟 LED 的情況不同。

---

## 模擬設計專案配置

### 元件清單
| 元件 | 數量 | 說明 |
|---|---|---|
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1 | 電路佈局 |
| Micro Servo（伺服馬達） | 1 | 閘門動力機構 |
| Pushbutton | 1 | 閘門開關觸發鈕 |
| 10kΩ 電阻 | 1 | 按鈕下拉電阻 |
| Potentiometer | 1 | 手動測試旋鈕 |
| TMP36 | 1 | 沿用第 6 週，做感測連動示範 |
| RGB LED（共陰極） | 1 | 閘門狀態指示燈 |
| 220Ω 電阻 | 3 | RGB 三色限流電阻 |

### 接線步驟

**Part A：伺服馬達**
1. 拖曳一顆 **Micro Servo** 到工作區，通常有三條線：**橘/黃色（訊號線）、紅色（電源 +5V）、棕/黑色（GND）**。
2. 訊號線接到 Arduino 的 **Pin 6**。
3. 電源線接 **5V**，接地線接 **GND**。

**Part B：按鈕（沿用第 2 週）**
1. 下拉電阻 + 訊號線接 **Pin 7**。

**Part C：旋鈕（沿用第 4 週）**
1. 中間腳接 **A0**，兩側接 5V 與 GND。

**Part D：TMP36（沿用第 6 週）**
1. 訊號腳接 **A2**。

**Part E：RGB LED（沿用第 3 週）**
1. R → ~9、G → ~10、B → ~11。

### 腳位對照表（本週共用）
| 元件 | 腳位 |
|---|---|
| 伺服馬達訊號線 | Pin 6 |
| 閘門觸發按鈕 | Pin 7 |
| 旋鈕 | A0 |
| TMP36 | A2 |
| RGB LED - R / G / B | ~9 / ~10 / ~11 |

> 💡 **配置檢查清單**
> - [ ] 伺服馬達三條線分別正確接到訊號(Pin 6)、電源(5V)、接地(GND)
> - [ ] 按鈕、旋鈕、TMP36、RGB LED 均已依先前週次的接法正確配置

---

## 後端程式設計：五道機關關卡

### 關卡 1：馬達初體驗 — 認識 Servo.h 函式庫

**機關故事**：在設計任何複雜的閘門邏輯前，你得先確認能成功控制伺服馬達轉到指定角度。

#### 範例 1-1：控制馬達轉到指定角度
```cpp
// ============================================
// 範例 1-1：伺服馬達基礎控制 - 轉到 0°、90°、180°
// ============================================

#include <Servo.h>   // 引入伺服馬達函式庫

Servo gateServo;      // 建立一個伺服馬達「物件」，命名為 gateServo

void setup() {
  gateServo.attach(6);   // 將 gateServo 物件與 Pin 6 綁定
}

void loop() {
  gateServo.write(0);     // 轉到 0 度（閘門關閉）
  delay(1000);
  gateServo.write(90);    // 轉到 90 度（閘門半開）
  delay(1000);
  gateServo.write(180);   // 轉到 180 度（閘門全開）
  delay(1000);
}
```
**新語法提醒**：`Servo gateServo;` 這行程式碼建立了一個「物件（Object）」，可以想像成一個「打包好的遙控器」，裡面已經內建了 `.attach()`、`.write()` 等好用的功能，只要透過 `物件名稱.功能名稱()` 的方式呼叫即可使用，這是這學期第一次接觸這種「物件導向」的寫法，往後 LCD、鍵盤等元件也會用類似的方式操作。

#### 範例 1-2：只轉一次，觀察起始角度
```cpp
// ============================================
// 範例 1-2：只在 setup() 設定一次角度，之後不再變動
// ============================================

#include <Servo.h>

Servo gateServo;

void setup() {
  gateServo.attach(6);
  gateServo.write(45);   // 只設定一次角度
}

void loop() {
  // loop() 保持空白，馬達會停留在 setup() 設定的角度不動
}
```
**思考問題**：為什麼這段程式碼馬達會停在 45 度不動，而不像範例 1-1 一樣持續切換角度？

#### 範例 1-3：用旋鈕手動測試各種角度
```cpp
// ============================================
// 範例 1-3：旋鈕即時控制馬達角度，方便測試各種角度的實際效果
// ============================================

#include <Servo.h>

Servo testServo;
int pinPot = A0;

void setup() {
  testServo.attach(6);
}

void loop() {
  int rawValue = analogRead(pinPot);
  int angle = map(rawValue, 0, 1023, 0, 180);
  testServo.write(angle);
  delay(15);   // 給馬達一點反應時間，避免指令下達太快
}
```

---

### 關卡 2：平滑掃描 — 連續角度變化

**機關故事**：閘門突然從 0 度跳到 180 度顯得很不自然，密室老闆希望閘門開關要有「平滑轉動」的高級質感。

#### 範例 2-1：緩慢來回掃描（Sweep）
```cpp
// ============================================
// 範例 2-1：伺服馬達平滑來回掃描
// ============================================

#include <Servo.h>

Servo myServo;

void setup() {
  myServo.attach(6);
}

void loop() {
  // 從 0 度掃到 180 度
  for (int angle = 0; angle <= 180; angle++) {
    myServo.write(angle);
    delay(15);   // 每個角度停留一小段時間，馬達移動才會平滑
  }
  // 從 180 度掃回 0 度
  for (int angle = 180; angle >= 0; angle--) {
    myServo.write(angle);
    delay(15);
  }
}
```

#### 範例 2-2：把掃描邏輯封裝成函式（可指定範圍）
```cpp
// ============================================
// 範例 2-2：把掃描動作封裝成帶參數的函式，可指定角度範圍
// ============================================

#include <Servo.h>

Servo myServo;

void sweepTo(int fromAngle, int toAngle, int stepDelay) {
  if (fromAngle < toAngle) {
    for (int angle = fromAngle; angle <= toAngle; angle++) {
      myServo.write(angle);
      delay(stepDelay);
    }
  } else {
    for (int angle = fromAngle; angle >= toAngle; angle--) {
      myServo.write(angle);
      delay(stepDelay);
    }
  }
}

void setup() {
  myServo.attach(6);
}

void loop() {
  sweepTo(0, 90, 15);    // 從 0 度平滑轉到 90 度
  delay(500);
  sweepTo(90, 180, 15);  // 從 90 度平滑轉到 180 度
  delay(500);
  sweepTo(180, 0, 10);   // 從 180 度較快速地轉回 0 度
  delay(500);
}
```
**教學重點**：`sweepTo()` 函式內部用 `if-else` 判斷「起始角度」跟「目標角度」哪個比較大，決定迴圈是遞增（`angle++`）還是遞減（`angle--`），這種「讓函式自動判斷方向」的設計，讓呼叫端完全不需要關心內部細節，只要說「我要從哪裡轉到哪裡」即可，是很典型的模組化設計思維。

#### 範例 2-3：速度可調的呼吸式擺動
```cpp
// ============================================
// 範例 2-3：結合旋鈕，即時控制掃描速度快慢
// ============================================

#include <Servo.h>

Servo myServo;
int pinPot = A0;

void setup() {
  myServo.attach(6);
}

void loop() {
  int rawValue = analogRead(pinPot);
  int stepDelay = map(rawValue, 0, 1023, 30, 3);   // 旋鈕轉越多，速度越快（延遲越短）

  for (int angle = 0; angle <= 180; angle++) {
    myServo.write(angle);
    delay(stepDelay);
  }
  for (int angle = 180; angle >= 0; angle--) {
    myServo.write(angle);
    delay(stepDelay);
  }
}
```

---

### 關卡 3：按鈕觸發閘門開關

**機關故事**：玩家找到隱藏機關後，按下按鈕就能觸發閘門開啟。

#### 範例 3-1：按鈕控制閘門升起與放下
```cpp
// ============================================
// 範例 3-1：按一下升起，再按一下放下（Toggle 邏輯）
// ============================================

#include <Servo.h>

Servo gateServo;
int pinButton = 7;
int lastButtonState = LOW;
bool gateOpen = false;

void setup() {
  gateServo.attach(6);
  pinMode(pinButton, INPUT);
  gateServo.write(0);   // 初始狀態：閘門放下（0度）
}

void loop() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);   // 防彈跳
    gateOpen = !gateOpen;

    if (gateOpen) {
      gateServo.write(90);   // 升起
    } else {
      gateServo.write(0);    // 放下
    }
  }
  lastButtonState = currentState;
}
```

#### 範例 3-2：按鈕觸發平滑開關（結合關卡 2 的 sweepTo）
```cpp
// ============================================
// 範例 3-2：按鈕觸發平滑的開關動作，而非瞬間跳到目標角度
// ============================================

#include <Servo.h>

Servo gateServo;
int pinButton = 7;
int lastButtonState = LOW;
bool gateOpen = false;

void sweepTo(int fromAngle, int toAngle, int stepDelay) {
  if (fromAngle < toAngle) {
    for (int angle = fromAngle; angle <= toAngle; angle++) {
      gateServo.write(angle);
      delay(stepDelay);
    }
  } else {
    for (int angle = fromAngle; angle >= toAngle; angle--) {
      gateServo.write(angle);
      delay(stepDelay);
    }
  }
}

void setup() {
  gateServo.attach(6);
  pinMode(pinButton, INPUT);
  gateServo.write(0);
}

void loop() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    gateOpen = !gateOpen;

    if (gateOpen) {
      sweepTo(0, 90, 10);
    } else {
      sweepTo(90, 0, 10);
    }
  }
  lastButtonState = currentState;
}
```

#### 範例 3-3：加上聲音與燈光反饋
```cpp
// ============================================
// 範例 3-3：閘門動作時同時搭配聲音提示與 RGB 狀態燈
// ============================================

#include <Servo.h>

Servo gateServo;
int pinButton = 7;
int pinBuzzer = 8;
int pinR = 9, pinG = 10, pinB = 11;
int lastButtonState = LOW;
bool gateOpen = false;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  gateServo.attach(6);
  pinMode(pinButton, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  gateServo.write(0);
  setColor(255, 0, 0);   // 初始：紅燈代表閘門關閉
}

void loop() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    gateOpen = !gateOpen;

    tone(pinBuzzer, 1000);
    delay(200);
    noTone(pinBuzzer);

    if (gateOpen) {
      gateServo.write(90);
      setColor(0, 255, 0);   // 綠燈代表閘門開啟
    } else {
      gateServo.write(0);
      setColor(255, 0, 0);
    }
  }
  lastButtonState = currentState;
}
```

---

### 關卡 4：感測連動 — 旋鈕與溫度控制馬達角度

**機關故事**：密室老闆希望閘門不只是單純的開/關兩態，還要能呈現「連續角度」的儀表指針效果，像模擬指針式儀表一樣呈現溫度資訊。

#### 範例 4-1：旋鈕即時無段控制角度
```cpp
// ============================================
// 範例 4-1：旋鈕轉多少，馬達角度就跟著轉多少（複習關卡 1 範例 1-3）
// ============================================

#include <Servo.h>

Servo myServo;
int pinPot = A0;

void setup() {
  myServo.attach(6);
}

void loop() {
  int rawValue = analogRead(pinPot);
  int angle = map(rawValue, 0, 1023, 0, 180);
  myServo.write(angle);
  delay(15);
}
```

#### 範例 4-2：溫度儀表板（伺服馬達當指針）
```cpp
// ============================================
// 範例 4-2：用伺服馬達模擬指針式溫度計
// ============================================

#include <Servo.h>

Servo gaugeServo;
int pinTemp = A2;

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  gaugeServo.attach(6);
}

void loop() {
  float celsius = readTemperature();

  // 假設儀表範圍設定為 0°C ~ 50°C，對應指針角度 0°~180°
  int angle = map(celsius, 0, 50, 0, 180);
  angle = constrain(angle, 0, 180);   // 確保角度不會超出範圍（避免感測誤差造成馬達異常動作）
  gaugeServo.write(angle);
}
```
**教學重點**：`map()` 的第一個參數這裡直接放入 `float celsius`，而不是整數——這是可以運作的，因為 `map()` 內部運算會自動處理型別轉換，但結果仍然是整數（因為 `map()` 回傳型別是 `long`），所以角度呈現仍然是整數角度的階梯狀變化，而非真正連續平滑，這也是為什麼工業上更精密的儀表通常會用其他更進階的控制方式（超出本課程範圍，但值得知道這個限制存在）。

#### 範例 4-3：溫度超標時馬達自動歸零警示
```cpp
// ============================================
// 範例 4-3：結合第 6 週的警報鎖定概念 - 溫度超標時指針自動歸零並警示
// ============================================

#include <Servo.h>

Servo gaugeServo;
int pinTemp = A2;
int pinBuzzer = 8;
float threshold = 35.0;
bool alarmLatched = false;

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  gaugeServo.attach(6);
}

void loop() {
  float celsius = readTemperature();

  if (celsius > threshold) {
    alarmLatched = true;
  }

  if (alarmLatched) {
    gaugeServo.write(0);      // 指針歸零，代表系統異常
    tone(pinBuzzer, 2000);
    delay(100);
    noTone(pinBuzzer);
    delay(100);
  } else {
    int angle = map(celsius, 0, 50, 0, 180);
    angle = constrain(angle, 0, 180);
    gaugeServo.write(angle);
  }
}
```

---

### 關卡 5：自動化閘門邏輯 — 延遲自動關閉與流量管制

**機關故事**：機械保全閘門需要更聰明的自動化邏輯——開啟後過一段時間自動關閉，並且限制同時通過的人數（模擬場內容納量管制）。

#### 範例 5-1：自動延遲關閉
```cpp
// ============================================
// 範例 5-1：按下升起，延遲 3 秒後自動降下
// ============================================

#include <Servo.h>

Servo gateServo;
int pinButton = 7;
int lastButtonState = LOW;

void setup() {
  gateServo.attach(6);
  pinMode(pinButton, INPUT);
  gateServo.write(0);
}

void loop() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    gateServo.write(90);   // 升起
    delay(3000);            // 維持升起狀態 3 秒
    gateServo.write(0);     // 自動放下
  }
  lastButtonState = currentState;
}
```

#### 範例 5-2：進出場計數管制（滿載拒絕開啟）
```cpp
// ============================================
// 範例 5-2：場內數量管制 - 滿載時閘門拒絕開啟
// ============================================

#include <Servo.h>

Servo gateServo;
int pinButton = 7;
int lastButtonState = LOW;
int occupantCount = 0;
int maxCapacity = 5;

void setup() {
  gateServo.attach(6);
  pinMode(pinButton, INPUT);
  gateServo.write(0);
  Serial.begin(9600);
}

void loop() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);

    if (occupantCount < maxCapacity) {
      occupantCount++;
      Serial.print("玩家進場，目前人數：");
      Serial.println(occupantCount);
      gateServo.write(90);
      delay(2000);
      gateServo.write(0);
    } else {
      Serial.println("已達容納上限，閘門拒絕開啟！");
    }
  }
  lastButtonState = currentState;
}
```

#### 範例 5-3：加上聲音提示區分「正常開門」與「拒絕開門」
```cpp
// ============================================
// 範例 5-3：兩種不同的聲音提示，讓玩家清楚知道結果
// ============================================

#include <Servo.h>

Servo gateServo;
int pinButton = 7;
int pinBuzzer = 8;
int lastButtonState = LOW;
int occupantCount = 0;
int maxCapacity = 5;

void setup() {
  gateServo.attach(6);
  pinMode(pinButton, INPUT);
  gateServo.write(0);
}

void loop() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);

    if (occupantCount < maxCapacity) {
      occupantCount++;
      tone(pinBuzzer, 1200, 150);   // 通過提示音：單一短音
      delay(200);
      gateServo.write(90);
      delay(2000);
      gateServo.write(0);
    } else {
      // 拒絕提示音：連續兩聲低頻嗶聲
      tone(pinBuzzer, 400, 150);
      delay(200);
      tone(pinBuzzer, 400, 150);
      delay(200);
    }
  }
  lastButtonState = currentState;
}
```

---

### 🏆 BOSS 關：機械保全閘門主控系統

**機關故事**：整合本週所有技巧，打造完整的機械保全閘門系統——平滑開關動作、容納量管制、溫度異常自動鎖定閘門（模擬安全連鎖：過熱時系統拒絕開門，確保安全）。

```cpp
// ============================================
// BOSS 關：機械保全閘門主控系統 - 整合本週所有技巧
// 功能：平滑開關 + 容納量管制 + 聲光反饋 + 溫度異常安全連鎖
// ============================================

#include <Servo.h>

Servo gateServo;
int pinButton = 7;
int pinBuzzer = 8;
int pinR = 9, pinG = 10, pinB = 11;
int pinTemp = A2;

int lastButtonState = LOW;
int occupantCount = 0;
int maxCapacity = 5;
float tempThreshold = 35.0;
bool tempAlarmLatched = false;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void sweepTo(int fromAngle, int toAngle, int stepDelay) {
  if (fromAngle < toAngle) {
    for (int angle = fromAngle; angle <= toAngle; angle++) {
      gateServo.write(angle);
      delay(stepDelay);
    }
  } else {
    for (int angle = fromAngle; angle >= toAngle; angle--) {
      gateServo.write(angle);
      delay(stepDelay);
    }
  }
}

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  gateServo.attach(6);
  pinMode(pinButton, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  gateServo.write(0);
  Serial.begin(9600);
  Serial.println("=== 機械保全閘門系統啟動 ===");
}

void loop() {
  // --- 安全連鎖：溫度異常時，閘門系統整體鎖定 ---
  float celsius = readTemperature();
  if (celsius > tempThreshold) {
    tempAlarmLatched = true;
  }

  if (tempAlarmLatched) {
    setColor(255, 0, 0);
    tone(pinBuzzer, 2000);
    delay(100);
    noTone(pinBuzzer);
    delay(100);
    return;   // 提前結束本輪迴圈，閘門功能完全停用，體現「安全優先」原則
  }

  // --- 正常閘門邏輯 ---
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);

    if (occupantCount < maxCapacity) {
      occupantCount++;
      Serial.print("玩家進場，目前人數：");
      Serial.println(occupantCount);

      tone(pinBuzzer, 1200, 150);
      delay(200);
      setColor(0, 255, 0);
      sweepTo(0, 90, 10);
      delay(2000);
      sweepTo(90, 0, 10);
      setColor(0, 0, 255);   // 待機狀態：藍燈
    } else {
      Serial.println("已達容納上限，閘門拒絕開啟！");
      tone(pinBuzzer, 400, 150);
      delay(200);
      tone(pinBuzzer, 400, 150);
    }
  }
  lastButtonState = currentState;

  if (occupantCount < maxCapacity) {
    setColor(0, 0, 255);   // 平時待機：藍燈
  } else {
    setColor(255, 165, 0); // 已滿載：橘燈警示
  }
}
```
**教學重點**：這段程式碼中的 `return;` 用法跟第 6 週學過的一樣——一旦偵測到溫度異常，立刻跳過整個閘門邏輯，確保「安全條件永遠享有最高優先權」，這是工業控制系統中「安全連鎖（Safety Interlock）」設計的縮影：任何自動化系統，都必須先確保安全條件滿足，才能執行正常功能。

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| 伺服馬達完全不動 | 忘記呼叫 `.attach()`，或訊號線接錯腳位 | 檢查 `setup()` 中是否有 `gateServo.attach(6);` |
| 伺服馬達持續抖動或發出異音 | 角度數值超出 0~180 有效範圍，或指令下達太頻繁 | 加上 `constrain(angle, 0, 180)` 確保角度合法 |
| 掃描動作看起來卡卡的不平滑 | `delay()` 時間太長，或角度遞增/遞減間隔太大 | 縮短 `delay()` 時間，或改成每次只變化 1 度 |
| 按鈕觸發閘門後完全沒有平滑過渡效果 | 直接用 `.write()` 瞬間跳到目標角度，而非用迴圈逐步過渡 | 改用類似 `sweepTo()` 的漸進寫法 |
| 容納量管制邏輯永遠顯示「已滿載」 | `occupantCount` 只會累加、沒有離場的遞減邏輯 | 這是刻意簡化的設計，如需要離場機制需額外新增按鈕與遞減邏輯（可作為練習題挑戰） |
| BOSS 關程式碼中溫度正常時閘門卻仍無法動作 | `return;` 誤放在條件判斷之外，導致每次迴圈都提前結束 | 確認 `return;` 只在 `if (tempAlarmLatched)` 的判斷式內部 |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改範例 1-1，把三個角度改成 0°、60°、120°、180° 四段。
2. 修改範例 2-1 的掃描速度，把 `delay(15)` 改成 `delay(5)`，觀察速度差異。
3. 修改範例 3-1，把 Toggle 的目標角度從 0°/90° 改成 0°/180°。
4. 修改範例 5-1 的自動延遲關閉時間，從 3 秒改成 5 秒。
5. 修改範例 4-2 的溫度儀表範圍，從 0~50°C 改成 10~40°C。

### 🟡 中等題
6. 設計一個「三段角度循環」機關：每按一次按鈕，依序切換 0°→90°→180°→回到 0°，而不是只有兩段。
7. 修改範例 5-2 的容納量管制，新增一顆「離場按鈕」，讓 `occupantCount` 可以遞減（但不能減到負數）。
8. 結合第 4 週所學，設計一個「旋鈕預先設定角度、按鈕確認執行」的兩段式操作機關（旋鈕決定目標角度但不立即轉動，按下按鈕才真正觸發 `sweepTo()`）。
9. 修改 BOSS 關程式碼，新增「溫度警戒」（尚未達到鎖定門檻，但已偏高）狀態，此時閘門仍可正常使用，但 RGB 燈號改為黃色提前警示。
10. 設計一個「馬達回中測試」機關：按下按鈕後，馬達先快速回到 90 度（中間位置），暫停 1 秒，再依照正常邏輯執行開關動作。

### 🔴 挑戰題
11. **雙馬達協同控制（延伸電路）**：想像有第二顆伺服馬達（需自行擴充接線到 Pin 5），設計一個「雙開式閘門」——按下按鈕，兩顆馬達同時分別轉向相反方向，模擬電影中常見的雙開自動門效果。
12. **角度平滑度優化**：範例 4-2 提到 `map()` 的整數限制會讓指針呈現階梯狀變化。試著設計一個方案，讓指針在相鄰角度之間也能有更細緻的過渡感（提示：可以考慮在兩個整數角度之間，用更短的 `delay()` 配合更小的變化量模擬）。
13. **多重安全連鎖**：修改 BOSS 關程式碼，除了溫度異常會鎖定閘門，新增「容納量超過上限的 1.5 倍」（這是刻意設計的極端情境，模擬感測器異常導致計數超標）也會觸發另一種鎖定狀態，並用不同的燈光顏色區分兩種鎖定原因。
14. **閘門使用紀錄統計**：設計一個系統，統計「今天總共開關了幾次閘門」以及「平均每次開門後，過了多久按下下一次」（可以用簡易的計數方式模擬，不需要真正精準的時間量測）。
15. **完全自訂機械機關**：不參考任何範例，設計一個「你認為最酷的機械保全機關」，至少要用到：1 個帶參數的角度控制函式、1 個安全連鎖邏輯（某種異常狀況下機關會被鎖定）、1 個聲光反饋機制。

---

## 🎨 額外主題卡（給想多做的人）

> **主題卡 A：停車場道閘模擬**
> 模擬真實停車場的道閘系統：車輛靠近感應（可用第 4 週的旋鈕模擬車輛接近程度）時閘門自動升起，延遲一段時間後自動放下，並統計今天進出的車輛總數。

> **主題卡 B：機械手臂夾爪模擬**
> 用一顆伺服馬達模擬機械手臂的夾爪開合，設計「夾取」與「放開」兩種角度，並搭配按鈕觸發，模擬簡易產線夾取動作。

> **主題卡 C：太陽能板追光模擬**
> 結合第 5 週學過的光敏電阻，設計一個「馬達角度隨光線強弱調整」的系統，模擬太陽能板自動追蹤光源角度的簡化版本。

> **主題卡 D：密室保險箱旋鈕鎖**
> 設計一個機關：伺服馬達模擬保險箱轉盤鎖，玩家必須先用旋鈕「預先設定」正確的角度，再按下確認鈕，如果角度落在正確範圍內（可設計一個容許誤差區間）才會觸發開鎖動作。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| 伺服馬達（Servo Motor） | 伺服馬達 | 能精準定位到指定角度的馬達 |
| `<Servo.h>` | 伺服馬達函式庫 | Arduino 內建、專門用來控制伺服馬達的工具包 |
| 物件（Object） | 物件 | 打包了資料與功能的程式概念，透過「物件名稱.功能()」使用 |
| `.attach()` | 綁定腳位 | 把伺服馬達物件與實體腳位建立連結 |
| `.write()` | 寫入角度 | 命令伺服馬達轉到指定角度 |
| 安全連鎖（Safety Interlock） | 安全連鎖 | 確保安全條件優先滿足，否則系統功能會被鎖定的設計原則 |
| 容納量管制 | 容納量管制 | 限制系統內部單位數量不超過安全上限的邏輯設計 |

---

## 反思與延伸討論

1. 這週第一次接觸「物件（Object）」的概念（`Servo gateServo;`）。請討論：相較於前幾週單純用變數儲存腳位編號的寫法，「物件」這種把資料與功能打包在一起的方式，帶來了什麼樣的優勢？
2. BOSS 關程式碼展示了「安全連鎖」的設計——溫度異常時，閘門功能整個被鎖定，不管按鈕再怎麼按都沒有反應。請討論：在真實世界的機械設備中，你還能想到哪些「必須先確保安全條件、才能執行正常功能」的例子？
3. 練習題 12 提到 `map()` 的整數限制造成指針呈現「階梯狀」而非真正平滑的過渡。請討論：在什麼樣的應用場景下，這種「階梯狀」的細微誤差完全不影響使用（例如這週的密室機關），而在什麼場景下，這種誤差可能造成嚴重問題（提示：想想精密醫療儀器或工業機械手臂）？

---

## 本週檢核清單

- [ ] 能正確使用 `<Servo.h>` 函式庫控制伺服馬達
- [ ] 理解物件（Object）的基本概念與使用方式
- [ ] 能實作平滑的角度掃描效果，並封裝成帶參數的函式
- [ ] 能結合按鈕、旋鈕、溫度感測器控制馬達角度
- [ ] 能設計具備自動延遲與容納量管制的閘門邏輯
- [ ] 理解安全連鎖的設計原則，並能在程式中正確實作
- [ ] 完成至少 8 題基礎/中等練習題

---

## 下週預告

第 8 週是這學期第一個「期中專題週」——沒有新元件、沒有新語法，而是要把前 7 週學過的**所有**技巧（LED、按鈕、RGB、旋鈕、蜂鳴器、光敏電阻、溫度感測、伺服馬達）全部整合到同一個完整專案中，打造一套「智慧密室綜合監控站」。這將是檢驗你這一個半月來學習成果的重要里程碑。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 6 週](./week-06.md) | [下一週：第 8 週 — 期中專題 ➡](./week-08.md)
