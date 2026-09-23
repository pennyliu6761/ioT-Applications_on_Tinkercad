[⬅ 回目錄](../README.md) | [⬅ 上一週：第 1 週](./week-01.md) | [下一週：第 3 週 — 迪斯可燈光秀 ➡](./week-03.md)

---

# 第 2 週：搶答對決
## 主題專案：讓玩家真正「動手」參與機關

> 🎯 **本週定位**：上週的機關全部都是自動播放，玩家只能看不能動。這一週我們要加上密室逃脫遊戲最關鍵的元件——**按鈕**——讓玩家第一次能「真正影響機關的行為」。你也會遇到這學期第一個經典硬體 bug：**按鍵彈跳（Bouncing）**，並學會用程式解決它，這是所有做過按鈕輸入的工程師都一定遇過的坑。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [電路體檢站（3 分鐘快速複習）](#電路體檢站3-分鐘快速複習)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道機關關卡](#後端程式設計五道機關關卡)
  - [關卡 1：機關感應測試 — 認識 digitalRead()](#關卡-1機關感應測試--認識-digitalread)
  - [關卡 2：確認鈕邏輯 — if-else 條件判斷](#關卡-2確認鈕邏輯--if-else-條件判斷)
  - [關卡 3：電源鍵 Toggle — 狀態記憶與防彈跳](#關卡-3電源鍵-toggle--狀態記憶與防彈跳)
  - [關卡 4：多鈕協同 — AND / OR 邏輯運算](#關卡-4多鈕協同--and--or-邏輯運算)
  - [關卡 5：搶答對決 — 狀態鎖定與計分](#關卡-5搶答對決--狀態鎖定與計分)
  - [🏆 BOSS 關：三隊搶答計分台](#-boss-關三隊搶答計分台)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎨 額外主題卡（給想多做的人）](#-額外主題卡給想多做的人)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1 週）
- ✅ `pinMode`、`digitalWrite`、`delay` 的基本用法
- ✅ 用陣列 + `for` 迴圈控制多顆 LED
- ✅ 自訂函式的寫法與呼叫方式
- ✅ `Serial.println()` 印出除錯訊息

### 這週要新學的
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| `digitalRead()` | 讀取某個腳位目前是 HIGH 還是 LOW | 任何感測器、限位開關、安全門偵測的基礎 |
| `INPUT` 模式 | 把腳位設定為「讀取外界訊號」 | 與 `OUTPUT` 相對，是輸入輸出的第二種模式 |
| 下拉電阻（Pull-down） | 確保沒按按鈕時，訊號是穩定的 LOW | 避免感測線路「懸浮」產生誤判 |
| `if-else` 條件判斷 | 依照條件做出不同反應 | 幾乎所有控制邏輯的骨架 |
| 按鍵彈跳（Bouncing） | 機械開關瞬間接觸會產生雜訊抖動 | 真實世界的感測訊號永遠不完美，要學會處理 |
| 狀態變數（State Variable） | 用變數「記住」系統目前的狀態 | 電源鍵開關、模式切換、記錄歷史的核心手法 |
| 且(AND) 或(OR) | 邏輯運算子，組合多個條件 | 安全連鎖、多重確認機制的基礎 |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室老闆這週交辦了新任務——玩家反應太差了！上週的機關全部自動播放，玩家根本沒有參與感，體驗評價很低。你需要在這週加入「按鈕互動」，讓玩家真正動手參與。
>
> 這週結束時，你會做出：
> 1. 一個能偵測按鈕狀態的感應測試機關（暖身）
> 2. 一個「按住才生效」的確認鈕機關
> 3. 一個「按一下切換」的電源鍵開關（並解決按鍵彈跳問題）
> 4. 一個需要「雙手同時按住」才會啟動的安全機關（雙人合作解謎）
> 5. 一個記錄「誰先按到」的搶答邏輯
> 6. 一個把以上全部整合的「三隊搶答計分台」——這將是密室逃脫最終關卡的核心機關！

---

## 電路體檢站（3 分鐘快速複習）

**體檢題 1**：上週學過 `pinMode(pin, OUTPUT)` 是設定腳位為輸出。如果我們要讀取按鈕的狀態，應該設定成什麼模式？
> 答案：`INPUT`。輸出（OUTPUT）代表 Arduino 主動發送訊號給元件（如點亮 LED）；輸入（INPUT）代表 Arduino 被動接收元件傳來的訊號（如讀取按鈕有沒有被按下）。

**體檢題 2**：LED 電路一定要接電阻才不會燒毀，那按鈕電路呢？按鈕本身會不會燒毀？
> 答案：按鈕本身不會燒毀（它只是一個機械開關），但按鈕電路仍然需要搭配一顆電阻——這顆電阻的作用跟保護 LED 完全不同，是用來「穩定訊號」，這正是這一週要學的「下拉電阻」概念，稍後會詳細說明。

**體檢題 3**：還記得串聯電路「一條路徑、電流相同」的特性嗎？按鈕接下拉電阻的電路，其實就是善用了這個串聯特性——按鈕沒按下時，電流會走「電阻到 GND」這條路；按下時，電流改走「按鈕到 5V」這條路，讓 Arduino 感應腳位讀到不同的電壓。這跟上期的串並聯觀念是相通的，只是這次不是拿來點燈，而是拿來「切換路徑」。

---

## 模擬設計專案配置

### 元件清單
| 元件 | 數量 | 說明 |
|---|---|---|
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1 | 電路佈局 |
| LED | 3 | 指示燈（紅、黃、綠各一顆，方便分辨隊伍） |
| 220Ω 電阻 | 3 | LED 限流電阻 |
| Pushbutton（按鈕） | 3 | 三隊搶答鈕 |
| 10kΩ 電阻 | 3 | 按鈕下拉電阻 |

### 接線步驟

**Part A：LED 指示燈區（沿用第 1 週作法）**
1. 三顆 LED 插在麵包板上，各自串接 220Ω 電阻。
2. 正極（經電阻）分別接到 **Pin 8（紅）、Pin 9（黃）、Pin 10（綠）**。
3. 負極全部接到麵包板負極軌，負極軌接回 Arduino GND。

**Part B：按鈕區（本週新增）**
1. 拖曳第一顆 **Pushbutton** 到麵包板，跨越中間凹槽放置。
2. 按鈕的一側，接一顆 **10kΩ 電阻**到負極軌（GND）——這就是「下拉電阻」。
3. 同一側，再接一條線到 Arduino 的 **Pin 2**。
4. 按鈕另一側，接到麵包板的正極軌（+），正極軌接回 Arduino 的 **5V**。
5. 重複以上步驟，第二顆按鈕接 **Pin 3**，第三顆按鈕接 **Pin 4**。

### 下拉電阻原理圖解（文字版）
```
       5V
        │
      按鈕
        │
        ├──────── 接到 Arduino Pin（訊號線）
        │
     10kΩ 電阻
        │
       GND
```
- **沒按下時**：電流走「電阻 → GND」這條路，Pin 讀到接近 0V（LOW）。
- **按下時**：電流改走「5V → 按鈕 → Pin」，Pin 讀到接近 5V（HIGH）。
- 如果沒有這顆下拉電阻，按鈕放開時 Pin 會處於「懸浮」狀態，讀值可能忽高忽低、毫無規律，這在工程上稱為「浮動輸入（Floating Input）」，是新手最常忽略卻影響很大的細節。

<img width="656" height="322" alt="image" src="https://github.com/user-attachments/assets/05305383-df78-4049-9a26-2e6299dd2d36" />

### 腳位對照表（本週共用）
| 元件 | 腳位 |
|---|---|
| 按鈕 A（紅隊） | Pin 2 |
| 按鈕 B（黃隊） | Pin 3 |
| 按鈕 C（綠隊） | Pin 4 |
| LED 紅 | Pin 8 |
| LED 黃 | Pin 9 |
| LED 綠 | Pin 10 |

> 💡 **配置檢查清單**
> - [ ] 三顆按鈕都各自接了一顆 10kΩ 下拉電阻到 GND
> - [ ] 三顆按鈕的訊號線分別接到 Pin 2、3、4
> - [ ] 三顆按鈕的另一側都接到 5V 正極軌
> - [ ] 三顆 LED 依照第 1 週的方式正確接線

---

## 後端程式設計：五道機關關卡

### 關卡 1：機關感應測試 — 認識 digitalRead()

**機關故事**：在做出任何複雜邏輯前，你必須先確認「按鈕的訊號真的能被 Arduino 正確讀到」，這是所有輸入型機關開發的第一步。

#### 範例 1-1：讀取單顆按鈕並印出狀態
```cpp
// ============================================
// 範例 1-1：機關感應測試 - 讀取按鈕狀態
// ============================================

int pinButton = 2;

void setup() {
  pinMode(pinButton, INPUT);   // 設定為輸入模式：這隻腳位負責「聽」外界訊號
  Serial.begin(9600);
}

void loop() {
  int state = digitalRead(pinButton);   // 讀取目前是 HIGH（1）還是 LOW（0）
  Serial.println(state);
  delay(100);
}
```
**操作提醒**：開啟 Serial Monitor，用滑鼠點擊按鈕不放，觀察數值從 0 變成 1；放開後應立刻變回 0。

#### 範例 1-2：三顆按鈕同時監控
```cpp
// ============================================
// 範例 1-2：一次驗證三顆按鈕是否都正常
// ============================================

int buttonPins[] = {2, 3, 4};
int numButtons = 3;

void setup() {
  for (int i = 0; i < numButtons; i++) {
    pinMode(buttonPins[i], INPUT);
  }
  Serial.begin(9600);
}

void loop() {
  for (int i = 0; i < numButtons; i++) {
    Serial.print("按鈕");
    Serial.print(i);
    Serial.print(": ");
    Serial.print(digitalRead(buttonPins[i]));
    Serial.print("   ");
  }
  Serial.println();   // 印完三顆按鈕狀態後換行
  delay(150);
}
```
**教學重點**：`Serial.print()`（不換行）和 `Serial.println()`（會換行）的差異——這裡故意混用，讓三顆按鈕的資料可以印在同一行方便比對。

#### 範例 1-3：按下時對應 LED 立刻反應
```cpp
// ============================================
// 範例 1-3：按鈕與 LED 一對一連動測試
// ============================================

int buttonPins[] = {2, 3, 4};
int ledPins[] = {8, 9, 10};
int numPairs = 3;

void setup() {
  for (int i = 0; i < numPairs; i++) {
    pinMode(buttonPins[i], INPUT);
    pinMode(ledPins[i], OUTPUT);
  }
}

void loop() {
  for (int i = 0; i < numPairs; i++) {
    int state = digitalRead(buttonPins[i]);
    digitalWrite(ledPins[i], state);   // 直接把讀到的狀態（0或1）當作輸出值
  }
}
```
**教學重點**：這裡示範了一個小技巧——`digitalWrite()` 的第二個參數其實可以直接放 `digitalRead()` 讀到的數值（因為 HIGH 就是 1、LOW 就是 0），不一定要透過 `if-else` 轉換，程式碼可以更精簡。

---

### 關卡 2：確認鈕邏輯 — if-else 條件判斷

**機關故事**：密室設計了一個「品管確認站」，玩家必須把手放在感應鈕上維持按住，代表「正在確認線索」，放開手就代表「確認結束」。

#### 範例 2-1：按住才亮的確認燈
```cpp
// ============================================
// 範例 2-1：按住時亮起，代表「正在確認中」
// ============================================

int pinButton = 2;
int pinLed = 8;

void setup() {
  pinMode(pinButton, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  int state = digitalRead(pinButton);

  if (state == HIGH) {
    digitalWrite(pinLed, HIGH);   // 按下 → 燈亮
  } else {
    digitalWrite(pinLed, LOW);    // 放開 → 燈滅
  }
}
```

#### 範例 2-2：搭配 Serial 訊息的雙態機關
```cpp
// ============================================
// 範例 2-2：加上文字提示，讓機關狀態更清楚
// ============================================

int pinButton = 2;
int pinLed = 8;

void setup() {
  pinMode(pinButton, INPUT);
  pinMode(pinLed, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int state = digitalRead(pinButton);

  if (state == HIGH) {
    digitalWrite(pinLed, HIGH);
    Serial.println("狀態：確認中...");
  } else {
    digitalWrite(pinLed, LOW);
    Serial.println("狀態：待機");
  }
  delay(200);   // 避免 Serial Monitor 洗版太快
}
```

#### 範例 2-3：三段情境反應（多重 else if）
```cpp
// ============================================
// 範例 2-3：三顆按鈕各自對應不同機關反應
// 引入 else if，處理超過兩種情況的判斷
// ============================================

int pinButtonA = 2;
int pinButtonB = 3;
int pinButtonC = 4;
int pinLedA = 8;
int pinLedB = 9;
int pinLedC = 10;

void setup() {
  pinMode(pinButtonA, INPUT);
  pinMode(pinButtonB, INPUT);
  pinMode(pinButtonC, INPUT);
  pinMode(pinLedA, OUTPUT);
  pinMode(pinLedB, OUTPUT);
  pinMode(pinLedC, OUTPUT);
}

void loop() {
  // 先全部熄滅，避免殘留上一輪的亮燈狀態
  digitalWrite(pinLedA, LOW);
  digitalWrite(pinLedB, LOW);
  digitalWrite(pinLedC, LOW);

  if (digitalRead(pinButtonA) == HIGH) {
    digitalWrite(pinLedA, HIGH);
  } else if (digitalRead(pinButtonB) == HIGH) {
    digitalWrite(pinLedB, HIGH);
  } else if (digitalRead(pinButtonC) == HIGH) {
    digitalWrite(pinLedC, HIGH);
  }
  // 如果三顆都沒按，三顆燈維持熄滅（因為迴圈一開始已經全部關閉）
}
```
**思考問題**：如果玩家同時按下 A 鈕與 B 鈕，範例 2-3 最後只會點亮哪一顆燈？為什麼？（提示：`else if` 具有「順序優先權」，只要前面的條件成立，後面的就不會再被檢查）

---

### 關卡 3：電源鍵 Toggle — 狀態記憶與防彈跳

**機關故事**：品管站的主管抱怨「按著才亮」太累人，希望改成像家用電燈開關一樣——「按一下開機、再按一下關機」。

#### 範例 3-1：初版 Toggle（會出現彈跳問題）
```cpp
// ============================================
// 範例 3-1：電源鍵切換 - 第一次嘗試（尚未處理彈跳問題）
// ============================================

int pinButton = 2;
int pinLed = 8;

bool machineOn = false;       // 記錄機台目前開/關狀態
int lastButtonState = LOW;    // 記錄「上一次」讀到的按鈕狀態

void setup() {
  pinMode(pinButton, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  int currentState = digitalRead(pinButton);

  // 偵測「由 LOW 變成 HIGH」的瞬間，而不是「持續按著」
  if (currentState == HIGH && lastButtonState == LOW) {
    machineOn = !machineOn;   // 反轉狀態：true 變 false，false 變 true
  }

  digitalWrite(pinLed, machineOn ? HIGH : LOW);
  lastButtonState = currentState;
}
```
**觀察任務**：在 Tinkercad 模擬環境中快速連續點擊這顆按鈕，你可能會發現燈的切換偶爾「跳過一次」或「連續切兩次」——這就是**按鍵彈跳**造成的現象。真實硬體上的機械開關，接觸瞬間會有極短暫（幾毫秒）的高頻抖動，Arduino 反應速度極快，甚至能「看到」這些抖動，誤判成多次按壓。

#### 範例 3-2：加入防彈跳延遲
```cpp
// ============================================
// 範例 3-2：加入簡易防彈跳延遲，解決範例 3-1 的問題
// ============================================

int pinButton = 2;
int pinLed = 8;

bool machineOn = false;
int lastButtonState = LOW;

void setup() {
  pinMode(pinButton, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);   // 偵測到變化瞬間，暫停 50 毫秒讓訊號穩定下來，再繼續判斷
    machineOn = !machineOn;
  }

  digitalWrite(pinLed, machineOn ? HIGH : LOW);
  lastButtonState = currentState;
}
```

#### 範例 3-3：計數器機關（累計按壓次數）
```cpp
// ============================================
// 範例 3-3：每按一次，累加密室通關次數並印出
// ============================================

int pinButton = 2;
int passCount = 0;
int lastButtonState = LOW;

void setup() {
  pinMode(pinButton, INPUT);
  Serial.begin(9600);
}

void loop() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    passCount = passCount + 1;
    Serial.print("累計通關人數：");
    Serial.println(passCount);
  }

  lastButtonState = currentState;
}
```

#### 範例 3-4：三段模式循環切換（不只開/關兩種狀態）
```cpp
// ============================================
// 範例 3-4：按一次切換到下一個模式，循環 0 → 1 → 2 → 0...
// 情境：機關燈光模式選擇（0=待機、1=慢閃、2=快閃）
// ============================================

int pinButton = 2;
int pinLed = 8;
int lastButtonState = LOW;
int modeIndex = 0;   // 目前模式編號

void setup() {
  pinMode(pinButton, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  int currentState = digitalRead(pinButton);

  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    modeIndex = (modeIndex + 1) % 3;   // % 取餘數，讓數字在 0、1、2 之間循環
  }
  lastButtonState = currentState;

  if (modeIndex == 0) {
    digitalWrite(pinLed, LOW);          // 模式 0：待機（熄滅）
  } else if (modeIndex == 1) {
    digitalWrite(pinLed, HIGH);
    delay(500);
    digitalWrite(pinLed, LOW);
    delay(500);                          // 模式 1：慢閃
  } else if (modeIndex == 2) {
    digitalWrite(pinLed, HIGH);
    delay(100);
    digitalWrite(pinLed, LOW);
    delay(100);                          // 模式 2：快閃
  }
}
```
**思考問題**：範例 3-4 的模式 1 與模式 2 內部都用了 `delay()`，這會不會影響按鈕偵測的即時性？（先記住這個疑問，第 14 週學到 `millis()` 之後會有更好的解法）

---

### 關卡 4：多鈕協同 — AND / OR 邏輯運算

**機關故事**：密室有一個「雙人合作機關」，需要兩位玩家同時各按住一顆按鈕，機關才會啟動，模擬需要團隊合作才能解開的謎題（工業上這正是沖床等危險設備的「雙手啟動裝置」安全設計理念）。

#### 範例 4-1：AND 邏輯 — 兩顆都要按才啟動
```cpp
// ============================================
// 範例 4-1：雙人合作機關 - 必須同時按住兩顆按鈕
// ============================================

int pinButtonA = 2;
int pinButtonB = 3;
int pinLed = 8;

void setup() {
  pinMode(pinButtonA, INPUT);
  pinMode(pinButtonB, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  bool playerA = digitalRead(pinButtonA) == HIGH;
  bool playerB = digitalRead(pinButtonB) == HIGH;

  // && 代表「兩者都必須成立」
  if (playerA && playerB) {
    digitalWrite(pinLed, HIGH);
  } else {
    digitalWrite(pinLed, LOW);
  }
}
```

#### 範例 4-2：OR 邏輯 — 任一顆按下就啟動
```cpp
// ============================================
// 範例 4-2：緊急求救鈕 - 任何一位玩家按下都能觸發
// ============================================

int pinButtonA = 2;
int pinButtonB = 3;
int pinButtonC = 4;
int pinLed = 8;

void setup() {
  pinMode(pinButtonA, INPUT);
  pinMode(pinButtonB, INPUT);
  pinMode(pinButtonC, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  bool a = digitalRead(pinButtonA) == HIGH;
  bool b = digitalRead(pinButtonB) == HIGH;
  bool c = digitalRead(pinButtonC) == HIGH;

  // || 代表「任何一個成立就算成立」
  if (a || b || c) {
    digitalWrite(pinLed, HIGH);
  } else {
    digitalWrite(pinLed, LOW);
  }
}
```

#### 範例 4-3：三人團隊機關（至少 2 人同意才啟動）
```cpp
// ============================================
// 範例 4-3：進階邏輯 - 三人中至少兩人同時按住才啟動
// 情境：模擬「多數決」機制
// ============================================

int pinButtonA = 2;
int pinButtonB = 3;
int pinButtonC = 4;
int pinLed = 8;

void setup() {
  pinMode(pinButtonA, INPUT);
  pinMode(pinButtonB, INPUT);
  pinMode(pinButtonC, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  bool a = digitalRead(pinButtonA) == HIGH;
  bool b = digitalRead(pinButtonB) == HIGH;
  bool c = digitalRead(pinButtonC) == HIGH;

  // 至少兩人同意 = (A且B) 或 (B且C) 或 (A且C)
  bool atLeastTwo = (a && b) || (b && c) || (a && c);

  digitalWrite(pinLed, atLeastTwo ? HIGH : LOW);
}
```
**教學重點**：這裡示範了如何把多個 `&&` 和 `||` 組合起來，構成更複雜的邏輯判斷。這種「多數決」邏輯其實也是真實工業安全系統常見的設計，例如核電廠的多重感測器表決機制。

---

### 關卡 5：搶答對決 — 狀態鎖定與計分

**機關故事**：密室的最終謎題公布區，需要一個經典的「搶答器」——誰先按下按鈕，誰的燈就亮起，並且鎖定其他人的按鈕，直到裁判重置。

#### 範例 5-1：基礎搶答邏輯（雙人版）
```cpp
// ============================================
// 範例 5-1：雙人搶答 - 先按先贏，並鎖定另一方
// ============================================

int pinButtonA = 2;
int pinButtonB = 3;
int pinLedA = 8;
int pinLedB = 9;

int winner = 0;   // 0 = 尚未有人搶到, 1 = A, 2 = B

void setup() {
  pinMode(pinButtonA, INPUT);
  pinMode(pinButtonB, INPUT);
  pinMode(pinLedA, OUTPUT);
  pinMode(pinLedB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // 只有在「尚未有人搶到」時，才允許判定新的贏家
  if (winner == 0) {
    if (digitalRead(pinButtonA) == HIGH) {
      winner = 1;
      Serial.println("A 搶到了！");
    } else if (digitalRead(pinButtonB) == HIGH) {
      winner = 2;
      Serial.println("B 搶到了！");
    }
  }

  digitalWrite(pinLedA, winner == 1 ? HIGH : LOW);
  digitalWrite(pinLedB, winner == 2 ? HIGH : LOW);
}
```
**觀察任務**：這段程式碼「沒有」重置機制，一旦有人搶到，之後不管怎麼按都不會改變結果。這是刻意留下的設計缺陷，我們會在 BOSS 關補上重置按鈕。

#### 範例 5-2：加上裁判重置鈕
```cpp
// ============================================
// 範例 5-2：加入第三顆按鈕作為裁判重置鍵
// ============================================

int pinButtonA = 2;
int pinButtonB = 3;
int pinReset = 4;
int pinLedA = 8;
int pinLedB = 9;

int winner = 0;

void setup() {
  pinMode(pinButtonA, INPUT);
  pinMode(pinButtonB, INPUT);
  pinMode(pinReset, INPUT);
  pinMode(pinLedA, OUTPUT);
  pinMode(pinLedB, OUTPUT);
}

void loop() {
  if (digitalRead(pinReset) == HIGH) {
    winner = 0;   // 裁判按下重置鈕，清空贏家紀錄
  }

  if (winner == 0) {
    if (digitalRead(pinButtonA) == HIGH) {
      winner = 1;
    } else if (digitalRead(pinButtonB) == HIGH) {
      winner = 2;
    }
  }

  digitalWrite(pinLedA, winner == 1 ? HIGH : LOW);
  digitalWrite(pinLedB, winner == 2 ? HIGH : LOW);
}
```

#### 範例 5-3：搶答計時器（記錄從開始到搶到的反應時間概念）
```cpp
// ============================================
// 範例 5-3：用簡易計數器模擬「反應時間排名」概念
// 說明：這裡先用「累加迴圈次數」土法煉鋼模擬時間流逝，
//       第 14 週學到 millis() 之後，會有更精準的做法
// ============================================

int pinButtonA = 2;
int pinButtonB = 3;
int pinReset = 4;

int winner = 0;
int elapsedTicks = 0;   // 累計「已經過了幾輪迴圈」，粗略代表反應時間快慢

void setup() {
  pinMode(pinButtonA, INPUT);
  pinMode(pinButtonB, INPUT);
  pinMode(pinReset, INPUT);
  Serial.begin(9600);
}

void loop() {
  if (digitalRead(pinReset) == HIGH) {
    winner = 0;
    elapsedTicks = 0;
    Serial.println("--- 重置，準備下一輪 ---");
  }

  if (winner == 0) {
    elapsedTicks++;   // 每跑一輪 loop() 就累加，模擬時間流逝

    if (digitalRead(pinButtonA) == HIGH) {
      winner = 1;
      Serial.print("A 搶到！用時刻度：");
      Serial.println(elapsedTicks);
    } else if (digitalRead(pinButtonB) == HIGH) {
      winner = 2;
      Serial.print("B 搶到！用時刻度：");
      Serial.println(elapsedTicks);
    }
  }
}
```

---

### 🏆 BOSS 關：三隊搶答計分台

**機關故事**：整合本週所有技巧，打造密室最終謎題公布區的完整三隊搶答系統——先搶先贏、鎖定其他隊伍、裁判可重置、並且累計每一隊的總得分。

```cpp
// ============================================
// BOSS 關：三隊搶答計分台 - 整合本週所有技巧
// 功能：搶答鎖定 → 裁判判定得分 → 裁判重置 → 累計比分
// ============================================

int buttonPins[] = {2, 3, 4};    // 紅、黃、綠三隊搶答鈕
int ledPins[] = {8, 9, 10};      // 紅、黃、綠三隊指示燈
int pinJudgeCorrect = 5;         // 裁判判定「答對」按鈕
int pinJudgeReset = 6;           // 裁判重置按鈕

int numTeams = 3;
int winner = -1;                 // -1 代表尚未有隊伍搶到
int scores[] = {0, 0, 0};        // 三隊各自的累計得分

int lastJudgeCorrectState = LOW;
int lastJudgeResetState = LOW;

void setup() {
  for (int i = 0; i < numTeams; i++) {
    pinMode(buttonPins[i], INPUT);
    pinMode(ledPins[i], OUTPUT);
  }
  pinMode(pinJudgeCorrect, INPUT);
  pinMode(pinJudgeReset, INPUT);
  Serial.begin(9600);
  Serial.println("=== 搶答系統啟動！比分歸零 ===");
}

void loop() {
  // --- 搶答判定：只有尚未有人搶到時才受理新的搶答 ---
  if (winner == -1) {
    for (int i = 0; i < numTeams; i++) {
      if (digitalRead(buttonPins[i]) == HIGH) {
        winner = i;
        Serial.print("隊伍 ");
        Serial.print(i);
        Serial.println(" 搶到作答權！");
        break;   // 找到第一個搶到的隊伍後，立刻跳出迴圈，不再檢查後面的按鈕
      }
    }
  }

  // --- 更新搶答指示燈 ---
  for (int i = 0; i < numTeams; i++) {
    digitalWrite(ledPins[i], (i == winner) ? HIGH : LOW);
  }

  // --- 裁判判定答對，該隊加分並重置搶答狀態 ---
  int judgeCorrectState = digitalRead(pinJudgeCorrect);
  if (judgeCorrectState == HIGH && lastJudgeCorrectState == LOW) {
    delay(50);   // 防彈跳
    if (winner != -1) {
      scores[winner] = scores[winner] + 1;
      Serial.print("隊伍 ");
      Serial.print(winner);
      Serial.print(" 答對！目前比分 → 紅:");
      Serial.print(scores[0]);
      Serial.print(" 黃:");
      Serial.print(scores[1]);
      Serial.print(" 綠:");
      Serial.println(scores[2]);
      winner = -1;   // 該題結束，重置搶答狀態，準備下一題
    }
  }
  lastJudgeCorrectState = judgeCorrectState;

  // --- 裁判重置（不加分，單純該題作廢重來） ---
  int judgeResetState = digitalRead(pinJudgeReset);
  if (judgeResetState == HIGH && lastJudgeResetState == LOW) {
    delay(50);
    winner = -1;
    Serial.println("裁判重置，本題重新搶答");
  }
  lastJudgeResetState = judgeResetState;
}
```
**教學重點**：這段程式碼綜合運用了：陣列管理多隊資料、`break` 提前跳出迴圈、防彈跳、狀態鎖定、多組計分變數。這正是這學期第一個具備「完整商業邏輯」的專案——如果拿去真實的班級搶答比賽使用，幾乎可以直接上場。

**新語法提醒**：`break;` 會立刻跳出目前所在的迴圈，不再檢查剩下的內容。這裡用來確保「只要找到第一個搶到的隊伍，就不用再浪費時間檢查後面的按鈕」。

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| 按鈕讀值一直跳動或恆為 HIGH | 忘記接下拉電阻，腳位處於「懸浮」狀態 | 確認下拉電阻確實接到 GND，且訊號線接在按鈕與電阻的交界處 |
| 按一下切換卻連續切好幾次 | 按鍵彈跳（Bouncing） | 加入 `delay(50)` 簡易防彈跳，並確認用的是「瞬間變化偵測」而非「持續狀態」 |
| Toggle 邏輯完全沒反應 | `lastButtonState` 初始值設錯，或忘記在迴圈最後更新它 | 檢查 `lastButtonState = currentState;` 是否確實寫在 `loop()` 的最後 |
| 搶答器永遠鎖定在第一次結果 | 忘記加上重置邏輯，或 `winner` 變數沒有被正確清空 | 檢查裁判重置按鈕的程式碼是否正確執行 `winner = -1;` |
| AND / OR 邏輯結果與預期相反 | 誤用 `&`（位元運算）而非 `&&`（邏輯運算），或 `=` 與 `==` 混淆 | 條件判斷務必使用 `&&`、`||`、`==`，單一等號 `=` 是賦值不是比較 |
| `break` 用了但邏輯還是跑完整個迴圈 | `break` 寫在錯誤的位置（例如寫在 if 外層） | 確認 `break` 確實放在「找到目標後」立即要跳出的那個位置內 |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改範例 1-1，把 `delay(100)` 改成 `delay(500)`，觀察 Serial Monitor 更新速度的變化，並說明優缺點。
2. 修改範例 2-1，改成「按下時熄滅、放開時點亮」（邏輯完全相反）。
3. 修改範例 3-2 的防彈跳延遲時間，從 50 毫秒改成 5 毫秒，觀察是否還會出現彈跳問題。
4. 寫一個程式，讓範例 4-1 的雙人合作機關，改成需要「三顆按鈕全部同時按住」才會啟動。
5. 修改範例 5-1，把雙人搶答擴充為三人搶答（新增第三顆按鈕與燈）。

### 🟡 中等題
6. 設計一個「二選一投票器」：兩顆按鈕分別代表「同意」與「不同意」，各自累計按壓次數，並即時印出目前票數比。
7. 修改範例 3-4 的三段模式，新增第四個模式「交替閃爍」（兩顆燈輪流亮）。
8. 幫範例 4-3 的「三人多數決」機關，加上一顆 LED 指示燈，只要判定結果為啟動就亮起。
9. 修改 BOSS 關程式碼，新增功能：當任何一隊得分達到 5 分時，該隊 LED 開始快速閃爍慶祝（提示：需要在計分判斷後加入新的 if 判斷式）。
10. 設計一個「長按判斷」練習：按鈕按下超過某個累計次數（模擬長按 2 秒以上，用類似範例 5-3 的計數方式），才觸發某個動作，短按則不觸發。

### 🔴 挑戰題
11. **防作弊搶答器**：修改 BOSS 關程式碼，如果裁判還沒開放搶答（新增一個「開放中/未開放」的狀態變數）玩家就按下按鈕，該隊應被判定「搶答犯規」，直接扣 1 分並鎖住這一題的作答資格。
12. **加權計分系統**：設計一個機制，讓「越早搶到」得分越高（例如第一輪比賽用 `elapsedTicks` 概念，時間刻度越小分數越高，可參考範例 5-3 並自行設計換算公式）。
13. **多重連鎖安全機關**：設計一個模擬「三道安全鎖」的機關——鎖 1（按鈕 A）解開後，鎖 2（按鈕 B）才會生效；鎖 2 解開後，鎖 3（按鈕 C）才會生效；全部解開後 LED 才會亮起，代表通關（提示：需要三個獨立的布林狀態變數，並用巢狀 `if` 或 `&&` 串接判斷条件）。
14. **搶答與燈光特效整合**：把第 1 週學過的跑馬燈效果，套用在「尚未有人搶答」時的待機畫面，一旦有人搶到，立刻切換成該隊的固定燈號（需要結合本週的狀態變數與第 1 週的迴圈特效）。
15. **自訂裁判系統**：不參考任何範例，自己設計一套「至少 4 隊、含犯規判定、含重置、含最終冠軍宣布」的完整搶答系統，並寫一小段文字說明你的規則設計。

---

## 🎨 額外主題卡（給想多做的人）

> **主題卡 A：密室逃脫問答機**
> 設計一個機關，裁判先口頭出題，玩家按下自己認為正確的按鈕（例如三顆按鈕分別代表 A、B、C 三個選項），裁判再用另一顆按鈕公布正確答案是哪個選項，答對的隊伍燈亮起慶祝。

> **主題卡 B：真心話大冒險轉盤模擬**
> 用「隨機挑選」的概念（第 1 週學過的 `random()`）結合本週的按鈕，設計一個「按下按鈕後，隨機決定要做真心話還是大冒險」的機關，並用不同的燈光效果代表兩種結果。

> **主題卡 C：門禁刷卡模擬**
> 模擬公司門禁系統：平時綠燈亮（代表門鎖狀態），按下按鈕視為「刷卡」，燈光切換為黃燈 2 秒（代表驗證中），驗證完成後短暫亮綠燈並印出「歡迎進入」，之後恢復平時狀態。

> **主題卡 D：多益考場作答燈號**
> 模擬考場監控系統：三個燈號分別代表「作答中」、「舉手發問」、「已交卷」三種狀態，用一顆按鈕依序切換三種狀態（類似範例 3-4 的模式切換邏輯），並在 Serial Monitor 印出對應的狀態說明文字。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| `INPUT` | 輸入模式 | 這隻腳位要拿來「接收」外界訊號 |
| `digitalRead()` | 數位讀取 | 讀取某個腳位目前是 HIGH 還是 LOW |
| 下拉電阻（Pull-down Resistor） | 下拉電阻 | 確保沒有訊號時，腳位穩定讀到 LOW，避免懸浮雜訊 |
| `if / else / else if` | 條件判斷 | 依照條件成立與否，執行不同的程式碼區塊 |
| 按鍵彈跳（Bouncing） | 按鍵彈跳 | 機械開關瞬間接觸產生的短暫訊號抖動 |
| 防彈跳（Debounce） | 防彈跳 | 用程式手法（如短暫延遲）過濾掉彈跳雜訊 |
| 狀態變數（State Variable） | 狀態變數 | 用變數記住系統目前所處的狀態 |
| `bool` | 布林變數 | 只能存 `true` 或 `false` 兩種值的變數型別 |
| `&&`（AND） | 邏輯且 | 兩個條件都成立，結果才成立 |
| `\|\|`（OR） | 邏輯或 | 任一個條件成立，結果就成立 |
| `%`（取餘數） | 取餘數運算 | 計算除法後剩下的餘數，常用於做「循環」效果 |
| `break` | 跳出迴圈 | 立刻結束目前所在的迴圈，不再執行剩餘的迴圈內容 |

---

## 反思與延伸討論

1. 下拉電阻確保按鈕沒按時讀到穩定的 LOW，如果反過來，把電阻接到 5V（稱為「上拉電阻」），按鈕邏輯會變成怎樣？（沒按時讀到 HIGH，按下讀到 LOW）你能想到什麼情境下，工程師會故意選擇這種「邏輯反過來」的設計嗎？
2. 按鍵彈跳問題告訴我們，真實世界的感測訊號往往不像教科書那樣「乾淨」。請討論：除了機械按鈕，還有哪些感測器（想想第 5 週之後會學到的光敏電阻、溫度感測器）也可能有類似的雜訊問題？工程師可以用哪些通用手法應對（提示：多次讀取取平均、加入延遲確認、設定合理的容忍範圍）？
3. BOSS 關的搶答系統中，`winner` 變數扮演了「鎖定」的角色。請討論：在真實工廠自動化系統中，還有哪些場景需要類似的「一旦觸發就鎖定、需要人工或特定條件才能解除」的設計邏輯？（提示：可以回想上學期或未來會學到的異常警報機制）

---

## 本週檢核清單

- [ ] 能正確說明 `INPUT` 與 `OUTPUT` 模式的差異
- [ ] 理解下拉電阻的作用，並能自行畫出簡易電路示意
- [ ] 能獨立寫出 `if-else` 條件判斷邏輯
- [ ] 理解按鍵彈跳現象，並能實作防彈跳邏輯
- [ ] 能用狀態變數實作 Toggle（開關切換）邏輯
- [ ] 能正確使用 `&&` 與 `||` 組合多重條件
- [ ] 能獨立完成一個具備「鎖定」與「重置」機制的搶答系統
- [ ] 完成至少 8 題基礎/中等練習題

---

## 下週預告

第 3 週我們要幫密室的燈光系統升級成「全彩」——學習 PWM 類比輸出與 RGB LED 混色，打造一整套「迪斯可派對燈光秀」主題機關。你會學到如何用同一顆燈呈現無限多種顏色，而不再需要為每種顏色各接一顆獨立的 LED。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 1 週](./week-01.md) | [下一週：第 3 週 — 迪斯可燈光秀 ➡](./week-03.md)
