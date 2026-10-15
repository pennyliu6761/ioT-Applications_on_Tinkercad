[⬅ 回目錄](../README.md) | [⬅ 上一週：第 13 週](week-13.md) | [下一週：第 15 週 — 期末整合大專題（上）➡](week-15.md)

---

# 第 14 週：自動與手動的智慧場域

## 主題專案：感應、操作與「誰說了算」——工業控制盤的核心邏輯

> 🎯 **本週定位**：這是學習新元件的**最後一週**。我們加入三個元件，補齊工業控制盤最常見的拼圖：
>
> - **繼電器（Relay）**：讓 Arduino 控制「另一條電路」的開關，例如電燈、冷氣、抽水馬達
> - **PIR 人體感測器**：直接偵測「有沒有人在動」
> - **類比搖桿（Joystick）**：讓操作員用手推動，同時控制兩個方向
>
> 本週最重要的配線決策：**繼電器的負載接 NO 還是 NC？**——這決定了「系統壞掉時，設備停在什麼狀態」。同一個情境，抽到不同限制卡就會做出不同的失效安全選擇。另一個主角是管理概念：**手動／停止／自動（HOA）**三段模式與優先權。
>
> 每一個關卡照「五個固定環節」進行：抽**客戶限制卡** → 回答**接線前決策題** → 照**接線任務**一步步接 → 套用**程式模板**（只改腳位區、參數區） → 完成**配線變化挑戰**（程式一個字都不改，只改線）。
>
> 🏁 **本週成果**：完成 **4 個關卡** ＋ **練習題 Lv.1～Lv.6**（套用本週的程式模板 T37～T40）。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件 ①：繼電器](#認識新元件-繼電器)
- [認識新元件 ②：PIR 人體感測器](#認識新元件-pir-人體感測器)
- [認識新元件 ③：類比搖桿](#認識新元件-類比搖桿)
- [本週核心觀念：HOA 三段模式與優先權](#本週核心觀念hoa-三段模式與優先權)
- [本週配線新技巧](#本週配線新技巧)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [關卡怎麼玩：五個固定環節](#關卡怎麼玩五個固定環節)
- [關卡 1：樓梯間感應燈（模板 T37）](#關卡-1樓梯間感應燈模板-t37)
- [關卡 2：會議室空調節能（模板 T38）](#關卡-2會議室空調節能模板-t38)
- [關卡 3：倉庫天車操作台（模板 T39）](#關卡-3倉庫天車操作台模板-t39)
- [關卡 4：輸送線控制盤（模板 T40）](#關卡-4輸送線控制盤模板-t40)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 倉庫走道感應燈](#-lv1-倉庫走道感應燈)
  - [Lv.2 咖啡機保溫電源](#-lv2-咖啡機保溫電源)
  - [Lv.3 停車場監視器雲台](#-lv3-停車場監視器雲台)
  - [Lv.4 自助洗車輸送線](#-lv4-自助洗車輸送線)
  - [Lv.5 機場行李輸送控制盤](#-lv5-機場行李輸送控制盤)
  - [🏆 Lv.6 BOSS 關：期末專題預演提案](#-lv6-boss-關期末專題預演提案)
  - [📝 配線設計單](#-配線設計單)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～13 週）

- ✅ 15 種元件的接線（含 74HC595、七段顯示器）
- ✅ 「改線就能解決」的技巧：共腳、反相、共用訊號線、整組對調、馬達並聯、按鈕並聯……
- ✅ 安全連鎖、急停、鎖定與復歸、優先權順序（期中專題）

### 這週要新學的

| 新東西 | 白話說明 | 跟哪個學過的最像 |
| --- | --- | --- |
| 繼電器 | 用電控制的開關：Arduino 一個小訊號，切換另一條電路 | L293D（第 10 週）：都是「以小控大」 |
| 常開（NO）／常閉（NC） | 沒通電時，開關是「斷開」還是「接通」 | 全新觀念 |
| PIR 人體感測器 | 偵測到有人移動就輸出 HIGH | 按鈕（第 2 週）：都是數位輸入 |
| 類比搖桿 | 兩顆旋鈕合體，一顆管左右、一顆管上下 | 旋鈕（第 4 週） |
| HOA 三段模式 | 手動（Hand）／停止（Off）／自動（Auto） | 全新管理觀念 |
| 程式模板 T37～T40 | 本週四個新模板 | 公司的標準表單 |

### 本週可用元件（只限這些）

第 1～13 週學過的全部元件，加上本週的 **繼電器、PIR 人體感測器、類比搖桿**。

---

## 開場任務簡報

> 📖 **情境**：顧問公司的客戶開始要求「真正能上線」的系統：要能控制真實的電器、要能自動偵測有人，也要能讓現場人員隨時接手操作。這週的四個委託案，都在處理「自動」和「手動」怎麼共存。
>
> | 關卡 | 客戶 | 委託內容 | 套用模板 | 難度 |
> | --- | --- | --- | --- | --- |
> | 1 | 社區大樓 | 樓梯間感應燈 | T37 感應照明 | ⭐ |
> | 2 | 共享辦公室 | 會議室空調節能 | T38 HOA 節能控制 | ⭐⭐ |
> | 3 | 物流倉庫 | 天車操作台 | T39 搖桿操作台 | ⭐⭐⭐ |
> | 4 | 食品工廠 | 輸送線控制盤 | T40 輸送線控制盤 | ⭐⭐⭐⭐ |

---

## 認識新元件 ①：繼電器

### 它在做什麼？

繼電器是一個**用電控制的開關**。裡面有一個小電磁鐵：Arduino 送電給電磁鐵，電磁鐵就把開關「吸過去」。

```
   控制端（Arduino 這邊）              負載端（被控制的電器那邊）
  ┌──────────────┐                ┌──────────────────────┐
  │ Arduino 腳位 ─┤ 線圈（電磁鐵） ├─ COM ──┬── NO（常開）│
  │ GND ─────────┤                ├        └── NC（常閉）│
  └──────────────┘                └──────────────────────┘
       小訊號                           另一條獨立的電路
```

- **控制端**和**負載端**是兩條完全分開的電路（電氣隔離）。真實世界中，負載端可以是 110V 的電燈、冷氣——Arduino 完全碰不到高壓電。
- 在 Tinkercad 本課程用 **LED＋220Ω** 模擬負載，負載端的電源取自麵包板 5V。

### 接法

| 繼電器的腳 | 白話 | 接到哪裡 |
| --- | --- | --- |
| 線圈（Coil）兩腳 | 電磁鐵 | 一腳接 Arduino 數位腳、一腳接 GND |
| COM | 共同點 | 負載電源（5V） |
| NO（常開） | 沒通電時**斷開**，通電時接通 | 負載（LED＋220Ω → GND） |
| NC（常閉） | 沒通電時**接通**，通電時斷開 | 負載（LED＋220Ω → GND） |

> ⚠️ **真實硬體**上，Arduino 腳位的力氣不足以直接推動繼電器線圈，通常會使用內建驅動電路的「繼電器模組」（VCC、GND、IN 三腳）。Tinkercad 模擬可以直接接。

### NO 還是 NC？

| 程式送出 | 接在 NO 的負載 | 接在 NC 的負載 |
| --- | --- | --- |
| `HIGH`（線圈通電） | **開** | 關 |
| `LOW`（線圈不通電） | 關 | **開** |
| **Arduino 當機、斷電** | **關** | **開** |

選擇的關鍵問題只有一個：**「如果控制系統壞了，這個負載應該停在什麼狀態，才是安全的？」**

| 情境 | 系統壞掉時應該 | 接哪一個 |
| --- | --- | --- |
| 輸送帶馬達、加熱器 | 停止（避免受傷、火災） | NO |
| 一般照明、冷氣 | 關掉（省電） | NO |
| 逃生出口指示燈 | 保持亮著 | NC |
| 電磁門鎖（斷電開門型） | 打開（讓人能逃生） | NC |

> 🏭 這叫做**失效安全（Fail-safe）**設計：系統故障時，自動回到最安全的狀態。

---

## 認識新元件 ②：PIR 人體感測器

PIR 偵測人體發出的**紅外線變化**：有人在範圍內移動 → 輸出 HIGH；沒人或人靜止不動 → 輸出 LOW。

| 比較 | 光敏電阻（第 5 週） | 超音波（第 9 週） | PIR（本週） |
| --- | --- | --- | --- |
| 偵測什麼 | 光線被擋住 | 前方物體的距離 | **人或動物在移動** |
| 讀到什麼 | 0～1023 | 公分數 | HIGH／LOW |
| 缺點 | 受環境光影響 | 只看正前方一條線 | **人不動就偵測不到** |

| PIR 的腳 | 接到哪裡 |
| --- | --- |
| 電源（+） | 5V |
| 訊號（OUT） | 任何數位腳 |
| 接地（−） | GND |

- 讀法和按鈕一樣，但**不需要 10kΩ**。
- 在 Tinkercad 開始模擬後，**點一下 PIR**，前方會出現可以拖曳的物體，在偵測範圍內移動它就會觸發。

---

## 認識新元件 ③：類比搖桿

搖桿就是**兩顆旋鈕合體**：往左右推是 X 軸、往前後推是 Y 軸，各自輸出 0～1023，**放開時回到中間（約 512）**。

| 搖桿的腳 | 接到哪裡 |
| --- | --- |
| 電源（+5V） | 5V |
| 接地（GND） | GND |
| X 軸（VRx） | A 腳 |
| Y 軸（VRy） | 另一隻 A 腳 |

> 💡 **Tinkercad 若找不到搖桿元件**，用**兩顆旋鈕**代替：一顆接 X 軸的 A 腳、一顆接 Y 軸的 A 腳，接法和程式**完全相同**，只是放開時不會自動回中，要記得手動轉回中間。

**死區（Deadband）**：搖桿放開時很少剛好是 512。本週模板設定一個死區參數：在中間 ±150 以內都當作「沒推」。

---

## 本週核心觀念：HOA 三段模式與優先權

### HOA：工業控制盤上一定有的三段開關

| 模式 | 英文 | 誰在控制 | 什麼時候用 |
| --- | --- | --- | --- |
| **自動** | Auto | 感測器與程式 | 平常運轉 |
| **手動** | Hand | 現場人員（按鈕、搖桿） | 維修、測試、感測器故障時 |
| **停止** | Off | 沒有人能啟動 | 保養、清潔、停機檢查 |

本週模板用一顆「模式鈕」循環切換：**AUTO → HAND → OFF → AUTO**，並用燈號顯示目前模式。

### 優先權：不管哪個模式，安全永遠第一

```
每一輪依序檢查（越上面越優先）：
  ① 急停？                → 全部停止，什麼模式都沒用
  ② 安全感測（有人闖入）？ → 停止或降速
  ③ 目前模式是 OFF？       → 停止
  ④ 目前模式是 HAND？      → 聽操作員的
  ⑤ 目前模式是 AUTO？      → 聽感測器的
```

---

## 本週配線新技巧

### 技巧 1：負載換到 NC，要同步改一個參數

本週模板都有 `loadOnNC`（或類似名稱）參數。**負載接 NO 時填 `false`、接 NC 時填 `true`**，程式會自動把開／關訊號反過來，讓「正常運作時」的行為一樣，只有「系統壞掉時」的狀態不同。

> ⚠️ 這是少數「改線之後也要改參數」的情況（和第 12 週的鍵盤 C4 一樣）。只改線、不改參數，設備會「該開的時候關、該關的時候開」。

### 技巧 2：一顆繼電器同時帶兩個相反的負載

繼電器的 NO、NC **可以同時接負載**：

```
5V ── COM ──┬── NO ── 220Ω ── 運轉燈 ── GND   （通電時亮）
            └── NC ── 220Ω ── 停止燈 ── GND   （不通電時亮）
```

「運轉中」和「停止中」兩顆燈永遠一亮一滅——**不用改程式、不多佔 Arduino 的腳**。

### 技巧 3：同一個接點帶多個負載

NO（或 NC）可以並聯好幾個負載（各自串 220Ω），例如「走道兩端的燈一起亮」——一顆繼電器、一隻腳。

### 技巧 4：搖桿方向反了 → 改線

| 現象 | 修正（不改程式） |
| --- | --- |
| 往右推卻往左轉 | 對調那一軸的 5V、GND（用旋鈕代替時：對調旋鈕兩側腳） |
| 推左右卻是上下在動 | 對調 X、Y 兩條 A 腳線 |

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **繼電器：線圈一腳接 Arduino、一腳接 GND；COM 接負載電源** | 控制端和負載端是兩條電路 |
| ② **先問「壞掉時要停在哪裡」，再決定接 NO 或 NC** | 接 NC 時，`loadOnNC` 改成 `true` |
| ③ NO、NC 可以同時接負載；同一接點可以並聯多個負載 | 不多佔腳 |
| ④ **PIR：+ 接 5V、OUT 接數位腳、− 接 GND，不用電阻** | 讀法和按鈕一樣 |
| ⑤ **搖桿＝兩顆旋鈕：X、Y 各接一隻 A 腳** | 方向反了改線 |
| ⑥ 每條燈線一顆 220Ω（負載 LED 也要） | 繼電器、PIR、搖桿不用電阻 |
| ⑦ 腳位不夠時：借 A 腳、I2C LCD、74HC595、鍵盤 3 行 | 參考第 12 週總整理 |

---

## 共用工具

### 🔧 工具 1：繼電器＋PIR＋搖桿綜合測試器

一次測三個新元件：繼電器每 2 秒切換一次（負載 LED 亮滅交替）；Serial Monitor 同時顯示 PIR 狀態和搖桿 X、Y 數值。

```cpp
// ===== 改成你的腳位 =====
int pinRelay = 7;    // 繼電器線圈
int pinPIR   = 2;    // PIR 訊號
int joyX = A0, joyY = A1;   // 搖桿 X、Y（或兩顆旋鈕）
// =======================

unsigned long lastToggle = 0;
bool relayOn = false;

void setup() {
  pinMode(pinRelay, OUTPUT);
  pinMode(pinPIR, INPUT);
  Serial.begin(9600);
}

void loop() {
  if (millis() - lastToggle > 2000) {
    relayOn = !relayOn;
    digitalWrite(pinRelay, relayOn ? HIGH : LOW);
    lastToggle = millis();
  }
  Serial.print("Relay: ");
  Serial.print(relayOn ? "ON " : "OFF");
  Serial.print("  PIR: ");
  Serial.print(digitalRead(pinPIR) == HIGH ? "MOTION" : "-     ");
  Serial.print("  X: ");
  Serial.print(analogRead(joyX));
  Serial.print("  Y: ");
  Serial.println(analogRead(joyY));
  delay(200);
}
```

**檢查標準**：

| 元件 | 正常現象 |
| --- | --- |
| 繼電器 | Relay ON 時，接在 NO 的 LED 亮、接在 NC 的 LED 滅；OFF 時相反 |
| PIR | 拖曳物體在範圍內移動時顯示 MOTION |
| 搖桿 | 推到底約 0 或 1023，放開回到約 512 |

**失效安全表**（每個繼電器都要填）：

| 繼電器 | 負載 | 接 NO 或 NC | 系統壞掉時負載會…… | 為什麼這樣比較安全 |
| --- | --- | --- | --- | --- |
| 第 1 顆 | | | | |

### 🔧 其他工具（沿用前幾週）

LCD 測試器、馬達測試器、超音波測距檢查器、溫度讀值檢查器、伺服測試器、蜂鳴器測試器、接線驗證器。

---

## 關卡怎麼玩：五個固定環節

| 環節 | 你要做什麼 | 產出 |
| --- | --- | --- |
| 🃏 **① 抽客戶限制卡** | 依座號尾數找到你的限制卡 | 圈出你的限制卡 |
| ✏️ **② 接線前決策題** | **先填失效安全表（NO／NC）**、畫優先權順序，填好**腳位規劃表** | 決策題答案＋兩張表 |
| 🔌 **③ 接線任務** | 照 Step 1 → 2 → 3 接線，每一步都用工具檢查 | 每一步的檢查截圖 |
| 💻 **④ 套用程式模板** | 只改「腳位區」和「參數區」，再把功能開關逐一打開 | 可運作的 Tinkercad 檔案 |
| 🔄 **⑤ 配線變化挑戰** | **程式一個字都不能改**，只靠改線完成客戶的臨時需求 | 修改後的截圖＋說明 |

| 座號尾數 | 抽到 |
| --- | --- |
| 1、4、7 | 限制卡 A |
| 2、5、8 | 限制卡 B |
| 3、6、9、0 | 限制卡 C |

---

## 關卡 1：樓梯間感應燈（模板 T37）

⭐ **難度**：PIR ＋ 繼電器，再加光敏電阻與第二顆 PIR

### 📖 情境

社區大樓的樓梯間，有人走過燈就亮，**最後一次偵測到人之後 10 秒**才熄滅（避免人走到一半燈就暗了）。燈泡是 110V 的，所以 Arduino 不能直接控制，要透過繼電器。白天光線充足時不需要開燈。第三版：樓梯**上下兩端各一顆 PIR**。

### 🎯 本關配線學習目標

- 第一次接繼電器：控制端、負載端兩條電路
- 依失效安全原則選 NO 或 NC

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 一般樓梯燈：系統壞掉時**關燈省電** |
| **B** | 這是**逃生樓梯**，消防規定：系統壞掉時燈**必須亮著** |
| **C** | 一般樓梯燈；**每層樓兩盞燈**一起亮；樓梯口還要一顆「**節電中**」指示燈（燈沒開時亮） |

### ✏️ ② 接線前決策題

1. 填失效安全表：依你的限制卡，燈接 NO 還是 NC？`loadOnNC` 填什麼？
2. 光敏電阻要裝在哪裡？如果裝在樓梯燈正下方會怎樣？
3. 限制卡 C：兩盞燈和「節電中」指示燈，一共要幾顆繼電器？幾隻 Arduino 腳？（提示：技巧 2、3）
4. 兩顆 PIR 要佔幾隻腳？能不能像第 13 週的按鈕一樣並聯？（提示：PIR 的輸出是「主動送出 HIGH 或 LOW」，按鈕是「接通或斷開」——兩顆 PIR 一顆送 HIGH、一顆送 LOW 時會怎樣？）

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| PIR ①（樓下） | |
| PIR ②（樓上） | |
| 繼電器線圈 | |
| 光敏電阻 | |

### 🔌 ③ 接線任務

**Step 1：PIR ①＋繼電器＋負載燈**

1. 繼電器線圈接 Arduino 腳與 GND；COM 接 5V；燈（串 220Ω）接在 NO 或 NC（依失效安全表）。限制卡 C：兩盞燈並聯在同一個接點，指示燈接在另一個接點。
2. ✅ 檢查：**工具 1**。

**Step 2：光敏電阻**

1. 光敏電阻配 10kΩ，接 A 腳。
2. ✅ 檢查：類比讀值檢查器，記錄白天、晚上的讀值。

**Step 3：PIR ②**

1. 接好第二顆 PIR。

### 💻 ④ 套用程式模板 T37

```cpp
// ===== 程式模板 T37：感應照明 =====

// ----- 腳位區（沒接的填 -1）-----
int pinPIR   = ?;                   // PIR ①
int pinPIR2  = -1;                  // PIR ②（第二個偵測點）
int pinRelay = ?;                   // 照明繼電器線圈
int pinLDR   = -1;                  // 光敏電阻

// ----- 參數區 -----
int holdTime = 10000;               // 最後一次偵測後維持幾毫秒
int darkLine = 300;                 // 光線低於這個數字才需要開燈
bool loadOnNC = false;              // 燈接在 NC 時改成 true
String areaName = "Stairs";         // 顯示名稱（英文）

// ----- 功能開關 -----
bool useDaylight = false;           // Step 2：白天不開燈
bool useSecond   = false;           // Step 3：第二顆 PIR

// ===== 以下不用改 =====
bool everMotion = false, lampOn = false;
unsigned long lastMotion = 0;

void relay(bool on) {
  bool coil = loadOnNC ? !on : on;                         // 接 NC 時，訊號反過來
  digitalWrite(pinRelay, coil ? HIGH : LOW);
}

void setup() {
  pinMode(pinPIR, INPUT);
  if (pinPIR2 >= 0) pinMode(pinPIR2, INPUT);
  pinMode(pinRelay, OUTPUT);
  relay(false);
  Serial.begin(9600);
}

void loop() {
  int who = 0;
  if (digitalRead(pinPIR) == HIGH) who = 1;
  if (useSecond && pinPIR2 >= 0 && digitalRead(pinPIR2) == HIGH) who = 2;
  if (who > 0) { everMotion = true; lastMotion = millis(); }

  bool want = everMotion && (millis() - lastMotion < (unsigned long)holdTime);
  if (useDaylight && pinLDR >= 0 && analogRead(pinLDR) >= darkLine) want = false;   // 白天不開

  if (want != lampOn) {
    lampOn = want;
    Serial.print(areaName + (lampOn ? " lamp ON" : " lamp OFF"));
    if (lampOn && who > 0) { Serial.print("  (sensor "); Serial.print(who); Serial.print(")"); }
    Serial.println();
  }
  relay(lampOn);
  delay(50);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表 |
| 參數區 | `darkLine` 依記錄表；`loadOnNC` 依失效安全表（限制卡 B 是 `true`） |
| 功能開關 | Step 2 → `useDaylight = true`；Step 3 → `useSecond = true` |

### 🔄 ⑤ 配線變化挑戰

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「樓梯口加一顆『節電中』指示燈，燈沒開時亮」（限制卡 C 的同學：改成「再加一盞燈」） | （提示：技巧 2） |
| 「這座樓梯被改成逃生梯了，系統壞掉時燈要亮」（限制卡 B 的同學：改成一般樓梯） | （提示：技巧 1——這題要改線**也要**改一個參數） |
| 「燈一直亮著不會熄」——老師會動手腳 | |

### 💼 情境思考

1. 有人在樓梯間停下來講電話 2 分鐘，燈會怎樣？這是 PIR 的什麼特性造成的？你會怎麼改善？
2. 管委會想知道「改成感應燈一年可以省多少電費」。你需要收集哪些資料才能估算？

### ✅ 關卡 1 驗收

- [ ] 失效安全表已填，燈接在正確的接點、`loadOnNC` 正確
- [ ] 有人移動時燈亮，最後一次偵測後約 10 秒熄滅
- [ ] 白天（光線充足）時即使有人也不開燈
- [ ] 兩顆 PIR 都能觸發（Serial Monitor 顯示是哪一顆）
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 2：會議室空調節能（模板 T38）

⭐⭐ **難度**：繼電器 ＋ HOA 模式鈕 ＋ PIR ＋ TMP36 ＋ 模式燈 ＋ 蜂鳴器

### 📖 情境

共享辦公室的會議室冷氣常常「開會結束沒人關」。客戶希望：

- **AUTO**：有人在（PIR）**而且**溫度高於 26°C 才開冷氣；降到 24°C 以下關掉；**沒人超過 15 秒**自動關閉
- **HAND**：不管感測器，冷氣一直開（例如 PIR 偵測不到坐著不動的人時）
- **OFF**：冷氣關閉，誰也開不了（例如冷氣保養中）
- 模式燈顯示目前模式（AUTO 綠、HAND 黃、OFF 紅），切換模式時嗶一聲

### 🎯 本關配線學習目標

- 用一顆模式鈕做 HOA 三段切換
- 一顆繼電器同時帶多個負載

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：冷氣 |
| **B** | 這是**冬天用的暖氣**：低於 **18°C** 開、高於 **20°C** 關 |
| **C** | 冷氣和**新風機**一起開關；模式燈改用**綠、黃、紅三顆單色燈**；**Pin 2～7** 被門禁系統佔用 |

### ✏️ ② 接線前決策題

1. 填失效安全表：冷氣（或暖氣）接 NO 還是 NC？
2. 模式燈只需要綠、黃、紅（全亮全滅），需要 `~` 腳嗎？
3. 限制卡 B：暖氣要改哪幾個參數？
4. 限制卡 C：冷氣、新風機要幾顆繼電器？三顆單色燈怎麼對應 `pinR`、`pinG`、`pinB`？顏色參數怎麼改？
5. 畫出 HOA 的切換順序和每個模式下冷氣的行為。

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| 繼電器線圈 | |
| 模式鈕 | |
| PIR | |
| TMP36 | |
| 模式燈 R／G／B（C：綠、黃、紅三顆） | |
| 蜂鳴器 | |

### 🔌 ③ 接線任務

**Step 1：繼電器＋負載＋模式鈕**

1. 負載燈接在你選的接點（限制卡 C：新風機的燈並聯在同一個接點）；模式鈕配 10kΩ。
2. ✅ 檢查：工具 1。

**Step 2：PIR＋TMP36**

1. 接好 PIR、TMP36。
2. ✅ 檢查：工具 1、溫度讀值檢查器。

**Step 3：模式燈＋蜂鳴器**

1. 燈各串 220Ω；接好蜂鳴器。
2. ✅ 檢查：接線驗證器、蜂鳴器測試器。

### 💻 ④ 套用程式模板 T38

```cpp
// ===== 程式模板 T38：HOA 節能控制 =====

// ----- 腳位區（沒接的填 -1）-----
int pinRelay = ?;                   // 設備繼電器線圈
int btnMode  = ?;                   // 模式鈕（AUTO → HAND → OFF）
int pinPIR   = ?;                   // PIR
int pinTemp  = A0;                  // TMP36
int pinR = ?, pinG = ?, pinB = ?;   // 模式燈（只用全亮全滅顏色）
int pinBuzzer = ?;                  // 蜂鳴器

// ----- 參數區 -----
bool heatMode = false;              // false：冷氣（太熱才開）；true：暖氣（太冷才開）
float onLine  = 26.0;               // 冷氣：高於這個溫度開；暖氣：低於這個溫度開
float offLine = 24.0;               // 冷氣：低於這個溫度關；暖氣：高於這個溫度關
unsigned long emptyDelay = 15000;   // 沒人多久自動關（毫秒）
bool loadOnNC = false;              // 負載接在 NC 時改成 true
int colorAuto[3] = { 0, 1, 0 };     // AUTO（R, G, B 開關：1 亮、0 滅）
int colorHand[3] = { 1, 1, 0 };     // HAND
int colorOff[3]  = { 1, 0, 0 };     // OFF
int modeTone = 1200;
String deviceName = "AC";           // 顯示名稱（英文）

// ----- 功能開關 -----
bool useAuto      = false;          // Step 2：AUTO 依感測器
bool useIndicator = false;          // Step 3：模式燈＋蜂鳴器

// ===== 以下不用改 =====
int mode = 0;                       // 0=AUTO, 1=HAND, 2=OFF
String modeName[3] = { "AUTO", "HAND", "OFF" };
int lastButton = LOW;
unsigned long lastMotion = 0, lastReport = 0;
bool active = false, devOn = false;

float readC(int pin) {
  return (analogRead(pin) * 5.0 / 1023.0 - 0.5) * 100.0;
}

void relay(bool on) {
  bool coil = loadOnNC ? !on : on;
  digitalWrite(pinRelay, coil ? HIGH : LOW);
}

void setColor(int c[]) {
  if (pinR >= 0) digitalWrite(pinR, c[0] ? HIGH : LOW);
  if (pinG >= 0) digitalWrite(pinG, c[1] ? HIGH : LOW);
  if (pinB >= 0) digitalWrite(pinB, c[2] ? HIGH : LOW);
}

void setup() {
  pinMode(pinRelay, OUTPUT);
  relay(false);
  pinMode(btnMode, INPUT);
  if (pinPIR >= 0) pinMode(pinPIR, INPUT);
  if (pinR >= 0) pinMode(pinR, OUTPUT);
  if (pinG >= 0) pinMode(pinG, OUTPUT);
  if (pinB >= 0) pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // ---- 模式切換 ----
  int now = digitalRead(btnMode);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    mode = (mode + 1) % 3;
    Serial.print("MODE -> ");
    Serial.println(modeName[mode]);
    if (useIndicator && pinBuzzer >= 0) tone(pinBuzzer, modeTone, 100);
  }
  lastButton = now;

  // ---- 依模式決定設備開關 ----
  float t = readC(pinTemp);
  if (mode == 1) {
    devOn = true;                                          // HAND：一直開
  } else if (mode == 2) {
    devOn = false;                                         // OFF：一律關
  } else if (useAuto) {                                    // AUTO：有人＋溫度
    if (pinPIR >= 0 && digitalRead(pinPIR) == HIGH) lastMotion = millis();
    bool occupied = (lastMotion > 0) && (millis() - lastMotion < emptyDelay);
    if (heatMode) { if (t < onLine) active = true; if (t > offLine) active = false; }
    else          { if (t > onLine) active = true; if (t < offLine) active = false; }
    devOn = occupied && active;
  } else {
    devOn = false;                                         // Step 1：AUTO 尚無感測器
  }
  relay(devOn);

  if (useIndicator) {
    if (mode == 0)      setColor(colorAuto);
    else if (mode == 1) setColor(colorHand);
    else                setColor(colorOff);
  }

  if (millis() - lastReport > 1000) {
    lastReport = millis();
    Serial.print(modeName[mode]);
    Serial.print("  Temp ");
    Serial.print(t);
    Serial.println(" C  " + deviceName + (devOn ? " ON" : " OFF"));
  }
  delay(50);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表 |
| 參數區 | `loadOnNC` 依失效安全表；限制卡 B 的 `heatMode = true`、`onLine = 18`、`offLine = 20`、`deviceName = "HEATER"`；限制卡 C 依決策題 4 改三種顏色 |
| 功能開關 | Step 2 → `useAuto = true`；Step 3 → `useIndicator = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「冷氣開的時候，新風機也要一起開」（限制卡 C 的同學：改成「新風機拆掉」） | （提示：技巧 3） |
| 「會議室門口要一顆『空調運轉中』燈、一顆『空調關閉』燈」 | （提示：技巧 2） |
| 「OFF 模式時冷氣反而一直開」——老師會動手腳 | （提示：哪個接點？） |

### 💼 情境思考

1. 有人覺得 AUTO 太麻煩，把模式永遠切在 HAND。這對節能有什麼影響？管理上怎麼避免？
2. OFF 模式「誰也開不了」，是為了保護誰？在真實工廠中，OFF 模式通常會搭配什麼制度？（提示：上鎖掛牌 LOTO）

### ✅ 關卡 2 驗收

- [ ] 模式鈕能循環 AUTO → HAND → OFF，模式燈正確（限制卡 C：三顆單色燈）
- [ ] HAND 一直開、OFF 一律關
- [ ] AUTO：有人且溫度條件成立才開，回到另一條線時關，沒人 15 秒後關（限制卡 B：暖氣方向正確）
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 3：倉庫天車操作台（模板 T39）

⭐⭐⭐ **難度**：搖桿 ＋ 2 顆伺服 ＋ 繼電器（電磁鐵）＋ PIR 安全連鎖 ＋ 狀態燈

### 📖 情境

倉庫的小型天車（吊車），操作員用搖桿控制：**左右推 → 吊臂旋轉、前後推 → 吊鉤升降**（兩顆伺服）。按「吸附鈕」切換吊鉤上的**電磁鐵**（繼電器控制，負載用 LED 模擬），吸起或放下鐵製貨物。

**安全規定**：作業區內有人走動（PIR）時，**天車立刻停止所有動作**，狀態燈轉紅；但**電磁鐵不能放開**——放開的話，吊在半空中的貨物會砸下來。人離開 3 秒後才恢復操作。

### 🎯 本關配線學習目標

- 搖桿兩軸接 A 腳，方向靠接線調整
- 電磁鐵的失效安全：NO 還是 NC 都有風險，怎麼取捨

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：搖桿；一般電磁鐵（通電吸住） |
| **B** | 庫存沒有搖桿，用**兩顆旋鈕**代替；操作員習慣「旋鈕往右轉＝吊臂往**左**轉」 |
| **C** | 作業區有**兩個入口**，各裝一顆 PIR；客戶採用「**斷電吸住型**」電磁鐵（不通電時吸住，通電才放開） |

### ✏️ ② 接線前決策題

1. 填失效安全表：一般電磁鐵接 NO 時，Arduino 斷電，貨物會怎樣？限制卡 C 的「斷電吸住型」要怎麼接、`magnetOnNC` 填什麼？
2. 系統有伺服，哪些 `~` 腳不能調光？狀態燈需要調光嗎？
3. 限制卡 B：「往右轉＝往左轉」要改參數還是改線？
4. 限制卡 C：兩顆 PIR 各接哪裡？

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| 搖桿 X、Y（B：兩顆旋鈕） | |
| 旋轉伺服、升降伺服 | |
| 吸附鈕 | |
| 電磁鐵繼電器線圈 | |
| PIR ①、PIR ② | |
| 狀態燈 R／G／B | |

### 🔌 ③ 接線任務

**Step 1：搖桿＋2 顆伺服**

1. 搖桿 X、Y 接兩隻 A 腳（限制卡 B：兩顆旋鈕；X 那顆的兩側腳依決策題 3 決定）；兩顆伺服接好。
2. ✅ 檢查：工具 1（搖桿數值）、伺服測試器。

**Step 2：吸附鈕＋繼電器（電磁鐵）**

1. 吸附鈕配 10kΩ；繼電器負載燈接在你選的接點。
2. ✅ 檢查：工具 1。

**Step 3：PIR＋狀態燈**

1. 接好 PIR（限制卡 C：兩顆）；狀態燈各串 220Ω。
2. ✅ 檢查：工具 1、接線驗證器。

### 💻 ④ 套用程式模板 T39

```cpp
// ===== 程式模板 T39：搖桿操作台 =====
#include <Servo.h>

// ----- 腳位區（沒接的填 -1）-----
int joyX = A0, joyY = A1;           // 搖桿 X、Y（或兩顆旋鈕）
int pinRotate = ?;                  // 旋轉伺服（X 軸控制）
int pinLift   = ?;                  // 升降伺服（Y 軸控制）
int btnGrab   = ?;                  // 吸附鈕
int pinRelay  = ?;                  // 電磁鐵繼電器線圈
int pinPIR    = ?;                  // 作業區 PIR ①
int pinPIR2   = -1;                 // 作業區 PIR ②
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈（只用全亮全滅顏色）

// ----- 參數區 -----
int deadband = 150;                 // 搖桿死區
int stepDeg  = 1;                   // 推一下每輪轉幾度（越大越快）
int rotMin = 0,   rotMax = 180;     // 旋轉範圍
int liftMin = 20, liftMax = 160;    // 升降範圍
int resumeDelay = 3000;             // 人離開後幾毫秒恢復
bool magnetOnNC = false;            // 電磁鐵負載接在 NC 時改成 true
int colorEmpty[3]  = { 0, 1, 0 };   // 空鉤（綠）
int colorLoaded[3] = { 0, 0, 1 };   // 吊著貨（藍）
int colorStop[3]   = { 1, 0, 0 };   // 有人停止（紅）

// ----- 功能開關 -----
bool useMagnet = false;             // Step 2：電磁鐵
bool useSafety = false;             // Step 3：PIR 安全連鎖＋狀態燈

// ===== 以下不用改 =====
Servo rotateArm, liftHook;
int angleR = 90, angleL = 90;
bool magnetOn = false, stopped = false;
int lastGrab = LOW;
unsigned long lastPerson = 0;

void setColor(int c[]) {
  if (pinR >= 0) digitalWrite(pinR, c[0] ? HIGH : LOW);
  if (pinG >= 0) digitalWrite(pinG, c[1] ? HIGH : LOW);
  if (pinB >= 0) digitalWrite(pinB, c[2] ? HIGH : LOW);
}

void magnet(bool on) {
  bool coil = magnetOnNC ? !on : on;
  digitalWrite(pinRelay, coil ? HIGH : LOW);
}

int direction(int value) {                                 // 搖桿推到哪一邊：-1、0、+1
  if (value > 512 + deadband) return 1;
  if (value < 512 - deadband) return -1;
  return 0;
}

void setup() {
  rotateArm.attach(pinRotate);
  liftHook.attach(pinLift);
  rotateArm.write(angleR);
  liftHook.write(angleL);
  pinMode(btnGrab, INPUT);
  pinMode(pinRelay, OUTPUT);
  magnet(false);
  pinMode(pinPIR, INPUT);
  if (pinPIR2 >= 0) pinMode(pinPIR2, INPUT);
  if (pinR >= 0) pinMode(pinR, OUTPUT);
  if (pinG >= 0) pinMode(pinG, OUTPUT);
  if (pinB >= 0) pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // ---- 安全連鎖：作業區有人 ----
  if (useSafety) {
    bool seen = digitalRead(pinPIR) == HIGH || (pinPIR2 >= 0 && digitalRead(pinPIR2) == HIGH);
    if (seen) lastPerson = millis();
    bool personNear = (lastPerson > 0) && (millis() - lastPerson < (unsigned long)resumeDelay);
    if (personNear != stopped) {
      stopped = personNear;
      Serial.println(stopped ? "PERSON IN ZONE - CRANE STOPPED" : "Zone clear - resumed");
    }
    if (stopped) {
      setColor(colorStop);
      magnet(magnetOn);                                    // 電磁鐵維持原狀，不放開貨物
      delay(50);
      return;
    }
    setColor(magnetOn ? colorLoaded : colorEmpty);
  }

  // ---- 搖桿控制：推多久就轉多少，放開就停 ----
  angleR = constrain(angleR + direction(analogRead(joyX)) * stepDeg, rotMin, rotMax);
  angleL = constrain(angleL + direction(analogRead(joyY)) * stepDeg, liftMin, liftMax);
  rotateArm.write(angleR);
  liftHook.write(angleL);

  // ---- 電磁鐵 ----
  if (useMagnet) {
    int now = digitalRead(btnGrab);
    if (now == HIGH && lastGrab == LOW) {
      delay(50);
      magnetOn = !magnetOn;
      Serial.println(magnetOn ? "MAGNET ON - load lifted" : "MAGNET OFF - load released");
    }
    lastGrab = now;
    magnet(magnetOn);
  }
  delay(15);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表；限制卡 C 的 `pinPIR2` |
| 參數區 | `magnetOnNC` 依失效安全表 |
| 功能開關 | Step 2 → `useMagnet = true`；Step 3 → `useSafety = true` |

> 💡 限制卡 C 的「斷電吸住型」：不通電時吸住，所以「吸住」要接在**不通電時接通**的那個接點——這就是 NC 的意思。`magnetOnNC = true` 後，程式送出的訊號會反過來，操作方式和 A、B 完全一樣。

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「搖桿往前推，吊鉤卻往下」 | （提示：技巧 4） |
| 「操作員說左右、前後控制的東西反了」 | （提示：技巧 4） |
| 「作業區入口要一顆『天車吊貨中』警示燈，電磁鐵吸住時亮」 | （提示：電磁鐵的負載接在哪個接點？警示燈可以並聯嗎？） |

### 💼 情境思考

1. 為什麼有人進入作業區時，天車「停止動作」但電磁鐵「保持吸住」？如果反過來（一律全部關掉）會發生什麼事？
2. 限制卡 A 的一般電磁鐵接 NO：Arduino 斷電時貨物會掉下來；限制卡 C 的斷電吸住型就不會。為什麼不是每台天車都用斷電吸住型？（提示：成本、維修時怎麼放下貨物）

### ✅ 關卡 3 驗收

- [ ] 搖桿左右推，吊臂旋轉；前後推，吊鉤升降；放開停在原位（限制卡 B：方向符合操作員習慣）
- [ ] 吸附鈕能切換電磁鐵，失效安全表與接點一致
- [ ] PIR 偵測到人時所有動作停止、狀態燈轉紅，電磁鐵維持原狀；人離開 3 秒後恢復（限制卡 C：兩顆 PIR 都有效）
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 4：輸送線控制盤（模板 T40）

⭐⭐⭐⭐ **難度**：HOA ＋ 急停 ＋ PIR 安全區 ＋ 搖桿手動 ＋ L293D 馬達 ＋ 繼電器警示燈 ＋ LCD → **期末專題的預演**

### 📖 情境

食品工廠的輸送線控制盤，整合本週所有觀念：

| 模式／狀況 | 輸送帶行為 |
| --- | --- |
| **AUTO** | 入口超音波偵測到產品 → 輸送帶前進；最後一次偵測後 3 秒停止 |
| **HAND** | 操作員推搖桿 Y 軸：往前推前進、往後推倒退，推越多越快（排除卡料用） |
| **OFF** | 停止，任何操作無效 |
| **安全區有人**（PIR） | AUTO：立刻停止；HAND：限速 30% |
| **急停** | 最高優先：立刻停止，**再按一次急停才解除** |

- 輸送帶運轉時，**警示旋轉燈**（繼電器控制，負載 LED 模擬）亮起
- 有人進入安全區、或按下急停時，蜂鳴器提醒
- LCD 第 0 行顯示模式，第 1 行顯示速度與方向或警告訊息

### 🎯 本關配線學習目標

- 優先權流程圖與 L293D 接法選擇
- 平行版 LCD 與 I2C 版的取捨（期末專題預演）

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：要倒退、要調速；I2C 版 LCD |
| **B** | 這條輸送帶**不需要倒退**（往後推搖桿＝停止）；I2C 版 LCD |
| **C** | 要倒退、要調速；工廠只有**平行版 LCD**；客戶要求「**控制盤故障時，警示燈一定要亮**」 |

### ✏️ ② 接線前決策題

**先畫優先權流程圖**（請補完）：

```
開始
 │
 ├─ 急停中？ ── 是 → ______________________
 │
 ├─ 模式是 OFF？ ── 是 → __________________
 │
 ├─ 安全區有人？ ── 是 → AUTO：________  HAND：________
 │
 ├─ 模式是 HAND？ ── 是 → 依搖桿 ___________
 │
 └─ 模式是 AUTO → 依超音波 ________________
```

1. 依你的限制卡，L293D 用哪種接法？佔幾隻腳？（第 10 週）
2. 有蜂鳴器，EN 要避開哪兩隻腳？
3. 填失效安全表：警示燈接 NO 還是 NC？`beaconOnNC` 填什麼？
4. 限制卡 C：平行版 LCD＋其他元件，一共幾隻？放得下嗎？

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| L293D EN／IN1／IN2 | |
| HC-SR04 Trig／Echo | |
| 警示燈繼電器線圈 | |
| PIR（安全區） | |
| 模式鈕、急停鈕 | |
| 蜂鳴器 | |
| 搖桿 Y 軸 | |
| LCD | |
| **合計／還剩幾隻** | |

### 🔌 ③ 接線任務

**Step 1：L293D＋馬達＋HC-SR04＋模式鈕＋LCD**

1. 依接法接 L293D；HC-SR04 裝在入口；模式鈕配 10kΩ；接好 LCD。
2. ✅ 檢查：馬達測試器、超音波測距檢查器、LCD 測試器。

**Step 2：搖桿＋繼電器（警示燈）**

1. 搖桿 Y 軸接 A 腳；警示燈接在你選的接點。
2. ✅ 檢查：工具 1。

**Step 3：PIR＋急停鈕＋蜂鳴器**

1. 接好 PIR、急停鈕（配 10kΩ）、蜂鳴器。

### 💻 ④ 套用程式模板 T40

```cpp
// ===== 程式模板 T40：輸送線控制盤 =====
#include <LiquidCrystal.h>
#include <Adafruit_LiquidCrystal.h>

// ----- 腳位區（沒接的填 -1；L293D 固定接 5V／GND 的腳也填 -1）-----
int pinEN = ?, pinIN1 = ?, pinIN2 = ?;   // L293D
int pinTrig = ?, pinEcho = ?;            // 入口 HC-SR04
int pinRelay  = ?;                       // 警示燈繼電器線圈
int pinPIR    = ?;                       // 安全區 PIR
int btnMode   = ?;                       // 模式鈕（AUTO → HAND → OFF）
int btnEStop  = ?;                       // 急停鈕
int pinBuzzer = ?;                       // 蜂鳴器
int joyY = A0;                           // 搖桿 Y 軸（或旋鈕）
bool useI2C = true;                      // 用平行版改成 false
int lcdRS = -1, lcdE = -1, lcdD4 = -1, lcdD5 = -1, lcdD6 = -1, lcdD7 = -1;   // 平行版才填

// ----- 參數區 -----
float productCM = 20;                    // 入口偵測距離
int autoSpeed = 180;                     // AUTO 速度
int runAfter  = 3000;                    // AUTO：最後一次偵測後再跑幾毫秒
int deadband  = 150;                     // 搖桿死區
int handLimitPct = 30;                   // HAND 時安全區有人的限速 %
bool beaconOnNC = false;                 // 警示燈接在 NC 時改成 true
String lineName = "LINE 1";              // LCD 名稱

// ----- 功能開關 -----
bool useHand   = false;                  // Step 2：HAND 模式＋警示燈
bool useSafety = false;                  // Step 3：PIR、急停、蜂鳴器

// ===== 以下不用改 =====
LiquidCrystal* lcdP = 0;
Adafruit_LiquidCrystal* lcdI = 0;
int mode = 0;                            // 0=AUTO, 1=HAND, 2=OFF
String modeName[3] = { "AUTO", "HAND", "OFF" };
bool eStopped = false;
int lastMode = LOW, lastEStop = LOW, tick = 0;
unsigned long lastProduct = 0;

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

void motor(int dir, int speed) {                         // dir：1 前進、-1 倒退、0 停止
  if (dir == -1 && pinIN2 < 0) dir = 0;                  // 不能倒退的接法：倒退指令一律停止
  if (pinIN1 >= 0) digitalWrite(pinIN1, dir == 1  ? HIGH : LOW);
  if (pinIN2 >= 0) digitalWrite(pinIN2, dir == -1 ? HIGH : LOW);
  if (pinEN  >= 0) analogWrite(pinEN, dir == 0 ? 0 : speed);
}

void belt(int speed) {                                   // 正數前進、負數倒退、0 停止
  int dir = (speed > 0) ? 1 : (speed < 0 ? -1 : 0);
  if (dir == -1 && pinIN2 < 0) speed = 0;
  motor(dir, abs(speed));
  if (useHand) {
    bool on = speed != 0;
    bool coil = beaconOnNC ? !on : on;
    digitalWrite(pinRelay, coil ? HIGH : LOW);           // 運轉時警示燈亮
  }
}

void setup() {
  lcdStart();
  if (pinEN >= 0)  pinMode(pinEN, OUTPUT);
  if (pinIN1 >= 0) pinMode(pinIN1, OUTPUT);
  if (pinIN2 >= 0) pinMode(pinIN2, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinRelay, OUTPUT);
  pinMode(pinPIR, INPUT);
  pinMode(btnMode, INPUT);
  pinMode(btnEStop, INPUT);
  belt(0);
}

void loop() {
  tick++;

  if (pressed(btnMode, lastMode)) {
    mode = (mode + 1) % 3;
    if (mode == 1 && !useHand) mode = 2;                 // Step 1 沒有 HAND
  }
  if (useSafety && pressed(btnEStop, lastEStop)) {
    eStopped = !eStopped;
    if (eStopped) tone(pinBuzzer, 400, 600);
  }

  // ===== ① 急停：最高優先 =====
  if (useSafety && eStopped) {
    belt(0);
    showLine(0, "!! E-STOP !!");
    showLine(1, "PRESS AGAIN=OFF");
    delay(50);
    return;
  }

  // ===== ② OFF =====
  if (mode == 2) {
    belt(0);
    showLine(0, lineName + " OFF");
    showLine(1, "BELT STOPPED");
    delay(50);
    return;
  }

  bool person = useSafety && digitalRead(pinPIR) == HIGH;
  if (person && tick % 20 == 0) tone(pinBuzzer, 1500, 100);

  int speed = 0;
  String info = "";
  if (mode == 1) {
    // ===== ④ HAND：依搖桿 =====
    int y = analogRead(joyY);
    if (y > 512 + deadband)      speed = map(y, 512 + deadband, 1023, 80, 255);
    else if (y < 512 - deadband) speed = -map(y, 512 - deadband, 0, 80, 255);
    if (person) speed = speed * handLimitPct / 100;      // ③ 安全區有人：限速
    info = person ? "PERSON! LIMITED" : "";
  } else {
    // ===== ⑤ AUTO：依超音波 =====
    if (readCM(pinTrig, pinEcho) < productCM) lastProduct = millis();
    bool hasWork = (lastProduct > 0) && (millis() - lastProduct < (unsigned long)runAfter);
    speed = hasWork ? autoSpeed : 0;
    if (person) speed = 0;                               // ③ 安全區有人：停止
    info = person ? "PERSON! STOPPED" : (hasWork ? "" : "WAITING PRODUCT");
  }
  if (speed < 0 && pinIN2 < 0) speed = 0;                // 不能倒退的接法

  belt(speed);
  showLine(0, lineName + " " + modeName[mode]);
  if (info.length() > 0) showLine(1, info);
  else showLine(1, "SPD " + String(abs(speed)) + (speed > 0 ? " FWD" : speed < 0 ? " REV" : " STOP"));
  delay(50);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依接法與腳位規劃表；限制卡 B 的 L293D 固定腳填 `-1`；限制卡 C 的 `useI2C = false`，填 6 隻平行腳 |
| 參數區 | `beaconOnNC` 依失效安全表（限制卡 C：依客戶要求） |
| 功能開關 | Step 2 → `useHand = true`；Step 3 → `useSafety = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「控制盤上要一顆『停止中』綠燈，輸送帶停的時候亮」 | （提示：技巧 2） |
| 「輸送帶裝反了，AUTO 時往入口方向跑」 | （提示：第 10 週） |
| 「搖桿往前推卻倒退」 | （提示：技巧 4） |

### 💼 情境思考

1. 安全區有人時，為什麼 AUTO 要「停止」，HAND 卻只「限速」？反過來設計會有什麼問題？
2. 這個控制盤的判斷順序，就是一份「現場作業規則」。如果要把它寫成給操作員看的 SOP，你會怎麼寫？請寫出 3～5 條。

### ✅ 關卡 4 驗收

- [ ] 優先權流程圖完整，與程式行為一致
- [ ] AUTO：產品到入口時前進，最後一次偵測後 3 秒停止
- [ ] HAND：搖桿前推前進、後推倒退（限制卡 B：後推停止），推越多越快
- [ ] OFF：任何操作都無法啟動；安全區有人：AUTO 停止、HAND 限速
- [ ] 急停最優先，再按一次才解除；警示燈與失效安全表一致
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：每一關都是**新的客戶情境**，從 T37～T40 挑一個模板，**先填失效安全表、畫優先權**，再規劃腳位、接線，並填寫**參數修改表**。**不寫新程式**。
>
> | 關卡 | 情境 | 套用模板 | 配線重點 |
> | --- | --- | --- | --- |
> | Lv.1 | 倉庫走道感應燈 | T37 | 兩顆 PIR、一個接點帶多盞燈 |
> | Lv.2 | 咖啡機保溫電源 | T38 | 加熱器的失效安全、反向溫控 |
> | Lv.3 | 停車場監視器雲台 | T39 | 不用電磁鐵、方向改線 |
> | Lv.4 | 自助洗車輸送線 | T40 | 不倒退接法、NO＋NC 雙燈 |
> | Lv.5 | 機場行李輸送控制盤 | T40 | **平行版放不下** |
> | Lv.6 | 期末專題預演提案 | 自選 | 自己設計情境 |
>
> 📌 **作答規則**：先填「失效安全表」「腳位規劃表」「參數修改表」，再接線；接好後先用工具檢查，最後才上傳程式。Lv.3 之後每關要附**配線設計單**。

---

### 🟢 Lv.1 倉庫走道感應燈

**情境**：倉庫長走道兩端各一顆 PIR，任一端有人就開燈，最後一次偵測後 **20 秒**熄滅。走道有 **3 盞燈**一起亮。倉庫沒有窗戶，**不需要光敏電阻**。系統壞掉時燈要關（省電）。

**元件**：2 顆 PIR＋1 顆繼電器＋3 顆單色 LED（模擬燈）

**套用模板**：T37

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `pinPIR2` | `-1` | | |
| `pinLDR` | `-1` | | |
| `holdTime` | 10000 | | |
| `loadOnNC` | `false` | | |
| `areaName` | `"Stairs"` | | |
| 功能開關 | | | |

**情境思考**：堆高機在走道上移動，PIR 偵測得到嗎？（提示：PIR 偵測的是什麼？）

---

### 🟢 Lv.2 咖啡機保溫電源

**情境**：辦公室咖啡機的保溫盤，**低於 70°C 開始加熱、高於 80°C 停止**；茶水間有人（PIR）才保溫，**沒人 30 秒**後關掉。HAND 模式強制加熱（清洗除垢時）、OFF 模式完全斷電。模式燈綠／黃／紅。

**元件**：繼電器＋按鈕＋PIR＋TMP36＋RGB＋蜂鳴器

**套用模板**：T38

**你要想的問題**：

- 加熱器的失效安全：系統壞掉時應該停在哪裡？接 NO 還是 NC？
- 這是「太冷才開」，要改哪個參數？`onLine`、`offLine` 各填多少？

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `heatMode` | `false` | | |
| `onLine`、`offLine` | 26.0、24.0 | | |
| `emptyDelay` | 15000 | | |
| `loadOnNC` | `false` | | |
| `deviceName` | `"AC"` | | |
| 功能開關 | | | |

**情境思考**：有人把模式切到 HAND 去清洗，洗完忘了切回 AUTO，保溫盤整晚加熱。你會建議加什麼「防呆」？

---

### 🟡 Lv.3 停車場監視器雲台

**情境**：停車場警衛室用搖桿控制監視器雲台：左右推轉動水平角度（**0～180°**）、前後推調整俯仰角度（**30～120°**）。不需要電磁鐵。有人靠近警衛室（PIR）時雲台停止，狀態燈紅，3 秒後恢復。

**元件**：搖桿（或兩顆旋鈕）＋2 顆伺服＋PIR＋RGB

**套用模板**：T39

**限制條件**：雲台裝在天花板上（**倒吊**），所以搖桿往上推時鏡頭會往下轉——**不改程式**要怎麼修？

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `btnGrab`、`pinRelay` | `?` | | |
| `rotMin`、`rotMax` | 0、180 | | |
| `liftMin`、`liftMax` | 20、160 | | |
| `stepDeg` | 1 | | 監視器要轉快一點還是慢一點？ |
| `useMagnet` | `false` | | |
| 功能開關 | | | |

**情境思考**：「有人靠近警衛室時雲台停止」這個需求合理嗎？從保全的角度，你會建議客戶改成什麼？

---

### 🟡 Lv.4 自助洗車輸送線

**情境**：自助洗車場的車輛輸送帶，**只會往前**（不需要倒退）。AUTO：入口超音波偵測到車（**50 cm** 內）就前進，最後一次偵測後 **5 秒**停止。HAND：推搖桿前進（往後推＝停止）。急停、安全區 PIR 同關卡 4。控制盤上「**運轉中**」黃燈和「**可進車**」綠燈永遠一亮一滅。

**元件**：L293D＋馬達＋HC-SR04＋繼電器＋2 顆單色 LED＋PIR＋2 顆按鈕＋蜂鳴器＋搖桿＋I2C LCD

**套用模板**：T40

**你要想的問題**：

- 不需要倒退，L293D 用哪種接法？哪幾個腳位填 `-1`？
- 「運轉中」「可進車」兩顆燈要怎麼接，才不多佔腳？

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `pinEN`、`pinIN1`、`pinIN2` | `?` | | |
| `productCM` | 20 | | |
| `runAfter` | 3000 | | |
| `beaconOnNC` | `false` | | |
| `lineName` | `"LINE 1"` | | |
| 功能開關 | | | |

**情境思考**：車子停在輸送帶上、駕駛還沒下車。洗車場要怎麼確保「人一定在安全的位置」？只靠 PIR 夠嗎？

---

### 🔴 Lv.5 機場行李輸送控制盤

**情境**：機場行李輸送帶控制盤，功能和關卡 4 相同（要倒退、要調速）。客戶要求「控制盤故障時，警示燈一定要亮」。

**元件**：L293D＋馬達＋HC-SR04＋繼電器＋PIR＋2 顆按鈕＋蜂鳴器＋搖桿＋LCD

**套用模板**：T40

**限制條件**：**Pin 12、13** 已被行李掃描器佔用；客戶的倉庫裡只有**平行版 LCD**，希望優先使用。

**Step 0：腳位預算（本關重點）**

| 項目 | 平行版 | I2C 版 |
| --- | --- | --- |
| L293D（接法 D） | 3 | 3 |
| HC-SR04 | 2 | 2 |
| 繼電器、PIR、2 顆按鈕、蜂鳴器 | 5 | 5 |
| LCD | 6 | 2（A4、A5） |
| 搖桿 | 1（A 腳） | 1（A 腳） |
| **合計** | | |
| **可用腳位（扣掉 Pin 12、13）** | | |
| **放得下嗎？** | | |

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 |
| --- | --- | --- |
| `useI2C` | `true` | |
| `beaconOnNC` | `false` | |
| `lineName` | `"LINE 1"` | |
| 腳位區 | | |
| 功能開關 | | |

**情境思考**：

1. 附上你的腳位預算表。平行版放不下時，你要怎麼向客戶說明「為什麼不能用庫存的 LCD」？
2. 行李輸送帶旁常有地勤人員。急停按鈕應該裝幾顆、裝在哪裡？多顆急停按鈕要怎麼接？（提示：第 13 週技巧 3）

---

### 🏆 Lv.6 BOSS 關：期末專題預演提案

**情境**：下週就要開始期末專題。請用本週的模板，為你們小組預計的期末場域，先做一個「自動／手動」子系統的預演。

**最低規格**：

- 至少 1 顆繼電器，附**失效安全表**
- 至少 2 種運作模式（HOA、營業／打烊、布防／撤防……）與**優先權流程圖**
- 至少修改模板參數區的 **5 個參數**
- 至少用到一個本週配線技巧（NO＋NC 雙負載、一個接點多負載、方向改線）
- 自己設計 1 題「配線變化挑戰」並附解答

**繳交內容**：情境說明、失效安全表、優先權流程圖、配線設計單、參數修改表、配線變化挑戰與解答、Tinkercad 連結

---

### 📝 配線設計單

| 項目 | 內容 |
| --- | --- |
| 情境名稱 | |
| 套用模板 | T37／T38／T39／T40 |
| 失效安全表 | 每個繼電器的負載、NO／NC、壞掉時的狀態、理由 |
| 優先權流程圖 | 從最高安全條件到一般功能 |
| 元件清單 | 元件名稱 × 數量（含 220Ω、10kΩ） |
| 腳位分配表 | 元件 → 腳位（標註 `~` 腳、借用 A 腳、LCD 版本） |
| 遇到的問題 | 接線過程中出過什麼錯？怎麼修正的？ |

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 繼電器有切換，但負載燈不亮 | COM 是否接 5V；燈是否接在 NO／NC 再經 220Ω 到 GND | 對照接法表 |
| 負載「該開的時候關、該關的時候開」 | 負載接的接點和 `loadOnNC` 是否一致 | 改線或改參數，讓兩者一致 |
| PIR 一直顯示 MOTION | 模擬中物體是否一直在範圍內移動 | 把物體拖出範圍 |
| 搖桿放開時伺服還在慢慢動 | 死區太小 | 加大 `deadband` |
| 搖桿方向相反 | 該軸的 5V、GND（或旋鈕兩側腳） | 對調（技巧 4） |
| 不能倒退 | L293D 是否用了接法 A 或 B | 需要倒退改接法 C 或 D |
| 平行版 LCD 沒字 | RS、E、DB4～DB7；RW 是否接 GND | 對照第 11 週 |
| 程式無法上傳 | 腳位區是否還有 `?` 沒填 | 用到的填腳位，沒接的填 `-1` |

---

## 成果繳交與評分

### 繳交清單

每個關卡、每個練習題各自建立一個 Tinkercad 檔案，命名格式：`W14-關卡或Lv編號-限制卡-學號`。

| 項目 | 繳交內容 |
| --- | --- |
| 關卡 1～4 | Tinkercad 連結＋決策題答案＋失效安全表＋腳位規劃表＋配線變化挑戰說明＋同學驗收簽名 |
| Lv.1～Lv.2 | Tinkercad 連結＋參數修改表＋情境思考 |
| Lv.3～Lv.5 | ＋配線設計單 |
| Lv.6 | 期末專題預演提案 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 失效安全與腳位規劃 | 25% | NO／NC 選擇有理由、與參數一致；符合限制卡 |
| 電路正確性 | 25% | 繼電器兩端電路、PIR、搖桿正確；每個 Step 通過工具檢查 |
| 配線變化挑戰 | 20% | 正確運用 NO＋NC 雙負載、多負載、方向改線 |
| 優先權與參數修改表 | 15% | 流程圖與程式行為一致，參數正確 |
| 情境思考 | 15% | 💼 管理面向論述 |

> 🎯 **最低完成標準**：關卡 1～4 全部完成（含配線變化挑戰）、Lv.1～Lv.3 完成。

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 40 分鐘 | 認識繼電器（NO／NC）、PIR、搖桿、HOA、四個新技巧、綜合測試器 |
| 第 1 節後段 | 關卡 1 |
| 第 2 節 | 關卡 2、關卡 3 |
| 第 3 節前段 | 關卡 4 |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，其餘帶回家；分組討論期末專題方向 |

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 繼電器（Relay） | 用電控制的開關，控制端與負載端電氣隔離 |
| COM／NO／NC | 共同點／常開（通電才接通）／常閉（不通電時接通） |
| 失效安全（Fail-safe） | 系統故障時自動回到最安全的狀態 |
| 失效安全表 | 逐一記錄每個負載接 NO 或 NC、壞掉時的狀態與理由 |
| PIR 人體感測器 | 偵測人體紅外線變化，有人移動輸出 HIGH |
| 類比搖桿 | 兩顆旋鈕合體，X、Y 兩軸各 0～1023 |
| 死區（Deadband） | 搖桿中間一段範圍當作「沒推」 |
| HOA | 手動（Hand）／停止（Off）／自動（Auto）三段模式 |
| 上鎖掛牌（LOTO） | 設備保養時鎖住電源開關並掛牌，避免被誤啟動 |

---

## 反思與延伸討論

1. 本週同一個樓梯燈，限制卡 A 接 NO、限制卡 B 接 NC。這說明「安全」的定義會隨情境改變。請舉一個生活中的例子，說明同一個設備在不同場合的失效安全選擇不同。
2. 技巧 1 提到「負載換到 NC，要同步改參數」。如果現場人員只改線、忘了改參數，會發生什麼事？在真實工廠，硬體變更和軟體設定要怎麼一起管理？
3. HOA 的 HAND 模式讓人可以「蓋過」自動控制。人什麼時候比感測器可靠？什麼時候反而比較危險？

---

## 本週檢核清單

- [ ] 能正確接上繼電器（控制端、負載端），並用測試器確認 NO、NC
- [ ] 能填失效安全表，並讓接點與 `loadOnNC` 一致
- [ ] 能用一顆繼電器同時帶 NO、NC 兩個相反負載
- [ ] 能接上 PIR、搖桿，並用改線修正搖桿方向
- [ ] 能畫出 HOA 與安全條件的優先權流程圖
- [ ] 完成關卡 1～4，並取得同學驗收簽名
- [ ] 完成練習題 Lv.1～Lv.3，挑戰 Lv.4～Lv.6

---

## 下週預告

第 15～16 週是**期末整合大專題**，不再學新元件。各組會抽到**小組客戶限制卡**，用整學期的 40 個模板與配線技巧，為客戶建置一套完整的智慧場域系統——第 16 週還有老師現場出題的配線變化挑戰。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 13 週](week-13.md) | [下一週：第 15 週 — 期末整合大專題（上）➡](week-15.md)
