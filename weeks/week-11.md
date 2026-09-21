[⬅ 回目錄](../README.md) | [⬅ 上一週：第 10 週](./week-10.md) | [下一週：第 12 週 — 期末統整與電子密碼鎖 ➡](./week-12.md)

---

# 第 11 週：產線戰情看板
## 主題專案：讓機關第一次「說人話」

> 🎯 **本週定位**：這學期十週來，機關與玩家的溝通方式一直只有「燈光」跟「聲音」——玩家必須自己解讀顏色或音調代表什麼意思。這一週你會學到 **LCD 16x2 顯示器**，讓機關第一次能直接顯示「文字」，把過去十週學過的各種感測器資料（溫度、距離、狀態），整合到一塊真正的螢幕看板上。密室老闆想打造一間「產線戰情看板」主題房，模擬工廠中控室的即時資訊顯示系統。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [電路體檢站（3 分鐘快速複習）](#電路體檢站3-分鐘快速複習)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道機關關卡](#後端程式設計五道機關關卡)
  - [關卡 1：LCD 基礎控制 — 認識 LiquidCrystal.h](#關卡-1lcd-基礎控制--認識-liquidcrystalh)
  - [關卡 2：動態文字 — 顯示變數與數值](#關卡-2動態文字--顯示變數與數值)
  - [關卡 3：多感測器戰情看板](#關卡-3多感測器戰情看板)
  - [關卡 4：畫面更新優化 — 解決閃爍問題](#關卡-4畫面更新優化--解決閃爍問題)
  - [關卡 5：自訂字元與跑馬燈文字](#關卡-5自訂字元與跑馬燈文字)
  - [🏆 BOSS 關：產線戰情看板主控系統](#-boss-關產線戰情看板主控系統)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎨 額外主題卡（給想多做的人）](#-額外主題卡給想多做的人)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～10 週）
- ✅ 數位/類比輸出入、`map()`、`constrain()`、`float` 運算
- ✅ 陣列、迴圈、自訂函式、物件（Servo）
- ✅ 蜂鳴器、光敏電阻、TMP36、伺服馬達、超音波、直流馬達
- ✅ 模組化系統架構設計、優先權排序、緩啟停控制

### 這週要新學的
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| LCD 16x2 顯示器 | 能顯示 16 字元 x 2 行文字的液晶顯示器 | 幾乎所有工業設備都有類似的文字顯示面板 |
| `<LiquidCrystal.h>` | Arduino 內建的 LCD 控制函式庫 | 這學期第二次使用外部函式庫（第一次是 Servo） |
| `lcd.setCursor()` | 把游標移動到指定位置（欄, 列） | 精準控制文字顯示位置的必要技巧 |
| 畫面更新優化 | 只在數值真正改變時才重新印刷，避免閃爍 | 使用者介面設計的重要細節，影響專業感 |
| 自訂字元 | 用 5x8 點陣自行設計 LCD 顯示的圖示 | 突破純文字限制，做出更豐富的視覺呈現 |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室老闆參觀了一間真正的工廠中控室後大受啟發，決定打造一間「產線戰情看板」逃脫房——牆上有一塊即時顯示各項數據的螢幕，玩家必須讀懂螢幕上的資訊變化，才能找到正確的解謎線索。你被指派設計整套顯示系統。
>
> 這週結束時，你會做出：
> 1. 一個能顯示基本文字的 LCD 測試機關（暖身）
> 2. 一套能顯示即時變動數值的動態文字系統
> 3. 一套整合多個感測器資料的戰情看板
> 4. 一套解決畫面閃爍問題的優化顯示邏輯
> 5. 一套具備自訂圖示與跑馬燈文字效果的進階顯示
> 6. 一個整合以上所有技巧的「產線戰情看板主控系統」

---

## 電路體檢站（3 分鐘快速複習）

**體檢題 1**：LCD 顯示器需要接的線特別多（本週會用到 6 條資料線加上電源線），這跟第 7 週伺服馬達（只要 3 條線）或第 9 週超音波感測器（只要 4 條線）差很多。回想這學期一路的接線經驗——元件功能越複雜，通常需要的控制線也越多，LCD 需要同時控制「要顯示哪個字元」與「顯示在哵個位置」，自然需要較多訊號線來傳遞這些複雜的指令。

**體檢題 2**：這週會再次用到「函式庫」（`<LiquidCrystal.h>`），這是第 7 週學過 `<Servo.h>` 之後第二次接觸。回想第 7 週學過的「物件」概念——LCD 的操作方式也是先建立一個物件（例如 `LiquidCrystal lcd(...)`），再透過「物件名稱.功能()」的方式操作，這種寫法模式在往後遇到新的複雜元件時會不斷重複出現，是這學期非常重要的可遷移技能。

**體檢題 3**：LCD 顯示器需要接電阻保護嗎？
> 答案：不需要限流電阻，但需要一顆可變電阻（沿用第 4 週學過的元件）用來調整螢幕的「對比度」，這是 LCD 特有的接線需求，稍後接線指引會詳細說明。

---

## 模擬設計專案配置

### 元件清單
| 元件 | 數量 | 說明 |
|---|---|---|
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1 | 電路佈局 |
| LCD 16x2 | 1 | 顯示器核心元件 |
| Potentiometer | 1 | LCD 對比度調整（沿用第 4 週元件，但用途不同） |
| Ultrasonic Distance Sensor（HC-SR04） | 1 | 沿用第 9 週，做整合顯示示範 |
| TMP36 | 1 | 沿用第 6 週，做整合顯示示範 |
| Pushbutton | 1 | 頁面切換鈕 |
| 10kΩ 電阻 | 1 | 按鈕下拉電阻 |

### 接線步驟

**Part A：LCD 16x2**
依照下表接線（Tinkercad 元件上已標示每隻腳的名稱，對照即可）：

| LCD 腳位 | 接到 Arduino |
|---|---|
| VSS | GND |
| VDD | 5V |
| V0（對比度調整） | 接一顆可變電阻的中間腳，可變電阻另兩腳分接 5V 與 GND |
| RS | Pin 7 |
| RW | GND |
| E | Pin 8 |
| D4 | Pin 9 |
| D5 | Pin 10 |
| D6 | Pin 11 |
| D7 | Pin 12 |
| A（背光+） | 5V |
| K（背光-） | GND |

**Part B：頁面切換按鈕**
1. 沿用第 2 週接法：下拉電阻 + 訊號線接 **Pin 2**。

**Part C：超音波感測器（沿用第 9 週）**
1. Trig 接 **Pin 3**，Echo 接 **Pin 4**（因 LCD 已佔用大量腳位，本週腳位需重新調整）。

**Part D：TMP36（沿用第 6 週）**
1. 訊號腳接 **A2**。

> ⚠️ **教學提醒**：LCD 接線腳位較多，建議教師提供「已完成接線」的 Tinkercad 範例電路連結，把教學重點放在程式邏輯而非反覆除錯接線。

### 腳位對照表（本週共用）
| 元件 | 腳位 |
|---|---|
| LCD RS / E / D4 / D5 / D6 / D7 | 7 / 8 / 9 / 10 / 11 / 12 |
| 頁面切換按鈕 | Pin 2 |
| 超音波 Trig / Echo | Pin 3 / Pin 4 |
| TMP36 | A2 |

> 💡 **配置檢查清單**
> - [ ] LCD 十二隻腳全部依表格正確接線，對比度可變電阻已接妥
> - [ ] 頁面切換按鈕、超音波感測器、TMP36 均已依本週新腳位配置正確接線

---

## 後端程式設計：五道機關關卡

### 關卡 1：LCD 基礎控制 — 認識 LiquidCrystal.h

**機關故事**：在做出任何複雜的資訊看板前，你得先確認 LCD 螢幕真的能顯示文字。

#### 範例 1-1：顯示 "Hello World"
```cpp
// ============================================
// 範例 1-1：LCD 基礎顯示
// ============================================

#include <LiquidCrystal.h>

// 依序對應 RS, E, D4, D5, D6, D7 腳位
LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

void setup() {
  lcd.begin(16, 2);          // 初始化 LCD：16 字元 x 2 行
  lcd.print("Hello World");  // 預設從第一行第一格開始印
}

void loop() {
  // LCD 顯示內容通常在 setup() 就設定好，不需要每次 loop 都重印
}
```

#### 範例 1-2：游標定位 — 第二行顯示
```cpp
// ============================================
// 範例 1-2：用 setCursor() 控制文字顯示位置
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

void setup() {
  lcd.begin(16, 2);
  lcd.print("Control Room");
  lcd.setCursor(0, 1);        // 游標移到第二行（行號從 0 開始）第一格
  lcd.print("System: OK");
}

void loop() {
}
```
**新語法提醒**：`lcd.setCursor(欄, 列)` 的第一個參數是「第幾個字元位置」（0~15），第二個參數是「第幾行」（0 或 1，因為只有兩行）。務必記得欄與列都是從 0 開始算。

#### 範例 1-3：清空螢幕與重新顯示
```cpp
// ============================================
// 範例 1-3：認識 lcd.clear() 清空螢幕
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

void setup() {
  lcd.begin(16, 2);
  lcd.print("Loading...");
  delay(2000);

  lcd.clear();                // 清空整個螢幕
  lcd.print("System Ready!");
}

void loop() {
}
```

---

### 關卡 2：動態文字 — 顯示變數與數值

**機關故事**：靜態文字太無趣，戰情看板需要顯示「會即時變動」的數字資訊。

#### 範例 2-1：顯示會自動遞增的計數器
```cpp
// ============================================
// 範例 2-1：第二行顯示會自動加 1 的運轉時間計數器
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int seconds = 0;

void setup() {
  lcd.begin(16, 2);
  lcd.print("Line Status");
}

void loop() {
  lcd.setCursor(0, 1);
  lcd.print("Uptime: ");
  lcd.print(seconds);
  lcd.print("s  ");   // 多印幾個空白字元，蓋掉上一次的殘留數字

  delay(1000);
  seconds++;
}
```
**教學重點**：注意 `lcd.print("s  ")` 特地多印了兩個空白字元，這是為了「蓋掉」上一次顯示的殘留數字（例如秒數從 100 變成 99 時，如果不多印空白，殘留的字元 "0" 不會自動消失）。這是這學期第一次接觸「螢幕殘影」的實務問題，第 4 關會有更完整的解法。

#### 範例 2-2：顯示超音波距離
```cpp
// ============================================
// 範例 2-2：LCD 顯示超音波測得的距離
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int pinTrig = 3;
int pinEcho = 4;

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
  lcd.begin(16, 2);
  lcd.print("Distance:");
}

void loop() {
  float distance = readDistance();
  lcd.setCursor(0, 1);
  lcd.print("Dist: ");
  lcd.print(distance);
  lcd.print(" cm   ");
  delay(300);
}
```

#### 範例 2-3：顯示溫度數值
```cpp
// ============================================
// 範例 2-3：LCD 顯示 TMP36 溫度
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int pinTemp = A2;

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  lcd.begin(16, 2);
}

void loop() {
  float temp = readTemperature();
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(temp);
  lcd.print("C   ");
  delay(300);
}
```

---

### 關卡 3：多感測器戰情看板

**機關故事**：真正的戰情看板需要同時呈現多項數據，這一關我們把前幾週學過的感測器資料整合到同一塊螢幕上。

#### 範例 3-1：雙行顯示 — 第一行溫度、第二行距離
```cpp
// ============================================
// 範例 3-1：綜合儀表板 - 第一行溫度、第二行距離
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int pinTrig = 3;
int pinEcho = 4;
int pinTemp = A2;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  lcd.begin(16, 2);
}

void loop() {
  float temp = readTemperature();
  float distance = readDistance();

  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(temp);
  lcd.print("C   ");

  lcd.setCursor(0, 1);
  lcd.print("Dist: ");
  lcd.print(distance);
  lcd.print("cm   ");

  delay(300);
}
```

#### 範例 3-2：頁面切換 — 按鈕控制顯示不同資訊頁
```cpp
// ============================================
// 範例 3-2：按鈕切換頁面 - 頁面1顯示溫度，頁面2顯示距離
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int pinTrig = 3;
int pinEcho = 4;
int pinTemp = A2;
int pinButton = 2;
int lastButtonState = LOW;
int currentPage = 0;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinButton, INPUT);
  lcd.begin(16, 2);
}

void loop() {
  int currentState = digitalRead(pinButton);
  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    currentPage = (currentPage + 1) % 2;
    lcd.clear();   // 切換頁面前先清空螢幕，避免文字重疊
  }
  lastButtonState = currentState;

  if (currentPage == 0) {
    float temp = readTemperature();
    lcd.setCursor(0, 0);
    lcd.print("Page 1: Temp");
    lcd.setCursor(0, 1);
    lcd.print(temp);
    lcd.print(" C     ");
  } else {
    float distance = readDistance();
    lcd.setCursor(0, 0);
    lcd.print("Page 2: Dist");
    lcd.setCursor(0, 1);
    lcd.print(distance);
    lcd.print(" cm    ");
  }
  delay(200);
}
```

---

### 關卡 4：畫面更新優化 — 解決閃爍問題

**機關故事**：主管注意到螢幕數字一直在閃爍，看起來很不專業，要求你優化顯示邏輯。

#### 範例 4-1：只在數值真正改變時才更新畫面
```cpp
// ============================================
// 範例 4-1：只在溫度變化超過門檻時才重新印出，避免不必要的閃爍
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int pinTemp = A2;
float lastDisplayedTemp = -999;   // 給一個不可能出現的初始值，確保第一次一定會顯示

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  lcd.begin(16, 2);
  lcd.print("Temp Monitor");
}

void loop() {
  float temp = readTemperature();

  // 只有溫度變化超過 0.5 度，才重新印出（避免小數點跳動造成畫面一直閃）
  if (abs(temp - lastDisplayedTemp) > 0.5) {
    lcd.setCursor(0, 1);
    lcd.print("Temp: ");
    lcd.print(temp);
    lcd.print("C   ");
    lastDisplayedTemp = temp;
  }
  delay(200);
}
```
**新函式提醒**：`abs()` 會回傳一個數字的絕對值（去掉正負號），這裡用來計算「新舊溫度差距的大小」，不管溫度是上升還是下降，只要變化幅度夠大就更新畫面。

#### 範例 4-2：把更新判斷邏輯封裝成通用函式
```cpp
// ============================================
// 範例 4-2：設計一個通用的「數值變化才更新」判斷函式
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int pinTemp = A2;
float lastDisplayedTemp = -999;

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

// 回傳 true 代表「數值變化夠大，應該更新畫面」
bool shouldUpdate(float newValue, float lastValue, float threshold) {
  return abs(newValue - lastValue) > threshold;
}

void setup() {
  lcd.begin(16, 2);
  lcd.print("Temp Monitor");
}

void loop() {
  float temp = readTemperature();

  if (shouldUpdate(temp, lastDisplayedTemp, 0.5)) {
    lcd.setCursor(0, 1);
    lcd.print("Temp: ");
    lcd.print(temp);
    lcd.print("C   ");
    lastDisplayedTemp = temp;
  }
  delay(200);
}
```

---

### 關卡 5：自訂字元與跑馬燈文字

**機關故事**：純文字的戰情看板還不夠吸睛，密室老闆希望能有一些自訂圖示與動態文字效果。

#### 範例 5-1：自訂字元 — 電池符號
```cpp
// ============================================
// 範例 5-1：LCD 自訂字元 - 畫出簡易電池圖示
// LCD 每個字元由 5x8 個像素組成，用 0/1 的 byte 陣列定義形狀
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

// 定義一個簡易電池符號（5 欄 x 8 列的點陣圖）
byte batteryIcon[8] = {
  B01110,
  B11011,
  B10001,
  B10001,
  B10001,
  B10001,
  B10001,
  B11111
};

void setup() {
  lcd.begin(16, 2);
  lcd.createChar(0, batteryIcon);   // 將圖案註冊為編號 0 的自訂字元
  lcd.print("Battery: ");
  lcd.write(byte(0));               // 顯示編號 0 的自訂字元
}

void loop() {
}
```

#### 範例 5-2：文字跑馬燈效果
```cpp
// ============================================
// 範例 5-2：警報發生時，文字左右滑動提醒（跑馬燈效果）
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
String message = "  WARNING: HIGH TEMPERATURE DETECTED  ";

void setup() {
  lcd.begin(16, 2);
}

void loop() {
  for (int i = 0; i < message.length() - 16; i++) {
    lcd.setCursor(0, 0);
    lcd.print(message.substring(i, i + 16));   // 每次只截取畫面能容納的 16 個字元
    delay(300);
  }
}
```
**新語法提醒**：`String` 是 Arduino 內建的文字字串型別，`.length()` 取得字串長度，`.substring(起, 迄)` 截取字串中的一段。這裡的迴圈邊界特別寫成 `message.length() - 16`，確保截取範圍不會超出字串本身的長度。

#### 範例 5-3：正常/異常雙模式顯示
```cpp
// ============================================
// 範例 5-3：正常時顯示良品數，異常時畫面鎖定顯示錯誤代碼
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int pinTemp = A2;
int goodCount = 128;
float alarmThreshold = 35.0;

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void setup() {
  lcd.begin(16, 2);
}

void loop() {
  float temp = readTemperature();

  if (temp > alarmThreshold) {
    lcd.setCursor(0, 0);
    lcd.print("*** ERROR ***   ");
    lcd.setCursor(0, 1);
    lcd.print("E01: High Temp  ");
  } else {
    lcd.setCursor(0, 0);
    lcd.print("Status: Normal  ");
    lcd.setCursor(0, 1);
    lcd.print("Good Qty: ");
    lcd.print(goodCount);
    lcd.print("   ");
  }
  delay(300);
}
```

---

### 🏆 BOSS 關：產線戰情看板主控系統

**機關故事**：整合本週所有技巧，打造完整的產線戰情看板——多頁面切換、畫面更新優化、異常自動鎖定顯示。

```cpp
// ============================================
// BOSS 關：產線戰情看板主控系統 - 整合本週所有技巧
// 功能：多頁面切換 + 畫面優化 + 異常鎖定顯示
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int pinTrig = 3;
int pinEcho = 4;
int pinTemp = A2;
int pinButton = 2;

int lastButtonState = LOW;
int currentPage = 0;
float lastDisplayedValue = -999;
float alarmThreshold = 35.0;

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

float readTemperature() {
  int rawValue = analogRead(pinTemp);
  float voltage = rawValue * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

bool shouldUpdate(float newValue, float lastValue, float threshold) {
  return abs(newValue - lastValue) > threshold;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinButton, INPUT);
  lcd.begin(16, 2);
  lcd.print("System Booting..");
  delay(1000);
  lcd.clear();
}

void loop() {
  float temp = readTemperature();

  // --- 異常優先權：溫度異常時，畫面鎖定顯示錯誤代碼，忽略頁面切換 ---
  if (temp > alarmThreshold) {
    lcd.setCursor(0, 0);
    lcd.print("*** ERROR ***   ");
    lcd.setCursor(0, 1);
    lcd.print("E01: High Temp  ");
    delay(200);
    return;
  }

  // --- 正常狀態：允許頁面切換 ---
  int currentState = digitalRead(pinButton);
  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    currentPage = (currentPage + 1) % 2;
    lcd.clear();
    lastDisplayedValue = -999;   // 換頁後重置，確保新頁面第一次一定會顯示
  }
  lastButtonState = currentState;

  if (currentPage == 0) {
    lcd.setCursor(0, 0);
    lcd.print("Page 1: Temp    ");
    if (shouldUpdate(temp, lastDisplayedValue, 0.5)) {
      lcd.setCursor(0, 1);
      lcd.print(temp);
      lcd.print(" C          ");
      lastDisplayedValue = temp;
    }
  } else {
    float distance = readDistance();
    lcd.setCursor(0, 0);
    lcd.print("Page 2: Dist    ");
    if (shouldUpdate(distance, lastDisplayedValue, 1.0)) {
      lcd.setCursor(0, 1);
      lcd.print(distance);
      lcd.print(" cm         ");
      lastDisplayedValue = distance;
    }
  }
  delay(150);
}
```
**教學重點**：這個 BOSS 關再次體現了第 8 週學過的「優先權排序」設計——溫度異常的判斷寫在最前面，並用 `return;` 讓後續的頁面切換邏輯完全不會被執行，這代表「異常顯示」永遠享有比「一般頁面瀏覽」更高的優先權，玩家在異常狀態下無法透過按鈕逃避看到錯誤訊息。

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| LCD 螢幕完全空白或只顯示黑色方塊 | 對比度未調整，或接線腳位對應錯誤 | 確認 V0 腳位有接可變電阻調整對比度；核對 RS/E/D4-D7 接線順序 |
| 文字顯示位置跑掉或重疊 | `setCursor()` 的欄或列參數寫錯 | 記得第一個參數是欄（0~15），第二個是列（0 或 1） |
| 數字後面殘留舊資料的字元 | 新資料位數比舊資料少，卻沒有多印空白蓋掉 | 在 `lcd.print()` 內容後方多加幾個空白字元 |
| 換頁後畫面出現前一頁的殘留文字 | 忘記在換頁時呼叫 `lcd.clear()` | 檢查按鈕切換頁面的程式碼是否有 `lcd.clear()` |
| 跑馬燈文字顯示異常或程式當機 | `substring()` 的範圍超出字串實際長度 | 檢查迴圈邊界條件，確保不會截取超出字串長度的範圍 |
| 自訂字元顯示出奇怪的亂碼而非預期圖案 | `byte` 陣列的二進位數字寫錯，或 `createChar()` 編號與 `write()` 編號不一致 | 逐行核對點陣圖數字，並確認兩處使用相同的編號 |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改範例 1-1，把顯示文字改成你自己的學號與姓名。
2. 修改範例 2-1 的計數器，改成每 2 秒才遞增一次（而非每秒）。
3. 修改範例 3-1，交換溫度與距離顯示的行數（溫度改顯示在第二行）。
4. 修改範例 4-1 的更新門檻，從 0.5 改成 1.0，觀察畫面更新頻率的變化。
5. 修改範例 5-3 的異常門檻，從 35.0 改成 30.0。

### 🟡 中等題
6. 設計一個「開機動畫」：LCD 開機時先顯示 "Loading" 並搭配三個逐漸增加的點（Loading. → Loading.. → Loading...），營造載入中的效果。
7. 修改範例 3-2 的頁面系統，新增第三頁顯示「系統運轉時間」（複習範例 2-1 的計數器技巧）。
8. 設計一個「數值範圍提示」：溫度顯示時，額外根據數值大小在旁邊顯示簡易文字標籤（如 "LOW"、"OK"、"HIGH"）。
9. 修改範例 5-1，設計第二個自訂圖示（例如溫度計符號），並在戰情看板中適當位置顯示。
10. 結合第 2 週所學，設計一個「長按重置」功能：按住頁面切換按鈕超過某個累計次數，強制回到頁面 0 並清空所有計數器。

### 🔴 挑戰題
11. **多重異常代碼系統**：修改 BOSS 關程式碼，設計至少 3 種不同的異常情況（溫度過高、距離過近、兩者同時發生），並顯示對應的不同錯誤代碼（如 E01、E02、E03）。
12. **選單層級設計**：設計一個具備「主選單 → 子選單」概念的兩層頁面架構（例如主選單顯示「1.溫度 2.距離 3.設定」，選擇後才進入對應的詳細畫面），需要額外的狀態變數記錄目前在哪一層選單。
13. **資料記錄與回顧**：設計一個功能，記錄「最近 3 次」的溫度讀值到一個陣列中，並能透過某個操作切換顯示「目前溫度」與「歷史記錄」畫面。
14. **跑馬燈速度控制**：修改範例 5-2 的跑馬燈效果，改用第 4 週學過的旋鈕（需自行擴充接線）即時控制文字滾動速度。
15. **完全自訂戰情看板**：不參考任何範例，設計一個「你認為最實用的產線資訊看板」，至少要用到：畫面更新優化（避免閃爍）、至少兩頁可切換頁面、一種異常鎖定顯示邏輯。

---

## 🎨 額外主題卡（給想多做的人）

> **主題卡 A：電梯樓層顯示器**
> 設計一個模擬電梯樓層顯示的效果：用一個變數代表目前樓層，搭配按鈕模擬「呼叫電梯」，樓層數字動態變化並在到達時顯示 "到站" 提示。

> **主題卡 B：倒數計時看板**
> 結合第 1 週學過的倒數計時概念，在 LCD 上顯示大型的倒數數字，搭配蜂鳴器在時間快到時發出提示音。

> **主題卡 C：多國語言問候看板**
> 設計一個效果：每隔幾秒鐘切換顯示不同語言的問候語（如中文、英文、日文的 "歡迎光臨"），模擬機場或展場的多語言顯示看板。

> **主題卡 D：氣象站顯示器**
> 結合溫度與光線感測器，設計一個模擬氣象站的顯示畫面，用自訂字元設計簡易的天氣圖示（晴天、陰天符號），依感測數值切換顯示對應圖示。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| LCD（Liquid Crystal Display） | 液晶顯示器 | 能顯示文字或圖案的顯示元件 |
| `<LiquidCrystal.h>` | LCD 函式庫 | Arduino 內建、專門控制 LCD 的工具包 |
| `lcd.begin()` | LCD 初始化 | 設定 LCD 的欄數與列數，開始使用前必須呼叫 |
| `lcd.setCursor()` | 游標定位 | 移動游標到指定的欄與列 |
| `lcd.clear()` | 清空螢幕 | 清除 LCD 上所有已顯示的內容 |
| `abs()` | 絕對值 | 回傳一個數字去掉正負號後的數值 |
| `String` | 字串型別 | 儲存一段文字的資料型別，可用 `.length()`、`.substring()` 等方法操作 |
| 自訂字元（Custom Character） | 自訂字元 | 用 5x8 點陣自行設計的 LCD 顯示圖案 |
| 畫面殘影 | 畫面殘影 | 新資料位數較短，未被完全覆蓋的舊資料殘留痕跡 |

---

## 反思與延伸討論

1. 這學期前十週，機關與玩家的溝通方式主要是燈光與聲音，這週第一次能顯示文字。請討論：文字顯示相較於燈光和聲音，在傳遞「精確資訊」上有什麼優勢？但在傳遞「緊急程度」上，是否反而不如燈光和聲音直覺？
2. 範例 4-1 學到的「只在數值變化夠大時才更新畫面」，是使用者介面設計中很重要的細節。請討論：如果一個系統的畫面每 0.1 秒就強制重新整理一次，即使數字沒有實質變化，會對使用者的觀看體驗造成什麼影響？
3. BOSS 關程式碼再次使用了「優先權排序 + `return`」的設計手法（複習自第 6、7、8 週）。請討論：到目前為止，這學期已經在幾個不同的專題中使用過這個手法？這是否代表某些程式設計模式，一旦學會就能不斷應用在完全不同的情境中？

---

## 本週檢核清單

- [ ] 能正確使用 `<LiquidCrystal.h>` 函式庫顯示文字
- [ ] 能用 `setCursor()` 精準控制文字顯示位置
- [ ] 能顯示動態變化的變數數值，並處理畫面殘影問題
- [ ] 能實作「數值變化才更新」的畫面優化邏輯
- [ ] 能設計自訂字元與跑馬燈文字效果
- [ ] 能整合多個感測器資料到同一塊看板，並實作頁面切換
- [ ] 完成至少 8 題基礎/中等練習題

---

## 下週預告

第 12 週是 Arduino 基礎元件學習階段的最後一週——你將學會 4x4 矩陣鍵盤，打造一套完整的電子密碼鎖機關，並進行 12 週以來的期末統整驗收。完成這一週後，我們就要正式進入第四階段——補齊工業自動化最後幾個關鍵元件（位移暫存器、繼電器等），並在最後兩週把整學期所學收斂成一套完整的終極整合大專題！

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 10 週](./week-10.md) | [下一週：第 12 週 — 期末統整與電子密碼鎖 ➡](./week-12.md)
