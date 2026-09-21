[⬅ 回目錄](../README.md) | [⬅ 上一週：第 15 週](./week-15.md)

---

# 第 16 週：終極整合大專題（下）
## 主題專案：從 v1.0 到最終完整版 — 進階重構、壓力測試與最終完成

> 🎯 **本週定位**：這是整個學期的最後一週。上週你完成了終極密室系統 v1.0，這週我們要在這個基礎上做四件事：把 `delay()` 架構重構成真正的 `millis()` 非阻塞式狀態機、設計多階段關卡進度系統、進行專業等級的壓力測試與除錯、最後優化整體使用者體驗。完成本週後，你會擁有一套這學期最複雜、也最完整的作品——這不只是一份作業，更是你這一學期程式設計能力最好的證明。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [後端程式設計：四大進階升級](#後端程式設計四大進階升級)
  - [升級 1：從 delay() 到 millis() 狀態機重構](#升級-1從-delay-到-millis-狀態機重構)
  - [升級 2：多階段關卡進度系統](#升級-2多階段關卡進度系統)
  - [升級 3：極限壓力測試與除錯](#升級-3極限壓力測試與除錯)
  - [升級 4：使用者體驗優化](#升級-4使用者體驗優化)
  - [🏆 最終整合：終極密室系統 最終完整版](#-最終整合終極密室系統-最終完整版)
- [QA 品質保證：完整驗收檢核表](#qa-品質保證完整驗收檢核表)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎓 期末成果展演指南](#-期末成果展演指南)
- [🎨 給有餘力的你：延伸挑戰](#-給有餘力的你延伸挑戰)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [學期總回顧與本週檢核清單](#學期總回顧與本週檢核清單)

---

## 學習地圖

### 這週要精進的核心能力
| 技能 | 說明 | 對應的過去學習 |
|---|---|---|
| `millis()` 非阻塞式狀態機 | 讓系統能同時處理多個任務，不被 `delay()` 卡住 | 第 8、14 週初步接觸過的概念，這週正式完整實作 |
| 多階段進度系統 | 用狀態變數追蹤玩家在整個機關中的進度 | 第 6、7、8、14 週的狀態管理技巧總集合 |
| 壓力測試 | 系統性地用極端情境測試系統穩定性 | 第 8 週學過的 QA 邊界測試，這週規模擴大到整個系統 |
| 使用者體驗優化 | 開機動畫、聲光反饋細節、操作流暢度 | 這學期各週累積的聲光效果技巧的綜合運用 |

---

## 開場任務簡報

> 📖 **情境**：密室老闆下週就要正式對外營運，要求你在這週完成終極密室的最終驗收——不能再有「大概可以動」的程度，必須是「經得起反覆測試、玩家體驗流暢」的完整作品。這是這學期最後一次，也是最重要的一次「從堪用到完善」的品質提升過程。

---

## 後端程式設計：四大進階升級

### 升級 1：從 delay() 到 millis() 狀態機重構

**為什麼要做這件事**：上週的 v1.0 系統中，任何 `delay()` 都會讓整個系統「卡住」，這段期間所有感測器都無法即時反應（例如緊急停止按鈕在 `delay()` 期間完全沒有用）。這是這學期最後、也是最重要的一次架構升級。

#### 範例 1-1：基礎 millis() 計時回顧
```cpp
// ============================================
// 範例 1-1：複習 millis() 的基本用法
// ============================================

unsigned long previousTime = 0;
long interval = 1000;
int pinLed = 13;
bool ledState = false;

void setup() {
  pinMode(pinLed, OUTPUT);
}

void loop() {
  unsigned long currentTime = millis();

  if (currentTime - previousTime >= interval) {
    previousTime = currentTime;
    ledState = !ledState;
    digitalWrite(pinLed, ledState);
  }
  // 沒有任何 delay()，可以同時處理其他邏輯
}
```

#### 範例 1-2：把「開門後等待再關門」重構成非阻塞式
```cpp
// ============================================
// 範例 1-2：伺服馬達開門動作，不再用 delay() 卡住整個系統
// 原本 v1.0 寫法：gateServo.write(90); delay(3000); gateServo.write(0);
// 這裡改用狀態變數 + millis() 追蹤「門開啟後經過了多久」
// ============================================

#include <Servo.h>

Servo gateServo;
int pinButton = 4;
int lastButtonState = LOW;

bool gateOpen = false;
unsigned long gateOpenTime = 0;
long gateHoldDuration = 3000;

void setup() {
  gateServo.attach(6);
  pinMode(pinButton, INPUT);
  gateServo.write(0);
}

void loop() {
  int currentState = digitalRead(pinButton);
  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);   // 按鈕防彈跳，這個極短暫的 delay 可以接受
    gateOpen = true;
    gateOpenTime = millis();   // 記錄開門的時間點
    gateServo.write(90);
  }
  lastButtonState = currentState;

  // 非阻塞式判斷：門開啟後是否已經過了指定的維持時間
  if (gateOpen && (millis() - gateOpenTime >= gateHoldDuration)) {
    gateServo.write(0);
    gateOpen = false;
  }

  // 這裡可以繼續執行其他邏輯，不會被開門動作卡住！
}
```
**教學重點**：這是這學期最重要的架構升級——`gateOpenTime` 記住「事件發生的時間點」，接著在每一輪 `loop()` 中持續檢查「距離事件發生過了多久」，一旦超過設定時間就執行後續動作。這種寫法讓「等待」這件事，從「卡住整個程式」變成「一個會被持續檢查的條件」，這正是真正專業的嵌入式系統程式設計方式。

#### 範例 1-3：多個非阻塞計時器並存
```cpp
// ============================================
// 範例 1-3：同時管理「開門計時」與「警報閃爍計時」兩個獨立的時間軸
// ============================================

#include <Servo.h>

Servo gateServo;
int pinAlarm = 9;

bool gateOpen = false;
unsigned long gateOpenTime = 0;
long gateHoldDuration = 3000;

unsigned long lastBlinkTime = 0;
long blinkInterval = 200;
bool alarmActive = true;
bool blinkState = false;

void setup() {
  gateServo.attach(6);
  pinMode(pinAlarm, OUTPUT);
  gateServo.write(0);
}

void loop() {
  unsigned long now = millis();

  // --- 時間軸 A：開門計時 ---
  if (gateOpen && (now - gateOpenTime >= gateHoldDuration)) {
    gateServo.write(0);
    gateOpen = false;
  }

  // --- 時間軸 B：警報燈閃爍計時（完全獨立，互不干擾） ---
  if (alarmActive && (now - lastBlinkTime >= blinkInterval)) {
    lastBlinkTime = now;
    blinkState = !blinkState;
    digitalWrite(pinAlarm, blinkState);
  }
}
```
**教學重點**：這裡示範了 `millis()` 架構最強大的特性——**可以同時管理任意多組獨立的「時間軸」，彼此完全不會互相干擾**。如果用 `delay()` 寫法，光是要讓兩個不同節奏的動作同時進行就幾乎不可能做到，這正是為什麼專業的嵌入式系統開發幾乎不會依賴 `delay()` 的根本原因。

---

### 升級 2：多階段關卡進度系統

**為什麼要做這件事**：真正的密室逃脫不會只有單一任務，而是一連串環環相扣的關卡。這一關我們用狀態機的概念，設計清楚的多階段進度追蹤。

#### 範例 2-1：用 enum 定義清楚的關卡狀態
```cpp
// ============================================
// 範例 2-1：用 enum 取代多個零散的布林變數，讓狀態管理更清晰
// ============================================

enum GameStage {
  STAGE_INTRO,       // 開場介紹
  STAGE_PUZZLE_1,    // 第一道謎題
  STAGE_PUZZLE_2,    // 第二道謎題
  STAGE_FINAL,        // 最終挑戰
  STAGE_COMPLETE      // 全部完成
};

GameStage currentStage = STAGE_INTRO;

void setup() {
  Serial.begin(9600);
}

void loop() {
  switch (currentStage) {
    case STAGE_INTRO:
      Serial.println("階段：開場介紹");
      break;
    case STAGE_PUZZLE_1:
      Serial.println("階段：第一道謎題");
      break;
    case STAGE_PUZZLE_2:
      Serial.println("階段：第二道謎題");
      break;
    case STAGE_FINAL:
      Serial.println("階段：最終挑戰");
      break;
    case STAGE_COMPLETE:
      Serial.println("階段：全部完成！");
      break;
  }
  delay(1000);
}
```
**新語法提醒**：`enum`（列舉型別）讓你可以用「有意義的名字」代表一組相關的狀態，取代原本用 0、1、2、3 這種數字代表狀態的寫法。程式碼讀起來會清楚很多——`currentStage == STAGE_PUZZLE_1` 遠比 `currentStage == 1` 容易理解。

#### 範例 2-2：階段轉換邏輯
```cpp
// ============================================
// 範例 2-2：依據條件，讓關卡狀態往下一階段推進
// ============================================

enum GameStage {
  STAGE_INTRO, STAGE_PUZZLE_1, STAGE_PUZZLE_2, STAGE_FINAL, STAGE_COMPLETE
};

GameStage currentStage = STAGE_INTRO;
int pinButton = 4;
int lastButtonState = LOW;

void advanceStage() {
  if (currentStage == STAGE_INTRO) {
    currentStage = STAGE_PUZZLE_1;
  } else if (currentStage == STAGE_PUZZLE_1) {
    currentStage = STAGE_PUZZLE_2;
  } else if (currentStage == STAGE_PUZZLE_2) {
    currentStage = STAGE_FINAL;
  } else if (currentStage == STAGE_FINAL) {
    currentStage = STAGE_COMPLETE;
  }
  Serial.print("進入階段：");
  Serial.println(currentStage);
}

void setup() {
  pinMode(pinButton, INPUT);
  Serial.begin(9600);
}

void loop() {
  int currentState = digitalRead(pinButton);
  if (currentState == HIGH && lastButtonState == LOW) {
    delay(50);
    advanceStage();
  }
  lastButtonState = currentState;
}
```

#### 範例 2-3：不同階段各自對應不同的機關邏輯
```cpp
// ============================================
// 範例 2-3：每個階段呼叫各自對應的子函式，維持程式碼整潔
// ============================================

enum GameStage {
  STAGE_INTRO, STAGE_PUZZLE_1, STAGE_PUZZLE_2, STAGE_FINAL, STAGE_COMPLETE
};

GameStage currentStage = STAGE_INTRO;
int pinR = 9, pinG = 10, pinB = 11;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void runIntroStage() {
  setColor(255, 255, 255);   // 開場：白色
}

void runPuzzle1Stage() {
  setColor(255, 255, 0);     // 第一題：黃色
}

void runPuzzle2Stage() {
  setColor(255, 165, 0);     // 第二題：橘色
}

void runFinalStage() {
  setColor(255, 0, 0);       // 最終挑戰：紅色
}

void runCompleteStage() {
  setColor(0, 255, 0);       // 完成：綠色
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  switch (currentStage) {
    case STAGE_INTRO:    runIntroStage();    break;
    case STAGE_PUZZLE_1: runPuzzle1Stage();  break;
    case STAGE_PUZZLE_2: runPuzzle2Stage();  break;
    case STAGE_FINAL:    runFinalStage();    break;
    case STAGE_COMPLETE: runCompleteStage(); break;
  }
}
```
**教學重點**：這種「每個狀態各自對應一個處理函式，`loop()` 用 `switch` 分派」的架構，稱為**狀態機模式（State Machine Pattern）**，是這學期程式架構設計的集大成——回顧第 8 週的模組化、第 13 週的查表法、第 14 週的優先權設計，全部都在這個模式中匯流。

---

### 升級 3：極限壓力測試與除錯

**為什麼要做這件事**：複習第 8 週學過的 QA 品質保證精神，這一關我們把測試規模擴大到整個終極系統。

#### 壓力測試檢核清單範本
```cpp
// ============================================
// 範例 3-1：內建「自我測試模式」的程式碼片段
// 開機時可選擇進入測試模式，快速驗證所有子系統
// ============================================

int pinTestButton = 5;   // 開機時按住此按鈕進入測試模式

void runSelfTest() {
  Serial.println("=== 開始自我測試 ===");

  Serial.println("測試 1：LED 全亮測試");
  // 此處填入實際測試邏輯

  Serial.println("測試 2：伺服馬達全範圍測試");
  // 此處填入實際測試邏輯

  Serial.println("測試 3：感測器讀值合理性測試");
  // 此處填入實際測試邏輯

  Serial.println("=== 自我測試完成 ===");
}

void setup() {
  pinMode(pinTestButton, INPUT);
  Serial.begin(9600);

  if (digitalRead(pinTestButton) == HIGH) {
    runSelfTest();
  }
}

void loop() {
}
```
**教學重點**：「開機自我測試（Power-On Self Test, POST）」是真實電腦與工業設備開機時都會執行的標準流程，這裡示範了如何在你的專題中加入類似的概念——透過按住某個按鈕進入特殊的測試模式，快速驗證系統各部分是否正常，而不用每次都手動逐一操作測試。

#### 壓力測試情境清單（請逐一測試你自己的系統）

| 測試情境 | 預期結果 | 你的測試結果 |
|---|---|---|
| 同時觸發兩個以上的安全連鎖條件 | 系統應完全鎖定，且不因先後順序而出現矛盾行為 | |
| 快速連續按壓所有按鈕 | 不應出現計數異常或狀態錯亂 | |
| 感測器數值拉到極端值（最大/最小） | 系統不應當機或出現不合理的輸出 | |
| 在關鍵動作進行中（如開門）觸發緊急停止 | 應立刻優先執行緊急停止，中斷原本動作 | |
| 讓系統連續運作至少 5 分鐘不中斷 | 不應出現記憶體問題或行為漂移（Arduino 較少見，但仍建議測試） |
| 跳過某個關卡直接嘗試後面的機關 | 系統應能合理拒絕，或至少不出現無法預期的錯誤 | |

---

### 升級 4：使用者體驗優化

**為什麼要做這件事**：功能正確只是及格，真正優秀的作品需要在細節上展現用心。

#### 範例 4-1：開機迎賓動畫
```cpp
// ============================================
// 範例 4-1：開機時播放一小段歡迎動畫與音效
// ============================================

#include <LiquidCrystal.h>

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
int pinBuzzer = 13;

void showWelcomeAnimation() {
  lcd.clear();
  lcd.print("Initializing");
  for (int i = 0; i < 3; i++) {
    delay(400);
    lcd.print(".");
  }
  delay(500);

  lcd.clear();
  lcd.print("Welcome to the");
  lcd.setCursor(0, 1);
  lcd.print("Ultimate Vault!");

  tone(pinBuzzer, 523, 150); delay(200);
  tone(pinBuzzer, 659, 150); delay(200);
  tone(pinBuzzer, 784, 300); delay(400);

  delay(1500);
  lcd.clear();
}

void setup() {
  lcd.begin(16, 2);
  showWelcomeAnimation();
}

void loop() {
}
```

#### 範例 4-2：操作反饋一致性設計
```cpp
// ============================================
// 範例 4-2：設計一套一致的聲音語彙，讓玩家憑聲音就能理解結果
// ============================================

int pinBuzzer = 13;

// 統一的反饋音效函式庫，往後所有子系統都呼叫這些函式，確保體驗一致
void playSuccessSound() {
  tone(pinBuzzer, 1200, 150);
}

void playErrorSound() {
  tone(pinBuzzer, 300, 300);
}

void playProgressSound() {
  tone(pinBuzzer, 800, 100);
}

void playAlarmSound() {
  tone(pinBuzzer, 2000, 100);
}

void setup() {
}

void loop() {
  playSuccessSound();
  delay(1000);
}
```
**教學重點**：這個範例的重點不在程式碼技巧，而在設計思維——**把所有音效反饋統一封裝成語意清楚的函式**，確保系統中每一處「成功」都用同一種音效、每一處「錯誤」都用同一種音效，這種一致性正是專業產品使用者體驗設計的核心原則之一。

---

### 🏆 最終整合：終極密室系統 最終完整版

**任務**：把升級 1~4 的技巧，套用到你上週完成的 v1.0 系統中。這裡提供一份「整合骨架」，展示最終版本大致的程式碼架構應該長什麼樣子（實際內容仍需依照你自己的規劃填入）：

```cpp
// ============================================
// 終極密室系統 最終完整版 - 架構骨架
// 整合：millis() 狀態機 + 多階段進度 + 完整安全連鎖 + UX 優化
// ============================================

#include <LiquidCrystal.h>
#include <Servo.h>

// --- 關卡狀態定義 ---
enum GameStage { STAGE_INTRO, STAGE_PUZZLE_1, STAGE_PUZZLE_2, STAGE_FINAL, STAGE_COMPLETE };
GameStage currentStage = STAGE_INTRO;

// --- 腳位與物件宣告（請依你的規劃填入實際內容）---
LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
Servo gateServo;
int pinEmergency = 4;
int pinBuzzer = 13;

// --- 非阻塞計時變數 ---
unsigned long gateOpenTime = 0;
bool gateOpen = false;
long gateHoldDuration = 3000;

// --- 安全連鎖狀態 ---
bool safetyLocked = false;

// --- 工具函式庫 ---
void playSuccessSound() { tone(pinBuzzer, 1200, 150); }
void playErrorSound()   { tone(pinBuzzer, 300, 300); }
void playAlarmSound()   { tone(pinBuzzer, 2000, 100); }

void showWelcomeAnimation() {
  lcd.clear();
  lcd.print("Welcome!");
  delay(1500);
  lcd.clear();
}

// --- 安全連鎖檢查（最高優先權）---
bool checkSafety() {
  bool emergencyActive = digitalRead(pinEmergency) == HIGH;
  if (emergencyActive) {
    safetyLocked = true;
  }
  return safetyLocked;
}

// --- 各階段處理函式（請依你的規劃填入實際機關邏輯）---
void runIntroStage()    { /* 待實作 */ }
void runPuzzle1Stage()  { /* 待實作 */ }
void runPuzzle2Stage()  { /* 待實作 */ }
void runFinalStage()    { /* 待實作 */ }
void runCompleteStage() { /* 待實作 */ }

// --- 非阻塞式開門動作 ---
void updateGate() {
  if (gateOpen && (millis() - gateOpenTime >= gateHoldDuration)) {
    gateServo.write(0);
    gateOpen = false;
  }
}

void setup() {
  lcd.begin(16, 2);
  gateServo.attach(6);
  pinMode(pinEmergency, INPUT);
  Serial.begin(9600);
  showWelcomeAnimation();
}

void loop() {
  // 優先權①：安全連鎖
  if (checkSafety()) {
    lcd.setCursor(0, 0);
    lcd.print("SYSTEM LOCKED   ");
    playAlarmSound();
    delay(200);
    return;
  }

  // 優先權②：非阻塞式動作更新（永遠要執行，不受關卡影響）
  updateGate();

  // 優先權③：依目前關卡階段執行對應邏輯
  switch (currentStage) {
    case STAGE_INTRO:    runIntroStage();    break;
    case STAGE_PUZZLE_1: runPuzzle1Stage();  break;
    case STAGE_PUZZLE_2: runPuzzle2Stage();  break;
    case STAGE_FINAL:    runFinalStage();    break;
    case STAGE_COMPLETE: runCompleteStage(); break;
  }
}
```
**這份骨架就是你這學期程式設計能力的畢業證書**——它包含了：`enum` 狀態機、非阻塞式計時、安全連鎖優先權、模組化函式庫、統一的使用者體驗設計。請把你自己的機關邏輯，依照這個架構逐步填入 `待實作` 的部分，完成屬於你自己的終極密室系統。

### 完整範例：藍圖 A（智慧保全金庫）關鍵片段實作

如果你選用的是第 15 週的藍圖 A，這裡提供 `runPuzzle1Stage()` 等函式的具體實作範例，示範如何把骨架中的「待實作」部分填入真正的機關邏輯：

```cpp
// ============================================
// 藍圖 A 範例：第一道謎題 - 光敏電阻雷射網防線
// ============================================

int pinLdr = A1;
int lightThreshold = 300;

void runPuzzle1Stage() {
  lcd.setCursor(0, 0);
  lcd.print("Stage 1: Laser  ");

  int lightLevel = analogRead(pinLdr);

  if (lightLevel < lightThreshold) {
    lcd.setCursor(0, 1);
    lcd.print("BREACH DETECTED!");
    playErrorSound();
  } else {
    lcd.setCursor(0, 1);
    lcd.print("Path is clear...");

    // 假設玩家需要維持「未觸發」狀態達 3 秒，才算通過這道防線
    static unsigned long clearStartTime = 0;
    static bool wasBreached = true;

    if (wasBreached) {
      clearStartTime = millis();
      wasBreached = false;
    }

    if (millis() - clearStartTime >= 3000) {
      playSuccessSound();
      currentStage = STAGE_PUZZLE_2;
    }
  }
}
```
**新語法提醒**：這裡第一次出現 `static` 關鍵字——`static` 讓一個在函式內部宣告的變數，「記住」上一次呼叫時的數值，而不會每次呼叫函式都重新歸零。這在只想讓某個變數的作用範圍侷限在單一函式內、卻又需要跨次呼叫保留數值的情境下非常好用，是這學期最後一個值得認識的重要語法技巧。

```cpp
// ============================================
// 藍圖 A 範例：最終階段 - 矩陣鍵盤密碼驗證後開啟金庫
// ============================================

String inputCode = "";
String correctPassword = "1234";

void runFinalStage() {
  lcd.setCursor(0, 0);
  lcd.print("Enter Password: ");

  char key = keypad.getKey();   // 假設 keypad 物件已在骨架中宣告

  if (key) {
    if (key >= '0' && key <= '9' && inputCode.length() < 4) {
      inputCode = inputCode + key;
      lcd.setCursor(0, 1);
      lcd.print(inputCode);
    }

    if (inputCode.length() == 4) {
      delay(300);
      if (inputCode == correctPassword) {
        playSuccessSound();
        gateServo.write(90);
        gateOpen = true;
        gateOpenTime = millis();
        currentStage = STAGE_COMPLETE;
      } else {
        playErrorSound();
      }
      inputCode = "";
      lcd.clear();
    }
  }
}
```

---

## QA 品質保證：完整驗收檢核表

在正式展演前，請對照以下檢核表逐一確認：

### 功能完整性
- [ ] 所有子系統（安全連鎖、感測顯示、動力互動、計分記錄）都已整合進最終版本
- [ ] 至少 3 個關卡階段都有實際內容，而非空白函式
- [ ] 沒有任何會卡住整個系統超過 1 秒的 `delay()`（緊急防彈跳用的極短 `delay(50)` 可接受）

### 穩定性
- [ ] 完成升級 3 的所有壓力測試情境，且系統表現符合預期
- [ ] 連續操作 5 分鐘以上沒有出現異常行為

### 使用者體驗
- [ ] 有開機迎賓畫面或音效
- [ ] 成功、錯誤、警報三種狀態，各自有清楚且一致的聲光反饋
- [ ] 玩家不需要額外口頭說明，就能從畫面/燈光/聲音理解目前系統狀態

### 程式碼品質
- [ ] 所有變數與函式命名清楚，符合這學期一路要求的「工業邏輯語言」註解規範
- [ ] `loop()` 保持精簡，主要邏輯都封裝在各自的函式中

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| 重構成 millis() 後，某個動作完全不會觸發 | 忘記在事件發生時記錄 `xxxTime = millis();` | 檢查每個非阻塞計時器，事件觸發瞬間是否有更新對應的時間戳記變數 |
| 兩個 millis() 計時器互相干擾 | 誤用了同一個變數名稱記錄不同用途的時間 | 確認每組獨立的計時邏輯，都有各自專屬、不重複的變數名稱 |
| `switch` 語句中某個 `case` 沒有執行預期行為 | 忘記寫 `break;` 導致程式繼續往下一個 `case` 執行（穿透現象） | 檢查每個 `case` 結�uture是否都有 `break;`（除非你是刻意設計穿透邏輯） |
| 關卡永遠停留在同一階段 | 關卡推進的觸發條件沒有正確連接到實際的玩家操作 | 檢查 `advanceStage()` 或類似函式是否真的被對應的感測/按鈕事件呼叫到 |
| 壓力測試時系統出現「卡死」現象 | 仍然殘留某處忘記移除的長時間 `delay()` | 搜尋整份程式碼中所有 `delay()`，逐一確認是否都是必要且極短暫的 |

---

## 練習題大補帖

### 🟢 基礎題
1. 把你上週 v1.0 系統中，至少一處使用 `delay()` 等待的邏輯，改寫成 `millis()` 非阻塞式寫法。
2. 用 `enum` 重新定義你自己系統的關卡階段名稱。
3. 為你的系統加上開機迎賓動畫（可參考範例 4-1 修改）。
4. 統一你系統中所有的音效反饋，確保「成功」與「錯誤」各自使用一致的音效。
5. 完成 QA 驗收檢核表中的「功能完整性」與「使用者體驗」兩個區塊。

### 🟡 中等題
6. 設計一個關卡階段的「提示系統」：玩家在同一階段停留超過一段時間（用 `millis()` 判斷）沒有進度，系統主動給予提示。
7. 幫你的系統加上「重新開始」功能：在特定條件下（如按住某按鈕 3 秒），系統能重置回 `STAGE_INTRO`。
8. 設計至少 2 個你自己想到的壓力測試情境（不在本週提供的清單內），並實際測試你的系統。
9. 幫你的系統的每個關卡階段，設計專屬的 RGB 顏色或 LCD 文字提示，讓玩家能清楚辨識目前進度。
10. 完成 QA 驗收檢核表全部項目，並針對未通過的項目進行修正。

### 🔴 挑戰題
11. **巢狀狀態機設計**：如果某個關卡階段（例如 STAGE_PUZZLE_1）內部又需要有自己的子階段（例如「輸入中」、「驗證中」、「已通過」），你會如何設計資料結構來管理這種「狀態中還有狀態」的情況？
12. **關卡計時競速功能**：為每個關卡階段加上獨立的計時器，記錄玩家完成每個階段花費的時間，並在最終顯示「總完成時間」與「各階段分別花費時間」的統計報告。
13. **完整事件日誌系統**：設計一個字串陣列，記錄系統運作過程中的關鍵事件（進入新階段、觸發安全連鎖、任務完成等），並能在 Serial Monitor 印出完整的事件時間軸。
14. **動態難度調整**：設計一個機制，如果偵測到玩家在某階段多次失敗（如多次密碼輸入錯誤），系統自動給予額外提示或放寬某些判定條件，模擬遊戲界常見的「動態難度平衡」設計。
15. **完整最終作品**：把你的終極密室系統開發到你認為「可以自信地向密室老闆展示」的完成度，並確保通過 QA 驗收檢核表的所有項目。

---

## 🎓 期末成果展演指南

### 展演流程建議（每組 8-10 分鐘）
1. **情境簡報**（1-2 分鐘）：說明你的密室情境設定、玩家會經歷的機關流程
2. **實機演示**（3-4 分鐘）：完整走一遍正常流程，展示各關卡與最終開鎖
3. **刁難測試**（2-3 分鐘）：邀請老師或同學現場測試至少 2 個壓力測試情境
4. **技術亮點分享**（1-2 分鐘）：分享你認為自己作品中「最值得驕傲」的一個技術細節

### 評分項目建議
| 評分項目 | 權重 |
|---|---|
| 功能完整性（是否涵蓋多元件、多階段） | 25% |
| 系統穩定性（QA 測試表現） | 20% |
| 程式碼品質（架構、命名、註解） | 20% |
| 使用者體驗（聲光反饋、流暢度） | 15% |
| 情境創意與故事完整度 | 10% |
| 口頭表達與技術理解 | 10% |

### 刁難測試題庫參考（供老師或同儕評審使用）
- 「如果我同時觸發你的緊急停止跟安全連鎖，系統會怎麼反應？」
- 「你的計分系統，滿分之後會發生什麼事？」
- 「如果我故意跳過中間關卡，直接想觸發最終機關，你的系統擋得住嗎？」
- 「你程式碼裡還有沒有殘留的 `delay()`？它們分別在做什麼？」
- 「這套系統如果要擴充成兩間密室協同運作，你會怎麼改？」

---

## 🎨 給有餘力的你：延伸挑戰

如果你已經完成所有必要項目還有餘力，這裡提供幾個學期結束後可以繼續鑽研的方向：

> **延伸挑戰 A：資料持久化**
> Arduino 有內建的 EEPROM（斷電後仍能保存資料的記憶體），研究看看如何用它記錄「歷史最佳完成時間」，即使重新開機也不會遺失。

> **延伸挑戰 B：無線互動**
> 研究 Arduino 搭配藍牙或 Wi-Fi 模組，讓手機能即時查看密室的運作狀態，甚至遠端解鎖某些機關。

> **延伸挑戰 C：多台 Arduino 協同**
> 研究 Arduino 之間如何透過 I2C 或序列埠通訊，讓多間密室房間（多塊 Arduino 板）能互相傳遞「這一間已通關」的訊息，觸發下一間房間的機關。

這些延伸方向已經超出這學期的正式課程範圍，但如果你對物聯網或嵌入式系統有興趣，這些都是很好的自學起點。

> 💬 **給老師與同學的話**：這學期從電阻與串並聯的基礎複習開始，一路走到今天具備狀態機與非阻塞式架構的完整系統，是一段扎實的旅程。無論你未來是否會繼續深入電子與程式領域，這學期培養的「先規劃、模組化開發、系統化測試」思維，都會是你在任何工程與管理領域受用一生的能力。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| 狀態機（State Machine） | 狀態機 | 用一組明確定義的狀態管理系統行為的設計模式 |
| `enum` | 列舉型別 | 用有意義的名字定義一組相關狀態的語法 |
| 非阻塞式（Non-blocking） | 非阻塞式 | 程式不會被單一動作「卡住」，能同時處理多項任務 |
| 開機自我測試（POST） | 開機自我測試 | 系統啟動時自動驗證各部件是否正常的標準流程 |
| 壓力測試（Stress Testing） | 壓力測試 | 用極端情境測試系統穩定性的品質保證手法 |
| 使用者體驗（UX） | 使用者體驗 | 使用者操作系統時的整體感受與流暢度 |

---

## 反思與延伸討論

1. 從 `delay()` 架構重構到 `millis()` 非阻塞式狀態機，是這學期最後、也是最重要的一次程式思維躍進。請回顧：這個轉變讓你對「時間」在程式設計中的角色，有了什麼新的理解？
2. 本週的狀態機設計（`enum` + `switch`），整合了這學期學過的模組化、查表法、優先權排序等多個概念。請討論：如果請你向一個完全沒學過程式的人解釋「狀態機」，你會怎麼用生活中的例子比喻？
3. 回顧這整個學期，從第 1 週最簡單的 LED 閃爍，到這週具備多階段狀態機、非阻塞式計時、完整安全連鎖的終極密室系統。請寫下至少 3 句話，總結你這學期最大的收穫或轉變。
4. `static` 關鍵字讓函式內的變數能「記住」跨次呼叫的數值，這跟這學期一路使用的「全域變數」在效果上有些類似，但作用範圍更侷限。請討論：什麼情況下你會傾向使用 `static` 區域變數，而不是乾脆宣告一個全域變數？兩者在程式碼的「可維護性」上有什麼差異？

---

## 學期總回顧與本週檢核清單

### 本週檢核清單
- [ ] 完成至少一處 `delay()` 到 `millis()` 的重構
- [ ] 用 `enum` 設計並實作多階段關卡系統
- [ ] 完成 QA 完整驗收檢核表所有項目
- [ ] 加入開機迎賓動畫與統一的聲光反饋設計
- [ ] 完成終極密室系統最終完整版，並準備好期末成果展演
- [ ] 完成至少 8 題基礎/中等練習題

### 16 週學期總回顧
恭喜你完成這學期完整的 16 週 Arduino / Tinkercad 課程！從第一週學會讓一顆 LED 閃爍，到這週完成一套具備多元件整合、狀態機架構、安全連鎖設計的完整系統，這段旅程中你已經具備了：
- 紮實的 C/C++ 程式邏輯基礎（變數、陣列、迴圈、函式、字串處理、位元運算）
- 完整的感測器與致動器操作經驗（超過 15 種不同元件）
- 專業等級的系統設計思維（模組化、優先權排序、狀態機、QA 測試）

這些能力，不只是這門課的終點，更是你未來接觸自動化、物聯網、智慧製造等領域最重要的起點。

恭喜畢業，密室工程師。

### 給密室老闆的一封結案報告（選寫）

如果時間允許，建議每位同學（或每組）額外寫一份簡短的「結案報告」給密室老闆，內容可包含：你的機關情境設計理念、遇到的最大技術挑戰與解法、如果有更多時間你還想加入什麼功能。這份報告不計入正式評分，但能幫助你更完整地整理這學期的學習軌跡，也是很好的個人作品集素材。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 15 週](./week-15.md)
