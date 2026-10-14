[⬅ 回目錄](../README.md) | [⬅ 上一週：第 7 週](week-07.md) | [下一週：第 9 週 — 停車場雷達 ➡](week-09.md)

---

# 第 8 週：期中專題 — 智慧場域整合提案

## 主題專案：從客戶需求到可驗收的系統

> 🎯 **本週定位**：本週**沒有新元件、沒有新語法**。你們的顧問團隊要承接一個完整的客戶案，把前七週學過的所有元件整合成一套「智慧場域監控站」。
>
> 和前幾週一樣，**重點不在寫程式**（程式模板都已準備好），而在於顧問真正要做的事：
>
> 1. **需求分析**：客戶要解決什麼問題？誰會使用？
> 2. **規格訂定**：溫度幾度算危險？可以容納幾人？燈多暗要開？
> 3. **腳位規劃**：全學期最多元件的一次，`~` 腳怎麼分？
> 4. **分模組建置**：先把每個功能各自做好、各自驗收
> 5. **整合與安全優先權**：功能衝突時，誰說了算？
> 6. **驗收測試與提案簡報**：證明系統可靠，說服客戶買單
>
> 🏁 **期中成果**：每組繳交 **4 個獨立的 Tinkercad 檔案**（3 個功能模組＋1 個整合系統）、1 份規劃書、1 份驗收測試報告，並進行 5 分鐘提案簡報。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [專題簡報：選擇你的客戶](#專題簡報選擇你的客戶)
- [系統架構：四個模組](#系統架構四個模組)
- [全元件清單與腳位預算](#全元件清單與腳位預算)
- [里程碑 0：需求分析與規劃書](#里程碑-0需求分析與規劃書)
- [里程碑 1：照明模組](#里程碑-1照明模組)
- [里程碑 2：溫度安全模組](#里程碑-2溫度安全模組)
- [里程碑 3：出入管制模組](#里程碑-3出入管制模組)
- [里程碑 4：系統整合與安全優先權](#里程碑-4系統整合與安全優先權)
- [里程碑 5：驗收測試（QA）](#里程碑-5驗收測試qa)
- [里程碑 6：提案簡報](#里程碑-6提案簡報)
- [配線除錯站](#配線除錯站)
- [繳交清單與評分](#繳交清單與評分)
- [時程建議](#時程建議)
- [反思與延伸討論](#反思與延伸討論)
- [📎 教師參考：腳位配置範例](#-教師參考腳位配置範例)

---

## 學習地圖

### 你已經會的（第 1～7 週）

| 週次 | 元件 | 本專題用在哪裡 |
| --- | --- | --- |
| 第 1 週 | 單色 LED、220Ω | 照明燈 |
| 第 2 週 | 按鈕、10kΩ | 進場、離場、復歸、急停 |
| 第 3 週 | RGB LED | 系統總狀態燈 |
| 第 4 週 | 旋鈕 | 現場人員調整照明門檻 |
| 第 5 週 | 蜂鳴器、光敏電阻 | 警報聲、環境光偵測 |
| 第 6 週 | TMP36 | 溫度監控、鎖定與復歸 |
| 第 7 週 | 伺服馬達 | 出入口閘門、安全連鎖 |

### 本週要練的能力（不是新元件，而是新層次）

| 能力 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 模組化 | 先把每個功能各自做好、驗收，再組合 | 專案管理的「工作分解結構（WBS）」 |
| 優先權 | 兩個功能衝突時，誰先誰後 | 緊急事件處理 SOP 的層級 |
| 驗收測試 | 主動找出系統的弱點，而不是等客戶抱怨 | 品管的出貨檢驗、使用者驗收測試（UAT） |
| 提案簡報 | 用客戶聽得懂的語言說明價值與風險 | 顧問提案、業務簡報 |

---

## 專題簡報：選擇你的客戶

> 📖 **情境**：顧問公司七週來接了 20 多個小案子，口碑傳開了。這次有三家客戶同時來詢問一個「整合型」的大案子。每組選一家客戶（或自訂），組成 3～4 人的顧問團隊承接。

三家客戶的需求骨架相同，都由**照明、溫度安全、出入管制、緊急停止**四大功能組成，只是情境和規格不同：

| 功能 | 案例 A：社區便利商店 | 案例 B：冷鏈倉儲中心 | 案例 C：溫室農場參觀園區 |
| --- | --- | --- | --- |
| **照明** | 天黑時招牌燈自動亮，越暗越亮 | 倉庫天黑時照明自動亮 | 陰天或傍晚補光燈自動亮 |
| **溫度安全** | 冷藏櫃溫度監控 | 冷藏區溫度監控 | 溫室溫度監控 |
| ─ 警戒線 | 7°C | 6°C | 32°C |
| ─ 危險線 | 10°C | 8°C | 38°C |
| **出入管制** | 自動門，店內人數上限 | 冷藏區門禁閘門，同時進入人數上限 | 參觀步道閘門，園區人數上限 |
| ─ 人數上限 | 8 人 | 3 人 | 10 人 |
| **緊急停止** | 店員急停鈕 | 倉管急停鈕 | 園區管理員急停鈕 |
| **溫度危險時的連鎖** | 自動門鎖住不開（避免冷氣外洩、等待維修） | 門禁鎖住（避免人員進入異常區域） | 步道閘門鎖住（避免遊客進入過熱溫室） |

> **案例 D：自訂客戶**
> 自己找一個需要這四大功能的場域（例如：自習室、健身房、社區活動中心、實驗室）。須在里程碑 0 提出規格表，經老師同意後才能開始。

> 💡 **思考**：三個案例的溫度危險線一個是 10°C、一個是 38°C，但程式邏輯完全相同——都是「超過危險線就鎖定」。這正是「把規格放在設定區」的好處：**同一套系統，換一組數字就能賣給不同客戶**。

---

## 系統架構：四個模組

```
┌──────────────── 智慧場域監控站 ────────────────┐
│                                                │
│  ┌─ 模組 1 照明 ─┐  ┌─ 模組 2 溫度安全 ─┐       │
│  │ 光敏電阻       │  │ TMP36             │       │
│  │ 旋鈕（門檻）   │  │ 蜂鳴器            │       │
│  │ 照明燈（調光） │  │ RGB 狀態燈        │       │
│  └────────────────┘  │ 復歸鈕            │       │
│                      └───────────────────┘       │
│  ┌─ 模組 3 出入管制 ─┐  ┌─ 模組 4 總控 ─┐       │
│  │ 伺服閘門          │  │ 急停鈕         │       │
│  │ 進場鈕、離場鈕    │  │ 優先權順序     │       │
│  │ （共用蜂鳴器）    │  │ 安全連鎖       │       │
│  └───────────────────┘  └────────────────┘       │
└────────────────────────────────────────────────┘
```

**開發順序（也是繳交順序）**：

| 里程碑 | 內容 | 產出 |
| --- | --- | --- |
| M0 | 需求分析、規格表、腳位規劃 | 規劃書（初稿） |
| M1 | 照明模組 | Tinkercad 檔案 ①（獨立可運作） |
| M2 | 溫度安全模組 | Tinkercad 檔案 ②（獨立可運作） |
| M3 | 出入管制模組 | Tinkercad 檔案 ③（獨立可運作） |
| M4 | 系統整合＋急停＋優先權 | Tinkercad 檔案 ④（完整系統） |
| M5 | 驗收測試 | QA 測試報告 |
| M6 | 提案簡報 | 5 分鐘簡報 |

> 📌 **重要規則**：M1～M3 每個模組都要**照著 M0 的腳位規劃接線**。這樣到了 M4，只需要把三個電路「合併」到同一塊麵包板，腳位完全不用改。如果 M1～M3 各接各的，M4 會非常痛苦。

---

## 全元件清單與腳位預算

### 元件清單

| 元件 | 數量 | 用途 |
| --- | --- | --- |
| Arduino Uno R3 | 1 | 主控板 |
| Breadboard | 1～2 | 電路佈局（元件多，可用兩塊） |
| 光敏電阻＋10kΩ | 1 組 | 環境光偵測 |
| 旋鈕 | 1 | 照明門檻調整 |
| TMP36 | 1 | 溫度監控 |
| 單色 LED＋220Ω | 1 組 | 照明燈 |
| RGB LED＋220Ω × 3 | 1 組 | 系統總狀態燈 |
| 蜂鳴器 | 1 | 警報與提示音 |
| 伺服馬達 | 1 | 出入口閘門 |
| 按鈕＋10kΩ | 4 組 | 進場、離場、復歸、急停 |

### 腳位預算表（M0 必填）

| 類別 | 元件 | 需要幾隻腳 | 需要哪一種腳？ |
| --- | --- | --- | --- |
| 類比輸入 | 光敏電阻、旋鈕、TMP36 | 3 | A 腳 |
| 調光輸出 | 照明燈 | 1 | `~` 腳 |
| 狀態輸出 | RGB | 3 | ？（請判斷） |
| 數位輸出 | 蜂鳴器、伺服 | 2 | 一般腳即可 |
| 數位輸入 | 4 顆按鈕 | 4 | 一般腳即可 |
| **合計** | | **13** | |

> 🤔 **規劃關鍵題**（回顧第 7 週「~ 腳可用表」）：
>
> 1. 系統裡同時有伺服和蜂鳴器，還能調光的 `~` 腳剩哪幾隻？
> 2. 照明燈「越暗越亮」，一定要調光，它該接哪裡？
> 3. RGB 總狀態燈只需要顯示綠、黃、紅、紫這些「全亮全滅組合」的顏色，它需要 `~` 腳嗎？
> 4. 用完之後，還剩幾隻腳可以留給未來擴充？

---

## 里程碑 0：需求分析與規劃書

**這是全專題最重要的一步，也是工管系最擅長的部分。** 規劃書請用以下格式（可以用 Word、Google 文件或簡報）：

### 一、需求分析（半頁）

| 項目 | 你的回答 |
| --- | --- |
| 客戶是誰？目前遇到什麼問題？ | |
| 這套系統的**使用者**有哪些角色？（例如：店員、顧客、維修人員） | |
| 每個角色會「看到」或「操作」系統的哪些部分？ | |
| 如果系統故障，最嚴重的後果是什麼？ | |

### 二、規格表

| 規格項目 | 數值 | 理由或資料來源 |
| --- | --- | --- |
| 照明門檻（旋鈕可調範圍） | | |
| 溫度警戒線 | | |
| 溫度危險線 | | |
| 人數上限 | | |
| 閘門開啟角度／關閉角度 | | 用伺服測試器實測 |
| 閘門開啟時間 | | |

### 三、狀態燈號定義表

RGB 總狀態燈同一時間只能顯示一種顏色，請定義**優先順序**（數字越小越優先）：

| 優先 | 狀況 | 顏色 | 蜂鳴器 |
| --- | --- | --- | --- |
| 1 | 緊急停止中 | 紅色閃爍 | 間歇低音 |
| 2 | 溫度危險（已鎖定） | 紅色恆亮 | 持續高音 |
| 3 | 溫度警戒 | 黃 | 間歇短嗶 |
| 4 | 人數已滿 | 紫 | 有人要進場時發出拒絕聲 |
| 5 | 一切正常 | 綠 | 安靜 |

> 上表是範例，各組可以依客戶情境調整顏色或順序，但必須說明理由。

### 四、腳位規劃表

| 元件 | 腳位 | 是不是 `~` 腳？ | 為什麼這樣接？ |
| --- | --- | --- | --- |
| 光敏電阻 | | — | |
| 旋鈕 | | — | |
| TMP36 | | — | |
| 照明燈 | | | |
| RGB（R / G / B） | | | |
| 蜂鳴器 | | | |
| 伺服 | | | |
| 進場鈕 | | | |
| 離場鈕 | | | |
| 復歸鈕 | | | |
| 急停鈕 | | | |
| **剩餘可擴充腳位** | | | |

### 五、麵包板佈局草圖

用紙筆或簡報畫出元件大致位置。建議：

- **左半部放輸入**（感測器、按鈕），**右半部放輸出**（燈、蜂鳴器、伺服）
- 同一模組的元件放在一起
- 上下兩條電源軌：一條給感測器與旋鈕，一條給伺服與按鈕

> ✅ **M0 審查點**：規劃書初稿交給老師確認後，才開始接線。腳位規劃錯了，後面全部要重來。

---

## 里程碑 1：照明模組

**功能**：環境變暗時，照明燈自動亮起，**越暗越亮**；現場人員用旋鈕調整「多暗才開始亮」。

**接線**：光敏電阻（含 10kΩ）、旋鈕、照明燈。**腳位照 M0 規劃表**。

```cpp
// ===== M1 照明模組 =====
// ===== 設定區：只改這裡 =====
int pinLDR  = A1;   // 光敏電阻
int pinKnob = A0;   // 照明門檻旋鈕
int pinLamp = ?;    // 照明燈（調光）
// ============================

void setup() {
  pinMode(pinLamp, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int light = analogRead(pinLDR);
  int threshold = analogRead(pinKnob);   // 旋鈕決定「多暗才開燈」

  int lamp = 0;
  if (light < threshold) {
    lamp = map(light, 0, threshold, 255, 30);   // 越暗越亮
  }
  analogWrite(pinLamp, lamp);

  Serial.print("Light: ");
  Serial.print(light);
  Serial.print("   Threshold: ");
  Serial.print(threshold);
  Serial.print("   Lamp: ");
  Serial.println(lamp);
  delay(200);
}
```

**M1 驗收**：

- [ ] 拖曳光線滑桿變暗，照明燈亮起，且越暗越亮
- [ ] 轉旋鈕能改變開燈門檻（Serial Monitor 的 Threshold 有變化）
- [ ] 接線與 M0 腳位規劃表一致

**存檔**：`MID-M1-組別`

---

## 里程碑 2：溫度安全模組

**功能**：溫度分正常、警戒、危險三段，RGB 與蜂鳴器提示；進入危險後**鎖定**，溫度降回危險線以下並按「復歸鈕」才解除。

**接線**：TMP36、RGB、蜂鳴器、復歸鈕。**腳位照 M0 規劃表**。

```cpp
// ===== M2 溫度安全模組 =====
// ===== 設定區：只改這裡 =====
int pinTemp   = A2;                 // TMP36
int pinR = ?, pinG = ?, pinB = ?;   // 狀態 RGB
int pinBuzzer = ?;                  // 蜂鳴器
int btnReset  = ?;                  // 復歸鈕
float warnLine   = 30.0;            // 警戒線（依客戶規格修改）
float dangerLine = 35.0;            // 危險線（依客戶規格修改）
// ============================

bool tempLocked = false;
int lastReset = LOW;
int tick = 0;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setColor(int r, int g, int b) {
  digitalWrite(pinR, r);
  digitalWrite(pinG, g);
  digitalWrite(pinB, b);
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(btnReset, INPUT);
  Serial.begin(9600);
}

void loop() {
  tick++;
  float t = readC(pinTemp);

  if (t >= dangerLine && !tempLocked) {
    tempLocked = true;
    Serial.println("TEMP DANGER - LOCKED");
  }

  int now = digitalRead(btnReset);
  if (now == HIGH && lastReset == LOW) {
    delay(50);
    if (tempLocked && t < dangerLine) {
      tempLocked = false;
      noTone(pinBuzzer);
      Serial.println("Temp lock RESET");
    } else if (tempLocked) {
      Serial.println("Cannot reset: still too hot");
    }
  }
  lastReset = now;

  if (tempLocked) {
    setColor(HIGH, LOW, LOW);                        // 紅
    tone(pinBuzzer, 2000);
  } else if (t >= warnLine) {
    setColor(HIGH, HIGH, LOW);                       // 黃
    if (tick % 20 == 0) tone(pinBuzzer, 1000, 80);
  } else {
    setColor(LOW, HIGH, LOW);                        // 綠
  }

  if (tick % 10 == 0) {
    Serial.print("Temp: ");
    Serial.print(t);
    Serial.println(" C");
  }
  delay(50);
}
```

> 💡 注意：本模組的 `setColor()` 用的是 `digitalWrite`（全亮或全滅），所以 RGB **不需要接 `~` 腳**。這正是 M0 規劃關鍵題第 3 題的答案。

**M2 驗收**：

- [ ] 溫度依客戶規格顯示綠／黃／紅
- [ ] 進入危險後即使降溫也維持紅燈與警報
- [ ] 溫度仍高於危險線時按復歸無效；降溫後按復歸才解除
- [ ] 接線與 M0 腳位規劃表一致

**存檔**：`MID-M2-組別`

---

## 里程碑 3：出入管制模組

**功能**：按「進場鈕」代表有人要進入：未客滿時閘門打開 2 秒再關上，人數加 1；客滿時閘門不動並發出拒絕聲。按「離場鈕」人數減 1。

**接線**：伺服、進場鈕、離場鈕、蜂鳴器。**腳位照 M0 規劃表**。

```cpp
// ===== M3 出入管制模組 =====
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int pinServo  = ?;      // 閘門伺服
int btnEnter  = ?;      // 進場鈕
int btnExit   = ?;      // 離場鈕
int pinBuzzer = ?;      // 蜂鳴器
int capacity    = 5;    // 人數上限（依客戶規格修改）
int closedAngle = 0;    // 閘門關閉角度
int openAngle   = 90;   // 閘門開啟角度
int openTime    = 2000; // 開啟時間（毫秒）
// ============================

Servo gate;
int inside = 0;
int lastEnter = LOW, lastExit = LOW;

void sweepTo(int from, int to) {
  int step = (to > from) ? 1 : -1;
  for (int a = from; a != to; a += step) {
    gate.write(a);
    delay(8);
  }
  gate.write(to);
}

void setup() {
  gate.attach(pinServo);
  gate.write(closedAngle);
  pinMode(btnEnter, INPUT);
  pinMode(btnExit, INPUT);
  Serial.begin(9600);
}

void loop() {
  int nowEnter = digitalRead(btnEnter);
  if (nowEnter == HIGH && lastEnter == LOW) {
    delay(50);
    if (inside >= capacity) {
      tone(pinBuzzer, 300, 600);
      Serial.println("FULL - entry refused");
    } else {
      inside++;
      tone(pinBuzzer, 1200, 100);
      Serial.print("Enter. Inside: ");
      Serial.println(inside);
      sweepTo(closedAngle, openAngle);
      delay(openTime);
      sweepTo(openAngle, closedAngle);
    }
  }
  lastEnter = nowEnter;

  int nowExit = digitalRead(btnExit);
  if (nowExit == HIGH && lastExit == LOW) {
    delay(50);
    if (inside > 0) inside--;
    Serial.print("Exit. Inside: ");
    Serial.println(inside);
  }
  lastExit = nowExit;
}
```

**M3 驗收**：

- [ ] 未客滿時按進場，閘門平滑打開、停留、關上，人數加 1
- [ ] 客滿時按進場，閘門不動並發出拒絕聲
- [ ] 按離場人數減 1，人數不會變成負數
- [ ] 接線與 M0 腳位規劃表一致

**存檔**：`MID-M3-組別`

---

## 里程碑 4：系統整合與安全優先權

### 整合步驟

1. **另存新檔**：從 M3 的檔案另存為 `MID-M4-組別`。
2. **合併電路**：把 M1、M2 的元件照原腳位搬到同一塊麵包板，再加上**急停鈕**。
3. **全面體檢**：用接線驗證器、類比讀值檢查器、溫度讀值檢查器、伺服測試器逐一確認。
4. **上傳整合程式**：填好設定區，依序打開四個功能開關，每開一個就測一次。

### 優先權：功能衝突時，誰說了算？

整合後最大的問題是「功能互相打架」。例如：溫度危險時，有人按進場鈕，閘門該不該開？程式用**檢查順序**決定優先權，越前面越優先：

```
每一輪 loop 依序檢查：
  ① 急停中？        → 是：所有馬達停止，只保留照明與警示，後面全部跳過
  ② 溫度危險鎖定？  → 是：閘門鎖住（安全連鎖），照明照常運作
  ③ 照明控制        → 依光線調整
  ④ 出入管制        → 只有在 ①② 都沒事時才會執行
  ⑤ 總狀態燈        → 依燈號定義表顯示最優先的狀況
```

> 🤔 **急停時照明要不要關？** 本範本選擇「急停時照明保持運作」——因為緊急狀況下人員需要看得見才能疏散。你的客戶情境也是這樣嗎？請在規劃書說明。

### 整合程式（只改設定區）

```cpp
// ===== M4 智慧場域監控站：整合系統 =====
#include <Servo.h>

// ===== 設定區：只改這裡 =====
// --- 腳位（照 M0 規劃表） ---
int pinKnob = A0, pinLDR = A1, pinTemp = A2;
int pinLamp   = ?;                  // 照明燈（調光 ~ 腳）
int pinR = ?, pinG = ?, pinB = ?;   // 總狀態 RGB
int pinBuzzer = ?;                  // 蜂鳴器
int pinServo  = ?;                  // 閘門伺服
int btnEnter  = ?;                  // 進場鈕
int btnExit   = ?;                  // 離場鈕
int btnReset  = ?;                  // 復歸鈕
int btnEStop  = ?;                  // 急停鈕
// --- 客戶規格 ---
float warnLine   = 30.0;            // 溫度警戒線
float dangerLine = 35.0;            // 溫度危險線
int capacity     = 5;               // 人數上限
int closedAngle  = 0;               // 閘門關閉角度
int openAngle    = 90;              // 閘門開啟角度
int openTime     = 2000;            // 閘門開啟時間（毫秒）
// --- 功能開關：逐一打開測試 ---
bool useLighting = false;           // 照明模組
bool useTemp     = false;           // 溫度安全模組
bool useGate     = false;           // 出入管制模組
bool useEStop    = false;           // 急停
// ============================

Servo gate;
bool eStopped = false;
bool tempLocked = false;
int inside = 0;
int tick = 0;
int lastEnter = LOW, lastExit = LOW, lastReset = LOW, lastEStop = LOW;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setColor(int r, int g, int b) {
  digitalWrite(pinR, r);
  digitalWrite(pinG, g);
  digitalWrite(pinB, b);
}

void sweepTo(int from, int to) {
  int step = (to > from) ? 1 : -1;
  for (int a = from; a != to; a += step) {
    gate.write(a);
    delay(8);
  }
  gate.write(to);
}

// 偵測按鈕「剛按下」的瞬間
bool pressed(int pin, int &last) {
  int now = digitalRead(pin);
  bool edge = (now == HIGH && last == LOW);
  last = now;
  if (edge) delay(50);
  return edge;
}

// ---- 模組 1：照明 ----
void handleLighting() {
  int light = analogRead(pinLDR);
  int threshold = analogRead(pinKnob);
  int lamp = 0;
  if (light < threshold) lamp = map(light, 0, threshold, 255, 30);
  analogWrite(pinLamp, lamp);
}

void setup() {
  gate.attach(pinServo);
  gate.write(closedAngle);
  pinMode(pinLamp, OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(btnEnter, INPUT);
  pinMode(btnExit, INPUT);
  pinMode(btnReset, INPUT);
  pinMode(btnEStop, INPUT);
  Serial.begin(9600);
  Serial.println("=== Smart Site Monitor READY ===");
}

void loop() {
  tick++;
  bool enterHit = pressed(btnEnter, lastEnter);
  bool exitHit  = pressed(btnExit, lastExit);
  bool resetHit = pressed(btnReset, lastReset);
  bool eStopHit = pressed(btnEStop, lastEStop);

  float t = readC(pinTemp);

  // ===== ① 急停：最高優先 =====
  if (useEStop) {
    if (eStopHit && !eStopped) {
      eStopped = true;
      gate.write(closedAngle);
      Serial.println("!!! EMERGENCY STOP !!!");
    }
    if (eStopped) {
      if (resetHit) {
        eStopped = false;
        resetHit = false;                         // 這一按只用來解除急停
        noTone(pinBuzzer);
        Serial.println("E-STOP released by reset");
      } else {
        if (useLighting) handleLighting();       // 急停時照明保持運作
        bool blink = (tick / 10) % 2;
        setColor(blink ? HIGH : LOW, LOW, LOW);   // 紅色閃爍
        if (tick % 20 == 0) tone(pinBuzzer, 400, 150);
        delay(30);
        return;                                   // 後面全部跳過
      }
    }
  }

  // ===== ② 溫度安全 =====
  int tempLevel = 0;                              // 0=正常, 1=警戒, 2=危險
  if (useTemp) {
    if (t >= dangerLine)    tempLevel = 2;
    else if (t >= warnLine) tempLevel = 1;

    if (tempLevel == 2 && !tempLocked) {
      tempLocked = true;
      Serial.println("TEMP DANGER - LOCKED, gate disabled");
    }
    if (resetHit && tempLocked) {
      if (tempLevel < 2) {
        tempLocked = false;
        noTone(pinBuzzer);
        Serial.println("Temp lock RESET");
      } else {
        Serial.println("Cannot reset: still too hot");
      }
    }
  }

  // ===== ③ 照明 =====
  if (useLighting) handleLighting();

  // ===== ④ 出入管制（溫度鎖定時不執行：安全連鎖） =====
  if (useGate && !tempLocked) {
    if (enterHit) {
      if (inside >= capacity) {
        tone(pinBuzzer, 300, 600);
        Serial.println("FULL - entry refused");
      } else {
        inside++;
        tone(pinBuzzer, 1200, 100);
        Serial.print("Enter. Inside: ");
        Serial.println(inside);
        sweepTo(closedAngle, openAngle);
        delay(openTime);
        sweepTo(openAngle, closedAngle);
      }
    }
    if (exitHit) {
      if (inside > 0) inside--;
      Serial.print("Exit. Inside: ");
      Serial.println(inside);
    }
  } else if (useGate && tempLocked && enterHit) {
    Serial.println("Gate LOCKED by temperature interlock");
  }

  // ===== ⑤ 總狀態燈（依燈號定義表的優先順序） =====
  if (tempLocked) {
    setColor(HIGH, LOW, LOW);                     // 紅：溫度危險
    tone(pinBuzzer, 2000);
  } else if (tempLevel == 1) {
    setColor(HIGH, HIGH, LOW);                    // 黃：溫度警戒
    if (tick % 30 == 0) tone(pinBuzzer, 1000, 80);
  } else if (useGate && inside >= capacity) {
    setColor(HIGH, LOW, HIGH);                    // 紫：客滿
  } else {
    setColor(LOW, HIGH, LOW);                     // 綠：一切正常
  }

  if (tick % 30 == 0) {
    Serial.print("Temp: ");
    Serial.print(t);
    Serial.print(" C   Inside: ");
    Serial.print(inside);
    Serial.print("/");
    Serial.println(capacity);
  }
  delay(30);
}
```

**M4 驗收**：

- [ ] 四個功能開關全部打開後，各模組功能都和 M1～M3 一樣正常
- [ ] 按急停：閘門關閉並停止回應，RGB 紅色閃爍，照明仍運作；按復歸解除
- [ ] 溫度危險時按進場鈕，閘門不動（Serial Monitor 顯示被溫度連鎖鎖住）
- [ ] 總狀態燈依燈號定義表的優先順序顯示

**存檔**：`MID-M4-組別`

---

## 里程碑 5：驗收測試（QA）

> 🏭 **為什麼要做 QA？** 工程師的工作不只是「讓它動起來」，還要**主動找出系統在什麼情況下會出錯**。客戶發現的每一個 bug，都比你自己發現的貴十倍。

### 測試案例表（每組必測，並記錄結果）

| 編號 | 測試情境 | 操作步驟 | 預期結果 | 實際結果 | 通過？ |
| --- | --- | --- | --- | --- | --- |
| T1 | 正常進場 | 正常溫度下按進場鈕 | 閘門開 → 關，人數 +1，綠燈 | | |
| T2 | 客滿拒絕 | 連續按進場超過上限 | 第（上限＋1）次起閘門不動、拒絕聲、紫燈 | | |
| T3 | 離場歸零 | 人數 0 時按離場 | 人數維持 0，不變負數 | | |
| T4 | 溫度連鎖 | 溫度拉過危險線後按進場 | 閘門不動、紅燈長鳴 | | |
| T5 | 復歸條件 | 溫度仍在危險線上方時按復歸 | 無法解除 | | |
| T6 | 急停優先 | 溫度危險時再按急停 | 紅色閃爍（急停優先於溫度） | | |
| T7 | 急停照明 | 天黑時按急停 | 照明燈保持亮著 | | |
| T8 | 旋鈕極端值 | 旋鈕轉到兩端 | 照明分別「永遠不亮」與「幾乎一直亮」，系統不當機 | | |
| T9 | 快速連按 | 快速交替按進場、離場、急停 | 系統不錯亂，人數合理 | | |
| T10 | 自訂測試 | （由各組設計） | | | |

### 同儕交叉測試

1. 兩組交換 Tinkercad 連結。
2. 對方組依測試案例表測試你的系統，**並至少再設計 2 個你們沒想到的測試**。
3. 記錄對方發現的問題，以及你們的修正方式。

> 💼 **思考**：交叉測試發現的問題，你們會怎麼分類？哪些是「一定要修」、哪些是「寫進使用說明書提醒使用者」就好？這就是真實產品開發中的「問題分級」。

---

## 里程碑 6：提案簡報

每組 **5 分鐘簡報＋2 分鐘問答**，對象是「客戶老闆」（不懂技術的人）。

| 段落 | 時間 | 內容 |
| --- | --- | --- |
| 1. 客戶痛點 | 1 分鐘 | 客戶目前遇到什麼問題？不解決會損失什麼？ |
| 2. 系統展示 | 2 分鐘 | Tinkercad 現場操作：正常情境 → 異常情境 → 急停 |
| 3. 安全與可靠度 | 1 分鐘 | 你們設計了哪些安全連鎖？QA 測過哪些極端情況？ |
| 4. 擴充與建議 | 1 分鐘 | 還剩幾隻腳可擴充？下一階段建議加什麼功能？ |

**簡報規則**：

- 不准講程式碼。客戶不在乎 `if` 和 `else`，在乎「店員按這顆鈕會發生什麼事」。
- 每個技術名詞都要翻成客戶聽得懂的話。例如「安全連鎖」→「冷藏櫃壞掉時，自動門會自動鎖住，避免冷氣外洩」。

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| M1～M3 各自正常，合併後某個功能失效 | 兩個模組是否用了同一隻腳 | 對照 M0 腳位規劃表 |
| 照明燈只有全亮全滅 | 是否接在 9、10（伺服借走）或 3、11（蜂鳴器借走） | 改接 5 或 6 |
| 急停後系統「卡住」不動 | 這是正確設計 | 按復歸鈕解除 |
| 溫度鎖定後閘門還能開 | `useTemp` 是否打開 | 開啟功能開關後重測 |
| 合併後感測器讀值亂跳 | 電源軌是否有斷開（麵包板中間的分段） | 檢查 5V、GND 是否全部連通 |
| 伺服動作時其他元件異常 | 伺服紅、棕線是否接在共用電源軌 | 確認電源軌連接 |
| 程式無法上傳 | 設定區是否還有 `?` 沒填 | 填完所有腳位 |
| Serial Monitor 訊息太多看不清 | 正常現象 | 只看事件訊息（Enter、LOCKED、E-STOP） |

---

## 繳交清單與評分

### 繳交清單

| 項目 | 格式 | 對應里程碑 |
| --- | --- | --- |
| 規劃書（需求分析、規格表、燈號定義、腳位規劃、佈局草圖） | PDF 或線上文件 | M0 |
| 照明模組 | Tinkercad 連結 `MID-M1-組別` | M1 |
| 溫度安全模組 | Tinkercad 連結 `MID-M2-組別` | M2 |
| 出入管制模組 | Tinkercad 連結 `MID-M3-組別` | M3 |
| 整合系統 | Tinkercad 連結 `MID-M4-組別` | M4 |
| QA 測試報告（含同儕交叉測試紀錄） | PDF 或線上文件 | M5 |
| 提案簡報檔 | PPT／PDF | M6 |

### 評分標準

| 項目 | 比重 | 優秀（A） | 合格（C） |
| --- | --- | --- | --- |
| 規劃書 | 20% | 需求分析具體、規格有資料來源、腳位規劃正確處理 `~` 腳限制並說明理由 | 表格填寫完整 |
| 三個功能模組 | 15% | 三個模組各自完整運作，接線與規劃書一致 | 至少兩個模組可運作 |
| 整合系統 | 20% | 四大功能整合正常，優先權與安全連鎖正確 | 整合後主要功能可運作 |
| QA 測試報告 | 15% | 測試案例完整、有自訂測試、交叉測試問題有分級與修正 | 完成必測案例 |
| 提案簡報 | 20% | 以客戶語言說明價值與風險，展示流暢，回答問題得當 | 能完整展示系統功能 |
| 團隊分工與麵包板整潔 | 10% | 分工明確、佈局分區清楚 | 有分工紀錄 |

### 自評與互評

每位組員填寫：

| 項目 | 自評（1～5） | 說明 |
| --- | --- | --- |
| 我在 M0 規劃中的貢獻 | | |
| 我負責的模組 | | |
| 我在整合與除錯中的貢獻 | | |
| 我在 QA 與簡報中的貢獻 | | |

---

## 時程建議

本專題建議安排 **2 週**（第 8 週課堂＋第 9 週前半，或依課程調整）：

| 時段 | 內容 | 檢核點 |
| --- | --- | --- |
| 第 1 節 | 選客戶、分組、需求分析、規格表 | — |
| 第 2 節 | 腳位規劃、佈局草圖 | ✅ M0 審查（老師簽核後才能接線） |
| 第 3 節 | 分工完成 M1、M2、M3（每人負責一個模組） | ✅ M1～M3 現場驗收 |
| 課後 | M4 整合、M5 QA 測試、同儕交叉測試 | — |
| 下次上課 | M6 提案簡報 | ✅ 期中成績 |

> 💡 **分工建議**：3 人組一人一個模組；4 人組多出的一人擔任「整合與 QA 負責人」，從 M0 開始就負責腳位規劃表與測試案例。

---

## 反思與延伸討論

1. 本專題先分三個模組各自完成，再整合。如果一開始就把所有元件接在一起、寫一支大程式，會遇到什麼困難？這和專案管理中的「工作分解結構（WBS）」有什麼關係？
2. 整合程式的優先權順序是「急停 → 溫度 → 照明 → 出入管制」。如果把出入管制移到溫度檢查之前，會發生什麼事？這說明了什麼設計原則？
3. 三個客戶案例用的是**同一套程式**，只改設定區的數字。從商業角度來看，這種「平台化」的做法對顧問公司有什麼好處？有什麼風險？
4. 為什麼要主動做 QA 測試，而不是等客戶用了再修？請從成本、商譽、安全三個角度說明。

---

## 下週預告

第 9 週我們要加入**超音波測距模組**，打造「停車場雷達」——系統第一次能「感知距離」。第 5 週用旋鈕假裝的倒車雷達，下週就能換成真正的距離感測器了。

---

## 📎 教師參考：腳位配置範例

<details>
<summary>點此展開（建議 M0 審查後再公布）</summary>

| 元件 | 腳位 | 理由 |
| --- | --- | --- |
| 旋鈕 | A0 | 類比輸入 |
| 光敏電阻 | A1 | 類比輸入 |
| TMP36 | A2 | 類比輸入 |
| 照明燈 | ~5 | 需要調光；有伺服＋蜂鳴器時只剩 5、6 能調光 |
| RGB（R / G / B） | 9 / 10 / 11 | 只用全亮全滅顏色，接在被借走的 `~` 腳也沒問題，等於回收利用 |
| 蜂鳴器 | 8 | 一般腳 |
| 伺服 | 12 | 一般腳，把 ~6 留給未來擴充 |
| 進場鈕 | 2 | 一般腳 |
| 離場鈕 | 3 | 一般腳（~3 已被蜂鳴器借走，拿來接按鈕剛好） |
| 復歸鈕 | 4 | 一般腳 |
| 急停鈕 | 7 | 一般腳 |
| **剩餘** | ~6、13、A3～A5 | ~6 可再接一顆調光燈 |

**審查重點**：

- 照明燈是否接在 5 或 6（最常見錯誤：接在 9 或 10）
- 學生是否能說明 RGB 為什麼不需要 `~` 腳
- 是否有「把被借走的 `~` 腳拿去接按鈕、RGB」的資源回收思維（加分）

</details>

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 7 週](week-07.md) | [下一週：第 9 週 — 停車場雷達 ➡](week-09.md)
