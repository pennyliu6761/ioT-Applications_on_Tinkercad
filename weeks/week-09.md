[⬅ 回目錄](../README.md) | [⬅ 上一週：第 8 週](./week-08.md) | [下一週：第 10 週 — 智慧輸送帶 ➡](./week-10.md)

---

# 第 9 週：停車場雷達
## 主題專案：讓機關擁有「空間距離」的感知能力

> 🎯 **本週定位**：期中專題後，密室老闆決定拓展營運，開了一間「停車場雷達逃脫房」——玩家要在停車場情境中解謎，機關必須能偵測物體的距離，模擬真實的停車輔助雷達系統。這一週你會學到超音波測距模組（HC-SR04），這是這學期第一次接觸「透過時間差計算距離」的感測技術，也是下週智慧輸送帶自動避障邏輯最重要的技術鋪墊。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [電路體檢站（3 分鐘快速複習）](#電路體檢站3-分鐘快速複習)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道機關關卡](#後端程式設計五道機關關卡)
  - [關卡 1：超音波測距原理 — pulseIn() 與時間換算](#關卡-1超音波測距原理--pulsein-與時間換算)
  - [關卡 2：距離門檻判斷 — 安全區警示](#關卡-2距離門檻判斷--安全區警示)
  - [關卡 3：倒車雷達邏輯 — 分級警報頻率](#關卡-3倒車雷達邏輯--分級警報頻率)
  - [關卡 4：距離連動視覺化 — RGB 與伺服馬達整合](#關卡-4距離連動視覺化--rgb-與伺服馬達整合)
  - [關卡 5：物件通過計數 — 光遮斷概念的距離版](#關卡-5物件通過計數--光遮斷概念的距離版)
  - [🏆 BOSS 關：停車場智慧雷達主控系統](#-boss-關停車場智慧雷達主控系統)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎨 額外主題卡（給想多做的人）](#-額外主題卡給想多做的人)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～8 週）
- ✅ 數位/類比輸出入、`map()`、`constrain()`、`float` 運算
- ✅ 陣列、迴圈、自訂函式（含有回傳值的函式）
- ✅ 狀態變數、防彈跳、狀態轉變偵測、警報鎖定
- ✅ 蜂鳴器、光敏電阻、TMP36、伺服馬達
- ✅ 模組化系統架構設計、優先權排序

### 這週要新學的
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| HC-SR04 超音波感測器 | 用聲波發射與接收的時間差計算距離 | AGV 自走車避障、停車雷達、液位偵測的核心元件 |
| `pulseIn()` | 測量某個腳位維持高電位的時間長度（微秒） | 讀取超音波、紅外線等以時間編碼的訊號的標準函式 |
| `delayMicroseconds()` | 比 `delay()` 更精細的微秒級延遲 | 產生精準的短脈衝訊號，超音波測距的必要技巧 |
| 距離換算公式 | 把時間差換算成實際距離（公分） | 理解感測器規格書上時間與物理量的轉換關係 |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室老闆開了新分店，主題是「停車場尋車大冒險」——玩家要在昏暗的停車場中，透過雷達感應找到正確的車位並停車入庫，過程中會有各種距離相關的機關考驗。你被指派設計整套超音波感測系統。
>
> 這週結束時，你會做出：
> 1. 一個能準確測量距離的超音波感測器（暖身）
> 2. 一個簡單的安全距離警示機關
> 3. 一套完整的倒車雷達分級警報系統
> 4. 一套距離連動的視覺化儀表（RGB 顏色 + 伺服馬達指針）
> 5. 一個物件通過計數器（停車場車輛進出統計）
> 6. 一個整合以上所有技巧的「停車場智慧雷達主控系統」

---

## 電路體檢站（3 分鐘快速複習）

**體檢題 1**：超音波感測器需要兩隻訊號腳位——Trig（發射觸發）跟 Echo（接收回波），這跟前幾週學過的感測器（旋鈕只需一隻訊號腳、TMP36 也只需一隻）不太一樣。回想第 2 週學過的「輸入」與「輸出」概念——Trig 是 Arduino 要「發送」訊號給感測器（所以是 OUTPUT），Echo 則是 Arduino 要「接收」感測器傳回的訊號（所以是 INPUT）。一顆感測器同時包含輸出與輸入兩種角色，這是這學期第一次遇到這種情況。

**體檢題 2**：回想第 6 週學過的 `float` 浮點數運算——這週的距離計算同樣需要處理帶小數點的數值（因為距離常常不是整數公分），公式設計上會用到類似的除法運算技巧。

**體檢題 3**：超音波感測器需要接電阻保護嗎？
> 答案：不需要。跟伺服馬達一樣，HC-SR04 內部已有完整的驅動電路，只需要正確接上 VCC（5V）、GND、Trig、Echo 四條線即可。

---

## 模擬設計專案配置

### 元件清單
| 元件 | 數量 | 說明 |
|---|---|---|
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1 | 電路佈局 |
| Ultrasonic Distance Sensor（HC-SR04） | 1 | 測距核心元件 |
| RGB LED（共陰極） | 1 | 距離狀態指示燈（沿用第 3 週接法） |
| 220Ω 電阻 | 3 | RGB 三色限流電阻 |
| Buzzer | 1 | 警報發聲器（沿用第 5 週接法） |
| Micro Servo | 1 | 距離指針（沿用第 7 週接法） |

### 接線步驟

**Part A：HC-SR04 超音波感測器**
1. 拖曳一顆 **Ultrasonic Distance Sensor（HC-SR04）** 到工作區，模組有 4 隻腳：**VCC、Trig、Echo、GND**。
2. VCC 接 **5V**，GND 接 **GND**。
3. Trig 接 Arduino **Pin 9**（負責發射超音波脈衝）。
4. Echo 接 Arduino **Pin 10**（負責接收反射回波，計算時間差）。

**Part B：RGB LED（沿用第 3 週）**
1. R → ~9？—— **注意**：本週 Pin 9 已被 Trig 佔用，請改接 RGB LED 至 **~5、~6、~11**。

**Part C：蜂鳴器（沿用第 5 週）**
1. 正極接 **Pin 8**，負極接 GND。

**Part D：伺服馬達（沿用第 7 週）**
1. 訊號線接 **Pin 3**（因 Pin 6 需求調整，本週統一改用 Pin 3）。

### 腳位對照表（本週共用，注意與先前週次不同）
| 元件 | 腳位 |
|---|---|
| HC-SR04 Trig | Pin 9 |
| HC-SR04 Echo | Pin 10 |
| RGB LED - R / G / B | ~5 / ~6 / ~11 |
| 蜂鳴器 | Pin 8 |
| 伺服馬達訊號線 | Pin 3 |

> ⚠️ **腳位調整提醒**：因為 HC-SR04 需要用到 Pin 9 和 Pin 10 這兩個常用的 PWM 腳位，本週 RGB LED 與伺服馬達的腳位配置與之前週次略有不同，請務必依照本週的對照表重新接線，養成「每次接新元件都要重新檢視腳位分配」的良好工程習慣——這在真實專案開發中是家常便飯。

> 💡 **配置檢查清單**
> - [ ] HC-SR04 四隻腳正確接線，Trig 與 Echo 沒有接反
> - [ ] RGB LED 已改接至 ~5、~6、~11
> - [ ] 伺服馬達訊號線已改接至 Pin 3

---

## 後端程式設計：五道機關關卡

### 關卡 1：超音波測距原理 — pulseIn() 與時間換算

**機關故事**：在做出任何警示邏輯前，你得先確認能準確測量物體距離。

#### 範例 1-1：發射脈衝與讀取回波時間
```cpp
// ============================================
// 範例 1-1：超音波感測器基礎讀取
// 原理：Trig 發出短脈衝聲波，聲波遇到物體反彈回來被 Echo 接收
//      聲波來回所花的「時間」，就能反推出「距離」
// ============================================

int pinTrig = 9;
int pinEcho = 10;

void setup() {
  pinMode(pinTrig, OUTPUT);   // Trig 負責「發送」，是輸出腳位
  pinMode(pinEcho, INPUT);    // Echo 負責「接收」，是輸入腳位
  Serial.begin(9600);
}

void loop() {
  // 發射一個短脈衝訊號
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);   // 標準規格：Trig 需維持 10 微秒的高電位
  digitalWrite(pinTrig, LOW);

  // pulseIn() 會測量 Echo 腳位維持 HIGH 狀態的時間（微秒）
  long duration = pulseIn(pinEcho, HIGH);

  Serial.println(duration);
  delay(200);
}
```
**新語法提醒**：`delayMicroseconds()` 跟 `delay()` 功能類似，但單位是「微秒」（百萬分之一秒），用於需要極精準短暫延遲的場合。`pulseIn(pin, HIGH)` 則會一直等待，直到指定腳位變成 HIGH，然後開始計時，直到變回 LOW 為止，回傳這段「維持 HIGH」的時間長度。

#### 範例 1-2：微秒轉換為公分
```cpp
// ============================================
// 範例 1-2：把時間換算成距離
// 公式：距離(cm) = 時間(微秒) / 58
//（聲速約 340 m/s，來回需除以 2，經單位換算後簡化為除以 58）
// ============================================

int pinTrig = 9;
int pinEcho = 10;

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  Serial.begin(9600);
}

void loop() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);

  long duration = pulseIn(pinEcho, HIGH);
  float distanceCm = duration / 58.0;

  Serial.print("距離: ");
  Serial.print(distanceCm);
  Serial.println(" cm");
  delay(200);
}
```

#### 範例 1-3：把測距邏輯封裝成函式
```cpp
// ============================================
// 範例 1-3：封裝成可重複呼叫的 readDistance() 函式
// 複習第 6 週學過的「有回傳值的函式」寫法
// ============================================

int pinTrig = 9;
int pinEcho = 10;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);

  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  Serial.begin(9600);
}

void loop() {
  float distance = readDistance();
  Serial.println(distance);
  delay(200);
}
```
**教學重點**：往後每個關卡都會直接呼叫 `readDistance()`，不用重複寫這六行程式碼。這正是這學期一路強調的「模組化」精神——把常用邏輯封裝一次，到處呼叫使用。

---

### 關卡 2：距離門檻判斷 — 安全區警示

**機關故事**：停車場的柱子附近設有安全警示，物體太靠近就要提醒。

#### 範例 2-1：安全距離門檻警告
```cpp
// ============================================
// 範例 2-1：物件小於 10cm 亮起警告燈
// ============================================

int pinTrig = 9;
int pinEcho = 10;
int pinR = 5, pinG = 6, pinB = 11;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  float distance = readDistance();

  if (distance < 10) {
    setColor(255, 0, 0);
  } else {
    setColor(0, 255, 0);
  }
}
```

#### 範例 2-2：距離連動 RGB 燈色（三段式）
```cpp
// ============================================
// 範例 2-2：遠(綠)、中(黃)、近(紅) 三段距離顏色指示
// ============================================

int pinTrig = 9;
int pinEcho = 10;
int pinR = 5, pinG = 6, pinB = 11;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  float distance = readDistance();

  if (distance < 15) {
    setColor(255, 0, 0);        // 近：紅
  } else if (distance < 40) {
    setColor(255, 255, 0);      // 中：黃
  } else {
    setColor(0, 255, 0);        // 遠：綠
  }
}
```

---

### 關卡 3：倒車雷達邏輯 — 分級警報頻率

**機關故事**：真正的停車雷達不只是燈光警示，還要有越來越急促的嗶聲，這一關我們實作經典的倒車雷達聲音效果。

#### 範例 3-1：倒車雷達進階版 — 三個等級的警報音頻率
```cpp
// ============================================
// 範例 3-1：依距離遠近，設定三個等級的警報音頻率
// ============================================

int pinTrig = 9;
int pinEcho = 10;
int pinBuzzer = 8;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
}

void loop() {
  float distance = readDistance();

  if (distance < 10) {
    tone(pinBuzzer, 2000);      // 極近：高頻急促
    delay(100);
    noTone(pinBuzzer);
    delay(100);
  } else if (distance < 30) {
    tone(pinBuzzer, 1000);      // 中距離：中頻
    delay(200);
    noTone(pinBuzzer);
    delay(300);
  } else if (distance < 50) {
    tone(pinBuzzer, 500);       // 較遠：低頻緩慢
    delay(100);
    noTone(pinBuzzer);
    delay(700);
  } else {
    noTone(pinBuzzer);          // 安全距離：無聲
  }
}
```

#### 範例 3-2：連續映射版本（不用分級判斷，用 map 連續變化）
```cpp
// ============================================
// 範例 3-2：用 map() 讓警報頻率隨距離連續變化，而非跳躍式分級
// ============================================

int pinTrig = 9;
int pinEcho = 10;
int pinBuzzer = 8;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
}

void loop() {
  float distance = readDistance();
  distance = constrain(distance, 5, 50);   // 限制在合理範圍，避免極端值造成頻率異常

  int frequency = map(distance, 5, 50, 2000, 300);   // 越近頻率越高
  tone(pinBuzzer, frequency);
  delay(50);
}
```
**思考問題**：範例 3-1（分級判斷）跟範例 3-2（連續映射）哪一種聽起來更接近真實汽車倒車雷達的聲音效果？兩種寫法各自的優缺點是什麼？

---

### 關卡 4：距離連動視覺化 — RGB 與伺服馬達整合

**機關故事**：停車場管理室需要一個「距離儀表板」，用伺服馬達指針呈現距離資訊，就像真實的類比儀表。

#### 範例 4-1：距離儀表板（伺服馬達當指針）
```cpp
// ============================================
// 範例 4-1：用伺服馬達模擬指針式距離儀表
// ============================================

#include <Servo.h>

Servo gaugeServo;
int pinTrig = 9;
int pinEcho = 10;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  gaugeServo.attach(3);
}

void loop() {
  float distance = readDistance();

  // 假設儀表範圍設定為 0~50cm，對應指針角度 180°(近)~0°(遠)
  int angle = map(distance, 0, 50, 180, 0);
  angle = constrain(angle, 0, 180);
  gaugeServo.write(angle);
}
```

#### 範例 4-2：RGB + 伺服馬達雙重呈現
```cpp
// ============================================
// 範例 4-2：同時用顏色與指針角度呈現距離資訊
// ============================================

#include <Servo.h>

Servo gaugeServo;
int pinTrig = 9;
int pinEcho = 10;
int pinR = 5, pinG = 6, pinB = 11;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  gaugeServo.attach(3);
}

void loop() {
  float distance = readDistance();
  distance = constrain(distance, 0, 50);

  int angle = map(distance, 0, 50, 180, 0);
  gaugeServo.write(angle);

  int redAmount = map(distance, 0, 50, 255, 0);     // 越近越紅
  int greenAmount = map(distance, 0, 50, 0, 255);   // 越遠越綠
  setColor(redAmount, greenAmount, 0);
}
```

#### 範例 4-3：停車輔助自動剎車模擬
```cpp
// ============================================
// 範例 4-3：模擬自動停車輔助 - 太近時伺服馬達模擬「煞車拉桿」動作
// ============================================

#include <Servo.h>

Servo brakeServo;
int pinTrig = 9;
int pinEcho = 10;
int pinBuzzer = 8;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  brakeServo.attach(3);
  brakeServo.write(0);   // 初始：未煞車
}

void loop() {
  float distance = readDistance();

  if (distance < 8) {
    brakeServo.write(90);   // 模擬煞車拉桿拉起
    tone(pinBuzzer, 2500);
  } else {
    brakeServo.write(0);    // 煞車釋放
    noTone(pinBuzzer);
  }
}
```

---

### 關卡 5：物件通過計數 — 光遮斷概念的距離版

**機關故事**：停車場入口需要統計車流量，這一關我們把第 5 週學過的「狀態轉變偵測」手法，應用在超音波感測器上。

#### 範例 5-1：車輛通過計數器
```cpp
// ============================================
// 範例 5-1：物體靠近又離開時計數一次
// 手法複習：第 5 週的光遮斷狀態轉變偵測，這次改用距離感測
// ============================================

int pinTrig = 9;
int pinEcho = 10;
int carCount = 0;
bool wasNear = false;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  Serial.begin(9600);
}

void loop() {
  float distance = readDistance();
  bool isNear = distance < 20;

  // 偵測「由遠變近」的瞬間，代表有車輛經過感應區
  if (isNear && !wasNear) {
    carCount++;
    Serial.print("車輛通過，累計數量：");
    Serial.println(carCount);
  }

  wasNear = isNear;
  delay(100);
}
```

#### 範例 5-2：容納量管制（結合第 7 週閘門概念）
```cpp
// ============================================
// 範例 5-2：停車場車位管制 - 滿場時額外警示
// ============================================

int pinTrig = 9;
int pinEcho = 10;
int pinBuzzer = 8;
int carCount = 0;
int maxSpots = 10;
bool wasNear = false;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  Serial.begin(9600);
}

void loop() {
  float distance = readDistance();
  bool isNear = distance < 20;

  if (isNear && !wasNear) {
    if (carCount < maxSpots) {
      carCount++;
      Serial.print("車輛入場，剩餘車位：");
      Serial.println(maxSpots - carCount);
    } else {
      Serial.println("車位已滿！");
      tone(pinBuzzer, 800, 300);
    }
  }

  wasNear = isNear;
  delay(100);
}
```

---

### 🏆 BOSS 關：停車場智慧雷達主控系統

**機關故事**：整合本週所有技巧，打造完整的停車場智慧雷達系統——距離儀表顯示、分級聲音警報、車輛通過計數與容納量管制。

```cpp
// ============================================
// BOSS 關：停車場智慧雷達主控系統 - 整合本週所有技巧
// 功能：距離儀表 + RGB顏色 + 分級警報聲 + 車輛計數與容納量管制
// ============================================

#include <Servo.h>

int pinTrig = 9;
int pinEcho = 10;
int pinR = 5, pinG = 6, pinB = 11;
int pinBuzzer = 8;
Servo gaugeServo;

int carCount = 0;
int maxSpots = 10;
bool wasNear = false;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  gaugeServo.attach(3);
  Serial.begin(9600);
  Serial.println("=== 停車場智慧雷達系統啟動 ===");
}

void loop() {
  float distance = readDistance();
  float clampedDistance = constrain(distance, 0, 50);

  // --- 儀表指針與顏色呈現 ---
  int angle = map(clampedDistance, 0, 50, 180, 0);
  gaugeServo.write(angle);

  int redAmount = map(clampedDistance, 0, 50, 255, 0);
  int greenAmount = map(clampedDistance, 0, 50, 0, 255);
  setColor(redAmount, greenAmount, 0);

  // --- 分級警報聲 ---
  if (distance < 10) {
    tone(pinBuzzer, 2000);
    delay(80);
    noTone(pinBuzzer);
    delay(80);
  } else if (distance < 30) {
    tone(pinBuzzer, 1000);
    delay(150);
    noTone(pinBuzzer);
    delay(250);
  } else {
    noTone(pinBuzzer);
  }

  // --- 車輛通過計數與容納量管制 ---
  bool isNear = distance < 20;
  if (isNear && !wasNear) {
    if (carCount < maxSpots) {
      carCount++;
      Serial.print("車輛入場，剩餘車位：");
      Serial.println(maxSpots - carCount);
    } else {
      Serial.println("車位已滿，請改道其他停車場！");
    }
  }
  wasNear = isNear;
}
```
**教學重點**：這段程式碼再次驗證了模組化開發的價值——`readDistance()` 與 `setColor()` 兩個工具函式，被多個地方重複呼叫，而 `loop()` 本身則清楚呈現三個子功能（儀表顯示、警報聲、計數管制）依序執行的架構,這跟第 8 週學過的系統設計思維完全一致。

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| `pulseIn()` 讀值永遠是 0 | Trig 與 Echo 接反，或忘記設定正確的 `pinMode` | 確認 Trig 是 `OUTPUT`，Echo 是 `INPUT` |
| 距離數值忽大忽小、非常不穩定 | 超音波訊號雜訊，或物體表面角度造成反射不良 | 可嘗試多次讀取取平均值（可參考第 4 週練習題的平滑化手法） |
| 距離永遠顯示一個固定的極端值 | 感測器前方沒有物體反射（在 Tinkercad 中可嘗試調整虛擬環境或物體位置） | 檢查模擬環境中是否有物體可供偵測 |
| 伺服馬達角度呈現與預期相反 | `map()` 的目標範圍寫反（如近距離該轉大角度卻寫成小角度） | 檢查 `map(distance, 0, 50, 180, 0)` 的目標範圍順序是否符合設計預期 |
| 警報聲音效果卡頓 | `delay()` 時間設定不當，導致偵測不夠即時 | 適度縮短警報迴圈中的 `delay()` 時間 |
| 車輛計數器不斷瘋狂累加 | 忘記使用「狀態轉變偵測」（`isNear && !wasNear`） | 檢查是否正確比較目前與上一輪的狀態 |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改範例 2-1 的距離門檻，從 10cm 改成 15cm。
2. 修改範例 3-1 的三段警報頻率數值，自行設計一組你認為合理的頻率。
3. 修改範例 4-1 的儀表範圍，從 0~50cm 改成 0~30cm。
4. 修改範例 5-2 的 `maxSpots`，從 10 改成 5。
5. 在範例 1-2 的基礎上，新增功能：距離小於 5cm 時，額外印出 "極度接近警告" 的文字。

### 🟡 中等題
6. 設計一個「多重距離區間」系統：把距離分成 5 個等級（而非 3 個），並分別對應不同的燈光顏色。
7. 修改範例 5-1 的計數邏輯，新增「離場」判斷：如果物體從近距離變回遠距離，視為車輛離場，`carCount` 遞減。
8. 結合第 6 週所學的警報鎖定概念，設計一個「連續 3 次偵測到極近距離（<5cm）」才觸發永久鎖定警報的系統，需要人工按鈕才能解除。
9. 設計一個「平均距離平滑化」程式，連續讀取 5 次距離取平均值，再拿平均值做後續判斷，並用 Serial Plotter 比較平滑化前後的差異。
10. 修改 BOSS 關程式碼，新增功能：當車位剩餘數量少於 2 個時，儀表板額外閃爍黃燈提醒「車位即將額滿」。

### 🔴 挑戰題
11. **移動速度估算**：利用連續兩次距離讀值的差異，除以讀取間隔時間，估算物體的「接近速度」，並在速度過快時觸發特殊警報（模擬偵測到「快速接近的危險狀況」）。
12. **雙感測器交叉驗證**：想像停車場入口與出口各裝一顆超音波感測器（需自行擴充接線到 Pin 4/Pin 5 做第二組 Trig/Echo），分別統計入場與離場車輛數，讓 `carCount` 的計算更準確可靠。
13. **雷達掃描模擬（進階整合）**：結合第 7 週的伺服馬達平滑掃描技巧，設計一個「雷達旋轉掃描」效果——伺服馬達持續在 0°~180° 之間掃描，每次停頓時讀取當下角度的距離值並印出，模擬簡易雷達站的掃描回報功能。
14. **停車格導引系統**：設計一個功能，模擬「多個停車格各自的佔用狀態」——用陣列儲存 3 個停車格的佔用狀態（true/false），並能透過某種操作（如短距離觸發代表某格被佔用）更新對應格位的狀態，並印出目前空格總覽。
15. **完全自訂雷達系統**：不參考任何範例，設計一個「你認為最實用的距離感測應用」，至少要用到：`readDistance()` 封裝函式、至少一種視覺化呈現（RGB 或伺服馬達）、一個狀態轉變偵測邏輯。

---

## 🎨 額外主題卡（給想多做的人）

> **主題卡 A：自動門感應系統**
> 模擬商場自動門：有人靠近（距離小於某門檻）時，伺服馬達模擬門扇打開，離開後自動關閉，並統計今天的開門次數。

> **主題卡 B：機器人前方避障預告**
> 這是為第 10 週鋪路的預告題：如果超音波感測器接到會移動的小車上（想像一下，不需要真的實作馬達），你會怎麼設計「距離越近，車速自動放慢，太近就緊急煞停」的邏輯？先用文字描述你的設計構想。

> **主題卡 C：智慧垃圾桶感應開蓋**
> 手靠近垃圾桶時（距離小於某門檻），伺服馬達自動開蓋，離開後延遲數秒自動關閉，並用蜂鳴器發出提示音效。

> **主題卡 D：多層次距離音樂**
> 設計一個「距離控制音階」的互動樂器效果：把偵測範圍切分成 8 個區間，每個區間對應一個不同的音符（複習第 5 週的音符頻率對照表），手在感測器前後移動就能「彈奏」出簡單的音階。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| HC-SR04 | HC-SR04 | 一款透過發射與接收超音波計算距離的感測模組 |
| Trig（Trigger） | 觸發腳位 | 負責發射超音波脈衝的輸出腳位 |
| Echo | 回波腳位 | 負責接收反射回波的輸入腳位 |
| `pulseIn()` | 脈衝讀取 | 測量某腳位維持特定電位狀態的時間長度 |
| `delayMicroseconds()` | 微秒延遲 | 比 `delay()` 更精細的微秒級延遲函式 |
| 狀態轉變偵測 | 狀態轉變偵測 | 只在狀態剛改變的瞬間觸發動作（複習自第 5 週） |
| 平滑化（Smoothing） | 平滑化 | 透過多次讀值取平均降低感測雜訊的手法 |

---

## 反思與延伸討論

1. 超音波感測器同時需要一個輸出腳位（Trig）與一個輸入腳位（Echo），這跟之前學過「一個感測器對應一個腳位」的模式不同。請討論：你還能想到哪些現實生活中的裝置，也是需要「先發送、再接收」才能完成一次感測動作（提示：想想雷達、聲納、甚至蝙蝠的回聲定位）？
2. 練習題 11 提到可以用連續兩次距離讀值的差異估算「接近速度」。請討論：這種「用位置變化量除以時間，推算速度」的概念，在物理課本中叫做什麼？這是不是代表程式設計與其他學科的知識是可以互相connect應用的？
3. 這週學到的超音波測距，是下週開始接觸的「移動機器人避障」的核心技術基礎。請討論：如果你要設計一台能自動避開障礙物的小車，除了知道「前方有多遠」，你認為還需要知道哪些額外的資訊，才能做出更聰明的避障決策？

---

## 本週檢核清單

- [ ] 理解超音波測距的基本原理（發射、接收、時間差換算距離）
- [ ] 能正確使用 `pulseIn()` 與 `delayMicroseconds()`
- [ ] 能把測距邏輯封裝成有回傳值的函式
- [ ] 能實作多段距離門檻判斷與連續映射兩種警示手法
- [ ] 能結合伺服馬達與 RGB LED 呈現距離資訊
- [ ] 能實作物件通過計數與容納量管制邏輯
- [ ] 完成至少 8 題基礎/中等練習題

---

## 下週預告

第 10 週我們要學習「連續動力驅動」——直流馬達與 L293D 馬達驅動晶片。密室老闆想打造一間「智慧輸送帶」主題房，你將學會如何控制真正會持續轉動的馬達（而非伺服馬達的定點角度控制），這是這學期最後一次接觸新的動力元件，也是第 14 週學習繼電器「以小控大」概念前最重要的暖身。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 8 週](./week-08.md) | [下一週：第 10 週 — 智慧輸送帶 ➡](./week-10.md)
