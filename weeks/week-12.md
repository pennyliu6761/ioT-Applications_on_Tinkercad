[⬅ 回目錄](../README.md) | [⬅ 上一週：第 11 週](week-11.md) | [下一週：第 13 週 — 用 3 隻腳點亮整面看板 ➡](week-13.md)

---

# 第 12 週：會輸入的智慧場域＋Arduino 單元總整理

## 主題專案：讓現場人員「輸入數字」，系統依指令行動

> 🎯 **本週定位**：前十一週的系統只能用按鈕「按一下」，沒辦法輸入「員工編號 1024」「商品 13 號」「密碼 2580」。這一週加入 **4×4 矩陣鍵盤**，搭配上週的 LCD，系統第一次可以跟人「對話」：LCD 提示、人輸入、系統回應。
>
> 鍵盤一顆就佔 **7～8 隻腳**，加上 LCD 的 2～6 隻，**這是全學期腳位最緊的一週**——鍵盤接 3 行還是 4 行？LCD 用哪一版？同一個情境，抽到不同限制卡就會配出完全不同的電路。另外，**操作流程怎麼設計，使用者才不會按錯**，是本週最重要的情境思考。
>
> 每一個關卡照「五個固定環節」進行：抽**客戶限制卡** → 回答**接線前決策題** → 照**接線任務**一步步接 → 套用**程式模板**（只改腳位區、參數區） → 完成**配線變化挑戰**（程式一個字都不改，只改線）。
>
> 🏁 **本週成果**：完成 **4 個關卡** ＋ **練習題 Lv.1～Lv.6**（套用本週的程式模板 T29～T32），並完成 **Arduino 單元總整理**。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：4×4 矩陣鍵盤](#認識新元件44-矩陣鍵盤)
- [本週配線新技巧](#本週配線新技巧)
- [操作流程設計原則](#操作流程設計原則)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [關卡怎麼玩：五個固定環節](#關卡怎麼玩五個固定環節)
- [關卡 1：員工打卡機（模板 T29）](#關卡-1員工打卡機模板-t29)
- [關卡 2：校園飲料販賣機（模板 T30）](#關卡-2校園飲料販賣機模板-t30)
- [關卡 3：辦公室密碼門鎖（模板 T31）](#關卡-3辦公室密碼門鎖模板-t31)
- [關卡 4：品管抽檢輸入站（模板 T32）](#關卡-4品管抽檢輸入站模板-t32)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 健身房置物櫃](#-lv1-健身房置物櫃)
  - [Lv.2 員工餐廳餐券兌換](#-lv2-員工餐廳餐券兌換)
  - [Lv.3 社團義賣自助站](#-lv3-社團義賣自助站)
  - [Lv.4 麵包重量抽檢](#-lv4-麵包重量抽檢)
  - [Lv.5 藥局管制藥品櫃](#-lv5-藥局管制藥品櫃)
  - [🏆 Lv.6 BOSS 關：畢業機關提案](#-lv6-boss-關畢業機關提案)
  - [📝 配線設計單](#-配線設計單)
- [🎓 Arduino 單元總整理](#-arduino-單元總整理)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～11 週）

- ✅ LED、RGB、蜂鳴器、伺服、直流馬達、LCD 六種輸出元件
- ✅ 按鈕、旋鈕、光敏電阻、TMP36、HC-SR04 五種輸入元件
- ✅ 配線技巧：兩燈共腳、光敏電阻反相、全亮全滅顏色接一般腳、逐腳判斷 `~` 腳、伺服共用訊號線、感測器整組對調、L293D 接法 A～D、馬達並聯、LCD 版本選擇、對比度旋鈕、背光跟燈

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 4×4 矩陣鍵盤 | 16 顆按鍵，只用 8 條線 | 提款機、門禁、收銀機的數字鍵 |
| 3 行省腳接法 | 不需要 A～D 鍵時，只接 7 條線 | 依需求選設備規格 |
| 操作流程設計 | 提示 → 輸入 → 確認 → 回應 | 標準作業程序（SOP）、防呆 |
| 程式模板 T29～T32 | 本週四個新模板 | 公司的標準表單 |

### 本週可用元件（全學期 Arduino 基礎元件）

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
| LCD 16×2（平行版、I2C 版） | 第 11 週 |
| 4×4 矩陣鍵盤 | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：顧問公司的看板很受歡迎，客戶又提出新需求：「看板只能看，不能操作。我們想讓員工、顧客自己輸入資料。」這週的四個委託，都需要鍵盤＋LCD。
>
> | 關卡 | 客戶 | 委託內容 | 套用模板 | 難度 |
> | --- | --- | --- | --- | --- |
> | 1 | 中小企業 | 員工打卡機 | T29 編號登錄 | ⭐ |
> | 2 | 校園商店 | 飲料販賣機 | T30 選號付款出貨 | ⭐⭐ |
> | 3 | 共享辦公室 | 密碼門鎖 | T31 密碼門鎖 | ⭐⭐⭐ |
> | 4 | 食品工廠 | 品管抽檢輸入站 | T32 數值輸入判定 | ⭐⭐⭐⭐ |

---

## 認識新元件：4×4 矩陣鍵盤

### 外觀與按鍵

```
       第1行 第2行 第3行 第4行
       (C1)  (C2)  (C3)  (C4)
第1列 R1 │ 1 │ 2 │ 3 │ A │
第2列 R2 │ 4 │ 5 │ 6 │ B │
第3列 R3 │ 7 │ 8 │ 9 │ C │
第4列 R4 │ * │ 0 │ # │ D │
```

- 16 顆按鍵，但只有 **8 隻腳**：4 條列線（R1～R4）＋ 4 條行線（C1～C4）。
- 按下一顆鍵，就是把某一條列線和某一條行線接通。Arduino 輪流檢查每一條線，就知道是哪顆鍵被按了——這叫**行列掃描**。
- 在 Tinkercad 元件庫搜尋「Keypad 4x4」，把滑鼠移到腳上會顯示 R1～R4、C1～C4。

### 接法：8 隻腳，不用電阻

| 鍵盤的腳 | 接到哪裡 |
| --- | --- |
| R1、R2、R3、R4 | 任何 4 隻數位腳（也可以借 A 腳） |
| C1、C2、C3、C4 | 任何 4 隻數位腳（也可以借 A 腳） |

- **不需要**任何電阻（和第 2 週的單顆按鈕不同），也不需要 `~` 腳。

### 程式裡的「鍵盤設定」

本週所有模板的**腳位區**都有同一段鍵盤設定，你只要填腳位、選 3 行或 4 行：

```cpp
byte rowPins[4] = { ?, ?, ?, ? };   // R1～R4 接的腳位
byte colPins[4] = { ?, ?, ?, ? };   // C1～C4 接的腳位（只接 3 行時，第 4 格填 0，不會用到）
int numCols = 3;                    // 接 4 行填 4（可以用 A～D 鍵）；只接 3 行填 3
```

LCD 設定和第 11 週相同（`useI2C` ＋ 6 隻平行腳）。

---

## 本週配線新技巧

### 技巧 1：不需要 A～D 鍵 → 只接 3 行，省 1 隻腳

很多情境只需要 **0～9、\*、#** 這 12 顆鍵（輸入密碼、員工編號），用不到右邊那一行的 A、B、C、D。這時候 **C4 不接**，只佔 **7 隻腳**，腳位區的 `numCols` 填 3。

| 接法 | 可用的鍵 | 佔幾隻腳 | 適合情境 |
| --- | --- | --- | --- |
| 完整接法（4 行） | 全部 16 鍵 | 8 | 需要 A～D 當功能鍵（模式切換、管理選單） |
| 省腳接法（3 行） | 0～9、\*、# | 7 | 只需要輸入數字、確認、取消 |

### 技巧 2：鍵盤＋LCD 的腳位總帳

| 組合 | 佔用腳位 | 18 隻中還剩 |
| --- | --- | --- |
| 鍵盤（4 行）＋平行版 LCD | 8 ＋ 6 ＝ 14 | 4 |
| 鍵盤（3 行）＋平行版 LCD | 7 ＋ 6 ＝ 13 | 5 |
| 鍵盤（4 行）＋I2C 版 LCD | 8 ＋ 2 ＝ 10 | **8** |
| 鍵盤（3 行）＋I2C 版 LCD | 7 ＋ 2 ＝ 9 | **9** |

> 💡 Arduino Uno 可用腳位：Pin 2～13（12 隻）＋ A0～A5（6 隻）＝ 18 隻。用平行版時，A4、A5 也可以借來接鍵盤或燈。

### 技巧 3：按鍵對不上 → 從「錯的方式」反推接線

| 按下 | 卻顯示 | 原因 | 修正（不改程式） |
| --- | --- | --- | --- |
| `2` | `4` | 列線和行線**整組**接反 | 把 R 組和 C 組的線整組對調 |
| `1` | `4`、按 `4` 顯示 `1` | R1、R2 兩條線對調 | 對調那兩條線 |
| 某一整排都沒反應 | — | 那一條 R 或 C 沒接好 | 補接 |

---

## 操作流程設計原則

鍵盤讓使用者可以「輸入」，也讓使用者可能「按錯」。好的操作流程讓人**不用看說明書就會用**。

| 原則 | 說明 | 範例 |
| --- | --- | --- |
| ① 每一步都告訴使用者「現在要做什麼」 | LCD 第 0 行放提示 | `ENTER ID:`、`SELECT ITEM` |
| ② 按下去要有回饋 | 每按一鍵 LCD 就顯示 | 輸入的數字立刻出現 |
| ③ 一定要能取消 | 統一用 `*` 清除重來 | `* = CLEAR` |
| ④ 統一用 `#` 確認 | 和提款機、電話一樣 | `# = OK` |
| ⑤ 密碼不能直接顯示 | 用 `*` 遮住 | `PIN: ****` |
| ⑥ 錯誤要說清楚 | 不只說「錯」，還說「怎麼辦」 | `WRONG (2/3)`、`WAIT 30 s` |

> 🏭 這些原則和**標準作業程序（SOP）**、**防呆（Poka-yoke）**的精神一樣：讓不同的人做同一件事，都能得到一樣的結果。

每個關卡都要畫**操作流程圖**：

```
LCD 顯示什麼 ──► 使用者按什麼 ──► LCD 顯示什麼 ──► ……
（正常流程一條線，再畫出「按錯」「取消」的分支）
```

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **鍵盤 8 隻腳：R1～R4、C1～C4，不用電阻** | 滑鼠移上去確認腳位名稱 |
| ② **不需要 A～D 鍵，C4 不接，`numCols` 填 3** | 省 1 隻腳 |
| ③ **先算鍵盤＋LCD 的總帳，再決定其他元件放哪** | 查技巧 2 的表 |
| ④ 鍵盤、LCD 訊號線都不要佔 `~` 腳 | 把 `~` 腳留給調光、調速 |
| ⑤ 用 I2C 版時，A4、A5 不能再借 | 鍵盤可以借 A0～A3 |
| ⑥ 有伺服，9、10 不能調；有蜂鳴器，3、11 不能調 | 兩者都有時只剩 5、6 |
| ⑦ 每條燈線一顆 220Ω；按鈕、光敏電阻一顆 10kΩ | 鍵盤、LCD、伺服、蜂鳴器、HC-SR04 不用 |

---

## 共用工具

### 🔧 工具 1：鍵盤測試器

接好鍵盤後先跑這支，**每一顆鍵都按一遍**，確認 Serial Monitor 印出的字元和你按的位置一致。

```cpp
#include <Keypad.h>

byte rowPins[4] = { 2, 3, 4, 5 };   // ← R1~R4
byte colPins[4] = { 6, 7, 8, 9 };   // ← C1~C4（只接 3 行時，第 4 格填 0）
int numCols = 4;                    // ← 接 4 行填 4、3 行填 3

char keys4[4][4] = { {'1','2','3','A'}, {'4','5','6','B'}, {'7','8','9','C'}, {'*','0','#','D'} };
char keys3[4][3] = { {'1','2','3'}, {'4','5','6'}, {'7','8','9'}, {'*','0','#'} };
Keypad keypad = Keypad(numCols == 4 ? makeKeymap(keys4) : makeKeymap(keys3), rowPins, colPins, 4, numCols);

void setup() {
  Serial.begin(9600);
  Serial.println("Press every key...");
}

void loop() {
  char k = keypad.getKey();
  if (k) {
    Serial.print("Key: ");
    Serial.println(k);
  }
}
```

**檢查標準**：按 `5` 印出 `5`、按 `#` 印出 `#`。對不上時查技巧 3 的表。

### 🔧 其他工具（沿用前幾週）

| 工具 | 用途 | 出處 |
| --- | --- | --- |
| LCD 測試器（平行版、I2C 版） | 檢查 LCD | 第 11 週 |
| 接線驗證器 | 檢查燈的腳位 | 第 3 週起 |
| 蜂鳴器測試器 | 檢查蜂鳴器 | 第 5 週 |
| 伺服測試器 | 找伺服角度 | 第 7 週 |

---

## 關卡怎麼玩：五個固定環節

| 環節 | 你要做什麼 | 產出 |
| --- | --- | --- |
| 🃏 **① 抽客戶限制卡** | 依座號尾數找到你的限制卡 | 圈出你的限制卡 |
| ✏️ **② 接線前決策題** | **先決定鍵盤 3 行或 4 行、LCD 版本、算總帳**，畫出**操作流程圖**，填好**腳位規劃表** | 決策題答案＋流程圖＋規劃表 |
| 🔌 **③ 接線任務** | 照 Step 1 → 2 → 3 接線，每一步都用工具檢查 | 每一步的檢查截圖 |
| 💻 **④ 套用程式模板** | 只改「腳位區」和「參數區」，再把功能開關逐一打開 | 可運作的 Tinkercad 檔案 |
| 🔄 **⑤ 配線變化挑戰** | **程式一個字都不能改**，只靠改線完成客戶的臨時需求 | 修改後的截圖＋說明 |

| 座號尾數 | 抽到 |
| --- | --- |
| 1、4、7 | 限制卡 A |
| 2、5、8 | 限制卡 B |
| 3、6、9、0 | 限制卡 C |

---

## 關卡 1：員工打卡機（模板 T29）

⭐ **難度**：鍵盤 ＋ LCD，再逐步加燈號與上下班模式

### 📖 情境

中小企業的員工打卡機。員工輸入 4 碼員工編號，按 `#` 打卡，LCD 顯示打卡結果與目前在公司的人數；按 `*` 清除重打。同一個人不能重複上班打卡。打卡成功亮綠燈、短嗶；失敗亮紅燈、低音。第三版老闆希望能**分上班、下班**：按 `A` 切換到上班模式、按 `B` 切換到下班模式。

### 🎯 本關配線學習目標

- 第一次接鍵盤：R1～R4、C1～C4
- 依「需不需要 A～D 鍵」決定接 3 行或 4 行

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 只需要**上班打卡**（不分上下班，Step 3 不做）；I2C 版 LCD |
| **B** | 要**分上下班**；公司有一批**平行版 LCD 庫存**，指定使用平行版 |
| **C** | 只需要上班打卡；員工編號 **3 碼**；I2C 版 LCD；**Pin 2～5** 被門禁系統佔用 |

### ✏️ ② 接線前決策題

1. 依你的限制卡，需要 A、B 鍵嗎？鍵盤接 3 行還是 4 行？`numCols` 填多少？
2. 算總帳：鍵盤＋LCD＋2 顆燈＋蜂鳴器，共幾隻腳？18 隻夠嗎？
3. 限制卡 B：鍵盤 4 行＋平行版 LCD，還剩幾隻？A4、A5 可以借嗎？
4. 限制卡 C：扣掉 Pin 2～5 和 A4、A5，還剩哪些腳？鍵盤要借幾隻 A 腳？
5. 畫出操作流程圖：正常打卡、編號位數不對、重複打卡三條路線。

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| 鍵盤 R1～R4 | |
| 鍵盤 C1～C3（C4） | |
| LCD | |
| 綠燈／紅燈 | |
| 蜂鳴器 | |
| **總共用了幾隻／剩幾隻** | |

### 🔌 ③ 接線任務

**Step 1：鍵盤＋LCD**

1. 鍵盤 R1～R4、C1～C3（需要時 C4）接你規劃的腳位。
2. 接好 LCD。
3. ✅ 檢查：**工具 1**（每一顆鍵按一遍）、LCD 測試器。

**Step 2：綠燈、紅燈＋蜂鳴器**

1. 兩顆燈各串 220Ω；接好蜂鳴器。
2. ✅ 檢查：接線驗證器、蜂鳴器測試器。

**Step 3：上下班模式**（限制卡 B 才做）

1. 確認 C4 已接上，用工具 1 測試 A、B 鍵。

### 💻 ④ 套用程式模板 T29

```cpp
// ===== 程式模板 T29：編號登錄（打卡） =====
#include <Keypad.h>
#include <LiquidCrystal.h>
#include <Adafruit_LiquidCrystal.h>

// ----- 腳位區（沒接的填 -1）-----
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1～R4
byte colPins[4] = { ?, ?, ?, 0 };   // 鍵盤 C1～C4（只接 3 行時，第 4 格填 0）
int numCols = 3;                    // 接 4 行填 4；只接 3 行填 3
bool useI2C = true;                 // 用平行版改成 false
int lcdRS = -1, lcdE = -1, lcdD4 = -1, lcdD5 = -1, lcdD6 = -1, lcdD7 = -1;   // 平行版才填
int ledOK  = ?;                     // 綠燈（成功）
int ledErr = ?;                     // 紅燈（失敗）
int pinBuzzer = ?;                  // 蜂鳴器

// ----- 參數區 -----
int idLen = 4;                      // 編號幾碼
String inTitle  = "CLOCK IN";       // 上班模式的提示（英文，16 格以內）
String outTitle = "CLOCK OUT";      // 下班模式的提示
char inKey = 'A', outKey = 'B';     // 切換模式的按鍵（需要 4 行鍵盤）
int showTime = 1500;                // 結果畫面停留幾毫秒

// ----- 功能開關 -----
bool useSignal = false;             // Step 2：燈號＋蜂鳴器
bool useModes  = false;             // Step 3：上下班模式

// ===== 以下不用改 =====
char keys4[4][4] = { {'1','2','3','A'}, {'4','5','6','B'}, {'7','8','9','C'}, {'*','0','#','D'} };
char keys3[4][3] = { {'1','2','3'}, {'4','5','6'}, {'7','8','9'}, {'*','0','#'} };
Keypad keypad = Keypad(numCols == 4 ? makeKeymap(keys4) : makeKeymap(keys3), rowPins, colPins, 4, numCols);
LiquidCrystal* lcdP = 0;
Adafruit_LiquidCrystal* lcdI = 0;
String input = "";
String inside[20];
int count = 0;
bool modeIn = true;

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

void signal(bool ok) {
  if (!useSignal) return;
  if (ledOK >= 0)  digitalWrite(ledOK,  ok ? HIGH : LOW);
  if (ledErr >= 0) digitalWrite(ledErr, ok ? LOW : HIGH);
  if (pinBuzzer >= 0) tone(pinBuzzer, ok ? 1500 : 300, ok ? 100 : 400);
}

void idle() {
  if (ledOK >= 0)  digitalWrite(ledOK, LOW);
  if (ledErr >= 0) digitalWrite(ledErr, LOW);
  showLine(0, modeIn ? inTitle : outTitle);
  showLine(1, "ID: " + input);
}

void result(bool ok, String line0, String line1) {
  showLine(0, line0);
  showLine(1, line1);
  signal(ok);
  Serial.println(line0 + " / " + line1);
  delay(showTime);
  input = "";
  idle();
}

int findId(String id) {
  for (int i = 0; i < count; i++) if (inside[i] == id) return i;
  return -1;
}

void setup() {
  lcdStart();
  if (ledOK >= 0)  pinMode(ledOK, OUTPUT);
  if (ledErr >= 0) pinMode(ledErr, OUTPUT);
  Serial.begin(9600);
  idle();
}

void loop() {
  char k = keypad.getKey();
  if (!k) return;

  if (k >= '0' && k <= '9') {
    if ((int)input.length() < idLen) input += k;
    idle();
  } else if (k == '*') {
    input = "";
    idle();
  } else if (useModes && k == inKey) {
    modeIn = true;  input = ""; idle();
  } else if (useModes && k == outKey) {
    modeIn = false; input = ""; idle();
  } else if (k == '#') {
    int pos = findId(input);
    if ((int)input.length() != idLen) {
      result(false, "NEED " + String(idLen) + " DIGITS", "PRESS * TO CLEAR");
    } else if (modeIn && pos >= 0) {
      result(false, "ALREADY IN: " + input, "IN OFFICE: " + String(count));
    } else if (modeIn && count >= 20) {
      result(false, "LIST FULL", "CALL ADMIN");
    } else if (modeIn) {
      inside[count++] = input;
      result(true, "WELCOME " + input, "IN OFFICE: " + String(count));
    } else if (pos < 0) {
      result(false, "NOT IN: " + input, "IN OFFICE: " + String(count));
    } else {
      inside[pos] = inside[--count];
      result(true, "BYE " + input, "IN OFFICE: " + String(count));
    }
  }
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表；限制卡 B 的 `numCols = 4`、`useI2C = false`，填 6 隻平行腳 |
| 參數區 | 限制卡 C 的 `idLen = 3` |
| 功能開關 | Step 2 → `useSignal = true`；Step 3 → `useModes = true`（限制卡 B） |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「按 `1` 卻出現 `4`」——老師會動手腳，找出來並修好 | （提示：技巧 3） |
| 「警衛室也要看到紅燈（打卡失敗）」 | |
| 「老闆決定不分上下班了，把多的那條線拆掉，省下一隻腳給以後用」（限制卡 A、C 的同學：說明為什麼你們不用拆） | （提示：這題要改一個參數嗎？想想 `numCols`） |

> 💡 第 3 題的陷阱：拆掉 C4 的線、參數不動，程式不會出錯，A～D 只是按了沒反應。但程式**還在掃描那隻腳**，如果你把新元件接上去，兩邊會互相干擾。所以要真正「空出來給以後用」，還要把 `numCols` 改成 3、`colPins` 第 4 格改成 0——這題是本學期少數「只改線不夠」的例子。

### 💼 情境思考

1. 員工可以幫同事代打卡（只要知道編號）。這個漏洞要怎麼補？需要什麼元件？
2. 打卡資料要拿來算薪水。Serial Monitor 的紀錄夠嗎？真實系統還需要記錄什麼？（提示：時間）

### ✅ 關卡 1 驗收

- [ ] 工具 1 每一顆鍵都正確
- [ ] 輸入正確位數的編號按 `#` 打卡成功；位數不對、重複打卡都有清楚的錯誤訊息
- [ ] `*` 能清除重打；燈號與聲音正確
- [ ] 限制卡 B：`A`、`B` 能切換上下班，下班打卡後人數減少
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 2：校園飲料販賣機（模板 T30）

⭐⭐ **難度**：鍵盤 ＋ LCD ＋ 投幣鈕 ＋ 伺服出貨口 ＋ 蜂鳴器

### 📖 情境

校園商店的飲料販賣機，有 4 種商品，編號 11～14。顧客輸入 2 位數商品編號按 `#`，LCD 顯示品名與價格；按「投幣鈕」每次投入 10 元，LCD 顯示已投金額；金額足夠時出貨口（伺服）打開 1.5 秒，並顯示找零。按 `*` 取消並退回已投的錢。

| 編號 | 品名 | 價格 |
| --- | --- | --- |
| 11 | TEA | 25 |
| 12 | COLA | 30 |
| 13 | JUICE | 35 |
| 14 | WATER | 20 |

### 🎯 本關配線學習目標

- 鍵盤＋LCD＋伺服的總帳
- 伺服共用訊號線、借 A 腳的綜合應用

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：I2C 版 LCD |
| **B** | 出貨口是**雙開門**（兩顆伺服同步）；指定**平行版 LCD** |
| **C** | 投幣機只收 **50 元硬幣**；只賣 **3 種**商品（拿掉 WATER）；**Pin 10～13** 被冷藏壓縮機控制板佔用 |

### ✏️ ② 接線前決策題

1. 需要 A～D 鍵嗎？鍵盤接幾行？
2. 投幣鈕需要 10kΩ 嗎？伺服需要電阻嗎？
3. 算總帳：限制卡 B 的平行版 LCD＋鍵盤＋投幣鈕＋伺服＋蜂鳴器，共幾隻？放得下嗎？
4. 限制卡 C：只賣 3 種商品，`numItems` 和商品陣列要怎麼改？`coinValue` 呢？
5. 畫出操作流程圖：正常購買、編號不存在、投錢後取消三條路線。

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| 鍵盤 R1～R4、C1～C3 | |
| LCD | |
| 投幣鈕 | |
| 出貨口伺服（B：兩顆共用） | |
| 蜂鳴器 | |
| **總共用了幾隻／剩幾隻** | |

### 🔌 ③ 接線任務

**Step 1：鍵盤＋LCD**

1. 接好鍵盤（3 行）與 LCD。
2. ✅ 檢查：工具 1、LCD 測試器。

**Step 2：投幣鈕**

1. 投幣鈕配 10kΩ。

**Step 3：出貨口伺服＋蜂鳴器**

1. 伺服紅線 5V、棕線 GND、橘線接你規劃的腳位（限制卡 B：兩顆橘線共用）；接好蜂鳴器。
2. ✅ 檢查：伺服測試器（填角度記錄表）、蜂鳴器測試器。

### 💻 ④ 套用程式模板 T30

```cpp
// ===== 程式模板 T30：選號付款出貨 =====
#include <Keypad.h>
#include <Servo.h>
#include <LiquidCrystal.h>
#include <Adafruit_LiquidCrystal.h>

// ----- 腳位區（沒接的填 -1）-----
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1～R4
byte colPins[4] = { ?, ?, ?, 0 };   // 鍵盤 C1～C4（只接 3 行時，第 4 格填 0）
int numCols = 3;                    // 接 4 行填 4；只接 3 行填 3
bool useI2C = true;                 // 用平行版改成 false
int lcdRS = -1, lcdE = -1, lcdD4 = -1, lcdD5 = -1, lcdD6 = -1, lcdD7 = -1;   // 平行版才填
int btnCoin  = ?;                   // 投幣鈕
int pinServo = ?;                   // 出貨口伺服
int pinBuzzer = ?;                  // 蜂鳴器

// ----- 參數區 -----
int numItems = 4;                   // 商品數量（最多 4）
int itemCode[4]     = { 11, 12, 13, 14 };
String itemName[4]  = { "TEA", "COLA", "JUICE", "WATER" };
int itemPrice[4]    = { 25, 30, 35, 20 };
int codeLen   = 2;                  // 商品編號幾碼
int coinValue = 10;                 // 投幣鈕按一次代表幾元
int closedAngle = 0, openAngle = 90;   // 出貨口角度（依角度記錄表）
int openTime = 1500;                // 出貨口打開幾毫秒
String welcome = "SELECT ITEM";     // 待機提示

// ----- 功能開關 -----
bool usePay      = false;           // Step 2：投幣
bool useDispense = false;           // Step 3：出貨口＋蜂鳴器

// ===== 以下不用改 =====
char keys4[4][4] = { {'1','2','3','A'}, {'4','5','6','B'}, {'7','8','9','C'}, {'*','0','#','D'} };
char keys3[4][3] = { {'1','2','3'}, {'4','5','6'}, {'7','8','9'}, {'*','0','#'} };
Keypad keypad = Keypad(numCols == 4 ? makeKeymap(keys4) : makeKeymap(keys3), rowPins, colPins, 4, numCols);
LiquidCrystal* lcdP = 0;
Adafruit_LiquidCrystal* lcdI = 0;
Servo door;
String input = "";
int chosen = -1, paid = 0, lastCoin = LOW, sold = 0;

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

void screen() {
  if (chosen < 0) {
    showLine(0, welcome);
    showLine(1, "CODE: " + input);
  } else {
    showLine(0, itemName[chosen] + " $" + String(itemPrice[chosen]));
    showLine(1, usePay ? "PAID $" + String(paid) : "* = CANCEL");
  }
}

void message(String a, String b, int ms) {
  showLine(0, a);
  showLine(1, b);
  Serial.println(a + " / " + b);
  delay(ms);
}

void setup() {
  lcdStart();
  if (pinServo >= 0) { door.attach(pinServo); door.write(closedAngle); }
  if (btnCoin >= 0) pinMode(btnCoin, INPUT);
  Serial.begin(9600);
  screen();
}

void loop() {
  char k = keypad.getKey();
  if (k) {
    if (k == '*') {
      if (paid > 0) message("CANCELLED", "REFUND $" + String(paid), 1500);
      input = ""; chosen = -1; paid = 0;
    } else if (chosen < 0 && k >= '0' && k <= '9') {
      if ((int)input.length() < codeLen) input += k;
    } else if (chosen < 0 && k == '#') {
      int code = input.toInt();
      for (int i = 0; i < numItems; i++) if (itemCode[i] == code) chosen = i;
      if (chosen < 0) message("NO SUCH ITEM", "TRY AGAIN", 1200);
      input = "";
    }
    screen();
  }

  if (usePay && chosen >= 0 && btnCoin >= 0) {
    int now = digitalRead(btnCoin);
    if (now == HIGH && lastCoin == LOW) {
      delay(50);
      paid += coinValue;
      screen();
    }
    lastCoin = now;

    if (paid >= itemPrice[chosen]) {
      int change = paid - itemPrice[chosen];
      sold++;
      showLine(0, "ENJOY " + itemName[chosen]);
      showLine(1, "CHANGE $" + String(change));
      Serial.print("Sold #");
      Serial.println(sold);
      if (useDispense) {
        if (pinBuzzer >= 0) tone(pinBuzzer, 1200, 150);
        if (pinServo >= 0) {
          door.write(openAngle);
          delay(openTime);
          door.write(closedAngle);
        }
      }
      delay(1500);
      chosen = -1; paid = 0;
      screen();
    }
  }
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表；限制卡 B 的 `useI2C = false`，填 6 隻平行腳 |
| 參數區 | 角度依角度記錄表；限制卡 C 的 `numItems = 3`、`coinValue = 50` |
| 功能開關 | Step 2 → `usePay = true`；Step 3 → `useDispense = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「出貨口改成雙開門」（限制卡 B 的同學：改回單門） | |
| 「投幣口旁邊也要一顆『投幣成功』燈，投一次閃一下」 | （提示：投幣鈕按下時，那隻腳是 HIGH） |
| 「按 `2` 出現 `4`、按 `4` 出現 `2`……整個亂掉」——老師會動手腳 | （提示：技巧 3） |

> 💡 第 2 題提示：燈可以直接接在按鈕的訊號那一端（串 220Ω）——按鈕按下時，那個點是 5V，燈就亮。這是另一種「不用多佔 Arduino 腳」的接法。

### 💼 情境思考

1. 顧客投了 20 元後走掉了（沒按取消）。下一位顧客會發生什麼事？要怎麼改善？
2. Serial Monitor 記錄了銷售數。如果每台販賣機都能回報銷售，補貨人員的工作可以怎麼改變？

### ✅ 關卡 2 驗收

- [ ] 輸入編號按 `#` 顯示品名、價格；編號不存在有提示
- [ ] 投幣金額累加；夠了自動出貨並顯示找零（限制卡 C：50 元一次）
- [ ] 出貨口平滑打開 1.5 秒後關上（限制卡 B：兩扇同步）
- [ ] `*` 取消時顯示退款金額
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 3：辦公室密碼門鎖（模板 T31）

⭐⭐⭐ **難度**：鍵盤 ＋ LCD ＋ 伺服門鎖 ＋ 燈號 ＋ 蜂鳴器 ＋ 錯誤鎖定

### 📖 情境

共享辦公室的門鎖。輸入 4 碼密碼按 `#`：

- 正確：門鎖（伺服）打開，綠燈亮，5 秒後自動上鎖
- 錯誤：紅燈閃、低音，LCD 顯示 `WRONG (1/3)`；**連續錯 3 次**，鎖定 30 秒不接受輸入，蜂鳴器提醒
- 輸入時 LCD 用 `*` 遮住密碼

### 🎯 本關配線學習目標

- 18 隻腳全部用滿的規劃
- 依「需不需要功能鍵」決定 3 行或 4 行

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：I2C 版 LCD |
| **B** | 要能按 **`D` 鍵立即上鎖**（不用等 5 秒）；指定**平行版 LCD** |
| **C** | **雙開門**（兩顆伺服同步）；**門內、門外各一組紅綠燈**（同步）；**Pin 2～4** 被消防系統佔用 |

### ✏️ ② 接線前決策題

1. 限制卡 B 需要 `D` 鍵，鍵盤接幾行？其他限制卡呢？
2. 算總帳：限制卡 B 的鍵盤＋平行版 LCD＋伺服＋2 顆燈＋蜂鳴器，共幾隻？18 隻剩幾隻？
3. 限制卡 C：4 顆燈、2 顆伺服共用幾隻腳？扣掉 Pin 2～4 和 A4、A5，放得下嗎？
4. 限制卡 A、C 用不到 `D` 鍵，參數 `lockKey` 要填什麼？
5. 畫出操作流程圖：正確、錯 1 次、連錯 3 次三條路線。

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| 鍵盤 R1～R4、C1～C4 | |
| LCD | |
| 門鎖伺服（C：兩顆共用） | |
| 綠燈／紅燈（C：兩組共用） | |
| 蜂鳴器 | |
| **總共用了幾隻／剩幾隻** | |

### 🔌 ③ 接線任務

**Step 1：鍵盤＋LCD＋伺服**

1. 接好鍵盤、LCD、伺服。
2. ✅ 檢查：工具 1、LCD 測試器、伺服測試器（填角度記錄表：「上鎖」「開鎖」各是幾度）。

**Step 2：綠燈、紅燈＋蜂鳴器**

1. 燈各串 220Ω（限制卡 C：兩組共腳）；接好蜂鳴器。
2. ✅ 檢查：接線驗證器、蜂鳴器測試器。

**Step 3：錯誤鎖定**

1. 不用接新元件，打開功能開關測試。

### 💻 ④ 套用程式模板 T31

```cpp
// ===== 程式模板 T31：密碼門鎖 =====
#include <Keypad.h>
#include <Servo.h>
#include <LiquidCrystal.h>
#include <Adafruit_LiquidCrystal.h>

// ----- 腳位區（沒接的填 -1）-----
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1～R4
byte colPins[4] = { ?, ?, ?, 0 };   // 鍵盤 C1～C4（只接 3 行時，第 4 格填 0）
int numCols = 3;                    // 接 4 行填 4；只接 3 行填 3
bool useI2C = true;                 // 用平行版改成 false
int lcdRS = -1, lcdE = -1, lcdD4 = -1, lcdD5 = -1, lcdD6 = -1, lcdD7 = -1;   // 平行版才填
int pinServo  = ?;                  // 門鎖伺服
int ledOpen   = ?;                  // 綠燈（開鎖）
int ledLocked = ?;                  // 紅燈（上鎖）
int pinBuzzer = ?;                  // 蜂鳴器

// ----- 參數區 -----
String code = "2580";               // 密碼
int codeLen = 4;                    // 密碼幾碼
bool selfSet = false;               // true：置物櫃模式（空櫃時由使用者自設密碼，開鎖後清除）
int lockedAngle = 0, openAngle = 90;   // 依角度記錄表
int openSec = 5;                    // 開鎖後幾秒自動上鎖（置物櫃模式不用）
char lockKey = 0;                   // 按這個鍵立即上鎖（例如 'D'；3 行鍵盤填 0）
int maxTries = 3;                   // 連續錯幾次就鎖定
int lockoutSec = 30;                // 鎖定幾秒
String doorName = "OFFICE";         // 顯示名稱

// ----- 功能開關 -----
bool useSignal  = false;            // Step 2：燈號＋蜂鳴器
bool useLockout = false;            // Step 3：錯誤鎖定

// ===== 以下不用改 =====
char keys4[4][4] = { {'1','2','3','A'}, {'4','5','6','B'}, {'7','8','9','C'}, {'*','0','#','D'} };
char keys3[4][3] = { {'1','2','3'}, {'4','5','6'}, {'7','8','9'}, {'*','0','#'} };
Keypad keypad = Keypad(numCols == 4 ? makeKeymap(keys4) : makeKeymap(keys3), rowPins, colPins, 4, numCols);
LiquidCrystal* lcdP = 0;
Adafruit_LiquidCrystal* lcdI = 0;
Servo lock;
String input = "";
int tries = 0;
bool isOpen = false;
bool empty = true;                  // 置物櫃模式：櫃子是否空著
unsigned long openedAt = 0;

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

void lights(bool open) {
  if (!useSignal) return;
  if (ledOpen >= 0)   digitalWrite(ledOpen,   open ? HIGH : LOW);
  if (ledLocked >= 0) digitalWrite(ledLocked, open ? LOW : HIGH);
}

void setLock(bool open) {
  lock.write(open ? openAngle : lockedAngle);
  isOpen = open;
  lights(open);
  Serial.println(doorName + (open ? " UNLOCKED" : " LOCKED"));
}

void screen() {
  String stars = "";
  for (unsigned int i = 0; i < input.length(); i++) stars += "*";
  if (selfSet && empty) showLine(0, "SET NEW PIN:");
  else if (isOpen)      showLine(0, doorName + " OPEN");
  else                  showLine(0, "ENTER PIN:");
  showLine(1, stars);
}

void beep(int f, int ms) {
  if (useSignal && pinBuzzer >= 0) tone(pinBuzzer, f, ms);
}

void setup() {
  lcdStart();
  lock.attach(pinServo);
  if (ledOpen >= 0)   pinMode(ledOpen, OUTPUT);
  if (ledLocked >= 0) pinMode(ledLocked, OUTPUT);
  setLock(selfSet);                                        // 置物櫃模式：空櫃先開著
  Serial.begin(9600);
  screen();
}

void loop() {
  // ---- 一般模式：開鎖後時間到自動上鎖 ----
  if (!selfSet && isOpen && millis() - openedAt > (unsigned long)openSec * 1000) {
    setLock(false);
    screen();
  }

  char k = keypad.getKey();
  if (!k) return;

  if (lockKey != 0 && k == lockKey && isOpen && !selfSet) {
    setLock(false);
  } else if (k >= '0' && k <= '9') {
    if ((int)input.length() < codeLen) input += k;
  } else if (k == '*') {
    input = "";
  } else if (k == '#') {
    if ((int)input.length() != codeLen) {
      showLine(0, "NEED " + String(codeLen) + " DIGITS");
      beep(300, 300);
      delay(1200);
    } else if (selfSet && empty) {                         // 置物櫃：設定新密碼並上鎖
      code = input;
      empty = false;
      setLock(false);
      showLine(0, "PIN SAVED");
      beep(1500, 100);
      delay(1200);
    } else if (input == code) {
      tries = 0;
      setLock(true);
      openedAt = millis();
      showLine(0, "ACCESS OK");
      beep(1500, 100);
      delay(800);
      if (selfSet) empty = true;                           // 置物櫃：取物後清除，櫃子恢復可用
    } else {
      tries++;
      showLine(0, "WRONG (" + String(tries) + "/" + String(maxTries) + ")");
      beep(300, 400);
      if (useSignal && ledLocked >= 0) {
        digitalWrite(ledLocked, LOW);  delay(200);
        digitalWrite(ledLocked, HIGH);
      }
      delay(1000);
      if (useLockout && tries >= maxTries) {
        Serial.println("LOCKOUT");
        for (int s = lockoutSec; s > 0; s--) {
          showLine(0, "LOCKED OUT");
          showLine(1, "WAIT " + String(s) + " s");
          if (s % 5 == 0) beep(800, 200);
          delay(1000);
        }
        tries = 0;
      }
    }
    input = "";
  }
  screen();
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表；限制卡 B 的 `numCols = 4`、`useI2C = false`，填 6 隻平行腳 |
| 參數區 | 角度依角度記錄表；限制卡 B 的 `lockKey = 'D'` |
| 功能開關 | Step 2 → `useSignal = true`；Step 3 → `useLockout = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「門外也要一組紅綠燈」（限制卡 C 的同學：改成拆掉門內那組） | |
| 「改成雙開門」（限制卡 C 的同學：改回單門） | |
| 「門鎖該開的時候鎖、該鎖的時候開」——這題能不能只靠改線解決？ | （提示：第 7 週，伺服方向靠什麼決定？） |

### 💼 情境思考

1. 錯誤鎖定提高了安全性，但真正的員工也可能被鎖在門外。以提款機或手機解鎖為例，討論「安全」和「方便」的取捨。
2. 所有員工都用同一組密碼，離職員工也知道。管理上要怎麼處理？（提示：多組密碼、定期更換、門禁紀錄）

### ✅ 關卡 3 驗收

- [ ] 密碼以 `*` 遮蔽；正確開鎖、5 秒後自動上鎖（限制卡 B：`D` 鍵立即上鎖）
- [ ] 錯誤時顯示次數；連錯 3 次鎖定 30 秒並倒數
- [ ] 燈號與聲音正確（限制卡 C：兩組燈、兩扇門同步）
- [ ] 腳位總帳正確（限制卡 B：18 隻全部用滿）
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 4：品管抽檢輸入站（模板 T32）

⭐⭐⭐⭐ **難度**：鍵盤 ＋ LCD ＋ 狀態燈 ＋ 蜂鳴器 ＋ 統計功能鍵

### 📖 情境

食品工廠包裝線的品管站，品管員把抽檢的產品放上磅秤，**用鍵盤輸入重量**（公克）按 `#`。規格是 **495～505 公克**：合格亮綠燈，不合格亮紅燈並嗶一聲，LCD 顯示是「過重」還是「過輕」。LCD 第 1 行顯示累計的合格數、不合格數與**良率**。

### 🎯 本關配線學習目標

- 功能鍵（A、B）讓系統多一個「管理選單」，要付出 1 隻腳的代價
- 平行版、I2C 版與燈號的綜合腳位規劃

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：重量 **495～505 g**；RGB 狀態燈；I2C 版 LCD |
| **B** | 飲料充填線：容量 **1980～2020 mL**；班長要能按 **`A` 看總表、`B` 清除統計**；I2C 版 LCD |
| **C** | 重量 495～505 g；狀態燈改用**紅、綠兩顆單色燈**；品管主管桌也要一顆**不合格紅燈**（同步）；指定**平行版 LCD** |

### ✏️ ② 接線前決策題

1. 限制卡 B 需要 A、B 鍵，其他限制卡呢？鍵盤接幾行？
2. 狀態燈只顯示綠、紅（全亮全滅），需要 `~` 腳嗎？RGB 的 B 腳會用到嗎？
3. 限制卡 C：紅、綠兩顆單色燈要接在 `pinR`、`pinG`、`pinB` 的哪裡？主管桌的紅燈怎麼接？
4. 算總帳：限制卡 C 的平行版＋鍵盤＋燈＋蜂鳴器，共幾隻？
5. 畫出操作流程圖：合格、過重、過輕、（限制卡 B：看總表、清除統計）。

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| 鍵盤 R1～R4、C1～C4 | |
| LCD | |
| 狀態燈 R／G／B（C：紅、綠兩顆＋主管桌紅燈） | |
| 蜂鳴器 | |
| **總共用了幾隻／剩幾隻** | |

### 🔌 ③ 接線任務

**Step 1：鍵盤＋LCD**

1. 接好鍵盤與 LCD。
2. ✅ 檢查：工具 1、LCD 測試器。

**Step 2：狀態燈＋蜂鳴器**

1. 燈各串 220Ω；接好蜂鳴器。
2. ✅ 檢查：接線驗證器、蜂鳴器測試器。

**Step 3：統計功能**

1. 不用接新元件（限制卡 B：確認 C4 已接，A、B 鍵可用）。

### 💻 ④ 套用程式模板 T32

```cpp
// ===== 程式模板 T32：數值輸入判定 =====
#include <Keypad.h>
#include <LiquidCrystal.h>
#include <Adafruit_LiquidCrystal.h>

// ----- 腳位區（沒接的填 -1）-----
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1～R4
byte colPins[4] = { ?, ?, ?, 0 };   // 鍵盤 C1～C4（只接 3 行時，第 4 格填 0）
int numCols = 3;                    // 接 4 行填 4；只接 3 行填 3
bool useI2C = true;                 // 用平行版改成 false
int lcdRS = -1, lcdE = -1, lcdD4 = -1, lcdD5 = -1, lcdD6 = -1, lcdD7 = -1;   // 平行版才填
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈（只用全亮全滅顏色）
int pinBuzzer = ?;                  // 蜂鳴器

// ----- 參數區 -----
long lowSpec  = 495;                // 規格下限
long highSpec = 505;                // 規格上限
int maxDigits = 4;                  // 最多輸入幾位數
String itemName = "WEIGHT";         // 第 0 行提示（英文）
String unitName = "g";              // 單位
char statsKey = 0;                  // 看總表的按鍵（例如 'A'；3 行鍵盤填 0）
char resetKey = 0;                  // 清除統計的按鍵（例如 'B'；3 行鍵盤填 0）
int colorPass[3] = { 0, 1, 0 };     // 合格（R, G, B 開關：1 亮、0 滅）
int colorFail[3] = { 1, 0, 0 };     // 不合格
int failTone = 300;

// ----- 功能開關 -----
bool useSignal = false;             // Step 2：狀態燈＋蜂鳴器
bool useStats  = false;             // Step 3：統計

// ===== 以下不用改 =====
char keys4[4][4] = { {'1','2','3','A'}, {'4','5','6','B'}, {'7','8','9','C'}, {'*','0','#','D'} };
char keys3[4][3] = { {'1','2','3'}, {'4','5','6'}, {'7','8','9'}, {'*','0','#'} };
Keypad keypad = Keypad(numCols == 4 ? makeKeymap(keys4) : makeKeymap(keys3), rowPins, colPins, 4, numCols);
LiquidCrystal* lcdP = 0;
Adafruit_LiquidCrystal* lcdI = 0;
String input = "";
int passN = 0, failN = 0;
String lastResult = "";

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

void setColor(int c[]) {
  if (pinR >= 0) digitalWrite(pinR, c[0] ? HIGH : LOW);
  if (pinG >= 0) digitalWrite(pinG, c[1] ? HIGH : LOW);
  if (pinB >= 0) digitalWrite(pinB, c[2] ? HIGH : LOW);
}

int yieldPct() {
  int total = passN + failN;
  return (total > 0) ? (passN * 100 / total) : 0;
}

void screen() {
  if (input.length() > 0) showLine(0, itemName + ": " + input + " " + unitName);
  else if (lastResult != "") showLine(0, lastResult);
  else showLine(0, itemName + "? # = OK");
  if (useStats) showLine(1, "P" + String(passN) + " F" + String(failN) + " Y" + String(yieldPct()) + "%");
  else          showLine(1, "SPEC " + String(lowSpec) + "~" + String(highSpec));
}

void setup() {
  lcdStart();
  if (pinR >= 0) pinMode(pinR, OUTPUT);
  if (pinG >= 0) pinMode(pinG, OUTPUT);
  if (pinB >= 0) pinMode(pinB, OUTPUT);
  Serial.begin(9600);
  screen();
}

void loop() {
  char k = keypad.getKey();
  if (!k) return;

  if (k >= '0' && k <= '9') {
    if ((int)input.length() < maxDigits) input += k;
  } else if (k == '*') {
    input = "";
  } else if (k == '#' && input.length() > 0) {
    long v = input.toInt();
    bool ok = (v >= lowSpec && v <= highSpec);
    if (ok) passN++; else failN++;
    if (ok)               lastResult = String(v) + unitName + " PASS";
    else if (v > highSpec) lastResult = String(v) + unitName + " FAIL HI";
    else                   lastResult = String(v) + unitName + " FAIL LO";
    Serial.println(lastResult);
    if (useSignal) {
      setColor(ok ? colorPass : colorFail);
      if (!ok && pinBuzzer >= 0) tone(pinBuzzer, failTone, 400);
    }
    input = "";
  } else if (useStats && statsKey != 0 && k == statsKey) {
    showLine(0, "TOTAL " + String(passN + failN));
    showLine(1, "YIELD " + String(yieldPct()) + "%");
    delay(2000);
  } else if (useStats && resetKey != 0 && k == resetKey) {
    passN = 0; failN = 0; lastResult = "";
    showLine(0, "STATS CLEARED");
    delay(1000);
  }
  screen();
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表；限制卡 B 的 `numCols = 4`；限制卡 C 的 `useI2C = false`、`pinB = -1` |
| 參數區 | 限制卡 B 的規格、`itemName = "VOLUME"`、`unitName = "mL"`、`statsKey = 'A'`、`resetKey = 'B'` |
| 功能開關 | Step 2 → `useSignal = true`；Step 3 → `useStats = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「品管主管桌也要一顆不合格紅燈」（限制卡 C 的同學：改成「產線入口也要一顆合格綠燈」） | |
| 「品管站很吵，蜂鳴器聽不到——再加一顆蜂鳴器放在主管桌，同時響」 | （提示：兩顆蜂鳴器可以共用一隻腳嗎？） |
| 「合格亮紅、不合格亮綠」——老師會動手腳 | |

### 💼 情境思考

1. 品管員可能打錯數字（例如 5010 打成 510）。這種「輸入錯誤」會被判成合格還是不合格？你會建議加什麼防呆？
2. 良率 92% 好不好？要跟什麼比較才知道？（提示：歷史資料、目標、同業）

### ✅ 關卡 4 驗收

- [ ] 輸入數值按 `#` 判定合格、過重、過輕，LCD 清楚顯示
- [ ] 狀態燈與蜂鳴器正確（限制卡 C：兩顆單色燈＋主管桌紅燈同步）
- [ ] 統計與良率正確（用計算機驗算）；限制卡 B：`A` 看總表、`B` 清除
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：每一關都是**新的客戶情境**，從 T29～T32 挑一個模板，**先算鍵盤＋LCD 的總帳、畫操作流程圖**，再規劃腳位、接線，並填寫**參數修改表**。**不寫新程式**。
>
> | 關卡 | 情境 | 套用模板 | 元件 | 配線重點 |
> | --- | --- | --- | --- | --- |
> | Lv.1 | 健身房置物櫃 | T31 | 鍵盤＋LCD＋伺服＋LED | 置物櫃模式 |
> | Lv.2 | 員工餐廳餐券兌換 | T29 | 鍵盤＋LCD＋2 LED＋蜂鳴器 | 避開被佔腳位 |
> | Lv.3 | 社團義賣自助站 | T30 | 鍵盤＋平行版 LCD＋按鈕＋伺服＋蜂鳴器 | 平行版借 A 腳 |
> | Lv.4 | 麵包重量抽檢 | T32 | 鍵盤（4 行）＋LCD＋RGB＋蜂鳴器 | 功能鍵、版本選擇 |
> | Lv.5 | 藥局管制藥品櫃 | T31 | 鍵盤（4 行）＋LCD＋2 伺服＋4 LED＋蜂鳴器 | **平行版放不下** |
> | Lv.6 | 畢業機關提案 | T1～T32 自選 | 自訂 | Arduino 單元畢業作品 |
>
> 📌 **作答規則**：先畫「操作流程圖」，再填「腳位規劃表」「參數修改表」，然後接線；接好後先用工具檢查，最後才上傳程式。Lv.3 之後每關要附**配線設計單**。

---

### 🟢 Lv.1 健身房置物櫃

**情境**：健身房的置物櫃沒有鑰匙。櫃子空著時，會員**自己設定一組 4 碼密碼**按 `#`，門鎖（伺服）鎖上、使用中紅燈亮；取物時輸入同一組密碼按 `#` 開鎖，密碼清除，櫃子恢復可用。**不需要聲音**，也不需要錯誤鎖定。

**元件**：鍵盤（3 行）＋I2C 版 LCD＋1 顆伺服＋1 顆單色 LED（紅）

**套用模板**：T31

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `numCols` | 3 | | |
| `ledOpen`、`ledLocked` | `?` | | 只有一顆紅燈 |
| `pinBuzzer` | `?` | | |
| `selfSet` | `false` | | |
| `lockedAngle`、`openAngle` | 0、90 | | 依角度記錄表 |
| `doorName` | `"OFFICE"` | | |
| 功能開關 | | | |

**情境思考**：會員忘記自己設的密碼怎麼辦？健身房櫃台要怎麼處理，才能兼顧安全與服務？

---

### 🟢 Lv.2 員工餐廳餐券兌換

**情境**：工廠員工餐廳，員工輸入 **5 碼**員工編號按 `#` 領餐，每人每餐只能領一次。成功亮綠燈、短嗶；重複領取亮紅燈、低音。LCD 顯示已領餐人數。

**元件**：鍵盤（3 行）＋I2C 版 LCD＋2 顆單色 LED＋1 顆蜂鳴器

**套用模板**：T29

**限制條件**：**Pin 2～6** 被餐廳 POS 機佔用。

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `rowPins`、`colPins` | | | 要借 A 腳嗎？ |
| `idLen` | 4 | | |
| `inTitle` | `"CLOCK IN"` | | |
| `useModes` | `false` | | |
| 其他腳位 | | | |
| 功能開關 | | | |

**情境思考**：模板最多只能記錄 20 人（陣列大小）。工廠有 300 位員工，這個限制在 Tinkercad 模擬和真實系統中各代表什麼？

---

### 🟡 Lv.3 社團義賣自助站

**情境**：社團義賣攤位的自助販售機，賣 3 種商品：**21 號 COOKIE 30 元、22 號 CAKE 40 元、23 號 TEA 20 元**。投幣鈕每按一次代表投入 10 元，夠了出貨口打開 2 秒，顯示找零。

**元件**：鍵盤（3 行）＋**平行版** LCD＋1 顆按鈕＋1 顆伺服＋1 顆蜂鳴器

**套用模板**：T30

**限制條件**：社團只借到**平行版** LCD。

**腳位帳**：

| 項目 | 你的答案 |
| --- | --- |
| 鍵盤＋平行版 LCD 共幾隻？ | |
| 其他元件共幾隻？ | |
| Pin 2～13 放得下嗎？要借哪幾隻 A 腳？ | |

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 |
| --- | --- | --- |
| `useI2C`、平行腳 | | |
| `numItems` | 4 | |
| `itemCode[]`、`itemName[]`、`itemPrice[]` | | |
| `openTime` | 1500 | |
| `welcome` | `"SELECT ITEM"` | |
| 功能開關 | | |

**情境思考**：義賣結束要對帳。Serial Monitor 只記錄「賣了幾件」，夠嗎？你會建議多記錄什麼？

---

### 🟡 Lv.4 麵包重量抽檢

**情境**：連鎖麵包店中央工廠，品管員抽檢吐司重量，規格 **395～405 g**。合格綠、不合格紅並嗶聲。班長要能按 `A` 看總表、按 `B` 清除統計。**烤箱旁也要一顆合格綠燈**，跟狀態燈同步。

**元件**：鍵盤（4 行）＋LCD（平行版或 I2C 版，自己決定）＋1 顆 RGB＋1 顆單色 LED＋1 顆蜂鳴器

**套用模板**：T32

**限制條件**：**Pin 2、3** 被磅秤通訊佔用。

**你要想的問題**：

- 分別算平行版、I2C 版的腳位帳，兩種都放得下嗎？
- RGB 只顯示綠、紅，B 腳要接嗎？

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 |
| --- | --- | --- |
| `numCols` | 3 | |
| `useI2C` | `true` | |
| `lowSpec`、`highSpec` | 495、505 | |
| `statsKey`、`resetKey` | 0、0 | |
| `itemName` | `"WEIGHT"` | |
| 腳位區 | | |
| 功能開關 | | |

**情境思考**：班長按 `B` 清除統計，但忘了先記下今天的良率。這是誰的問題？系統可以怎麼設計來避免？

---

### 🔴 Lv.5 藥局管制藥品櫃

**情境**：藥局的管制藥品櫃，必須輸入 **6 碼**密碼。櫃門是**雙開門**（兩顆伺服同步），開鎖後 **8 秒**自動上鎖，藥師也可以按 `D` 立即上鎖。**連錯 2 次**就鎖定 **60 秒**。**櫃上、藥師桌各一組紅綠燈**（同步）。

**元件**：鍵盤（4 行）＋LCD＋2 顆伺服＋4 顆單色 LED＋1 顆蜂鳴器

**套用模板**：T31

**限制條件**：**Pin 13** 已壞掉不能用。

**Step 0：腳位總帳（本關重點）**

| 項目 | 平行版 | I2C 版 |
| --- | --- | --- |
| 鍵盤（4 行） | 8 | 8 |
| LCD | 6 | 2（A4、A5） |
| 伺服（兩顆共用） | | |
| 紅綠燈（兩組共用） | | |
| 蜂鳴器 | | |
| **合計** | | |
| **可用腳位（扣掉 Pin 13）** | | |
| **放得下嗎？** | | |

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 |
| --- | --- | --- |
| `numCols`、`useI2C` | | |
| `code`、`codeLen` | `"2580"`、4 | |
| `openSec` | 5 | |
| `lockKey` | 0 | |
| `maxTries`、`lockoutSec` | 3、30 | |
| `doorName` | `"OFFICE"` | |
| 腳位區 | | |
| 功能開關 | | |

**情境思考**：

1. 附上你的腳位總帳。為什麼這一關平行版放不下？
2. 管制藥品的法規通常要求「誰在什麼時間開了櫃子」都要記錄。只靠一組共用密碼，做得到嗎？你會建議怎麼改？

---

### 🏆 Lv.6 BOSS 關：畢業機關提案

**情境**：這是 Arduino 單元的**畢業作品**。你的顧問團隊要從零開始，為一個真實場域設計一套完整的智慧系統。

> **案例 A：自助圖書借還站**
> 讀者用鍵盤輸入借書證號碼，系統確認後開啟還書口（伺服）；超音波偵測書本放入；LCD 顯示借閱紀錄。
>
> **案例 B：社區包裹智慧櫃**
> 住戶輸入取件碼取件；櫃門（伺服）打開；冷藏格用 TMP36 監控溫度。
>
> **案例 C：自訂情境**
> 自己找一個需要「輸入」的生活或職場場域（例如：餐廳點餐機、停車繳費機、醫院報到機、工廠換線設定面板）。

**最低規格**：

- 必須使用矩陣鍵盤＋LCD，並附**鍵盤 3 行／4 行、LCD 版本的選擇理由與腳位總帳**
- 從 **T1～T32** 選一個主模板（只改參數），說明為什麼選它
- 另外至少使用**三類**前面學過的元件（例如：感測器、馬達、聲光）
- 至少用到**兩個**配線技巧（兩燈共腳、反相接法、共用訊號線、馬達並聯、對比度旋鈕、借 A 腳……）
- 至少一個安全或防呆機制，並說明理由
- 自己設計 2 題「配線變化挑戰」並附解答

> 💡 主模板只能涵蓋部分功能時，可以把其他元件做成「不改程式」的配線設計（例如用按鈕直接點燈、旋鈕接 V0），或在提案中說明「下一版要加的功能」。

**繳交內容**：

1. **需求分析**：這個系統解決什麼問題？有哪些使用者角色？（3～5 句）
2. **操作流程圖**：每一種使用者，從開始到結束按了什麼、LCD 顯示什麼
3. **配線設計單**：Pin 2～13、A0～A5 逐一列出
4. **參數修改表**
5. **Tinkercad 實作**：電路截圖＋各元件測試器截圖
6. **配線變化挑戰與解答**
7. **成果短文**（150～200 字）：說明你最引以為傲的一個配線決策

**驗收標準**：

- [ ] 鍵盤＋LCD 操作流程完整，符合「操作流程設計原則」
- [ ] 腳位規劃合理，有說明取捨
- [ ] 防呆或安全機制確實有效
- [ ] 每條燈線有 220Ω，按鈕與光敏電阻有 10kΩ，其他元件沒有多接電阻
- [ ] 麵包板依功能分區

---

### 📝 配線設計單

| 項目 | 內容 |
| --- | --- |
| 情境名稱 | |
| 套用模板 | |
| 鍵盤接法 | 3 行／4 行，理由：需要哪些功能鍵？ |
| LCD 版本 | 平行版／I2C 版，附腳位總帳 |
| 操作流程圖 | 正常流程＋按錯、取消的分支 |
| 元件清單 | 元件名稱 × 數量（含 220Ω、10kΩ） |
| 腳位分配表 | Pin 2～13、A0～A5 逐一列出（標註 `~` 腳、借用 A 腳） |
| 遇到的問題 | 接線過程中出過什麼錯？怎麼修正的？ |

---

## 🎓 Arduino 單元總整理

恭喜你完成了 Arduino 基礎元件的全部內容！在進入第 13～16 週的整合階段前，用下面四張表做一次完整盤點。

### 📋 全元件配線速查表（建議印出來貼在桌上）

| 元件 | 腳數 | 接 Arduino 幾隻 | 要哪種腳 | 需要電阻嗎 | 最容易犯的錯 |
| --- | --- | --- | --- | --- | --- |
| 單色 LED | 2 | 1 | 一般；要調光接 `~` | 220Ω | 長短腳接反 |
| RGB LED（共陰極） | 4 | 3 | 逐腳判斷：有中間值的顏色腳才要 `~` | 每色 220Ω | 有伺服、蜂鳴器時接到被借走的 `~` 腳 |
| 按鈕 | 4 | 1 | 一般 | 10kΩ 下拉 | 訊號線接錯側 |
| 旋鈕 | 3 | 1 | A 腳 | 不用 | 中間腳沒接 A 腳 |
| 蜂鳴器 | 2 | 1 | 一般 | 不用 | 會借走 3、11 的調光 |
| 光敏電阻 | 2 | 1 | A 腳 | 10kΩ | 忘了 10kΩ |
| TMP36 | 3 | 1 | A 腳 | 不用 | 平面方向接反 |
| 伺服馬達 | 3 | 1 | 一般 | 不用 | 會借走 9、10 的調光 |
| HC-SR04 | 4 | 2 | 一般 | 不用 | Trig、Echo 接反 |
| L293D＋直流馬達 | 16＋2 | 1～3（每組馬達） | EN 要調速接 `~` | 不用 | 電源腳 8、16 或 GND 漏接 |
| LCD 平行版 | 16 | 6 | 一般 | 不用 | RW 沒接 GND |
| LCD I2C 版 | 4 | 2 | 固定 A4、A5 | 不用 | SDA、SCL 接反 |
| 4×4 鍵盤 | 8 | 7～8 | 一般 | 不用 | 行列接反 |

### 📋 十二週配線技巧總整理：「改線就能解決」的 14 招

| 技巧 | 出處 | 解決什麼問題 |
| --- | --- | --- |
| 兩燈共腳（每顆各自 220Ω） | 第 5 週 | 兩個地方要同步的燈，只佔 1 隻腳 |
| 光敏電阻與 10kΩ 對調 | 第 5 週 | 「變亮觸發」和「變暗觸發」互換 |
| 旋鈕兩側腳對調 | 第 4 週 | 旋鈕方向反過來 |
| 全亮全滅顏色接一般腳 | 第 5 週 | 省 `~` 腳 |
| 逐腳判斷 RGB 哪一腳要 `~` | 第 6 週 | 只有出現中間值的顏色腳才要 `~` |
| 用參數讓三顆單色燈當成 RGB | 第 8 週 | 客戶指定紅綠黃燈號 |
| 兩顆伺服共用訊號線 | 第 7 週 | 雙開門同步 |
| 借 A 腳 | 第 4 週起 | 一般腳不夠 |
| 兩顆感測器整組對調 | 第 9 週 | 入口、出口裝反 |
| 依需求選 L293D 接法 | 第 10 週 | 每組馬達佔 1～3 隻腳 |
| 對調馬達兩條線 | 第 10 週 | 方向相反 |
| 兩顆馬達並聯同一組輸出 | 第 10 週 | 兩台設備同步運轉 |
| LCD 版本選擇、V0 旋鈕、背光跟燈 | 第 11 週 | 省腳、調字深淺、背光同步 |
| 鍵盤只接 3 行 | 第 12 週 | 省 1 隻腳 |

> 💡 反過來看：**伺服方向、安裝位置換算、門檻與規格**，都只能靠**改參數**解決。分清楚「改線」和「改參數」，就是你這學期最重要的收穫。

### 📋 程式模板索引（T1～T32）

| 週 | 模板 |
| --- | --- |
| 第 4 週 | T1 調光、T2 調速＋狀態燈、T3 長條圖＋門檻、T4 雙旋鈕選擇面板 |
| 第 5 週 | T5 叫號鈴、T6 光線自動控制、T7 光柵警報、T8 光柵計數 |
| 第 6 週 | T9 溫度範圍監測、T10 溫控輸出、T11 三段鎖定、T12 雙區看板 |
| 第 7 週 | T13 按鈕柵欄、T14 感測比例開合、T15 光柵自動門＋人數、T16 雙伺服分揀＋急停 |
| 第 9 週 | T17 近接觸發、T18 距離分段指示、T19 雙感測開蓋＋容量、T20 進出計數柵欄 |
| 第 10 週 | T21 風扇控制、T22 正反轉行程、T23 緩啟動輸送帶、T24 雙馬達差速 |
| 第 11 週 | T25 狀態牌、T26 溫度看板、T27 產線績效看板、T28 多頁戰情看板 |
| 第 12 週 | T29 編號登錄、T30 選號付款出貨、T31 密碼門鎖、T32 數值輸入判定 |

### 📋 技能自評表（1～5 分）

| 週次 | 核心能力 | 自評 |
| --- | --- | --- |
| 第 1～2 週 | LED、按鈕接線；狀態改變偵測 | ___ / 5 |
| 第 3～4 週 | `~` 腳與 PWM；旋鈕與換算 | ___ / 5 |
| 第 5～6 週 | 蜂鳴器、光敏電阻、TMP36；警報處理方式 | ___ / 5 |
| 第 7 週 | 伺服馬達；安全連鎖 | ___ / 5 |
| 第 8 週 | 需求分析、模組化、QA 測試、提案簡報 | ___ / 5 |
| 第 9 週 | 超音波；安裝位置換算 | ___ / 5 |
| 第 10 週 | L293D 四種接法；緩啟動 | ___ / 5 |
| 第 11 週 | LCD 兩種版本；看板設計 | ___ / 5 |
| 第 12 週 | 鍵盤；操作流程設計 | ___ / 5 |
| 全學期 | **依限制卡做腳位規劃、只靠改線應變** | ___ / 5 |

> 自評 3 分以下的週次，建議在第 13 週前回頭重做該週的關卡 1、2（換一張限制卡）。

### 銜接第 13～16 週

| 你學過的 | 接下來會用在 |
| --- | --- |
| 省腳位技巧（第 10～12 週） | 第 13 週：位移暫存器——用 3 隻腳控制 8 顆燈，把省腳位推到極致 |
| L293D「以小控大」（第 10 週） | 第 14 週：繼電器——用 Arduino 控制家電等大功率設備 |
| 光敏電阻、超音波的「偵測有人」（第 5、9 週） | 第 14 週：PIR 人體感測器 |
| 期中專題的模組化、限制卡與優先權（第 8 週） | 第 15～16 週：期末整合大專題 |

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 按鍵完全沒反應 | 鍵盤 8 條線是否都接上；腳位區是否和實際一致 | 用鍵盤測試器逐鍵確認 |
| 按 `2` 卻印出 `4` | 行線和列線整組接反 | 整組對調（技巧 3） |
| 某一整排按鍵都沒反應 | 那一條列線或行線沒接好 | 檢查對應的 R 或 C 腳 |
| 按 A～D 沒反應 | 是否只接了 3 行；`numCols` 是否為 4 | 需要 A～D 時補接 C4 並改 `numCols = 4` |
| 只接 3 行，按鍵卻全部錯位 | `numCols` 是否還是 4 | 改成 3 |
| LCD 沒有字 | I2C 版 SDA→A4、SCL→A5；`useI2C` 是否正確 | 用 LCD 測試器確認 |
| 加了 I2C LCD 後，接在 A4、A5 的元件失效 | A4、A5 已被 LCD 佔用 | 把該元件改接其他腳 |
| 腳位不夠 | 鍵盤能不能只接 3 行？LCD 能不能改 I2C？燈能不能共腳？ | 參考「十二週配線技巧總整理」 |
| 程式無法上傳 | 腳位區是否還有 `?` 沒填 | 用到的填腳位，沒接的填 `-1`（`colPins` 第 4 格填 0） |

---

## 成果繳交與評分

### 繳交清單

每個關卡、每個練習題各自建立一個 Tinkercad 檔案，命名格式：`W12-關卡或Lv編號-限制卡-學號`。

| 項目 | 繳交內容 |
| --- | --- |
| 關卡 1～4 | Tinkercad 連結＋決策題答案（含鍵盤、LCD 選擇與腳位總帳）＋操作流程圖＋腳位規劃表＋配線變化挑戰說明＋同學驗收簽名 |
| Lv.1～Lv.2 | Tinkercad 連結＋參數修改表＋情境思考 |
| Lv.3～Lv.5 | ＋配線設計單 |
| Lv.6 畢業機關 | 完整提案（可分組） |
| 單元總整理 | 技能自評表 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 腳位總帳與版本選擇 | 25% | 符合限制卡；鍵盤 3／4 行、LCD 版本選擇有理由 |
| 電路正確性 | 25% | 每個 Step 通過工具檢查 |
| 配線變化挑戰 | 20% | 正確運用本學期的配線技巧，能判斷哪些題目只能改參數 |
| 操作流程圖與參數修改表 | 15% | 符合操作流程設計原則，參數正確 |
| 情境思考 | 15% | 💼 管理面向論述 |

> 🎯 **最低完成標準**：關卡 1～4 全部完成（含配線變化挑戰）、Lv.1～Lv.3 完成、技能自評表。

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 30 分鐘 | 認識矩陣鍵盤、三個新技巧、操作流程設計原則、鍵盤測試器 |
| 第 1 節後段 | 關卡 1 |
| 第 2 節 | 關卡 2、關卡 3 |
| 第 3 節前段 | 關卡 4 |
| 第 3 節後段 | Arduino 單元總整理（自評表、速查表）；練習題 Lv.1～Lv.2 當堂完成，其餘帶回家 |

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 4×4 矩陣鍵盤 | 16 顆按鍵排成 4 列 4 行，只用 8 條線 |
| 行列掃描 | Arduino 輪流檢查每一條線，找出哪顆鍵被按下 |
| 列線 R1～R4／行線 C1～C4 | 鍵盤的 8 隻腳 |
| 3 行省腳接法 | 不需要 A～D 鍵時不接 C4，只佔 7 隻腳 |
| 腳位總帳 | 鍵盤＋LCD＋其他元件，在 18 隻腳中怎麼分配 |
| 操作流程 | 使用者每一步按什麼、看到什麼的完整順序 |
| 防呆（Poka-yoke） | 讓人不容易犯錯、犯錯也能被發現的設計 |
| 錯誤鎖定 | 連續輸入錯誤達上限，暫時停止接受輸入 |
| 良率（Yield） | 合格品占總檢驗數的比例 |

---

## 反思與延伸討論

1. 本週每個關卡都要先畫「操作流程圖」。這和你在其他管理課程中畫過的**流程圖、SOP、顧客旅程地圖**有什麼共通點？
2. 關卡 1 的配線變化挑戰第 3 題告訴我們：有些變更「只改線」不夠，還要同步改參數。在真實專案中，硬體變更和軟體設定沒有同步，會造成什麼問題？要怎麼管理？
3. 回顧十二週，你從「每個元件都要一隻腳」，到學會依限制卡選接法、選版本、共腳、並聯。這些「在有限資源下達成需求」的思考，和管理中的**資源規劃、產能配置**有什麼相似之處？

---

## 本週檢核清單

- [ ] 能正確接上 4×4 鍵盤，並用鍵盤測試器確認每一顆鍵
- [ ] 能依需求決定鍵盤接 3 行或 4 行，並正確設定 `numCols`
- [ ] 能算出鍵盤＋LCD＋其他元件的腳位總帳，選擇適合的 LCD 版本
- [ ] 能畫出操作流程圖，並應用操作流程設計原則
- [ ] 完成關卡 1～4，並取得同學驗收簽名
- [ ] 完成練習題 Lv.1～Lv.3，挑戰 Lv.4～Lv.6
- [ ] 完成 Arduino 單元技能自評表

---

## 下週預告

恭喜完成 Arduino 基礎元件階段！第 13 週進入整合階段，第一個新元件是**位移暫存器（74HC595）與七段顯示器**，打造「產線數字看板升級站」。你會看到省腳位技巧的極致：**只用 3 隻腳就能控制 8 顆燈**。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 11 週](week-11.md) | [下一週：第 13 週 — 用 3 隻腳點亮整面看板 ➡](week-13.md)
