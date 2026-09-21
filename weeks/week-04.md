[⬅ 回目錄](../README.md) | [⬅ 上一週：第 3 週](./week-03.md) | [下一週：第 5 週 — 密室音效與雷射網 ➡](./week-05.md)

---

# 第 4 週：格鬥遊戲搖桿與加速旋鈕
## 主題專案：連續數值的世界

> 🎯 **本週定位**：前三週我們處理的訊號都是「非黑即白」——按鈕不是按下就是放開、LED 不是亮就是暗（就算有 PWM，我們給的目標值也都是程式裡寫死的固定數字）。這一週我們要正式進入「連續類比輸入」的世界：**可變電阻（旋鈕）**。密室老闆這次想加入「格鬥遊戲主題房」，玩家要用旋鈕模擬搖桿轉動的力道，這也是這學期第一次遇到「感測器讀到的原始數值」跟「我們想要的輸出數值」範圍完全不一樣的情況，你會學到整學期最實用的函式之一：`map()`。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [電路體檢站（3 分鐘快速複習）](#電路體檢站3-分鐘快速複習)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道機關關卡](#後端程式設計五道機關關卡)
  - [關卡 1：讀取旋鈕 — 認識 analogRead() 與 ADC](#關卡-1讀取旋鈕--認識-analogread-與-adc)
  - [關卡 2：數值轉換 — map() 函式全面解析](#關卡-2數值轉換--map-函式全面解析)
  - [關卡 3：力道連動 — 旋鈕控制亮度與速度](#關卡-3力道連動--旋鈕控制亮度與速度)
  - [關卡 4：分級判斷 — 多區間門檻邏輯](#關卡-4分級判斷--多區間門檻邏輯)
  - [關卡 5：綜合連動 — 旋鈕 + 按鈕雙輸入整合](#關卡-5綜合連動--旋鈕--按鈕雙輸入整合)
  - [🏆 BOSS 關：格鬥能量條主控台](#-boss-關格鬥能量條主控台)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎨 額外主題卡（給想多做的人）](#-額外主題卡給想多做的人)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～3 週）
- ✅ 數位輸出入：`digitalWrite`、`digitalRead`
- ✅ PWM 類比輸出：`analogWrite`
- ✅ 陣列、迴圈、帶參數的自訂函式
- ✅ 狀態變數、防彈跳、多模式切換

### 這週要新學的
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| ADC（類比數位轉換） | 把連續變化的電壓，轉換成 Arduino 看得懂的數字 | 幾乎所有感測器（溫度、光線、壓力、位移）都靠 ADC 轉換 |
| `analogRead()` | 讀取類比腳位（A0~A5）的數值，範圍 0~1023 | 讀取旋鈕、感測器的標準方式 |
| `map()` | 把一個範圍的數值，等比例轉換成另一個範圍 | 感測資料轉換成設備能理解的控制訊號，工業上天天用到 |
| `constrain()` | 把數值限制在指定範圍內，避免超出邊界 | 防止異常數值造成設備錯誤動作 |
| Serial Plotter | 把數值畫成即時波形圖 | 觀察連續訊號變化趨勢的除錯利器 |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室老闆這週決定新增一間「格鬥擂台房」——玩家要轉動一顆巨大的「能量旋鈕」蓄力，蓄力值會即時反映在燈光亮度與閃爍速度上，蓄力滿了才能觸發「必殺技機關」（開啟暗門）。你被指派設計這套「連續數值驅動」的燈光系統。
>
> 這週結束時，你會做出：
> 1. 一個能讀取並顯示旋鈕原始數值的感應測試機關（暖身）
> 2. 一套完整掌握 `map()` 數值轉換的練習
> 3. 一個用旋鈕即時控制 LED 亮度與跑馬燈速度的機關
> 4. 一個依旋鈕數值分級顯示不同警示燈的機關
> 5. 一個旋鈕 + 按鈕協同運作的機關
> 6. 一個整合以上所有技巧的「格鬥能量條主控台」

---

## 電路體檢站（3 分鐘快速複習）

**體檢題 1**：上週學的 `analogWrite()` 輸出範圍是 0~255。這週要學的 `analogRead()` 讀取範圍卻是 0~1023，為什麼兩者不一樣？
> 答案：這跟 Arduino 硬體內部電路設計有關——`analogWrite()` 走的是 PWM（8 位元，2⁸=256 種可能，也就是 0~255），而 `analogRead()` 走的是 ADC（10 位元，2¹⁰=1024 種可能，也就是 0~1023）。這是這週會反覆用到 `map()` 的原因之一：兩邊的數值範圍常常對不上，需要轉換。

**體檢題 2**：回想第 1 週體檢過的「串聯電路，電壓會分掉」的觀念。可變電阻（旋鈕）內部其實就是一個會依照轉動角度改變「分壓比例」的元件——這正是善用了串聯分壓的原理，只是這次分壓的比例可以連續調整，而不是固定不變。

**體檢題 3**：LED 需要限流電阻保護，那可變電阻本身需要額外加保護電阻嗎？
> 答案：不需要。可變電阻本身內部就有電阻值，不會有過大電流燒毀元件的疑慮，這跟 LED 的情況不同。

---

## 模擬設計專案配置

### 元件清單
| 元件 | 數量 | 說明 |
|---|---|---|
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1 | 電路佈局 |
| Potentiometer（可變電阻） | 1 | 能量旋鈕 |
| RGB LED（共陰極） | 1 | 能量條指示燈（沿用第 3 週接法） |
| 220Ω 電阻 | 3 | RGB 三色限流電阻 |
| LED | 5 | 能量條分段指示燈（另外新增） |
| 220Ω 電阻 | 5 | 上述 5 顆 LED 各自限流 |
| Pushbutton | 1 | 必殺技觸發鈕 |
| 10kΩ 電阻 | 1 | 按鈕下拉電阻 |

### 接線步驟

**Part A：可變電阻**
1. 拖曳一顆 **Potentiometer** 到工作區。
2. 左右兩腳分別接 **5V** 與 **GND**（方向不影響邏輯功能）。
3. 中間腳（訊號輸出腳）接到 Arduino 的 **A0**。

**Part B：RGB LED（沿用第 3 週）**
1. R → ~9、G → ~10、B → ~11，各自串接 220Ω 電阻，共同負極接 GND。

**Part C：5 顆能量條分段 LED**
1. 5 顆 LED 各自串接 220Ω 電阻，正極分別接到 **Pin 2, 3, 4, 5, 6**。
2. 負極全部接到負極軌，接回 GND。

**Part D：必殺技按鈕**
1. 沿用第 2 週接法：下拉電阻 + 訊號線接 **Pin 7**。

### 腳位對照表（本週共用）
| 元件 | 腳位 |
|---|---|
| 可變電阻 | A0 |
| RGB LED - R / G / B | ~9 / ~10 / ~11 |
| 能量條 LED 1~5 | Pin 2, 3, 4, 5, 6 |
| 必殺技按鈕 | Pin 7 |

> 💡 **配置檢查清單**
> - [ ] 可變電阻中間腳接到 A0，兩側分別接 5V 與 GND
> - [ ] RGB LED 三色腳接在 PWM 腳位（~9、~10、~11）
> - [ ] 5 顆能量條 LED 依序接在 Pin 2~6
> - [ ] 必殺技按鈕已接妥下拉電阻

---

## 後端程式設計：五道機關關卡

### 關卡 1：讀取旋鈕 — 認識 analogRead() 與 ADC

**機關故事**：在做出任何華麗效果前，你得先確認「旋鈕轉動時，Arduino 真的能讀到變化的數值」。

#### 範例 1-1：讀取旋鈕原始數值
```cpp
// ============================================
// 範例 1-1：讀取可變電阻的類比數值（0~1023）
// ============================================

int pinPot = A0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int value = analogRead(pinPot);
  Serial.println(value);
  delay(100);
}
```
**操作提醒**：開啟 Serial Monitor，緩慢轉動旋鈕，觀察數值從接近 0 一路變化到接近 1023。

#### 範例 1-2：用 Serial Plotter 觀察連續波形
```cpp
// ============================================
// 範例 1-2：程式碼相同，改用 Serial Plotter 視覺化
// 操作：Tinkercad 右側切換 Serial Monitor → Serial Plotter
// ============================================

int pinPot = A0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int value = analogRead(pinPot);
  Serial.println(value);
  delay(30);
}
```
**教學重點**：緩慢轉動旋鈕，觀察 Plotter 畫出平滑曲線；快速左右扭動，觀察數值劇烈跳動的樣子。這能建立你對「連續類比訊號」最直覺的視覺印象，往後遇到任何類比感測器，都可以先用這招快速確認訊號品質。

#### 範例 1-3：簡易門檻判斷（暖身用，先不用 map）
```cpp
// ============================================
// 範例 1-3：旋鈕轉超過一半（>512）才點亮指示燈
// ============================================

int pinPot = A0;
int pinLed = 2;

void setup() {
  pinMode(pinLed, OUTPUT);
}

void loop() {
  int value = analogRead(pinPot);

  if (value > 512) {
    digitalWrite(pinLed, HIGH);
  } else {
    digitalWrite(pinLed, LOW);
  }
}
```

---

### 關卡 2：數值轉換 — map() 函式全面解析

**機關故事**：旋鈕讀到的是 0~1023 這種不直覺的數字，但 LED 亮度只吃 0~255，能量條有 5 段、警示分級只有 3 種。工業上經常需要把感測器的原始數值「轉換」成設備能理解的單位，這就是 `map()` 函式存在的意義，這一整關我們要徹底搞懂它。

#### 範例 2-1：map() 基礎語法拆解
```cpp
// ============================================
// 範例 2-1：認識 map() 的四個參數
// 語法：map(原始值, 原始最小, 原始最大, 目標最小, 目標最大)
// ============================================

int pinPot = A0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int rawValue = analogRead(pinPot);                // 原始範圍 0~1023
  int mapped = map(rawValue, 0, 1023, 0, 255);      // 轉換為 0~255

  Serial.print("原始值: ");
  Serial.print(rawValue);
  Serial.print("   轉換後: ");
  Serial.println(mapped);

  delay(100);
}
```

#### 範例 2-2：map() 反向映射（讓大變小、小變大）
```cpp
// ============================================
// 範例 2-2：反向映射 - 旋鈕轉越多，數值反而越小
// ============================================

int pinPot = A0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int rawValue = analogRead(pinPot);
  // 注意目標範圍寫成「反向」：0~1023 對應到 255~0（順序顛倒）
  int inverted = map(rawValue, 0, 1023, 255, 0);

  Serial.print("旋鈕值: ");
  Serial.print(rawValue);
  Serial.print("   反向映射: ");
  Serial.println(inverted);

  delay(100);
}
```
**思考問題**：什麼情境下你會想要「反向映射」？（提示：想像一個「音量旋鈕」控制的是「靜音遮蔽程度」而非「音量本身」）

#### 範例 2-3：map() 搭配 constrain() 防止超出範圍
```cpp
// ============================================
// 範例 2-3：constrain() 確保數值不會超出安全範圍
// 情境：即使感測器出現異常雜訊，數值也不會失控
// ============================================

int pinPot = A0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int rawValue = analogRead(pinPot);
  int angle = map(rawValue, 0, 1023, 0, 180);

  // 即使 map() 算出來的結果理論上不會超出 0~180，
  // 但在真實硬體上偶爾因雜訊导致異常值，加上 constrain() 更保險
  angle = constrain(angle, 0, 180);

  Serial.println(angle);
  delay(100);
}
```

#### 範例 2-4：多重映射練習（一個原始值同時轉換成多種用途）
```cpp
// ============================================
// 範例 2-4：同一個旋鈕數值，同時轉換成三種不同用途的數字
// ============================================

int pinPot = A0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  int rawValue = analogRead(pinPot);

  int brightness = map(rawValue, 0, 1023, 0, 255);   // 給 LED 亮度用
  int delayTime  = map(rawValue, 0, 1023, 500, 20);  // 給跑馬燈速度用（反向：轉越多越快）
  int litCount   = map(rawValue, 0, 1023, 0, 5);     // 給能量條格數用

  Serial.print("亮度: "); Serial.print(brightness);
  Serial.print("  延遲: "); Serial.print(delayTime);
  Serial.print("  格數: "); Serial.println(litCount);

  delay(150);
}
```
**教學重點**：這一段程式碼是本週最重要的示範——**同一個感測器讀值，可以同時餵給好幾個不同用途的 `map()`，各自轉換成不同範圍的輸出**，這正是 BOSS 關會大量用到的核心手法。

---

### 關卡 3：力道連動 — 旋鈕控制亮度與速度

**機關故事**：格鬥擂台的能量條需要即時反映玩家的蓄力旋鈕，轉得越多、燈光越亮、跑馬燈越快。

#### 範例 3-1：旋鈕即時控制 LED 亮度
```cpp
// ============================================
// 範例 3-1：旋鈕轉多少，LED 就亮多少
// ============================================

int pinPot = A0;
int pinLed = 9;   // 注意：調光需要 PWM 腳位

void setup() {
  pinMode(pinLed, OUTPUT);
}

void loop() {
  int rawValue = analogRead(pinPot);
  int brightness = map(rawValue, 0, 1023, 0, 255);
  analogWrite(pinLed, brightness);
}
```

#### 範例 3-2：旋鈕控制跑馬燈速度
```cpp
// ============================================
// 範例 3-2：能量條跑馬燈 - 旋鈕轉越多，跑越快
// ============================================

int pinPot = A0;
int ledPins[] = {2, 3, 4, 5, 6};
int numLeds = 5;

void setup() {
  for (int i = 0; i < numLeds; i++) {
    pinMode(ledPins[i], OUTPUT);
  }
}

void loop() {
  int rawValue = analogRead(pinPot);
  // 轉速旋鈕：轉越多，delay 越短，跑馬燈移動越快（範圍 300~20 毫秒）
  int speedDelay = map(rawValue, 0, 1023, 300, 20);

  for (int i = 0; i < numLeds; i++) {
    digitalWrite(ledPins[i], HIGH);
    delay(speedDelay);
    digitalWrite(ledPins[i], LOW);
  }
}
```

#### 範例 3-3：旋鈕控制 RGB 顏色連續變化
```cpp
// ============================================
// 範例 3-3：旋鈕連續控制顏色 - 從冷靜藍到爆發紅
// ============================================

int pinPot = A0;
int pinR = 9, pinG = 10, pinB = 11;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  int rawValue = analogRead(pinPot);
  int red  = map(rawValue, 0, 1023, 0, 255);    // 蓄力越多，紅色越強
  int blue = map(rawValue, 0, 1023, 255, 0);    // 蓄力越多，藍色越弱
  setColor(red, 0, blue);
}
```

---

### 關卡 4：分級判斷 — 多區間門檻邏輯

**機關故事**：能量條除了連續變化的顏色，還需要清楚的「等級指示」——低、中、高、爆發，方便玩家一眼判斷目前蓄力狀態。

#### 範例 4-1：三段能量等級指示
```cpp
// ============================================
// 範例 4-1：三段能量等級（低、中、高）分別點亮不同 LED
// ============================================

int pinPot = A0;
int pinLow = 2;
int pinMid = 4;
int pinHigh = 6;

void setup() {
  pinMode(pinLow, OUTPUT);
  pinMode(pinMid, OUTPUT);
  pinMode(pinHigh, OUTPUT);
}

void loop() {
  int value = analogRead(pinPot);

  digitalWrite(pinLow, LOW);
  digitalWrite(pinMid, LOW);
  digitalWrite(pinHigh, LOW);

  if (value < 341) {
    digitalWrite(pinLow, HIGH);
  } else if (value < 682) {
    digitalWrite(pinMid, HIGH);
  } else {
    digitalWrite(pinHigh, HIGH);
  }
}
```

#### 範例 4-2：五段能量條（善用 map 直接算出「該點亮幾顆」）
```cpp
// ============================================
// 範例 4-2：能量條長條圖效果 - 依旋鈕數值點亮對應數量的 LED
// ============================================

int pinPot = A0;
int ledPins[] = {2, 3, 4, 5, 6};
int numLeds = 5;

void setup() {
  for (int i = 0; i < numLeds; i++) {
    pinMode(ledPins[i], OUTPUT);
  }
}

void loop() {
  int rawValue = analogRead(pinPot);
  int litCount = map(rawValue, 0, 1023, 0, numLeds);   // 轉換為 0~5 顆燈的數量

  for (int i = 0; i < numLeds; i++) {
    if (i < litCount) {
      digitalWrite(ledPins[i], HIGH);
    } else {
      digitalWrite(ledPins[i], LOW);
    }
  }
}
```
**教學重點**：這是「長條圖（Progress Bar）」最經典的實作手法——不用一堆 `if-else` 分別判斷每一顆燈該不該亮，而是先算出「該亮幾顆」，再用迴圈搭配比較 `i < litCount` 一次判斷完畢。這種寫法遠比寫 5 個獨立的 `if` 判斷式優雅得多。

#### 範例 4-3：滿能量觸發爆發模式（門檻 + 特效）
```cpp
// ============================================
// 範例 4-3：能量條滿格時，自動切換成爆發閃爍特效
// ============================================

int pinPot = A0;
int ledPins[] = {2, 3, 4, 5, 6};
int numLeds = 5;

void setup() {
  for (int i = 0; i < numLeds; i++) {
    pinMode(ledPins[i], OUTPUT);
  }
}

void loop() {
  int rawValue = analogRead(pinPot);
  int litCount = map(rawValue, 0, 1023, 0, numLeds);

  if (litCount >= numLeds) {
    // 能量滿格：全部快速閃爍，代表準備爆發
    for (int i = 0; i < numLeds; i++) {
      digitalWrite(ledPins[i], HIGH);
    }
    delay(100);
    for (int i = 0; i < numLeds; i++) {
      digitalWrite(ledPins[i], LOW);
    }
    delay(100);
  } else {
    // 尚未滿格：正常顯示長條圖
    for (int i = 0; i < numLeds; i++) {
      digitalWrite(ledPins[i], i < litCount ? HIGH : LOW);
    }
  }
}
```

---

### 關卡 5：綜合連動 — 旋鈕 + 按鈕雙輸入整合

**機關故事**：格鬥擂台的最終機關需要「蓄力滿格 + 按下必殺技鈕」兩個條件同時滿足，才會真正觸發開門機關。

#### 範例 5-1：滿能量後按鈕才生效
```cpp
// ============================================
// 範例 5-1：能量條滿格後，必殺技按鈕才會生效
// ============================================

int pinPot = A0;
int pinButton = 7;
int pinLed = 9;
int ledPins[] = {2, 3, 4, 5, 6};
int numLeds = 5;

void setup() {
  for (int i = 0; i < numLeds; i++) {
    pinMode(ledPins[i], OUTPUT);
  }
  pinMode(pinButton, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  int rawValue = analogRead(pinPot);
  int litCount = map(rawValue, 0, 1023, 0, numLeds);

  for (int i = 0; i < numLeds; i++) {
    digitalWrite(ledPins[i], i < litCount ? HIGH : LOW);
  }

  bool isFull = litCount >= numLeds;
  bool buttonPressed = digitalRead(pinButton) == HIGH;

  if (isFull && buttonPressed) {
    digitalWrite(pinLed, HIGH);   // 必殺技發動！
  } else {
    digitalWrite(pinLed, LOW);
  }
}
```

#### 範例 5-2：旋鈕決定亮度、按鈕決定總開關
```cpp
// ============================================
// 範例 5-2：按鈕控制總開關（Toggle），旋鈕決定開機後的亮度
// ============================================

int pinPot = A0;
int pinButton = 7;
int pinLed = 9;
int lastButtonState = LOW;
bool systemOn = false;

void setup() {
  pinMode(pinButton, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  int currentState = digitalRead(pinButton);
  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    systemOn = !systemOn;
  }
  lastButtonState = currentState;

  if (systemOn) {
    int rawValue = analogRead(pinPot);
    int brightness = map(rawValue, 0, 1023, 0, 255);
    analogWrite(pinLed, brightness);
  } else {
    analogWrite(pinLed, 0);   // 關機時強制熄滅，旋鈕此時無效
  }
}
```

---

### 🏆 BOSS 關：格鬥能量條主控台

**機關故事**：整合本週所有技巧，打造完整的格鬥擂台能量條系統——旋鈕即時控制能量條與顏色連動、滿格自動爆發閃爍、按鈕觸發必殺技結算。

```cpp
// ============================================
// BOSS 關：格鬥能量條主控台 - 整合本週所有技巧
// 功能：旋鈕控制能量條格數 + RGB顏色隨蓄力變化 + 滿格爆發特效 + 按鈕結算必殺技
// ============================================

int pinPot = A0;
int pinButton = 7;
int pinR = 9, pinG = 10, pinB = 11;
int ledPins[] = {2, 3, 4, 5, 6};
int numLeds = 5;

int lastButtonState = LOW;
int specialMoveCount = 0;   // 累計使出必殺技的次數

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  for (int i = 0; i < numLeds; i++) {
    pinMode(ledPins[i], OUTPUT);
  }
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(pinButton, INPUT);
  Serial.begin(9600);
  Serial.println("=== 格鬥能量條系統啟動！轉動旋鈕蓄力 ===");
}

void loop() {
  int rawValue = analogRead(pinPot);

  // --- 同一個原始值，轉換成多種用途（呼應關卡 2 範例 2-4 的手法） ---
  int litCount  = map(rawValue, 0, 1023, 0, numLeds);
  int redAmount = map(rawValue, 0, 1023, 0, 255);
  int blueAmount = map(rawValue, 0, 1023, 255, 0);

  bool isFull = litCount >= numLeds;

  // --- 能量條顯示 ---
  for (int i = 0; i < numLeds; i++) {
    digitalWrite(ledPins[i], i < litCount ? HIGH : LOW);
  }

  // --- RGB 顏色連動：滿格時額外加上爆發閃爍效果 ---
  if (isFull) {
    setColor(255, 255, 0);   // 滿格先顯示金黃色
    delay(80);
    setColor(255, 0, 0);     // 快速切紅
    delay(80);
  } else {
    setColor(redAmount, 0, blueAmount);
  }

  // --- 必殺技按鈕判定 ---
  int currentButtonState = digitalRead(pinButton);
  if (currentButtonState == HIGH && lastButtonState == LOW) {
    delay(50);
    if (isFull) {
      specialMoveCount++;
      Serial.print("*** 必殺技發動！累計次數：");
      Serial.println(specialMoveCount);
    } else {
      Serial.println("能量不足，必殺技無法發動！");
    }
  }
  lastButtonState = currentButtonState;
}
```
**教學重點**：這段程式碼把本週學到的「一個感測值、多種映射用途」發揮到極致——同一個 `rawValue` 同時決定了能量條格數、紅色分量、藍色分量三種輸出，這正是善用 `map()` 靈活性的最佳範例。

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| `analogRead()` 數值一直是 0 或一直是 1023 不會變 | 可變電阻接線錯誤，中間腳沒接到 A0，或兩側電源接反 | 檢查中間腳確實接 A0，兩側分別接 5V 與 GND |
| `map()` 算出來的數字怪怪的、超出預期範圍 | 四個參數順序搞混，或原始範圍寫錯 | 對照語法 `map(原始值, 原始最小, 原始最大, 目標最小, 目標最大)` 逐一核對 |
| 想要「反向」控制卻沒有效果 | 目標範圍的最小最大值沒有對調 | 確認目標範圍寫成「大, 小」而非「小, 大」，如 `map(v, 0, 1023, 255, 0)` |
| 長條圖效果（範例 4-2）某幾顆燈邏輯錯亂 | `i < litCount` 條件寫反，或陣列索引超出範圍 | 確認迴圈變數 `i` 是從 0 開始，且比較符號方向正確 |
| 數值在滿格臨界值附近跳動不穩定（忽亮忽滅） | 旋鈕本身或模擬環境的訊號雜訊 | 可以把判斷條件稍微放寬容忍範圍，或不用刻意追求臨界值的絕對精準 |
| BOSS 關程式碼中 `isFull` 邏輯永遠是 false | `litCount >= numLeds` 寫成 `>` 導致差一點永遠無法觸發 | 檢查比較運算子，注意 `map()` 理論上最大值就是 `numLeds`，用 `>=` 才保險 |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改範例 3-1，把亮度範圍從 0~255 改成 50~255（讓 LED 永遠不會完全熄滅，保持一點基本亮度）。
2. 修改範例 4-1 的三段門檻，改成四段（新增一個「極高」等級）。
3. 修改範例 2-2 的反向映射，改成把 0~1023 轉換到 0~100（模擬百分比顯示）。
4. 修改範例 3-2 的跑馬燈速度範圍，讓最快與最慢的速度差異更明顯（例如從 500~20 改成 800~10）。
5. 在範例 1-1 的基礎上，加上文字說明，當數值小於 300 時額外印出 "電量偏低" 的提示。

### 🟡 中等題
6. 設計一個「溫度模擬器」：用旋鈕模擬溫度感測器（0~1023 映射到 -10°C~50°C），並依溫度顯示不同顏色的 RGB LED（冷=藍、常溫=綠、高溫=紅）。
7. 修改範例 4-2 的長條圖，改成「從中間往兩側」點亮的效果，而不是從左到右依序點亮（提示：可以參考第 1 週學過的雙指標寫法）。
8. 設計一個「音量旋鈕模擬器」：用 Serial Monitor 印出目前音量百分比（0~100%），並且當音量超過 80% 時，額外印出 "警告：音量過大！" 的提示。
9. 結合第 2 週所學的按鈕 Toggle 邏輯，設計一個系統：按鈕切換「顯示模式」（模式 A 顯示能量條格數、模式 B 顯示 RGB 顏色漸變），旋鈕在兩種模式下都持續發揮作用，只是呈現方式不同。
10. 修改範例 4-3，把「滿格閃爍」的顏色從固定的黃紅交替，改成使用 `random()` 隨機顏色閃爍。

### 🔴 挑戰題
11. **雙旋鈕系統（延伸電路）**：如果加入第二顆可變電阻（接 A1），設計一個「兩個數值同時控制不同輸出」的系統，例如旋鈕 A 控制能量條格數、旋鈕 B 控制整體閃爍速度。
12. **精準度陷阱挑戰**：`map()` 函式的計算過程其實是整數運算，可能因為捨去小數而產生「跳動不均勻」的現象（例如 0~1023 對應到 0~5，每一格對應的原始數值範圍並不是剛好平均分配的）。請設計一個 Serial Monitor 測試程式，印出旋鈕從 0 轉到底時，`litCount` 分別在哪些「原始數值」發生跳動，並討論這個現象。
13. **平滑化讀值（降躁挑戰）**：類比訊號有時會有雜訊，導致數值忽大忽小。設計一個程式，連續讀取旋鈕數值 5 次並計算平均值，再拿平均值去做後續的 `map()` 轉換，並比較平滑化前後的差異（可以用 Serial Plotter 觀察）。
14. **能量隨時間自動流失**：修改 BOSS 關程式碼，讓能量條除了受旋鈕控制，還會隨著時間緩慢自動下降（模擬「蓄力會消退」的遊戲機制），需要引入一個獨立的變數來追蹤「目前實際能量值」而不能完全依賴即時讀取的旋鈕數值。
15. **完全自訂連動系統**：不參考任何範例，自己設計一個「旋鈕 + 至少兩種輸出裝置（LED / RGB / 跑馬燈）」的連動系統，至少要用到 2 次不同用途的 `map()` 轉換。

---

## 🎨 額外主題卡（給想多做的人）

> **主題卡 A：賽車油門模擬器**
> 用旋鈕模擬賽車油門，數值越大代表車速越快，用跑馬燈速度呈現車速感，並在超過某個門檻時顯示「超速警告」燈號。

> **主題卡 B：調酒師刻度計**
> 設計一個「調配比例顯示器」：旋鈕代表某種原料的比例（0~100%），用 RGB LED 顏色深淺呈現濃度高低，並用能量條顯示目前刻度位置。

> **主題卡 C：麥克風收音等級表（音量表模擬）**
> 用旋鈕模擬麥克風收音音量，設計一個經典的「音量等級表」效果——多顆 LED 由下往上依序點亮，超過某個等級時最上方燈號變成紅色代表「爆音警告」。

> **主題卡 D：體感遊戲蓄力条**
> 設計一個「長按蓄力」的機關：玩家轉動旋鈕到最大值後，必須額外「按住按鈕不放」累積 3 秒（可以用累加計數器模擬），才會真正觸發爆發特效，否則放開按鈕蓄力歸零重來。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| ADC（Analog-to-Digital Converter） | 類比數位轉換器 | 把連續變化的電壓，轉換成電腦看得懂的數字 |
| `analogRead()` | 類比讀取 | 讀取 A0~A5 類比腳位的數值，範圍 0~1023 |
| `map()` | 數值映射 | 把一個範圍的數值等比例轉換到另一個範圍 |
| `constrain()` | 數值限制 | 把數值限制在指定的最小最大範圍內 |
| Serial Plotter | 序列埠繪圖器 | 把數值即時畫成波形圖的除錯工具 |
| 長條圖 / Progress Bar | 長條圖 | 用點亮的元件數量呈現進度或數值大小的視覺化手法 |
| 反向映射 | 反向映射 | 讓原始值越大、目標值反而越小的映射方式 |
| 資料平滑化 | 資料平滑化 | 透過多次讀取取平均等手法，降低感測雜訊的影響 |

---

## 反思與延伸討論

1. `map()` 函式讓我們可以輕鬆把「感測器讀值」轉換成「設備能理解的控制訊號」。請討論：在真實工業場景中，你能想到哪些感測器（想想未來幾週會學到的溫度、距離、光線感測器）的原始數值，最後也需要經過類似的轉換，才能變成有意義的控制動作？
2. BOSS 關程式碼中，同一個 `rawValue` 被拿去做了三種不同的 `map()` 轉換。這種「一份原始資料、多種用途轉換」的設計方式，跟工廠中「一顆感測器的資料同時餵給好幾個不同的監控系統」的概念是否相似？你能舉出類似的真實案例嗎？
3. 練習題 12 提到 `map()` 是整數運算，可能造成「跳動不均勻」的現象。請討論：如果某個應用場景要求極高的精準度（例如精密儀器的角度控制），純粹依賴 `map()` 整數運算是否足夠？你會如何改善？（提示：可以想想是否需要用 `float` 浮點數運算）

---

## 本週檢核清單

- [ ] 理解 ADC 的基本概念，並能正確使用 `analogRead()`
- [ ] 能熟練使用 `map()` 進行數值範圍轉換，包含正向與反向映射
- [ ] 理解 `constrain()` 的用途並能正確使用
- [ ] 能用旋鈕即時控制 LED 亮度、跑馬燈速度、RGB 顏色
- [ ] 能實作多區間門檻判斷與長條圖效果
- [ ] 能整合類比輸入（旋鈕）與數位輸入（按鈕）於同一個系統
- [ ] 完成至少 8 題基礎/中等練習題

---

## 🔬 補充實驗室：map() 背後的數學

如果你想更深入理解 `map()` 到底在做什麼數學運算，這裡拆解給你看。`map(value, fromLow, fromHigh, toLow, toHigh)` 內部的計算公式，其實等同於：

```cpp
// map() 函式的內部運算邏輯（僅供理解，不需要自己重寫）
long myMap(long value, long fromLow, long fromHigh, long toLow, long toHigh) {
  return (value - fromLow) * (toHigh - toLow) / (fromHigh - fromLow) + toLow;
}
```

拆解成三個步驟理解：
1. `(value - fromLow)`：先看看目前數值，「已經超過原始範圍最小值多少」。
2. `* (toHigh - toLow) / (fromHigh - fromLow)`：把這個「超過量」，依照兩個範圍的比例關係做縮放。
3. `+ toLow`：最後加上目標範圍的起始值，得到最終結果。

**手動練習**：假設 `rawValue = 512`，計算 `map(512, 0, 1023, 0, 255)` 的結果應該接近多少？試著不用 Arduino，自己用計算機算一次，再回到 Tinkercad 驗證答案是否一致。

**延伸驗算**：再試著手算 `map(256, 0, 1023, 0, 5)` 大約會得到多少（提示：對照範例 4-2 的能量條邏輯，這代表旋鈕轉到四分之一位置時，大約會點亮幾顆燈）？完成計算後，實際在 Tinkercad 上把旋鈕轉到大約四分之一位置驗證看看。

**討論題**：如果 `fromHigh` 與 `fromLow` 剛好相等（例如都寫成 0），公式裡會出現「除以 0」的情況，程式會發生什麼事？這提醒我們使用 `map()` 時，原始範圍的最小值與最大值務必不能相同，這是這學期第一次接觸「數學運算的邊界條件」概念，未來在其他計算場景也要隨時留意類似的陷阱。

---

## 🧮 加碼練習：手繪能量條草圖

在正式上機前，建議先用紙筆完成這個小練習，訓練「先設計、再實作」的工程習慣：

1. 畫出一條有 5 格的能量條示意圖。
2. 假設旋鈕目前讀值為 700（滿刻度 1023），請手動計算 `map(700, 0, 1023, 0, 5)` 大約等於多少，並在你畫的草圖上塗滿對應格數。
3. 再假設旋鈕讀值降到 200，重新計算並塗出對應格數，比較兩次的差異。
4. 想一想：如果將來能量條格數從 5 格擴充到 10 格，你的程式碼（範例 4-2）需要修改哪幾個地方？（提示：只需要修改陣列與一個變數）

完成這個練習後，你會發現「動手畫草圖」其實能幫助你在寫程式之前，先把邏輯在腦中或紙上跑過一遍，這是專業工程師開發任何系統前都會做的習慣——先設計，再實作，最後才是除錯。

---

## 下週預告

第 5 週我們要幫密室加入「聲音」與「光線偵測」——蜂鳴器與光敏電阻。密室老闆想打造一間「雷射防盜網」主題房，玩家必須在不觸發光線感應警報的前提下通過房間，你將學會如何讓 Arduino「發出聲音」，以及讓機關「感應到光線變化」。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 3 週](./week-03.md) | [下一週：第 5 週 — 密室音效與雷射網 ➡](./week-05.md)
