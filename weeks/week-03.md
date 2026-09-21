[⬅ 回目錄](../README.md) | [⬅ 上一週：第 2 週](./week-02.md) | [下一週：第 4 週 — 格鬥搖桿與旋鈕 ➡](./week-04.md)

---

# 第 3 週：迪斯可派對燈光秀
## 主題專案：一顆燈，無限種顏色

> 🎯 **本週定位**：前兩週我們每種顏色都要接一顆獨立的 LED，紅燈要紅色 LED、綠燈要綠色 LED。這一週你會學到一個大幅提升設計彈性的技巧——用 **PWM（脈衝寬度調變）** 讓一顆 LED 呈現「無段亮度」，再搭配 **RGB LED**（三色合一封裝），只用一顆元件就能混出上百萬種顏色。密室老闆這次要求把逃脫遊戲最後一關改裝成「迪斯可派對主題」，你要打造一整套絢麗的燈光秀。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [電路體檢站（3 分鐘快速複習）](#電路體檢站3-分鐘快速複習)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道機關關卡](#後端程式設計五道機關關卡)
  - [關卡 1：無段調光 — 認識 analogWrite() 與 PWM](#關卡-1無段調光--認識-analogwrite-與-pwm)
  - [關卡 2：RGB 三原色混色 — 自訂函式封裝](#關卡-2rgb-三原色混色--自訂函式封裝)
  - [關卡 3：呼吸燈與淡入淡出](#關卡-3呼吸燈與淡入淡出)
  - [關卡 4：彩虹漸變特效](#關卡-4彩虹漸變特效)
  - [關卡 5：按鈕切換派對模式](#關卡-5按鈕切換派對模式)
  - [🏆 BOSS 關：迪斯可派對主控台](#-boss-關迪斯可派對主控台)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎨 額外主題卡（給想多做的人）](#-額外主題卡給想多做的人)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～2 週）
- ✅ `digitalWrite`／`digitalRead` 數位訊號輸出入
- ✅ 陣列、迴圈、自訂函式
- ✅ `if-else`、`&&`、`||` 條件判斷
- ✅ 狀態變數與防彈跳

### 這週要新學的
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| PWM（脈衝寬度調變） | 用「快速開關的時間比例」模擬類比輸出效果 | 馬達調速、燈光調光、加熱器功率控制的核心技術 |
| `analogWrite()` | 輸出 0~255 的類比數值到支援 PWM 的腳位 | 遠比純粹 HIGH/LOW 更細緻的控制手段 |
| RGB LED | 三色（紅綠藍）封裝在同一顆元件內的 LED | 色彩指示系統的標準元件 |
| 色彩混合原理 | 三原色以不同強度組合出各種顏色 | 螢幕、燈光設計、視覺化儀表板的基礎知識 |
| 帶參數的自訂函式 | 函式可以接收外部傳入的數值進行運算 | 讓函式更通用、更容易重複使用 |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室老闆這次想擴大營運，決定把最後一間房間改裝成「迪斯可派對逃脫室」——玩家要在絢麗的燈光效果中，找出隱藏在燈光變化規律裡的線索才能過關。你被指派負責設計整套燈光秀系統。
>
> 這週結束時，你會做出：
> 1. 一顆能無段調光的呼吸燈（暖身）
> 2. 一顆能混出任意顏色的 RGB LED 控制函式
> 3. 一套平滑的呼吸與淡入淡出特效
> 4. 一套連續變化的彩虹漸變特效
> 5. 一個能用按鈕切換不同派對模式的系統
> 6. 一個整合以上所有技巧的「迪斯可派對主控台」

---

## 電路體檢站（3 分鐘快速複習）

**體檢題 1**：上週我們用 `digitalWrite(pin, HIGH)` 讓 LED「全亮」，用 `LOW` 讓它「全滅」。如果我想要 LED「半亮」呢？直接寫 `digitalWrite(pin, 0.5)` 可以嗎？
> 答案：不行。`digitalWrite()` 只認識 HIGH（5V）與 LOW（0V）兩種狀態，沒有中間值。要做出「半亮」的效果，需要今天要學的 `analogWrite()`。

**體檢題 2**：回想上學期學過的「並聯電路」，各支路電壓相同、電流各自獨立分配。RGB LED 內部其實就是三顆迷你 LED（紅、綠、藍）並聯封裝在同一顆外殼裡（各自有獨立的接腳），所以你依然可以用學過的「多顆 LED 各自獨立控制」的概念來理解它，只是這次三顆燈physically 長在同一顆透明外殼裡。

**體檢題 3**：還記得為什麼 LED 一定要接電阻嗎？RGB LED 因為裡面其實是三顆獨立的 LED，所以需要幾顆電阻？
> 答案：三顆，R、G、B 三個接腳都要各自串一顆電阻，道理跟單顆 LED 完全一樣。

---

## 模擬設計專案配置

### 元件清單
| 元件 | 數量 | 說明 |
|---|---|---|
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1 | 電路佈局 |
| RGB LED（共陰極, Common Cathode） | 1 | 全彩指示燈 |
| 220Ω 電阻 | 3 | RGB 三色各一顆 |
| Pushbutton | 1 | 模式切換鈕（沿用第 2 週接法） |
| 10kΩ 電阻 | 1 | 按鈕下拉電阻 |

### 接線步驟

**Part A：RGB LED**
1. 拖曳一顆 **RGB LED（共陰極）** 到麵包板。RGB LED 有 4 隻腳：最長的那隻是共同負極（GND），其餘三隻分別對應 R（紅）、G（綠）、B（藍）。
2. 三個顏色腳分別**各自串接一顆 220Ω 電阻**，再接到 Arduino 支援 PWM 的腳位（腳位旁標示 `~` 符號）：**R → Pin 9、G → Pin 10、B → Pin 11**。
3. 最長的共同負極腳，接到麵包板負極軌，再接回 Arduino GND。

**Part B：模式切換按鈕**
1. 沿用第 2 週的接法：按鈕一側接 10kΩ 下拉電阻到 GND，同一側接訊號線到 **Pin 2**，另一側接 5V。

> ⚠️ **PWM 腳位小提醒**：Arduino Uno 只有特定幾隻腳位支援 `analogWrite()`（在 Tinkercad 元件圖上通常會標示 `~`），常見的有 3, 5, 6, 9, 10, 11。本週務必把 RGB LED 接在這些腳位上，接錯腳位會導致 `analogWrite()` 完全沒有調光效果。

### 腳位對照表（本週共用）
| 元件 | 腳位 |
|---|---|
| RGB LED - 紅（R） | ~9 |
| RGB LED - 綠（G） | ~10 |
| RGB LED - 藍（B） | ~11 |
| 模式切換按鈕 | Pin 2 |

> 💡 **配置檢查清單**
> - [ ] RGB LED 的三個顏色腳都各自接了電阻，且接在 `~9`、`~10`、`~11` 這三個支援 PWM 的腳位
> - [ ] RGB LED 最長腳（共同負極）接到 GND
> - [ ] 按鈕已接妥下拉電阻與訊號線

---

## 後端程式設計：五道機關關卡

### 關卡 1：無段調光 — 認識 analogWrite() 與 PWM

**機關故事**：派對燈光不能只有「全亮／全滅」兩種狀態，你要先確認能做出「無段亮度」的調光效果。

#### 範例 1-1：測試 analogWrite() 基礎用法
```cpp
// ============================================
// 範例 1-1：認識 analogWrite() - 控制 LED 亮度
// PWM 原理：讓燈以極快速度閃爍，透過「亮的時間比例」製造出視覺上的亮度感
// ============================================

int pinLed = 9;   // 必須接在支援 PWM（標示 ~）的腳位

void setup() {
  pinMode(pinLed, OUTPUT);
}

void loop() {
  analogWrite(pinLed, 128);   // 128 大約是最大值 255 的一半亮度
}
```

#### 範例 1-2：五段亮度測試
```cpp
// ============================================
// 範例 1-2：測試不同亮度數值的視覺效果
// ============================================

int pinLed = 9;

void setup() {
  pinMode(pinLed, OUTPUT);
}

void loop() {
  analogWrite(pinLed, 0);    delay(700);   // 全暗
  analogWrite(pinLed, 64);   delay(700);   // 微亮
  analogWrite(pinLed, 128);  delay(700);   // 中亮
  analogWrite(pinLed, 192);  delay(700);   // 偏亮
  analogWrite(pinLed, 255);  delay(700);   // 全亮
}
```

#### 範例 1-3：三顆顏色腳各自測試（先不混色）
```cpp
// ============================================
// 範例 1-3：分別測試 RGB LED 三個顏色腳位
// ============================================

int pinR = 9;
int pinG = 10;
int pinB = 11;

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  analogWrite(pinR, 255); analogWrite(pinG, 0);   analogWrite(pinB, 0);   delay(800); // 純紅
  analogWrite(pinR, 0);   analogWrite(pinG, 255); analogWrite(pinB, 0);   delay(800); // 純綠
  analogWrite(pinR, 0);   analogWrite(pinG, 0);   analogWrite(pinB, 255); delay(800); // 純藍
}
```

---

### 關卡 2：RGB 三原色混色 — 自訂函式封裝

**機關故事**：派對燈光需要頻繁切換各種顏色，如果每次都要寫三行 `analogWrite`，程式碼會變得又臭又長。這是本週最重要的技巧——把重複邏輯封裝成好用的函式。

#### 範例 2-1：自訂函式 setColor(r, g, b)
```cpp
// ============================================
// 範例 2-1：把三行 analogWrite 封裝成一個函式
// 好處：之後只要呼叫 setColor(255, 0, 0) 就能設紅色，程式碼更好讀
// ============================================

int pinR = 9;
int pinG = 10;
int pinB = 11;

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

// 自訂函式：輸入紅、綠、藍三個數值，直接設定 RGB LED 顏色
void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void loop() {
  setColor(255, 0, 0);     delay(800);  // 紅
  setColor(0, 255, 0);     delay(800);  // 綠
  setColor(0, 0, 255);     delay(800);  // 藍
  setColor(255, 255, 0);   delay(800);  // 黃（紅+綠）
  setColor(0, 255, 255);   delay(800);  // 青（綠+藍）
  setColor(255, 0, 255);   delay(800);  // 洋紅（紅+藍）
  setColor(255, 255, 255); delay(800);  // 白（三色全開）
}
```
**混色原理小整理**：
| 顏色 | R | G | B |
|---|---|---|---|
| 紅 | 255 | 0 | 0 |
| 綠 | 0 | 255 | 0 |
| 藍 | 0 | 0 | 255 |
| 黃 | 255 | 255 | 0 |
| 青 | 0 | 255 | 255 |
| 洋紅 | 255 | 0 | 255 |
| 白 | 255 | 255 | 255 |
| 橘 | 255 | 100 | 0 |
| 紫 | 128 | 0 | 128 |

#### 範例 2-2：色彩陣列 + 迴圈輪播
```cpp
// ============================================
// 範例 2-2：把多組顏色存成二維陣列，用迴圈依序播放
// ============================================

int pinR = 9;
int pinG = 10;
int pinB = 11;

// 二維陣列：每一列是一組 [R, G, B] 顏色資料
int colorList[][3] = {
  {255, 0, 0},     // 紅
  {255, 165, 0},   // 橘
  {255, 255, 0},   // 黃
  {0, 255, 0},     // 綠
  {0, 0, 255},      // 藍
  {75, 0, 130},     // 靛
  {148, 0, 211}     // 紫
};
int numColors = 7;

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
  for (int i = 0; i < numColors; i++) {
    setColor(colorList[i][0], colorList[i][1], colorList[i][2]);
    delay(500);
  }
}
```
**新語法提醒**：`colorList[][3]` 是「二維陣列」，可以想像成一張表格，每一列（row）代表一組顏色，每一列固定有 3 個欄位（R、G、B）。`colorList[i][0]` 就是取出第 `i` 組顏色的第 0 個欄位（也就是紅色數值）。

---

### 關卡 3：呼吸燈與淡入淡出

**機關故事**：派對燈光不應該是「瞬間切換」，而要有「平滑漸變」的高級感，這一關我們來實作經典的呼吸燈效果。

#### 範例 3-1：單色呼吸燈
```cpp
// ============================================
// 範例 3-1：呼吸燈效果 - 平滑漸亮漸暗
// ============================================

int pinLed = 9;

void setup() {
  pinMode(pinLed, OUTPUT);
}

void loop() {
  // 漸亮：從 0 到 255
  for (int brightness = 0; brightness <= 255; brightness++) {
    analogWrite(pinLed, brightness);
    delay(5);
  }
  // 漸暗：從 255 到 0
  for (int brightness = 255; brightness >= 0; brightness--) {
    analogWrite(pinLed, brightness);
    delay(5);
  }
}
```

#### 範例 3-2：RGB 版呼吸燈（自訂顏色的呼吸效果）
```cpp
// ============================================
// 範例 3-2：把呼吸燈效果套用在任意指定顏色上
// ============================================

int pinR = 9;
int pinG = 10;
int pinB = 11;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

// 帶參數的呼吸函式：可以指定「目標顏色」，函式內部自動處理漸亮漸暗
void breathe(int targetR, int targetG, int targetB) {
  for (int b = 0; b <= 255; b++) {
    setColor(targetR * b / 255, targetG * b / 255, targetB * b / 255);
    delay(4);
  }
  for (int b = 255; b >= 0; b--) {
    setColor(targetR * b / 255, targetG * b / 255, targetB * b / 255);
    delay(4);
  }
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  breathe(255, 0, 100);   // 玫瑰紅呼吸
}
```
**教學重點**：`targetR * b / 255` 這個算式，是把「目標顏色的最大值」乘上「目前呼吸進度的比例（0~255 之間）」，等於是把顏色「按比例縮放」。這是一個很實用的技巧：先決定最終顏色，再讓整體亮度依比例漸變。

#### 範例 3-3：兩色交叉淡入淡出（Crossfade）
```cpp
// ============================================
// 範例 3-3：兩種顏色之間平滑交叉淡入淡出
// ============================================

int pinR = 9;
int pinG = 10;
int pinB = 11;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void crossfade(int r1, int g1, int b1, int r2, int g2, int b2) {
  for (int i = 0; i <= 255; i++) {
    int r = r1 + (r2 - r1) * i / 255;   // 從顏色1 線性過渡到 顏色2
    int g = g1 + (g2 - g1) * i / 255;
    int b = b1 + (b2 - b1) * i / 255;
    setColor(r, g, b);
    delay(6);
  }
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  crossfade(255, 0, 0, 0, 0, 255);   // 紅 → 藍
  crossfade(0, 0, 255, 0, 255, 0);   // 藍 → 綠
  crossfade(0, 255, 0, 255, 0, 0);   // 綠 → 紅
}
```

---

### 關卡 4：彩虹漸變特效

**機關故事**：派對的高潮時刻需要一段連續不間斷的彩虹漸變特效，這一關我們把前面學到的 `crossfade` 概念串接成完整的彩虹循環。

#### 範例 4-1：六色彩虹循環
```cpp
// ============================================
// 範例 4-1：紅橙黃綠藍紫 六色彩虹循環
// ============================================

int pinR = 9;
int pinG = 10;
int pinB = 11;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void crossfade(int r1, int g1, int b1, int r2, int g2, int b2) {
  for (int i = 0; i <= 255; i += 3) {   // 每次跳 3，讓速度快一點
    int r = r1 + (r2 - r1) * i / 255;
    int g = g1 + (g2 - g1) * i / 255;
    int b = b1 + (b2 - b1) * i / 255;
    setColor(r, g, b);
    delay(4);
  }
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  crossfade(255, 0, 0,   255, 165, 0);  // 紅 → 橙
  crossfade(255, 165, 0, 255, 255, 0);  // 橙 → 黃
  crossfade(255, 255, 0, 0, 255, 0);    // 黃 → 綠
  crossfade(0, 255, 0,   0, 0, 255);    // 綠 → 藍
  crossfade(0, 0, 255,   75, 0, 130);   // 藍 → 靛
  crossfade(75, 0, 130,  148, 0, 211);  // 靛 → 紫
  crossfade(148, 0, 211, 255, 0, 0);    // 紫 → 回到紅，完成一圈
}
```

#### 範例 4-2：彩虹迴圈函式化（用陣列重構範例 4-1）
```cpp
// ============================================
// 範例 4-2：把彩虹色票存成陣列，用迴圈自動產生 crossfade 序列
// 展現「資料與邏輯分離」的程式設計思維
// ============================================

int pinR = 9;
int pinG = 10;
int pinB = 11;

int rainbow[][3] = {
  {255, 0, 0}, {255, 165, 0}, {255, 255, 0},
  {0, 255, 0}, {0, 0, 255}, {75, 0, 130}, {148, 0, 211}
};
int numColors = 7;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void crossfade(int r1, int g1, int b1, int r2, int g2, int b2) {
  for (int i = 0; i <= 255; i += 3) {
    int r = r1 + (r2 - r1) * i / 255;
    int g = g1 + (g2 - g1) * i / 255;
    int b = b1 + (b2 - b1) * i / 255;
    setColor(r, g, b);
    delay(4);
  }
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  // 用迴圈依序在色票陣列中的每一組顏色之間做 crossfade
  for (int i = 0; i < numColors; i++) {
    int nextIndex = (i + 1) % numColors;   // 最後一色的下一個是回到第一色，形成循環
    crossfade(
      rainbow[i][0], rainbow[i][1], rainbow[i][2],
      rainbow[nextIndex][0], rainbow[nextIndex][1], rainbow[nextIndex][2]
    );
  }
}
```
**教學重點**：範例 4-1 跟 4-2 做出來的效果幾乎一樣，但範例 4-2 只要修改 `rainbow[][3]` 這個陣列的內容，就能改變整組色票，完全不用去改動迴圈邏輯本身。這正是「資料驅動設計」的精神——**把會變動的資料與固定不變的邏輯分開**。

#### 範例 4-3：警示閃爍模式（緊急狀態切換）
```cpp
// ============================================
// 範例 4-3：派對燈光切換到緊急疏散模式 - 紅藍交替閃爍
// ============================================

int pinR = 9;
int pinG = 10;
int pinB = 11;

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
  setColor(255, 0, 0);  delay(150);   // 紅
  setColor(0, 0, 255);  delay(150);   // 藍
}
```

---

### 關卡 5：按鈕切換派對模式

**機關故事**：DJ 需要能用一顆按鈕，隨時切換不同的燈光模式，而不用重新上傳程式。

#### 範例 5-1：按鈕切換三種固定顏色
```cpp
// ============================================
// 範例 5-1：按一次切換到下一種顏色
// ============================================

int pinR = 9, pinG = 10, pinB = 11;
int pinButton = 2;
int lastButtonState = LOW;
int colorIndex = 0;

int colorList[][3] = {
  {255, 0, 0},
  {0, 255, 0},
  {0, 0, 255}
};
int numColors = 3;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(pinButton, INPUT);
}

void loop() {
  int currentState = digitalRead(pinButton);
  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    colorIndex = (colorIndex + 1) % numColors;
  }
  lastButtonState = currentState;

  setColor(colorList[colorIndex][0], colorList[colorIndex][1], colorList[colorIndex][2]);
}
```

#### 範例 5-2：按鈕切換「效果模式」而非單純顏色
```cpp
// ============================================
// 範例 5-2：按鈕切換不同的燈光「效果」（靜態/呼吸/彩虹）
// ============================================

int pinR = 9, pinG = 10, pinB = 11;
int pinButton = 2;
int lastButtonState = LOW;
int modeIndex = 0;   // 0=靜態白光, 1=呼吸紅光, 2=彩虹循環

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(pinButton, INPUT);
}

void checkButton() {
  int currentState = digitalRead(pinButton);
  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    modeIndex = (modeIndex + 1) % 3;
  }
  lastButtonState = currentState;
}

void loop() {
  checkButton();

  if (modeIndex == 0) {
    setColor(255, 255, 255);   // 靜態白光
  } else if (modeIndex == 1) {
    // 呼吸紅光（簡化版，每次迴圈只走一小步，維持按鈕反應速度）
    for (int b = 0; b <= 255; b += 5) {
      checkButton();
      if (modeIndex != 1) return;   // 如果按鈕切換了模式，立刻跳出，避免卡住
      setColor(b, 0, 0);
      delay(10);
    }
  } else if (modeIndex == 2) {
    setColor(random(0, 255), random(0, 255), random(0, 255));   // 隨機色彩快閃
    delay(200);
  }
}
```
**教學重點**：這裡示範了一個重要觀念——在呼吸模式的迴圈內部，特地加了 `checkButton()` 與 `if (modeIndex != 1) return;`，確保「即使正在執行慢速的呼吸動畫，按鈕仍然保持一定程度的反應性」。這是第 14 週會深入探討的「多工處理」議題的暖身。

---

### 🏆 BOSS 關：迪斯可派對主控台

**機關故事**：整合本週所有技巧，打造完整的派對燈光主控台——具備多種模式、按鈕切換、並且在特定模式下會自動閃爍警示。

```cpp
// ============================================
// BOSS 關：迪斯可派對主控台 - 整合本週所有技巧
// 模式 0：靜態彩色展示（依序輪播色票）
// 模式 1：呼吸燈效果
// 模式 2：彩虹連續漸變
// 模式 3：緊急警示閃爍（紅藍交替）
// ============================================

int pinR = 9, pinG = 10, pinB = 11;
int pinButton = 2;
int lastButtonState = LOW;
int modeIndex = 0;
int numModes = 4;

int rainbow[][3] = {
  {255, 0, 0}, {255, 165, 0}, {255, 255, 0},
  {0, 255, 0}, {0, 0, 255}, {75, 0, 130}, {148, 0, 211}
};
int numColors = 7;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void checkButton() {
  int currentState = digitalRead(pinButton);
  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    modeIndex = (modeIndex + 1) % numModes;
    Serial.print("切換到模式：");
    Serial.println(modeIndex);
  }
  lastButtonState = currentState;
}

void crossfadeStep(int r1, int g1, int b1, int r2, int g2, int b2, int steps) {
  for (int i = 0; i <= 255; i += steps) {
    checkButton();
    if (modeIndex != 2) return;   // 若使用者切走彩虹模式，立刻中止動畫
    int r = r1 + (r2 - r1) * i / 255;
    int g = g1 + (g2 - g1) * i / 255;
    int b = b1 + (b2 - b1) * i / 255;
    setColor(r, g, b);
    delay(4);
  }
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(pinButton, INPUT);
  Serial.begin(9600);
  Serial.println("=== 派對主控台啟動！目前模式：0（靜態展示）===");
}

void loop() {
  checkButton();

  if (modeIndex == 0) {
    // --- 模式 0：靜態色票輪播 ---
    for (int i = 0; i < numColors; i++) {
      checkButton();
      if (modeIndex != 0) return;
      setColor(rainbow[i][0], rainbow[i][1], rainbow[i][2]);
      delay(500);
    }
  } else if (modeIndex == 1) {
    // --- 模式 1：呼吸燈（紫色系） ---
    for (int b = 0; b <= 255; b += 5) {
      checkButton();
      if (modeIndex != 1) return;
      setColor(148 * b / 255, 0, 211 * b / 255);
      delay(8);
    }
    for (int b = 255; b >= 0; b -= 5) {
      checkButton();
      if (modeIndex != 1) return;
      setColor(148 * b / 255, 0, 211 * b / 255);
      delay(8);
    }
  } else if (modeIndex == 2) {
    // --- 模式 2：彩虹連續漸變 ---
    for (int i = 0; i < numColors; i++) {
      int nextIndex = (i + 1) % numColors;
      crossfadeStep(
        rainbow[i][0], rainbow[i][1], rainbow[i][2],
        rainbow[nextIndex][0], rainbow[nextIndex][1], rainbow[nextIndex][2],
        3
      );
      if (modeIndex != 2) return;
    }
  } else if (modeIndex == 3) {
    // --- 模式 3：緊急警示閃爍 ---
    setColor(255, 0, 0);
    delay(150);
    checkButton();
    if (modeIndex != 3) return;
    setColor(0, 0, 255);
    delay(150);
  }
}
```
**教學重點**：這是這學期第一個「四模式切換」的完整系統。特別注意每個模式的動畫迴圈內都插入了 `checkButton()` 與 `if (modeIndex != X) return;`，這種寫法讓使用者「隨時按按鈕都能立刻切換模式」，而不會被卡在某個正在跑的動畫裡動彈不得——這是這學期第一次真正處理「反應性（Responsiveness）」議題，第 14 週會用更優雅的 `millis()` 方式重新解決同一個問題。

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| `analogWrite()` 完全沒有調光效果，只有全亮全滅 | 腳位沒有接在支援 PWM 的腳位上 | 確認接線在 `~3`、`~5`、`~6`、`~9`、`~10`、`~11` 這些標示 `~` 的腳位 |
| RGB LED 顏色跟預期完全相反（例如想要紅色卻出現青色） | 買到或選到「共陽極」而非「共陰極」的 RGB LED | 確認元件類型為共陰極；若真的是共陽極，需要把所有數值改成 `255 - 數值` |
| 呼吸燈效果卡卡的、不平滑 | `delay()` 時間太長，或迴圈的遞增/遞減間隔太大 | 縮短 `delay()` 時間（如從 20 改成 5），或把 `brightness++` 改成更小的步進 |
| 彩虹漸變顏色跳動很明顯不平滑 | `crossfade` 的步進值（如 `i += 3`）設太大 | 調小步進值，讓過渡更細緻（但也會讓速度變慢，需要抓平衡） |
| 按鈕在動畫執行中完全沒反應 | 動畫迴圈內沒有即時檢查按鈕狀態 | 參考 BOSS 關寫法，在迴圈內插入 `checkButton()` 並判斷是否需要提前 `return` |
| 二維陣列取值出現錯誤 | 索引寫反（例如 `colorList[0][3]` 而非 `colorList[3][0]`） | 記得第一個中括號是「第幾組」，第二個中括號是「這組裡的第幾個欄位」 |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改範例 2-1，自行設計並加入至少 2 種新的自訂混色（例如粉紅色、天藍色），並算出對應的 R、G、B 數值。
2. 修改範例 3-1，把呼吸燈的速度加快一倍（提示：調整 `delay()` 數值）。
3. 修改範例 1-2，改成從暗到亮再到暗的「連續」漸變，而不是五段跳躍式切換。
4. 修改範例 4-3 的警示閃爍，把紅藍改成綠黃交替。
5. 修改範例 5-1，把三色改成四色循環（新增一種你喜歡的顏色）。

### 🟡 中等題
6. 設計一個「心情燈」：分別用三個不同顏色代表「開心（黃）」、「平靜（綠）」、「生氣（紅）」，用按鈕依序切換，並在 Serial Monitor 印出對應的心情文字。
7. 修改範例 3-3 的 crossfade，改成三種顏色依序循環（紅→綠→藍→回到紅），而不只是兩種顏色。
8. 設計一個「電量指示燈」：用顏色代表電量高低（滿電=綠、中等=黃、低電量=紅、警示=紅色閃爍），並用一個模擬電量的變數（可以自行遞減）觸發顏色變化。
9. 修改範例 4-2 的彩虹陣列，設計一組「只有冷色系」（藍、青、紫、靛）的漸變循環。
10. 結合第 2 週所學，設計一個「雙按鈕」控制系統：一顆按鈕切換顏色（如範例 5-1），另一顆按鈕切換亮度（例如暗、中、亮三段，使用 `analogWrite` 搭配倍率縮放）。

### 🔴 挑戰題
11. **HSV 色彩轉換挑戰（進階數學）**：目前我們都是直接指定 R、G、B 數值來決定顏色。試著研究看看「色相（Hue）」的概念——只用一個 0~255 的數值，就能透過數學公式自動算出對應的 R、G、B（這是專業燈光控制系統常用的手法），並嘗試寫出一個 `setHue(int hue)` 函式（提示：這是進階挑戰，可以先上網查詢 HSV 轉 RGB 的簡化公式）。
12. **音樂節奏燈光模擬**：設計一個效果，模擬「隨音樂節奏跳動」的燈光——用 `random()` 產生不規則的顏色與跳動間隔，模擬夜店雷射燈光效果。
13. **多模式記憶功能**：修改 BOSS 關程式碼，新增第 5 種模式「上次關閉前的最後顏色」，需要用變數記住使用者切換到模式 3（警示模式）之前，原本停留在哪個顏色。
14. **漸層填色文字效果（純燈光模擬）**：假設有 3 顆 RGB LED（可自行擴充電路），設計一個效果讓三顆燈呈現「同一個漸層色系」但各自進度不同，模擬跑馬燈文字般的漸層流動感（此題為開放式挑戰，需要自行擴充硬體與程式邏輯）。
15. **完全自訂派對效果**：不參考任何範例，設計一個「你認為最酷的迪斯可燈光效果」，至少要用到：1 個帶參數的自訂函式、1 個二維陣列、1 個可以隨時被按鈕中斷的動畫迴圈。

---

## 🎨 額外主題卡（給想多做的人）

> **主題卡 A：情緒溫度計**
> 設計一個燈光效果，模擬情緒從「平靜（藍）」逐漸累積到「爆發（紅）」的連續漸變過程，可以用一個累加變數控制目前的「情緒值」，情緒值越高顏色越偏紅。

> **主題卡 B：生日蛋糕蠟燭模擬**
> 用呼吸燈效果模擬燭光搖曳的感覺（暖黃色系，亮度做出不規則的小幅度隨機閃爍，而非固定規律的呼吸），可以在原本的呼吸燈迴圈中加入 `random()` 製造出的微幅擾動。

> **主題卡 C：紅綠燈進化版**
> 把第 1 週做過的三色獨立 LED 紅綠燈，改用一顆 RGB LED 重新實作，並額外加上「黃燈時同時緩慢閃爍」的細節效果，讓警示感更明顯。

> **主題卡 D：調色盤練習器**
> 設計一個「猜色遊戲」的燈光提示系統：先隨機決定一個目標顏色並短暫顯示，接著熄滅，讓密室裡的另一個機關（可以只是想像中的機關）等待玩家操作某個裝置去「複製」出一樣的顏色。這一題主要練習「先展示、再考驗記憶」的機關設計思路。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| PWM（Pulse Width Modulation） | 脈衝寬度調變 | 用快速開關的時間比例，模擬出類比的中間值效果 |
| `analogWrite()` | 類比輸出 | 輸出 0~255 的數值到支援 PWM 的腳位 |
| RGB LED | 三色 LED | 內含紅、綠、藍三顆迷你 LED 的封裝元件 |
| 共陰極（Common Cathode） | 共陰極 | 三色 LED 共用同一根負極接腳的類型 |
| 混色（Color Mixing） | 混色 | 用不同強度的三原色組合出各種顏色 |
| 呼吸燈（Breathing Light） | 呼吸燈 | 亮度平滑漸亮漸暗、周而復始的燈光效果 |
| 淡入淡出 / 交叉淡化（Crossfade） | 交叉淡化 | 兩種狀態之間平滑過渡的效果 |
| 二維陣列 | 二維陣列 | 陣列中的每個元素本身又是一個陣列，像一張表格 |
| 帶參數的函式 | 帶參數的函式 | 呼叫時可以傳入不同數值，讓函式行為隨之改變 |
| `return` | 提前返回 | 立刻結束目前函式（或 `loop()`），不執行後面剩餘的程式碼 |

---

## 反思與延伸討論

1. 用一顆 RGB LED（3 個腳位）就能做出上百萬種顏色，相較於第 1 週用 8 顆各自獨立的 LED，你認為在「硬體成本」與「程式複雜度」之間，各自帶來了什麼樣的取捨？
2. BOSS 關程式碼中，每個動畫迴圈都要不斷呼叫 `checkButton()` 才能維持反應性，這種寫法雖然可行，但如果模式越來越多、動畫越來越複雜，程式碼會變得怎樣？你能想像有沒有更優雅的解法嗎？（這個問題會在第 14 週得到完整解答）
3. 範例 4-2 把顏色資料獨立存成陣列，讓「資料」與「邏輯」分開。請討論：這種設計方式，如果之後老闆要求「顏色改成 10 種而不是 7 種」，你只需要修改哪裡？如果程式碼沒有這樣設計，會需要改動多少地方？

---

## 本週檢核清單

- [ ] 理解 PWM 的基本原理，並能正確使用 `analogWrite()`
- [ ] 知道哪些腳位支援 PWM，並能識別 Tinkercad 上的 `~` 標示
- [ ] 能寫出帶參數的自訂函式（如 `setColor(r, g, b)`）
- [ ] 能運用二維陣列管理多組相關資料
- [ ] 能實作呼吸燈與交叉淡化效果
- [ ] 能設計按鈕切換多種模式的系統，並維持動畫過程中的按鈕反應性
- [ ] 完成至少 8 題基礎/中等練習題

---

## 下週預告

第 4 週我們要幫密室加入「類比輸入」——可變電阻（旋鈕）。玩家將能透過轉動旋鈕，即時控制機關的速度、亮度、甚至角色移動的參數，這也是這學期第一次遇到「連續數值」的輸入訊號，會學到非常實用的 `map()` 數值轉換函式。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 2 週](./week-02.md) | [下一週：第 4 週 — 格鬥搖桿與旋鈕 ➡](./week-04.md)
