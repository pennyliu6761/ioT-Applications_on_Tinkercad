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
  - [BOSS 關：迪斯可派對主控台](#-boss-關迪斯可派對主控台)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖情境配置挑戰)
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

**體檢題 2**：回想上期學過的「並聯電路」，各支路電壓相同、電流各自獨立分配。RGB LED 內部其實就是三顆迷你 LED（紅、綠、藍）並聯封裝在同一顆外殼裡（各自有獨立的接腳），所以你依然可以用學過的「多顆 LED 各自獨立控制」的概念來理解它，只是這次三顆燈在同一顆透明外殼裡。

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
> - [ ] RGB LED 共同負極接到 GND
> - [ ] 按鈕已接妥下拉電阻與訊號線

<img width="953" height="413" alt="image" src="https://github.com/user-attachments/assets/0d8a6d58-4e17-4447-8947-a11ff3dbffb3" />

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

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：前面五道關卡都在同一套電路上換程式。這一區每一關都是一個**新的真實情境**，你要根據需求決定「要用哪些元件、用幾顆、接到哪幾隻腳」。程式大多已經準備好，你只需要填入腳位或沿用前面的範例。
>
> 📈 **難度安排**：從 1 顆燈開始，每一關多加一點東西，最後要自己設計一整套系統。
>
> 🧰 **可用元件**（只限第 1～3 週學過的）：單色 LED、RGB LED（共陰極）、220Ω 電阻、按鈕、10kΩ 電阻

---

### 📋 配線口訣卡（每一關都用得到）

| 口訣 | 說明 |
| --- | --- |
| ① 要漸變、要混色 → 接 `~` 腳 | 只有 3、5、6、9、10、11 能用 `analogWrite()` 調亮度 |
| ② 只要亮或滅 → 哪隻腳都可以 | 單純開關、閃爍的燈，接一般腳位就好，把 `~` 腳留給需要的燈 |
| ③ 按鈕不要佔 `~` 腳 | 按鈕只需要讀「有沒有按」，用一般腳位即可 |
| ④ 每條燈線都要一顆 220Ω | 單色 LED 一顆；RGB LED 三個顏色腳各一顆 |
| ⑤ 按鈕照第 2 週接法 | 一側接 5V，另一側接訊號線＋10kΩ 到 GND |
| ⑥ Pin 0、1 先不要用 | 它們負責和 Serial Monitor 通訊 |

---

### 🔧 共用工具：接線驗證器

接好線後，先用這支程式「點名」，確認每一條線都接對，再上傳正式程式。只需要把腳位填進陣列。

```cpp
// 接線驗證器：依序點亮每一隻腳位，並在 Serial Monitor 顯示目前測哪一隻
int pins[] = { 9, 10, 11 };   // ← 填入這一關所有「燈」的腳位
int numPins = 3;              // ← 腳位數量

void setup() {
  Serial.begin(9600);
  for (int i = 0; i < numPins; i++) {
    pinMode(pins[i], OUTPUT);
  }
}

void loop() {
  for (int i = 0; i < numPins; i++) {
    Serial.print("Testing Pin ");
    Serial.println(pins[i]);
    digitalWrite(pins[i], HIGH);
    delay(500);
    digitalWrite(pins[i], LOW);
  }
  Serial.println("---");
  delay(1000);
}
```

---

### 🟢 Lv.1 手搖飲店的營業招牌

**情境**：學校旁的手搖飲店想在門口放一個營業燈。營業中亮**綠燈**，打烊亮**紅燈**，每 3 秒自動切換一次做展示。

**本關新增**：1 顆 RGB LED

**你要想的問題**：

- 這個招牌只需要綠色和紅色。RGB LED 的三隻顏色腳，每一隻都需要接嗎？
- 如果藍色腳不接，你可以少用幾顆電阻、幾條線？

**程式**：

```cpp
int pinR = ?;   // ← 填腳位
int pinG = ?;   // ← 填腳位

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
}

void loop() {
  analogWrite(pinR, 0);   analogWrite(pinG, 255);  delay(3000);   // 營業中
  analogWrite(pinR, 255); analogWrite(pinG, 0);    delay(3000);   // 打烊
}
```

**加分小改**：老闆說「準備中」要亮黃燈。你需要加接線嗎？還是只要改程式？（提示：查一下關卡 2 的混色表）

---

### 🟢 Lv.2 取餐叫號燈

**情境**：飲料做好時，店員按一下櫃台按鈕，燈從**紅色（製作中）**變成**綠色（可取餐）**；客人取走後再按一下，燈熄滅（閒置）。

**本關新增**：1 顆按鈕

**你要想的問題**：

- 按鈕要接到哪一隻腳？依照口訣③，哪些腳位你**不應該**選？
- 把你的接法和旁邊同學比較，你們選的腳位一樣嗎？兩種都可以嗎？

**程式**：沿用**範例 5-1**，只需要改兩個地方：

1. 把 `pinButton` 改成你選的腳位
2. 把 `colorList` 改成三組顏色：閒置 `{0, 0, 0}`、製作中 `{255, 0, 0}`、可取餐 `{0, 255, 0}`

---

### 🟡 Lv.3 工廠產線狀態燈（Andon 安燈）

**情境**：工廠產線旁有一座「三色塔燈」，讓遠處的主管一眼看出產線狀況。作業員按按鈕切換狀態：

| 狀態 | 塔燈表現 |
| --- | --- |
| 正常運轉 | 綠燈**緩慢呼吸** |
| 待料中 | 黃燈**恆亮** |
| 設備異常 | 紅燈**閃爍** |

> 🏭 **業界小知識**：Andon（安燈）來自豐田生產系統，是精實管理中「目視化管理」的代表工具——讓問題被看見，才能被即時處理。

**本關新增**：改用 3 顆**單色 LED**（紅、黃、綠）做成塔燈，而不是 RGB LED

**你要想的問題**：

- 三顆燈中，哪一顆**一定要**接 `~` 腳？哪兩顆接一般腳位就可以？說出你的理由。
- 為什麼工廠塔燈用三顆分開的燈，而不是一顆 RGB LED？（想想看：站在 20 公尺外、或是色弱的同仁，哪一種比較容易辨認？）

**程式**：

```cpp
int ledRed    = ?;   // ← 填腳位
int ledYellow = ?;   // ← 填腳位
int ledGreen  = ?;   // ← 填腳位（要呼吸，記得口訣①）
int pinButton = ?;   // ← 填腳位

int mode = 0;        // 0=正常, 1=待料, 2=異常
int lastButtonState = LOW;

void checkButton() {
  int currentState = digitalRead(pinButton);
  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    mode = (mode + 1) % 3;
  }
  lastButtonState = currentState;
}

void allOff() {
  digitalWrite(ledRed, LOW);
  digitalWrite(ledYellow, LOW);
  analogWrite(ledGreen, 0);
}

void setup() {
  pinMode(ledRed, OUTPUT);
  pinMode(ledYellow, OUTPUT);
  pinMode(ledGreen, OUTPUT);
  pinMode(pinButton, INPUT);
}

void loop() {
  checkButton();
  allOff();

  if (mode == 0) {                       // 正常：綠燈呼吸
    for (int b = 0; b <= 255; b += 5) {
      checkButton();
      if (mode != 0) return;
      analogWrite(ledGreen, b);
      delay(10);
    }
    for (int b = 255; b >= 0; b -= 5) {
      checkButton();
      if (mode != 0) return;
      analogWrite(ledGreen, b);
      delay(10);
    }
  } else if (mode == 1) {                // 待料：黃燈恆亮
    digitalWrite(ledYellow, HIGH);
  } else {                               // 異常：紅燈閃爍
    digitalWrite(ledRed, HIGH);
    delay(300);
    digitalWrite(ledRed, LOW);
    delay(300);
  }
}
```

**試試看**：故意把綠燈接到一般腳位（例如 Pin 7），觀察呼吸效果變成什麼樣子，再接回 `~` 腳。

---

### 🟡 Lv.4 雙櫃台取餐系統

**情境**：飲料店生意太好，開了第二個取餐櫃台。兩個櫃台各有一顆 RGB 叫號燈、各有一顆按鈕，互不干擾。

**本關新增**：RGB LED 變成 2 顆、按鈕變成 2 顆

**你要想的問題**：

- 兩顆 RGB LED 共需要幾條顏色線？Arduino 的 `~` 腳夠不夠用？
- `~` 腳全部用完之後，兩顆按鈕只能接到哪些腳位？
- 先填好下面的腳位表再接線：

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| 櫃台 A 燈（R / G / B） | | |
| 櫃台 B 燈（R / G / B） | | |
| 櫃台 A 按鈕 | | |
| 櫃台 B 按鈕 | | |

**程式**：

```cpp
int lightA[3] = { ?, ?, ? };   // ← 櫃台 A 的 R, G, B
int lightB[3] = { ?, ?, ? };   // ← 櫃台 B 的 R, G, B
int btnA = ?;                  // ← 櫃台 A 按鈕
int btnB = ?;                  // ← 櫃台 B 按鈕

int stateA = 0, stateB = 0;    // 0=閒置, 1=製作中, 2=可取餐
int lastA = LOW, lastB = LOW;

void setRGB(int p[], int r, int g, int b) {
  analogWrite(p[0], r);
  analogWrite(p[1], g);
  analogWrite(p[2], b);
}

void showState(int p[], int s) {
  if (s == 0)      setRGB(p, 0, 0, 0);     // 閒置：熄滅
  else if (s == 1) setRGB(p, 255, 0, 0);   // 製作中：紅
  else             setRGB(p, 0, 255, 0);   // 可取餐：綠
}

void setup() {
  for (int i = 0; i < 3; i++) {
    pinMode(lightA[i], OUTPUT);
    pinMode(lightB[i], OUTPUT);
  }
  pinMode(btnA, INPUT);
  pinMode(btnB, INPUT);
}

void loop() {
  int nowA = digitalRead(btnA);
  if (nowA == HIGH && lastA == LOW) { delay(50); stateA = (stateA + 1) % 3; }
  lastA = nowA;

  int nowB = digitalRead(btnB);
  if (nowB == HIGH && lastB == LOW) { delay(50); stateB = (stateB + 1) % 3; }
  lastB = nowB;

  showState(lightA, stateA);
  showState(lightB, stateB);
}
```

**驗收**：按 A 鈕只會改變 A 燈，按 B 鈕只會改變 B 燈。如果按 A 卻是 B 燈在變，代表哪裡接錯了？

---

### 🔴 Lv.5 物流倉庫出貨碼頭

**情境**：物流倉庫有兩個出貨碼頭，每個碼頭有一顆狀態燈和一顆按鈕；碼頭前方的貨車進場通道，有 3 顆黃色引導燈依序跑動，提醒司機往前開。

| 區域 | 需求 |
| --- | --- |
| 碼頭 A、B 狀態燈 | 空閒（藍）→ 裝貨中（黃）→ 完成（綠），各自用按鈕切換 |
| 進場通道引導燈 | 3 顆黃色單色 LED，持續跑馬燈 |

**本關新增**：在 Lv.4 的基礎上，加入 3 顆單色 LED

**你要想的問題**：

- 這一關總共要用幾隻腳位？Pin 2～13 一共有 12 隻，夠不夠？
- 引導燈只需要「亮／滅」輪流跑，依照口訣②，它們應該接在哪裡？
- 在麵包板上，你打算怎麼擺放元件，讓「碼頭區」和「通道區」一眼就分得出來？（擺得清楚，之後除錯會輕鬆很多）

**程式**：拿 Lv.4 的程式來改，需要做三件事：

1. 把 `showState()` 裡的顏色改成：空閒藍 `(0, 0, 255)`、裝貨中黃 `(255, 255, 0)`、完成綠 `(0, 255, 0)`
2. 在最上面加入引導燈的腳位與計數器：

```cpp
int lane[3] = { ?, ?, ? };   // ← 3 顆引導燈的腳位
int laneStep = 0;
```

3. 在 `setup()` 設定 `lane` 為 OUTPUT，並在 `loop()` 最後加入：

```cpp
  for (int i = 0; i < 3; i++) {
    digitalWrite(lane[i], (i == laneStep) ? HIGH : LOW);
  }
  laneStep = (laneStep + 1) % 3;
  delay(200);
```

**擴充討論**：主管想再加開「碼頭 C」，也要一顆 RGB 狀態燈。`~` 腳已經用完了，你有什麼替代方案？請提出至少一種，並說明會犧牲什麼功能。（提示：只用「全亮／全滅」組合 R、G、B，還能做出哪些顏色？）

---

### 🏆 Lv.6 BOSS 關：系統整合提案

**情境**：你是一家顧問公司的系統規劃師，從下面三個客戶需求中**任選一個**，提出完整的燈號系統規劃，並在 Tinkercad 實作出來。

> **案例 A：診所候診叫號**
> 3 間診間，每間門口一顆燈：看診中（紅）、請下一位（綠色呼吸）、休診（熄滅）。櫃台有 1 顆按鈕，按一下輪流選擇要操作哪一間診間。
>
> **案例 B：停車場空位指示**
> 4 個車位，每格上方一顆燈：有車紅燈、空位綠燈。每格用一顆按鈕模擬「車子停入／駛離」。入口另有一顆總覽燈：全滿時紅燈閃爍，還有空位時綠燈恆亮。
>
> **案例 C：自訂情境**
> 自己找一個生活或工作中的情境（例如：會議室使用狀態、圖書館座位、超商自助結帳），至少使用 1 顆 RGB LED、2 顆按鈕、總共 8 隻以上的腳位。

**繳交內容**：

1. **需求分析**：用一兩句話說明這個系統要解決什麼問題、誰會看這些燈。
2. **配線設計單**（見下方）。
3. **Tinkercad 實作**：電路截圖＋接線驗證器的 Serial Monitor 截圖。
4. **程式來源**：說明你是從哪幾個範例改來的，改了哪些地方。

**驗收標準**：

- [ ] 需要漸變或混色的燈接在 `~` 腳，只需亮滅的燈接在一般腳位
- [ ] 每條燈線都有 220Ω 電阻，每顆按鈕都有 10kΩ 電阻
- [ ] 麵包板擺放有分區，看得出哪些元件屬於同一個功能
- [ ] 通過接線驗證器測試
- [ ] 功能符合情境需求

---

### 📝 配線設計單（Lv.4 之後每關必填）

| 項目 | 內容 |
| --- | --- |
| 情境名稱 | |
| 元件清單 | 元件名稱 × 數量（含電阻） |
| 腳位分配表 | 元件 → 腳位（標註是否為 `~` 腳） |
| 腳位使用量 | 共用了幾隻腳？還剩幾隻可以擴充？ |
| 配置理由 | 為什麼這顆燈接 `~` 腳、那顆不用？ |
| 遇到的問題 | 接線過程中出過什麼錯？怎麼發現、怎麼修正的？ |

**評分比重建議**：配線設計單 40%｜電路正確並通過驗證 30%｜功能符合情境 20%｜麵包板擺放整齊度 10%

---

## 本週檢核清單

- [ ] 能判斷一顆燈需不需要接在 `~` 腳位
- [ ] 能依照需求決定要用 RGB LED 還是多顆單色 LED，並說出理由
- [ ] 能在元件變多時，先規劃腳位表再動手接線
- [ ] 能使用接線驗證器自行檢查接線
- [ ] 能沿用既有範例，只修改腳位與顏色就完成新情境
- [ ] 完成 Lv.1～Lv.5 至少 4 關，並完成 Lv.6 系統整合提案

---

## 下週預告

第 4 週我們要幫密室加入「類比輸入」——可變電阻（旋鈕）。玩家將能透過轉動旋鈕，即時控制機關的速度、亮度、甚至角色移動的參數，這也是這學期第一次遇到「連續數值」的輸入訊號，會學到非常實用的 `map()` 數值轉換函式。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 2 週](./week-02.md) | [下一週：第 4 週 — 格鬥搖桿與旋鈕 ➡](./week-04.md)
