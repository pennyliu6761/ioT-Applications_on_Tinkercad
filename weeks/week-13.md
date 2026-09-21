[⬅ 回目錄](../README.md) | [⬅ 上一週：第 12 週](./week-12.md) | [下一週：第 14 週 — 智慧感應與操作台 ➡](./week-14.md)

---

# 第 13 週：產線數字看板升級站
## 主題專案：用 3 隻腳位控制無限多個輸出

> 🎯 **本週定位**：第 1 週我們學過「用陣列 + 迴圈」控制多顆 LED，但無論怎麼優化程式碼，硬體上還是得「一顆燈一條線」，8 顆燈就要佔用 8 隻腳位。這一週你會學到工業控制中極為重要的技巧——**位移暫存器（Shift Register）**，只用 **3 隻腳位**就能控制 8 個、16 個、甚至上百個輸出，這正是真實 PLC（可程式化邏輯控制器）擴充輸出點位的核心概念。密室老闆想把逃脫房裡的簡易指示燈，升級成真正的「數字看板」——你會學會用位移暫存器驅動 **七段顯示器**，做出產線常見的數字計數看板。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [電路體檢站（3 分鐘快速複習）](#電路體檢站3-分鐘快速複習)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道機關關卡](#後端程式設計五道機關關卡)
  - [關卡 1：認識位移暫存器 — shiftOut() 基礎](#關卡-1認識位移暫存器--shiftout-基礎)
  - [關卡 2：用二進位思維控制多顆 LED](#關卡-2用二進位思維控制多顆-led)
  - [關卡 3：七段顯示器基礎 — 認識段位與字型碼](#關卡-3七段顯示器基礎--認識段位與字型碼)
  - [關卡 4：位移暫存器驅動七段顯示器](#關卡-4位移暫存器驅動七段顯示器)
  - [關卡 5：計數看板應用 — 按鈕與感測連動](#關卡-5計數看板應用--按鈕與感測連動)
  - [🏆 BOSS 關：產線數字看板主控系統](#-boss-關產線數字看板主控系統)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎨 額外主題卡（給想多做的人）](#-額外主題卡給想多做的人)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～12 週，整個 Arduino 單元）
- ✅ 數位/類比輸出入、`map()`、`constrain()`、`float` 運算
- ✅ 陣列、迴圈、自訂函式、物件（Servo、LiquidCrystal、Keypad）
- ✅ LED、RGB LED、按鈕、旋鈕、蜂鳴器、光敏電阻、TMP36
- ✅ 伺服馬達、超音波、直流馬達、LCD、矩陣鍵盤
- ✅ 模組化系統架構設計、優先權排序、字串處理

### 這週要新學的
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| 位移暫存器（Shift Register） | 用少量腳位，透過「序列傳輸」控制大量輸出的晶片 | PLC 擴充輸出點位、LED 看板、跑馬燈招牌的標準做法 |
| `shiftOut()` | Arduino 內建函式，把一個位元組（8 個 0/1）依序送出 | 序列通訊最基礎的手法之一 |
| 二進位思維 | 用 8 個位元（bit）的組合代表 8 顆燈的開關狀態 | 這學期第一次正式接觸「用一個數字代表多個開關狀態」的思維 |
| 七段顯示器（7-Segment Display） | 由 7 段 LED 組成，可顯示 0~9 數字的顯示元件 | 產線計數器、電子錶、計分板最常見的數字顯示方式 |
| 字型碼（Segment Pattern） | 每個數字對應一組「該點亮哪幾段」的資料 | 資料驅動設計的再一次實踐——用查表取代大量 if-else |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室老闆參觀完第 11 週的產線戰情看板後意猶未盡，這次要求打造一塊真正「像產線牆上會掛的」數字計數器——不是螢幕文字，而是那種紅色數字跳動的七段顯示器看板。你被指派負責這項升級任務，同時老闆特別強調：「不要浪費太多 Arduino 腳位，我還要留給其他機關用！」這正是這一週要解決的核心工程挑戰。
>
> 這週結束時，你會做出：
> 1. 一個能用 3 隻腳位控制 8 個輸出的基礎測試機關（暖身）
> 2. 一套用二進位思維設計的跑馬燈效果（複習並升級第 1 週的跑馬燈）
> 3. 一個能顯示單一數字的七段顯示器測試機關
> 4. 一套用位移暫存器驅動七段顯示器的整合系統
> 5. 一套結合按鈕與感測器的計數看板應用
> 6. 一個整合以上所有技巧的「產線數字看板主控系統」

---

## 電路體檢站（3 分鐘快速複習）

**體檢題 1**：回想第 1 週的跑馬燈，8 顆 LED 需要 8 隻腳位（Pin 2~9）。這週的位移暫存器只用 3 隻腳位就能控制同樣的 8 顆燈，甚至更多。這跟第 12 週學過的矩陣鍵盤「用 8 隻腳位讀取 16 顆按鍵」的設計思維其實是同一個方向——**用巧妙的電路與程式邏輯，突破硬體腳位數量的限制**，只是這次是「輸出」而非「輸入」。

**體檢題 2**：回想第 3 週學過的「PWM 用時間比例模擬類比效果」——這週的位移暫存器同樣是靠「時間序列」的巧思運作：資料不是同時送出，而是「一個位元接著一個位元，依序傳送」，最後再一次性「鎖定」顯示出來。這種「序列傳輸、平行輸出」的概念，是許多現代通訊協定（如 SPI、I2C）的基礎原型。

**體檢題 3**：位移暫存器與七段顯示器需要接電阻保護嗎？
> 答案：七段顯示器內部本質上是多顆 LED，所以需要限流電阻（通常一組七段顯示器會搭配一顆共用電阻，或每段各自一顆，依電路設計而定）；位移暫存器晶片本身不需要限流電阻，但需要正確接妥電源與接地腳位。

---

## 模擬設計專案配置

### 元件清單
| 元件 | 數量 | 說明 |
|---|---|---|
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1 | 電路佈局 |
| 74HC595 位移暫存器 | 1 | 本週核心元件 |
| LED | 8 | 測試用（關卡 1、2） |
| 220Ω 電阻 | 8 | LED 限流電阻 |
| 7-Segment Display（單顆，共陰極） | 1 | 數字顯示核心元件 |
| 220Ω 電阻 | 1 | 七段顯示器限流電阻（本課程簡化為共用一顆） |
| Pushbutton | 1 | 計數觸發鈕 |
| 10kΩ 電阻 | 1 | 按鈕下拉電阻 |

### 接線步驟

**Part A：74HC595 位移暫存器（關卡 1、2 使用）**
74HC595 是 16 隻腳的晶片，關鍵接法如下：
1. **Pin 8（GND）**：接 GND
2. **Pin 16（VCC）**：接 5V
3. **Pin 10（MR，重置腳）**：接 5V（設為「不重置」的常態，保持晶片正常運作）
4. **Pin 13（OE，輸出致能）**：接 GND（設為「持續輸出」的常態）
5. **Pin 14（DS，資料輸入）**：接 Arduino **Pin 2**（本課程稱為 dataPin）
6. **Pin 12（STCP，鎖存時脈）**：接 Arduino **Pin 3**（本課程稱為 latchPin）
7. **Pin 11（SHCP，位移時脈）**：接 Arduino **Pin 4**（本課程稱為 clockPin）
8. **Pin 15、Pin 1~7（Q0~Q7，輸出腳）**：依序接 8 顆 LED 的正極（各自經過 220Ω 電阻），負極共同接 GND

**Part B：七段顯示器（關卡 3 以後使用，取代 Part A 的 8 顆 LED）**
1. 七段顯示器共陰極的公共腳接 GND（透過共用限流電阻，或依 Tinkercad 元件設定調整）。
2. 七段顯示器的 7 個段位腳（a~g），改接到 74HC595 的 Q0~Q6（原本接 LED 的位置）。

**Part C：計數按鈕**
1. 沿用第 2 週接法：下拉電阻 + 訊號線接 **Pin 5**。

### 腳位對照表（本週共用）
| 元件 | 腳位 |
|---|---|
| 74HC595 DS（資料） | Pin 2 |
| 74HC595 STCP（鎖存） | Pin 3 |
| 74HC595 SHCP（時脈） | Pin 4 |
| 計數按鈕 | Pin 5 |

> ⚠️ **教學提醒**：位移暫存器是這學期腳位對應關係最抽象的一次——Arduino 的 3 隻腳位並不會「直接對應」到某一顆 LED，而是透過程式碼「送出的資料內容」決定哪些輸出腳位是高電位。這跟之前所有元件「一隻 Arduino 腳位對應一個功能」的邏輯完全不同，請務必花時間讓學生理解這個概念轉換。

> 💡 **配置檢查清單**
> - [ ] 74HC595 的 MR（Pin 10）接 5V，OE（Pin 13）接 GND
> - [ ] DS、STCP、SHCP 三隻控制腳正確接到 Arduino Pin 2、3、4
> - [ ] Q0~Q7 依序接到 8 顆 LED（或七段顯示器的對應段位）

---

## 後端程式設計：五道機關關卡

### 關卡 1：認識位移暫存器 — shiftOut() 基礎

**機關故事**：在做出任何顯示效果前，你得先確認 3 隻腳位真的能控制 8 顆燈。

#### 範例 1-1：全部點亮測試
```cpp
// ============================================
// 範例 1-1：用 shiftOut() 一次點亮全部 8 顆燈
// ============================================

int dataPin = 2;    // DS
int latchPin = 3;   // STCP
int clockPin = 4;   // SHCP

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
}

void loop() {
  digitalWrite(latchPin, LOW);                          // 開始準備送資料，先把鎖存腳拉低
  shiftOut(dataPin, clockPin, MSBFIRST, 255);            // 送出 8 個位元，255 代表全部都是 1（全亮）
  digitalWrite(latchPin, HIGH);                          // 資料送完，拉高鎖存腳，正式顯示出來
}
```
**新語法提醒**：`shiftOut(資料腳, 時脈腳, 位元順序, 資料值)` 會把「資料值」轉換成 8 個位元（0 或 1），依序透過資料腳送出。`MSBFIRST` 代表「從最高位元開始送」（Most Significant Bit First），這是最常見的慣例設定。`latchPin` 則像是「確認鍵」——資料送完之前先保持 LOW，送完後才拉 HIGH，讓 8 個輸出腳「一次性」同步更新顯示，避免顯示過程中出現閃爍的中間狀態。

#### 範例 1-2：全部熄滅
```cpp
// ============================================
// 範例 1-2：0 代表全部熄滅
// ============================================

int dataPin = 2;
int latchPin = 3;
int clockPin = 4;

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
}

void loop() {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, 0);   // 0 代表全部都是 0（全滅）
  digitalWrite(latchPin, HIGH);
}
```

#### 範例 1-3：交替閃爍全亮全滅
```cpp
// ============================================
// 範例 1-3：全亮、全滅交替，驗證動態控制效果
// ============================================

int dataPin = 2;
int latchPin = 3;
int clockPin = 4;

void updateShiftRegister(byte value) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, value);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
}

void loop() {
  updateShiftRegister(255);
  delay(500);
  updateShiftRegister(0);
  delay(500);
}
```
**教學重點**：這裡把「送資料到位移暫存器」的三行程式碼（拉低鎖存、送出資料、拉高鎖存）封裝成 `updateShiftRegister()` 函式，往後只要呼叫這個函式並傳入想要的數值即可，這是這學期第 N 次應用「模組化封裝」的手法。

---

### 關卡 2：用二進位思維控制多顆 LED

**機關故事**：全亮全滅太單調，你需要理解「數字如何對應到哪幾顆燈亮」，才能做出精細的圖案效果。

#### 範例 2-1：認識二進位與燈的對應關係
```cpp
// ============================================
// 範例 2-1：測試不同數值，觀察對應的燈光圖案
// ============================================

int dataPin = 2;
int latchPin = 3;
int clockPin = 4;

void updateShiftRegister(byte value) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, value);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
}

void loop() {
  updateShiftRegister(1);    // 二進位 00000001 → 只有 Q0 亮
  delay(1000);
  updateShiftRegister(2);    // 二進位 00000010 → 只有 Q1 亮
  delay(1000);
  updateShiftRegister(4);    // 二進位 00000100 → 只有 Q2 亮
  delay(1000);
  updateShiftRegister(85);   // 二進位 01010101 → Q0,Q2,Q4,Q6 交錯亮起
  delay(1000);
}
```
**教學重點**：每個位元對應一顆燈，把數值換算成二進位（8 位元），從右邊數來第幾位是 1，對應的那顆燈就會亮。例如 `85` 的二進位是 `01010101`，代表 Q0、Q2、Q4、Q6 這幾顆燈（位元值為 1 的位置）會亮起，形成交錯的圖案。這是這學期第一次需要「用二進位思考」的地方，建議搭配計算機的「十進位轉二進位」功能讓學生實際驗證。

#### 範例 2-2：升級版跑馬燈（複習第 1 週，但只用 3 隻腳位）
```cpp
// ============================================
// 範例 2-2：用位移暫存器重新實作跑馬燈效果
// 對照第 1 週：原本需要 8 隻腳位，現在只需要 3 隻！
// ============================================

int dataPin = 2;
int latchPin = 3;
int clockPin = 4;

void updateShiftRegister(byte value) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, value);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
}

void loop() {
  for (int i = 0; i < 8; i++) {
    byte pattern = 1 << i;   // 位元左移運算：讓 1 這個位元往左移動 i 格
    updateShiftRegister(pattern);
    delay(150);
  }
}
```
**新語法提醒**：`1 << i` 是「位元左移運算子」，代表把數字 `1`（二進位 `00000001`）往左移動 `i` 位。當 `i=0` 時結果是 `00000001`（也就是 1），`i=1` 時是 `00000010`（也就是 2），`i=2` 時是 `00000100`（也就是 4）……以此類推。這是控制單一位元移動位置最簡潔的寫法，遠比逐一列出 1, 2, 4, 8, 16, 32, 64, 128 更優雅。

#### 範例 2-3：來回掃描（用位元運算重新實作）
```cpp
// ============================================
// 範例 2-3：來回掃描效果，複習第 1 週的霹靂燈
// ============================================

int dataPin = 2;
int latchPin = 3;
int clockPin = 4;

void updateShiftRegister(byte value) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, value);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
}

void loop() {
  for (int i = 0; i < 8; i++) {
    updateShiftRegister(1 << i);
    delay(100);
  }
  for (int i = 6; i >= 0; i--) {
    updateShiftRegister(1 << i);
    delay(100);
  }
}
```

---

### 關卡 3：七段顯示器基礎 — 認識段位與字型碼

**機關故事**：在整合位移暫存器之前，你得先單獨理解七段顯示器本身怎麼顯示數字。

#### 範例 3-1：直接用 8 隻腳位點亮七段顯示器（暫不用位移暫存器）
```cpp
// ============================================
// 範例 3-1：暫時直接接線測試，理解每一段對應的腳位
// 這裡假設段位 a~g 分別直接接在 Pin 2~8（僅供本範例理解原理使用）
// ============================================

int segA = 2, segB = 3, segC = 4, segD = 5, segE = 6, segF = 7, segG = 8;

void setup() {
  pinMode(segA, OUTPUT);
  pinMode(segB, OUTPUT);
  pinMode(segC, OUTPUT);
  pinMode(segD, OUTPUT);
  pinMode(segE, OUTPUT);
  pinMode(segF, OUTPUT);
  pinMode(segG, OUTPUT);
}

void loop() {
  // 顯示數字 "0"：點亮 a, b, c, d, e, f（不點亮 g）
  digitalWrite(segA, HIGH);
  digitalWrite(segB, HIGH);
  digitalWrite(segC, HIGH);
  digitalWrite(segD, HIGH);
  digitalWrite(segE, HIGH);
  digitalWrite(segF, HIGH);
  digitalWrite(segG, LOW);
}
```
**教學重點**：七段顯示器由 a~g 七個獨立的 LED 段位組成（外加一個小數點 dp），排列成「日」字形，不同的段位組合就能拼出 0~9 的數字。這種「直接接線」的方式雖然直覺，但一顆顯示器就要佔用 7~8 隻腳位，這正是為什麼我們需要位移暫存器來節省腳位的原因。

#### 範例 3-2：用陣列定義字型碼，一次可切換 0~9
```cpp
// ============================================
// 範例 3-2：把 0~9 每個數字對應的段位組合，整理成陣列（字型碼表）
// ============================================

int segPins[] = {2, 3, 4, 5, 6, 7, 8};   // a, b, c, d, e, f, g

// 每個數字對應一組 7 個 0/1，代表 a~g 是否要點亮
int digitPatterns[10][7] = {
  {1,1,1,1,1,1,0},   // 0
  {0,1,1,0,0,0,0},   // 1
  {1,1,0,1,1,0,1},   // 2
  {1,1,1,1,0,0,1},   // 3
  {0,1,1,0,0,1,1},   // 4
  {1,0,1,1,0,1,1},   // 5
  {1,0,1,1,1,1,1},   // 6
  {1,1,1,0,0,0,0},   // 7
  {1,1,1,1,1,1,1},   // 8
  {1,1,1,1,0,1,1}    // 9
};

void showDigit(int digit) {
  for (int i = 0; i < 7; i++) {
    digitalWrite(segPins[i], digitPatterns[digit][i]);
  }
}

void setup() {
  for (int i = 0; i < 7; i++) {
    pinMode(segPins[i], OUTPUT);
  }
}

void loop() {
  for (int d = 0; d <= 9; d++) {
    showDigit(d);
    delay(500);
  }
}
```
**教學重點**：這是這學期「用資料表取代大量 if-else」精神的又一次體現——與其寫 10 組 `if (digit == 0) {...} else if (digit == 1) {...}`，不如把所有數字的字型碼整理成一個二維陣列（複習自第 3 週的混色陣列、第 4 週的顏色資料表），用 `showDigit()` 函式統一查表顯示，程式碼精簡許多。

---

### 關卡 4：位移暫存器驅動七段顯示器

**機關故事**：把關卡 1~3 學過的兩個技術正式結合——用只需要 3 隻腳位的位移暫存器，控制七段顯示器顯示數字。

#### 範例 4-1：用位移暫存器顯示單一數字
```cpp
// ============================================
// 範例 4-1：結合位移暫存器與七段顯示器字型碼
// ============================================

int dataPin = 2;
int latchPin = 3;
int clockPin = 4;

// 把字型碼改成直接對應 byte 數值（每個 bit 代表一段，共 7 段 + 1 個不使用的小數點位）
byte digitCodes[10] = {
  B00111111,   // 0
  B00000110,   // 1
  B01011011,   // 2
  B01001111,   // 3
  B01100110,   // 4
  B01101101,   // 5
  B01111101,   // 6
  B00000111,   // 7
  B01111111,   // 8
  B01101111    // 9
};

void updateShiftRegister(byte value) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, value);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
}

void loop() {
  for (int d = 0; d <= 9; d++) {
    updateShiftRegister(digitCodes[d]);
    delay(500);
  }
}
```
**教學重點**：這裡把字型碼直接寫成 `B00111111` 這種「二進位字面量」格式（`B` 開頭代表這是二進位數字），比關卡 3 的二維陣列更精簡——因為位移暫存器本來就是以「一個 byte（8 位元）」為單位傳輸資料，字型碼剛好也是 7~8 位元，兩者天生適合搭配使用。

#### 範例 4-2：把顯示邏輯封裝成函式
```cpp
// ============================================
// 範例 4-2：封裝成 displayDigit() 函式，方便重複呼叫
// ============================================

int dataPin = 2;
int latchPin = 3;
int clockPin = 4;

byte digitCodes[10] = {
  B00111111, B00000110, B01011011, B01001111, B01100110,
  B01101101, B01111101, B00000111, B01111111, B01101111
};

void displayDigit(int digit) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, digitCodes[digit]);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
}

void loop() {
  displayDigit(8);   // 之後只要呼叫 displayDigit(想要的數字) 即可
}
```

---

### 關卡 5：計數看板應用 — 按鈕與感測連動

**機關故事**：真正的產線計數器需要能夠隨著事件即時累加數字，這一關我們把按鈕輸入與顯示邏輯結合。

#### 範例 5-1：按鈕觸發計數遞增
```cpp
// ============================================
// 範例 5-1：每按一次按鈕，顯示數字 +1（超過 9 就歸零）
// ============================================

int dataPin = 2;
int latchPin = 3;
int clockPin = 4;
int pinButton = 5;
int lastButtonState = LOW;
int count = 0;

byte digitCodes[10] = {
  B00111111, B00000110, B01011011, B01001111, B01100110,
  B01101101, B01111101, B00000111, B01111111, B01101111
};

void displayDigit(int digit) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, digitCodes[digit]);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(pinButton, INPUT);
  displayDigit(0);
}

void loop() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    count = (count + 1) % 10;   // 超過 9 自動歸零，複習第 3 週的取餘數循環技巧
    displayDigit(count);
  }
  lastButtonState = currentState;
}
```

#### 範例 5-2：結合超音波感測器（自動計數，複習第 9 週）
```cpp
// ============================================
// 範例 5-2：用超音波感測器取代按鈕，模擬產線物件通過自動計數
// ============================================

int dataPin = 2;
int latchPin = 3;
int clockPin = 4;
int pinTrig = 6;
int pinEcho = 7;
int count = 0;
bool wasNear = false;

byte digitCodes[10] = {
  B00111111, B00000110, B01011011, B01001111, B01100110,
  B01101101, B01111101, B00000111, B01111111, B01101111
};

void displayDigit(int digit) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, digitCodes[digit]);
  digitalWrite(latchPin, HIGH);
}

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
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  displayDigit(0);
}

void loop() {
  float distance = readDistance();
  bool isNear = distance < 15;

  if (isNear && !wasNear) {
    count = (count + 1) % 10;
    displayDigit(count);
  }
  wasNear = isNear;
  delay(100);
}
```

---

### 🏆 BOSS 關：產線數字看板主控系統

**機關故事**：整合本週所有技巧，打造完整的產線數字看板——按鈕手動計數、超音波自動計數雙模式切換、達到特定數字自動觸發提示。

```cpp
// ============================================
// BOSS 關：產線數字看板主控系統 - 整合本週所有技巧
// 功能：按鈕/超音波雙模式計數 + 位移暫存器驅動七段顯示 + 達標提示
// ============================================

int dataPin = 2;
int latchPin = 3;
int clockPin = 4;
int pinButton = 5;
int pinTrig = 6;
int pinEcho = 7;

int lastButtonState = LOW;
int count = 0;
bool wasNear = false;
int targetCount = 5;   // 達到這個數字視為「一箱完成」

byte digitCodes[10] = {
  B00111111, B00000110, B01011011, B01001111, B01100110,
  B01101101, B01111101, B00000111, B01111111, B01101111
};

void displayDigit(int digit) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, digitCodes[digit]);
  digitalWrite(latchPin, HIGH);
}

void flashAll() {
  // 達標慶祝效果：讓顯示器快速閃爍（透過送出 0 與目前數字交替）
  for (int i = 0; i < 3; i++) {
    digitalWrite(latchPin, LOW);
    shiftOut(dataPin, clockPin, MSBFIRST, 0);
    digitalWrite(latchPin, HIGH);
    delay(150);
    displayDigit(count);
    delay(150);
  }
}

float readDistance() {
  digitalWrite(pinTrig, LOW);
  delayMicroseconds(2);
  digitalWrite(pinTrig, HIGH);
  delayMicroseconds(10);
  digitalWrite(pinTrig, LOW);
  long duration = pulseIn(pinEcho, HIGH);
  return duration / 58.0;
}

void incrementCount() {
  count = (count + 1) % 10;
  displayDigit(count);
  Serial.print("目前計數: ");
  Serial.println(count);

  if (count == targetCount) {
    Serial.println(">>> 已達標，一箱完成！ <<<");
    flashAll();
  }
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(pinButton, INPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  Serial.begin(9600);
  displayDigit(0);
  Serial.println("=== 產線數字看板系統啟動 ===");
}

void loop() {
  // --- 按鈕手動計數 ---
  int currentButtonState = digitalRead(pinButton);
  if (currentButtonState == HIGH && lastButtonState == LOW) {
    delay(50);
    incrementCount();
  }
  lastButtonState = currentButtonState;

  // --- 超音波自動計數 ---
  float distance = readDistance();
  bool isNear = distance < 15;
  if (isNear && !wasNear) {
    incrementCount();
  }
  wasNear = isNear;

  delay(50);
}
```
**教學重點**：這段程式碼把「按鈕觸發」與「超音波觸發」兩種完全不同的輸入來源，統一導向同一個 `incrementCount()` 函式——這再次展現了模組化設計的價值：不管資料從哪裡來，只要呼叫同一個處理函式，就能確保計數邏輯、達標判斷、顯示更新永遠保持一致，不會因為輸入來源不同而出現邏輯分歧或重複程式碼。

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| 位移暫存器完全沒有任何輸出反應 | MR（重置腳）沒有接 5V，或 OE（致能腳）沒有接 GND | 檢查 Pin 10 與 Pin 13 是否依指示正確接線，這兩隻腳常被忽略 |
| 送出的數字跟預期的燈光圖案不一樣 | 對二進位與十進位的對應關係理解有誤 | 用計算機的「程式設計師模式」把十進位數字轉換成二進位，逐一核對每個位元對應哪顆燈 |
| 七段顯示器顯示出奇怪的字型（不是預期的數字） | 字型碼數值填寫錯誤，或共陰極/共陽極接線方式搞混 | 逐一核對 `digitCodes[]` 陣列中每個數字的二進位字型碼是否正確 |
| 畫面更新有明顯的閃爍或過渡效果 | 忘記正確使用 `latchPin` 的「先拉低、送資料、再拉高」流程 | 檢查每次更新顯示時，是否確實依照「LOW → shiftOut → HIGH」的順序執行 |
| 位元左移運算 `1 << i` 結果不如預期 | 對位元運算不熟悉，實際數值與想像有落差 | 建議先手動列出 `i` 從 0 到 7 時，`1 << i` 各自等於多少（1, 2, 4, 8, 16, 32, 64, 128），逐一驗證 |
| 計數超過 9 沒有正確歸零 | `% 10` 的取餘數邏輯寫錯位置或漏寫 | 檢查 `count = (count + 1) % 10;` 是否正確 |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改範例 1-3 的閃爍間隔，從 500 毫秒改成 200 毫秒。
2. 修改範例 2-1，自行設計一個新的燈光圖案數值並觀察效果（提示：可以先用計算機把想要的圖案換算成二進位再轉十進位）。
3. 修改範例 3-2 的字型碼陣列，讓數字 6 改成「不點亮上方橫段」的簡化版寫法（俗稱另一種常見的 6 字型）。
4. 修改範例 4-1 的顯示速度，從 500 毫秒改成 1000 毫秒。
5. 修改範例 5-1，把計數上限從 9 改成 5（超過 5 就歸零）。

### 🟡 中等題
6. 設計一個「倒數顯示」：從 9 開始，每按一次按鈕遞減 1，數到 0 之後額外觸發蜂鳴器（需自行擴充接線）。
7. 修改範例 2-2 的跑馬燈，改成「兩顆燈同時點亮並一起移動」的效果（提示：可以用 `(1 << i) | (1 << (i+1))` 這種位元或運算組合出兩個位元同時為 1）。
8. 結合第 6 週所學，設計一個「溫度整數位數顯示器」：讀取 TMP36 溫度，取整數部分（需了解如何把 `float` 轉換成 `int`），顯示在七段顯示器上。
9. 設計一個「錯誤圖案」：當計數器達到某個特定值時，顯示器改為顯示一個自訂的「非數字」圖案（例如全部橫段組成的圖案），代表異常警示。
10. 修改 BOSS 關程式碼，新增功能：達標慶祝閃爍時，額外統計「今天總共完成了幾箱」並印出。

### 🔴 挑戰題
11. **雙位數顯示挑戰（進階電路思考）**：如果想顯示兩位數字（例如 0~99），你會需要第二顆七段顯示器。請思考：這種情況下，你會如何設計電路與程式邏輯（提示：可以研究「共用位移暫存器、輪流快速切換兩個顯示器」的多工顯示技巧，這是真實多位數顯示器產品的常見做法）。
12. **級聯多顆位移暫存器**：74HC595 有一隻「串接輸出腳（Q7'）」可以接到下一顆 74HC595 的資料輸入腳，理論上能無限串接擴充更多輸出。請查閱相關資料，說明這種「串接（Daisy Chain）」的接線方式與程式碼需要如何調整（本題為研究型挑戰，不強制要求完整實作）。
13. **二進位圖案設計器**：設計一個透過旋鈕（需自行擴充接線）即時選擇 0~255 之間任意數值，並即時顯示對應燈光圖案的「二進位視覺化教具」，可以用來教其他同學認識二進位。
14. **位元運算應用練習**：research並嘗試使用 `|`（位元或）、`&`（位元且）、`~`（位元反轉）等其他位元運算子，設計一個能「切換單一顆燈的開關狀態，而不影響其他顆燈」的函式（提示：這需要用到 XOR 位元運算 `^`）。
15. **完全自訂數字看板應用**：不參考任何範例，設計一個「你認為最實用的產線數字看板應用」，至少要用到：`shiftOut()`、字型碼查表顯示、至少一種感測或按鈕觸發的計數邏輯。

---

## 🎨 額外主題卡（給想多做的人）

> **主題卡 A：骰子模擬器**
> 用位移暫存器搭配 8 顆 LED（排列成骰子的九宮格點位樣式），設計按下按鈕隨機顯示 1~6 點的骰子效果，可複習第 1 週學過的 `random()`。

> **主題卡 B：倒數計時器看板**
> 結合七段顯示器與蜂鳴器，設計一個從 9 開始倒數的計時器，數到 0 時觸發警報聲，模擬產線上的「循環週期計時器」。

> **主題卡 C：二進位教學展示板**
> 專門設計一套「輸入十進位數字（用第 12 週的矩陣鍵盤），自動轉換並顯示對應二進位燈光圖案」的教學小工具，適合用來介紹電腦如何用二進位儲存資料。

> **主題卡 D：分數等級燈號**
> 結合第 4 週的旋鈕，把旋鈕數值轉換成 0~9 的等級並顯示在七段顯示器上，模擬遊戲機台的「難度選擇顯示」介面。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| 位移暫存器（Shift Register） | 位移暫存器 | 用少量腳位、透過序列傳輸控制大量輸出的晶片 |
| `shiftOut()` | 位移輸出 | 把一個位元組的資料依序透過資料腳送出 |
| MSBFIRST | 高位元優先 | 資料傳輸時，從最高位元開始送出的順序慣例 |
| 位元（bit） | 位元 | 只能是 0 或 1 的最小資料單位 |
| 位元組（byte） | 位元組 | 8 個位元組成的資料單位 |
| 位元左移（`<<`） | 位元左移運算 | 把二進位數字的所有位元往左移動指定的格數 |
| 七段顯示器（7-Segment Display） | 七段顯示器 | 由 7 段可獨立點亮的 LED 組成，能顯示 0~9 數字的元件 |
| 字型碼（Segment Pattern） | 字型碼 | 每個數字對應應該點亮哪幾段的資料組合 |
| 鎖存（Latch） | 鎖存 | 確保資料傳輸完成後才一次性更新顯示，避免中途閃爍 |

---

## 反思與延伸討論

1. 這週學到「用 3 隻腳位控制 8 個以上輸出」的位移暫存器技術，跟第 12 週學過「用 8 隻腳位讀取 16 顆按鍵」的矩陣鍵盤，都是「用巧妙設計突破硬體腳位限制」的例子。請討論：這種設計思維，對於一個腳位數量有限的微控制器（如 Arduino Uno 只有約 20 隻可用腳位）來說，帶來了什麼樣的擴充彈性？
2. 這是這學期第一次正式接觸「二進位思維」——用一個數字的每個位元代表不同的開關狀態。請討論：這種「一個變數同時儲存多個獨立資訊」的手法，相較於「每個狀態各自用一個獨立變數儲存」，在記憶體使用與程式效率上分別有什麼優缺點？
3. 七段顯示器的字型碼設計，再次運用了「資料表取代大量條件判斷」的手法（這學期已經在色票陣列、密碼表等地方看過多次類似手法）。請討論：你認為「查表法」相較於「條件判斷法」，在程式碼的可維護性上有什麼優勢？如果要新增一種新的顯示圖案（例如字母 A），哪一種寫法修改起來比較輕鬆？

---

## 本週檢核清單

- [ ] 理解位移暫存器如何用少量腳位控制大量輸出
- [ ] 能正確使用 `shiftOut()` 函式
- [ ] 理解二進位與位元運算的基本概念（特別是位元左移 `<<`）
- [ ] 能用字型碼查表方式控制七段顯示器顯示數字
- [ ] 能結合位移暫存器與七段顯示器，實作完整的數字看板
- [ ] 能整合按鈕與感測器實作計數應用
- [ ] 完成至少 8 題基礎/中等練習題

---

## 下週預告

第 14 週我們要學習「繼電器」與「PIR 動作感測器」，打造一間「智慧感應與操作台」主題房，你會學到如何用小訊號切換大功率負載（這是真實工業配電盤的核心元件），並設計「自動偵測模式」與「手動搖桿操作模式」互相切換的完整控制邏輯。

---

## 🔬 補充實驗室：十進位轉二進位速查表

這張表提供 0~9 及本週範例常用數值的十進位/二進位對照，方便你在設計燈光圖案時快速查閱，不用每次都重新計算：

| 十進位 | 二進位（8 位元） | 對應說明 |
|---|---|---|
| 0 | 00000000 | 全部熄滅 |
| 1 | 00000001 | 只有 Q0 亮 |
| 2 | 00000010 | 只有 Q1 亮 |
| 4 | 00000100 | 只有 Q2 亮 |
| 8 | 00001000 | 只有 Q3 亮 |
| 16 | 00010000 | 只有 Q4 亮 |
| 32 | 00100000 | 只有 Q5 亮 |
| 64 | 01000000 | 只有 Q6 亮 |
| 128 | 10000000 | 只有 Q7 亮 |
| 85 | 01010101 | Q0、Q2、Q4、Q6 交錯亮起 |
| 170 | 10101010 | Q1、Q3、Q5、Q7 交錯亮起（與 85 互補） |
| 255 | 11111111 | 全部點亮 |

**小練習**：對照這張表，試著推算「三顆相鄰的燈同時亮起（例如 Q0、Q1、Q2）」對應的十進位數值應該是多少？寫出來後，實際在 Tinkercad 中驗證你的答案是否正確。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 12 週](./week-12.md) | [下一週：第 14 週 — 智慧感應與操作台 ➡](./week-14.md)
