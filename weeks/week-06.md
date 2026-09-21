[⬅ 回目錄](../README.md) | [⬅ 上一週：第 5 週](./week-05.md) | [下一週：第 7 週 — 機械保全閘門 ➡](./week-07.md)

---

# 第 6 週：智慧溫控保險庫
## 主題專案：把感測數值換算成有意義的物理量

> 🎯 **本週定位**：前五週的感測器讀值，我們大多直接拿 0~1023 的原始數字來用（或用 `map()` 轉換成另一個抽象範圍）。這一週我們要學會把感測器讀值換算成「真正有物理意義的單位」——攝氏溫度。密室老闆這次想打造一間「智慧溫控保險庫」主題房：保險庫必須維持在特定溫度區間，一旦過熱就會觸發警報，而且警報一旦觸發就會**持續鎖定**，玩家必須找到「復歸鈕」才能解除——這是工業控制系統中極為重要的安全設計理念，這週你會徹底搞懂它。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [電路體檢站（3 分鐘快速複習）](#電路體檢站3-分鐘快速複習)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道機關關卡](#後端程式設計五道機關關卡)
  - [關卡 1：溫度轉換 — 從電壓到攝氏度數](#關卡-1溫度轉換--從電壓到攝氏度數)
  - [關卡 2：雙態警示 — 簡易溫度門檻判斷](#關卡-2雙態警示--簡易溫度門檻判斷)
  - [關卡 3：三段分級 — 正常/警戒/危險](#關卡-3三段分級--正常警戒危險)
  - [關卡 4：警報鎖定與人工復歸](#關卡-4警報鎖定與人工復歸)
  - [關卡 5：可調式門檻 — 旋鈕客製化警戒值](#關卡-5可調式門檻--旋鈕客製化警戒值)
  - [🏆 BOSS 關：智慧溫控保險庫主控系統](#-boss-關智慧溫控保險庫主控系統)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎨 額外主題卡（給想多做的人）](#-額外主題卡給想多做的人)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～5 週）
- ✅ 數位/類比輸出入、`map()` 數值轉換
- ✅ 陣列、迴圈、自訂函式
- ✅ 狀態變數、防彈跳、狀態轉變偵測
- ✅ 蜂鳴器發聲、光敏電阻讀取、分壓電路原理

### 這週要新學的
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| TMP36 溫度感測器 | 輸出電壓隨溫度線性變化的感測元件 | 冷鏈物流、機房散熱、製程溫控的基礎元件 |
| 電壓換算公式 | 把 `analogRead()` 讀到的原始值轉換成實際電壓 | 理解感測器規格書上數值的意義 |
| `float` 浮點數 | 能儲存小數點的變數型別 | 溫度、電壓等連續物理量幾乎都需要小數精度 |
| 警報鎖定（Latching） | 一旦觸發異常，就持續維持警報狀態 | 確保異常事件不會被輕易忽略，是工安管理核心精神 |
| 人工復歸（Reset） | 需要人為確認並操作，才能解除鎖定的警報 | 避免自動系統在問題尚未真正解決前就「自己假裝沒事」 |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室老闆想打造一間「智慧溫控保險庫」逃脫房——保險庫內部裝有溫度感測器，玩家必須設法讓房間溫度維持在安全範圍內（劇情上可以想像是某種易融化的道具鎖），一旦溫度過高，警報系統就會鎖定，玩家必須找到隱藏的「復歸鈕」機關才能解除警報並繼續遊戲。你被指派設計整套溫控保護系統。
>
> 這週結束時，你會做出：
> 1. 一個能把感測值換算成真實攝氏溫度的讀取系統（暖身）
> 2. 一個簡單的雙態溫度警示機關
> 3. 一個三段式（正常/警戒/危險）分級警示系統
> 4. 一個具備警報鎖定與人工復歸功能的完整安全機關
> 5. 一個能用旋鈕客製化警戒門檻的彈性系統
> 6. 一個整合以上所有技巧的「智慧溫控保險庫主控系統」

---

## 電路體檢站（3 分鐘快速複習）

**體檢題 1**：上週學的光敏電阻，是「電阻值隨光線變化」；這週的 TMP36 則完全不同——它是主動輸出「電壓隨溫度變化」的感測器（不需要額外配對電阻做分壓）。這提醒我們：不同感測器的內部原理可能完全不同，但最終都會被 Arduino 用 `analogRead()` 讀取成 0~1023 的數字，後續的處理手法（門檻判斷、`map()` 轉換）則是相通的。

**體檢題 2**：回想第 4 週的 `map()` 概念——它是把一個數值範圍「等比例」轉換到另一個範圍。這週的溫度轉換公式，其實也是一種數值換算，只是這次換算的依據不是任意設定的範圍，而是 TMP36 規格書上明確定義的物理公式（稍後會詳細介紹）。

**體檢題 3**：為什麼溫度數值需要用 `float`（浮點數）而不是 `int`（整數）儲存？
> 答案：因為真實世界的溫度幾乎不會剛好是整數（例如 25.34°C），如果用 `int` 儲存，小數點後的資訊會直接被捨去，導致精確度下降。

---

## 模擬設計專案配置

### 元件清單
| 元件 | 數量 | 說明 |
|---|---|---|
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1 | 電路佈局 |
| TMP36 | 1 | 溫度感測器 |
| RGB LED（共陰極） | 1 | 溫度狀態指示燈（沿用第 3 週接法） |
| 220Ω 電阻 | 3 | RGB 三色限流電阻 |
| Buzzer | 1 | 警報發聲器（沿用第 5 週接法） |
| Pushbutton | 1 | 人工復歸鈕 |
| 10kΩ 電阻 | 1 | 按鈕下拉電阻 |
| Potentiometer | 1 | 門檻調整旋鈕 |

### 接線步驟

**Part A：TMP36 溫度感測器**
1. 拖曳一顆 **TMP36** 到麵包板（外觀類似電晶體，三隻腳）。
2. 面對感測器平面朝向自己時，由左至右依序為：**電源腳（5V）、訊號輸出腳（接 A2）、接地腳（GND）**（實際腳位排列請以 Tinkercad 元件說明為準，滑鼠移到元件上通常會顯示腳位標籤）。

**Part B：RGB LED（沿用第 3 週）**
1. R → ~9、G → ~10、B → ~11，各自串接 220Ω 電阻，共同負極接 GND。

**Part C：蜂鳴器（沿用第 5 週）**
1. 正極接 **Pin 8**，負極接 GND。

**Part D：人工復歸按鈕**
1. 沿用第 2 週接法：下拉電阻 + 訊號線接 **Pin 7**。

**Part E：門檻調整旋鈕**
1. 沿用第 4 週接法：中間腳接 **A0**，兩側接 5V 與 GND。

### 腳位對照表（本週共用）
| 元件 | 腳位 |
|---|---|
| TMP36 訊號腳 | A2 |
| RGB LED - R / G / B | ~9 / ~10 / ~11 |
| 蜂鳴器 | Pin 8 |
| 復歸按鈕 | Pin 7 |
| 門檻調整旋鈕 | A0 |

> 💡 **配置檢查清單**
> - [ ] TMP36 三隻腳分別正確接到 5V、A2、GND
> - [ ] RGB LED 與蜂鳴器已依先前週次的接法正確配置
> - [ ] 復歸按鈕與門檻旋鈕已正確接線

---

## 後端程式設計：五道機關關卡

### 關卡 1：溫度轉換 — 從電壓到攝氏度數

**機關故事**：在做出任何警報邏輯前，你得先確認能把 TMP36 的讀值正確換算成真實的攝氏溫度。

#### 範例 1-1：讀取 TMP36 原始數值
```cpp
// ============================================
// 範例 1-1：讀取 TMP36 的原始類比數值（0~1023）
// ============================================

int pinTemp = A2;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int rawValue = analogRead(pinTemp);
  Serial.println(rawValue);
  delay(300);
}
```

#### 範例 1-2：換算成實際電壓
```cpp
// ============================================
// 範例 1-2：把 0~1023 的原始值換算成 0~5V 的實際電壓
// 公式：實際電壓 = 原始值 × (5.0 / 1023.0)
// ============================================

int pinTemp = A2;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);

  Serial.print("原始值: ");
  Serial.print(rawValue);
  Serial.print("   電壓: ");
  Serial.print(voltage);
  Serial.println(" V");

  delay(300);
}
```
**教學重點**：這裡第一次正式使用 `float` 浮點數變數。注意公式裡特地寫成 `5.0` 與 `1023.0`（帶小數點），而不是 `5` 與 `1023`——如果不寫成小數形式，Arduino 會誤以為這是「整數除法」，導致結果被無條件捨去小數，得到不正確的答案。這是新手非常容易犯的錯誤，請務必牢記。

#### 範例 1-3：套用 TMP36 公式換算成攝氏溫度
```cpp
// ============================================
// 範例 1-3：TMP36 感測器讀值轉換公式
// TMP36 特性：攝氏 0 度時輸出 0.5V，之後每上升 1 度輸出增加 10mV（0.01V）
// 公式：攝氏溫度 = (電壓 - 0.5) × 100
// ============================================

int pinTemp = A2;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  float celsius = (voltage - 0.5) * 100.0;

  Serial.print("溫度: ");
  Serial.print(celsius);
  Serial.println(" °C");
  delay(300);
}
```
**Tinkercad 操作提醒**：點擊 TMP36 元件，可以直接拖曳「溫度」滑桿模擬真實環境溫度變化，不需要真的用手指觸摸感測器。

#### 範例 1-4：把換算邏輯封裝成函式
```cpp
// ============================================
// 範例 1-4：把溫度換算邏輯封裝成一個可重複呼叫的函式
// 好處：之後任何地方需要讀溫度，直接呼叫 readTemperature() 即可
// ============================================

int pinTemp = A2;

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  float celsius = (voltage - 0.5) * 100.0;
  return celsius;   // 把計算結果「回傳」給呼叫這個函式的地方
}

void setup() {
  Serial.begin(9600);
}

void loop() {
  float temp = readTemperature();
  Serial.println(temp);
  delay(300);
}
```
**新語法提醒**：`float readTemperature()` 開頭的 `float` 代表這個函式執行完會「回傳」一個浮點數結果，函式內部用 `return celsius;` 把結果送出去。這跟之前學過「沒有回傳值」的函式（開頭寫 `void`）不同，是這學期第一次接觸「有回傳值的函式」，往後會經常用到這種寫法讓程式碼更模組化。

---

### 關卡 2：雙態警示 — 簡易溫度門檻判斷

**機關故事**：保險庫溫度過高會觸發簡單的雙態警示——正常顯示綠燈，超溫顯示紅燈。

#### 範例 2-1：超溫判斷
```cpp
// ============================================
// 範例 2-1：溫度超過門檻亮紅燈，否則亮綠燈
// ============================================

int pinTemp = A2;
int pinR = 9, pinG = 10, pinB = 11;
float threshold = 30.0;

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

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  float celsius = readTemperature();

  if (celsius > threshold) {
    setColor(255, 0, 0);
  } else {
    setColor(0, 255, 0);
  }
}
```

#### 範例 2-2：加上聲音警示
```cpp
// ============================================
// 範例 2-2：超溫時同時發出警報聲
// ============================================

int pinTemp = A2;
int pinR = 9, pinG = 10, pinB = 11;
int pinBuzzer = 8;
float threshold = 30.0;

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

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  float celsius = readTemperature();

  if (celsius > threshold) {
    setColor(255, 0, 0);
    tone(pinBuzzer, 2000);
  } else {
    setColor(0, 255, 0);
    noTone(pinBuzzer);
  }
}
```

---

### 關卡 3：三段分級 — 正常/警戒/危險

**機關故事**：品保主管要求不能只有「正常/異常」兩段，而要有「正常、警戒、危險」三段式管理，讓玩家能提早感受到溫度接近臨界值。

#### 範例 3-1：三段溫度狀態
```cpp
// ============================================
// 範例 3-1：三段溫度狀態 - 正常(綠)、警戒(黃)、危險(紅)
// ============================================

int pinTemp = A2;
int pinR = 9, pinG = 10, pinB = 11;

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

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  float celsius = readTemperature();

  if (celsius < 25.0) {
    setColor(0, 255, 0);         // 正常：綠燈
  } else if (celsius < 32.0) {
    setColor(255, 255, 0);       // 警戒：黃燈
  } else {
    setColor(255, 0, 0);         // 危險：紅燈
  }
}
```

#### 範例 3-2：三段狀態搭配文字說明
```cpp
// ============================================
// 範例 3-2：加上 Serial 文字說明，方便除錯與監控
// ============================================

int pinTemp = A2;
int pinR = 9, pinG = 10, pinB = 11;

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

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float celsius = readTemperature();

  Serial.print("目前溫度: ");
  Serial.print(celsius);

  if (celsius < 25.0) {
    setColor(0, 255, 0);
    Serial.println("  狀態: 正常");
  } else if (celsius < 32.0) {
    setColor(255, 255, 0);
    Serial.println("  狀態: 警戒");
  } else {
    setColor(255, 0, 0);
    Serial.println("  狀態: 危險");
  }
  delay(300);
}
```

#### 範例 3-3：三段狀態各自搭配不同聲音節奏
```cpp
// ============================================
// 範例 3-3：三段狀態除了燈光顏色，聲音節奏也各不相同
// ============================================

int pinTemp = A2;
int pinR = 9, pinG = 10, pinB = 11;
int pinBuzzer = 8;

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

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  float celsius = readTemperature();

  if (celsius < 25.0) {
    setColor(0, 255, 0);
    noTone(pinBuzzer);
  } else if (celsius < 32.0) {
    setColor(255, 255, 0);
    tone(pinBuzzer, 600);
    delay(300);
    noTone(pinBuzzer);
    delay(700);
  } else {
    setColor(255, 0, 0);
    tone(pinBuzzer, 2000);
    delay(100);
    noTone(pinBuzzer);
    delay(100);
  }
}
```

---

### 關卡 4：警報鎖定與人工復歸

**機關故事**：真實工廠規範通常要求「一旦異常觸發，即使數值恢復正常，也必須有人到場確認並手動解除」，避免問題被輕忽——這是工安管理中「事件必須被記錄與處理」的重要精神，也是這週最核心的教學重點。

#### 範例 4-1：警報鎖定（Latching）
```cpp
// ============================================
// 範例 4-1：一旦超溫觸發警報，即使溫度下降，警報仍持續響
// 關鍵觀念：用一個變數「鎖住」異常狀態，不隨感測值即時變動
// ============================================

int pinTemp = A2;
int pinBuzzer = 8;
float threshold = 30.0;
bool alarmLatched = false;   // 警報鎖定旗標

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
}

void loop() {
  float celsius = readTemperature();

  if (celsius > threshold) {
    alarmLatched = true;   // 一旦超過門檻，永久鎖定為 true
  }

  if (alarmLatched) {
    tone(pinBuzzer, 2000);
  } else {
    noTone(pinBuzzer);
  }
}
```
**觀察任務**：在 Tinkercad 中把溫度滑桿調高觸發警報，再把滑桿調回正常溫度，你會發現蜂鳴器**仍然持續響著**——這正是警報鎖定的效果。

#### 範例 4-2：加入復歸按鈕
```cpp
// ============================================
// 範例 4-2：必須人工按下 Reset 鈕才能解除警報鎖定
// ============================================

int pinTemp = A2;
int pinBuzzer = 8;
int pinReset = 7;
float threshold = 30.0;
bool alarmLatched = false;

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  pinMode(pinReset, INPUT);
}

void loop() {
  float celsius = readTemperature();

  if (celsius > threshold) {
    alarmLatched = true;
  }

  // 人工復歸：只有按下 Reset 鈕，且此時溫度已經恢復正常，才允許解除警報
  if (digitalRead(pinReset) == HIGH && celsius <= threshold) {
    alarmLatched = false;
  }

  if (alarmLatched) {
    tone(pinBuzzer, 2000);
  } else {
    noTone(pinBuzzer);
  }
}
```
**教學討論重點**：為什麼 Reset 邏輯要加上「且溫度已恢復正常」這個條件？如果不加會發生什麼問題？（引導思考：若不加，代表可以在異常狀態下強制消音卻不解決問題，這在真實工廠中是嚴重的管理漏洞）

#### 範例 4-3：加上異常次數統計
```cpp
// ============================================
// 範例 4-3：每次觸發警報鎖定，累計異常次數並印出紀錄
// ============================================

int pinTemp = A2;
int pinBuzzer = 8;
int pinReset = 7;
float threshold = 30.0;
bool alarmLatched = false;
int alarmCount = 0;

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  pinMode(pinReset, INPUT);
  Serial.begin(9600);
}

void loop() {
  float celsius = readTemperature();

  if (celsius > threshold && !alarmLatched) {
    alarmLatched = true;
    alarmCount++;
    Serial.print("警報觸發！累計次數：");
    Serial.println(alarmCount);
  }

  if (digitalRead(pinReset) == HIGH && celsius <= threshold) {
    if (alarmLatched) {
      Serial.println("警報已人工解除");
    }
    alarmLatched = false;
  }

  if (alarmLatched) {
    tone(pinBuzzer, 2000);
  } else {
    noTone(pinBuzzer);
  }
}
```
**教學重點**：注意這裡的計數條件是 `celsius > threshold && !alarmLatched`——加上 `!alarmLatched` 是為了確保「同一次異常事件」不會因為溫度持續處於超標狀態而被重複計數，這又是一次「狀態轉變偵測」概念的應用（跟第 5 週的入侵計數器邏輯相通）。

---

### 關卡 5：可調式門檻 — 旋鈕客製化警戒值

**機關故事**：不同保險庫存放的物品耐熱程度不同，需要能現場彈性調整警戒溫度，而不用重新上傳程式。

#### 範例 5-1：旋鈕設定溫度門檻
```cpp
// ============================================
// 範例 5-1：用旋鈕設定「警報觸發溫度」，範圍 0~50°C
// ============================================

int pinTemp = A2;
int pinPot = A0;
int pinR = 9, pinG = 10, pinB = 11;

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

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int potValue = analogRead(pinPot);
  float threshold = map(potValue, 0, 1023, 0, 50);   // 旋鈕設定門檻範圍：0~50°C

  float celsius = readTemperature();

  if (celsius > threshold) {
    setColor(255, 0, 0);
  } else {
    setColor(0, 255, 0);
  }

  Serial.print("目前門檻: ");
  Serial.print(threshold);
  Serial.print("   目前溫度: ");
  Serial.println(celsius);
}
```
**教學提醒**：`map()` 的來源範例中提到 `map()` 是整數運算，但這裡目標範圍是 `0, 50`，而 `threshold` 被宣告成 `float`——這種寫法在 Arduino 中是可以運作的（`map()` 回傳的整數會自動轉型成浮點數存入 `threshold`），但如果需要更精確的小數門檻（例如 25.5°C），單純用 `map()` 是不夠精細的，這也是第 4 週練習題提過的「整數運算精度陷阱」。

#### 範例 5-2：旋鈕門檻 + 警報鎖定整合
```cpp
// ============================================
// 範例 5-2：把可調式門檻與警報鎖定機制結合
// ============================================

int pinTemp = A2;
int pinPot = A0;
int pinBuzzer = 8;
int pinReset = 7;
bool alarmLatched = false;

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  pinMode(pinReset, INPUT);
  Serial.begin(9600);
}

void loop() {
  int potValue = analogRead(pinPot);
  float threshold = map(potValue, 0, 1023, 0, 50);

  float celsius = readTemperature();

  if (celsius > threshold) {
    alarmLatched = true;
  }
  if (digitalRead(pinReset) == HIGH && celsius <= threshold) {
    alarmLatched = false;
  }

  if (alarmLatched) {
    tone(pinBuzzer, 2000);
  } else {
    noTone(pinBuzzer);
  }
}
```

---

### 🏆 BOSS 關：智慧溫控保險庫主控系統

**機關故事**：整合本週所有技巧，打造完整的保險庫溫控系統——三段分級顯示、可調式門檻、警報鎖定與人工復歸、異常次數統計，這是這學期目前為止功能最完整的一個系統。

```cpp
// ============================================
// BOSS 關：智慧溫控保險庫主控系統 - 整合本週所有技巧
// 功能：即時溫度讀取 + 旋鈕可調門檻 + 三段分級顯示 + 警報鎖定與復歸 + 異常統計
// ============================================

int pinTemp = A2;
int pinPot = A0;
int pinR = 9, pinG = 10, pinB = 11;
int pinBuzzer = 8;
int pinReset = 7;

bool alarmLatched = false;
int alarmCount = 0;

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

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(pinReset, INPUT);
  Serial.begin(9600);
  Serial.println("=== 保險庫溫控系統啟動 ===");
}

void loop() {
  int potValue = analogRead(pinPot);
  float threshold = map(potValue, 0, 1023, 20, 45);   // 旋鈕可調範圍設定為 20~45°C，較貼近實際保險庫應用情境

  float celsius = readTemperature();
  float warningThreshold = threshold - 5;   // 警戒門檻比危險門檻低 5 度，預留緩衝空間

  // --- 異常觸發判定（含次數統計，只在剛觸發瞬間計數） ---
  if (celsius > threshold && !alarmLatched) {
    alarmLatched = true;
    alarmCount++;
    Serial.print("!!! 保險庫過熱警報！累計異常次數：");
    Serial.println(alarmCount);
  }

  // --- 人工復歸判定 ---
  if (digitalRead(pinReset) == HIGH && celsius <= threshold) {
    if (alarmLatched) {
      Serial.println(">>> 警報已由管理員人工解除 <<<");
    }
    alarmLatched = false;
  }

  // --- 燈光與聲音呈現 ---
  if (alarmLatched) {
    setColor(255, 0, 0);
    tone(pinBuzzer, 2000);
    delay(100);
    noTone(pinBuzzer);
    delay(100);
  } else if (celsius > warningThreshold) {
    setColor(255, 255, 0);   // 警戒中但尚未觸發鎖定
    noTone(pinBuzzer);
  } else {
    setColor(0, 255, 0);     // 正常
    noTone(pinBuzzer);
  }
}
```
**教學重點**：這段程式碼引入了一個新概念——`warningThreshold = threshold - 5`，用「危險門檻」動態算出「警戒門檻」，而不是另外設定一個獨立的固定數字。這種「用一個基準值推算出其他相關數值」的手法，在工業參數設定中非常常見（例如安全裕度、容許誤差範圍的設計），能讓系統維護更簡單——未來只要調整一個基準值，其他相關門檻就會自動跟著連動。

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| 溫度數值完全不合理（例如顯示幾千度） | 電壓換算公式寫錯，忘記寫成 `5.0 / 1023.0`（浮點數除法） | 檢查是否誤寫成整數除法 `5 / 1023`，導致結果永遠是 0 |
| 溫度數值偏移一個固定量（例如都比預期高 10 度） | TMP36 換算公式的 `0.5` 或 `100.0` 寫錯 | 對照範例 1-3 逐字核對公式 |
| 警報永遠不會鎖定 | 忘記把 `if (celsius > threshold)` 寫成「只設定為 true，不設為 false」 | 檢查邏輯中是否誤寫了會讓 `alarmLatched` 自動變回 false 的程式碼 |
| 復歸按鈕按了沒反應 | 忘記加上「溫度已恢復正常」的條件，或按鈕接線有誤 | 檢查 `if (digitalRead(pinReset) == HIGH && celsius <= threshold)` 兩個條件是否都成立 |
| 異常次數統計不斷瘋狂累加 | 忘記加上 `!alarmLatched` 的狀態轉變判斷 | 檢查計數條件是否為 `celsius > threshold && !alarmLatched` |
| `float` 變數印出來的小數點後面全是 0 | 屬正常現象，`Serial.println()` 預設只顯示到小數點後 2 位 | 這不是錯誤，是 Arduino 的預設顯示格式 |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改範例 2-1 的溫度門檻，從 30.0 改成 28.0，並測試效果。
2. 修改範例 3-1 的三段門檻數值，自行設計一組你認為合理的正常/警戒/危險溫度區間。
3. 在範例 1-3 的基礎上，新增功能：溫度低於 0°C 時，額外印出 "注意：低溫警告" 的文字。
4. 修改範例 4-1，把警報聲的頻率從 2000Hz 改成 1500Hz。
5. 修改範例 5-1，把旋鈕可調範圍從 0~50°C 改成 10~40°C。

### 🟡 中等題
6. 設計一個「最高溫度紀錄器」：新增一個變數記錄「歷史最高溫度」，即使目前溫度下降，仍持續顯示曾經出現過的最高值，並在每次刷新最高紀錄時印出提示。
7. 修改 BOSS 關程式碼，新增「低溫警報」：溫度低於某個下限（例如 5°C）時，也要觸發類似的警報鎖定機制（模擬冷凍設備故障失溫的情境）。
8. 設計一個「平均溫度計算器」：連續讀取溫度 5 次並計算平均值，再拿平均值去做門檻判斷，藉此降低單次讀值雜訊造成的誤判（可參考第 4 週練習題 13 的類似手法）。
9. 修改範例 4-3 的異常次數統計，新增功能：異常次數達到 3 次以上時，額外觸發更急促的警報聲（頻率提高、間隔縮短）。
10. 設計一個「雙重確認復歸」機制：復歸按鈕必須「連續按兩次」（模擬需要二次確認的安全設計）才能真正解除警報，單按一次只會印出 "請再次確認" 的提示。

### 🔴 挑戰題
11. **溫度變化速率警報**：除了絕對溫度門檻，工業上有時更關心「溫度上升速度」（例如短時間內溫度暴衝，即使還沒到達絕對門檻也該示警）。請設計一個機制，比較「這一輪讀到的溫度」與「上一輪讀到的溫度」，如果差距超過某個數值，觸發「溫度異常上升」的提示。
12. **多重感測整合**：想像保險庫加裝了第二顆 TMP36（需自行擴充接線到 A3），設計一個「兩個感測器讀值平均」或「取兩者中較高值」的系統，模擬雙重感測互相校驗的可靠度設計。
13. **時間窗口式異常判定（進階思考）**：目前的警報邏輯是「只要瞬間超過門檻就觸發」，但真實世界中，感測器偶爾的瞬間雜訊不應該被當作真正異常。請設計（可先用文字描述邏輯，不一定要完整實作）一個「溫度必須連續超標一段時間才觸發警報」的機制。
14. **多段復歸權限模擬**：設計一個機制，異常次數達到 5 次以上時，一般的復歸按鈕不再有效，必須額外用第二顆「主管授權」按鈕（需自行擴充接線）才能解除，模擬真實工廠中「重大異常需要更高權限才能復歸」的管理邏輯。
15. **完全自訂溫控系統**：不參考任何範例，設計一個你認為合理的「智慧倉儲溫控保護系統」，至少要用到：`float` 換算函式、警報鎖定與人工復歸、至少一個可調式門檻。

---

## 🎨 額外主題卡（給想多做的人）

> **主題卡 A：發燒警示手環模擬**
> 用 TMP36 模擬體溫感測，設計「正常體溫、微燒、高燒」三段警示，並在高燒時額外閃爍紅燈提醒。

> **主題卡 B：3C 產品過熱保護模擬**
> 模擬手機或筆電的過熱保護機制：溫度正常時全速運作（RGB 顯示綠色），溫度過高時「降頻保護」（RGB 顯示黃色並且原本快速的某個燈光效果變慢），溫度持續過高則直接「強制關機」（RGB 全暗並響起警報）。

> **主題卡 C：溫室栽培監控站**
> 設計一個溫室溫度監控系統：溫度過低（低於某門檻）時觸發「加溫警示」，溫度過高時觸發「通風警示」，兩種異常各自有獨立的燈光顏色與警報鎖定邏輯。

> **主題卡 D：食品倉儲多層級管理**
> 模擬不同食品類別（冷凍、冷藏、常溫）各自有不同的安全溫度範圍，用旋鈕切換「目前監控的食品類別」，程式自動套用對應的門檻設定（提示：可以用陣列儲存多組門檻資料）。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| TMP36 | TMP36 | 一款輸出電壓隨溫度線性變化的溫度感測器 |
| `float` | 浮點數 | 能儲存帶有小數點數值的變數型別 |
| 浮點數除法 | 浮點數除法 | 用帶小數點的數字進行除法，才能得到精確的小數結果 |
| `return` | 回傳 | 函式執行完後，把結果送回給呼叫它的地方 |
| 警報鎖定（Latching） | 警報鎖定 | 一旦異常觸發，就持續維持狀態，不隨即時數值變動而自動恢復 |
| 人工復歸（Manual Reset） | 人工復歸 | 需要人為操作（如按下按鈕）才能解除鎖定狀態 |
| 緩衝空間（Buffer / Margin） | 緩衝空間 | 在真正危險門檻之前，預留一段警戒區間提前示警 |
| 安全裕度 | 安全裕度 | 系統實際容忍範圍與理論極限之間預留的安全空間 |

---

## 反思與延伸討論

1. 本週最核心的觀念是「警報鎖定與人工復歸」。請討論：為什麼工業安全系統普遍採用這種「寧可繁瑣也要人工確認」的設計，而不是讓系統「數值恢復正常就自動解除警報」？這種設計理念，跟我們日常生活中還有哪些場景相似（例如：家用瓦斯外洩警報器、電梯異常按鈕）？
2. BOSS 關程式碼中，`warningThreshold` 是用 `threshold - 5` 動態算出來的，而不是另外設定一個固定數字。請討論：這種「用一個基準值推算其他相關參數」的設計方式，相較於「每個門檻都各自獨立設定」，各自有什麼優缺點？
3. 練習題 13 提到「時間窗口式異常判定」——溫度必須連續超標一段時間才觸發警報，而不是瞬間超標就觸發。請討論：這種設計在什麼情境下特別重要？如果反過來，什麼情境下「瞬間就要立刻觸發警報」才是正確的設計（提示：想想跟人身安全高度相關的場景）？

---

## 本週檢核清單

- [ ] 能把 `analogRead()` 讀值正確換算成攝氏溫度
- [ ] 理解 `float` 浮點數與整數運算的差異，避免除法精度陷阱
- [ ] 能寫出「有回傳值」的自訂函式（`float` 開頭 + `return`）
- [ ] 能實作多段（三段以上）門檻分級判斷
- [ ] 能實作警報鎖定與人工復歸機制
- [ ] 能結合旋鈕設計可調式門檻系統
- [ ] 完成至少 8 題基礎/中等練習題

---

## 下週預告

第 7 週我們要幫密室加入「精準角度控制」——伺服馬達。密室老闆想打造一間「機械保全閘門」主題房，玩家必須透過解謎觸發閘門機構的開啟，你將學會如何控制馬達轉到指定角度，並整合前幾週學過的感測器，做出會自動反應的機械裝置。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 5 週](./week-05.md) | [下一週：第 7 週 — 機械保全閘門 ➡](./week-07.md)
