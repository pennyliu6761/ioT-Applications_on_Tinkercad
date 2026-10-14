[⬅ 回目錄](../README.md) | [⬅ 上一週：第 10 週](week-10.md) | [下一週：第 12 週 — 期末統整與電子密碼鎖 ➡](week-12.md)

---

# 第 11 週：會說話的智慧場域

## 主題專案：把數據變成現場看得懂的看板

> 🎯 **本週定位**：十週以來，系統只能用燈號和聲音跟人溝通，詳細數字要打開電腦看 Serial Monitor——現場人員根本看不到。這一週加入 **LCD 16×2 顯示器**，系統第一次可以直接在現場「顯示文字和數字」。
>
> 對工管人來說，這週特別重要：**看板管理**是精實生產與現場管理的核心工具。數據要被看見，才能被管理。
>
> 和前幾週一樣，本週**重點不在寫程式**，而在於：
>
> 1. 讀懂情境需求，決定看板上**要顯示什麼、不顯示什麼**（只有 2 行 × 16 格！）
> 2. 規劃腳位——LCD 一次要佔 6 隻腳，是全學期最「吃腳位」的元件
> 3. 選擇 LCD 的版本：**平行版**（6 隻腳）還是 **I2C 版**（2 隻腳）
>
> 顯示文字的程式都已經準備好，你只需要填腳位、修改要顯示的文字、把功能開關從 `false` 改成 `true`。
>
> 🏁 **本週成果**：完成 **4 個獨立專案**，每個專案存成一個獨立的 Tinkercad 檔案繳交。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：LCD 16×2 顯示器](#認識新元件lcd-162-顯示器)
- [看板設計原則](#看板設計原則)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [專案一：會議室門口狀態牌](#專案一會議室門口狀態牌)
- [專案二：藥品冷藏櫃溫度看板](#專案二藥品冷藏櫃溫度看板)
- [專案三：產線績效看板](#專案三產線績效看板)
- [專案四：冷鏈倉儲戰情中心](#專案四冷鏈倉儲戰情中心)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 店門口跑馬燈招牌](#-lv1-店門口跑馬燈招牌)
  - [Lv.2 診所排隊叫號機](#-lv2-診所排隊叫號機)
  - [Lv.3 健身房跑步機面板](#-lv3-健身房跑步機面板)
  - [Lv.4 停車場剩餘車位看板](#-lv4-停車場剩餘車位看板)
  - [Lv.5 病房護理站呼叫看板](#-lv5-病房護理站呼叫看板)
  - [🏆 Lv.6 BOSS 關：物聯網顧問提案](#-lv6-boss-關物聯網顧問提案)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～10 週）

- ✅ 輸出：LED、RGB、蜂鳴器、伺服、直流馬達（L293D）
- ✅ 輸入：按鈕、旋鈕、光敏電阻、TMP36、HC-SR04
- ✅ 「~ 腳可用表」、借用 A 腳、L293D 四種接法
- ✅ 狀態改變偵測、鎖定與復歸、安全連鎖、安裝位置換算

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| LCD 16×2 | 2 行、每行 16 個字的小螢幕 | 產線看板、叫號螢幕、設備面板 |
| 平行版 LCD | 用 6 條訊號線控制，接法較多 | 傳統設備面板 |
| I2C 版 LCD | 只用 2 條訊號線（A4、A5），接法很簡單 | 現代模組化設備 |
| `showLine()` 顯示函式 | 已寫好的函式：指定第幾行、要顯示什麼字 | 在看板上貼一行字 |
| 分頁顯示 | 螢幕太小，用按鈕切換不同頁面 | 手機上滑切換頁面 |

### 本週可用元件（只限這些）

| 元件 | 第幾週學的 |
| --- | --- |
| 單色 LED、RGB LED、220Ω | 第 1、3 週 |
| 按鈕、10kΩ | 第 2 週 |
| 旋鈕 | 第 4 週 |
| 蜂鳴器、光敏電阻 | 第 5 週 |
| TMP36 | 第 6 週 |
| 伺服馬達 | 第 7 週 |
| 超音波 HC-SR04 | 第 9 週 |
| 直流馬達、L293D | 第 10 週 |
| LCD 16×2（平行版、I2C 版） | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：前十週的客戶紛紛回頭反映同一件事：「系統很好用，但現場人員看不懂燈號代表什麼，詳細數字又要開電腦才看得到。」這週的四個委託案，都要加上一塊**現場看板**。
>
> | 客戶 | 委託內容 | 難度 |
> | --- | --- | --- |
> | 共享辦公室 | 會議室門口狀態牌 | ⭐ |
> | 社區藥局 | 藥品冷藏櫃溫度看板 | ⭐⭐ |
> | 食品工廠 | 產線績效看板 | ⭐⭐⭐ |
> | 冷鏈物流 | 倉儲戰情中心 | ⭐⭐⭐⭐ |
>
> 每個專案都分成 3 個步驟，從最簡單的版本開始，每一步多接幾個元件、多一項功能。

---

## 認識新元件：LCD 16×2 顯示器

### 螢幕長什麼樣子？

```
     第 0 格                第 15 格
       ↓                      ↓
     ┌────────────────────────┐
第0行 │T E M P :   4 . 2  C  │
第1行 │S T A T U S :   O K    │
     └────────────────────────┘
```

- 一共 **2 行**，每行 **16 格**，每格放一個英文字母、數字或符號。
- **只能顯示英文、數字和符號**，不能顯示中文。
- 位置從 **0** 開始數：第 0 行是上面那行，第 1 行是下面那行。

### 兩種版本，該選哪一種？

Tinkercad 元件庫裡有兩種 LCD：**「LCD 16 x 2」**（平行版）和 **「LCD 16 x 2 (I2C)」**（I2C 版）。

| 比較 | 平行版 | I2C 版 |
| --- | --- | --- |
| 佔用 Arduino 腳位 | **6 隻**（任何數位腳或 A 腳） | **2 隻**（固定是 A4、A5） |
| 接線難度 | 16 隻腳，要接 12 條線 | 4 隻腳，只接 4 條線 |
| 程式寫法 | `LiquidCrystal` | `Adafruit_LiquidCrystal` |
| 適合 | 元件少、腳位很夠用 | 元件多、腳位吃緊 |
| 限制 | 吃掉一半的數位腳 | A4、A5 被佔用後，**不能再借來接其他東西** |

> 💡 **選擇原則**：先算出系統其他元件要用幾隻腳。如果加上平行版 LCD 的 6 隻腳後還有剩，用平行版；如果不夠，改用 I2C 版。

### 平行版接法（12 條線）

LCD 上每隻腳都有標示名稱，照下表接：

| LCD 腳位 | 白話說明 | 接到哪裡 |
| --- | --- | --- |
| GND（VSS） | 螢幕接地 | GND |
| VCC（VDD） | 螢幕電源 | 5V |
| V0 | 對比度（字的深淺） | 旋鈕中間腳（旋鈕兩側接 5V、GND）；Tinkercad 也可直接接 GND |
| RS | 訊號線 ① | 數位腳 |
| RW | 讀寫切換 | **GND**（我們只寫不讀） |
| E | 訊號線 ② | 數位腳 |
| DB0～DB3 | 不使用 | **不接** |
| DB4、DB5、DB6、DB7 | 訊號線 ③～⑥ | 數位腳 |
| LED+（A） | 背光正極 | 5V（真實硬體建議串 220Ω） |
| LED−（K） | 背光負極 | GND |

> ⚠️ **6 條訊號線（RS、E、DB4～DB7）接一般腳就可以**，不需要 `~` 腳；不夠的話也可以借 A 腳。

### I2C 版接法（4 條線）

| LCD 腳位 | 接到哪裡 |
| --- | --- |
| GND | GND |
| VCC | 5V |
| SDA | **A4**（固定） |
| SCL | **A5**（固定） |

### 程式裡的「顯示函式」

本週所有程式都附上 `showLine()`，你**不需要修改它**，只要知道怎麼用：

```cpp
showLine(0, "TEMP: 4.2 C");     // 在第 0 行（上面）顯示這段字
showLine(1, "STATUS: OK");      // 在第 1 行（下面）顯示這段字
```

它會自動在後面補空白，把舊的字蓋掉，避免畫面殘留。超過 16 個字的部分會被切掉。

---

## 看板設計原則

LCD 只有 32 格，比一則簡訊還短。**看板設計的關鍵不是「能顯示什麼」，而是「最該顯示什麼」**。

| 原則 | 說明 | 範例 |
| --- | --- | --- |
| ① 最重要的放第 0 行 | 現場人員第一眼看上面 | 溫度、目前號碼、異常訊息 |
| ② 用縮寫，但要一致 | 16 格很擠，要省字 | `TEMP`、`QTY`、`AVL`（Available） |
| ③ 異常要搶眼 | 異常時整行換成大寫警示 | `!! ALARM !!` |
| ④ 單位要寫 | 數字沒有單位等於沒說 | `4.2 C`、`35 cm`、`12 pcs` |
| ⑤ 太多就分頁 | 用按鈕切換，每頁一個主題 | 第 1 頁產量、第 2 頁稼動率 |

> 🏭 **看板管理（Visual Management）**：豐田生產系統中，看板讓「異常一眼可見」。好的看板讓現場人員**不用問、不用算**，就知道現在該做什麼。

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **先算腳位，再選 LCD 版本** | 平行版 6 隻、I2C 版 2 隻（A4、A5） |
| ② **平行版：RW 接 GND、DB0～DB3 不接** | 只有 RS、E、DB4～DB7 接 Arduino |
| ③ LCD 訊號線接一般腳就好 | 不要佔 `~` 腳 |
| ④ 用 I2C 版時，A4、A5 不能再借來接其他元件 | |
| ⑤ 有伺服，9、10 不能調速調光；有蜂鳴器，3、11 不能 | 兩者都有時只剩 5、6 |
| ⑥ 每條燈線一顆 220Ω；按鈕、光敏電阻一顆 10kΩ | |
| ⑦ Pin 0、1 先不要用；腳位不夠時借 A 腳 | |

---

## 共用工具

### 🔧 工具 1：LCD 測試器（平行版）

接好平行版 LCD 後先跑這支。第 0 行顯示 `LCD TEST OK`，第 1 行每秒加 1。

```cpp
#include <LiquidCrystal.h>

// ===== 改成你的腳位：RS, E, DB4, DB5, DB6, DB7 =====
LiquidCrystal lcd(7, 8, 9, 10, 11, 12);

void setup() {
  lcd.begin(16, 2);
  lcd.setCursor(0, 0);
  lcd.print("LCD TEST OK");
}

void loop() {
  lcd.setCursor(0, 1);
  lcd.print("Seconds: ");
  lcd.print(millis() / 1000);
  delay(200);
}
```

### 🔧 工具 2：LCD 測試器（I2C 版）

```cpp
#include <Adafruit_LiquidCrystal.h>

Adafruit_LiquidCrystal lcd(0);   // I2C 版，SDA 接 A4、SCL 接 A5

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  lcd.setCursor(0, 0);
  lcd.print("I2C LCD TEST OK");
}

void loop() {
  lcd.setCursor(0, 1);
  lcd.print("Seconds: ");
  lcd.print(millis() / 1000);
  delay(200);
}
```

**檢查標準**：第 0 行出現文字、第 1 行數字持續增加。如果畫面全黑或全白方塊，先檢查電源、V0、RW（平行版）。

### 🔧 其他工具（沿用前幾週）

| 工具 | 用途 | 出處 |
| --- | --- | --- |
| 接線驗證器 | 檢查燈的腳位 | 第 3 週起 |
| 類比讀值檢查器 | 檢查旋鈕、光敏電阻 | 第 5 週 |
| 溫度讀值檢查器 | 檢查 TMP36 | 第 6 週 |
| 超音波測距檢查器 | 檢查 HC-SR04 | 第 9 週 |
| 馬達測試器 | 檢查 L293D 與馬達 | 第 10 週 |

> 📌 **每個專案的固定流程**
> 1. **Step 0**：讀完情境，先算腳位、選 LCD 版本，再畫出**看板版面草圖**（2 行 × 16 格）
> 2. 先只接 LCD，用 LCD 測試器確認有字
> 3. 照 Step 1 → 2 → 3 逐步接線，每完成一步就把對應的功能開關改成 `true`

---

## 專案一：會議室門口狀態牌

⭐ **難度**：第一次接 LCD，再逐步加按鈕與燈號

**客戶需求**：共享辦公室每間會議室門口放一塊狀態牌。平常顯示會議室名稱和「可預約」；有人開會時，按一下按鈕切換成「使用中」，並顯示**已經使用了多久**；會議結束再按一下恢復。門口另有紅綠燈，讓走廊上的人遠遠就看得到。

### Step 0：腳位規劃與看板草圖

| 元件 | 腳位 |
| --- | --- |
| LCD RS、E、DB4、DB5、DB6、DB7 | |
| 使用中切換鈕 | |
| 紅燈（使用中）、綠燈（可預約） | |
| **合計用了幾隻？還剩幾隻？** | |

**看板草圖**（填入你預期的畫面）：

```
可預約時：            使用中時：
┌────────────────┐   ┌────────────────┐
│                │   │                │
│                │   │                │
└────────────────┘   └────────────────┘
```

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 平行版 LCD | 顯示會議室名稱與 `Available` | （不用改） |
| Step 2 | ＋1 顆按鈕 | 按一下變 `IN USE` 並計時，再按恢復 | `useButton = true` |
| Step 3 | ＋紅燈＋綠燈 | 可預約亮綠、使用中亮紅 | `useLights = true` |

### 程式（只改設定區）

```cpp
#include <LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
int pinRS = ?, pinE = ?, pinD4 = ?, pinD5 = ?, pinD6 = ?, pinD7 = ?;   // LCD
int btnToggle = ?;                  // 使用中切換鈕
int ledRed = ?, ledGreen = ?;       // 紅燈、綠燈
String roomName = "Room A-301";     // 會議室名稱（英文，16 字以內）
bool useButton = false;             // Step 2 接好後改成 true
bool useLights = false;             // Step 3 接好後改成 true
// ============================

LiquidCrystal lcd(pinRS, pinE, pinD4, pinD5, pinD6, pinD7);
bool inUse = false;
unsigned long startTime = 0;
int lastButton = LOW;

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

String twoDigits(int n) {
  return (n < 10) ? "0" + String(n) : String(n);
}

void setup() {
  lcd.begin(16, 2);
  pinMode(btnToggle, INPUT);
  pinMode(ledRed, OUTPUT);
  pinMode(ledGreen, OUTPUT);
}

void loop() {
  if (useButton) {
    int now = digitalRead(btnToggle);
    if (now == HIGH && lastButton == LOW) {
      delay(50);
      inUse = !inUse;
      startTime = millis();
    }
    lastButton = now;
  }

  showLine(0, roomName);
  if (inUse) {
    unsigned long sec = (millis() - startTime) / 1000;
    showLine(1, "IN USE  " + twoDigits(sec / 60) + ":" + twoDigits(sec % 60));
  } else {
    showLine(1, "Available");
  }

  if (useLights) {
    digitalWrite(ledRed,   inUse ? HIGH : LOW);
    digitalWrite(ledGreen, inUse ? LOW : HIGH);
  }
  delay(200);
}
```

### 情境思考

1. 平行版 LCD 加上一顆按鈕、兩顆燈，一共用了幾隻腳？這個情境需要改用 I2C 版嗎？
2. 有了 LCD，為什麼還要紅綠燈？兩者分別給誰看？
3. 💼 「使用中已經 2 小時 15 分」這個資訊，對辦公室管理者有什麼用？（提示：預約制度、超時收費、空間規劃）

### ✅ 專案一驗收

- [ ] LCD 第 0 行顯示會議室名稱，第 1 行顯示 `Available`
- [ ] 按一下按鈕變 `IN USE` 並開始計時（分:秒），再按一下恢復
- [ ] 可預約亮綠、使用中亮紅
- [ ] 畫面沒有殘留的舊字

---

## 專案二：藥品冷藏櫃溫度看板

⭐⭐ **難度**：TMP36 ＋ LCD ＋ 最高／最低溫紀錄 ＋ 警報

**客戶需求**：第 6 週的藥品冷藏櫃（2～8°C）升級版。藥局希望看板上除了即時溫度，還要顯示**今天的最高溫和最低溫**（衛生稽查時要看）。藥師每天早上按一下「歸零鈕」重新記錄。溫度超出範圍時，看板第 1 行改成警示訊息，蜂鳴器和紅燈同時提醒。

### Step 0：腳位規劃與看板草圖

| 元件 | 腳位 |
| --- | --- |
| LCD（6 隻） | |
| TMP36 | A0 |
| 紀錄歸零鈕 | |
| 蜂鳴器 | |
| 紅燈（異常） | |

**看板草圖**：

```
正常時：               異常時：
┌────────────────┐   ┌────────────────┐
│NOW  4.2C  OK   │   │NOW  9.6C       │
│MAX 6.1 MIN 3.0 │   │!! OUT OF RANGE │
└────────────────┘   └────────────────┘
```

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 平行版 LCD＋TMP36 | 第 0 行顯示即時溫度與 OK／NG | （不用改） |
| Step 2 | ＋歸零鈕 | 第 1 行顯示最高／最低溫，按鈕歸零 | `useRecord = true` |
| Step 3 | ＋蜂鳴器＋紅燈 | 超出範圍時第 1 行改為警示，並響鈴亮燈 | `useAlarm = true` |

### 程式（只改設定區）

```cpp
#include <LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
int pinRS = ?, pinE = ?, pinD4 = ?, pinD5 = ?, pinD6 = ?, pinD7 = ?;   // LCD
int pinTemp   = A0;                 // TMP36
int btnReset  = ?;                  // 紀錄歸零鈕
int pinBuzzer = ?;                  // 蜂鳴器
int ledAlarm  = ?;                  // 異常紅燈
float lowLimit = 2.0, highLimit = 8.0;
bool useRecord = false;             // Step 2 接好後改成 true
bool useAlarm  = false;             // Step 3 接好後改成 true
// ============================

LiquidCrystal lcd(pinRS, pinE, pinD4, pinD5, pinD6, pinD7);
float maxT = -100, minT = 100;
int lastButton = LOW;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void setup() {
  lcd.begin(16, 2);
  pinMode(btnReset, INPUT);
  pinMode(ledAlarm, OUTPUT);
}

void loop() {
  float t = readC(pinTemp);
  bool ok = (t >= lowLimit && t <= highLimit);

  if (t > maxT) maxT = t;
  if (t < minT) minT = t;

  if (useRecord) {
    int now = digitalRead(btnReset);
    if (now == HIGH && lastButton == LOW) {
      delay(50);
      maxT = t;
      minT = t;
    }
    lastButton = now;
  }

  showLine(0, "NOW " + String(t, 1) + "C  " + (ok ? "OK" : "NG"));

  if (useAlarm && !ok) {
    showLine(1, "!! OUT OF RANGE");
    digitalWrite(ledAlarm, HIGH);
    tone(pinBuzzer, 1200, 150);
  } else {
    digitalWrite(ledAlarm, LOW);
    if (useRecord) showLine(1, "MAX " + String(maxT, 1) + " MIN " + String(minT, 1));
    else           showLine(1, "Range 2-8 C");
  }
  delay(500);
}
```

### 情境思考

1. 第 1 行在異常時被警示訊息「蓋掉」，最高／最低溫暫時看不到。這樣的設計合理嗎？為什麼？
2. 藥師忘了每天早上按歸零鈕，看板上的「最高溫」會變成什麼意思？你會怎麼提醒藥師？
3. 💼 衛生稽查員要看的是「過去 30 天每天的最高、最低溫」。這塊看板做得到嗎？還缺什麼？

### ✅ 專案二驗收

- [ ] 第 0 行顯示即時溫度（一位小數）與 OK／NG
- [ ] 第 1 行顯示最高、最低溫，按歸零鈕後重新記錄
- [ ] 超出 2～8°C 時第 1 行改為 `!! OUT OF RANGE`，紅燈亮、蜂鳴器響
- [ ] 看板版面與 Step 0 草圖一致

---

## 專案三：產線績效看板

⭐⭐⭐ **難度**：HC-SR04 計數 ＋ 旋鈕設目標 ＋ 停機紀錄 ＋ 分頁 → **工管人最熟悉的看板**

**客戶需求**：食品工廠包裝線的末端看板，讓班長隨時掌握兩個關鍵指標：

- **第 1 頁：產量達成率**——超音波計算通過的產品數，旋鈕設定本班目標產量，顯示「實際／目標」與達成率 %；達標時蜂鳴器播放提示
- **第 2 頁：稼動率**——作業員按「停機鈕」記錄停機開始、再按一次記錄恢復；看板顯示累計運轉時間、停機時間，以及**稼動率＝運轉時間 ÷（運轉＋停機）**
- 一顆「換頁鈕」切換兩頁

> 🏭 **稼動率（Availability）** 是 OEE（設備綜合效率）的三大指標之一。稼動率低，代表設備常常停下來，要找出停機原因。

### Step 0：腳位規劃與看板草圖

| 元件 | 腳位 |
| --- | --- |
| LCD（6 隻） | |
| HC-SR04 Trig／Echo | |
| 目標產量旋鈕 | A0 |
| 停機鈕、換頁鈕 | |
| 蜂鳴器 | |
| **合計用了幾隻？平行版夠嗎？** | |

**看板草圖**：

```
第 1 頁：              第 2 頁：
┌────────────────┐   ┌────────────────┐
│QTY  37/ 50     │   │RUN 12:05 [ON]  │
│RATE  74%       │   │DOWN 2:10 A 85% │
└────────────────┘   └────────────────┘
```

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 平行版 LCD＋HC-SR04 | 每有產品通過（擋一下），QTY 加 1 | （不用改） |
| Step 2 | ＋旋鈕＋蜂鳴器 | 顯示目標與達成率，達標時播放提示 | `useTarget = true` |
| Step 3 | ＋停機鈕＋換頁鈕 | 記錄停機，第 2 頁顯示稼動率 | `usePages = true` |

### 程式（只改設定區）

```cpp
#include <LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
int pinRS = ?, pinE = ?, pinD4 = ?, pinD5 = ?, pinD6 = ?, pinD7 = ?;   // LCD
int pinTrig = ?, pinEcho = ?;       // HC-SR04（產品通過偵測）
int pinKnob = A0;                   // 目標產量旋鈕
int btnDown = ?;                    // 停機鈕
int btnPage = ?;                    // 換頁鈕
int pinBuzzer = ?;                  // 蜂鳴器
float passCM = 10;                  // 小於這個距離算「產品經過」
bool useTarget = false;             // Step 2 接好後改成 true
bool usePages  = false;             // Step 3 接好後改成 true
// ============================

LiquidCrystal lcd(pinRS, pinE, pinD4, pinD5, pinD6, pinD7);
int qty = 0;
bool wasPassing = false;
bool reached = false;
bool running = true;
unsigned long runMs = 0, downMs = 0, lastTick = 0;
int page = 0;
int lastDown = LOW, lastPage = LOW;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

String mmss(unsigned long ms) {
  unsigned long s = ms / 1000;
  String sec = (s % 60 < 10) ? "0" + String(s % 60) : String(s % 60);
  return String(s / 60) + ":" + sec;
}

bool pressed(int pin, int &last) {
  int now = digitalRead(pin);
  bool edge = (now == HIGH && last == LOW);
  last = now;
  if (edge) delay(50);
  return edge;
}

void setup() {
  lcd.begin(16, 2);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(btnDown, INPUT);
  pinMode(btnPage, INPUT);
  lastTick = millis();
}

void loop() {
  // ---- 運轉／停機時間累計 ----
  unsigned long nowMs = millis();
  if (running) runMs += nowMs - lastTick;
  else         downMs += nowMs - lastTick;
  lastTick = nowMs;

  // ---- 產品計數（只在運轉中計） ----
  bool passing = readCM(pinTrig, pinEcho) < passCM;
  if (passing && !wasPassing && running) qty++;
  wasPassing = passing;

  int target = map(analogRead(pinKnob), 0, 1023, 10, 200);

  if (useTarget && qty >= target && !reached) {
    reached = true;
    tone(pinBuzzer, 1047, 300);
  }
  if (qty < target) reached = false;

  if (usePages) {
    if (pressed(btnDown, lastDown)) running = !running;
    if (pressed(btnPage, lastPage)) page = (page + 1) % 2;
  }

  if (page == 0) {
    if (useTarget) {
      showLine(0, "QTY " + String(qty) + "/" + String(target));
      showLine(1, "RATE " + String(qty * 100 / target) + "%" + (reached ? "  DONE!" : ""));
    } else {
      showLine(0, "QTY " + String(qty));
      showLine(1, "Counting...");
    }
  } else {
    unsigned long total = runMs + downMs;
    int avail = (total > 0) ? (int)(runMs * 100 / total) : 100;
    showLine(0, "RUN " + mmss(runMs) + (running ? " [ON]" : " [DOWN]"));
    showLine(1, "DOWN " + mmss(downMs) + " A " + String(avail) + "%");
  }
  delay(100);
}
```

### 情境思考

1. 用你的腳位規劃表說明：平行版 LCD 加上其他元件一共幾隻腳？還剩幾隻？
2. 第 1 頁和第 2 頁，你覺得哪一頁應該是「預設頁」（開機時顯示的那一頁）？為什麼？
3. 💼 稼動率 85% 代表什麼？如果你是班長，看到稼動率一直下降，下一步要做什麼？（提示：停機時只按「停機鈕」，看板知道「為什麼停機」嗎？）

### ✅ 專案三驗收

- [ ] 每擋一次超音波，QTY 只加 1
- [ ] 旋鈕能改變目標，達成率正確，達標時蜂鳴器提示一次
- [ ] 停機鈕切換運轉／停機，第 2 頁時間正確累計
- [ ] 稼動率計算正確（可用計算機驗算）
- [ ] 換頁鈕能在兩頁間切換

---

## 專案四：冷鏈倉儲戰情中心

⭐⭐⭐⭐ **難度**：多感測器 ＋ RGB ＋ 蜂鳴器 ＋ 2 按鈕 ＋ 4 頁看板 → **平行版腳位不夠，改用 I2C 版**

**客戶需求**：冷鏈物流倉庫的辦公室要一塊戰情看板，整合四種資訊，用按鈕翻頁：

| 頁面 | 內容 | 資料來源 |
| --- | --- | --- |
| 第 1 頁 | 冷藏區溫度與狀態 | TMP36 |
| 第 2 頁 | 今日出貨件數 | 出貨口 HC-SR04 計數 |
| 第 3 頁 | 倉庫照明狀態（日間／夜間） | 光敏電阻 |
| 第 4 頁 | 異常次數與最近一次異常距今多久 | 系統紀錄 |

- RGB 總狀態燈：正常綠、溫度警戒黃、溫度異常紅
- 溫度異常時，**不論停在哪一頁，看板自動跳到第 1 頁**，蜂鳴器響；值班人員按「確認鈕」停止鳴叫

### Step 0：腳位規劃——平行版還是 I2C 版？（本專案的重頭戲）

**先算一次平行版：**

| 元件 | 需要幾隻腳 |
| --- | --- |
| LCD（平行版） | 6 |
| HC-SR04 Trig／Echo | 2 |
| 狀態 RGB | 3 |
| 蜂鳴器 | 1 |
| 換頁鈕、確認鈕 | 2 |
| **數位腳合計** | |
| TMP36、光敏電阻 | 2（A 腳） |
| **可用總腳位（Pin 2～13＋A0～A5）** | 18 |
| **還剩幾隻？** | |

**再算一次 I2C 版**（LCD 改用 A4、A5）：還剩幾隻？

> 🤔 **規劃提示**：平行版「勉強塞得下」，但幾乎沒有擴充空間。客戶明年想加一顆冷凍區溫度計和一扇自動門，哪一個版本放得下？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | I2C 版 LCD＋TMP36＋HC-SR04＋換頁鈕 | 第 1、2 頁顯示溫度與出貨數 | （不用改） |
| Step 2 | ＋光敏電阻＋RGB＋蜂鳴器 | 第 3 頁照明狀態；RGB 狀態燈；異常時跳回第 1 頁並鳴叫 | `useAlarm = true` |
| Step 3 | ＋確認鈕 | 按確認停止鳴叫；第 4 頁顯示異常紀錄 | `useLog = true` |

### 程式（只改設定區）

```cpp
#include <Adafruit_LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
// LCD 使用 I2C 版：SDA 接 A4、SCL 接 A5（固定，不用填）
int pinTemp = A0;                   // 冷藏區 TMP36
int pinLDR  = A1;                   // 倉庫光敏電阻
int pinTrig = ?, pinEcho = ?;       // 出貨口 HC-SR04
int pinR = ?, pinG = ?, pinB = ?;   // 狀態 RGB（黃色要調光，避開 3、11）
int pinBuzzer = ?;                  // 蜂鳴器
int btnPage = ?;                    // 換頁鈕
int btnAck  = ?;                    // 確認鈕
float warnLine = 6.0, dangerLine = 8.0;   // 冷藏區溫度警戒／異常線
float passCM = 15;                  // 出貨口偵測距離
int darkLine = 300;                 // 光線低於這個數字算夜間
bool useAlarm = false;              // Step 2 接好後改成 true
bool useLog   = false;              // Step 3 接好後改成 true
// ============================

Adafruit_LiquidCrystal lcd(0);
int page = 0, pageCount = 3;
int shipped = 0;
bool wasPassing = false;
bool wasDanger = false, acked = false;
int alarmCount = 0;
unsigned long lastAlarm = 0;
int lastPage = LOW, lastAck = LOW;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

bool pressed(int pin, int &last) {
  int now = digitalRead(pin);
  bool edge = (now == HIGH && last == LOW);
  last = now;
  if (edge) delay(50);
  return edge;
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(btnPage, INPUT);
  pinMode(btnAck, INPUT);
  if (useLog) pageCount = 4;
}

void loop() {
  float t = readC(pinTemp);
  int level = 0;                                   // 0=正常, 1=警戒, 2=異常
  if (t >= dangerLine)    level = 2;
  else if (t >= warnLine) level = 1;

  // ---- 出貨計數 ----
  bool passing = readCM(pinTrig, pinEcho) < passCM;
  if (passing && !wasPassing) shipped++;
  wasPassing = passing;

  if (pressed(btnPage, lastPage)) page = (page + 1) % pageCount;

  // ---- 異常處理 ----
  if (useAlarm) {
    bool danger = (level == 2);
    if (danger && !wasDanger) {                    // 剛變成異常
      alarmCount++;
      lastAlarm = millis();
      acked = false;
      page = 0;                                    // 自動跳回第 1 頁
    }
    wasDanger = danger;

    if (useLog && pressed(btnAck, lastAck)) acked = true;

    if (level == 2)      setColor(255, 0, 0);
    else if (level == 1) setColor(255, 180, 0);
    else                 setColor(0, 255, 0);

    if (danger && !acked) tone(pinBuzzer, 1500, 150);
  }

  // ---- 看板內容 ----
  if (page == 0) {
    showLine(0, "COLD " + String(t, 1) + " C");
    if (level == 2)      showLine(1, acked ? "ALARM (ACKED)" : "!! TEMP ALARM !!");
    else if (level == 1) showLine(1, "WARNING");
    else                 showLine(1, "STATUS OK");
  } else if (page == 1) {
    showLine(0, "SHIPPED TODAY");
    showLine(1, String(shipped) + " pcs");
  } else if (page == 2) {
    bool night = analogRead(pinLDR) < darkLine;
    showLine(0, "WAREHOUSE LIGHT");
    showLine(1, night ? "NIGHT MODE" : "DAY MODE");
  } else {
    showLine(0, "ALARMS: " + String(alarmCount));
    if (alarmCount == 0) showLine(1, "No alarm today");
    else showLine(1, "Last " + String((millis() - lastAlarm) / 60000) + " min ago");
  }
  delay(200);
}
```

### 情境思考

1. 用你的兩張腳位計算表說明：為什麼選 I2C 版？用 I2C 版之後，還能借哪些 A 腳？
2. 「異常時自動跳回第 1 頁」這個設計，解決了什麼問題？如果沒有這個設計，會發生什麼事？
3. 💼 這塊看板放在辦公室，但冷藏區的溫度異常可能發生在半夜沒人的時候。看板管理的極限在哪裡？下一步應該怎麼延伸？（提示：物聯網的「聯網」兩個字）

### ✅ 專案四驗收

- [ ] I2C 版 LCD 正確顯示，換頁鈕能循環切換所有頁面
- [ ] 出貨口每擋一次，件數只加 1
- [ ] RGB 依溫度顯示綠／黃／紅
- [ ] 溫度異常時看板自動跳回第 1 頁、蜂鳴器響，按確認後停止鳴叫
- [ ] 第 4 頁正確顯示異常次數與距今時間
- [ ] 腳位規劃表比較平行版與 I2C 版，並說明選擇理由

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：四個範例專案是「跟著做」，這一區換你「自己規劃」。每一關先**畫看板草圖**、**算腳位決定 LCD 版本**，再動手接線。
>
> 📈 **難度安排**：
>
> | 關卡 | LCD 版本 | 其他元件 | 配線重點 |
> | --- | --- | --- | --- |
> | Lv.1 | 平行版 | 按鈕 | 第一次自己接 LCD 12 條線 |
> | Lv.2 | 平行版 | 2 按鈕＋蜂鳴器 | 看板內容設計 |
> | Lv.3 | 平行版 | 旋鈕＋按鈕＋L293D 馬達＋蜂鳴器 | LCD＋馬達共存 |
> | Lv.4 | 平行版或 I2C | 2 個 HC-SR04＋RGB＋旋鈕 | 算腳位、決定版本 |
> | Lv.5 | 平行版或 I2C | 3 按鈕＋3 LED＋確認鈕＋蜂鳴器＋TMP36 | 腳位極限，比較兩種方案 |
> | Lv.6 | 自訂 | 自訂 | 顧問提案 |
>
> 📌 **作答規則**：每一關先畫看板草圖、填腳位規劃表，再接線；接好後先用 LCD 測試器確認有字，最後才上傳程式。程式只需要改「設定區」。

---

### 🟢 Lv.1 店門口跑馬燈招牌

**情境**：手搖飲店門口的小招牌，輪流顯示三則訊息（店名、今日推薦、營業時間），每 3 秒換一則；店員按按鈕可以立刻跳到下一則。

**元件**：平行版 LCD＋1 顆按鈕

**程式**：

```cpp
#include <LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
int pinRS = ?, pinE = ?, pinD4 = ?, pinD5 = ?, pinD6 = ?, pinD7 = ?;   // LCD
int btnNext = ?;                    // 下一則按鈕
String top[3]    = { "TEA TIME SHOP", "TODAY SPECIAL",  "OPEN HOURS" };
String bottom[3] = { "Welcome!",      "Mango Green Tea", "10:00 - 22:00" };
int showTime = 3000;                // 每則顯示幾毫秒
// ============================

LiquidCrystal lcd(pinRS, pinE, pinD4, pinD5, pinD6, pinD7);
int msg = 0;
unsigned long lastChange = 0;
int lastButton = LOW;

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void setup() {
  lcd.begin(16, 2);
  pinMode(btnNext, INPUT);
}

void loop() {
  int now = digitalRead(btnNext);
  bool next = (now == HIGH && lastButton == LOW);
  lastButton = now;

  if (next || millis() - lastChange > showTime) {
    if (next) delay(50);
    msg = (msg + 1) % 3;
    lastChange = millis();
  }
  showLine(0, top[msg]);
  showLine(1, bottom[msg]);
  delay(100);
}
```

**情境思考**：

1. 把 `bottom` 的某一則改成超過 16 個字，看板會怎麼顯示？你會怎麼改寫文字？
2. 💼 招牌每 3 秒換一則，路人走過大概只看得到一則。你會把哪一則排第一？為什麼？

---

### 🟢 Lv.2 診所排隊叫號機

**情境**：社區診所的叫號系統。病人按「取號鈕」拿號碼；護理師按「叫號鈕」叫下一位，蜂鳴器「叮咚」一聲。看板第 0 行顯示**目前叫到幾號**，第 1 行顯示**還有幾人在等**。沒有人等時按叫號無效。

**元件**：平行版 LCD＋2 顆按鈕＋1 顆蜂鳴器

**程式**：

```cpp
#include <LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
int pinRS = ?, pinE = ?, pinD4 = ?, pinD5 = ?, pinD6 = ?, pinD7 = ?;   // LCD
int btnTake = ?;                    // 病人取號鈕
int btnCall = ?;                    // 護理師叫號鈕
int pinBuzzer = ?;                  // 蜂鳴器
// ============================

LiquidCrystal lcd(pinRS, pinE, pinD4, pinD5, pinD6, pinD7);
int lastTicket = 0;                 // 最後發出的號碼
int nowServing = 0;                 // 目前叫到的號碼
int lastTake = LOW, lastCall = LOW;

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

String threeDigits(int n) {
  if (n < 10) return "00" + String(n);
  if (n < 100) return "0" + String(n);
  return String(n);
}

bool pressed(int pin, int &last) {
  int now = digitalRead(pin);
  bool edge = (now == HIGH && last == LOW);
  last = now;
  if (edge) delay(50);
  return edge;
}

void setup() {
  lcd.begin(16, 2);
  pinMode(btnTake, INPUT);
  pinMode(btnCall, INPUT);
}

void loop() {
  if (pressed(btnTake, lastTake)) lastTicket++;

  if (pressed(btnCall, lastCall) && nowServing < lastTicket) {
    nowServing++;
    tone(pinBuzzer, 880, 300); delay(350);
    tone(pinBuzzer, 700, 500); delay(550);
  }

  showLine(0, "NOW SERVING " + threeDigits(nowServing));
  showLine(1, "WAITING: " + String(lastTicket - nowServing));
  delay(100);
}
```

**情境思考**：

1. 病人最想知道的是什麼？「目前幾號」還是「還要等多久」？看板能不能顯示「預估等候時間」？需要什麼資料？
2. 💼 取號後離開的病人（過號）會讓等候人數失真。真實診所怎麼處理過號？這個系統要怎麼改？

---

### 🟡 Lv.3 健身房跑步機面板

**情境**：健身房的跑步機面板。按「開始鈕」啟動，跑帶（直流馬達）**緩啟動**到旋鈕設定的速度（1～12 km/h）。看板顯示速度、運動時間與累計距離。再按一次開始鈕暫停，跑帶緩停止，蜂鳴器提示。

**元件**：平行版 LCD＋L293D＋直流馬達＋1 顆旋鈕＋1 顆按鈕＋1 顆蜂鳴器

**Step 0：腳位規劃**

| 問題 | 你的答案 |
| --- | --- |
| 跑帶要調速、不用反轉，選哪種 L293D 接法？佔幾隻腳？ | |
| 有蜂鳴器，EN 要避開哪兩隻腳？ | |
| LCD 6 隻＋其他元件，一共幾隻？ | |

**程式**：

```cpp
#include <LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
int pinRS = ?, pinE = ?, pinD4 = ?, pinD5 = ?, pinD6 = ?, pinD7 = ?;   // LCD
int pinEN = ?;                      // L293D EN1（~ 腳，接法 B，避開 3、11）
int pinKnob  = A0;                  // 速度旋鈕
int btnStart = ?;                   // 開始／暫停鈕
int pinBuzzer = ?;                  // 蜂鳴器
// ============================

LiquidCrystal lcd(pinRS, pinE, pinD4, pinD5, pinD6, pinD7);
bool running = false;
int current = 0;                    // 目前油門
unsigned long runMs = 0, lastTick = 0;
float distanceKm = 0;
int lastButton = LOW;

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void setup() {
  lcd.begin(16, 2);
  pinMode(pinEN, OUTPUT);
  pinMode(btnStart, INPUT);
  lastTick = millis();
}

void loop() {
  int now = digitalRead(btnStart);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    running = !running;
    tone(pinBuzzer, running ? 1200 : 600, 200);
  }
  lastButton = now;

  int kmh = map(analogRead(pinKnob), 0, 1023, 1, 12);
  int target = running ? map(kmh, 1, 12, 80, 255) : 0;

  // 緩啟動／緩停止
  if (current < target) current = min(current + 5, target);
  if (current > target) current = max(current - 5, target);
  analogWrite(pinEN, current);

  unsigned long nowMs = millis();
  if (running) {
    runMs += nowMs - lastTick;
    distanceKm += kmh * (nowMs - lastTick) / 3600000.0;
  }
  lastTick = nowMs;

  unsigned long s = runMs / 1000;
  showLine(0, "SPEED " + String(kmh) + " km/h" + (running ? "" : " ||"));
  showLine(1, "T " + String(s / 60) + ":" + (s % 60 < 10 ? "0" : "") + String(s % 60) +
              "  " + String(distanceKm, 2) + "km");
  delay(50);
}
```

**情境思考**：

1. 本關的 LCD、L293D、按鈕、蜂鳴器一共用了幾隻腳？如果要加「心率燈號」（RGB），還放得下嗎？
2. 💼 健身房想知道「哪個時段使用率最高」。這台跑步機的看板資料可以怎麼幫忙？還缺什麼？

---

### 🟡 Lv.4 停車場剩餘車位看板

**情境**：第 9 週停車場入口的看板升級版。入口、出口各一顆超音波計算車輛進出；管理員用旋鈕設定總車位。看板第 0 行大大顯示**剩餘車位**，第 1 行顯示「已停／總車位」；RGB 在入口外顯示綠（充足）、黃（剩 3 格以下）、紅（客滿）。

**元件**：LCD（平行版或 I2C 版，自己決定）＋2 顆 HC-SR04＋1 顆 RGB＋1 顆旋鈕

**Step 0：腳位規劃（本關重點）**

| 元件 | 平行版方案 | I2C 版方案 |
| --- | --- | --- |
| LCD | 6 隻 | 2 隻（A4、A5） |
| 2 顆 HC-SR04 | 4 | 4 |
| RGB | 3 | 3 |
| 旋鈕 | 1（A 腳） | 1（A 腳） |
| **合計** | | |
| **剩餘可擴充** | | |
| **你選哪一個？為什麼？** | | |

**程式**（以平行版為例；選 I2C 版的同學，把開頭兩行換成工具 2 的寫法，並在 `setup()` 加上 `lcd.setBacklight(1);`）：

```cpp
#include <LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
int pinRS = ?, pinE = ?, pinD4 = ?, pinD5 = ?, pinD6 = ?, pinD7 = ?;   // LCD
int trigIn  = ?, echoIn  = ?;       // 入口 HC-SR04
int trigOut = ?, echoOut = ?;       // 出口 HC-SR04
int pinR = ?, pinG = ?, pinB = ?;   // 入口外 RGB
int pinKnob = A0;                   // 總車位旋鈕
float carCM = 50;
// ============================

LiquidCrystal lcd(pinRS, pinE, pinD4, pinD5, pinD6, pinD7);
int cars = 0;
bool wasIn = false, wasOut = false;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void setColor(int r, int g, int b) {
  digitalWrite(pinR, r);
  digitalWrite(pinG, g);
  digitalWrite(pinB, b);
}

void setup() {
  lcd.begin(16, 2);
  pinMode(trigIn, OUTPUT);  pinMode(echoIn, INPUT);
  pinMode(trigOut, OUTPUT); pinMode(echoOut, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  int capacity = map(analogRead(pinKnob), 0, 1023, 5, 50);

  bool carIn = readCM(trigIn, echoIn) < carCM;
  if (carIn && !wasIn && cars < capacity) cars++;
  wasIn = carIn;
  delay(30);

  bool carOut = readCM(trigOut, echoOut) < carCM;
  if (carOut && !wasOut && cars > 0) cars--;
  wasOut = carOut;

  if (cars > capacity) cars = capacity;
  int left = capacity - cars;

  if (left == 0) {
    showLine(0, "   ** FULL **");
    setColor(HIGH, LOW, LOW);
  } else {
    showLine(0, "SPACES LEFT: " + String(left));
    if (left <= 3) setColor(HIGH, HIGH, LOW);
    else           setColor(LOW, HIGH, LOW);
  }
  showLine(1, "PARKED " + String(cars) + "/" + String(capacity));
  delay(70);
}
```

**情境思考**：

1. 駕駛開車經過入口，大概只有 2 秒看看板。第 0 行的文字設計得夠清楚嗎？你會怎麼改？
2. 💼 停車場主管想知道「今天總共進出幾台車」。目前的看板做得到嗎？要加什麼？

---

### 🔴 Lv.5 病房護理站呼叫看板

**情境**：醫院病房有三間病房，每間一顆**呼叫鈕**和一顆**呼叫燈**。病人按呼叫鈕後，該房呼叫燈亮起，護理站看板依**先後順序**顯示呼叫：第 0 行顯示「最早的呼叫」與已等待秒數，第 1 行顯示還有幾筆在排隊。護理師處理完按「確認鈕」，清除最早的那一筆。另外顯示護理站的室溫（TMP36）。

**元件**：LCD＋3 顆呼叫鈕＋3 顆呼叫燈＋1 顆確認鈕＋1 顆蜂鳴器＋1 顆 TMP36

**Step 0：腳位規劃（本關重點）**

| 元件 | 平行版方案 | I2C 版方案 |
| --- | --- | --- |
| LCD | 6 | 2（A4、A5） |
| 3 顆呼叫鈕 | 3 | 3 |
| 3 顆呼叫燈 | 3 | 3 |
| 確認鈕 | 1 | 1 |
| 蜂鳴器 | 1 | 1 |
| TMP36 | 1（A 腳） | 1（A 腳） |
| **合計** | | |
| **需要借幾隻 A 腳？** | | |
| **你選哪一個？為什麼？** | | |

> 🤔 平行版需要借 A 腳才塞得下；I2C 版則會佔掉 A4、A5。兩種方案都畫出來，比較哪一種擴充空間比較大。

**程式**（以 I2C 版為例；選平行版的同學請改用工具 1 的寫法）：

```cpp
#include <Adafruit_LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
// LCD 使用 I2C 版：SDA 接 A4、SCL 接 A5
int btnCall[3] = { ?, ?, ? };       // 101、102、103 呼叫鈕
int ledCall[3] = { ?, ?, ? };       // 101、102、103 呼叫燈
int btnAck = ?;                     // 護理師確認鈕
int pinBuzzer = ?;                  // 蜂鳴器
int pinTemp = A0;                   // 護理站 TMP36
String roomName[3] = { "101", "102", "103" };
// ============================

Adafruit_LiquidCrystal lcd(0);
int queue[3];                       // 依先後順序排隊的房間編號
unsigned long callTime[3];          // 各房按下的時間
int queueLen = 0;
bool calling[3] = { false, false, false };
int lastCall[3] = { LOW, LOW, LOW };
int lastAck = LOW;
int tick = 0;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  for (int r = 0; r < 3; r++) {
    pinMode(btnCall[r], INPUT);
    pinMode(ledCall[r], OUTPUT);
  }
  pinMode(btnAck, INPUT);
}

void loop() {
  tick++;

  // ---- 病人呼叫：加入隊伍尾端 ----
  for (int r = 0; r < 3; r++) {
    int now = digitalRead(btnCall[r]);
    if (now == HIGH && lastCall[r] == LOW && !calling[r]) {
      delay(50);
      calling[r] = true;
      callTime[r] = millis();
      queue[queueLen] = r;
      queueLen++;
      tone(pinBuzzer, 1000, 200);
    }
    lastCall[r] = now;
    digitalWrite(ledCall[r], calling[r] ? HIGH : LOW);
  }

  // ---- 護理師確認：移除隊伍最前面那一筆 ----
  int now = digitalRead(btnAck);
  if (now == HIGH && lastAck == LOW && queueLen > 0) {
    delay(50);
    calling[queue[0]] = false;
    for (int i = 0; i < queueLen - 1; i++) queue[i] = queue[i + 1];
    queueLen--;
  }
  lastAck = now;

  // ---- 看板 ----
  if (queueLen > 0) {
    int r = queue[0];
    unsigned long waitSec = (millis() - callTime[r]) / 1000;
    showLine(0, "CALL " + roomName[r] + "  " + String(waitSec) + "s");
    showLine(1, "QUEUE " + String(queueLen) + "  " + String(readC(pinTemp), 1) + "C");
    if (waitSec > 60 && tick % 20 == 0) tone(pinBuzzer, 1500, 150);   // 等太久再提醒
  } else {
    showLine(0, "NO CALLS");
    showLine(1, "ROOM TEMP " + String(readC(pinTemp), 1) + "C");
  }
  delay(50);
}
```

**情境思考**：

1. 你最後選平行版還是 I2C 版？用腳位計算表說明理由。
2. 看板只顯示「最早的呼叫」，而不是「最新的呼叫」。這是什麼排隊原則？醫院一定要用這個原則嗎？有沒有例外？
3. 💼 「呼叫到確認的等待秒數」是醫院的服務品質指標（回應時間）。如果把每一筆的等待秒數記錄下來，護理長可以做什麼分析？

---

### 🏆 Lv.6 BOSS 關：物聯網顧問提案

**情境**：你的顧問公司接到新客戶。從下面三個案例中**任選一個**，自己完成需求分析、元件選擇、腳位規劃與 Tinkercad 實作。

> **案例 A：餐廳廚房出餐看板**
> 外場按按鈕新增訂單（三種餐點各一顆按鈕），廚房看板依順序顯示待出餐的餐點與等待時間；廚師按「出餐鈕」完成一筆。等待超過設定時間（旋鈕調整）時 RGB 轉紅並提醒。
>
> **案例 B：工廠能源看板**
> 用旋鈕模擬目前用電量、TMP36 監測機台溫度、光敏電阻判斷照明是否忘了關。看板分頁顯示各項數據，用電超過契約容量時警示，下班後偵測到燈還亮著時提醒。
>
> **案例 C：自訂情境**
> 自己找一個需要現場看板的情境（例如：圖書館座位數看板、公車站到站提示、自助洗衣店機台狀態、健身房人數看板）。

**最低規格**：

- 1 塊 LCD（平行版或 I2C 版，須說明選擇理由）
- 至少 2 種感測器或輸入元件、1 顆按鈕
- 總共使用 10 隻以上腳位
- 至少 2 頁顯示內容，或異常時的自動切換顯示
- 程式可以從專案一～四或 Lv.1～Lv.5 改寫，說明你是從哪裡改來的、改了哪些設定

**繳交內容**：

1. **需求分析**：誰會看這塊看板？他們在什麼情況下看？看多久？（3～5 句）
2. **看板版面設計**：每一頁的草圖（2 行 × 16 格），以及為什麼這樣排
3. **腳位規劃表**：平行版、I2C 版兩種方案的腳位計算，以及你的選擇
4. **Tinkercad 實作**：電路截圖＋LCD 測試器截圖
5. **KPI 說明**：看板上顯示的數字中，哪一個是最重要的管理指標？管理者看到後會做什麼決策？

**驗收標準**：

- [ ] LCD 接線正確，文字清楚無殘影
- [ ] 版面符合看板設計原則（重要的在上、有單位、異常搶眼）
- [ ] LCD 版本選擇合理，腳位規劃表有比較兩種方案
- [ ] 計數類功能「只在狀態改變的瞬間」計一次
- [ ] 麵包板依功能分區

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 畫面全黑、沒有任何東西 | VCC、GND、背光 LED+／LED− 是否接好 | 照接線表重接電源 |
| 第 0 行出現一排白色方塊，但沒有字 | RW 是否接 GND；RS、E 是否接對；程式腳位順序 | `LiquidCrystal lcd(RS, E, D4, D5, D6, D7)` 順序不能錯 |
| 有字但看不清楚 | V0（對比度） | 轉動對比度旋鈕，或 V0 直接接 GND 試試 |
| 出現亂碼 | DB4～DB7 是否接錯順序、是否誤接 DB0～DB3 | 只接 DB4～DB7，順序對照程式 |
| I2C 版完全沒反應 | SDA 是否接 A4、SCL 是否接 A5；程式是否用 `Adafruit_LiquidCrystal` | 對調檢查，並加上 `setBacklight(1)` |
| 數字變短後尾巴殘留舊數字 | 是否用 `showLine()` 顯示 | 改用 `showLine()`，它會自動補空白 |
| 中文顯示成亂碼 | LCD 不支援中文 | 改用英文與縮寫 |
| I2C 版接好後，A4、A5 上的其他元件失效 | A4、A5 已被 LCD 佔用 | 把其他元件改接別的腳 |
| 程式無法上傳 | 設定區是否還有 `?` 沒填 | 填完所有腳位 |

---

## 成果繳交與評分

### 範例專案繳交

每個專案各自建立一個 Tinkercad 檔案，命名格式：`W11-專案編號-學號`（例如 `W11-P3-B12345678`）。

| 繳交項目 | 專案一 | 專案二 | 專案三 | 專案四 |
| --- | --- | --- | --- | --- |
| Tinkercad 分享連結 | ✅ | ✅ | ✅ | ✅ |
| 看板版面草圖 | ✅ | ✅ | ✅ | ✅ |
| 腳位規劃表 | ✅ | ✅ | ✅ | ✅（含兩種方案比較） |
| 完成 Step 3 的電路截圖 | ✅ | ✅ | ✅ | ✅ |
| 情境思考題作答 | ✅ | ✅ | ✅ | ✅ |

> 🎯 **最低完成標準**：專案一、二完成 Step 3，專案三、四至少完成 Step 2。

### 練習題繳交

| 關卡 | 繳交內容 | 建議定位 |
| --- | --- | --- |
| Lv.1～Lv.2 | Tinkercad 連結＋看板草圖＋情境思考題 | 課堂暖身，全班必做 |
| Lv.3～Lv.4 | ＋腳位規劃表 | 課後作業，必做 |
| Lv.5 | ＋兩種方案腳位比較＋麵包板擺放截圖 | 選做加分 |
| Lv.6 BOSS | 完整顧問提案（需求分析、版面設計、腳位規劃、實作、KPI 說明） | 可分組 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 電路正確性 | 30% | LCD 正確顯示，每個 Step 功能正常 |
| 腳位規劃與版本選擇 | 20% | 正確計算腳位，LCD 版本選擇有理由 |
| 看板版面設計 | 20% | 符合看板設計原則，資訊取捨合理 |
| 情境思考題 | 20% | 尤其是 KPI 與 💼 管理面向論述 |
| 麵包板擺放 | 10% | LCD 線路整齊，分區清楚 |

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 40 分鐘 | 認識 LCD 兩種版本、看板設計原則、全班一起接平行版並跑 LCD 測試器 |
| 第 1 節後段 | 專案一 |
| 第 2 節 | 專案二、專案三（分組進行） |
| 第 3 節前段 | 專案四（I2C 版） |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，Lv.3 之後帶回家 |

> 💡 **教學建議**：平行版 LCD 有 12 條線，是學生最容易接錯的地方。建議第 1 節全班一起接，確認每個人的 LCD 都顯示 `LCD TEST OK` 之後，再開始專案。專案四再讓學生體驗 I2C 版「4 條線就搞定」的對比。

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| LCD 16×2 | 2 行、每行 16 格的文字顯示器，只能顯示英文、數字、符號 |
| 平行版 LCD | 用 RS、E、DB4～DB7 六條線控制，佔 6 隻腳 |
| I2C 版 LCD | 只用 SDA（A4）、SCL（A5）兩條線控制，佔 2 隻腳 |
| 對比度（V0） | 調整字的深淺 |
| `showLine()` | 已寫好的函式，在指定行顯示文字並自動蓋掉舊字 |
| 分頁顯示 | 螢幕太小時，用按鈕切換不同主題的頁面 |
| 看板管理（Visual Management） | 讓現場狀況一眼可見的管理方法 |
| 稼動率（Availability） | 運轉時間占總時間的比例，OEE 指標之一 |
| 先進先出（FIFO） | 先來的先處理，排隊與叫號的基本原則 |

---

## 反思與延伸討論

1. 本週每個看板都只有 32 格。這逼著你做取捨：什麼要顯示、什麼不顯示、什麼放第一行。在職場上做簡報、寫報告、設計儀表板時，這種「資訊取捨」的能力有多重要？
2. 專案四和 Lv.5 都要比較「平行版 vs I2C 版」。平行版便宜但吃腳位、I2C 版省腳位但佔掉 A4、A5。這種「沒有完美方案，只有適合方案」的決策，你在其他管理情境中遇過嗎？
3. 專案三的稼動率、Lv.5 的回應時間，都是「把現場行為轉成數字」。但看板只能顯示「現在」，不能保存「過去」。如果要做到「每週檢討、持續改善」，資料還需要怎麼處理？這就是下半學期物聯網「聯網」要解決的問題。

---

## 本週檢核清單

- [ ] 能正確接上平行版 LCD（RW 接 GND、只接 DB4～DB7），並用 LCD 測試器確認
- [ ] 能正確接上 I2C 版 LCD（SDA 接 A4、SCL 接 A5）
- [ ] 能依腳位需求，判斷該用平行版還是 I2C 版，並說明理由
- [ ] 能依看板設計原則規劃 2 行 × 16 格的版面
- [ ] 能用分頁顯示更多資訊，並設計異常時自動切換
- [ ] 能說明稼動率、回應時間等看板指標的管理意義
- [ ] 完成 4 個獨立專案，並各自繳交 Tinkercad 檔案
- [ ] 完成練習題 Lv.1～Lv.4，挑戰 Lv.5 與 Lv.6

---

## 下週預告

第 12 週是 Arduino 基礎元件的最後一週——加入 **4×4 矩陣鍵盤**，打造電子密碼鎖，並進行 12 週以來的期末統整。鍵盤要佔 8 隻腳，配上本週的 LCD，腳位規劃會是一大挑戰——本週學到的「I2C 版省腳位」下週就派上用場了。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 10 週](week-10.md) | [下一週：第 12 週 — 期末統整與電子密碼鎖 ➡](week-12.md)
