[⬅ 回目錄](../README.md) | [⬅ 上一週：第 11 週](./week-11.md) | [下一週：第 13 週 — 產線數字看板升級站 ➡](./week-13.md)

---

# 第 12 週：金庫密碼鎖與 Arduino 單元期末統整
## 主題專案：密室逃脫的最終保險庫，也是整個 Arduino 單元的畢業考

> 🎯 **本週定位**：這是 Arduino 基礎元件學習階段的**最後一週**。密室老闆要求打造整間逃脫房最終的「金庫密碼鎖」機關，玩家必須輸入正確密碼才能取得最終寶物。你會學到 **4x4 矩陣鍵盤**，這是這學期第一次能讓玩家「輸入一組完整的資料」（而不只是按一下按鈕），也是這學期人機互動複雜度最高的一次。本週後半段會進行 12 週以來的**期末統整驗收**，把過去所學的所有元件與技巧做一次全面盤點，為第 13 週開始更進階的工業元件（位移暫存器、繼電器）與最終的終極整合大專題做好完整準備。

---

## 📋 目錄
- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [電路體檢站（3 分鐘快速複習）](#電路體檢站3-分鐘快速複習)
- [模擬設計專案配置](#模擬設計專案配置)
- [後端程式設計：五道機關關卡](#後端程式設計五道機關關卡)
  - [關卡 1：矩陣掃描原理 — 認識 Keypad.h](#關卡-1矩陣掃描原理--認識-keypadh)
  - [關卡 2：字元組合 — 累積輸入成一組密碼](#關卡-2字元組合--累積輸入成一組密碼)
  - [關卡 3：密碼比對與電子鎖邏輯](#關卡-3密碼比對與電子鎖邏輯)
  - [關卡 4：整合 LCD — 完整輸入介面](#關卡-4整合-lcd--完整輸入介面)
  - [關卡 5：進階安全機制 — 錯誤鎖定與設定模式](#關卡-5進階安全機制--錯誤鎖定與設定模式)
  - [🏆 BOSS 關：終極金庫密碼鎖系統](#-boss-關終極金庫密碼鎖系統)
- [常見錯誤除錯站](#常見錯誤除錯站)
- [練習題大補帖](#練習題大補帖)
- [🎓 Arduino 單元期末統整驗收](#-arduino-單元期末統整驗收)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～11 週，整個 Arduino 單元）
- ✅ 數位/類比輸出入、`map()`、`constrain()`、`float` 運算
- ✅ 陣列、迴圈、自訂函式、物件（Servo、LiquidCrystal）
- ✅ LED、RGB LED、按鈕、旋鈕、蜂鳴器、光敏電阻、TMP36
- ✅ 伺服馬達、超音波、直流馬達、LCD 顯示器
- ✅ 模組化系統架構設計、優先權排序、狀態機雛形

### 這週要新學的
| 觀念 | 說明 | 為什麼工業上重要 |
|---|---|---|
| 矩陣鍵盤（Membrane Keypad） | 用行列交錯的方式，以少量腳位讀取大量按鍵 | 密碼輸入、設備參數設定介面的常見元件 |
| `<Keypad.h>` | Arduino 常用的矩陣鍵盤函式庫 | 這學期第三次使用外部函式庫 |
| `String` 字串累加 | 把多個字元依序組合成一個完整字串 | 密碼比對、資料輸入處理的基礎技巧 |
| 錯誤鎖定機制 | 連續輸入錯誤達到次數上限，系統鎖定 | 真實密碼系統防止暴力破解的標準設計 |

### 使用平台
本週繼續使用 [Tinkercad Circuits](https://www.tinkercad.com/)，程式編輯視窗維持「文字模式（Text）」。

---

## 開場任務簡報

> 📖 **情境**：密室逃脫遊戲來到最終章——玩家蒐集了前面所有房間的線索，湊出一組密碼，必須在金庫密碼鎖上正確輸入才能取得寶物、宣告成功逃脫。你被指派設計整套密碼鎖系統，這將是整個 Arduino 單元最終、也是最完整的一個作品。
>
> 這週結束時，你會做出：
> 1. 一個能讀取矩陣鍵盤按鍵的基礎測試機關（暖身）
> 2. 一套能累積多個字元組成密碼的輸入系統
> 3. 一套完整的密碼比對與電子鎖邏輯
> 4. 一套整合 LCD 顯示的完整輸入介面
> 5. 一套具備錯誤鎖定與設定模式的進階安全機制
> 6. 一個整合以上所有技巧的「終極金庫密碼鎖系統」

---

## 電路體檢站（3 分鐘快速複習）

**體檢題 1**：矩陣鍵盤只用 8 隻腳位（4 條行線 + 4 條列線）就能讀取 16 顆按鍵，而不是每顆按鍵都各自接一條線（那樣會需要 16 隻腳位）。回想第 2 週學過的「按鈕電路」——單顆按鈕需要一條訊號線。矩陣鍵盤運用巧妙的「行列交錯掃描」原理，讓少量腳位就能覆蓋大量按鍵組合，這是這學期第一次接觸「用演算法巧思節省硬體資源」的設計思維。

**體檢題 2**：這週會用到第三個外部函式庫 `<Keypad.h>`（前兩個分別是第 7 週的 `<Servo.h>` 與第 11 週的 `<LiquidCrystal.h>`）。回想這學期一路累積的「函式庫使用模式」——先 `#include`、再建立物件、最後透過「物件.功能()」操作，這個固定套路這學期已經是第三次應用，代表你已經具備「自主學習新函式庫」的能力基礎。

**體檢題 3**：矩陣鍵盤需要接電阻保護嗎？
> 答案：不需要，直接把 8 隻腳位接到 Arduino 對應的數位腳位即可，不需要額外的限流或下拉電阻——這跟單顆按鈕電路（需要下拉電阻）不同，因為矩陣掃描的程式邏輯已經處理好了訊號穩定性的問題。

---

## 模擬設計專案配置

### 元件清單
| 元件 | 數量 | 說明 |
|---|---|---|
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1 | 電路佈局 |
| Membrane Keypad 4x4 | 1 | 密碼輸入鍵盤 |
| LCD 16x2 | 1 | 沿用第 11 週，顯示輸入過程 |
| Micro Servo | 1 | 沿用第 7 週，模擬鎖具開關 |
| Buzzer | 1 | 沿用第 5 週，錯誤警示音 |

### 接線步驟

**Part A：矩陣鍵盤**
1. 拖曳一顆 **Membrane Keypad 4x4** 元件到工作區，共有 8 隻接腳（4 條行線 Row + 4 條列線 Column）。
2. 依序接到 Arduino 的 **Pin 2, 3, 4, 5**（列, Row）與 **Pin 6, A0, A1, A2**（行, Column，類比腳位在此可當一般數位腳位使用）。實際腳位對應請依 Tinkercad 元件標示為準，並在程式碼中對應設定。

**Part B：LCD（沿用第 11 週，腳位需調整）**
1. 因矩陣鍵盤佔用了大量腳位，本週 LCD 改接：**RS=7, E=8, D4=9, D5=10, D6=11, D7=12**。

**Part C：伺服馬達**
1. 訊號線接 **Pin 13**。

**Part D：蜂鳴器**
1. 正極接 **A3**（本週腳位非常緊繃，改用類比腳位當一般數位輸出使用）。

> ⚠️ **教學提醒**：本週腳位使用量是整個 Arduino 單元最大的一次，強烈建議提供「已完成接線」的 Tinkercad 範本電路，讓學生專注在程式邏輯與鍵盤 + LCD 的互動設計，而非反覆除錯接線問題。

### 腳位對照表（本週共用）
| 元件 | 腳位 |
|---|---|
| 矩陣鍵盤 Row 1~4 | Pin 2, 3, 4, 5 |
| 矩陣鍵盤 Col 1~4 | Pin 6, A0, A1, A2 |
| LCD RS / E / D4 / D5 / D6 / D7 | 7 / 8 / 9 / 10 / 11 / 12 |
| 伺服馬達訊號線 | Pin 13 |
| 蜂鳴器 | A3 |

> 💡 **配置檢查清單**
> - [ ] 矩陣鍵盤 8 隻腳全部正確接線
> - [ ] LCD 依本週新腳位配置正確接線
> - [ ] 伺服馬達與蜂鳴器均已正確接線

---

## 後端程式設計：五道機關關卡

### 關卡 1：矩陣掃描原理 — 認識 Keypad.h

**機關故事**：在設計完整密碼系統前，你得先確認鍵盤真的能正確讀出每一顆按鍵。

#### 範例 1-1：讀取按鍵字元並印到 Serial
```cpp
// ============================================
// 範例 1-1：矩陣鍵盤基礎讀取
// ============================================

#include <Keypad.h>

const byte ROWS = 4;
const byte COLS = 4;

// 定義 4x4 鍵盤的按鍵配置
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

byte rowPins[ROWS] = {2, 3, 4, 5};      // 連接 4 條列線的腳位
byte colPins[COLS] = {6, A0, A1, A2};   // 連接 4 條行線的腳位

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

void setup() {
  Serial.begin(9600);
}

void loop() {
  char key = keypad.getKey();   // 讀取目前被按下的按鍵，若無按鍵則回傳空值

  if (key) {   // 如果有讀到按鍵（非空值）
    Serial.println(key);
  }
}
```
**新語法提醒**：`char keys[ROWS][COLS]` 是一個二維字元陣列（複習自第 3 週學過的二維陣列概念，只是這次存的是字元而非數字），用來定義每個按鍵位置對應的實際字元。`Keypad keypad = Keypad(...)` 則是建立一個鍵盤物件，這學期第三次用到「先建立物件，再操作」的固定套路。

#### 範例 1-2：計算按鍵次數
```cpp
// ============================================
// 範例 1-2：統計總共按了幾次按鍵
// ============================================

#include <Keypad.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

int pressCount = 0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    pressCount++;
    Serial.print("按下: ");
    Serial.print(key);
    Serial.print("  累計次數: ");
    Serial.println(pressCount);
  }
}
```

#### 範例 1-3：依按鍵類型做不同反應
```cpp
// ============================================
// 範例 1-3：分辨數字鍵與功能鍵（A/B/C/D/*/#）
// ============================================

#include <Keypad.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

void setup() {
  Serial.begin(9600);
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    if (key >= '0' && key <= '9') {
      Serial.println("這是數字鍵");
    } else if (key == '*' || key == '#') {
      Serial.println("這是特殊符號鍵");
    } else {
      Serial.println("這是功能鍵（A/B/C/D）");
    }
  }
}
```

---

### 關卡 2：字元組合 — 累積輸入成一組密碼

**機關故事**：單一字元太簡單，真正的密碼需要輸入「一組」數字才能驗證。

#### 範例 2-1：組合輸入 4 個字元
```cpp
// ============================================
// 範例 2-1：連續輸入 4 個字元並儲存於字串
// ============================================

#include <Keypad.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

String inputCode = "";   // 用來累積使用者輸入的字元

void setup() {
  Serial.begin(9600);
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    inputCode = inputCode + key;   // 把新字元接在字串後面

    if (inputCode.length() == 4) {   // 累積滿 4 位數
      Serial.print("你輸入的密碼是：");
      Serial.println(inputCode);
      inputCode = "";   // 清空，準備下一次輸入
    }
  }
}
```

#### 範例 2-2：允許退格刪除（結合 D 鍵）
```cpp
// ============================================
// 範例 2-2：按下 D 鍵可以刪除最後輸入的一個字元
// ============================================

#include <Keypad.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

String inputCode = "";

void setup() {
  Serial.begin(9600);
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    if (key == 'D') {
      // 刪除最後一個字元：取字串從第 0 個到「長度減 1」的部分
      if (inputCode.length() > 0) {
        inputCode = inputCode.substring(0, inputCode.length() - 1);
      }
      Serial.print("刪除後: ");
      Serial.println(inputCode);
    } else if (key >= '0' && key <= '9') {
      inputCode = inputCode + key;
      Serial.print("目前輸入: ");
      Serial.println(inputCode);
    }
  }
}
```

---

### 關卡 3：密碼比對與電子鎖邏輯

**機關故事**：正式進入密碼鎖的核心邏輯——比對輸入是否正確，並觸發對應動作。

#### 範例 3-1：密碼比對邏輯
```cpp
// ============================================
// 範例 3-1：檢查輸入密碼是否正確
// ============================================

#include <Keypad.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

String inputCode = "";
String correctPassword = "1234";

void setup() {
  Serial.begin(9600);
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    inputCode = inputCode + key;

    if (inputCode.length() == 4) {
      if (inputCode == correctPassword) {
        Serial.println("密碼正確！");
      } else {
        Serial.println("密碼錯誤！");
      }
      inputCode = "";
    }
  }
}
```

#### 範例 3-2：電子密碼鎖（伺服馬達開門）
```cpp
// ============================================
// 範例 3-2：完整電子密碼鎖 - 正確開門，錯誤響蜂鳴器
// ============================================

#include <Keypad.h>
#include <Servo.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

Servo lockServo;
int pinBuzzer = A3;
String inputCode = "";
String correctPassword = "1234";

void setup() {
  lockServo.attach(13);
  lockServo.write(0);   // 初始：鎖定
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    inputCode = inputCode + key;

    if (inputCode.length() == 4) {
      if (inputCode == correctPassword) {
        lockServo.write(90);   // 開門
        delay(3000);
        lockServo.write(0);    // 自動再次上鎖
      } else {
        tone(pinBuzzer, 1500);
        delay(500);
        noTone(pinBuzzer);
      }
      inputCode = "";
    }
  }
}
```

---

### 關卡 4：整合 LCD — 完整輸入介面

**機關故事**：純粹靠猜測輸入密碼太困難，需要在 LCD 上即時顯示輸入過程，讓玩家能確認自己按了哪些鍵。

#### 範例 4-1：即時顯示輸入的密碼
```cpp
// ============================================
// 範例 4-1：結合 LCD - 玩家按下的字元即時顯示在螢幕上
// ============================================

#include <Keypad.h>
#include <LiquidCrystal.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
String inputCode = "";

void setup() {
  lcd.begin(16, 2);
  lcd.print("Enter Password:");
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    inputCode = inputCode + key;
    lcd.setCursor(0, 1);
    lcd.print(inputCode);
  }
}
```

#### 範例 4-2：完整密碼鎖 + LCD 回饋訊息
```cpp
// ============================================
// 範例 4-2：輸入滿 4 碼後，LCD 顯示驗證結果
// ============================================

#include <Keypad.h>
#include <LiquidCrystal.h>
#include <Servo.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
Servo lockServo;
int pinBuzzer = A3;
String inputCode = "";
String correctPassword = "1234";

void setup() {
  lcd.begin(16, 2);
  lcd.print("Enter Password:");
  lockServo.attach(13);
  lockServo.write(0);
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    inputCode = inputCode + key;
    lcd.setCursor(0, 1);
    lcd.print(inputCode);
    lcd.print("    ");

    if (inputCode.length() == 4) {
      delay(300);
      lcd.clear();

      if (inputCode == correctPassword) {
        lcd.print("Access Granted!");
        lockServo.write(90);
        delay(3000);
        lockServo.write(0);
      } else {
        lcd.print("Access Denied!");
        tone(pinBuzzer, 1500);
        delay(500);
        noTone(pinBuzzer);
        delay(1500);
      }

      lcd.clear();
      lcd.print("Enter Password:");
      inputCode = "";
    }
  }
}
```

---

### 關卡 5：進階安全機制 — 錯誤鎖定與設定模式

**機關故事**：真正的密碼系統需要防止暴力猜測，這一關我們加入錯誤次數鎖定機制，並示範如何讓使用者自己「設定模式」修改密碼參數（呼應第 6 週學過的可調式門檻概念）。

#### 範例 5-1：連續錯誤鎖定機制
```cpp
// ============================================
// 範例 5-1：連續輸入錯誤 3 次，系統鎖定
// ============================================

#include <Keypad.h>
#include <LiquidCrystal.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
String inputCode = "";
String correctPassword = "1234";
int wrongAttempts = 0;
int maxAttempts = 3;
bool systemLocked = false;

void setup() {
  lcd.begin(16, 2);
  lcd.print("Enter Password:");
}

void loop() {
  if (systemLocked) {
    lcd.setCursor(0, 1);
    lcd.print("SYSTEM LOCKED   ");
    return;   // 系統鎖定後，完全不再接受任何按鍵輸入
  }

  char key = keypad.getKey();

  if (key) {
    inputCode = inputCode + key;
    lcd.setCursor(0, 1);
    lcd.print(inputCode);
    lcd.print("    ");

    if (inputCode.length() == 4) {
      if (inputCode == correctPassword) {
        lcd.clear();
        lcd.print("Access Granted!");
        wrongAttempts = 0;
      } else {
        wrongAttempts++;
        lcd.clear();
        lcd.print("Wrong! (");
        lcd.print(wrongAttempts);
        lcd.print("/");
        lcd.print(maxAttempts);
        lcd.print(")");

        if (wrongAttempts >= maxAttempts) {
          systemLocked = true;
        }
        delay(1000);
      }
      inputCode = "";

      if (!systemLocked) {
        lcd.clear();
        lcd.print("Enter Password:");
      }
    }
  }
}
```
**教學重點**：`systemLocked` 一旦被設為 `true`，`loop()` 開頭的 `if (systemLocked) { ...; return; }` 就會讓後續所有邏輯完全不再執行——這是這學期第五次以上使用「優先權 + `return`」的手法（第 6、7、8、11 週都出現過），這正是不斷刻意重複、讓你熟練這個重要設計模式的教學安排。

#### 範例 5-2：星號鍵進入設定模式
```cpp
// ============================================
// 範例 5-2：按 '*' 進入設定模式，可以修改密碼
// ============================================

#include <Keypad.h>
#include <LiquidCrystal.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
String inputCode = "";
String correctPassword = "1234";
bool settingMode = false;
String newPasswordInput = "";

void setup() {
  lcd.begin(16, 2);
  lcd.print("Enter Password:");
}

void loop() {
  char key = keypad.getKey();

  if (key == '*') {
    settingMode = true;
    newPasswordInput = "";
    lcd.clear();
    lcd.print("Set New Pass:");
    return;
  }

  if (settingMode) {
    if (key && key >= '0' && key <= '9') {
      newPasswordInput = newPasswordInput + key;
      lcd.setCursor(0, 1);
      lcd.print(newPasswordInput);

      if (newPasswordInput.length() == 4) {
        correctPassword = newPasswordInput;
        settingMode = false;
        lcd.clear();
        lcd.print("Password Set!");
        delay(1500);
        lcd.clear();
        lcd.print("Enter Password:");
      }
    }
    return;
  }

  // --- 一般密碼驗證模式（省略細節，可參考前面範例） ---
  if (key) {
    inputCode = inputCode + key;
    lcd.setCursor(0, 1);
    lcd.print(inputCode);
  }
}
```

---

### 🏆 BOSS 關：終極金庫密碼鎖系統

**機關故事**：整合本週所有技巧，打造完整的金庫密碼鎖——LCD 輸入介面、密碼比對、錯誤鎖定機制、退格刪除功能，這是整個 Arduino 單元功能最完整的最終作品。

```cpp
// ============================================
// BOSS 關：終極金庫密碼鎖系統 - 整合本週所有技巧
// 功能：LCD 輸入介面 + 退格刪除 + 密碼比對 + 錯誤鎖定 + 伺服馬達開鎖
// ============================================

#include <Keypad.h>
#include <LiquidCrystal.h>
#include <Servo.h>

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, A0, A1, A2};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

LiquidCrystal lcd(7, 8, 9, 10, 11, 12);
Servo lockServo;
int pinBuzzer = A3;

String inputCode = "";
String correctPassword = "1234";
int wrongAttempts = 0;
int maxAttempts = 3;
bool systemLocked = false;

void showEnterPrompt() {
  lcd.clear();
  lcd.print("Enter Password:");
}

void setup() {
  lcd.begin(16, 2);
  lockServo.attach(13);
  lockServo.write(0);
  showEnterPrompt();
}

void loop() {
  // --- 優先權：系統鎖定時，完全停止接受任何操作 ---
  if (systemLocked) {
    lcd.setCursor(0, 1);
    lcd.print("SYSTEM LOCKED   ");
    tone(pinBuzzer, 300);
    delay(400);
    noTone(pinBuzzer);
    delay(400);
    return;
  }

  char key = keypad.getKey();
  if (!key) return;   // 沒有按鍵，直接結束本輪，不用往下執行

  if (key == 'D') {
    // 退格刪除
    if (inputCode.length() > 0) {
      inputCode = inputCode.substring(0, inputCode.length() - 1);
    }
  } else if (key >= '0' && key <= '9') {
    if (inputCode.length() < 4) {
      inputCode = inputCode + key;
    }
  }

  lcd.setCursor(0, 1);
  lcd.print(inputCode);
  lcd.print("    ");

  if (inputCode.length() == 4) {
    delay(300);

    if (inputCode == correctPassword) {
      lcd.clear();
      lcd.print("Access Granted!");
      wrongAttempts = 0;
      tone(pinBuzzer, 1200, 150);
      lockServo.write(90);
      delay(3000);
      lockServo.write(0);
    } else {
      wrongAttempts++;
      lcd.clear();
      lcd.print("Wrong! (");
      lcd.print(wrongAttempts);
      lcd.print("/");
      lcd.print(maxAttempts);
      lcd.print(")");
      tone(pinBuzzer, 400, 150);
      delay(200);
      tone(pinBuzzer, 400, 150);

      if (wrongAttempts >= maxAttempts) {
        systemLocked = true;
      }
      delay(1200);
    }

    inputCode = "";
    if (!systemLocked) {
      showEnterPrompt();
    }
  }
}
```
**教學重點**：這是整個 Arduino 單元的最終作品，綜合運用了 12 週來學過的幾乎所有核心技巧——外部函式庫（Keypad、LCD、Servo）、字串處理、優先權排序（`return`）、聲音與燈光反饋、狀態鎖定機制。如果你能看懂並修改這段程式碼的每一行，代表你已經完整掌握了這學期 Arduino 單元的所有核心能力。

---

## 常見錯誤除錯站

| 現象 | 可能原因 | 解決方式 |
|---|---|---|
| 鍵盤按下沒有任何反應 | 8 隻接腳的行列順序接錯，或 `rowPins[]`/`colPins[]` 陣列數值與實際接線不符 | 逐一核對每隻腳位的行列對應關係 |
| 密碼永遠比對失敗（即使輸入正確） | `String` 比較時不小心含有多餘的空白字元，或 `correctPassword` 打錯 | 確認 `correctPassword` 字串內容與程式碼顯示一致，沒有多餘符號 |
| 退格鍵刪除後出現異常 | `substring()` 在字串長度為 0 時仍被呼叫 | 確認退格邏輯有先判斷 `inputCode.length() > 0` 才執行刪除 |
| 系統鎖定後仍然可以繼續輸入密碼 | `systemLocked` 判斷式忘記加上 `return` | 檢查 `if (systemLocked) { ...; return; }` 是否確實寫在 `loop()` 最前面 |
| LCD 與鍵盤同時使用時出現異常閃爍或當機 | 兩者腳位衝突（誤用了相同腳位） | 逐一核對本週腳位對照表，確認沒有重複使用 |
| 設定新密碼後，舊密碼卻仍然有效 | `correctPassword` 沒有被正確重新賦值 | 檢查設定模式邏輯中是否確實執行 `correctPassword = newPasswordInput;` |

---

## 練習題大補帖

### 🟢 基礎題
1. 修改範例 3-1 的密碼，改成你自己設計的 4 位數字。
2. 修改範例 5-1 的最大錯誤次數，從 3 次改成 5 次。
3. 修改 BOSS 關程式碼的開鎖維持時間，從 3 秒改成 5 秒。
4. 修改範例 4-2，在密碼正確時額外印出 Serial 訊息 "恭喜通關！"。
5. 修改範例 1-3，新增功能：按下 '#' 鍵時額外印出 "已按下確認鍵"。

### 🟡 中等題
6. 設計一個「按 # 提前確認」機制：不用等到輸入滿 4 碼，玩家可以隨時按 '#' 提前送出目前已輸入的內容進行比對（未滿 4 碼視為密碼錯誤）。
7. 修改範例 5-1 的錯誤鎖定機制，新增「鎖定倒數重新開放」的功能：鎖定後，透過某個固定次數的迴圈模擬等待一段時間，時間到後自動解除鎖定（而不需要永久鎖死）。
8. 設計一個「雙重密碼」系統：兩組不同的 4 位密碼，分別對應不同的伺服馬達角度（模擬「不同人員有不同權限」的概念）。
9. 結合第 11 週的自訂字元，在密碼正確開鎖時，額外顯示一個自訂的「打勾」或「笑臉」圖示。
10. 修改 BOSS 關程式碼，統計並顯示「歷史累計開鎖成功次數」。

### 🔴 挑戰題
11. **密碼強度檢查**：設計一個功能，在設定新密碼時，檢查密碼是否「四位數字都相同」（如 1111），如果是則拒絕設定並要求重新輸入，模擬簡易的密碼強度驗證。
12. **輸入逾時重置**：設計一個機制，如果玩家開始輸入密碼後超過一段時間沒有繼續按鍵，自動清空目前已輸入的內容並重新開始（提示：這需要用到類似 `millis()` 計時的概念，可以先用簡化的計數方式模擬，第 14 週會有更完整的解法）。
13. **多層選單設計**：設計一個具備「主選單」的密碼鎖系統，按 'A' 進入密碼驗證、按 'B' 進入密碼設定、按 'C' 進入系統資訊查詢（顯示累計開鎖次數等統計資料）。
14. **雙重確認機制**：修改設定新密碼的流程，要求玩家輸入兩次新密碼，兩次必須完全一致才會真正套用，否則要求重新設定（模擬真實系統常見的「確認密碼」欄位設計）。
15. **完全自訂密碼鎖系統**：不參考任何範例，設計一個「你認為最完整的密碼鎖系統」，至少要用到：退格刪除、錯誤次數鎖定、LCD 完整輸入回饋、伺服馬達開鎖動作。

---

## 🎓 Arduino 單元期末統整驗收

恭喜你走到這裡！這代表你已經完成了整個 Arduino 基礎元件學習階段的所有內容。在正式進入第 13 週開始的進階整合週之前，讓我們花點時間做一次完整的技能盤點。

### 技能總覽表（請誠實自評 1~5 分）

| 週次 | 核心技能 | 自評分數 |
|---|---|---|
| 第 1 週 | 數位輸出、陣列、迴圈、自訂函式 | ___ / 5 |
| 第 2 週 | 數位輸入、if-else、防彈跳、狀態變數 | ___ / 5 |
| 第 3 週 | PWM、RGB 混色、帶參數函式 | ___ / 5 |
| 第 4 週 | 類比輸入、map()、constrain() | ___ / 5 |
| 第 5 週 | 蜂鳴器、光敏電阻、狀態轉變偵測 | ___ / 5 |
| 第 6 週 | 溫度感測、float、警報鎖定與復歸 | ___ / 5 |
| 第 7 週 | 伺服馬達、物件導向雛形、安全連鎖 | ___ / 5 |
| 第 8 週 | 系統架構設計、模組化、QA 測試 | ___ / 5 |
| 第 9 週 | 超音波測距、pulseIn() | ___ / 5 |
| 第 10 週 | 直流馬達、L293D、緩啟停 | ___ / 5 |
| 第 11 週 | LCD 顯示、畫面優化、自訂字元 | ___ / 5 |
| 第 12 週 | 矩陣鍵盤、字串處理、密碼鎖邏輯 | ___ / 5 |

### 期末統整任務：設計你自己的「畢業機關」

不參考任何範例，運用這 12 週學過的技能，設計並實作一個**至少整合 4 種不同元件**的完整機關系統，並符合以下要求：
- [ ] 至少 1 個輸入元件（按鈕、旋鈕、光敏電阻、TMP36、超音波、矩陣鍵盤任選）
- [ ] 至少 2 個輸出元件（LED、RGB LED、蜂鳴器、伺服馬達、直流馬達、LCD 任選）
- [ ] 至少 1 個自訂函式（且必須有意義地被重複呼叫，而非只呼叫一次）
- [ ] 至少 1 個安全或優先權相關的邏輯設計（如警報鎖定、緊急停止、狀態機）
- [ ] 完整的程式碼註解，符合這學期一路要求的「工業邏輯語言」註解規範

完成後，建議寫一段 150~200 字的文字，說明你的機關情境設定、使用的元件、以及最引以為傲的一個技術細節，這將是你這學期 Arduino 單元學習成果最好的紀錄。

### 銜接第 13～16 週：進階整合準備

第 13 週開始，我們會補齊工業自動化最後幾個關鍵元件，並在最後兩週把整學期所學收斂成完整的畢業作品：
- 第 2 週的按鈕邏輯與狀態轉變偵測 → 第 13 週的位移暫存器計數應用、第 14 週的 PIR 感應邏輯
- 第 4 週的 `map()` → 貫穿第 13、14 週所有感測資料轉換
- 第 10 週的 L293D「以小控大」概念 → 第 14 週的繼電器控制大功率負載
- 第 8 週的系統架構設計、優先權排序 → 第 15～16 週終極整合大專題的核心骨架
- 第 6、7 週的安全連鎖與警報鎖定 → 第 14 週的手動優先權設計、第 15～16 週的安全連鎖子系統

**請把這 12 週的講義都保留好**，接下來的第 13～16 週會不斷回頭引用這些已經學過的概念。

---

## 📖 本週詞彙表

| 英文 / 語法 | 中文對照 | 白話解釋 |
|---|---|---|
| 矩陣鍵盤（Membrane Keypad） | 矩陣鍵盤 | 用行列交錯方式，以少量腳位讀取大量按鍵的鍵盤 |
| `<Keypad.h>` | 鍵盤函式庫 | Arduino 常用、專門處理矩陣鍵盤掃描的工具包 |
| 行列掃描（Row-Column Scanning） | 行列掃描 | 矩陣鍵盤判斷哪顆按鍵被按下的核心演算法原理 |
| 字串累加 | 字串累加 | 把多個字元依序串接成一個完整字串的操作 |
| `substring()` | 子字串擷取 | 從一個字串中截取指定範圍的一部分 |
| 錯誤鎖定機制 | 錯誤鎖定機制 | 連續輸入錯誤達到次數上限，系統自動鎖定的安全設計 |
| 設定模式（Setting Mode） | 設定模式 | 系統切換到可修改參數（如密碼）的特殊操作模式 |

---

## 反思與延伸討論

1. 矩陣鍵盤用「行列交錯掃描」的巧思，讓 8 隻腳位就能讀取 16 顆按鍵，這是這學期第一次接觸「用演算法思維節省硬體資源」的例子。請討論：如果沒有這種設計，直接讓每顆按鍵各自佔用一隻腳位，會對 Arduino 的腳位資源造成什麼樣的壓力（Arduino Uno 總共只有約 20 隻可用腳位）？
2. BOSS 關程式碼中，`systemLocked` 觸發後系統會「完全停止接受任何操作」，這跟真實世界的提款機、手機解鎖畫面在「連續輸入錯誤密碼」後的反應是否類似？請討論這種設計對「安全性」與「使用者體驗」之間的取捨。
3. 回顧這 12 週的學習歷程，從第 1 週最簡單的 LED 閃爍，到這週具備字串處理、錯誤鎖定、多元件整合的完整密碼鎖系統。請討論：你認為哪一個技術轉折點（例如：陣列、狀態轉變偵測、模組化架構、物件導向、字串處理）對你的程式思維帶來最大的提升？

---

## 本週檢核清單

- [ ] 理解矩陣鍵盤的行列掃描原理，並能正確使用 `<Keypad.h>`
- [ ] 能用字串累加技巧組合多個字元輸入
- [ ] 能實作退格刪除功能
- [ ] 能實作完整的密碼比對與電子鎖邏輯
- [ ] 能整合 LCD 顯示完整的輸入回饋介面
- [ ] 能實作錯誤次數鎖定的安全機制
- [ ] 完成期末統整的技能自評與「畢業機關」設計任務

---

## 下週預告

恭喜完成 Arduino 基礎元件學習階段的所有內容！從下週開始，我們將進入第四階段——補齊工業自動化最後幾個關鍵元件：位移暫存器與七段顯示器（第 13 週）、繼電器與 PIR 感測器（第 14 週），最後用整整兩週的時間（第 15～16 週），把這學期學到的**所有**技術收斂成一套完整的終極密室整合大專題，作為這學期最精彩的壓軸！

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 11 週](./week-11.md) | [下一週：第 13 週 — 產線數字看板升級站 ➡](./week-13.md)
