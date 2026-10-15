[⬅ 回目錄](../README.md) | [⬅ 上一週：第 14 週](week-14.md) | [下一週：第 16 週 — 期末整合大專題（下）➡](week-16.md)

---

# 第 15 週：期末整合大專題（上）

## 主題專案：兩個完整範例 → 你的智慧場域提案

> 🎯 **本週定位**：從這週開始的兩週是**期末整合大專題**，不再學新元件。你們的顧問團隊要把 14 週學過的元件與觀念，整合成一套能「上線」的智慧場域系統。
>
> 本週先帶大家完整看過**兩個期末整合範例**——一個是製造／物流場域、一個是零售服務場域。兩個範例用的元件不同，但**設計方法完全一樣**：
>
> 1. **需求分析** → 2. **拆成子系統** → 3. **腳位總規劃** → 4. **子系統逐一建置、各自驗收** → 5. **整合與優先權** → 6. **驗收測試**
>
> 看完範例後，各組套用同一套方法，提出自己的期末專題，並在本週完成**規劃書與子系統建置**；第 16 週完成整合、驗收測試與成果展演。
>
> 和整學期一樣，**重點不在寫程式**，而在**配線規劃與情境判斷**：
>
> - **跟做範例時**，每人依座號抽一張**跟做限制卡**，同一個範例會配出不同的電路（整合程式只改腳位區、參數區）。
> - **各組的期末專題**，每組抽一張**小組客戶限制卡**，影響整個系統的腳位總表；每個子系統都要從 **T1～T40** 找一個最接近的模板套用。
> - 第 16 週整合完成後，老師會到各組**現場出配線變化挑戰**，只能改線應變。
>
> 🏁 **本週成果**：
> - 個人：跟做一個範例（A 或 B），依跟做限制卡完成 4 個子系統的逐步建置
> - 各組：期末專題規劃書（含限制卡因應）＋至少 3 個子系統各自獨立的 Tinkercad 檔案

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [期末專題規格](#期末專題規格)
- [🃏 小組客戶限制卡](#-小組客戶限制卡)
- [設計方法：從需求到系統的六個步驟](#設計方法從需求到系統的六個步驟)
- [範例 A：智慧冷鏈出貨中心](#範例-a智慧冷鏈出貨中心)
- [範例 B：智慧便利商店](#範例-b智慧便利商店)
- [兩個範例比一比](#兩個範例比一比)
- [你的期末專題：本週里程碑](#你的期末專題本週里程碑)
- [小組分工建議](#小組分工建議)
- [配線除錯站](#配線除錯站)
- [本週繳交與評分](#本週繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 14 週技能總盤點

| 類別 | 元件 | 學習週次 |
| --- | --- | --- |
| **人工輸入** | 按鈕、旋鈕、4×4 鍵盤、搖桿 | 2、4、12、14 |
| **環境感測** | 光敏電阻、TMP36、HC-SR04、PIR | 5、6、9、14 |
| **聲光輸出** | LED、RGB、蜂鳴器 | 1、3、5 |
| **動作輸出** | 伺服、直流馬達（L293D）、繼電器 | 7、10、14 |
| **資訊顯示** | LCD（平行、I2C）、七段顯示器（74HC595） | 11、13 |

| 類別 | 觀念 | 學習週次 |
| --- | --- | --- |
| **配線規劃** | `~` 腳可用表、借 A 腳、L293D 接法、I2C、鍵盤 3 行、74HC595 串接 | 3～13 |
| **改線應變** | 兩燈共腳、反相接法、共用訊號線、整組對調、馬達並聯、按鈕並聯、NO＋NC 雙負載 | 5～14 |
| **程式模板** | T1～T40（只改腳位區、參數區、功能開關） | 4～14 |
| **安全設計** | 鎖定與復歸、安全連鎖、急停、失效安全（NO／NC） | 6、7、10、14 |
| **模式與優先權** | 模組化、優先權順序、HOA 三段模式 | 8、14 |
| **管理介面** | 看板設計、操作流程設計、KPI | 11、12、13 |

### 這兩週要練的能力

| 能力 | 在職場上像什麼 |
| --- | --- |
| 把模糊的需求拆成可執行的子系統 | 專案管理的工作分解結構（WBS） |
| 在有限資源下做取捨 | 預算與產能規劃 |
| 定義「做到什麼程度才算完成」 | 驗收標準、品質規格 |
| 用客戶聽得懂的話說明價值 | 顧問提案、業務簡報 |

---

## 期末專題規格

每組 3～4 人，選一個真實場域，設計並實作一套智慧系統。**最低規格**如下：

| 項目 | 最低要求 |
| --- | --- |
| 元件種類 | 至少 **8 種**不同元件，涵蓋「學習地圖」五大類中的**至少 4 類** |
| 子系統 | 至少 **3 個**子系統，每個都能**單獨運作、單獨驗收** |
| 安全設計 | 至少 **1 個**安全連鎖或急停，並說明優先權 |
| 模式設計 | 至少 **2 種**運作模式（例如 HOA、營業／打烊、布防／撤防） |
| 現場看板 | LCD 或七段顯示器至少一種，顯示至少 **1 個管理指標（KPI）** |
| 腳位規劃 | 完整腳位總表，並運用至少 **2 種**省腳位技巧，且符合**小組客戶限制卡** |
| 改線應變 | 至少用到 **2 種**「改線就能解決」的技巧（見第 12 週總整理、第 13～14 週新技巧） |
| 模板對照 | 每個子系統註明套用或改編自哪一個模板（T1～T40） |
| 失效安全 | 每個繼電器、馬達都填**失效安全表** |
| 繳交 | 規劃書、每個子系統的獨立檔案、整合檔案、QA 測試報告、展演 |

> 💡 **元件不是越多越好**。評分看的是「每個元件都有理由存在」，以及「元件之間的配合是否合理」。

---

## 🃏 小組客戶限制卡

每一組依**組別編號**抽一張小組限制卡。限制卡會影響你們期末專題的**腳位總表與配線設計**，規劃書第 8 項要說明怎麼因應。

| 組別編號 | 抽到 |
| --- | --- |
| 1、5、9 | 限制卡 甲 |
| 2、6、10 | 限制卡 乙 |
| 3、7、11 | 限制卡 丙 |
| 4、8、12 | 限制卡 丁 |

| 限制卡 | 客戶條件 | 主要考驗 |
| --- | --- | --- |
| **甲：擴充預留** | 客戶明年要擴充，腳位總表必須保留**至少 3 隻空腳**（其中 1 隻是 `~` 腳） | 省腳技巧的組合運用 |
| **乙：舊設備相容** | **Pin 2、3** 被客戶既有的收費系統佔用；如果要用 LCD，只能用**平行版** | 平行版 LCD 的腳位取捨 |
| **丙：消防與保險** | 至少 **1 個**繼電器負載在系統故障時必須**保持運作**（接 NC）；**急停必須兩處都能按** | 失效安全表、按鈕並聯 |
| **丁：雙現場** | 至少 **2 個**指示（燈號或數字）要在「現場」和「辦公室」**兩處同步顯示**；74HC595 最多只能用 **2 顆** | 共腳、一個 Q 接兩顆燈、NO 並聯 |

> 💡 限制卡不是刁難——真實的顧問案一定有「客戶現場的限制」。能在限制下做出合理設計，才是顧問的價值。

---

## 設計方法：從需求到系統的六個步驟

兩個範例都照這六步走，你們的專題也一樣：

| 步驟 | 要回答的問題 | 產出 | 時間 |
| --- | --- | --- | --- |
| ① 需求分析 | 誰會用？要解決什麼問題？出錯會怎樣？ | 需求表 | 第 15 週 |
| ② 子系統拆解 | 可以拆成哪幾個能獨立運作的功能？ | 系統架構圖 | 第 15 週 |
| ③ 腳位總規劃 | 全部元件要幾隻腳？用哪些省腳技巧？ | 腳位總表 | 第 15 週 |
| ④ 子系統建置 | 每個子系統單獨接線、單獨驗收 | 子系統檔案 | 第 15 週 |
| ⑤ 整合與優先權 | 功能衝突時誰優先？ | 優先權流程圖、整合檔案 | 第 16 週 |
| ⑥ 驗收測試 | 在什麼情況下會出錯？ | QA 測試報告 | 第 16 週 |

> 📌 **最重要的規則**（第 8 週期中專題學過）：步驟 ④ 的每個子系統都要**照步驟 ③ 的腳位總表接線**。這樣第 16 週整合時，只要把電路「合併」，腳位完全不用改。
>
> 📌 **步驟 ④ 的捷徑**：每個子系統先從 **T1～T40** 找最接近的模板（見第 12 週「程式模板索引」與第 13、14 週），套用後只改腳位區、參數區。範例 A、B 的「對應模板」欄就是示範。

---

## 範例 A：智慧冷鏈出貨中心

### 🃏 跟做限制卡（範例 A）

跟做範例 A 的同學，依座號尾數抽卡：1、4、7 → A；2、5、8 → B；3、6、9、0 → C。

| 限制卡 | 現場條件 | 要改哪裡 |
| --- | --- | --- |
| **A** | 依範例的腳位總表 | — |
| **B** | 客戶評估後改成「省電優先」：壓縮機負載改接 **NO**；出貨區有**兩條並排輸送帶**同步運轉 | 接線＋參數 `compOnNC` |
| **C** | **Pin 2、3** 的插座鬆脫不能用；輸送帶旁也要一顆「安全區有人」燈，和看板 Q7 同步 | 接線＋腳位區 |

### ① 需求分析

> 📖 **客戶**：一家冷凍食品公司的出貨中心。冷藏庫裡的產品放上輸送帶，送往貨車裝載區。
>
> **客戶的痛點**：
> - 冷藏庫壓縮機曾在半夜故障，第二天才發現，整批產品報廢
> - 溫度異常時，輸送帶還繼續出貨，把可能變質的產品送到客戶手上
> - 輸送帶旁常有人員走動，曾發生夾傷事故
> - 主管不知道每天出了多少貨，要等月底對帳

| 使用者 | 需要什麼 |
| --- | --- |
| 冷藏庫管理員 | 隨時知道溫度，異常時被通知 |
| 輸送帶操作員 | 平常自動運轉，排除卡料時能手動操作，緊急時能停下來 |
| 出貨主管 | 一眼看到今天出了幾箱 |
| 安全管理員 | 有人靠近輸送帶時自動停止 |

### ② 子系統拆解

```
┌─────────────────── 智慧冷鏈出貨中心 ───────────────────┐
│                                                        │
│  S1 冷藏安全          S2 出貨輸送          S3 現場看板   │
│  ・TMP36              ・L293D＋馬達         ・74HC595×3  │
│  ・繼電器（壓縮機）    ・HC-SR04（計數）     ・七段×2     │
│  ・蜂鳴器             ・                    ・狀態燈×8   │
│  ・復歸鈕                                   ・I2C LCD    │
│                                                        │
│  S4 總控與安全                                          │
│  ・模式鈕（HOA）  ・急停鈕  ・PIR（輸送帶安全區）         │
└────────────────────────────────────────────────────────┘
```

| 子系統 | 功能 | 可以單獨驗收嗎？ |
| --- | --- | --- |
| S1 冷藏安全 | 溫度控制壓縮機（兩條線），超溫鎖定警報，降溫後按復歸解除 | ✅ |
| S2 出貨輸送 | 偵測到箱子就啟動輸送帶，計算出貨數 | ✅ |
| S3 現場看板 | 七段顯示出貨數、8 顆狀態燈、LCD 顯示溫度與模式 | ✅ |
| S4 總控與安全 | HOA 模式、急停、安全區有人停止 | 需搭配 S2 |

### ③ 腳位總規劃

**先算需求：**

| 元件 | 需要 | 省腳技巧 |
| --- | --- | --- |
| 繼電器、蜂鳴器、復歸鈕 | 3 | — |
| L293D（單向、調速 → 接法 B） | 1（`~` 腳） | **L293D 接法 B**（省 2 隻） |
| HC-SR04 | 2 | — |
| 2 位七段＋8 顆狀態燈（含 RGB） | 24 → **3** | **74HC595 串接**（省 21 隻） |
| PIR、模式鈕、急停鈕 | 3 | — |
| TMP36 | 1（A 腳） | — |
| LCD | 6 → **2**（A4、A5） | **I2C 版**（省 4 隻） |

**腳位總表（範例答案）：**

| 腳位 | 元件 | 子系統 | 說明 |
| --- | --- | --- | --- |
| 2 | 壓縮機繼電器線圈 | S1 | 負載接 **NC**（理由見下方討論） |
| 3 | 蜂鳴器 | S1 | 蜂鳴器借走 3、11 的調速 |
| 4 | 溫度復歸鈕 | S1 | |
| ~5 | L293D EN1 | S2 | 唯一需要 `~` 的元件，避開 3、11 |
| 6、7 | HC-SR04 Trig、Echo | S2 | |
| 8 | PIR（安全區） | S4 | |
| 9 | 模式鈕 | S4 | |
| 10 | 急停鈕 | S4 | |
| 11、12、13 | 74HC595 資料、時脈、鎖存 | S3 | 3 顆串接 |
| A0 | TMP36 | S1 | |
| A4、A5 | I2C LCD | S3 | |
| A1～A3 | （空） | — | 保留擴充 |

**74HC595 輸出分配：**

| 顆 | Q0 | Q1 | Q2 | Q3 | Q4 | Q5 | Q6 | Q7 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 第 1 顆 | 十位 a | b | c | d | e | f | g | dp |
| 第 2 顆 | 個位 a | b | c | d | e | f | g | dp |
| 第 3 顆 | RGB 紅 | RGB 綠 | RGB 藍 | AUTO 燈 | HAND 燈 | OFF 燈 | 壓縮機運轉燈 | 安全區有人燈 |

> 💡 **RGB 接在 74HC595 上**：它只需要全亮全滅的顏色（綠、黃、紅），不必佔用 Arduino 的 3 隻腳。這是把第 7 週「全亮全滅顏色不需要 `~` 腳」和第 13 週 74HC595 結合起來的做法。

> 🤔 **壓縮機繼電器接 NO 還是 NC？** 接 NO：Arduino 故障時壓縮機停止 → 產品可能退冰。接 NC：Arduino 故障時壓縮機一直運轉 → 產品不會壞，但可能結冰、耗電。**範例選 NC**，理由是「產品報廢的損失遠大於多花的電費」。程式參數 `compOnNC = true`，會自動把訊號反過來（和第 14 週的 `loadOnNC` 一樣）。你同意嗎？

### ④ 子系統建置（逐步打開功能開關）

| 子系統 | 對應模板 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- | --- |
| S1 冷藏安全 | T11 三段鎖定＋T38 繼電器 | TMP36、繼電器（負載 LED 接 NC）、蜂鳴器、復歸鈕 | 高於 5°C 壓縮機運轉、低於 3°C 停止；超過 8°C 鎖定警報，降溫後按復歸解除 | `useCold = true` |
| S2 出貨輸送 | T23 緩啟動輸送帶 | L293D（接法 B）＋馬達、HC-SR04 | 箱子到入口（15 cm 內）輸送帶啟動，最後一箱後 3 秒停止；Serial Monitor 顯示出貨數 | `useConveyor = true` |
| S3 現場看板 | T35 計數器＋T26 看板 | 3 顆 74HC595（串接）、2 顆七段、8 顆燈（含 RGB）、I2C LCD | 七段顯示出貨數；狀態燈與 LCD 顯示溫度、模式 | `useBoard = true` |
| S4 總控與安全 | T40 輸送線控制盤 | 模式鈕、急停鈕、PIR | HOA 切換、急停、安全區有人停止 | `useControl = true` |

> 📌 **建置建議**：每完成一個子系統就**另存一個 Tinkercad 檔案**（`FINAL-A-S1`、`FINAL-A-S1S2`……），出問題時可以退回上一版。

### ⑤ 優先權

```
每一輪依序檢查（越上面越優先）：
  ① 急停中？             → 輸送帶停止；壓縮機照常運作（冷藏不能中斷）
  ② 溫度警報鎖定中？     → 輸送帶停止（品質連鎖：不出貨可能變質的產品）
  ③ 模式 OFF？           → 輸送帶停止
  ④ 安全區有人？         → AUTO 停止；HAND 限速 30%
  ⑤ 模式 HAND？          → 輸送帶以固定速度運轉（排除卡料）
  ⑥ 模式 AUTO？          → 依 HC-SR04 偵測箱子運轉
```

> 🤔 **為什麼急停不停壓縮機？** 急停是為了「人」的安全，針對的是會夾傷人的輸送帶；壓縮機不會傷人，停了反而讓產品報廢。**安全設計要針對真正的危險來源。**

### 整合程式

```cpp
// ===== 期末範例 A：智慧冷鏈出貨中心 =====
#include <Adafruit_LiquidCrystal.h>

// ----- 腳位區（照腳位總表；依跟做限制卡調整）-----
int pinRelay  = 2;                  // S1 壓縮機繼電器線圈
int pinBuzzer = 3;                  // S1 蜂鳴器
int btnReset  = 4;                  // S1 溫度警報復歸鈕
int pinEN     = 5;                  // S2 L293D EN1（接法 B：IN1 接 5V、IN2 接 GND）
int pinTrig = 6, pinEcho = 7;       // S2 HC-SR04
int pinPIR    = 8;                  // S4 輸送帶安全區 PIR
int btnMode   = 9;                  // S4 模式鈕（AUTO → HAND → OFF）
int btnEStop  = 10;                 // S4 急停鈕
int dataPin = 11, clockPin = 12, latchPin = 13;   // S3 74HC595 ×3
int pinTemp   = A0;                 // S1 TMP36
// I2C LCD：SDA → A4、SCL → A5

// ----- 參數區：客戶規格 -----
bool compOnNC = true;               // 壓縮機負載接在 NC 填 true；接在 NO 填 false
float compOn = 5.0, compOff = 3.0;  // 壓縮機：高於 5°C 運轉、低於 3°C 停止
float warnLine = 6.0, dangerLine = 8.0;
float boxCM = 15;                   // 箱子到入口的距離
int beltSpeed = 200;                // 輸送帶速度
int runAfter  = 3000;               // 最後一箱後再跑幾毫秒
int handLimitPct = 30;              // HAND 時安全區有人的限速
String siteName = "COLD";           // LCD 名稱（建議 4 格）

// ----- 功能開關：逐一打開 -----
bool useCold     = false;           // S1
bool useConveyor = false;           // S2
bool useBoard    = false;           // S3
bool useControl  = false;           // S4

// ===== 以下不用改 =====
Adafruit_LiquidCrystal lcd(0);
byte font[10] = {
  0b00111111, 0b00000110, 0b01011011, 0b01001111, 0b01100110,
  0b01101101, 0b01111101, 0b00000111, 0b01111111, 0b01101111
};
int mode = 0;                       // 0=AUTO, 1=HAND, 2=OFF
String modeName[3] = { "AUTO", "HAND", "OFF" };
bool eStopped = false, tempLocked = false, compRunning = false, wasBox = false;
int shipped = 0, tick = 0;
unsigned long lastBox = 0;
int lastReset = LOW, lastMode = LOW, lastEStop = LOW;

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
  int now = digitalRead(pin);
  bool edge = (now == HIGH && last == LOW);
  last = now;
  if (edge) delay(50);
  return edge;
}

void sendOut(byte chip[], int n) {
  digitalWrite(latchPin, LOW);
  for (int i = n - 1; i >= 0; i--) shiftOut(dataPin, clockPin, MSBFIRST, chip[i]);
  digitalWrite(latchPin, HIGH);
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void compressor(bool on) {
  bool coil = compOnNC ? !on : on;                 // 接 NC 時，訊號反過來
  digitalWrite(pinRelay, coil ? HIGH : LOW);
}

void setup() {
  pinMode(pinRelay, OUTPUT);
  pinMode(btnReset, INPUT);
  pinMode(pinEN, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinPIR, INPUT);
  pinMode(btnMode, INPUT);
  pinMode(btnEStop, INPUT);
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  if (useBoard) { lcd.begin(16, 2); lcd.setBacklight(1); }
  Serial.begin(9600);
}

void loop() {
  tick++;
  float t = readC(pinTemp);
  int tempLevel = 0;

  // ================= S1 冷藏安全 =================
  if (useCold) {
    if (t > compOn)  compRunning = true;
    if (t < compOff) compRunning = false;
    compressor(compRunning);

    if (t >= dangerLine)    tempLevel = 2;
    else if (t >= warnLine) tempLevel = 1;

    if (tempLevel == 2 && !tempLocked) {
      tempLocked = true;
      Serial.println("COLD ROOM ALARM - LOCKED");
    }
    if (pressed(btnReset, lastReset) && tempLocked) {
      if (tempLevel < 2) { tempLocked = false; noTone(pinBuzzer); Serial.println("Alarm reset"); }
      else Serial.println("Cannot reset: still too warm");
    }
    if (tempLocked) tone(pinBuzzer, 2000);
    else if (tempLevel == 1 && tick % 40 == 0) tone(pinBuzzer, 1000, 80);
  }

  // ================= S4 總控：按鈕 =================
  bool person = false;
  if (useControl) {
    if (pressed(btnMode, lastMode)) {
      mode = (mode + 1) % 3;
      Serial.print("MODE -> ");
      Serial.println(modeName[mode]);
    }
    if (pressed(btnEStop, lastEStop)) {
      eStopped = !eStopped;
      Serial.println(eStopped ? "!!! E-STOP !!!" : "E-STOP released");
      if (eStopped) tone(pinBuzzer, 400, 600);
    }
    person = digitalRead(pinPIR) == HIGH;
  }

  // ================= S2 出貨輸送（依優先權） =================
  int speed = 0;
  String status = "";
  if (useConveyor) {
    bool box = readCM(pinTrig, pinEcho) < boxCM;
    if (box && !wasBox) {
      shipped++;
      Serial.print("Box shipped: ");
      Serial.println(shipped);
    }
    if (box) lastBox = millis();
    wasBox = box;
    bool hasWork = (lastBox > 0) && (millis() - lastBox < (unsigned long)runAfter);

    if (eStopped)          { speed = 0; status = "!! E-STOP !!"; }          // ①
    else if (tempLocked)   { speed = 0; status = "QUALITY HOLD"; }          // ②
    else if (mode == 2)    { speed = 0; status = "MODE OFF"; }              // ③
    else if (person) {                                                     // ④
      speed = (mode == 1) ? beltSpeed * handLimitPct / 100 : 0;
      status = (mode == 1) ? "PERSON! SLOW" : "PERSON! STOP";
    }
    else if (mode == 1)    { speed = beltSpeed; status = "HAND RUN"; }      // ⑤
    else                   { speed = hasWork ? beltSpeed : 0;               // ⑥
                             status = hasWork ? "AUTO RUN" : "AUTO WAIT"; }
    analogWrite(pinEN, speed);
  }

  // ================= S3 現場看板 =================
  if (useBoard) {
    bool red    = eStopped || tempLocked;
    bool yellow = !red && (tempLevel == 1 || person);
    byte status8 = 0;
    if (red)         status8 |= 0b00000001;            // Q0 紅
    else if (yellow) status8 |= 0b00000011;            // Q0+Q1 黃
    else             status8 |= 0b00000010;            // Q1 綠
    status8 |= (1 << (3 + mode));                      // Q3 AUTO／Q4 HAND／Q5 OFF
    if (compRunning) status8 |= 0b01000000;            // Q6 壓縮機
    if (person)      status8 |= 0b10000000;            // Q7 安全區有人

    byte chip[3] = { font[(shipped / 10) % 10], font[shipped % 10], status8 };
    sendOut(chip, 3);

    if (tick % 5 == 0) {
      showLine(0, siteName + " " + String(t, 1) + "C " + (tempLocked ? "ALARM" : tempLevel == 1 ? "WARN" : "OK"));
      showLine(1, modeName[mode] + " " + status);
    }
  }
  delay(40);
}
```

**跟做時要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 限制卡 C：`pinRelay`、`pinBuzzer` 改到空著的 A 腳（例如 `A1`、`A2`） |
| 參數區 | 限制卡 B：`compOnNC = false` |
| 功能開關 | 依 S1 → S2 → S3 → S4 逐一打開，每開一個就驗收一次 |

### ⑥ 驗收測試案例（範例）

| 編號 | 測試情境 | 預期結果 |
| --- | --- | --- |
| A1 | 溫度 4°C → 拉到 6°C → 拉回 2°C | 壓縮機在 5°C 以上運轉、3°C 以下停止 |
| A2 | 溫度拉到 9°C 後拉回 4°C，不按復歸 | 警報與品質鎖定持續；按復歸後才解除 |
| A3 | 溫度 9°C 時按復歸 | 無法解除 |
| A4 | AUTO 模式，箱子連續通過 3 次 | 出貨數 +3，輸送帶在最後一箱後 3 秒停止 |
| A5 | AUTO 運轉中，PIR 偵測到人 | 輸送帶立刻停止，「安全區有人燈」亮 |
| A6 | HAND 模式，PIR 偵測到人 | 輸送帶降速，不停止 |
| A7 | 按急停 | 輸送帶停止、壓縮機**繼續**運作；再按一次解除 |
| A8 | 溫度鎖定時切到 HAND | 輸送帶仍不動（品質連鎖優先於模式） |
| A9 | 出貨數到 99 之後 | （由你決定應該怎樣，並寫進規格） |

### 範例 A 情境思考

1. 範例選擇壓縮機接 NC。請從「產品報廢成本」「電費」「設備壽命」三個角度，評估這個選擇。
2. 品質連鎖（溫度異常時停止出貨）是「技術手段」。冷鏈業者還需要什麼「管理手段」，才能確保有問題的產品不會流出去？
3. 💼 七段顯示器顯示的出貨數，和 ERP 系統裡的出貨單對不起來時，你會先懷疑哪裡？（提示：感測器誤判、重複計數、人工搬運）

---

## 範例 B：智慧便利商店

### 🃏 跟做限制卡（範例 B）

跟做範例 B 的同學，依座號尾數抽卡：1、4、7 → A；2、5、8 → B；3、6、9、0 → C。

| 限制卡 | 現場條件 | 要改哪裡 |
| --- | --- | --- |
| **A** | 依範例的腳位總表 | — |
| **B** | 店長認為「系統壞掉時招牌也要亮」：招牌燈改接 **NC**；門口是**雙開自動門**（兩顆伺服） | 接線＋參數 `signOnNC` |
| **C** | 店長密碼改成 **2468**；商品改成你們自己設計的 4 項；**店員休息室**也要一顆「冷藏異常」燈，和看板 Q5 同步 | 參數＋接線 |

### ① 需求分析

> 📖 **客戶**：社區型便利商店，想升級成「少人化」門市。
>
> **客戶的痛點**：
> - 尖峰時段只有一位店員，結帳大排長龍
> - 冷藏櫃溫度異常常常沒人注意到
> - 招牌燈有時白天忘了關、晚上忘了開
> - 店長想知道每天來客數和營業額，但要等收銀機日結

| 使用者 | 需要什麼 |
| --- | --- |
| 顧客 | 自動門、自助結帳、清楚的價格提示 |
| 店員 | 冷藏櫃異常時被提醒 |
| 店長 | 來客數、營業額；營業／打烊切換 |

### ② 子系統拆解

```
┌─────────────────── 智慧便利商店 ───────────────────┐
│                                                    │
│  S1 自動門與來客      S2 自助結帳        S3 環境節能  │
│  ・PIR               ・4×4 鍵盤（3 行）   ・光敏電阻   │
│  ・伺服（自動門）     ・I2C LCD           ・繼電器     │
│  ・蜂鳴器（迎賓）                         （招牌燈）  │
│                                          ・TMP36      │
│                                          （冷藏櫃）   │
│  S4 門市看板                                        │
│  ・74HC595×3  ・七段×2（今日來客）  ・狀態燈×8（含 RGB）│
└────────────────────────────────────────────────────┘
```

| 子系統 | 功能 | 可以單獨驗收嗎？ |
| --- | --- | --- |
| S1 自動門與來客 | 有人靠近開門、人離開 2 秒後關門；每次開門計一位來客並播放迎賓聲；打烊時門不開 | ✅ |
| S2 自助結帳 | 輸入商品編號加入購物車、`#` 結帳、`*` 清除；店長密碼切換營業／打烊 | ✅ |
| S3 環境節能 | 天黑且營業中開招牌燈；冷藏櫃超過 7°C 提醒 | ✅ |
| S4 門市看板 | 七段顯示今日來客；狀態燈顯示營業、招牌、冷藏、門 | ✅ |

### ③ 腳位總規劃

**先算需求：**

| 元件 | 需要 | 省腳技巧 |
| --- | --- | --- |
| 鍵盤（只需 0～9、`*`、`#`） | 8 → **7** | **鍵盤只接 3 行**（省 1 隻） |
| LCD | 6 → **2**（A4、A5） | **I2C 版**（省 4 隻） |
| 2 位七段＋8 顆狀態燈（含 RGB） | 24 → **3** | **74HC595 串接**（省 21 隻） |
| PIR、伺服、蜂鳴器、繼電器 | 4 | — |
| 光敏電阻、TMP36 | 2（A 腳） | — |
| **一般腳合計** | **14** | Pin 2～13 只有 12 隻 → **借 A2、A3** |

**腳位總表（範例答案）：**

| 腳位 | 元件 | 子系統 | 說明 |
| --- | --- | --- | --- |
| 2、3、4、5 | 鍵盤 R1～R4 | S2 | |
| 6、7、8 | 鍵盤 C1～C3 | S2 | C4 不接 |
| 9、10、11 | 74HC595 資料、時脈、鎖存 | S4 | 3 顆串接 |
| 12 | 自動門伺服 | S1 | |
| 13 | PIR（門口） | S1 | |
| A0 | 光敏電阻 | S3 | 搭配 10kΩ |
| A1 | TMP36（冷藏櫃） | S3 | |
| A2 | 招牌燈繼電器線圈 | S3 | **借 A 腳當數位輸出** |
| A3 | 蜂鳴器 | S1 | **借 A 腳當數位輸出** |
| A4、A5 | I2C LCD | S2 | |

> 💡 這個系統**沒有任何元件需要調光或調速**，所以伺服、蜂鳴器借走 `~` 腳完全沒有影響——鍵盤、74HC595 都可以放心接在 3、9、10、11 上。

**74HC595 輸出分配：**

| 顆 | Q0～Q7 |
| --- | --- |
| 第 1 顆 | 十位數七段（a～dp） |
| 第 2 顆 | 個位數七段（a～dp） |
| 第 3 顆 | Q0 RGB 紅、Q1 RGB 綠、Q2 RGB 藍、Q3 招牌燈指示、Q4 門開啟、Q5 冷藏異常、Q6 購物車有商品、Q7 （保留） |

> 🤔 **招牌燈繼電器接 NO 還是 NC？** 接 NO：系統故障時招牌熄滅（省電，但晚上看起來像沒開）。接 NC：系統故障時招牌一直亮（耗電，但不影響生意）。**範例選 NO**（`signOnNC = false`），理由是招牌燈不涉及安全，故障時以省電為優先。你同意嗎？

### ④ 子系統建置（逐步打開功能開關）

| 子系統 | 對應模板 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- | --- |
| S1 自動門與來客 | T37 感應＋T15 自動門 | PIR、伺服、蜂鳴器 | 有人靠近門開並播迎賓聲，人離開 2 秒後關門；Serial Monitor 顯示來客數 | `useDoor = true` |
| S2 自助結帳 | T30 選號付款出貨 | 鍵盤（3 行）、I2C LCD | 輸入 11～14＋`#` 加入商品，空白按 `#` 結帳，`*` 清除；`9999#` 切換營業／打烊 | `usePOS = true` |
| S3 環境節能 | T6 光線控制＋T9 溫度監測 | 光敏電阻、繼電器（招牌燈）、TMP36 | 天黑且營業中招牌亮；冷藏櫃超過 7°C 提醒 | `useEnv = true` |
| S4 門市看板 | T35 計數器 | 3 顆 74HC595、2 顆七段、8 顆燈（含 RGB） | 七段顯示今日來客；狀態燈顯示各項狀態 | `useBoard = true` |

### ⑤ 優先權

```
  ① 打烊中？           → 自動門不開、招牌燈關、結帳只接受店長密碼
  ② 冷藏櫃異常？       → 提醒店員（不影響其他功能）
  ③ 門口有人？         → 開門；門開著時繼續偵測，人在門口就不關（防夾）
  ④ 結帳操作           → 依鍵盤輸入
```

> 🤔 範例 B 的優先權比範例 A 簡單——因為零售門市的設備大多不會傷人。**優先權的複雜度，應該和場域的風險成正比。**

### 整合程式

```cpp
// ===== 期末範例 B：智慧便利商店 =====
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>
#include <Servo.h>

// ----- 腳位區（照腳位總表；依跟做限制卡調整）-----
byte rowPins[4] = { 2, 3, 4, 5 };   // S2 鍵盤 R1~R4
byte colPins[3] = { 6, 7, 8 };      // S2 鍵盤 C1~C3（C4 不接）
int dataPin = 9, clockPin = 10, latchPin = 11;   // S4 74HC595 ×3
int pinDoor   = 12;                 // S1 自動門伺服（雙開門：兩顆橘線共用）
int pinPIR    = 13;                 // S1 門口 PIR
int pinLDR    = A0;                 // S3 光敏電阻
int pinTemp   = A1;                 // S3 冷藏櫃 TMP36
int pinRelay  = A2;                 // S3 招牌燈繼電器
int pinBuzzer = A3;                 // S1 蜂鳴器
// I2C LCD：SDA → A4、SCL → A5

// ----- 參數區：客戶規格 -----
String names[4] = { "WATER", "MILK", "BREAD", "RICE BALL" };
int prices[4]   = { 20, 35, 30, 32 };   // 商品編號 11~14
String managerCode = "9999";        // 店長密碼：切換營業／打烊
bool signOnNC  = false;             // 招牌燈負載接在 NC 填 true；接在 NO 填 false
int darkLine   = 300;               // 光線低於這個數字開招牌燈
float fridgeMax = 7.0;              // 冷藏櫃上限
int doorHold   = 2000;              // 人離開後幾毫秒關門
int doorClosed = 0, doorOpen = 90;  // 自動門角度（依角度記錄表）

// ----- 功能開關：逐一打開 -----
bool useDoor  = false;              // S1
bool usePOS   = false;              // S2
bool useEnv   = false;              // S3
bool useBoard = false;              // S4

// ===== 以下不用改 =====
char keys[4][3] = { {'1','2','3'}, {'4','5','6'}, {'7','8','9'}, {'*','0','#'} };   // 4 列 × 3 行
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 3);
Adafruit_LiquidCrystal lcd(0);
Servo door;
byte font[10] = {
  0b00111111, 0b00000110, 0b01011011, 0b01001111, 0b01100110,
  0b01101101, 0b01111101, 0b00000111, 0b01111111, 0b01101111
};
bool storeOpen = true, isOpen = false, signOn = false, fridgeAlarm = false;
unsigned long lastSeen = 0, messageUntil = 0;
int visitors = 0, cartItems = 0, cartTotal = 0, tick = 0;
long salesToday = 0;
String input = "", message = "";

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void flash(String msg) {                                  // 在 LCD 第 0 行顯示 2 秒訊息
  message = msg;
  messageUntil = millis() + 2000;
}

void sendOut(byte chip[], int n) {
  digitalWrite(latchPin, LOW);
  for (int i = n - 1; i >= 0; i--) shiftOut(dataPin, clockPin, MSBFIRST, chip[i]);
  digitalWrite(latchPin, HIGH);
}

float readC(int pin) {
  return (analogRead(pin) * 5.0 / 1023.0 - 0.5) * 100.0;
}

void sign(bool on) {
  bool coil = signOnNC ? !on : on;                        // 接 NC 時，訊號反過來
  digitalWrite(pinRelay, coil ? HIGH : LOW);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(pinPIR, INPUT);
  pinMode(pinRelay, OUTPUT);
  pinMode(pinBuzzer, OUTPUT);
  door.attach(pinDoor);
  door.write(doorClosed);
  if (usePOS) { lcd.begin(16, 2); lcd.setBacklight(1); }
  Serial.begin(9600);
}

void loop() {
  tick++;

  // ================= S1 自動門與來客 =================
  if (useDoor) {
    if (digitalRead(pinPIR) == HIGH) lastSeen = millis();
    bool wantOpen = storeOpen && (lastSeen > 0) && (millis() - lastSeen < (unsigned long)doorHold);
    if (wantOpen && !isOpen) {
      isOpen = true;
      door.write(doorOpen);
      visitors++;
      tone(pinBuzzer, 880, 250);
      Serial.print("Visitor #");
      Serial.println(visitors);
    } else if (!wantOpen && isOpen) {
      isOpen = false;
      door.write(doorClosed);
    }
  }

  // ================= S2 自助結帳 =================
  if (usePOS) {
    char k = keypad.getKey();
    if (k >= '0' && k <= '9' && input.length() < 4) {
      input += k;
    } else if (k == '*') {
      input = "";
      cartItems = 0;
      cartTotal = 0;
      flash("CART CLEARED");
    } else if (k == '#') {
      if (input == managerCode) {                         // 店長：營業／打烊
        storeOpen = !storeOpen;
        flash(storeOpen ? "STORE OPEN" : "STORE CLOSED");
        Serial.print(storeOpen ? "OPEN" : "CLOSED  Sales today: $");
        if (!storeOpen) Serial.println(salesToday); else Serial.println();
      } else if (!storeOpen) {
        flash("SORRY, CLOSED");
      } else if (input == "") {                           // 空白按 # ＝ 結帳
        if (cartItems > 0) {
          salesToday += cartTotal;
          flash("PAID $" + String(cartTotal) + " THANKS");
          tone(pinBuzzer, 1319, 300);
          Serial.print("Sale $");
          Serial.println(cartTotal);
          cartItems = 0;
          cartTotal = 0;
        }
      } else {                                            // 加入商品
        int code = input.toInt();
        if (code >= 11 && code <= 14) {
          cartItems++;
          cartTotal += prices[code - 11];
          flash("+" + names[code - 11] + " $" + String(prices[code - 11]));
          tone(pinBuzzer, 2000, 40);
        } else {
          flash("NO ITEM " + input);
        }
      }
      input = "";
    }

    if (tick % 5 == 0) {
      if (millis() < messageUntil)  showLine(0, message);
      else if (input.length() > 0)  showLine(0, "CODE: " + input);
      else if (!storeOpen)          showLine(0, "CLOSED");
      else                          showLine(0, "ITEMS " + String(cartItems) + "  $" + String(cartTotal));
      showLine(1, "#=PAY  *=CLEAR");
    }
  }

  // ================= S3 環境節能 =================
  if (useEnv) {
    signOn = storeOpen && analogRead(pinLDR) < darkLine;
    sign(signOn);

    bool alarm = readC(pinTemp) > fridgeMax;
    if (alarm && !fridgeAlarm) Serial.println("FRIDGE TOO WARM!");
    fridgeAlarm = alarm;
    if (fridgeAlarm && tick % 50 == 0) tone(pinBuzzer, 1500, 150);
  }

  // ================= S4 門市看板 =================
  if (useBoard) {
    byte status8 = 0;
    if (!storeOpen)       status8 |= 0b00000001;          // 紅：打烊
    else if (fridgeAlarm) status8 |= 0b00000011;          // 黃：冷藏異常
    else                  status8 |= 0b00000010;          // 綠：營業正常
    if (signOn)        status8 |= 0b00001000;             // Q3 招牌燈
    if (isOpen)        status8 |= 0b00010000;             // Q4 門開啟
    if (fridgeAlarm)   status8 |= 0b00100000;             // Q5 冷藏異常
    if (cartItems > 0) status8 |= 0b01000000;             // Q6 購物車有商品

    byte chip[3] = { font[(visitors / 10) % 10], font[visitors % 10], status8 };
    sendOut(chip, 3);
  }
  delay(30);
}
```

> 💡 **鍵盤只接 3 行**：範例 B 直接把鍵盤寫成「4 列 × 3 行」（`keys[4][3]`、`colPins[3]`，最後的參數寫 `4, 3`），效果和第 12 週模板的 `numCols = 3` 完全一樣——第 4 行不佔任何腳。

**跟做時要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 限制卡 B：雙開門的第二顆伺服橘線接在 `pinDoor` 同一隻腳（腳位區不用改） |
| 參數區 | 限制卡 B：`signOnNC = true`、門的角度依角度記錄表；限制卡 C：`managerCode`、`names[]`、`prices[]` |
| 功能開關 | 依 S1 → S2 → S3 → S4 逐一打開，每開一個就驗收一次 |

### ⑥ 驗收測試案例（範例）

| 編號 | 測試情境 | 預期結果 |
| --- | --- | --- |
| B1 | PIR 偵測到人 | 門打開、迎賓聲、來客 +1；人離開 2 秒後關門 |
| B2 | 人一直在門口走動 | 門保持開著，來客**不重複**增加 |
| B3 | 輸入 12＋`#`、14＋`#`，再空白按 `#` | LCD 依序顯示加入的商品，結帳金額 $67 |
| B4 | 輸入 99＋`#` | 顯示 `NO ITEM 99`，購物車不變 |
| B5 | 購物車有商品時按 `*` | 購物車清空 |
| B6 | 輸入 `9999#` | 切換為打烊：門不開、招牌熄、結帳不受理 |
| B7 | 白天營業、晚上營業、晚上打烊 | 招牌燈分別為：滅、亮、滅 |
| B8 | 冷藏櫃溫度拉到 8°C | 狀態燈轉黃、冷藏異常燈亮、定時提醒 |
| B9 | 來客數到 99 之後 | （由你決定應該怎樣，並寫進規格） |

### 範例 B 情境思考

1. 自助結帳靠顧客「誠實輸入商品編號」。真實的無人商店怎麼防止漏刷？這是技術問題還是管理問題？
2. 範例 B 用 PIR 計算來客數，但店員進出也會被計入。你會怎麼改善這個 KPI 的準確度？
3. 💼 範例 B 可以算出「來客數」和「營業額」。這兩個數字相除得到什麼指標？店長可以拿它做什麼決策？

---

## 兩個範例比一比

| 比較項目 | 範例 A：冷鏈出貨中心 | 範例 B：便利商店 |
| --- | --- | --- |
| 場域類型 | 製造／物流 | 零售／服務 |
| 主要風險 | 人員受傷、產品變質 | 顧客體驗、營運效率 |
| 元件種類 | 13 種 | 12 種 |
| 用到的省腳技巧 | L293D 接法 B、74HC595 串接、I2C LCD | 鍵盤 3 行、74HC595 串接、I2C LCD、借 A 腳 |
| `~` 腳需求 | 1 隻（輸送帶調速） | 0 隻 |
| 模式設計 | HOA 三段 | 營業／打烊 |
| 安全設計 | 急停、安全區、品質連鎖 | 門口防夾、冷藏提醒 |
| 失效安全選擇 | 壓縮機接 NC | 招牌燈接 NO |
| 看板 KPI | 今日出貨數 | 今日來客數 |
| 優先權複雜度 | 6 層 | 4 層 |
| 跟做限制卡考的技巧 | NO／NC＋參數、馬達並聯、借 A 腳、一個 Q 接兩顆燈 | NO／NC＋參數、伺服共用訊號線、一個 Q 接兩顆燈 |

> 💡 **兩個範例告訴我們**：同一套設計方法，套用在不同場域，會得出完全不同的元件組合、腳位分配和安全設計。**沒有標準答案，只有「針對這個客戶最合理的答案」**。

---

## 你的期末專題：本週里程碑

### F0 選題（第 1 節前段）

每組選一個方向：

| 選項 | 說明 |
| --- | --- |
| 改編範例 A 或 B | 換一個相近場域（例如：範例 A → 醫院藥品冷藏、範例 B → 自助洗衣店），重新做需求分析與規格 |
| 延伸前 14 週的 BOSS 關提案 | 把你們某一週的 Lv.6 提案擴大成期末規模 |
| 全新自訂 | 自己找一個真實場域（例如：智慧停車場、智慧教室、社區長照站、農場溫室、物流揀貨中心） |

### F1 規劃書（第 1 節後段～第 2 節前段）✅ 老師審查

規劃書包含以下九項，**審查通過後才能開始接線**：

| 項目 | 內容 | 參考範例的哪一段 |
| --- | --- | --- |
| 1. 需求分析 | 客戶、痛點、使用者角色與需求 | ① 需求分析 |
| 2. 系統架構圖 | 子系統（至少 3 個）與各自的元件 | ② 子系統拆解 |
| 3. 腳位總表 | Pin 2～13、A0～A5 逐一列出；標示省腳技巧 | ③ 腳位總規劃 |
| 4. 失效安全分析 | 每個繼電器、馬達的 NO／NC 或停止策略 | ③ 的 🤔 討論 |
| 5. 優先權流程圖 | 從最高安全條件到一般功能 | ⑤ 優先權 |
| 6. 看板設計 | LCD 版面草圖或七段＋狀態燈分配 | ③ 的 74HC595 輸出分配 |
| 7. 驗收測試案例 | 至少 8 個測試情境（含極端情況、限制卡專屬測試） | ⑥ 驗收測試 |
| 8. 限制卡因應 | 抽到哪張小組限制卡？腳位總表、配線怎麼因應？ | 🃏 小組客戶限制卡 |
| 9. 模板與配線技巧清單 | 每個子系統對應的模板；用到哪些「改線就能解決」的技巧 | ④ 的「對應模板」欄 |

### F2 子系統建置（第 2 節後段～第 3 節）

- 每個子系統**照腳位總表接線**，單獨驗收後各自存檔：`FINAL-組別-S1`、`FINAL-組別-S2`……
- 每個子系統套用 T1～T40 中最接近的模板，或改編範例 A、B 的整合程式，**在檔案開頭註明來源與修改的參數**
- 每個子系統完成後，**自己出一題配線變化挑戰給隔壁組**（只能改線），為第 16 週的現場挑戰暖身
- 每個子系統都要有一段驗收紀錄：接了什麼、測了什麼、結果如何

### 本週結束時應該有

- [ ] 規劃書（已通過審查）
- [ ] 至少 3 個子系統的獨立 Tinkercad 檔案，各自通過驗收
- [ ] 一份「下週整合待辦清單」：還有哪些子系統沒做、整合時可能遇到的問題

---

## 小組分工建議

| 角色 | 職責 | 本週重點 |
| --- | --- | --- |
| **專案經理** | 時程控管、規劃書彙整、對外簡報 | F1 規劃書、整合待辦清單 |
| **系統架構師** | 腳位總表、優先權流程圖、整合程式 | 腳位總表審查、確保每個子系統照表接線 |
| **子系統工程師**（1～2 人） | 各自負責 1～2 個子系統的接線與驗收 | F2 子系統建置 |
| **品保工程師**（可由他人兼任） | 撰寫驗收測試案例、執行測試、記錄問題 | F1 第 7 項、各子系統驗收紀錄 |

> 💡 三人組：專案經理兼品保。每個人都要負責至少一個子系統的接線，**不能只做文書**。

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 子系統單獨正常，合併後失效 | 兩個子系統是否用到同一隻腳 | 對照腳位總表 |
| 需要調速／調光的元件沒效果 | 是否接在被伺服（9、10）或蜂鳴器（3、11）借走的腳 | 查「~ 腳可用表」 |
| 74HC595 上的燈亂亮 | MR、OE、串接線、Q0 位置 | 用第 13 週 74HC595 測試器 |
| 鍵盤某一行沒反應或干擾其他元件 | `colPins` 是否和其他元件共用腳位 | 每隻腳只接一個元件 |
| I2C LCD 沒反應 | SDA→A4、SCL→A5；A4、A5 是否又被其他元件借用 | A4、A5 只給 LCD |
| 繼電器有聲音但負載不動 | 負載端迴路（COM、NO／NC、電阻、GND） | 第 14 週負載端接法 |
| 整合後反應變慢 | 某個子系統用了很長的 `delay()` | 記入待辦清單，第 16 週處理 |
| 程式無法上傳 | 設定區是否有沒填的值；`#include` 是否齊全 | 逐一檢查 |

---

## 本週繳交與評分

### 繳交清單

| 項目 | 格式 |
| --- | --- |
| 範例 A 或 B 跟做成果（依跟做限制卡，4 個子系統全部打開） | Tinkercad 連結（個人） |
| 期末專題規劃書（9 項） | PDF 或線上文件（小組） |
| 子系統檔案（至少 3 個） | Tinkercad 連結（小組） |
| 子系統驗收紀錄 | 文件（小組） |
| 下週整合待辦清單 | 文件（小組） |

### 本週評分（占期末專題 40%）

| 項目 | 比重 | 優秀（A） | 合格（C） |
| --- | --- | --- | --- |
| 範例跟做 | 10% | 依跟做限制卡完成，4 個子系統全部正常，能回答情境思考題 | 至少 3 個子系統正常 |
| 規劃書 | 15% | 九項完整，腳位總表符合小組限制卡、正確運用省腳與改線技巧、失效安全有理由 | 九項都有填寫 |
| 子系統建置 | 10% | 3 個以上子系統各自通過驗收，接線與腳位總表一致 | 至少 2 個子系統可運作 |
| 驗收紀錄與待辦 | 5% | 紀錄清楚，待辦清單具體 | 有紀錄 |

> 第 16 週占期末專題 60%：整合系統、QA 測試報告、成果展演。

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 40 分鐘 | 期末規格說明；全班快速走過範例 A、B 的六個步驟 |
| 第 1 節後段 | 分組選題（F0）＋開始寫規劃書（F1） |
| 第 2 節前段 | 規劃書審查（老師逐組確認腳位總表與優先權） |
| 第 2 節後段～第 3 節 | 子系統建置（F2）；個人課後完成範例跟做 |

> 💡 **教學建議**：範例跟做建議當作**個人課後作業**（程式都已提供，主要是照腳位總表接線並逐一打開功能開關），課堂時間集中在各組的規劃書審查與子系統建置。

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 子系統（Subsystem） | 能單獨運作、單獨驗收的一組功能 |
| 系統架構圖 | 畫出系統有哪些子系統、各自包含哪些元件 |
| 腳位總表 | 把 Pin 2～13、A0～A5 每一隻接什麼全部列出來的表 |
| 品質連鎖 | 品質條件不符時，自動停止後續流程（例如溫度異常停止出貨） |
| 失效安全分析 | 逐一檢查每個設備在系統故障時會停在什麼狀態 |
| 驗收測試案例 | 事先寫好「做什麼操作、應該看到什麼結果」的測試清單 |
| 整合待辦清單 | 記錄還沒完成、或整合時要處理的問題 |
| 小組客戶限制卡 | 期末專題中，客戶現場的條件限制（腳位、設備、法規） |
| 跟做限制卡 | 跟做範例時，每人不同的接線與參數變化 |
| 工作分解結構（WBS） | 把大專案拆成可分工、可追蹤的小工作 |

---

## 反思與延伸討論

1. 範例 A 的急停「不停壓縮機」，範例 B 的招牌燈「故障時熄滅」。兩個決定的背後都在回答「什麼才是真正的風險」。在你的期末專題中，最大的風險是什麼？你的設計怎麼回應它？
2. 兩個範例都先拆子系統、各自驗收，再整合。如果一開始就把所有元件接在一起，會遇到什麼困難？這和專案管理中「分階段交付」的道理有什麼關係？
3. 範例 B 的 18 隻腳全部用完；限制卡甲卻要求保留 3 隻空腳。在真實的產品開發中，資源「剛好用完」常常是整合時出問題的來源。你的規劃書要留多少「擴充空間」才算合理？審查時要怎麼及早發現資源不足？

---

## 本週檢核清單

- [ ] 能說明期末專題的最低規格
- [ ] 能用六步驟方法分析範例 A、B 的設計
- [ ] 完成範例 A 或 B 的跟做，4 個子系統全部正常
- [ ] 能依跟做限制卡，正確修改範例的接線與參數（含 NO／NC 與參數同步）
- [ ] 小組完成規劃書九項（含小組限制卡因應），並通過審查
- [ ] 小組完成至少 3 個子系統的獨立檔案與驗收紀錄
- [ ] 小組完成下週整合待辦清單

---

## 下週預告

第 16 週是學期的最後一週：把本週完成的子系統**整合**成完整系統，老師會到各組出**現場配線變化挑戰**，接著進行**驗收測試與同儕交叉測試**，最後在**期末成果展演**中，以顧問身分向「客戶」提案。請保留好本週的規劃書、所有子系統檔案與整合待辦清單——下週直接在這個基礎上繼續。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 14 週](week-14.md) | [下一週：第 16 週 — 期末整合大專題（下）➡](week-16.md)
