[⬅ 回目錄](../README.md) | [⬅ 上一週：第 9 週](./week-09.md) | [下一週：第 11 週 — 產線戰情看板 ➡](./week-11.md)

---

# 第 10 週：智慧輸送帶
## 主題專案：從「定點角度」到「持續轉動」的動力世界

> 🎯 **本週定位**：第 7 週學的伺服馬達只能轉到「指定角度」就停住（0°~180°），這一週你要學習完全不同性質的動力元件——**直流馬達（DC Motor）**，它會「持續轉動」，可以正轉、反轉、變速，就像真正的傳送帶滾輪或車輪。密室老闆想打造一間「智慧輸送帶物流房」主題房，玩家要操控輸送帶運送機關道具。**這是整個 Arduino 單元最後一次接觸新的動力元件**，你會學到「以小訊號控制大功率裝置」的核心原理，這個概念在第 14 週學習繼電器時會再次出現，並且是真實工廠中無人搬運車（AGV）、自動輸送系統驅動輪子的基礎技術。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [電路體檢站（3 分鐘快速複習）](#電路體檢站3-分鐘快速複習)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道機關關卡](#後端程式設計五道機關關卡)
  - [關卡 1：馬達驅動基礎 — 認識 L293D 與 H 橋](#關卡-1馬達驅動基礎--認識-l293d-與-h-橋)
  - [關卡 2：正反轉控制 — 雙輸入腳位邏輯](#關卡-2正反轉控制--雙輸入腳位邏輯)
  - [關卡 3：無段變速 — PWM 控制轉速](#關卡-3無段變速--pwm-控制轉速)
  - [關卡 4：緩啟動與緩停止](#關卡-4緩啟動與緩停止)
  - [關卡 5：感測連動 — 超音波避障與馬達整合](#關卡-5感測連動--超音波避障與馬達整合)
  - [🏆 BOSS 關：智慧輸送帶主控系統](#-boss-關智慧輸送帶主控系統)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎨 額外主題卡（給想多做的人）](#-額外主題卡給想多做的人)
- [📖 延伸連結：這週學到的東西如何應用在真實自動化場域](#-延伸連結這週學到的東西如何應用在真實自動化場域)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～9 週）
- ✅ 數位/類比輸出入、`map()`、`constrain()`、`float` 運算
- ✅ 陣列、迴圈、自訂函式、狀態轉變偵測
- ✅ 蜂鳴器、光敏電阻、TMP36、伺服馬達
- ✅ 超音波測距（`pulseIn()`、`delayMicroseconds()`）

### 這週要新學的
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| 直流馬達（DC Motor） | 通電後會持續轉動的馬達，轉速與方向可控制 | 輸送帶、車輪、風扇等一切需要「持續轉動」的機構 |
| L293D 馬達驅動晶片 | 俗稱「H 橋」，讓小電流的 Arduino 能控制大電流的馬達 | Arduino 無法直接驅動馬達，這是業界標準解法之一 |
| Enable 腳位 | 控制馬達是否啟用，並可用 PWM 控制轉速 | 馬達調速的關鵛腳位 |
| Input 腳位（IN1、IN2） | 決定馬達電流方向，進而決定正轉或反轉 | H 橋電路的核心控制邏輯 |
| 緩啟動 / 緩停止 | 逐漸增加或減少 PWM 值，而非瞬間全速 | 減少機械衝擊、延長設備壽命的工業標準做法 |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室老闆的物流體驗館開幕在即——玩家要操控一條「智慧輸送帶」，把機關道具正確送到指定位置才能過關。你被指派設計整套馬達控制系統，這是這學期第一次真正操控「持續轉動」的動力元件。
>
> 這週結束時，你會做出：
> 1. 一個能讓馬達單向轉動與停止的基礎測試機關（暖身）
> 2. 一套正反轉切換的控制邏輯
> 3. 一套無段調速的輸送帶系統
> 4. 一套緩啟動緩停止的平順動力控制
> 5. 一套結合第 9 週超音波感測器的自動避障邏輯
> 6. 一個整合以上所有技巧的「智慧輸送帶主控系統」

---

## 電路體檢站（3 分鐘快速複習）

**體檢題 1**：為什麼 Arduino 不能直接接馬達，而需要透過 L293D 這顆晶片？
> 答案：Arduino 的腳位只能輸出極小的電流（通常不到 40 毫安），而馬達運轉需要遠大於這個數值的電流才能提供足夠的扭力。直接把馬達接在 Arduino 腳位上，輕則馬達轉不動，重則燒毀 Arduino。L293D 的作用是「用小電流的控制訊號，去驅動大電流的馬達迴路」，這種「以小控大」的概念，其實跟第 7 週學過的伺服馬達內部原理有異曲同工之妙——都是用一個小訊號去控制實際的動力輸出，只是伺服馬達的驅動電路已經內建好了，而直流馬達需要我們自己外接 L293D。

**體檢題 2**：回想第 3 週學過的 PWM 概念——這週要用 PWM 控制的不是 LED 亮度，而是馬達轉速，兩者的原理完全相同：「快速開關的時間比例」，只是這次控制的是機械動力輸出而非光的強弱。

**體檢題 3**：L293D 需要外接電阻保護嗎？
> 答案：不需要外接限流電阻（L293D 內部已有相應的驅動電路設計），但需要正確依照晶片腳位圖接上電源與訊號線，接線複雜度會比之前的元件都要高一些，請務必仔細對照本週的接線指引。

---

## 模擬設計專案配置

### 元件清單
| 元件 | 數量 | 說明 |
|---|---|---|
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1 | 電路佈局 |
| L293D | 1 | 馬達驅動晶片（16-pin DIP 封裝） |
| DC Motor | 1 | 直流馬達（輸送帶動力來源） |
| Ultrasonic Distance Sensor（HC-SR04） | 1 | 沿用第 9 週，做避障連動示範 |

### 接線步驟

**Part A：L293D 晶片接線**
L293D 是 16 隻腳的晶片，關鍵接法如下（Tinkercad 元件上有腳位編號標示，建議搭配元件說明面板逐一核對）：
1. **Pin 1（Enable 1）**：接 Arduino PWM 腳位 **~5**（控制轉速）
2. **Pin 2（Input 1）**：接 Arduino **Pin 3**（控制轉向 A）
3. **Pin 7（Input 2）**：接 Arduino **Pin 4**（控制轉向 B）
4. **Pin 3（Output 1）／Pin 6（Output 2）**：接馬達的兩隻接腳
5. **Pin 8（VCC2, 馬達電源）**：接 **5V**（Tinkercad 模擬環境可直接使用 Arduino 5V，真實硬體通常會使用獨立電源，避免馬達啟動時的電流突波干擾 Arduino 運作）
6. **Pin 16（VCC1, 邏輯電源）**：接 **5V**
7. **Pin 4, 5, 12, 13（GND）**：全部接 **GND**

**Part B：超音波感測器（沿用第 9 週）**
1. Trig 接 **Pin 9**，Echo 接 **Pin 10**。

> ⚠️ **教學提醒**：L293D 接線是本課程目前為止最複雜的一次，建議花額外時間用簡化圖解說明「H 橋」的基本概念——兩個輸入腳位（IN1、IN2）決定電流流動方向，進而決定馬達正轉或反轉：
> - IN1 = HIGH, IN2 = LOW → 正轉
> - IN1 = LOW, IN2 = HIGH → 反轉
> - IN1 = LOW, IN2 = LOW（或兩者相同）→ 停止

### 腳位對照表（本週共用）
| 元件 | 腳位 |
|---|---|
| L293D Enable（轉速控制） | ~5 |
| L293D Input 1（轉向控制 A） | Pin 3 |
| L293D Input 2（轉向控制 B） | Pin 4 |
| 超音波 Trig | Pin 9 |
| 超音波 Echo | Pin 10 |

> 💡 **配置檢查清單**
> - [ ] L293D 的 Enable、Input1、Input2 分別正確接到 ~5、Pin 3、Pin 4
> - [ ] L293D 的兩組電源腳（Pin 8、Pin 16）都接 5V，四個接地腳都接 GND
> - [ ] 馬達的兩隻接腳正確接到 L293D 的 Output 1 與 Output 2
> - [ ] 超音波感測器沿用第 9 週接法

---

## 後端程式設計：五道機關關卡

### 關卡 1：馬達驅動基礎 — 認識 L293D 與 H 橋

**機關故事**：在設計任何複雜的輸送帶邏輯前，你得先確認馬達真的能被驅動轉動。

#### 範例 1-1：控制馬達單向旋轉與停止
```cpp
// ============================================
// 範例 1-1：馬達基礎控制 - 單向轉動與停止
// ============================================

int pinIn1 = 3;    // 對應 L293D Input 1
int pinIn2 = 4;    // 對應 L293D Input 2
int pinEnable = 5; // 對應 L293D Enable（控制轉速，此階段先固定全速）

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
}

void loop() {
  // 正轉：Input1 = HIGH, Input2 = LOW
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
  analogWrite(pinEnable, 255);   // 全速

  delay(2000);

  // 停止：Enable 設為 0
  analogWrite(pinEnable, 0);
  delay(2000);
}
```

#### 範例 1-2：觀察 Enable 為 0 時的行為
```cpp
// ============================================
// 範例 1-2：驗證 Enable 對馬達的控制作用
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);

  // Input 方向固定設定好，只透過 Enable 決定馬達是否真正轉動
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
}

void loop() {
  analogWrite(pinEnable, 255);
  delay(1000);
  analogWrite(pinEnable, 0);
  delay(1000);
}
```
**教學重點**：這裡示範了一個重要觀念——即使 Input1/Input2 已經設定好轉動方向，只要 Enable 是 0，馬達依然不會轉動。這跟第 3 週學過的 PWM 概念相通：Enable 腳位就是負責控制「這個方向的電流，要用多大的比例通過」。

---

### 關卡 2：正反轉控制 — 雙輸入腳位邏輯

**機關故事**：輸送帶需要能夠正轉（往前送貨）跟反轉（退回重新整理），這一關我們實作完整的雙向控制。

#### 範例 2-1：正反轉切換
```cpp
// ============================================
// 範例 2-1：前進 2 秒、後退 2 秒
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
}

void loop() {
  // 正轉（前進）
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
  analogWrite(pinEnable, 255);
  delay(2000);

  // 反轉（後退）：交換 Input1、Input2 的高低電位
  digitalWrite(pinIn1, LOW);
  digitalWrite(pinIn2, HIGH);
  analogWrite(pinEnable, 255);
  delay(2000);
}
```

#### 範例 2-2：把方向控制封裝成函式
```cpp
// ============================================
// 範例 2-2：把常用的方向控制封裝成好用的函式
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;

void motorForward(int speed) {
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
  analogWrite(pinEnable, speed);
}

void motorBackward(int speed) {
  digitalWrite(pinIn1, LOW);
  digitalWrite(pinIn2, HIGH);
  analogWrite(pinEnable, speed);
}

void motorStop() {
  analogWrite(pinEnable, 0);
}

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
}

void loop() {
  motorForward(255);
  delay(2000);
  motorStop();
  delay(500);
  motorBackward(255);
  delay(2000);
  motorStop();
  delay(500);
}
```
**教學重點**：這是這學期第一次把「馬達控制」完整封裝成三個語意清楚的函式（`motorForward`、`motorBackward`、`motorStop`），之後的所有關卡都會直接呼叫這三個函式，不再需要記得 IN1/IN2 該設什麼值，這正是模組化設計「降低使用複雜度」的核心價值。

#### 範例 2-3：按鈕控制正轉、反轉與急煞
```cpp
// ============================================
// 範例 2-3：三顆按鈕分別控制馬達正轉、反轉、急煞
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;
int pinBtnForward = 7;
int pinBtnBackward = 6;
int pinBtnStop = 2;

void motorForward(int speed) {
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
  analogWrite(pinEnable, speed);
}

void motorBackward(int speed) {
  digitalWrite(pinIn1, LOW);
  digitalWrite(pinIn2, HIGH);
  analogWrite(pinEnable, speed);
}

void motorStop() {
  analogWrite(pinEnable, 0);
}

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
  pinMode(pinBtnForward, INPUT);
  pinMode(pinBtnBackward, INPUT);
  pinMode(pinBtnStop, INPUT);
}

void loop() {
  if (digitalRead(pinBtnForward) == HIGH) {
    motorForward(255);
  } else if (digitalRead(pinBtnBackward) == HIGH) {
    motorBackward(255);
  } else if (digitalRead(pinBtnStop) == HIGH) {
    motorStop();
  }
}
```

---

### 關卡 3：無段變速 — PWM 控制轉速

**機關故事**：不同的道具重量需要不同的輸送帶速度，太快容易讓道具掉落，太慢則影響遊戲節奏。

#### 範例 3-1：測試不同轉速
```cpp
// ============================================
// 範例 3-1：透過 Enable 腳位的 PWM 值控制轉速快慢
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
}

void loop() {
  analogWrite(pinEnable, 100);   // 慢速
  delay(2000);
  analogWrite(pinEnable, 255);   // 快速
  delay(2000);
}
```

#### 範例 3-2：旋鈕調速輸送帶
```cpp
// ============================================
// 範例 3-2：可變電阻即時控制輸送帶速度（需自行擴充接一顆旋鈕到 A0）
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;
int pinPot = A0;

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
}

void loop() {
  int rawValue = analogRead(pinPot);
  int speed = map(rawValue, 0, 1023, 0, 255);
  analogWrite(pinEnable, speed);
}
```

---

### 關卡 4：緩啟動與緩停止

**機關故事**：輸送帶瞬間全速啟動，會讓道具因慣性飛出去，密室老闆要求動力輸出要「柔順」，這正是工業上非常重要的緩啟動概念。

#### 範例 4-1：緩啟動與緩停止
```cpp
// ============================================
// 範例 4-1：逐漸加速、逐漸減速，減少機械衝擊
// 工業意義：馬達瞬間全速啟動會產生機械應力，緩啟動能延長設備壽命
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
}

void loop() {
  // 緩啟動：PWM 值由 0 逐漸增加到 255
  for (int speed = 0; speed <= 255; speed++) {
    analogWrite(pinEnable, speed);
    delay(10);
  }

  delay(1000);   // 全速運轉維持 1 秒

  // 緩停止：PWM 值由 255 逐漸減少到 0
  for (int speed = 255; speed >= 0; speed--) {
    analogWrite(pinEnable, speed);
    delay(10);
  }

  delay(1000);   // 停止狀態維持 1 秒
}
```

#### 範例 4-2：把緩啟動封裝成帶參數函式
```cpp
// ============================================
// 範例 4-2：封裝成可指定目標速度與變化快慢的函式
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;
int currentSpeed = 0;   // 記錄目前實際速度，供緩啟停邏輯參考

void rampTo(int targetSpeed, int stepDelay) {
  if (currentSpeed < targetSpeed) {
    for (int s = currentSpeed; s <= targetSpeed; s++) {
      analogWrite(pinEnable, s);
      delay(stepDelay);
    }
  } else {
    for (int s = currentSpeed; s >= targetSpeed; s--) {
      analogWrite(pinEnable, s);
      delay(stepDelay);
    }
  }
  currentSpeed = targetSpeed;   // 更新目前速度記錄
}

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
}

void loop() {
  rampTo(255, 8);   // 緩慢加速到全速
  delay(1000);
  rampTo(100, 5);   // 減速到中速（比全速更快地降速，速度變化間隔較短）
  delay(1000);
  rampTo(0, 10);    // 緩慢停止
  delay(1000);
}
```
**教學重點**：`rampTo()` 這個函式跟第 7 週學過的 `sweepTo()`（伺服馬達平滑掃描）在設計思路上完全一致——都是「用一個變數記住目前狀態，判斷該遞增還是遞減，逐步逼近目標值」。這種可以在不同元件（伺服馬達角度、馬達轉速）之間套用的通用思考模式，正是這學期不斷強調的核心程式設計素養。

---

### 關卡 5：感測連動 — 超音波避障與馬達整合

**機關故事**：智慧輸送帶需要具備感知能力——前方有障礙物（例如道具卡住）時，要自動停止或減速，這是這學期整合難度最高的一關，也是真實無人搬運車（AGV）避障邏輯的核心縮影。

#### 範例 5-1：整合超音波與馬達（平時全速前進）
```cpp
// ============================================
// 範例 5-1：輸送帶平時全速運轉
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;
int pinTrig = 9;
int pinEcho = 10;

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);

  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
  analogWrite(pinEnable, 255);
}

void loop() {
  // 本階段先讓馬達持續全速運轉，下一步驟才加入避障邏輯
}
```

#### 範例 5-2：遇障礙物自動減速、極近距離緊急停止
```cpp
// ============================================
// 範例 5-2：輸送帶避障邏輯 - 依距離分三段反應
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;
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
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
}

void loop() {
  float distance = readDistance();

  if (distance < 5) {
    analogWrite(pinEnable, 0);     // 極近：緊急停止
  } else if (distance < 20) {
    analogWrite(pinEnable, 100);   // 接近：減速
  } else {
    analogWrite(pinEnable, 255);   // 安全：全速
  }
}
```

#### 範例 5-3：結合蜂鳴器發出工程車警示音
```cpp
// ============================================
// 範例 5-3：完整避障系統 - 加上聲音警示
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;
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
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
}

void loop() {
  float distance = readDistance();

  if (distance < 5) {
    analogWrite(pinEnable, 0);
    tone(pinBuzzer, 1500);
  } else if (distance < 20) {
    analogWrite(pinEnable, 100);
    tone(pinBuzzer, 800);
    delay(100);
    noTone(pinBuzzer);
    delay(200);
  } else {
    analogWrite(pinEnable, 255);
    noTone(pinBuzzer);
  }
}
```

---

### 🏆 BOSS 關：智慧輸送帶主控系統

**機關故事**：整合本週所有技巧，打造完整的智慧輸送帶——具備緩啟動、避障減速、聲音警示，模擬真實物流輸送系統的運作邏輯。

```cpp
// ============================================
// BOSS 關：智慧輸送帶主控系統 - 整合本週所有技巧
// 功能：緩啟動 + 距離感應變速 + 聲音警示
// ============================================

int pinIn1 = 3;
int pinIn2 = 4;
int pinEnable = 5;
int pinTrig = 9;
int pinEcho = 10;
int pinBuzzer = 8;

int currentSpeed = 0;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void rampTo(int targetSpeed, int stepDelay) {
  if (currentSpeed < targetSpeed) {
    for (int s = currentSpeed; s <= targetSpeed; s++) {
      analogWrite(pinEnable, s);
      delay(stepDelay);
    }
  } else if (currentSpeed > targetSpeed) {
    for (int s = currentSpeed; s >= targetSpeed; s--) {
      analogWrite(pinEnable, s);
      delay(stepDelay);
    }
  }
  currentSpeed = targetSpeed;
}

void setup() {
  pinMode(pinIn1, OUTPUT);
  pinMode(pinIn2, OUTPUT);
  pinMode(pinEnable, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  digitalWrite(pinIn1, HIGH);
  digitalWrite(pinIn2, LOW);
  Serial.begin(9600);
  Serial.println("=== 智慧輸送帶系統啟動，緩啟動中 ===");
  rampTo(255, 10);   // 開機時緩慢加速到全速
}

void loop() {
  float distance = readDistance();

  if (distance < 5) {
    rampTo(0, 3);      // 緊急停止（用較快的遞減速率模擬急停）
    tone(pinBuzzer, 1800);
    delay(100);
    noTone(pinBuzzer);
    delay(100);
  } else if (distance < 20) {
    rampTo(100, 3);    // 平順減速，而非瞬間跳到慢速
    tone(pinBuzzer, 800);
    delay(150);
    noTone(pinBuzzer);
    delay(250);
  } else {
    rampTo(255, 3);    // 平順恢復全速
    noTone(pinBuzzer);
  }
}
```
**教學重點**：注意這裡即使是「避障減速」，仍然透過 `rampTo()` 讓速度變化保持平順，而非直接用 `analogWrite()` 瞬間跳到目標速度——這正是工業設備設計中「即使是緊急反應，也要盡量避免對機構造成瞬間衝擊」的細膩考量，是這學期至今對「動力控制品質」要求最高的一次示範。

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| 馬達完全不轉 | Enable 忘記設定，或 Input1/Input2 都設成相同電位 | 檢查 Enable 是否有給非零的 PWM 值，Input1/Input2 是否一高一低 |
| 馬達只能轉一個方向，切換方向沒反應 | Input1、Input2 沒有正確交換高低電位 | 檢查正反轉切換的程式碼是否確實交換了兩個腳位的邏輯值 |
| 馬達轉速無法用旋鈕/PWM 調整，只有全速或停止兩種狀態 | Enable 腳位接錯，接到非 PWM 腳位 | 確認 Enable 接在支援 `~` 標示的 PWM 腳位 |
| 緩啟動效果看起來還是很突然 | `rampTo()` 的 `stepDelay` 設太短，或起始與目標速度差距判斷有誤 | 適度增加 `stepDelay`，並確認 `currentSpeed` 有正確被更新追蹤 |
| 避障邏輯中馬達速度忽快忽慢跳動 | 超音波讀值本身雜訊造成的正常現象 | 可考慮加入平滑化處理（複習第 9 週練習題），降低雜訊影響 |
| L293D 接線後完全沒反應，懷疑晶片故障 | 兩組電源腳（Pin 8、16）或接地腳沒有全部接妥 | 逐一核對 L293D 十六隻腳位接線，這是最容易漏接的元件之一 |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改範例 1-1，把全速運轉時間從 2 秒改成 3 秒。
2. 修改範例 3-1，新增一個「中速」（PWM 值 180）的測試階段。
3. 修改範例 4-1 的緩啟動速度，把 `delay(10)` 改成 `delay(5)`，感受加速更快的效果。
4. 修改範例 5-2 的三段距離門檻，自行設計一組你認為合理的數值。
5. 在範例 2-2 的基礎上，新增一個 `motorForward()` 呼叫，測試不同速度值（如 150）的實際效果。

### 🟡 中等題
6. 設計一個「定時輸送帶」：馬達運轉 5 秒後自動停止 2 秒，如此反覆循環，並在 Serial Monitor 印出目前是「運轉中」還是「暫停中」。
7. 結合第 2 週所學的按鈕 Toggle 邏輯，設計一個「按一下啟動緩啟動、再按一下觸發緩停止」的控制系統。
8. 修改範例 4-2 的 `rampTo()` 函式，新增功能：如果 `currentSpeed` 已經等於 `targetSpeed`，直接跳過迴圈不執行任何動作（提升程式效率）。
9. 設計一個「三段速選擇器」：用旋鈕分成三個區間，分別對應慢速、中速、快速三種固定轉速（而非無段連續調速）。
10. 修改 BOSS 關程式碼，新增功能：緊急停止觸發時，額外統計「今天總共緊急停止了幾次」並印出。

### 🔴 挑戰題
11. **雙馬達差速驅動挑戰**：想像現在有兩顆直流馬達（模擬機器人或自走車的左右輪，需自行擴充第二組 L293D 接線），設計一個「左右輪轉速不同，讓輸送帶模型產生轉向效果」的邏輯（提示：這正是真實無人搬運車、掃地機器人轉向的核心原理，可以先在這裡預先體驗）。
12. **感測器雜訊防呆**：修改 BOSS 關程式碼，避障判斷改成「連續 3 次都偵測到近距離，才真正觸發減速」，避免單次雜訊誤判造成不必要的速度波動。
13. **馬達運轉時間統計**：設計一個系統，累計記錄馬達「今天總共運轉了多少秒」，並能區分全速運轉與減速運轉的時間比例。
14. **雙向輸送帶交替**：設計一個系統，輸送帶正轉 5 秒後，自動反轉 5 秒，如此交替運行，並在每次換向前先執行緩停止再緩啟動，確保動作平順。
15. **完全自訂動力系統**：不參考任何範例，設計一個「你認為最實用的馬達控制應用」，至少要用到：`rampTo()` 或類似的平順控制函式、至少一種感測連動邏輯、聲音或燈光反饋。

---

## 🎨 額外主題卡（給想多做的人）

> **主題卡 A：電動窗簾模擬**
> 用馬達模擬電動窗簾的開合（可以想像馬達轉動帶動窗簾軌道），設計「開啟」、「關閉」、「停止」三種按鈕控制，並加上緩啟動緩停止讓動作更平順。

> **主題卡 B：抽獎轉盤機**
> 用馬達模擬抽獎轉盤，按下按鈕後轉盤全速轉動，經過一段隨機時間（用 `random()` 決定）後緩慢停止，模擬真實抽獎機的運作邏輯。

> **主題卡 C：智慧風扇控制器**
> 用旋鈕控制馬達模擬風扇轉速，並結合第 6 週的 TMP36，設計「溫度越高，風扇轉速自動越快」的智慧散熱系統。

> **主題卡 D：物流分揀線模擬**
> 結合第 9 週的超音波感測器，設計一個「偵測到物體通過就短暫加速，確保物體被推送到下一階段」的分揀輸送帶邏輯。

---

## 📖 延伸連結：這週學到的東西如何應用在真實自動化場域

這週學到的技術，在真實工業與物流場域中有非常直接的對應應用：

| 這週學到的 | 真實世界的對應應用 |
|---|---|
| L293D 驅動一顆馬達 | 無人搬運車（AGV）、掃地機器人內建的雙馬達驅動電路，同時驅動左右兩顆輪子 |
| Enable 腳位控制轉速 | 真實自走車透過類似的 PWM 概念控制輪速，決定移動快慢 |
| Input1/Input2 決定正反轉 | 自走車透過設定左右輪「正轉/反轉」的組合，決定前進、後退、原地轉向 |
| 超音波避障邏輯（關卡 5） | 掃地機器人、無人搬運車內建的超音波或紅外線感測模組，邏輯幾乎完全相同：偵測到障礙物就減速或轉向 |
| 緩啟動 `rampTo()` | 真實自走車平滑加減速，避免移動時因慣性側翻或偏移，也是電動車、電梯常見的舒適度設計 |

這正是這學期反覆強調的重點——**你在 Tinkercad 上學到的邏輯思維，換到任何實體硬體平台都完全通用**。第 14 週我們會學到繼電器，那是另一種「以小控大」的元件，跟這週的 L293D 概念一脈相承。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| 直流馬達（DC Motor） | 直流馬達 | 通電後持續轉動的馬達，可控制轉速與方向 |
| L293D | L293D | 一款常見的馬達驅動晶片，俗稱「H 橋」 |
| H 橋（H-Bridge） | H 橋 | 一種能控制電流方向、進而控制馬達正反轉的電路架構 |
| Enable 腳位 | 致能腳位 | 決定馬達是否啟用，並可搭配 PWM 控制轉速 |
| Input 腳位（IN1/IN2） | 輸入腳位 | 決定 H 橋內部電流方向，進而決定馬達轉動方向 |
| 緩啟動（Soft Start） | 緩啟動 | 逐漸提升動力輸出，而非瞬間全速啟動 |
| 緩停止（Soft Stop） | 緩停止 | 逐漸降低動力輸出至停止，而非瞬間急停 |

---

## 反思與延伸討論

1. Arduino 無法直接驅動馬達，需要透過 L293D 這種「驅動晶片」放大控制能力。請討論：這種「用小訊號控制大功率裝置」的概念，在日常生活的哪些電器產品中也能找到類似的設計（提示：想想家裡的冷氣、電風扇遙控器）？
2. 這週的 `rampTo()` 函式跟第 7 週的 `sweepTo()` 函式，雖然控制的元件完全不同（馬達轉速 vs. 伺服馬達角度），但程式邏輯結構幾乎一模一樣。請討論：這種「不同硬體、相同程式邏輯」的現象，對於你們未來學習新的感測器或致動器，會帶來什麼樣的學習效率優勢？
3. 這是整個 Arduino 單元最後一次接觸新的動力元件，接下來兩週會學習顯示器與鍵盤輸入。請討論：回顧這十週學過的所有輸出裝置（LED、RGB LED、蜂鳴器、伺服馬達、直流馬達），你覺得哪一種元件的學習曲線最陡峭？為什麼？
4. 練習題 11 提到雙馬達差速驅動是機器人轉向的核心原理。請討論：如果左輪轉速比右輪快，機器人會往哪個方向轉？這跟划船時「一邊划得比較用力，船會往另一邊轉」的生活經驗是否類似？

> 💬 補充：以上四題建議在小組內輪流分享觀點，再統整出一份共同結論，這也是為未來期末專題分組合作預先累積的討論默契。

---

## 本週檢核清單

- [ ] 理解直流馬達與伺服馬達的差異（持續轉動 vs. 定點角度）
- [ ] 理解 L293D 驅動晶片與 H 橋的基本原理
- [ ] 能正確控制馬達正轉、反轉、停止
- [ ] 能用 PWM 實現無段變速控制
- [ ] 能實作緩啟動與緩停止的平順動力控制
- [ ] 能結合超音波感測器實作自動避障減速邏輯
- [ ] 完成至少 8 題基礎/中等練習題

---

## 下週預告

第 11 週我們要學習「產線資訊可視化」——LCD 16x2 顯示器。密室老闆想打造一間「產線戰情看板」主題房，你將學會如何在螢幕上顯示文字與數字資訊，把過去十週學過的各種感測器資料，整合到一塊真正的顯示螢幕上。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 9 週](./week-09.md) | [下一週：第 11 週 — 產線戰情看板 ➡](./week-11.md)
