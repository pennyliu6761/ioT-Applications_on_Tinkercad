[⬅ 回目錄](../README.md) | [⬅ 上一週：第 10 週](week-10.md) | [下一週：第 12 週 — 會輸入的智慧場域 ➡](week-12.md)

---

# 第 11 週：會說話的智慧場域

## 主題專案：把數據變成現場看得懂的看板

> 🎯 **本週定位**：前十週的系統用燈號、聲音和 Serial Monitor 回報狀態，但現場人員看不到電腦螢幕。這一週加入 **LCD 16×2 顯示器**——系統第一次可以直接「說」出數字和文字：溫度幾度、產量多少、會議室還要用多久。
>
> LCD 有兩種版本：**平行版佔 6 隻腳**、**I2C 版只佔 2 隻（固定 A4、A5）**。**先算腳位再選版本**是本週最重要的配線決策——同一個情境，抽到不同限制卡就會選出不同版本。另外，**看板要顯示什麼、怎麼排版**，是本週最重要的情境思考。
>
> 每一個關卡照「五個固定環節」進行：抽**客戶限制卡** → 回答**接線前決策題** → 照**接線任務**一步步接 → 套用**程式模板**（只改腳位區、參數區） → 完成**配線變化挑戰**（程式一個字都不改，只改線）。
>
> 🏁 **本週成果**：完成 **4 個關卡** ＋ **練習題 Lv.1～Lv.6**（套用本週的程式模板 T25～T28）。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：LCD 16×2 顯示器](#認識新元件lcd-162-顯示器)
- [本週配線新技巧](#本週配線新技巧)
- [看板設計原則](#看板設計原則)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [關卡怎麼玩：五個固定環節](#關卡怎麼玩五個固定環節)
- [關卡 1：會議室門口狀態牌（模板 T25）](#關卡-1會議室門口狀態牌模板-t25)
- [關卡 2：藥品冷藏櫃溫度看板（模板 T26）](#關卡-2藥品冷藏櫃溫度看板模板-t26)
- [關卡 3：產線績效看板（模板 T27）](#關卡-3產線績效看板模板-t27)
- [關卡 4：冷鏈倉儲戰情中心（模板 T28）](#關卡-4冷鏈倉儲戰情中心模板-t28)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 百貨公司哺乳室看板](#-lv1-百貨公司哺乳室看板)
  - [Lv.2 溫室育苗溫度看板](#-lv2-溫室育苗溫度看板)
  - [Lv.3 手工皂包裝線看板](#-lv3-手工皂包裝線看板)
  - [Lv.4 物流揀貨區績效看板](#-lv4-物流揀貨區績效看板)
  - [Lv.5 生鮮超市冷藏戰情看板](#-lv5-生鮮超市冷藏戰情看板)
  - [🏆 Lv.6 BOSS 關：顧問提案](#-lv6-boss-關顧問提案)
  - [📝 配線設計單](#-配線設計單)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～10 週）

- ✅ LED、RGB、蜂鳴器、伺服、直流馬達五種輸出元件
- ✅ 按鈕、旋鈕、光敏電阻、TMP36、HC-SR04 五種輸入元件
- ✅ 配線技巧：兩燈共腳、光敏電阻反相、全亮全滅顏色接一般腳、逐腳判斷 `~` 腳、伺服共用訊號線、感測器整組對調、L293D 接法 A～D、馬達並聯、借 A 腳

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| LCD 16×2 顯示器 | 2 行、每行 16 格的文字螢幕 | 工廠看板、電梯樓層顯示、收銀機螢幕 |
| 平行版 vs I2C 版 | 同樣的螢幕，佔 6 隻腳或 2 隻腳 | 同功能設備，選「接線多但便宜」或「接線少但佔固定介面」 |
| 看板設計 | 32 格要放什麼、怎麼排 | 目視管理（Visual Management） |
| 程式模板 T25～T28 | 本週四個新模板 | 公司的標準表單 |

### 本週可用元件（只限這些）

| 元件 | 第幾週學的 |
| --- | --- |
| 單色 LED、RGB LED、220Ω | 第 1、3 週 |
| 按鈕、10kΩ | 第 2 週 |
| 旋鈕 | 第 4 週 |
| 蜂鳴器、光敏電阻 | 第 5 週 |
| TMP36 | 第 6 週 |
| 伺服馬達 | 第 7 週 |
| HC-SR04 | 第 9 週 |
| 直流馬達、L293D | 第 10 週 |
| LCD 16×2（平行版、I2C 版） | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：客戶說：「你們的系統很聰明，可是我們的員工看不懂燈號的意思，也不會開 Serial Monitor。能不能直接用文字告訴他們？」這週的四個委託，都要做一塊現場看板。
>
> | 關卡 | 客戶 | 委託內容 | 套用模板 | 難度 |
> | --- | --- | --- | --- | --- |
> | 1 | 共享辦公室 | 會議室門口狀態牌 | T25 狀態牌 | ⭐ |
> | 2 | 社區藥局 | 藥品冷藏櫃溫度看板 | T26 溫度看板 | ⭐⭐ |
> | 3 | 食品工廠 | 產線績效看板 | T27 產線績效看板 | ⭐⭐⭐ |
> | 4 | 冷鏈物流 | 倉儲戰情中心 | T28 多頁戰情看板 | ⭐⭐⭐⭐ |

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

### 兩種版本

Tinkercad 元件庫裡有兩種 LCD：**「LCD 16 x 2」**（平行版）和 **「LCD 16 x 2 (I2C)」**（I2C 版）。

| 比較 | 平行版 | I2C 版 |
| --- | --- | --- |
| 佔用 Arduino 腳位 | **6 隻**（任何數位腳或 A 腳） | **2 隻**（固定是 A4、A5） |
| 接線難度 | 16 隻腳，要接 12 條線 | 4 隻腳，只接 4 條線 |
| 可調字的深淺、背光 | 可以（V0、LED 腳） | 不行 |
| 限制 | 吃掉一半的數位腳 | A4、A5 被佔用後，**不能再借來接其他東西** |

### 平行版接法（12 條線）

| LCD 腳位 | 白話說明 | 接到哪裡 |
| --- | --- | --- |
| GND（VSS） | 螢幕接地 | GND |
| VCC（VDD） | 螢幕電源 | 5V |
| V0 | 對比度（字的深淺） | GND（最深）；或接旋鈕中間腳（技巧 2） |
| RS | 訊號線 ① | 數位腳 |
| RW | 讀寫切換 | **GND**（我們只寫不讀） |
| E | 訊號線 ② | 數位腳 |
| DB0～DB3 | 不使用 | **不接** |
| DB4、DB5、DB6、DB7 | 訊號線 ③～⑥ | 數位腳 |
| LED+（A） | 背光正極 | 5V（或接燈的腳位，技巧 3） |
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

本週所有模板都附上 `showLine()`，你**不需要修改它**，只要知道它會在指定的那一行顯示文字，並自動補空白、蓋掉舊字、切掉超過 16 格的部分：

```cpp
showLine(0, "TEMP: 4.2 C");     // 在第 0 行（上面）顯示
showLine(1, "STATUS: OK");      // 在第 1 行（下面）顯示
```

本週模板的**腳位區**都有同一段 LCD 設定：

```cpp
bool useI2C = false;                // 用 I2C 版改成 true（自動使用 A4、A5）
int lcdRS = ?, lcdE = ?, lcdD4 = ?, lcdD5 = ?, lcdD6 = ?, lcdD7 = ?;   // 平行版 6 隻腳；I2C 版全部填 -1
```

---

## 本週配線新技巧

### 技巧 1：先算腳位，再選 LCD 版本

```
其他元件要用的一般腳 ＋ 6（平行版）≤ 可用腳位？
   ├─ 是 → 平行版（還保留 A4、A5 可以借）
   └─ 否 → I2C 版（只佔 A4、A5）
```

> 💡 Arduino Uno 可用腳位：Pin 2～13（12 隻）＋ A0～A5（6 隻）＝ 18 隻。平行版的 6 條訊號線也可以借 A 腳。

### 技巧 2：字太淡或太深 → V0 接旋鈕（平行版）

把 V0 從 GND 改接到一顆旋鈕的中間腳（旋鈕兩側接 5V、GND），轉旋鈕就能調字的深淺。**程式一個字都不用改**——這顆旋鈕不接 Arduino。

### 技巧 3：背光跟著燈走（平行版）

LCD 的 LED+ 平常接 5V（背光一直亮）。如果客戶要「營業中燈亮時背光才亮」，把 LED+ 改接到**那顆燈的腳位**（串一顆 220Ω），背光就會和那顆燈同步亮滅——**不用改程式**。

> ⚠️ 這是 Tinkercad 模擬可以做的技巧；真實硬體的背光比較耗電，腳位可能推不動，要加電晶體。

---

## 看板設計原則

LCD 只有 32 格，比一則簡訊還短。**看板設計的關鍵不是「能顯示什麼」，而是「最該顯示什麼」**。

| 原則 | 說明 | 範例 |
| --- | --- | --- |
| ① 最重要的放第 0 行 | 現場人員第一眼看上面 | 溫度、目前號碼、異常訊息 |
| ② 用縮寫，但要一致 | 16 格很擠，要省字 | `TEMP`、`QTY`、`AVL`（Available） |
| ③ 異常要搶眼 | 異常時整行換成大寫警示 | `!! TOO HOT !!` |
| ④ 單位要寫 | 數字沒有單位等於沒說 | `4.2 C`、`35 cm`、`12 pcs` |
| ⑤ 太多就分頁 | 用按鈕切換，每頁一個主題 | 第 1 頁產量、第 2 頁稼動率 |

> 🏭 **目視管理（Visual Management）**：豐田生產系統中，看板讓「異常一眼可見」。好的看板讓現場人員**不用問、不用算**，就知道現在該做什麼。

每個關卡都要畫**看板草圖**（2 行 × 16 格），填好再接線：

```
┌────────────────┐
│                │  ← 第 0 行：最重要的資訊
│                │  ← 第 1 行：補充或異常訊息
└────────────────┘
```

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **先算腳位，再選 LCD 版本** | 平行版 6 隻、I2C 版 2 隻（A4、A5） |
| ② **平行版：RW 接 GND、DB0～DB3 不接** | 只有 RS、E、DB4～DB7 接 Arduino |
| ③ LCD 訊號線接一般腳就好 | 不要佔 `~` 腳 |
| ④ 用 I2C 版時，A4、A5 不能再借來接其他元件 | 旋鈕、TMP36、光敏電阻只剩 A0～A3 |
| ⑤ 字太淡 → V0 接旋鈕；背光跟燈 → LED+ 接燈的腳位 | 平行版限定，不用改程式 |
| ⑥ 有伺服，9、10 不能調；有蜂鳴器，3、11 不能調 | 兩者都有時只剩 5、6 |
| ⑦ 每條燈線一顆 220Ω；按鈕、光敏電阻一顆 10kΩ | LCD、HC-SR04、蜂鳴器、旋鈕、TMP36 不用 |

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

**檢查標準**：第 0 行出現文字、第 1 行數字持續增加。如果畫面全黑或全白方塊，先檢查電源、V0、RW（平行版）；出現亂碼，檢查 DB4～DB7 有沒有接錯順序。

### 🔧 其他工具（沿用前幾週）

| 工具 | 用途 | 出處 |
| --- | --- | --- |
| 接線驗證器 | 檢查燈的腳位 | 第 3 週起 |
| 類比讀值檢查器 | 檢查旋鈕、光敏電阻 | 第 5 週 |
| 溫度讀值檢查器 | 檢查 TMP36 | 第 6 週 |
| 超音波測距檢查器 | 檢查 HC-SR04 | 第 9 週 |
| 馬達測試器 | 檢查 L293D 與馬達 | 第 10 週 |

---

## 關卡怎麼玩：五個固定環節

| 環節 | 你要做什麼 | 產出 |
| --- | --- | --- |
| 🃏 **① 抽客戶限制卡** | 依座號尾數找到你的限制卡 | 圈出你的限制卡 |
| ✏️ **② 接線前決策題** | **先算腳位、選 LCD 版本**，畫出**看板草圖**，填好**腳位規劃表** | 決策題答案＋草圖＋規劃表 |
| 🔌 **③ 接線任務** | 照 Step 1 → 2 → 3 接線，每一步都用工具檢查 | 每一步的檢查截圖 |
| 💻 **④ 套用程式模板** | 只改「腳位區」和「參數區」，再把功能開關逐一打開 | 可運作的 Tinkercad 檔案 |
| 🔄 **⑤ 配線變化挑戰** | **程式一個字都不能改**，只靠改線完成客戶的臨時需求 | 修改後的截圖＋說明 |

| 座號尾數 | 抽到 |
| --- | --- |
| 1、4、7 | 限制卡 A |
| 2、5、8 | 限制卡 B |
| 3、6、9、0 | 限制卡 C |

---

## 關卡 1：會議室門口狀態牌（模板 T25）

⭐ **難度**：LCD ＋ 按鈕，再逐步加紅綠燈與超時提醒

### 📖 情境

共享辦公室每間會議室門口放一塊狀態牌。平常顯示會議室名稱和「可預約」；有人開會時，按一下按鈕切換成「使用中」，並顯示**已經使用了多久**；會議結束再按一下恢復。門口有紅綠燈讓走廊上的人遠遠就看得到。會議超過預約時間（模擬用 **60 秒**）時，看板顯示 `TIME UP`，蜂鳴器每 5 秒提醒一次。

### 🎯 本關配線學習目標

- 第一次接 LCD：平行版 12 條線或 I2C 版 4 條線
- 依限制卡判斷版本與腳位

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：平行版 LCD；會議室名稱 `ROOM A` |
| **B** | 客戶指定 **I2C 版** LCD；**會議室裡面、走廊外面各一組紅綠燈**，只能用 2 隻腳；名稱 `ROOM B` |
| **C** | 平行版 LCD；**Pin 2～5** 已被門禁系統佔用；名稱 `BOARD RM` |

### ✏️ ② 接線前決策題

1. 平行版 LCD 的 16 隻腳中，哪幾隻接 Arduino？哪幾隻接 5V、GND？哪幾隻不接？
2. 本關一共要幾隻 Arduino 腳位？（LCD、按鈕、紅燈、綠燈、蜂鳴器）
3. 限制卡 B：用 I2C 版時，腳位區的 6 隻平行腳要填什麼？4 顆燈怎麼接？
4. 限制卡 C：扣掉 Pin 2～5，一般腳還剩幾隻？平行版放得下嗎？不夠要怎麼辦？
5. 畫出「可預約」「使用中」「超時」三種畫面的看板草圖。

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| LCD（平行版 RS、E、D4～D7／I2C 版 A4、A5） | |
| 切換鈕 | |
| 綠燈（可預約）／紅燈（使用中） | |
| 蜂鳴器 | |

### 🔌 ③ 接線任務

**Step 1：LCD＋切換鈕**

1. 平行版：照接法表接 12 條線（V0 先接 GND）；I2C 版：接 4 條線。
2. 切換鈕配 10kΩ。
3. ✅ 檢查：**工具 1 或工具 2**，確認有字。

**Step 2：紅綠燈**

1. 綠燈、紅燈各串 220Ω（限制卡 B：另一組接在同樣的兩隻腳，每顆各串 220Ω）。
2. ✅ 檢查：接線驗證器。

**Step 3：蜂鳴器**

1. 接好蜂鳴器。
2. ✅ 檢查：蜂鳴器測試器。

### 💻 ④ 套用程式模板 T25

```cpp
// ===== 程式模板 T25：狀態牌 =====
#include <LiquidCrystal.h>
#include <Adafruit_LiquidCrystal.h>

// ----- 腳位區（沒接的填 -1）-----
bool useI2C = false;                // 用 I2C 版改成 true（自動使用 A4、A5）
int lcdRS = ?, lcdE = ?, lcdD4 = ?, lcdD5 = ?, lcdD6 = ?, lcdD7 = ?;   // 平行版；I2C 版全部填 -1
int btnToggle = ?;                  // 切換鈕
int ledFree = ?;                    // 綠燈（可預約）
int ledBusy = ?;                    // 紅燈（使用中）
int pinBuzzer = ?;                  // 蜂鳴器

// ----- 參數區（文字只能用英文、數字，最多 16 格）-----
String roomName = "ROOM A";         // 第 0 行顯示的名稱
String freeText = "AVAILABLE";      // 可預約時第 1 行
String busyText = "IN USE";         // 使用中時第 0 行後面的字
int limitSec  = 60;                 // 使用超過幾秒算超時
int remindGap = 5;                  // 超時後每隔幾秒提醒一次
int remindTone = 1000;

// ----- 功能開關 -----
bool useLights   = false;           // Step 2：紅綠燈
bool useReminder = false;           // Step 3：超時提醒

// ===== 以下不用改 =====
LiquidCrystal* lcdP = 0;
Adafruit_LiquidCrystal* lcdI = 0;
bool busy = false;
unsigned long startMs = 0, lastRemind = 0;
int lastBtn = LOW, meetings = 0;

void lcdStart() {
  if (useI2C) { lcdI = new Adafruit_LiquidCrystal(0); lcdI->begin(16, 2); lcdI->setBacklight(1); }
  else        { lcdP = new LiquidCrystal(lcdRS, lcdE, lcdD4, lcdD5, lcdD6, lcdD7); lcdP->begin(16, 2); }
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  text = text.substring(0, 16);
  if (useI2C) { lcdI->setCursor(0, row); lcdI->print(text); }
  else        { lcdP->setCursor(0, row); lcdP->print(text); }
}

String mmss(unsigned long ms) {
  unsigned long s = ms / 1000;
  String sec = (s % 60 < 10) ? "0" + String(s % 60) : String(s % 60);
  return String(s / 60) + ":" + sec;
}

void setup() {
  lcdStart();
  pinMode(btnToggle, INPUT);
  if (ledFree >= 0) pinMode(ledFree, OUTPUT);
  if (ledBusy >= 0) pinMode(ledBusy, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int now = digitalRead(btnToggle);
  if (now == HIGH && lastBtn == LOW) {
    delay(50);
    busy = !busy;
    if (busy) { startMs = millis(); meetings++; }
    Serial.println(busy ? "Meeting START" : "Meeting END");
  }
  lastBtn = now;

  if (busy) {
    unsigned long used = millis() - startMs;
    bool over = useReminder && used / 1000 >= (unsigned long)limitSec;
    showLine(0, roomName + " " + busyText);
    showLine(1, over ? "TIME UP! " + mmss(used) : "TIME " + mmss(used));
    if (over && pinBuzzer >= 0 && millis() - lastRemind >= (unsigned long)remindGap * 1000) {
      tone(pinBuzzer, remindTone, 200);
      lastRemind = millis();
    }
  } else {
    showLine(0, roomName);
    showLine(1, freeText);
  }

  if (useLights) {
    if (ledFree >= 0) digitalWrite(ledFree, busy ? LOW : HIGH);
    if (ledBusy >= 0) digitalWrite(ledBusy, busy ? HIGH : LOW);
  }
  delay(100);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表；限制卡 B 的 `useI2C = true`、6 隻平行腳填 `-1` |
| 參數區 | `roomName` 依限制卡；其他用預設值 |
| 功能開關 | Step 2 → `useLights = true`；Step 3 → `useReminder = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「看板的字太淡，要能調深淺」（限制卡 B 的同學：改成「會議室裡面的紅綠燈拆掉」） | （提示：技巧 2） |
| 「沒人開會時看板太亮很耗電，背光只要開會時才亮」（限制卡 B 的同學：說明為什麼 I2C 版做不到） | （提示：技巧 3） |
| 「看板出現亂碼」——老師會動手腳，找出來並修好 | |

### 💼 情境思考

1. 有人開完會忘了按按鈕，看板一直顯示「使用中」。你會建議用什麼感測器自動判斷會議室有沒有人？要裝在哪裡？
2. Serial Monitor 記錄了每次會議的開始、結束。這些資料對辦公室管理者有什麼用？（提示：會議室使用率、要不要多租一間）

### ✅ 關卡 1 驗收

- [ ] LCD 正確顯示會議室名稱與「可預約」
- [ ] 按按鈕切換使用中，使用時間每秒增加；再按恢復
- [ ] 紅綠燈正確（限制卡 B：兩組同步）
- [ ] 超過 60 秒顯示 `TIME UP`，每 5 秒提醒一次
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 2：藥品冷藏櫃溫度看板（模板 T26）

⭐⭐ **難度**：LCD ＋ TMP36 ＋ 歸零鈕 ＋ 警示燈 ＋ 蜂鳴器

### 📖 情境

第 6 週的藥品冷藏櫃（2～8°C）升級版。藥局希望看板上除了即時溫度，還要顯示**今天的最高溫和最低溫**（衛生稽查時要看）。藥師每天早上按一下「歸零鈕」重新記錄。溫度超出範圍時，看板第 1 行改成警示訊息，蜂鳴器和警示燈同時提醒。

### 🎯 本關配線學習目標

- TMP36 和 I2C 版 LCD 搶 A 腳時，怎麼分配
- 依限制卡練習平行版借 A 腳

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：藥品冷藏櫃 **2～8°C**，平行版 LCD |
| **B** | **疫苗冷凍櫃 −25～−15°C**，客戶指定 **I2C 版** LCD |
| **C** | 藥品冷藏櫃 2～8°C，平行版 LCD；警示燈**櫃上、藥師桌各一顆**（只能用 1 隻腳）；歸零鈕必須接 **A3**；**Pin 13** 已壞掉不能用 |

### ✏️ ② 接線前決策題

1. TMP36 平面朝自己，三隻腳各接哪裡？要接 A 腳的哪一隻？
2. 限制卡 B：用 I2C 版時，A 腳還剩哪幾隻可以接 TMP36？
3. 限制卡 B：溫度是負數，TMP36 量得到嗎？看板上的 `-20.5 C` 要佔幾格？
4. 限制卡 C：歸零鈕接在 A3，需要 10kΩ 嗎？
5. 畫出「正常」「太熱」「太冷」三種畫面的看板草圖。

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| LCD | |
| TMP36 | |
| 歸零鈕 | |
| 警示燈（C：兩顆共用） | |
| 蜂鳴器 | |

### 🔌 ③ 接線任務

**Step 1：LCD＋TMP36**

1. 接好 LCD；TMP36 中間腳接 A 腳。
2. ✅ 檢查：LCD 測試器、溫度讀值檢查器。

**Step 2：歸零鈕**

1. 歸零鈕配 10kΩ。
2. ✅ 檢查：按下時用 Serial Monitor 確認（Step 2 開關打開後會印出 `MAX/MIN reset`）。

**Step 3：警示燈＋蜂鳴器**

1. 警示燈串 220Ω（限制卡 C：兩顆共腳）；接好蜂鳴器。
2. ✅ 檢查：接線驗證器、蜂鳴器測試器。

### 💻 ④ 套用程式模板 T26

```cpp
// ===== 程式模板 T26：溫度看板 =====
#include <LiquidCrystal.h>
#include <Adafruit_LiquidCrystal.h>

// ----- 腳位區（沒接的填 -1）-----
bool useI2C = false;                // 用 I2C 版改成 true（自動使用 A4、A5）
int lcdRS = ?, lcdE = ?, lcdD4 = ?, lcdD5 = ?, lcdD6 = ?, lcdD7 = ?;   // 平行版；I2C 版全部填 -1
int pinTemp  = A0;                  // TMP36
int btnReset = ?;                   // 歸零鈕
int ledAlarm = ?;                   // 警示燈
int pinBuzzer = ?;                  // 蜂鳴器

// ----- 參數區 -----
float lowLine  = 2.0;               // 低於這個溫度算太冷
float highLine = 8.0;               // 高於這個溫度算太熱
String placeName = "FRIDGE";        // 第 0 行名稱（建議 8 格以內）
int beepGap  = 2000;                // 異常時每隔幾毫秒嗶一次
int beepTone = 2000;

// ----- 功能開關 -----
bool useMinMax = false;             // Step 2：最高最低溫＋歸零鈕
bool useAlarm  = false;             // Step 3：警示燈＋蜂鳴器

// ===== 以下不用改 =====
LiquidCrystal* lcdP = 0;
Adafruit_LiquidCrystal* lcdI = 0;
float tMax = -99, tMin = 199;
int lastBtn = LOW;
unsigned long lastBeep = 0;

void lcdStart() {
  if (useI2C) { lcdI = new Adafruit_LiquidCrystal(0); lcdI->begin(16, 2); lcdI->setBacklight(1); }
  else        { lcdP = new LiquidCrystal(lcdRS, lcdE, lcdD4, lcdD5, lcdD6, lcdD7); lcdP->begin(16, 2); }
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  text = text.substring(0, 16);
  if (useI2C) { lcdI->setCursor(0, row); lcdI->print(text); }
  else        { lcdP->setCursor(0, row); lcdP->print(text); }
}

float readC(int pin) {
  return (analogRead(pin) * 5.0 / 1023.0 - 0.5) * 100.0;
}

void setup() {
  lcdStart();
  if (btnReset >= 0) pinMode(btnReset, INPUT);
  if (ledAlarm >= 0) pinMode(ledAlarm, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float t = readC(pinTemp);
  if (t > tMax) tMax = t;
  if (t < tMin) tMin = t;

  if (useMinMax && btnReset >= 0) {
    int now = digitalRead(btnReset);
    if (now == HIGH && lastBtn == LOW) {
      delay(50);
      tMax = t;
      tMin = t;
      Serial.println("MAX/MIN reset");
    }
    lastBtn = now;
  }

  int state = 0;                                           // 0 正常、1 太熱、2 太冷
  if (t > highLine)     state = 1;
  else if (t < lowLine) state = 2;

  showLine(0, placeName + " " + String(t, 1) + " C");
  if (useAlarm && state == 1)      showLine(1, "!! TOO HOT !!");
  else if (useAlarm && state == 2) showLine(1, "!! TOO COLD !!");
  else if (useMinMax)              showLine(1, "H" + String(tMax, 1) + " L" + String(tMin, 1));
  else                             showLine(1, "RANGE " + String(lowLine, 0) + "~" + String(highLine, 0) + " C");

  if (useAlarm) {
    if (ledAlarm >= 0) digitalWrite(ledAlarm, state > 0 ? HIGH : LOW);
    if (state > 0 && pinBuzzer >= 0 && millis() - lastBeep > (unsigned long)beepGap) {
      tone(pinBuzzer, beepTone, 300);
      lastBeep = millis();
    }
  }
  delay(200);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表；限制卡 B 的 `useI2C = true`；限制卡 C 的 `btnReset = A3` |
| 參數區 | `lowLine`、`highLine` 依限制卡；限制卡 B 的 `placeName` 改成 `"FREEZER"` |
| 功能開關 | Step 2 → `useMinMax = true`；Step 3 → `useAlarm = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「藥師桌也要一顆警示燈」（限制卡 C 的同學：改成「再加一顆蜂鳴器放在藥師桌」） | |
| 「夜間背光太刺眼，背光只要在異常時才亮」（限制卡 B 的同學：說明 I2C 版要怎麼辦） | （提示：技巧 3） |
| 「溫度一直顯示 −50 °C 左右」——老師會動手腳，找出來並修好 | （提示：TMP36 的方向） |

### 💼 情境思考

1. 衛生稽查只看「今天最高、最低溫」。如果夜裡停電 2 小時、溫度升到 15°C 又降回來，藥師早上歸零前有看看板，就能發現嗎？歸零後呢？
2. 「警示燈＋蜂鳴器」和「看板文字」各自適合讓誰看到？為什麼兩種都要？

### ✅ 關卡 2 驗收

- [ ] 看板顯示即時溫度，數字有單位
- [ ] 第 1 行顯示最高、最低溫，按歸零鈕重新記錄
- [ ] 超出範圍時第 1 行換成警示訊息，警示燈亮、蜂鳴器間歇提醒（限制卡 B：冷凍範圍正確）
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 3：產線績效看板（模板 T27）

⭐⭐⭐ **難度**：計數 ＋ 旋鈕設目標 ＋ 達標燈 ＋ 停機紀錄 ＋ 分頁 → **工管人最熟悉的看板**

### 📖 情境

食品工廠包裝線末端的看板，讓班長隨時掌握兩個關鍵指標：

- **第 1 頁：產量達成率**——超音波計算通過的產品數，旋鈕設定本班目標產量（10～200），顯示「實際／目標」與達成率 %；達標時達標燈亮、蜂鳴器提示一次
- **第 2 頁：稼動率**——作業員按「停機鈕」記錄停機開始、再按一次記錄恢復；看板顯示累計運轉時間、停機時間，以及**稼動率＝運轉時間 ÷（運轉＋停機）**
- 一顆「換頁鈕」切換兩頁

> 🏭 **稼動率（Availability）** 是 OEE（設備綜合效率）的三大指標之一。稼動率低，代表設備常常停下來，要找出停機原因。

### 🎯 本關配線學習目標

- 元件變多時，比較平行版與 I2C 版的剩餘腳位
- 同一個「計數」需求，有感測器和沒感測器的兩種接法

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：超音波計數、旋鈕設目標 |
| **B** | 產線沒有空間裝超音波，改由作業員每包好一箱按一下「**完成鈕**」計數；目標固定 **120 箱**（不裝旋鈕） |
| **C** | **Pin 2～4** 已被包裝機佔用；達標燈要**線頭、線尾各一顆**（同步） |

### ✏️ ② 接線前決策題

1. 先算一次：本關（依你的限制卡）需要幾隻一般腳？用平行版夠嗎？用 I2C 版還剩幾隻？你選哪一版？為什麼？
2. 限制卡 B：模板的計數來源要填哪個腳位？超音波的兩隻腳填什麼？
3. 限制卡 B：不裝旋鈕，目標要改哪個參數？
4. 第 1 頁和第 2 頁，哪一頁應該是「預設頁」（開機時顯示）？為什麼？
5. 畫出兩頁的看板草圖。

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| LCD | |
| HC-SR04 Trig／Echo（B：完成鈕） | |
| 目標旋鈕 | |
| 達標燈（C：兩顆共用） | |
| 蜂鳴器 | |
| 停機鈕、換頁鈕 | |
| **一般腳合計／剩餘** | |

### 🔌 ③ 接線任務

**Step 1：LCD＋計數來源**

1. 接好 LCD；HC-SR04（或完成鈕配 10kΩ）。
2. ✅ 檢查：LCD 測試器、超音波測距檢查器。

**Step 2：旋鈕＋達標燈＋蜂鳴器**

1. 旋鈕接 A 腳（限制卡 B 不接）；達標燈串 220Ω；接好蜂鳴器。
2. ✅ 檢查：類比讀值檢查器、接線驗證器、蜂鳴器測試器。

**Step 3：停機鈕＋換頁鈕**

1. 兩顆按鈕各配 10kΩ。

### 💻 ④ 套用程式模板 T27

```cpp
// ===== 程式模板 T27：產線績效看板 =====
#include <LiquidCrystal.h>
#include <Adafruit_LiquidCrystal.h>

// ----- 腳位區（沒接的填 -1）-----
bool useI2C = false;                // 用 I2C 版改成 true（自動使用 A4、A5）
int lcdRS = ?, lcdE = ?, lcdD4 = ?, lcdD5 = ?, lcdD6 = ?, lcdD7 = ?;   // 平行版；I2C 版全部填 -1
int pinTrig = ?, pinEcho = ?;       // 計數用 HC-SR04（不用就填 -1）
int btnCount = -1;                  // 計數用按鈕（沒有超音波時使用）
int pinKnob  = A0;                  // 目標產量旋鈕（不用就填 -1）
int ledDone  = ?;                   // 達標燈
int pinBuzzer = ?;                  // 蜂鳴器
int btnDown = ?;                    // 停機鈕
int btnPage = ?;                    // 換頁鈕

// ----- 參數區 -----
float passCM = 10;                  // 小於這個距離算「產品經過」
int fixedTarget = 50;               // 沒接旋鈕時的目標
int knobMin = 10, knobMax = 200;    // 旋鈕可設的目標範圍
String unitName = "pcs";            // 單位（英文）
int doneTone = 1047;

// ----- 功能開關 -----
bool useTarget = false;             // Step 2：目標、達標燈、蜂鳴器
bool usePages  = false;             // Step 3：停機鈕、換頁鈕

// ===== 以下不用改 =====
LiquidCrystal* lcdP = 0;
Adafruit_LiquidCrystal* lcdI = 0;
int qty = 0, page = 0;
bool wasPassing = false, reached = false, running = true;
unsigned long runMs = 0, downMs = 0, lastTick = 0;
int lastCount = LOW, lastDown = LOW, lastPage = LOW;

void lcdStart() {
  if (useI2C) { lcdI = new Adafruit_LiquidCrystal(0); lcdI->begin(16, 2); lcdI->setBacklight(1); }
  else        { lcdP = new LiquidCrystal(lcdRS, lcdE, lcdD4, lcdD5, lcdD6, lcdD7); lcdP->begin(16, 2); }
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  text = text.substring(0, 16);
  if (useI2C) { lcdI->setCursor(0, row); lcdI->print(text); }
  else        { lcdP->setCursor(0, row); lcdP->print(text); }
}

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

bool pressed(int pin, int &last) {
  if (pin < 0) return false;
  int now = digitalRead(pin);
  bool edge = (now == HIGH && last == LOW);
  last = now;
  if (edge) delay(50);
  return edge;
}

String mmss(unsigned long ms) {
  unsigned long s = ms / 1000;
  String sec = (s % 60 < 10) ? "0" + String(s % 60) : String(s % 60);
  return String(s / 60) + ":" + sec;
}

void setup() {
  lcdStart();
  if (pinTrig >= 0) { pinMode(pinTrig, OUTPUT); pinMode(pinEcho, INPUT); }
  if (btnCount >= 0) pinMode(btnCount, INPUT);
  if (ledDone >= 0)  pinMode(ledDone, OUTPUT);
  if (btnDown >= 0)  pinMode(btnDown, INPUT);
  if (btnPage >= 0)  pinMode(btnPage, INPUT);
  lastTick = millis();
  Serial.begin(9600);
}

void loop() {
  // ---- 運轉／停機時間累計 ----
  unsigned long nowMs = millis();
  if (running) runMs += nowMs - lastTick;
  else         downMs += nowMs - lastTick;
  lastTick = nowMs;

  // ---- 計數（只在運轉中計） ----
  bool hit = false;
  if (pinTrig >= 0) {
    bool passing = readCM(pinTrig, pinEcho) < passCM;
    hit = passing && !wasPassing;
    wasPassing = passing;
  } else {
    hit = pressed(btnCount, lastCount);
  }
  if (hit && running) {
    qty++;
    Serial.print("QTY: ");
    Serial.println(qty);
  }

  int target = (pinKnob >= 0) ? map(analogRead(pinKnob), 0, 1023, knobMin, knobMax) : fixedTarget;
  if (useTarget && qty >= target && !reached) {
    reached = true;
    if (pinBuzzer >= 0) tone(pinBuzzer, doneTone, 300);
    Serial.println("TARGET REACHED");
  }
  if (qty < target) reached = false;
  if (useTarget && ledDone >= 0) digitalWrite(ledDone, reached ? HIGH : LOW);

  if (usePages) {
    if (pressed(btnDown, lastDown)) {
      running = !running;
      Serial.println(running ? "Line RUNNING" : "Line DOWN");
    }
    if (pressed(btnPage, lastPage)) page = (page + 1) % 2;
  }

  if (page == 0) {
    if (useTarget) {
      showLine(0, "QTY " + String(qty) + "/" + String(target) + " " + unitName);
      showLine(1, "RATE " + String(qty * 100L / target) + "%" + (reached ? "  DONE!" : ""));
    } else {
      showLine(0, "QTY " + String(qty) + " " + unitName);
      showLine(1, running ? "Counting..." : "LINE DOWN");
    }
  } else {
    unsigned long total = runMs + downMs;
    int avail = (total > 0) ? (int)(runMs * 100 / total) : 100;
    showLine(0, "RUN " + mmss(runMs) + (running ? " [ON]" : " [DN]"));
    showLine(1, "DOWN " + mmss(downMs) + " A" + String(avail) + "%");
  }
  delay(100);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表；限制卡 B 的 `pinTrig`、`pinEcho`、`pinKnob` 填 `-1`，`btnCount` 填完成鈕腳位 |
| 參數區 | 限制卡 B 的 `fixedTarget = 120`、`unitName = "box"` |
| 功能開關 | Step 2 → `useTarget = true`；Step 3 → `usePages = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「達標時，班長辦公室也要亮燈」（限制卡 C 的同學：改成拆掉線尾那顆） | |
| 「目標旋鈕往右轉應該是目標**變少**」（限制卡 B 的同學：改成「完成鈕要搬到產線另一側」） | |
| 「停機鈕按了沒反應，按換頁卻變成停機」——老師會動手腳 | |

### 💼 情境思考

1. 稼動率 85% 代表什麼？如果你是班長，看到稼動率一直下降，下一步要做什麼？（提示：停機時只按「停機鈕」，看板知道「為什麼停機」嗎？）
2. 限制卡 B 用人工按鈕計數。和超音波自動計數比，資料品質有什麼差別？哪些情境人工計數反而比較好？

### ✅ 關卡 3 驗收

- [ ] 每通過一件產品（或按一次完成鈕），QTY 只加 1；停機中不計數
- [ ] 目標與達成率正確，達標時達標燈亮、蜂鳴器提示一次
- [ ] 停機鈕切換運轉／停機，第 2 頁時間與稼動率正確（用計算機驗算）
- [ ] 換頁鈕能在兩頁間切換
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 4：冷鏈倉儲戰情中心（模板 T28）

⭐⭐⭐⭐ **難度**：TMP36 ＋ HC-SR04 ＋ 光敏電阻 ＋ 狀態燈 ＋ 蜂鳴器 ＋ 2 按鈕 ＋ 4 頁看板 → **平行版、I2C 版的腳位大考驗**

### 📖 情境

冷鏈物流倉庫的辦公室要一塊戰情看板，整合四種資訊，用按鈕翻頁：

| 頁面 | 內容 | 資料來源 |
| --- | --- | --- |
| 第 1 頁 | 冷藏區溫度與狀態 | TMP36 |
| 第 2 頁 | 今日出貨件數 | 出貨口 HC-SR04 計數 |
| 第 3 頁 | 倉庫照明狀態（日間／夜間） | 光敏電阻 |
| 第 4 頁 | 異常次數與最近一次異常距今多久 | 系統紀錄 |

- 總狀態燈：正常綠、溫度警戒黃、溫度異常紅
- 溫度異常時，**不論停在哪一頁，看板自動跳到第 1 頁**，蜂鳴器響；值班人員按「確認鈕」停止鳴叫

### 🎯 本關配線學習目標

- 完整算一次平行版、I2C 版的腳位帳
- I2C 版佔用 A4、A5 後，類比感測器只剩 A0～A3

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：自己算腳位、選 LCD 版本 |
| **B** | 客戶倉庫裡有一批**平行版 LCD 庫存**，指定使用平行版 |
| **C** | **I2C 版**；狀態燈改用**紅、綠、黃三顆單色燈**；**Pin 2～6** 已被門禁系統佔用 |

### ✏️ ② 接線前決策題

**先算一次平行版：**

| 元件 | 需要幾隻腳 |
| --- | --- |
| LCD（平行版） | 6 |
| HC-SR04 Trig／Echo | 2 |
| 狀態燈 R／G／B | 3 |
| 蜂鳴器 | 1 |
| 換頁鈕、確認鈕 | 2 |
| **一般腳合計** | |
| TMP36、光敏電阻 | 2（A 腳） |
| **可用總腳位（Pin 2～13＋A0～A5）** | 18 |
| **還剩幾隻？** | |

**再算一次 I2C 版**（LCD 改用 A4、A5）：還剩幾隻？

1. 限制卡 A：你選哪一版？客戶明年想加一顆冷凍區溫度計和一扇自動門（伺服），哪一版放得下？
2. 限制卡 B：Pin 2～13 放得下 14 隻嗎？要借哪幾隻 A 腳？
3. 限制卡 C：扣掉 Pin 2～6 和 A4、A5，剩下的腳夠嗎？三顆單色燈的顏色參數怎麼改？
4. 狀態燈只用全亮全滅顏色，需要 `~` 腳嗎？
5. 畫出四頁的看板草圖。

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| LCD | |
| TMP36／光敏電阻 | |
| HC-SR04 Trig／Echo | |
| 狀態燈 R／G／B（C：紅、綠、黃三顆） | |
| 蜂鳴器 | |
| 換頁鈕／確認鈕 | |
| **剩餘可擴充腳位** | |

### 🔌 ③ 接線任務

**Step 1：LCD＋TMP36＋HC-SR04＋換頁鈕**

1. 接好 LCD、TMP36、HC-SR04（裝在出貨口）、換頁鈕（配 10kΩ）。
2. ✅ 檢查：LCD 測試器、溫度讀值檢查器、超音波測距檢查器。

**Step 2：光敏電阻＋狀態燈＋蜂鳴器**

1. 光敏電阻配 10kΩ；狀態燈各串 220Ω；接好蜂鳴器。
2. ✅ 檢查：類比讀值檢查器（記錄日、夜讀值）、接線驗證器、蜂鳴器測試器。

**Step 3：確認鈕**

1. 確認鈕配 10kΩ。

### 💻 ④ 套用程式模板 T28

```cpp
// ===== 程式模板 T28：多頁戰情看板 =====
#include <LiquidCrystal.h>
#include <Adafruit_LiquidCrystal.h>

// ----- 腳位區（沒接的填 -1）-----
bool useI2C = false;                // 用 I2C 版改成 true（自動使用 A4、A5）
int lcdRS = ?, lcdE = ?, lcdD4 = ?, lcdD5 = ?, lcdD6 = ?, lcdD7 = ?;   // 平行版；I2C 版全部填 -1
int pinTemp = A0;                   // TMP36
int pinLDR  = A1;                   // 光敏電阻
int pinTrig = ?, pinEcho = ?;       // 出貨口 HC-SR04
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈（只用全亮全滅顏色）
int pinBuzzer = ?;                  // 蜂鳴器
int btnPage = ?;                    // 換頁鈕
int btnAck  = ?;                    // 確認鈕

// ----- 參數區 -----
float warnLine  = 6.0;              // 溫度 ≥ 這個值：警戒
float alarmLine = 8.0;              // 溫度 ≥ 這個值：異常
float passCM  = 30;                 // 出貨口：小於這個距離算一件
int darkLine  = 300;                // 光敏電阻低於這個值算夜間
int colorOK[3]    = { 0, 1, 0 };    // 正常（R, G, B 開關：1 亮、0 滅）
int colorWarn[3]  = { 1, 1, 0 };    // 警戒
int colorAlarm[3] = { 1, 0, 0 };    // 異常
String siteName = "COLD HUB";       // 第 1 頁名稱（建議 8 格以內）
int alarmTone = 2000;

// ----- 功能開關 -----
bool useAlarm = false;              // Step 2：照明頁、狀態燈、異常跳頁＋鳴叫
bool useLog   = false;              // Step 3：確認鈕、異常紀錄頁

// ===== 以下不用改 =====
LiquidCrystal* lcdP = 0;
Adafruit_LiquidCrystal* lcdI = 0;
int page = 0, shipped = 0, alarms = 0, tick = 0;
int lastPage = LOW, lastAck = LOW;
bool wasPassing = false, alarmOn = false, acked = false;
unsigned long lastAlarmMs = 0;

void lcdStart() {
  if (useI2C) { lcdI = new Adafruit_LiquidCrystal(0); lcdI->begin(16, 2); lcdI->setBacklight(1); }
  else        { lcdP = new LiquidCrystal(lcdRS, lcdE, lcdD4, lcdD5, lcdD6, lcdD7); lcdP->begin(16, 2); }
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  text = text.substring(0, 16);
  if (useI2C) { lcdI->setCursor(0, row); lcdI->print(text); }
  else        { lcdP->setCursor(0, row); lcdP->print(text); }
}

float readC(int pin) {
  return (analogRead(pin) * 5.0 / 1023.0 - 0.5) * 100.0;
}

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

bool pressed(int pin, int &last) {
  if (pin < 0) return false;
  int now = digitalRead(pin);
  bool edge = (now == HIGH && last == LOW);
  last = now;
  if (edge) delay(50);
  return edge;
}

void setColor(int c[]) {
  if (pinR >= 0) digitalWrite(pinR, c[0] ? HIGH : LOW);
  if (pinG >= 0) digitalWrite(pinG, c[1] ? HIGH : LOW);
  if (pinB >= 0) digitalWrite(pinB, c[2] ? HIGH : LOW);
}

void setup() {
  lcdStart();
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  if (pinR >= 0) pinMode(pinR, OUTPUT);
  if (pinG >= 0) pinMode(pinG, OUTPUT);
  if (pinB >= 0) pinMode(pinB, OUTPUT);
  pinMode(btnPage, INPUT);
  if (btnAck >= 0) pinMode(btnAck, INPUT);
  Serial.begin(9600);
}

void loop() {
  tick++;
  float t = readC(pinTemp);
  int light = analogRead(pinLDR);

  bool passing = readCM(pinTrig, pinEcho) < passCM;
  if (passing && !wasPassing) shipped++;
  wasPassing = passing;

  int level = 0;                                           // 0 正常、1 警戒、2 異常
  if (t >= alarmLine)     level = 2;
  else if (t >= warnLine) level = 1;

  int numPages = useLog ? 4 : (useAlarm ? 3 : 2);
  if (pressed(btnPage, lastPage)) page = (page + 1) % numPages;

  if (useAlarm) {
    if (level == 2 && !alarmOn) {
      alarmOn = true;
      acked = false;
      alarms++;
      lastAlarmMs = millis();
      Serial.println("TEMP ALARM!");
    }
    if (level < 2) alarmOn = false;
    if (useLog && pressed(btnAck, lastAck) && alarmOn) {
      acked = true;
      Serial.println("Alarm acknowledged");
    }
    if (alarmOn && !acked) {
      page = 0;                                            // 異常未確認：強制第 1 頁
      if (tick % 5 == 0 && pinBuzzer >= 0) tone(pinBuzzer, alarmTone, 200);
    }
    if (level == 2)      setColor(colorAlarm);
    else if (level == 1) setColor(colorWarn);
    else                 setColor(colorOK);
  }

  if (page == 0) {
    showLine(0, siteName + " " + String(t, 1) + "C");
    if (level == 2)      showLine(1, (alarmOn && !acked) ? "!! ALARM !!" : "ALARM (ACKED)");
    else if (level == 1) showLine(1, "STATUS: WARNING");
    else                 showLine(1, "STATUS: OK");
  } else if (page == 1) {
    showLine(0, "SHIPPED TODAY");
    showLine(1, String(shipped) + " pcs");
  } else if (page == 2) {
    showLine(0, "LIGHT " + String(light));
    showLine(1, light < darkLine ? "MODE: NIGHT" : "MODE: DAY");
  } else {
    showLine(0, "ALARMS: " + String(alarms));
    if (alarms == 0) showLine(1, "LAST: --");
    else             showLine(1, "LAST: " + String((millis() - lastAlarmMs) / 1000) + " s ago");
  }
  delay(100);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表；I2C 版 `useI2C = true`、6 隻平行腳填 `-1` |
| 參數區 | `darkLine` 依類比讀值記錄；限制卡 C 依決策題 3 改三種顏色 |
| 功能開關 | Step 2 → `useAlarm = true`；Step 3 → `useLog = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「冷藏區門口也要一顆『異常』紅燈，跟狀態燈的紅色同步」 | |
| 「光敏電阻裝到倉庫外面，結果白天顯示 NIGHT、晚上顯示 DAY」——讓判斷反過來 | （提示：第 5 週技巧 2） |
| 「換頁鈕和確認鈕的位置要對調」 | |

### 💼 情境思考

1. 異常時「強制跳回第 1 頁」，但值班人員可能正在看第 2 頁的出貨數。這種「打斷」是好設計嗎？什麼情況下應該打斷、什麼情況下不應該？
2. 第 4 頁的「異常次數」對主管有什麼用？如果一週內異常 15 次，你會建議主管做什麼？（提示：根本原因分析）

### ✅ 關卡 4 驗收

- [ ] 腳位帳正確，平行版／I2C 版的選擇有理由（限制卡 B：借 A 腳正確）
- [ ] 四頁內容正確，換頁鈕能循環切換
- [ ] 狀態燈依溫度顯示綠／黃／紅（限制卡 C：三顆單色燈）
- [ ] 溫度異常時強制跳回第 1 頁並鳴叫；按確認鈕停止鳴叫；第 4 頁紀錄正確
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：每一關都是**新的客戶情境**，從 T25～T28 挑一個模板，**先算腳位、選 LCD 版本、畫看板草圖**，再規劃腳位、接線，並填寫**參數修改表**。**不寫新程式**。
>
> | 關卡 | 情境 | 套用模板 | 元件 | 配線重點 |
> | --- | --- | --- | --- | --- |
> | Lv.1 | 百貨公司哺乳室看板 | T25 | LCD＋按鈕＋2 LED | 安靜、改文字 |
> | Lv.2 | 溫室育苗溫度看板 | T26 | LCD＋TMP36＋按鈕＋LED | 對比度旋鈕、不要聲音 |
> | Lv.3 | 手工皂包裝線看板 | T27 | LCD＋4 按鈕＋LED＋蜂鳴器 | 人工計數、借 A 腳 |
> | Lv.4 | 物流揀貨區績效看板 | T27 | LCD＋HC-SR04＋旋鈕＋2 LED＋蜂鳴器＋2 按鈕 | 平行版 vs I2C 版 |
> | Lv.5 | 生鮮超市冷藏戰情看板 | T28 | 平行版 LCD＋TMP36＋光敏電阻＋HC-SR04＋RGB＋蜂鳴器＋2 按鈕 | **腳位幾乎用完** |
> | Lv.6 | 顧問提案 | 自選 | 自訂 | 自己設計情境 |
>
> 📌 **作答規則**：先畫「看板草圖」，再填「腳位規劃表」「參數修改表」，然後接線；接好後先用工具檢查，最後才上傳程式。Lv.3 之後每關要附**配線設計單**。

---

### 🟢 Lv.1 百貨公司哺乳室看板

**情境**：百貨公司哺乳室門口的看板。媽媽進去時按一下按鈕，看板顯示「使用中」與已使用時間，門口紅燈亮；離開時再按一下，顯示「歡迎使用」、綠燈亮。**哺乳室要安靜，不能有聲音**，也不需要超時提醒。

**元件**：LCD＋1 顆按鈕＋2 顆單色 LED（紅、綠）

**套用模板**：T25

**限制條件**：客戶指定 **I2C 版**。

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `useI2C`、6 隻平行腳 | | | |
| `pinBuzzer` | `?` | | |
| `roomName` | `"ROOM A"` | | 16 格以內的英文 |
| `freeText` | `"AVAILABLE"` | | |
| `busyText` | `"IN USE"` | | |
| 功能開關 | | | 哪一個不要打開？ |

**情境思考**：看板顯示「使用中 12:30」對門外排隊的人有什麼幫助？有沒有可能造成使用者的壓力？你會怎麼取捨？

---

### 🟢 Lv.2 溫室育苗溫度看板

**情境**：溫室育苗區適合 **18～28°C**。看板顯示即時溫度、今天最高最低溫；超出範圍時第 1 行警示、警示燈亮。**溫室裡有很多人工作，不要聲音**。溫室裡光線變化大，看板的字要能**調深淺**。

**元件**：平行版 LCD＋1 顆 TMP36＋1 顆按鈕（歸零）＋1 顆單色 LED＋1 顆旋鈕（調字的深淺）

**套用模板**：T26

**你要想的問題**：

- 調字深淺的旋鈕要接 Arduino 嗎？程式要改嗎？
- 不要聲音，`pinBuzzer` 填什麼？警示燈還會亮嗎？

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| LCD 腳位 | | | |
| `pinTemp`、`btnReset`、`ledAlarm`、`pinBuzzer` | | | |
| `lowLine`、`highLine` | 2.0、8.0 | | |
| `placeName` | `"FRIDGE"` | | |
| 功能開關 | | | |

**情境思考**：育苗溫度超出範圍幾分鐘可能沒事，超出幾小時就會影響發芽率。看板只顯示「現在」和「最高最低」，夠嗎？你會建議多記錄什麼？

---

### 🟡 Lv.3 手工皂包裝線看板

**情境**：小型手工皂工坊，包裝員每包好一盒按一下「完成鈕」，今天目標固定 **80 盒**。看板第 1 頁顯示「完成／目標」與達成率，達標時達標燈亮、蜂鳴器提示；第 2 頁顯示稼動率（停機鈕記錄休息與換料時間）；一顆換頁鈕。

**元件**：平行版 LCD＋3 顆按鈕（完成、停機、換頁）＋1 顆單色 LED＋1 顆蜂鳴器

**套用模板**：T27

**限制條件**：**Pin 12、13** 被工坊的封口機佔用。

**腳位帳**：

| 項目 | 你的答案 |
| --- | --- |
| 平行版 LCD＋其他元件，一般腳共要幾隻？ | |
| 可用的一般腳（扣掉 12、13） | |
| 要借幾隻 A 腳？借給誰？ | |

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `pinTrig`、`pinEcho`、`btnCount` | | | |
| `pinKnob` | `A0` | | |
| `fixedTarget` | 50 | | |
| `unitName` | `"pcs"` | | |
| 其他腳位 | | | |
| 功能開關 | | | |

**情境思考**：包裝員可能忘了按、或多按一次。人工計數的誤差要怎麼發現？（提示：和出貨單、原料用量比對）

---

### 🟡 Lv.4 物流揀貨區績效看板

**情境**：電商倉庫的揀貨區出口，超音波計算揀完的籃子數（**15 cm** 內算一籃），主管用旋鈕設定本班目標（**50～500 籃**）。達標時**揀貨區、主管室各一顆**達標燈亮、蜂鳴器提示。第 2 頁顯示稼動率（停機鈕記錄系統當機時間）。

**元件**：LCD（平行版或 I2C 版，自己決定）＋1 顆 HC-SR04＋1 顆旋鈕＋2 顆單色 LED＋1 顆蜂鳴器＋2 顆按鈕

**套用模板**：T27

**限制條件**：**Pin 2、3** 被條碼掃描器佔用。

**你要想的問題**：

- 分別算平行版、I2C 版的腳位帳，兩種都放得下嗎？各還剩幾隻？
- 客戶下個月想再加一顆「異常燈」和一顆「呼叫主管鈕」，哪一版比較好？

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 |
| --- | --- | --- |
| `useI2C` | `false` | |
| `passCM` | 10 | |
| `knobMin`、`knobMax` | 10、200 | |
| `unitName` | `"pcs"` | |
| 腳位區 | | |
| 功能開關 | | |

**情境思考**：揀貨員知道看板在計數後，可能會為了衝數字把籃子拆小。績效看板會不會造成「指標扭曲」？你會建議怎麼設計指標，避免這種情況？

---

### 🔴 Lv.5 生鮮超市冷藏戰情看板

**情境**：生鮮超市後場的冷藏庫戰情看板，四頁內容和關卡 4 相同：冷藏溫度（**警戒 4°C、異常 6°C**）、今日進貨件數（進貨口超音波，**40 cm** 內算一件）、後場照明日夜、異常紀錄。狀態燈用 RGB。溫度異常時強制跳回第 1 頁並鳴叫，按確認鈕停止。

**元件**：**平行版** LCD＋1 顆 TMP36＋1 顆光敏電阻＋1 顆 HC-SR04＋1 顆 RGB＋1 顆蜂鳴器＋2 顆按鈕

**套用模板**：T28

**限制條件**：超市只有**平行版** LCD；**Pin 13** 已壞掉不能用。

**Step 0：腳位預算（本關重點）**

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 平行版 LCD | 6 | |
| HC-SR04 | 2 | |
| RGB | 3 | |
| 蜂鳴器 | 1 | |
| 換頁鈕、確認鈕 | 2 | |
| TMP36、光敏電阻 | 2（A 腳） | |
| **可用總腳位（扣掉 Pin 13）** | | |
| **還剩幾隻？** | | |

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 |
| --- | --- | --- |
| `warnLine`、`alarmLine` | 6.0、8.0 | |
| `passCM` | 30 | |
| `darkLine` | 300 | 依記錄表 |
| `siteName` | `"COLD HUB"` | |
| 腳位區（含借用的 A 腳） | | |
| 功能開關 | | |

**情境思考**：

1. 附上你的腳位預算表。如果店長要再加一顆「冷藏庫門沒關」的按鈕，還放得下嗎？
2. 第 2 頁「今日進貨件數」和「今日出貨件數」放在同一塊看板，對庫存管理有什麼幫助？

---

### 🏆 Lv.6 BOSS 關：顧問提案

**情境**：自己找一個生活或職場情境，**從 T25～T28 選一個模板**，設計一塊現場看板。

| 情境靈感 | 可能適合的模板 |
| --- | --- |
| 自習室座位使用牌 | T25 |
| 寵物店爬蟲箱溫度看板 | T26 |
| 社團義賣成交數看板 | T27 |
| 社區管理室戰情看板 | T28 |

**最低規格**：

- 至少 1 塊 LCD，並說明**選平行版或 I2C 版的理由**（附兩種版本的腳位帳）
- 至少用到 8 隻腳位
- 至少修改模板參數區的 **5 個參數**（含顯示文字）
- 附**看板草圖**，說明符合哪幾條看板設計原則
- 自己設計 1 題「配線變化挑戰」並附解答

**繳交內容**：情境說明、看板草圖、配線設計單、參數修改表、配線變化挑戰與解答、Tinkercad 連結

---

### 📝 配線設計單

| 項目 | 內容 |
| --- | --- |
| 情境名稱 | |
| 套用模板 | T25／T26／T27／T28 |
| LCD 版本 | 平行版／I2C 版，理由（附兩種版本的腳位帳） |
| 看板草圖 | 每一頁的 2 行 × 16 格 |
| 元件清單 | 元件名稱 × 數量（含 220Ω、10kΩ） |
| 腳位分配表 | 元件 → 腳位（標註 `~` 腳、借用 A 腳、A4／A5 是否被 I2C 佔用） |
| 遇到的問題 | 接線過程中出過什麼錯？怎麼修正的？ |

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| LCD 全黑或全白方塊、沒有字 | 電源、GND；平行版的 V0、RW 是否接 GND | 對照接法表 |
| 出現亂碼 | DB4～DB7 順序；腳位區是否和實際接線一致 | 逐條對照 |
| 平行版完全沒反應 | RS、E 是否接反 | 對照腳位區 |
| I2C 版沒有字 | SDA、SCL 是否接在 A4、A5；`useI2C` 是否改成 `true` | 修正接線或開關 |
| 用 I2C 版後，接在 A4 或 A5 的感測器讀值怪怪的 | A4、A5 已被 LCD 佔用 | 感測器改接 A0～A3 |
| 字只有一半、後面被切掉 | 文字超過 16 格 | 縮短參數區的文字 |
| 溫度顯示 −50°C 左右或亂跳 | TMP36 方向 | 平面朝自己：左 5V、中 A 腳、右 GND |
| 程式無法上傳 | 腳位區是否還有 `?` 沒填 | 用到的填腳位，沒接的填 `-1` |

---

## 成果繳交與評分

### 繳交清單

每個關卡、每個練習題各自建立一個 Tinkercad 檔案，命名格式：`W11-關卡或Lv編號-限制卡-學號`。

| 項目 | 繳交內容 |
| --- | --- |
| 關卡 1～4 | Tinkercad 連結＋決策題答案（含 LCD 版本選擇）＋看板草圖＋腳位規劃表＋配線變化挑戰說明＋同學驗收簽名 |
| Lv.1～Lv.2 | Tinkercad 連結＋參數修改表＋情境思考 |
| Lv.3～Lv.5 | ＋配線設計單 |
| Lv.6 | 完整顧問提案 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 腳位帳與 LCD 版本選擇 | 25% | 符合限制卡；兩種版本的腳位帳正確，選擇有理由 |
| 電路正確性 | 25% | LCD 接線正確，每個 Step 通過工具檢查 |
| 配線變化挑戰 | 20% | 正確運用對比度旋鈕、背光跟燈、兩燈共腳等技巧 |
| 看板草圖與參數修改表 | 15% | 版面符合看板設計原則，文字不超過 16 格 |
| 情境思考 | 15% | 💼 管理面向論述 |

> 🎯 **最低完成標準**：關卡 1～4 全部完成（含配線變化挑戰）、Lv.1～Lv.3 完成。

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 40 分鐘 | 認識 LCD 兩種版本、三個新技巧、看板設計原則、LCD 測試器 |
| 第 1 節後段 | 關卡 1 |
| 第 2 節 | 關卡 2、關卡 3 |
| 第 3 節前段 | 關卡 4 |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，其餘帶回家 |

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| LCD 16×2 | 2 行、每行 16 格的文字顯示器 |
| 平行版 | 佔 6 隻腳（RS、E、DB4～DB7），可調對比、背光 |
| I2C 版 | 只佔 A4（SDA）、A5（SCL）兩隻腳 |
| V0 | 平行版的對比度腳，接旋鈕可調字的深淺 |
| 背光（LED+、LED−） | 螢幕後面的燈 |
| 看板草圖 | 先在 2 × 16 的格子裡規劃要顯示什麼 |
| 目視管理 | 讓異常一眼可見的現場管理方法 |
| 稼動率 | 運轉時間 ÷（運轉＋停機）時間 |

---

## 反思與延伸討論

1. 本週的四塊看板，你覺得哪一塊最能幫助現場人員「不用問、不用算」就知道該做什麼？哪一塊還可以改進？
2. 平行版「便宜但吃腳」、I2C 版「省腳但佔固定介面」。真實的採購決策中，你還會考慮哪些因素？（提示：庫存、維修、未來擴充）
3. 看板讓績效「被看見」。被看見的指標會被改善，沒被看見的指標可能被犧牲（例如只看產量不看品質）。設計看板時，要怎麼避免這種問題？

---

## 本週檢核清單

- [ ] 能正確接上平行版（12 條線）與 I2C 版（4 條線）LCD，並用測試器確認
- [ ] 能分別算出平行版、I2C 版的腳位帳，並依情境選擇
- [ ] 能用 V0 旋鈕調字的深淺、用 LED+ 讓背光跟著燈走（不改程式）
- [ ] 能畫出符合看板設計原則的看板草圖
- [ ] 完成關卡 1～4，並取得同學驗收簽名
- [ ] 完成練習題 Lv.1～Lv.3，挑戰 Lv.4～Lv.6

---

## 下週預告

第 12 週是 Arduino 單元的最後一週：加入 **4×4 矩陣鍵盤**，讓現場人員可以「輸入數字」。鍵盤一顆就佔 7～8 隻腳，加上 LCD，腳位規劃是全學期最緊的一次。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 10 週](week-10.md) | [下一週：第 12 週 — 會輸入的智慧場域 ➡](week-12.md)
