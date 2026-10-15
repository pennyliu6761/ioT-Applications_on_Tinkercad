[⬅ 回目錄](../README.md) | [⬅ 上一週：第 9 週](week-09.md) | [下一週：第 11 週 — 會說話的智慧場域 ➡](week-11.md)

---

# 第 10 週：會持續運轉的智慧場域

## 主題專案：讓系統「一直轉」，而且能控制快慢與方向

> 🎯 **本週定位**：第 7 週的伺服只能轉到某個角度停住。這一週加入**直流馬達**——風扇、輸送帶、捲門、搬運車的輪子，都是「一直轉」的馬達。馬達力氣大，不能直接接 Arduino，要透過 **L293D 驅動晶片**。
>
> 本週最重要的配線思考：**客戶要不要調速？要不要反轉？** 答案決定 L293D 要佔 Arduino **1～3 隻腳**——需求越簡單，越多腳可以直接固定接 5V 或 GND。同一個情境，抽到不同限制卡就會選出不同接法。
>
> 每一個關卡照「五個固定環節」進行：抽**客戶限制卡** → 回答**接線前決策題** → 照**接線任務**一步步接 → 套用**程式模板**（只改腳位區、參數區） → 完成**配線變化挑戰**（程式一個字都不改，只改線）。
>
> 🏁 **本週成果**：完成 **4 個關卡** ＋ **練習題 Lv.1～Lv.6**（套用本週的程式模板 T21～T24）。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：直流馬達與 L293D](#認識新元件直流馬達與-l293d)
- [本週配線新技巧](#本週配線新技巧)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [關卡怎麼玩：五個固定環節](#關卡怎麼玩五個固定環節)
- [關卡 1：廁所抽風扇（模板 T21）](#關卡-1廁所抽風扇模板-t21)
- [關卡 2：電動鐵捲門（模板 T22）](#關卡-2電動鐵捲門模板-t22)
- [關卡 3：包裝輸送帶（模板 T23）](#關卡-3包裝輸送帶模板-t23)
- [關卡 4：倉庫無人搬運車 AGV（模板 T24）](#關卡-4倉庫無人搬運車-agv模板-t24)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 手搖飲店果汁機](#-lv1-手搖飲店果汁機)
  - [Lv.2 機房散熱風扇](#-lv2-機房散熱風扇)
  - [Lv.3 住家車庫捲門](#-lv3-住家車庫捲門)
  - [Lv.4 麵包店出爐輸送帶](#-lv4-麵包店出爐輸送帶)
  - [Lv.5 倉庫夜間巡檢車](#-lv5-倉庫夜間巡檢車)
  - [🏆 Lv.6 BOSS 關：顧問提案](#-lv6-boss-關顧問提案)
  - [📝 配線設計單](#-配線設計單)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～9 週）

- ✅ LED、RGB、蜂鳴器、伺服四種輸出元件
- ✅ 按鈕、旋鈕、光敏電阻、TMP36、HC-SR04 五種輸入元件
- ✅ 配線技巧：兩燈共腳、光敏電阻反相、全亮全滅顏色接一般腳、逐腳判斷 `~` 腳、伺服共用訊號線、感測器整組對調、借 A 腳
- ✅ 「~ 腳可用表」：伺服借走 9、10；蜂鳴器借走 3、11

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 直流馬達（DC Motor） | 通電就一直轉的馬達，可以調速、可以反轉 | 風扇、輸送帶、捲門、車輪 |
| L293D 驅動晶片 | 幫 Arduino 出力推動馬達的「司機」，一顆可以帶兩組馬達 | 變頻器、馬達控制箱 |
| 接法 A／B／C／D | 依需求選擇 L293D 的接法，決定佔幾隻腳 | 依需求選設備規格，不多買不需要的功能 |
| 緩啟動、緩停止 | 速度慢慢加減，不要瞬間跳 | 電梯、輸送帶避免貨物倒下 |
| 程式模板 T21～T24 | 本週四個新模板 | 公司的標準表單 |

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
| 直流馬達（DC Motor）、L293D | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：客戶們發現系統會「量」也會「動」之後，開始要求更多：「能不能讓東西一直轉？能不能調快慢？能不能倒退？」這週的四個委託，都要用到直流馬達。
>
> | 關卡 | 客戶 | 委託內容 | 套用模板 | 難度 |
> | --- | --- | --- | --- | --- |
> | 1 | 連鎖餐廳 | 廁所抽風扇 | T21 風扇控制 | ⭐ |
> | 2 | 五金行 | 電動鐵捲門 | T22 正反轉行程 | ⭐⭐ |
> | 3 | 食品工廠 | 包裝輸送帶 | T23 緩啟動輸送帶 | ⭐⭐⭐ |
> | 4 | 物流倉庫 | 無人搬運車 AGV | T24 雙馬達差速 | ⭐⭐⭐⭐ |

---

## 認識新元件：直流馬達與 L293D

### 為什麼不能把馬達直接接到 Arduino？

Arduino 的腳位力氣很小（像人的手指），馬達需要很大的力氣（像要推一台車）。直接接上去，輕則推不動，重則把 Arduino 燒壞。所以要請一位「司機」——**L293D**：

```
Arduino（老闆）──下指令──► L293D（司機）──出力──► 馬達（車）
   小訊號                    聽指令、供大電流            轉動
```

### 直流馬達

- **2 隻腳**，接到 L293D 的輸出腳。
- 兩隻腳**接反只會轉向相反**，不會壞——發現方向不對，把兩條線對調就好。
- 在 Tinkercad 元件庫搜尋「DC Motor」。模擬時馬達上會顯示轉速（RPM）。

### L293D：一顆晶片可以帶兩組馬達

L293D 是一顆 16 隻腳的晶片，**左半管馬達 A，右半管馬達 B**。

**擺放方式**：晶片要**跨在麵包板中間的凹槽上**，有半圓缺口的一端朝上。腳位編號從缺口左邊開始，左排由上往下 1～8，右排由下往上 9～16。

```
          ┌───∪───┐
  EN1  1 ─┤       ├─ 16  VCC1（晶片電源 5V）
  IN1  2 ─┤       ├─ 15  IN4
 OUT1  3 ─┤       ├─ 14  OUT4
  GND  4 ─┤ L293D ├─ 13  GND
  GND  5 ─┤       ├─ 12  GND
 OUT2  6 ─┤       ├─ 11  OUT3
  IN2  7 ─┤       ├─ 10  IN3
 VCC2  8 ─┤       ├─  9  EN2
          └───────┘
  ← 馬達 A 這一側     馬達 B 這一側 →
```

> 💡 在 Tinkercad 把滑鼠移到晶片的腳上，會顯示該腳名稱。**每接一條線前都先確認一次**。

| 晶片腳 | 名稱 | 白話說明 | 接到哪裡 |
| --- | --- | --- | --- |
| 16 | VCC1 | 晶片自己的電源 | 5V（**一定要接**） |
| 8 | VCC2 | 給馬達用的電源 | 5V（**一定要接**） |
| 4、5、12、13 | GND | 接地 | GND（**四隻都要接**） |
| 1 | EN1 | 馬達 A 的**油門** | `~` 腳（要調速）或 5V（全速） |
| 2、7 | IN1、IN2 | 馬達 A 的**方向** | 數位腳，或固定接 5V／GND |
| 3、6 | OUT1、OUT2 | 馬達 A 的兩隻腳 | 馬達 A |
| 9、10、15、11、14 | EN2、IN3、IN4、OUT3、OUT4 | 馬達 B 的同樣功能 | 同上 |

> ⚠️ 真實硬體上，馬達電源（Pin 8）通常會接**獨立電源**（例如電池盒），避免馬達啟動時拖垮 Arduino。Tinkercad 模擬可以直接接 Arduino 的 5V。

### 方向怎麼控制？

| IN1 | IN2 | 馬達 |
| --- | --- | --- |
| HIGH | LOW | 正轉 |
| LOW | HIGH | 反轉 |
| LOW | LOW | 停止 |

### 程式裡的「馬達司機函式」

本週所有模板都附上同一個函式，你**不需要修改它**。腳位區裡 L293D 的三隻控制腳，**固定接 5V 或 GND 的就填 `-1`**，函式會自動配合你的接法：

```cpp
void motor(int dir, int speed) {          // dir：1 正轉、-1 反轉、0 停止；speed：0～255
  if (pinIN1 >= 0) digitalWrite(pinIN1, dir == 1  ? HIGH : LOW);
  if (pinIN2 >= 0) digitalWrite(pinIN2, dir == -1 ? HIGH : LOW);
  if (pinEN  >= 0) analogWrite(pinEN, dir == 0 ? 0 : speed);
}
```

---

## 本週配線新技巧

### 技巧 1：依需求選接法，省腳位（本週最重要）

不是每個情境都需要「調速＋正反轉」。**需求越簡單，L293D 可以把越多腳固定接到 5V 或 GND**：

| 接法 | 情境需求 | EN（油門） | IN1 | IN2 | 佔 Arduino 幾隻腳 | 腳位區怎麼填 |
| --- | --- | --- | --- | --- | --- | --- |
| **A** | 只要開／關，全速、單一方向 | 5V | 數位腳 | GND | **1 隻**（一般腳） | `pinEN = -1`、`pinIN2 = -1` |
| **B** | 單一方向，要調速（或緩啟動） | `~` 腳 | 5V | GND | **1 隻**（`~` 腳） | `pinIN1 = -1`、`pinIN2 = -1` |
| **C** | 要正反轉，全速 | 5V | 數位腳 | 數位腳 | **2 隻**（一般腳） | `pinEN = -1` |
| **D** | 要正反轉，也要調速 | `~` 腳 | 數位腳 | 數位腳 | **3 隻**（含 1 隻 `~` 腳） | 三隻都填 |

> 🤔 **每個關卡的決策題，第一題都是：選哪一種接法？**
>
> ⚠️ 「緩啟動、緩停止」也是一種調速——要接在 `~` 腳。

### 技巧 2：方向不對 → 對調馬達兩條線

風扇往室內吹、捲門按「上」卻往下——**不用改程式**，把馬達接在 OUT1、OUT2 的兩條線對調就好。

### 技巧 3：兩顆馬達同步 → 並聯在同一組輸出

客戶要「兩台風扇同時轉」「兩條輸送帶一起跑」時，把第二顆馬達的兩條線**接在第一顆馬達的同一組 OUT 腳**上：

```
L293D OUT1 ──┬── 馬達① 腳 1
             └── 馬達② 腳 1
L293D OUT2 ──┬── 馬達① 腳 2
             └── 馬達② 腳 2
```

- 兩顆馬達會**同速、同方向**轉，Arduino 不多佔任何一隻腳。
- 第二顆轉向相反時，對調它自己的兩條線（技巧 2）。
- 真實硬體要注意 L293D 的電流上限；Tinkercad 模擬可以直接接。

### ~ 腳可用表（沿用，加上馬達）

要調速的 EN 腳必須接 `~` 腳，它和 RGB、調光燈一起搶下面這些 `~` 腳：

| 系統裡有…… | 還能調光、調速的 `~` 腳 |
| --- | --- |
| 都沒有 | 3、5、6、9、10、11 |
| 只有蜂鳴器 | 5、6、9、10 |
| 只有伺服 | 3、5、6、11 |
| 伺服＋蜂鳴器 | 只剩 5、6 |

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **L293D 跨凹槽、缺口朝上；16 和 8 接 5V，4、5、12、13 接 GND** | 電源腳漏接是最常見錯誤 |
| ② **先選接法 A／B／C／D，再規劃腳位** | 固定接 5V／GND 的腳，腳位區填 `-1` |
| ③ 要調速（含緩啟動）→ EN 接 `~` 腳；不調速 → EN 接 5V | |
| ④ 方向不對 → 對調馬達兩條線 | 不用改程式 |
| ⑤ 兩顆馬達同步 → 接在同一組 OUT | 不多佔腳 |
| ⑥ 有伺服，9、10 不能調速；有蜂鳴器，3、11 不能調速 | 兩者都有時只剩 5、6 |
| ⑦ 每條燈線一顆 220Ω；按鈕、光敏電阻一顆 10kΩ | L293D、馬達、HC-SR04、伺服、蜂鳴器、旋鈕、TMP36 不用 |
| ⑧ Pin 0、1 不用；腳位不夠時借 A 腳 | A 腳也能接 IN 腳、Trig、Echo、燈、按鈕 |

---

## 共用工具

### 🔧 工具 1：馬達測試器（任何接法都能用）

接好 L293D 和馬達後先跑這支：正轉 → 停 → 反轉 → 停 → 半速正轉。固定接 5V／GND 的腳填 `-1`。

```cpp
int pinEN  = 5;    // ← EN（接 5V 填 -1）
int pinIN1 = 7;    // ← IN1（固定接 5V 填 -1）
int pinIN2 = 8;    // ← IN2（固定接 GND 填 -1）

void motor(int dir, int speed) {
  if (pinIN1 >= 0) digitalWrite(pinIN1, dir == 1  ? HIGH : LOW);
  if (pinIN2 >= 0) digitalWrite(pinIN2, dir == -1 ? HIGH : LOW);
  if (pinEN  >= 0) analogWrite(pinEN, dir == 0 ? 0 : speed);
}

void test(int dir, int speed, String label) {
  Serial.println(label);
  motor(dir, speed);
  delay(2000);
  motor(0, 0);
  Serial.println("Stop");
  delay(1000);
}

void setup() {
  if (pinEN >= 0)  pinMode(pinEN, OUTPUT);
  if (pinIN1 >= 0) pinMode(pinIN1, OUTPUT);
  if (pinIN2 >= 0) pinMode(pinIN2, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  test(1, 255, "Forward, full speed");
  test(-1, 255, "Reverse, full speed");
  test(1, 128, "Forward, half speed");
}
```

**馬達記錄表**（每次接馬達都要填）：

| 項目 | 接法 A | 接法 B | 接法 C | 接法 D |
| --- | --- | --- | --- | --- |
| 「正轉」時會轉嗎？ | 會 | 會 | 會 | 會 |
| 「反轉」時？ | 停 | 正轉 | 反轉 | 反轉 |
| 「半速」時？ | 全速 | 半速 | 全速 | 半速 |
| **我的接法：＿＿ 實際看到：** | | | | |

> 💡 實際看到的結果和表格對得上，就代表接法正確。正轉方向和情境需要的相反，用技巧 2 對調馬達兩條線。

### 🔧 其他工具（沿用前幾週）

| 工具 | 用途 | 出處 |
| --- | --- | --- |
| 接線驗證器 | 檢查燈的腳位 | 第 3 週起 |
| 類比讀值檢查器 | 檢查旋鈕、光敏電阻 | 第 5 週 |
| 溫度讀值檢查器 | 檢查 TMP36 | 第 6 週 |
| 伺服測試器 | 找伺服角度 | 第 7 週 |
| 超音波測距檢查器 | 檢查 HC-SR04 | 第 9 週 |

---

## 關卡怎麼玩：五個固定環節

| 環節 | 你要做什麼 | 產出 |
| --- | --- | --- |
| 🃏 **① 抽客戶限制卡** | 依座號尾數找到你的限制卡 | 圈出你的限制卡 |
| ✏️ **② 接線前決策題** | **先選 L293D 接法**，再回答配線判斷題，填好**腳位規劃表** | 決策題答案＋規劃表 |
| 🔌 **③ 接線任務** | 照 Step 1 → 2 → 3 接線，每一步都用工具檢查 | 檢查截圖＋馬達記錄表 |
| 💻 **④ 套用程式模板** | 只改「腳位區」和「參數區」，再把功能開關逐一打開 | 可運作的 Tinkercad 檔案 |
| 🔄 **⑤ 配線變化挑戰** | **程式一個字都不能改**，只靠改線完成客戶的臨時需求 | 修改後的截圖＋說明 |

| 座號尾數 | 抽到 |
| --- | --- |
| 1、4、7 | 限制卡 A |
| 2、5、8 | 限制卡 B |
| 3、6、9、0 | 限制卡 C |

---

## 關卡 1：廁所抽風扇（模板 T21）

⭐ **難度**：L293D 第一次接線，依限制卡選最省腳的接法

### 📖 情境

連鎖餐廳的廁所抽風扇。第一版只要一顆開關：按一下開、再按一下關，運轉燈顯示風扇在轉。第二版店長希望能**用旋鈕調整風量**（冬天開小一點）。第三版要**自動化**：廁所燈一亮（有人使用）風扇就轉，燈關掉後**再多轉 5 秒**把異味抽乾淨才停。

### 🎯 本關配線學習目標

- 第一次接 L293D：電源腳、接地腳、跨凹槽
- 依「要不要調速」選接法 A 或 B，固定腳填 `-1`

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：要能調風量 |
| **B** | 不需要調風量（固定全速）；而且 **3、5、6、9、10、11 全部被燈光控制器佔用** |
| **C** | 要能調風量；**兩間廁所的風扇同步**運轉（兩顆馬達）；旋鈕必須接 **A3** |

### ✏️ ② 接線前決策題

1. 抽風扇需要反轉嗎？需要調速嗎？依你的限制卡，選接法 A、B、C、D 的哪一種？佔幾隻腳？
2. 限制卡 B：所有 `~` 腳都被佔用了，還能接風扇嗎？Step 2 的旋鈕還需要接嗎？
3. 限制卡 C：兩顆風扇要用兩組 L293D 輸出嗎？還是有更省的接法？
4. 「廁所燈亮了」要用哪個元件偵測？裝在哪裡？

**腳位規劃表**：

| 元件 | 腳位（固定接 5V／GND 的寫「5V」「GND」） |
| --- | --- |
| L293D EN1 | |
| L293D IN1 | |
| L293D IN2 | |
| 開關鈕 | |
| 運轉燈 | |
| 風量旋鈕 | |
| 光敏電阻（偵測廁所燈） | |

### 🔌 ③ 接線任務

**Step 1：L293D＋馬達＋開關鈕＋運轉燈**

1. L293D 跨在凹槽上、缺口朝上；Pin 16、8 接 5V 軌，Pin 4、5、12、13 接 GND 軌。
2. 依你的接法接 EN1、IN1、IN2；馬達接 OUT1、OUT2（限制卡 C：第二顆馬達也接在 OUT1、OUT2）。
3. 開關鈕配 10kΩ；運轉燈串 220Ω。
4. ✅ 檢查：**工具 1**（填馬達記錄表）、接線驗證器。

**Step 2：風量旋鈕**（限制卡 B 跳過）

1. 旋鈕兩側接 5V、GND，中間接 A 腳。
2. ✅ 檢查：類比讀值檢查器。

**Step 3：光敏電阻**

1. 光敏電阻配 10kΩ，接 A 腳。
2. ✅ 檢查：類比讀值檢查器，記錄「廁所燈開／關」的讀值，決定門檻。

### 💻 ④ 套用程式模板 T21

```cpp
// ===== 程式模板 T21：風扇控制 =====

// ----- 腳位區（沒接的填 -1；L293D 固定接 5V／GND 的腳也填 -1）-----
int pinEN  = ?;                     // L293D EN（接 5V 時填 -1）
int pinIN1 = ?;                     // L293D IN1（固定接 5V 時填 -1）
int pinIN2 = ?;                     // L293D IN2（固定接 GND 時填 -1）
int btnPower = ?;                   // 開關鈕
int ledLevel[3] = { ?, -1, -1 };    // 段數燈（第 1 段亮 1 顆、第 2 段亮 2 顆…）
int pinKnob   = -1;                 // 風量旋鈕
int pinSensor = -1;                 // 自動感測器（光敏電阻或 TMP36）

// ----- 參數區 -----
int numLevels = 1;                  // 按鈕切換幾段：1＝開／關；3＝停→1→2→3→停
int levelSpeed[3] = { 255, 170, 255 };  // 各段速度（接法 A、C 都是全速）
int minSpeed  = 80;                 // 旋鈕轉到最小時的速度
int sensorType = 1;                 // 1：光敏電阻；2：TMP36
float autoLine = 500;               // 超過這個值就自動轉（光敏電阻填讀值；TMP36 填 °C）
int afterRun  = 5000;               // 條件消失後再多轉幾毫秒
String fanName = "Fan";             // 顯示名稱（英文）

// ----- 功能開關 -----
bool useKnob = false;               // Step 2：旋鈕調風量
bool useAuto = false;               // Step 3：自動感測

// ===== 以下不用改 =====
int level = 0, lastBtn = LOW, tick = 0;
bool autoOn = false, seen = false;
unsigned long lastTrig = 0;

void motor(int dir, int speed) {
  if (pinIN1 >= 0) digitalWrite(pinIN1, dir == 1  ? HIGH : LOW);
  if (pinIN2 >= 0) digitalWrite(pinIN2, dir == -1 ? HIGH : LOW);
  if (pinEN  >= 0) analogWrite(pinEN, dir == 0 ? 0 : speed);
}

float readSensor() {
  int raw = analogRead(pinSensor);
  if (sensorType == 2) return (raw * 5.0 / 1023.0 - 0.5) * 100.0;   // TMP36 → °C
  return raw;
}

void setup() {
  if (pinEN >= 0)  pinMode(pinEN, OUTPUT);
  if (pinIN1 >= 0) pinMode(pinIN1, OUTPUT);
  if (pinIN2 >= 0) pinMode(pinIN2, OUTPUT);
  if (btnPower >= 0) pinMode(btnPower, INPUT);
  for (int i = 0; i < 3; i++) if (ledLevel[i] >= 0) pinMode(ledLevel[i], OUTPUT);
  motor(0, 0);
  Serial.begin(9600);
}

void loop() {
  tick++;

  // ---- 手動：按鈕切換段數 ----
  int now = (btnPower >= 0) ? digitalRead(btnPower) : LOW;
  if (now == HIGH && lastBtn == LOW) {
    delay(50);
    level = (level + 1) % (numLevels + 1);
    Serial.print(fanName + " level: ");
    Serial.println(level);
  }
  lastBtn = now;

  // ---- 自動：感測器超過門檻就轉，消失後再轉一段時間 ----
  if (useAuto && pinSensor >= 0) {
    float v = readSensor();
    if (v > autoLine) { lastTrig = millis(); seen = true; }
    bool on = seen && (millis() - lastTrig < (unsigned long)afterRun);
    if (on != autoOn) {
      autoOn = on;
      Serial.println(autoOn ? "AUTO: ON" : "AUTO: OFF");
    }
    if (tick % 40 == 0) {
      Serial.print("Sensor: ");
      Serial.println(v);
    }
  }

  int lv = level;
  if (autoOn && lv == 0) lv = 1;                           // 自動啟動時至少第 1 段
  int speed = (lv > 0) ? levelSpeed[lv - 1] : 0;
  if (useKnob && pinKnob >= 0 && lv > 0) {
    speed = map(analogRead(pinKnob), 0, 1023, minSpeed, 255);
  }
  motor(lv > 0 ? 1 : 0, speed);

  for (int i = 0; i < 3; i++) {
    if (ledLevel[i] >= 0) digitalWrite(ledLevel[i], (i < lv) ? HIGH : LOW);
  }
  delay(50);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依你的接法填 L293D 三隻腳（固定接 5V／GND 的填 `-1`）；限制卡 C 的 `pinKnob = A3` |
| 參數區 | `autoLine` 依 Step 3 記錄的讀值（燈開、燈關的中間）；其他用預設值 |
| 功能開關 | Step 2 → `useKnob = true`（限制卡 B 不用）；Step 3 → `useAuto = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「風扇裝反了，往廁所裡面吹」 | （提示：技巧 2） |
| 「隔壁員工休息室的風扇也要跟著一起轉」（限制卡 C 的同學：改成拆掉一顆） | （提示：技巧 3） |
| 「廁所門外也要一顆運轉燈，讓人知道裡面有在排氣」 | |

### 💼 情境思考

1. 限制卡 B 的同學只用了 1 隻一般腳就讓風扇運轉。如果客戶之後又想調風量，要多花什麼成本（改線、換腳位）？在 Step 0 就問清楚需求有多重要？
2. 「燈關掉後再多轉 5 秒」在管理上叫做什麼？還有哪些設備也有類似設計？（提示：電腦關機後風扇、冷氣的送風模式）

### ✅ 關卡 1 驗收

- [ ] 馬達記錄表的結果和你選的接法一致
- [ ] 按鈕能開關風扇，運轉燈正確
- [ ] 旋鈕能調風量（限制卡 B 除外）
- [ ] 廁所燈亮時風扇自動轉，燈關後約 5 秒才停
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 2：電動鐵捲門（模板 T22）

⭐⭐ **難度**：正反轉（接法 C 或 D）＋ 燈號 ＋ 蜂鳴器 ＋ 超音波防壓

### 📖 情境

五金行的電動鐵捲門。按「上升鈕」捲門往上捲、按「下降鈕」往下放，約 4 秒到底自動停。門在動的時候，門口的燈號顯示上升中／下降中，蜂鳴器間歇提醒路人注意。**下降時門下方若偵測到東西（30 cm 內），立刻停止並改為上升 1 秒**，避免壓到人或車。

### 🎯 本關配線學習目標

- 正反轉需要哪一種接法？
- 依「要不要緩降」決定 EN 接 5V 還是 `~` 腳

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：上升、下降都全速 |
| **B** | 捲門很重，客戶要求**下降時慢速（約一半速度）**，避免重重摔下 |
| **C** | 只裝**一顆按鈕**（按一下上、再按一下下）；店面有**兩扇捲門同步**；門下 HC-SR04 必須接 **A1（Trig）、A2（Echo）** |

### ✏️ ② 接線前決策題

1. 捲門要正反轉，接法 A、B 可以嗎？依你的限制卡選 C 還是 D？
2. 限制卡 B：系統有蜂鳴器，EN 可以接哪些 `~` 腳？
3. 限制卡 C：一顆按鈕要接在模板的 `btnUp` 還是 `btnDown`？另一個填什麼？要改哪個參數？
4. 門下的超音波要裝在哪裡、朝哪裡看？（提示：第 9 週的「站在哪、看向哪」）

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| L293D EN1／IN1／IN2 | |
| 上升鈕／下降鈕 | |
| 上升中燈／下降中燈 | |
| 蜂鳴器 | |
| HC-SR04 Trig／Echo | |

### 🔌 ③ 接線任務

**Step 1：L293D＋馬達＋按鈕**

1. L293D 電源與接地同關卡 1；依接法接 EN1、IN1、IN2；馬達接 OUT1、OUT2（限制卡 C：第二扇門的馬達並聯）。
2. 按鈕各配 10kΩ。
3. ✅ 檢查：**工具 1**，填馬達記錄表；確認「正轉」就是捲門**往上**（不是的話用技巧 2）。

**Step 2：燈號＋蜂鳴器**

1. 上升中燈、下降中燈各串 220Ω；接好蜂鳴器。
2. ✅ 檢查：接線驗證器、蜂鳴器測試器。

**Step 3：門下 HC-SR04**

1. 接好 HC-SR04。
2. ✅ 檢查：超音波測距檢查器。

### 💻 ④ 套用程式模板 T22

```cpp
// ===== 程式模板 T22：正反轉行程控制 =====

// ----- 腳位區（沒接的填 -1；L293D 固定接 5V／GND 的腳也填 -1）-----
int pinEN  = -1;                    // L293D EN（接 5V 時填 -1）
int pinIN1 = ?;                     // L293D IN1
int pinIN2 = ?;                     // L293D IN2
int btnUp   = ?;                    // 上升鈕（單鈕模式：唯一的按鈕）
int btnDown = ?;                    // 下降鈕
int ledUp   = ?;                    // 上升中燈
int ledDown = ?;                    // 下降中燈
int pinBuzzer = ?;                  // 提醒蜂鳴器
int pinTrig = ?, pinEcho = ?;       // 防壓 HC-SR04

// ----- 參數區 -----
int runTime   = 4000;               // 從底到頂（或頂到底）要轉幾毫秒
int upSpeed   = 255;                // 上升速度（接法 C 都是全速）
int downSpeed = 255;                // 下降速度
bool oneButton = false;             // true：只用一顆按鈕，按一下上、再按一下下
float safeCM   = 30;                // 下降時小於這個距離就停止並上升
int reverseTime = 1000;             // 偵測到障礙後上升幾毫秒
int beepGap  = 400;                 // 移動中每隔幾毫秒嗶一次
int beepTone = 1000;
String doorName = "Door";           // 顯示名稱（英文）

// ----- 功能開關 -----
bool useSignal = false;             // Step 2：燈號＋蜂鳴器
bool useSafety = false;             // Step 3：防壓感測

// ===== 以下不用改 =====
int pos = 0;                        // 0：在底部；1：在頂部；2：停在中間
int lastUp = LOW, lastDown = LOW;

void motor(int dir, int speed) {
  if (pinIN1 >= 0) digitalWrite(pinIN1, dir == 1  ? HIGH : LOW);
  if (pinIN2 >= 0) digitalWrite(pinIN2, dir == -1 ? HIGH : LOW);
  if (pinEN  >= 0) analogWrite(pinEN, dir == 0 ? 0 : speed);
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

void signal(int dir) {
  if (!useSignal) return;
  if (ledUp >= 0)   digitalWrite(ledUp,   dir == 1  ? HIGH : LOW);
  if (ledDown >= 0) digitalWrite(ledDown, dir == -1 ? HIGH : LOW);
}

void travel(int dir) {
  Serial.println(doorName + (dir == 1 ? " going UP" : " going DOWN"));
  signal(dir);
  motor(dir, dir == 1 ? upSpeed : downSpeed);
  unsigned long t0 = millis(), lastBeep = 0;
  while (millis() - t0 < (unsigned long)runTime) {
    if (useSignal && pinBuzzer >= 0 && millis() - lastBeep > (unsigned long)beepGap) {
      tone(pinBuzzer, beepTone, 80);
      lastBeep = millis();
    }
    if (dir == -1 && useSafety && pinTrig >= 0 && readCM(pinTrig, pinEcho) < safeCM) {
      motor(0, 0);
      Serial.println("OBSTACLE! Reversing");
      if (useSignal && pinBuzzer >= 0) tone(pinBuzzer, 2000, 300);
      signal(1);
      motor(1, upSpeed);
      delay(reverseTime);
      motor(0, 0);
      signal(0);
      pos = 2;
      Serial.println(doorName + " stopped half way");
      return;
    }
    delay(50);
  }
  motor(0, 0);
  signal(0);
  pos = (dir == 1) ? 1 : 0;
  Serial.println(doorName + (pos == 1 ? " is UP" : " is DOWN"));
}

void setup() {
  if (pinEN >= 0)  pinMode(pinEN, OUTPUT);
  if (pinIN1 >= 0) pinMode(pinIN1, OUTPUT);
  if (pinIN2 >= 0) pinMode(pinIN2, OUTPUT);
  if (btnUp >= 0)   pinMode(btnUp, INPUT);
  if (btnDown >= 0) pinMode(btnDown, INPUT);
  if (ledUp >= 0)   pinMode(ledUp, OUTPUT);
  if (ledDown >= 0) pinMode(ledDown, OUTPUT);
  if (pinTrig >= 0) { pinMode(pinTrig, OUTPUT); pinMode(pinEcho, INPUT); }
  motor(0, 0);
  Serial.begin(9600);
  Serial.println(doorName + " READY (at bottom)");
}

void loop() {
  bool up   = pressed(btnUp, lastUp);
  bool down = pressed(btnDown, lastDown);

  if (oneButton) {
    if (up) travel(pos == 1 ? -1 : 1);                     // 在頂部就下降，否則上升
  } else {
    if (up)   { if (pos == 1) Serial.println("Already UP");   else travel(1); }
    if (down) { if (pos == 0) Serial.println("Already DOWN"); else travel(-1); }
  }
  delay(20);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依接法填；限制卡 B 的 `pinEN` 填 `~` 腳；限制卡 C 的 `btnDown = -1`、`pinTrig = A1`、`pinEcho = A2` |
| 參數區 | 限制卡 B 的 `downSpeed` 改成約一半；限制卡 C 的 `oneButton = true` |
| 功能開關 | Step 2 → `useSignal = true`；Step 3 → `useSafety = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「按上升，門卻往下」——老師會動手腳，找出來並修好 | （提示：技巧 2） |
| 「騎樓對面也要一顆『下降中』警示燈」 | |
| 「捲門完全不動，但 Serial Monitor 有顯示 going UP」——老師會動手腳 | （提示：L293D 的電源腳） |

### 💼 情境思考

1. 本關的捲門靠「轉 4 秒」判斷到頂、到底。如果停電後門停在中間，系統以為門在底部，會發生什麼事？真實捲門用什麼方法知道「到頂了」？
2. 防壓功能讓門上升 1 秒就停，為什麼不是直接升到頂？兩種設計各有什麼考量？

### ✅ 關卡 2 驗收

- [ ] 馬達記錄表已填，「正轉」就是捲門往上
- [ ] 上升、下降各約 4 秒自動停；已在頂部按上升無效
- [ ] 移動中燈號正確、蜂鳴器間歇提醒（限制卡 B：下降明顯較慢）
- [ ] 下降時門下有東西，立刻停止並上升 1 秒
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 3：包裝輸送帶（模板 T23）

⭐⭐⭐ **難度**：緩啟動（接法 B）＋ 超音波 ＋ 狀態燈 ＋ 蜂鳴器 → **`~` 腳要精打細算**

### 📖 情境

食品工廠的包裝輸送帶，只往一個方向跑：

- 旋鈕調整速度；啟動和減速都要**慢慢變化**，避免產品倒下
- 輸送帶末端裝超音波：產品接近末端（30 cm 內）自動**減速**；到達末端（10 cm 內）**停止並鎖住**，等作業員把產品拿走、按「繼續鈕」才再啟動
- 狀態燈：運轉綠、減速黃 (255,180,0)、停止紅；停止時蜂鳴器提醒
- Serial Monitor 記錄送達幾件產品

### 🎯 本關配線學習目標

- 「不用反轉、但要緩啟動」→ 接法 B，EN 一定要 `~` 腳
- EN 和 RGB 一起搶 `~` 腳時，怎麼分配

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款 |
| **B** | **兩條並排輸送帶同步**運轉；**~9、~10** 已被包裝機佔用 |
| **C** | 速度固定（**不裝旋鈕**，速度 200）；狀態燈改用**紅、綠、黃三顆單色燈** |

### ✏️ ② 接線前決策題

1. 輸送帶要反轉嗎？要調速嗎？選哪一種接法？
2. 限制卡 C 不用旋鈕調速了，EN 還需要接 `~` 腳嗎？為什麼？（提示：緩啟動）
3. 逐腳判斷：綠、黃 (255,180,0)、紅三種顏色，RGB 哪一腳需要 `~`？
4. 填 `~` 腳預算：系統有蜂鳴器，還有哪些 `~` 腳？EN 和 RGB 各用哪隻？限制卡 B 再扣掉 9、10 呢？
5. 限制卡 C：三顆單色燈接法和顏色參數要怎麼改？還需要 `~` 腳嗎？

**`~` 腳預算表**：

| 項目 | 你的答案 |
| --- | --- |
| 本系統可用的 `~` 腳 | |
| 需要 `~` 的元件腳 | EN、（RGB 的哪一腳？） |
| 分配結果 | |

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| L293D EN1／IN1／IN2 | |
| 速度旋鈕 | |
| HC-SR04 Trig／Echo | |
| 繼續鈕 | |
| 狀態燈 R／G／B（C：紅、綠、黃三顆） | |
| 蜂鳴器 | |

### 🔌 ③ 接線任務

**Step 1：L293D（接法 B）＋馬達＋旋鈕**

1. EN1 接 `~` 腳、IN1 接 5V、IN2 接 GND；馬達接 OUT1、OUT2（限制卡 B：第二條輸送帶馬達並聯）。
2. 旋鈕接 A 腳（限制卡 C 不接）。
3. ✅ 檢查：工具 1（馬達記錄表）、類比讀值檢查器。

**Step 2：HC-SR04＋繼續鈕**

1. HC-SR04 裝在輸送帶**末端**，朝向來貨方向；繼續鈕配 10kΩ。
2. ✅ 檢查：超音波測距檢查器。

**Step 3：狀態燈＋蜂鳴器**

1. 狀態燈各串 220Ω；接好蜂鳴器。
2. ✅ 檢查：接線驗證器、蜂鳴器測試器。

### 💻 ④ 套用程式模板 T23

```cpp
// ===== 程式模板 T23：緩啟動輸送帶 =====

// ----- 腳位區（沒接的填 -1；L293D 固定接 5V／GND 的腳也填 -1）-----
int pinEN  = ?;                     // L293D EN（緩啟動要用 ~ 腳）
int pinIN1 = -1;                    // L293D IN1（固定接 5V 時填 -1）
int pinIN2 = -1;                    // L293D IN2（固定接 GND 時填 -1）
int pinKnob = ?;                    // 速度旋鈕
int pinTrig = ?, pinEcho = ?;       // 末端 HC-SR04
int btnResume = ?;                  // 繼續鈕
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈
int pinBuzzer = ?;                  // 蜂鳴器

// ----- 參數區 -----
int baseSpeed = 200;                // 沒接旋鈕時的速度
int minSpeed  = 80;                 // 旋鈕轉到最小時的速度
float slowCM = 30;                  // 小於這個距離開始減速
float stopCM = 10;                  // 小於這個距離停止並鎖住
int slowPercent = 33;               // 減速時降到原速度的幾 %
int rampStep = 5;                   // 每次加減速的幅度（越小越平順）
int colorRun[3]  = { 0, 255, 0 };   // 運轉（綠）
int colorSlow[3] = { 255, 180, 0 }; // 減速（黃）
int colorStop[3] = { 255, 0, 0 };   // 停止（紅）
int stopTone = 800;
String lineName = "Conveyor";       // 顯示名稱（英文）

// ----- 功能開關 -----
bool useSensor = false;             // Step 2：末端感測＋繼續鈕
bool useStatus = false;             // Step 3：狀態燈＋蜂鳴器

// ===== 以下不用改 =====
int current = 0, lastResume = LOW, delivered = 0;
bool halted = false;

void motor(int dir, int speed) {
  if (pinIN1 >= 0) digitalWrite(pinIN1, dir == 1  ? HIGH : LOW);
  if (pinIN2 >= 0) digitalWrite(pinIN2, dir == -1 ? HIGH : LOW);
  if (pinEN  >= 0) analogWrite(pinEN, dir == 0 ? 0 : speed);
}

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setColor(int c[]) {
  if (pinR >= 0) analogWrite(pinR, c[0]);
  if (pinG >= 0) analogWrite(pinG, c[1]);
  if (pinB >= 0) analogWrite(pinB, c[2]);
}

void setup() {
  if (pinEN >= 0)  pinMode(pinEN, OUTPUT);
  if (pinIN1 >= 0) pinMode(pinIN1, OUTPUT);
  if (pinIN2 >= 0) pinMode(pinIN2, OUTPUT);
  if (pinTrig >= 0) { pinMode(pinTrig, OUTPUT); pinMode(pinEcho, INPUT); }
  if (btnResume >= 0) pinMode(btnResume, INPUT);
  if (pinR >= 0) pinMode(pinR, OUTPUT);
  if (pinG >= 0) pinMode(pinG, OUTPUT);
  if (pinB >= 0) pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int base = baseSpeed;
  if (pinKnob >= 0) base = map(analogRead(pinKnob), 0, 1023, minSpeed, 255);
  int target = base;
  bool slowing = false;

  if (useSensor) {
    float d = readCM(pinTrig, pinEcho);
    if (d < stopCM && !halted) {
      halted = true;
      delivered++;
      Serial.print(lineName + " STOPPED - product #");
      Serial.print(delivered);
      Serial.println(" at the end");
      if (useStatus && pinBuzzer >= 0) tone(pinBuzzer, stopTone, 400);
    } else if (d < slowCM) {
      target = base * slowPercent / 100;
      slowing = true;
    }

    int now = (btnResume >= 0) ? digitalRead(btnResume) : LOW;
    if (now == HIGH && lastResume == LOW) {
      delay(50);
      if (halted && d >= stopCM) { halted = false; Serial.println("Resumed"); }
      else if (halted)           Serial.println("Remove the product first");
    }
    lastResume = now;
    if (halted) target = 0;
  }

  // ---- 緩啟動／緩停止：每次只往目標靠近一點 ----
  if (current < target) current = min(current + rampStep, target);
  if (current > target) current = max(current - rampStep, target);
  motor(current > 0 ? 1 : 0, current);

  if (useStatus) {
    if (halted)       setColor(colorStop);
    else if (slowing) setColor(colorSlow);
    else              setColor(colorRun);
  }
  delay(40);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依 `~` 腳預算表填入；限制卡 C 的 `pinKnob = -1` |
| 參數區 | 限制卡 C 依決策題 5 改三種顏色（`baseSpeed` 維持 200） |
| 功能開關 | Step 2 → `useSensor = true`；Step 3 → `useStatus = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「產線重新配置，輸送帶要**反方向**跑」 | （提示：有兩種改法，你能都找到嗎？） |
| 「旁邊再加一條輸送帶，一起跑」（限制卡 B 的同學：改成拆掉一條） | |
| 「速度旋鈕往右轉要變**慢**」（限制卡 C 的同學：改成「停止時，作業區也要一顆紅燈」） | |

### 💼 情境思考

1. 把 `rampStep` 改成 255（等於沒有緩啟動），觀察馬達轉速變化。對玻璃瓶裝產品會有什麼影響？
2. 「到末端就停、要人按繼續」會不會讓產線變慢？從產能和安全的角度，討論這個設計值不值得。

### ✅ 關卡 3 驗收

- [ ] `~` 腳預算表正確：EN 和需要 `~` 的顏色腳都避開 3、11（限制卡 B：也避開 9、10）
- [ ] 速度是慢慢變化而不是瞬間跳
- [ ] 產品接近末端減速、到末端停止；產品沒拿走前按繼續無效
- [ ] 狀態燈綠／黃／紅正確（限制卡 C：三顆單色燈），停止時提醒
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 4：倉庫無人搬運車 AGV（模板 T24）

⭐⭐⭐⭐ **難度**：**一顆 L293D 帶兩顆馬達** ＋ 循線 ＋ 避障 ＋ 伺服貨叉 → **伺服＋蜂鳴器，`~` 腳只剩 5、6**

### 📖 情境

物流倉庫的地上貼了一條黑色引導線，無人搬運車（AGV）要沿著線走：

- 車底左右各一顆光敏電阻看地面：**左邊看到黑線 → 往左修正；右邊看到黑線 → 往右修正**；都沒看到 → 直走
- 兩邊**同時**看到黑線（站點的橫線）→ 停下來，貨叉（伺服）升起、放下完成裝卸，再繼續走
- 前方有超音波：20 cm 內有人或障礙物就停車並鳴叫，障礙消失後繼續
- 一顆啟動／停止鈕

> 🚗 **轉彎原理**：AGV 沒有方向盤，靠「左右輪速度不同」轉彎——要往左轉，就讓左輪慢、右輪快。

### 🎯 本關配線學習目標

- 一顆 L293D 兩側都用接法 B，兩個 EN 都要 `~` 腳
- Step 0 就把最後才接的伺服考慮進去

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款 |
| **B** | 倉庫的引導線是**反光膠帶**（比地面**亮**，不是暗）；**Pin 2～4** 已被無線通訊模組佔用 |
| **C** | **不裝貨叉**，改成到站時亮「到站燈」3 秒；車子是**四輪驅動**（左右各兩顆馬達） |

### ✏️ ② 接線前決策題

1. 兩顆輪子要調速嗎？要倒退嗎？選哪一種接法？兩顆馬達共佔幾隻腳？
2. 系統有伺服＋蜂鳴器，`~` 腳只剩哪兩隻？如果 Step 1 時把 EN 接在 9，Step 3 會發生什麼事？
3. 限制卡 B：引導線比地面亮，程式是用「讀值低於門檻＝看到線」判斷的。**不改程式**要怎麼辦？（提示：第 5 週技巧 2）
4. 限制卡 C：沒有伺服了，`~` 腳多了哪幾隻？四顆馬達要怎麼接？

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| 左輪 EN1（IN1、IN2 接哪？） | |
| 右輪 EN2（IN3、IN4 接哪？） | |
| 左、右循線光敏電阻 | |
| HC-SR04 Trig／Echo | |
| 蜂鳴器 | |
| 啟動／停止鈕 | |
| 貨叉伺服（C：到站燈） | |

### 🔌 ③ 接線任務

**Step 1：L293D（兩側接法 B）＋2 顆馬達＋光敏電阻＋按鈕**

1. L293D 電源與接地同前；EN1、EN2 接 `~` 腳，IN1、IN3 接 5V，IN2、IN4 接 GND。
2. 左輪接 OUT1、OUT2，右輪接 OUT3、OUT4（限制卡 C：同側的第二顆馬達並聯）。
3. 兩顆光敏電阻各配 10kΩ，左邊的接 A0、右邊的接 A1（限制卡 B 用反相接法）。
4. ✅ 檢查：工具 1（左右輪各測一次）、類比讀值檢查器（記錄「看到線／沒看到線」的讀值）。

**Step 2：HC-SR04＋蜂鳴器**

1. HC-SR04 裝在車頭朝前；接好蜂鳴器。
2. ✅ 檢查：超音波測距檢查器、蜂鳴器測試器。

**Step 3：貨叉伺服（限制卡 C：到站燈）**

1. 接好伺服（或到站燈串 220Ω）。
2. ✅ 檢查：伺服測試器（填角度記錄表）或接線驗證器。

### 💻 ④ 套用程式模板 T24

```cpp
// ===== 程式模板 T24：雙馬達差速（循線車） =====
#include <Servo.h>

// ----- 腳位區（沒接的填 -1）-----
int enLeft  = ?;                    // 左輪 EN（~ 腳；IN 腳固定接 5V／GND）
int enRight = ?;                    // 右輪 EN（~ 腳）
int ldrLeft = A0, ldrRight = A1;    // 左、右循線光敏電阻
int pinTrig = ?, pinEcho = ?;       // 前方 HC-SR04
int pinBuzzer = ?;                  // 蜂鳴器
int btnStart  = ?;                  // 啟動／停止鈕
int pinFork   = ?;                  // 貨叉伺服（沒有就填 -1）
int ledStation = -1;                // 到站燈（沒有就填 -1）

// ----- 參數區 -----
int lineDark = 300;                 // 讀值低於這個數字算「看到線」（依記錄表）
int cruise   = 180;                 // 直走速度
int slowSide = 60;                  // 修正方向時，內側輪的速度
float stopCM = 20;                  // 前方障礙停車距離
int forkDown = 0, forkUp = 70;      // 貨叉角度（依角度記錄表）
int stationTime = 3000;             // 到站停留時間（毫秒）
String carName = "AGV";             // 顯示名稱（英文）

// ----- 功能開關 -----
bool useObstacle = false;           // Step 2：避障
bool useStation  = false;           // Step 3：到站動作

// ===== 以下不用改 =====
Servo fork;
bool running = false;
int lastStart = LOW, tick = 0;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void drive(int left, int right) {
  analogWrite(enLeft, left);
  analogWrite(enRight, right);
}

void setup() {
  if (pinFork >= 0) { fork.attach(pinFork); fork.write(forkDown); }
  pinMode(enLeft, OUTPUT);
  pinMode(enRight, OUTPUT);
  if (pinTrig >= 0) { pinMode(pinTrig, OUTPUT); pinMode(pinEcho, INPUT); }
  pinMode(btnStart, INPUT);
  if (ledStation >= 0) pinMode(ledStation, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  tick++;
  int now = digitalRead(btnStart);
  if (now == HIGH && lastStart == LOW) {
    delay(50);
    running = !running;
    Serial.println(carName + (running ? " START" : " STOP"));
  }
  lastStart = now;

  if (!running) { drive(0, 0); delay(50); return; }

  // ---- 避障：最優先 ----
  if (useObstacle && readCM(pinTrig, pinEcho) < stopCM) {
    drive(0, 0);
    if (tick % 10 == 0) {
      if (pinBuzzer >= 0) tone(pinBuzzer, 1500, 150);
      Serial.println("OBSTACLE - waiting");
    }
    delay(50);
    return;
  }

  bool L = analogRead(ldrLeft)  < lineDark;
  bool R = analogRead(ldrRight) < lineDark;

  if (L && R) {                                            // 站點橫線
    drive(0, 0);
    Serial.println("STATION reached");
    if (useStation) {
      if (pinBuzzer >= 0) tone(pinBuzzer, 1000, 100);
      if (pinFork >= 0) {
        fork.write(forkUp);   delay(1500);
        fork.write(forkDown); delay(1000);
      }
      if (ledStation >= 0) {
        digitalWrite(ledStation, HIGH);
        delay(stationTime);
        digitalWrite(ledStation, LOW);
      }
      Serial.println("Station work done");
    }
    drive(cruise, cruise);                                 // 往前開，離開橫線
    delay(800);
  } else if (L) {
    drive(slowSide, cruise);                               // 往左修正
  } else if (R) {
    drive(cruise, slowSide);                               // 往右修正
  } else {
    drive(cruise, cruise);                                 // 直走
  }
  delay(30);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表填入；限制卡 C 的 `pinFork = -1`、`ledStation` 填到站燈腳位 |
| 參數區 | `lineDark` 依循線記錄表（限制卡 B：用反相接法後重新記錄）；貨叉角度依角度記錄表 |
| 功能開關 | Step 2 → `useObstacle = true`；Step 3 → `useStation = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「左邊看到線時，車子反而往右修」——老師會動手腳，找出來並修好 | （提示：可能是光敏電阻，也可能是馬達） |
| 「倉庫 B 區的引導線改用反光膠帶」（限制卡 B 的同學：改回黑色膠帶） | |
| 「右輪往後轉」——老師會動手腳 | |

### 💼 情境思考

1. 如果客戶要求 AGV 能**倒車**，兩顆馬達要改成哪一種接法？需要再多幾隻腳？你的腳位還放得下嗎？
2. AGV 遇到障礙物會停下來等。如果有員工把貨物堆在走道上，AGV 會一直卡著。管理上應該怎麼處理？（提示：等太久要通報誰？）

### ✅ 關卡 4 驗收

- [ ] 一顆 L293D 帶兩顆馬達，兩個 EN 接在伺服、蜂鳴器都沒借走的 `~` 腳
- [ ] 左側看到線時左輪變慢、右側看到時右輪變慢（限制卡 B：反光膠帶也正確）
- [ ] 前方 20 cm 內有障礙物時停車鳴叫，障礙消失後繼續
- [ ] 兩側同時看到線時停車、完成到站動作後繼續
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：每一關都是**新的客戶情境**，從 T21～T24 挑一個模板，**先選 L293D 接法**，再算元件、規劃腳位、接線，並填寫**參數修改表**。**不寫新程式**。
>
> | 關卡 | 情境 | 套用模板 | 元件 | 配線重點 |
> | --- | --- | --- | --- | --- |
> | Lv.1 | 手搖飲店果汁機 | T21 | L293D＋馬達＋按鈕＋3 LED | 三段速、`~` 腳只剩一隻 |
> | Lv.2 | 機房散熱風扇 | T21 | L293D＋2 馬達＋TMP36＋旋鈕＋按鈕＋LED | 溫度自動、兩顆並聯 |
> | Lv.3 | 住家車庫捲門 | T22 | L293D＋馬達＋按鈕＋4 LED＋蜂鳴器＋HC-SR04 | 單鈕、兩燈共腳、避開被佔腳位 |
> | Lv.4 | 麵包店出爐輸送帶 | T23 | L293D＋2 馬達＋HC-SR04＋按鈕＋3 LED＋蜂鳴器 | `~` 腳只剩一隻 |
> | Lv.5 | 倉庫夜間巡檢車 | T24 | L293D＋4 馬達＋2 光敏電阻＋HC-SR04＋伺服＋蜂鳴器＋按鈕＋LED | **腳位剛好用完** |
> | Lv.6 | 顧問提案 | 自選 | 自訂 | 自己設計情境 |
>
> 📌 **作答規則**：先填「接法選擇」「腳位規劃表」「參數修改表」，再接線；接好後先用工具檢查，最後才上傳程式。Lv.3 之後每關要附**配線設計單**。

---

### 🟢 Lv.1 手搖飲店果汁機

**情境**：手搖飲店的果汁機有一顆按鈕，每按一次切換：**停止 → 低速 → 中速 → 高速 → 停止**。三顆燈顯示目前段數（低速亮 1 顆、中速亮 2 顆、高速亮 3 顆）。

**元件**：L293D＋直流馬達＋1 顆按鈕＋3 顆單色 LED

**套用模板**：T21

**限制條件**：店裡的 LED 招牌控制器已佔用 **~5、~6、~9、~10、~11**。

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| 接法（A／B／C／D） | — | | |
| `pinEN`、`pinIN1`、`pinIN2` | `?` | | |
| `ledLevel[]` | `{ ?, -1, -1 }` | | |
| `pinKnob`、`pinSensor` | `-1` | | |
| `numLevels` | 1 | | |
| `levelSpeed[]` | 255、170、255 | | 低、中、高 |
| `fanName` | `"Fan"` | | |
| 功能開關 | | | |

**情境思考**：店員常常忘記關果汁機。你會建議加什麼功能？需要哪些元件？

---

### 🟢 Lv.2 機房散熱風扇

**情境**：小型機房有**兩台散熱風扇**要同步運轉。溫度超過 **30°C** 自動啟動，降到 30°C 以下後**再轉 10 秒**才停。風量用旋鈕調整。維修人員也可以按按鈕強制開啟（再按一下關閉）。運轉燈亮表示風扇在轉。

**元件**：L293D＋2 顆直流馬達＋1 顆 TMP36＋1 顆旋鈕＋1 顆按鈕＋1 顆單色 LED

**套用模板**：T21

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| 接法（A／B／C／D） | — | | |
| L293D 三隻腳 | | | |
| 兩顆馬達怎麼接 | — | | |
| `pinKnob`、`pinSensor` | `-1` | | |
| `sensorType` | 1 | | |
| `autoLine` | 500 | | |
| `afterRun` | 5000 | | |
| 功能開關 | | | |

**情境思考**：散熱風扇壞掉時，系統會知道嗎？（提示：風扇在轉、溫度卻一直上升）你會建議加什麼「異常警報」？

---

### 🟡 Lv.3 住家車庫捲門

**情境**：住家車庫的捲門，牆上只有**一顆按鈕**：按一下上升、再按一下下降，全程約 **5 秒**。上升中亮綠燈、下降中亮紅燈，蜂鳴器間歇提醒。**車庫裡、車庫外各一組綠紅燈**，同步顯示。下降時門下 **40 cm** 內有東西就停止並上升 1.5 秒。

**元件**：L293D＋直流馬達＋1 顆按鈕＋4 顆單色 LED＋1 顆蜂鳴器＋1 顆 HC-SR04

**套用模板**：T22

**限制條件**：**Pin 2～6** 已被保全系統佔用；4 顆燈最多只能用 **2 隻腳**。

**你要想的問題**：

- 只有一顆按鈕，要接在 `btnUp` 還是 `btnDown`？另一個填什麼？
- 4 顆燈只用 2 隻腳，怎麼接？每顆燈要幾顆 220Ω？
- 扣掉 Pin 2～6 後，一般腳夠不夠？要不要借 A 腳？

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| 接法（A／B／C／D） | — | | |
| 腳位區（含不用的元件） | | | |
| `runTime` | 4000 | | |
| `oneButton` | `false` | | |
| `safeCM`、`reverseTime` | 30、1000 | | |
| `doorName` | `"Door"` | | |
| 功能開關 | | | |

**情境思考**：住家捲門通常用遙控器，萬一遙控器被別人複製，有什麼風險？從「安全」與「便利」兩個角度評估。

---

### 🟡 Lv.4 麵包店出爐輸送帶

**情境**：麵包店的出爐區有**兩條並排輸送帶**，固定速度 **150** 同步運轉，啟動和停止都要**緩慢變化**（避免麵包滑落）。末端超音波：麵包接近（25 cm 內）減速到一半，到末端（8 cm 內）停止，店員拿走麵包按「繼續鈕」再走。狀態用**紅、綠、黃三顆單色燈**，停止時蜂鳴器提醒。

**元件**：L293D＋2 顆直流馬達＋1 顆 HC-SR04＋1 顆按鈕＋3 顆單色 LED＋1 顆蜂鳴器

**套用模板**：T23

**限制條件**：**~5、~6、~9** 已被烤箱溫控板佔用。

**`~` 腳預算**：

| 項目 | 你的答案 |
| --- | --- |
| 有蜂鳴器，可用的 `~` 腳 | |
| 再扣掉被佔用的 | |
| EN 接在哪？三顆燈需要 `~` 腳嗎？ | |

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 |
| --- | --- | --- |
| `pinKnob` | `?` | |
| `baseSpeed` | 200 | |
| `slowCM`、`stopCM` | 30、10 | |
| `slowPercent` | 33 | |
| `colorRun[]`、`colorSlow[]`、`colorStop[]` | | |
| `lineName` | `"Conveyor"` | |
| 功能開關 | | |

**情境思考**：輸送帶停住等店員，烤箱卻一直出爐。麵包會在哪裡堆積？從「瓶頸」的角度，你會建議怎麼調整人力或設備？

---

### 🔴 Lv.5 倉庫夜間巡檢車

**情境**：冷鏈倉庫的夜間巡檢車，沿地上的**反光膠帶**（比地面亮）行駛；車子是**四輪驅動**（左右各兩顆馬達）。前方 **30 cm** 有障礙就停車鳴叫。到站點（兩側都看到膠帶）時停車，**相機雲台（伺服）**左右轉一圈拍照，到站燈亮 2 秒，再繼續。

**元件**：L293D＋4 顆直流馬達＋2 顆光敏電阻＋1 顆 HC-SR04＋1 顆伺服＋1 顆蜂鳴器＋1 顆按鈕＋1 顆單色 LED

**套用模板**：T24

**限制條件**：**Pin 7～10** 已被無線回報模組佔用。

**Step 0：腳位預算（本關重點）**

| 元件 | 需要幾隻腳 | 需要 `~` 腳嗎？ | 腳位 |
| --- | --- | --- | --- |
| 左、右輪 EN | 2 | | |
| 左、右光敏電阻 | 2（A 腳） | — | |
| HC-SR04 | 2 | | |
| 伺服 | 1 | | |
| 蜂鳴器 | 1 | | |
| 啟動鈕 | 1 | | |
| 到站燈 | 1 | | |
| **可用的一般腳（扣掉 7～10）** | | | |
| **還剩幾隻？** | | | |

**你要想的問題**：

- 有伺服＋蜂鳴器，兩個 EN 只能接哪兩隻？
- 反光膠帶比地面亮，怎麼接光敏電阻？
- 四顆馬達怎麼接到一顆 L293D？

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 |
| --- | --- | --- |
| `lineDark` | 300 | 依反相接法後的記錄表 |
| `stopCM` | 20 | |
| `forkDown`、`forkUp` | 0、70 | 雲台「左」「右」角度 |
| `stationTime` | 3000 | |
| `carName` | `"AGV"` | |
| 腳位區 | | |
| 功能開關 | | |

**情境思考**：

1. 附上你的腳位預算表。如果要再加一顆「車頭照明燈」，還放得下嗎？
2. 巡檢車在無人的夜間運作。如果它卡在障礙物前，誰會知道？你會建議建立什麼「異常回報」流程？

---

### 🏆 Lv.6 BOSS 關：顧問提案

**情境**：自己找一個生活或職場情境，**從 T21～T24 選一個模板**，設計一套「會持續運轉」的系統。

| 情境靈感 | 可能適合的模板 |
| --- | --- |
| 溫室循環風扇（天亮自動轉） | T21 |
| 舞台布幕升降 | T22 |
| 機場行李轉盤 | T23 |
| 醫院送藥車 | T24 |

**最低規格**：

- 至少 1 顆直流馬達，並說明選擇的 **L293D 接法與理由**
- 至少用到 8 隻腳位
- 至少修改模板參數區的 **5 個參數**
- 至少用到一個本週配線技巧（選接法省腳、對調馬達線、馬達並聯）
- 自己設計 1 題「配線變化挑戰」並附解答

**繳交內容**：情境說明、接法選擇與理由、配線設計單、參數修改表、配線變化挑戰與解答、Tinkercad 連結

---

### 📝 配線設計單

| 項目 | 內容 |
| --- | --- |
| 情境名稱 | |
| 套用模板 | T21／T22／T23／T24 |
| L293D 接法 | A／B／C／D，理由：要不要調速？要不要反轉？ |
| 元件清單 | 元件名稱 × 數量（含 220Ω、10kΩ） |
| 腳位分配表 | 元件 → 腳位（標註 `~` 腳、固定接 5V／GND 的 L293D 腳、借用 A 腳） |
| `~` 腳預算 | 有伺服？有蜂鳴器？EN 和哪些燈需要 `~` 腳？ |
| 馬達記錄表 | 實際轉向、調速是否符合接法 |
| 遇到的問題 | 接線過程中出過什麼錯？怎麼修正的？ |

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 馬達完全不轉 | L293D 的 16、8 有沒有接 5V；4、5、12、13 有沒有接 GND | 補接電源、接地 |
| 馬達不轉，但 Serial Monitor 正常 | 接法 A、C 的 EN 有沒有接 5V；接法 B 的 IN1 有沒有接 5V | 對照接法表 |
| 方向相反 | 馬達兩條線 | 對調（技巧 2） |
| 不能調速，只有全速 | EN 是否接在 `~` 腳；有伺服時是否接在 9、10，有蜂鳴器時是否接在 3、11 | 查 ~ 腳可用表改接 |
| 反轉指令沒反應 | 是否用了接法 A 或 B | 需要反轉就改用接法 C 或 D |
| 兩顆並聯的馬達一顆轉一顆不轉 | 第二顆的兩條線是否接在同一組 OUT | 對照技巧 3 |
| 晶片腳位接錯一排 | 缺口方向 | 缺口朝上，重新對照腳位圖 |
| 程式無法上傳 | 腳位區是否還有 `?` 沒填 | 用到的填腳位，固定接 5V／GND 或沒接的填 `-1` |

---

## 成果繳交與評分

### 繳交清單

每個關卡、每個練習題各自建立一個 Tinkercad 檔案，命名格式：`W10-關卡或Lv編號-限制卡-學號`。

| 項目 | 繳交內容 |
| --- | --- |
| 關卡 1～4 | Tinkercad 連結＋決策題答案（含接法選擇）＋腳位規劃表＋馬達記錄表＋配線變化挑戰說明＋同學驗收簽名 |
| Lv.1～Lv.2 | Tinkercad 連結＋參數修改表＋情境思考 |
| Lv.3～Lv.5 | ＋配線設計單 |
| Lv.6 | 完整顧問提案 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 接法選擇與 `~` 腳預算 | 25% | 依需求選出最省腳的接法；EN 正確避開被借走的 `~` 腳 |
| 電路正確性 | 25% | L293D 電源、接地完整；每個 Step 通過工具檢查 |
| 配線變化挑戰 | 20% | 正確運用對調馬達線、馬達並聯、光敏電阻反相等技巧 |
| 參數修改表 | 15% | 速度、時間、門檻參數修改正確並說明理由 |
| 情境思考 | 15% | 💼 管理面向論述 |

> 🎯 **最低完成標準**：關卡 1～4 全部完成（含配線變化挑戰）、Lv.1～Lv.3 完成。

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 40 分鐘 | 認識 L293D、接法 A～D、三個新技巧、馬達測試器 |
| 第 1 節後段 | 關卡 1 |
| 第 2 節 | 關卡 2、關卡 3 |
| 第 3 節前段 | 關卡 4 |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，其餘帶回家 |

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 直流馬達（DC Motor） | 通電就一直轉，兩條線接反就反轉 |
| L293D | 馬達驅動晶片，一顆可以帶兩組馬達 |
| EN（Enable） | 馬達的「油門」，接 `~` 腳可以調速 |
| IN1、IN2 | 馬達的「方向」，HIGH／LOW 組合決定正轉、反轉、停止 |
| 接法 A／B／C／D | 依「要不要調速、要不要反轉」選擇的四種接法，佔 1～3 隻腳 |
| 馬達並聯 | 兩顆馬達接在同一組 OUT，同步轉動、不多佔腳 |
| 緩啟動、緩停止 | 速度慢慢加減，避免貨物倒下 |
| 差速轉向 | 左右輪速度不同來轉彎 |

---

## 反思與延伸討論

1. 本週同一個抽風扇，限制卡 A 佔 1 隻 `~` 腳、限制卡 B 只佔 1 隻一般腳。「依需求選規格」在採購管理上叫做什麼？買了用不到的功能（例如不需要反轉卻用接法 D），會造成哪些成本？
2. 馬達方向靠「對調兩條線」修正，伺服方向卻只能改參數。請把目前學過的元件分成兩類：方向問題靠改線解決的、靠改參數解決的。
3. 本週多次出現「時間到就當作到位」（捲門 4 秒、AGV 離開橫線 0.8 秒）。這種開環控制有什麼風險？真實設備怎麼確認「真的到位了」？

---

## 本週檢核清單

- [ ] 能正確擺放 L293D 並接好電源、接地（16、8 接 5V；4、5、12、13 接 GND）
- [ ] 能依「要不要調速、要不要反轉」選出接法 A～D，並在腳位區正確填 `-1`
- [ ] 能用對調馬達兩條線修正方向，用並聯讓兩顆馬達同步
- [ ] 能做出 EN 與 RGB 的 `~` 腳預算
- [ ] 完成關卡 1～4，並取得同學驗收簽名
- [ ] 完成練習題 Lv.1～Lv.3，挑戰 Lv.4～Lv.6

---

## 下週預告

第 11 週我們要讓系統「**會說話**」——加入 **LCD 16×2 顯示器**。LCD 有兩種版本：平行版佔 6 隻腳、I2C 版只佔 2 隻（固定 A4、A5）。選哪一種，又是一次腳位規劃的取捨。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 9 週](week-09.md) | [下一週：第 11 週 — 會說話的智慧場域 ➡](week-11.md)
