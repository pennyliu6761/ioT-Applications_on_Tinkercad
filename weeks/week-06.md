[⬅ 回目錄](../README.md) | [⬅ 上一週：第 5 週](week-05.md) | [下一週：第 7 週 — 機械保全閘門 ➡](week-07.md)

---

# 第 6 週：溫度監控的智慧場域

## 主題專案：讓系統「感覺冷熱」並守住溫度

> 🎯 **本週定位**：上週系統學會了「看光線」，這一週再加一個感官：**溫度感測器 TMP36**。溫度是工業與服務業最常監控的數據之一——藥品、食材、機房、溫室、冷鏈物流，溫度一旦失控，損失往往很大。
>
> 和第 4、5 週一樣，本週**重點不在寫程式**，而在於：
>
> 1. 讀懂情境需求，判斷需要哪些元件
> 2. 規劃腳位，在 Tinkercad 把電路一步步接起來
> 3. 想清楚「多少度算正常、多少度要警告、警報要不要人工確認」
>
> 「讀數換算成攝氏度」的程式已經寫好，你只需要填腳位、調整溫度範圍、把功能開關從 `false` 改成 `true`。
>
> 🏁 **本週成果**：完成 **4 個獨立專案**，每個專案存成一個獨立的 Tinkercad 檔案繳交。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：溫度感測器 TMP36](#認識新元件溫度感測器-tmp36)
- [配線口訣卡](#配線口訣卡)
- [三個共用工具](#三個共用工具)
- [專案一：藥品冷藏櫃溫度監測](#專案一藥品冷藏櫃溫度監測)
- [專案二：溫室自動通風](#專案二溫室自動通風)
- [專案三：機房過熱保護](#專案三機房過熱保護)
- [專案四：冷鏈物流雙溫層貨車](#專案四冷鏈物流雙溫層貨車)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 嬰兒房溫度提醒](#-lv1-嬰兒房溫度提醒)
  - [Lv.2 咖啡機保溫顯示](#-lv2-咖啡機保溫顯示)
  - [Lv.3 教室冷氣節能控制](#-lv3-教室冷氣節能控制)
  - [Lv.4 伺服器機櫃散熱監測](#-lv4-伺服器機櫃散熱監測)
  - [Lv.5 食品工廠三區溫控中心](#-lv5-食品工廠三區溫控中心)
  - [🏆 Lv.6 BOSS 關：物聯網顧問提案](#-lv6-boss-關物聯網顧問提案)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～5 週）

- ✅ 單色 LED、RGB LED 的接法，每條燈線串一顆 220Ω 電阻
- ✅ 按鈕、光敏電阻都要搭配 10kΩ；旋鈕不用電阻
- ✅ 蜂鳴器接一般腳位、不用電阻；有蜂鳴器時 RGB 避開 3、11
- ✅ 「只在狀態改變的瞬間」計數；警報鎖定、人工解除

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 溫度感測器 TMP36 | 依溫度輸出不同的數值，程式換算成 °C | 冷藏櫃溫度計、機房監控、體溫計 |
| 溫度換算函式 `readC()` | 已經寫好的「翻譯機」，把讀值變成攝氏溫度 | 把儀器讀數換成報表單位 |
| 溫度區間 | 正常／警戒／危險三段，用兩條線劃分 | 品管規格的「規格內、管制界限、規格外」 |
| 靜音 vs 鎖定 | 靜音只關掉聲音；鎖定要人工確認才解除 | 「知道了」和「處理好了」是兩件事 |

### 本週可用元件（只限這些）

| 元件 | 第幾週學的 |
| --- | --- |
| 單色 LED、220Ω 電阻 | 第 1 週 |
| 按鈕、10kΩ 電阻 | 第 2 週 |
| RGB LED（共陰極） | 第 3 週 |
| 旋鈕（Potentiometer） | 第 4 週 |
| 蜂鳴器、光敏電阻 | 第 5 週 |
| 溫度感測器 TMP36 | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：顧問公司這週接到四個和「溫度」有關的委託案。客戶都很緊張——溫度一旦失控，藥品報廢、作物枯死、伺服器當機、整車生鮮退貨，損失動輒數十萬。
>
> | 客戶 | 委託內容 | 難度 |
> | --- | --- | --- |
> | 社區藥局 | 疫苗冷藏櫃溫度監測 | ⭐ |
> | 有機農場 | 溫室自動通風 | ⭐⭐ |
> | 科技公司 | 機房過熱保護 | ⭐⭐⭐ |
> | 冷鏈物流公司 | 雙溫層貨車監控 | ⭐⭐⭐⭐ |
>
> 每個專案都分成 3 個步驟，從最簡單的版本開始，每一步多接幾個元件、多一項功能。

---

## 認識新元件：溫度感測器 TMP36

### 外觀與接法

TMP36 有 **3 隻腳**，外觀和旋鈕一樣是「左、中、右」，但它**有方向性，不能接反**：

> **平面朝向自己：左腳 5V、中間接 A 腳、右腳 GND。**

| TMP36 的腳（平面朝自己） | 接到哪裡 |
| --- | --- |
| 左腳 | 5V |
| **中間腳** | **A0～A5 其中一隻** |
| 右腳 | GND |

- TMP36 **不需要**電阻。
- 在 Tinkercad 把滑鼠移到腳上，會顯示該腳名稱（Power／Vout／GND），接線前先確認一次。
- ⚠️ 旋鈕的左右腳接反只會「方向相反」，但 **TMP36 接反會讀到錯誤溫度**（真實硬體上還會發燙損壞）。
- 開始模擬後，**點一下 TMP36**，會出現溫度滑桿，拖曳它就能模擬變冷或變熱（範圍 −40～125°C）。

### 讀值怎麼變成攝氏溫度？

本週所有程式都附上同一個「翻譯機」函式，你**不需要修改它**，只要知道它會回傳 °C：

```cpp
// 溫度翻譯機：輸入 A 腳，回傳攝氏溫度
float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;   // 讀值 → 電壓
  return (voltage - 0.5) * 100.0;                    // 電壓 → °C
}
```

> 💡 `float` 是「可以有小數點的數字」，所以溫度能顯示成 24.85°C，而不是只有 24。

### 溫度區間：用兩條線切成三段

本週的情境幾乎都用同一種思考方式——**先決定「正常範圍」，再決定「多偏離算危險」**：

```
  危險（紅）     ← 超過上限 + 緩衝
  ──────────── 危險線
  警戒（黃）     ← 已經偏離，但還有時間處理
  ──────────── 警戒線
  正常（綠）
```

> 🏭 這跟品管的「規格界限」很像：在規格內安心生產；接近界限要注意；超出界限必須停下處理。

### 🧭 四種「A 腳輸入元件」比一比

| 元件 | 誰來改變數值？ | 需要 10kΩ 嗎？ | 接反會怎樣？ |
| --- | --- | --- | --- |
| 按鈕（第 2 週） | 人按下 | 要 | — |
| 旋鈕（第 4 週） | 人轉動 | 不用 | 方向相反，沒關係 |
| 光敏電阻（第 5 週） | 環境光線 | 要 | 沒有方向 |
| **TMP36（本週）** | **環境溫度** | **不用** | **讀值錯誤，一定要接對** |

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **TMP36：平面朝自己，左 5V、中 A 腳、右 GND** | 不用電阻，不能接反 |
| ② 要調亮度、要混色 → 接 `~` 腳 | 只有 3、5、6、9、10、11 |
| ③ 有蜂鳴器時，RGB 避開 3、11 | 從 5、6、9、10 裡挑 |
| ④ 只要亮或滅、蜂鳴器、按鈕 → 一般腳位就好 | 把 `~` 腳留給需要調光的燈 |
| ⑤ 每條燈線一顆 220Ω；蜂鳴器、旋鈕、TMP36 不用 | |
| ⑥ 按鈕、光敏電阻各配一顆 10kΩ | |
| ⑦ Pin 0、1 先不要用；腳位不夠時借 A 腳 | A 腳也能接燈、按鈕、蜂鳴器 |

---

## 三個共用工具

### 🔧 工具 1：溫度讀值檢查器

接好 TMP36 後，先跑這支程式。拖曳 Tinkercad 的溫度滑桿，確認 Serial Monitor 顯示的 °C **跟滑桿上的溫度差不多**。

```cpp
int pinTemp = A0;   // ← 改成你 TMP36 中間腳接的 A 腳

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setup() {
  Serial.begin(9600);
}

void loop() {
  Serial.print("Raw: ");
  Serial.print(analogRead(pinTemp));
  Serial.print("   Temp: ");
  Serial.print(readC(pinTemp));
  Serial.println(" C");
  delay(300);
}
```

**檢查標準**：滑桿拉到 25°C，Serial Monitor 顯示約 24～26°C。如果差很多（例如差了幾十度），先檢查 TMP36 有沒有接反。

### 🔧 工具 2：接線驗證器（沿用前幾週）

```cpp
int pins[] = { 9, 10, 11 };   // ← 填入所有「燈」的腳位（不含蜂鳴器）
int numPins = 3;              // ← 腳位數量

void setup() {
  Serial.begin(9600);
  for (int i = 0; i < numPins; i++) pinMode(pins[i], OUTPUT);
}

void loop() {
  for (int i = 0; i < numPins; i++) {
    Serial.print("Testing Pin ");
    Serial.println(pins[i]);
    digitalWrite(pins[i], HIGH);
    delay(500);
    digitalWrite(pins[i], LOW);
  }
  Serial.println("---");
  delay(1000);
}
```

### 🔧 工具 3：蜂鳴器測試器（沿用第 5 週）

```cpp
int pinBuzzer = 8;   // ← 改成你蜂鳴器接的腳位
int notes[] = { 262, 294, 330, 349, 392 };

void setup() {
  for (int i = 0; i < 5; i++) {
    tone(pinBuzzer, notes[i], 300);
    delay(400);
  }
}

void loop() {
}
```

> 📌 **每個專案的固定流程**
> 1. **Step 0**：讀完情境，先填好腳位規劃表（包含還沒接的元件）
> 2. 把腳位填進程式的「設定區」，並確認溫度範圍符合情境
> 3. 照 Step 1 → 2 → 3 逐步接線，每完成一步就把對應的功能開關改成 `true`
> 4. 每一步都先用工具確認接線，再拖曳溫度滑桿測試

---

## 專案一：藥品冷藏櫃溫度監測

⭐ **難度**：TMP36 ＋ 兩顆單色燈，再逐步加蜂鳴器與靜音鈕

**客戶需求**：社區藥局的疫苗冷藏櫃必須維持在 **2～8°C**。溫度在範圍內亮綠燈，超出範圍亮紅燈並嗶嗶叫。藥師聽到警報後，可以按「靜音鈕」先關掉聲音去處理；但紅燈要繼續亮著，直到溫度回到正常範圍。

### Step 0：腳位規劃

| 元件 | 腳位 | 需要 `~` 腳嗎？為什麼？ |
| --- | --- | --- |
| TMP36 | A0 | — |
| 正常燈（綠色 LED） | | |
| 異常燈（紅色 LED） | | |
| 警報蜂鳴器 | | |
| 靜音鈕 | | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | TMP36＋綠燈＋紅燈 | 溫度在 2～8°C 亮綠，超出亮紅 | （不用改） |
| Step 2 | ＋蜂鳴器 | 超出範圍時嗶嗶叫 | `useBuzzer = true` |
| Step 3 | ＋1 顆按鈕 | 按靜音鈕，聲音停但紅燈繼續亮；溫度恢復後靜音自動取消 | `useMute = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinTemp   = A0;    // TMP36
int ledOK     = ?;     // 正常燈（綠）
int ledBad    = ?;     // 異常燈（紅）
int pinBuzzer = ?;     // 警報蜂鳴器
int pinMute   = ?;     // 靜音鈕
float lowLimit  = 2.0; // 下限 °C
float highLimit = 8.0; // 上限 °C
bool useBuzzer = false; // Step 2 接好後改成 true
bool useMute   = false; // Step 3 接好後改成 true
// ============================

bool muted = false;
int lastButton = LOW;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setup() {
  pinMode(ledOK, OUTPUT);
  pinMode(ledBad, OUTPUT);
  pinMode(pinMute, INPUT);
  Serial.begin(9600);
}

void loop() {
  float t = readC(pinTemp);
  bool ok = (t >= lowLimit && t <= highLimit);

  digitalWrite(ledOK,  ok ? HIGH : LOW);
  digitalWrite(ledBad, ok ? LOW : HIGH);

  if (ok) muted = false;               // 溫度恢復，靜音自動取消

  if (useMute) {
    int now = digitalRead(pinMute);
    if (now == HIGH && lastButton == LOW) {
      delay(50);
      if (!ok) {
        muted = true;
        Serial.println("Alarm muted by pharmacist");
      }
    }
    lastButton = now;
  }

  if (useBuzzer && !ok && !muted) {
    tone(pinBuzzer, 1200, 150);
  }

  Serial.print("Fridge: ");
  Serial.print(t);
  Serial.println(" C");
  delay(500);
}
```

### 情境思考

1. 為什麼按了靜音後，紅燈還要繼續亮？如果按一下就全部恢復成綠燈，會有什麼風險？
2. 藥局晚上沒人，冷藏櫃半夜壞掉、早上 8 點才被發現。這個系統能解決這個問題嗎？還缺什麼？（提示：想想之後物聯網課程會學到的「遠端通知」）
3. 💼 疫苗冷藏櫃的溫度紀錄常是衛生單位稽查的項目。除了即時警報，藥局還需要保存什麼資料？

### ✅ 專案一驗收

- [ ] 溫度在 2～8°C 亮綠燈，低於 2°C 或高於 8°C 亮紅燈
- [ ] 異常時蜂鳴器嗶嗶叫
- [ ] 按靜音鈕後聲音停止、紅燈仍亮
- [ ] 溫度恢復正常後再次超出，警報會重新響起

---

## 專案二：溫室自動通風

⭐⭐ **難度**：TMP36 ＋ 調光燈模擬風扇 ＋ 旋鈕 ＋ 光敏電阻

**客戶需求**：有機農場的溫室，溫度超過目標值時要開風扇通風，**越熱風扇轉越快**。（馬達要到下週才會學，所以先用一顆 LED 的亮度代表風扇轉速。）不同作物的目標溫度不同，農場主人要能用旋鈕自己設定。另外，夜間植物需要比較涼，所以晚上的目標溫度要自動**降低 5°C**；管理室的狀態燈白天亮黃、晚上亮藍。

### Step 0：腳位規劃

| 元件 | 腳位 | 需要 `~` 腳嗎？為什麼？ |
| --- | --- | --- |
| TMP36 | A0 | — |
| 風扇（用 LED 亮度代表轉速） | | |
| 目標溫度旋鈕 | | — |
| 光敏電阻（日夜偵測） | | — |
| 狀態燈 RGB（R / G / B） | | |

> 🤔 **規劃提示**：這一關**沒有蜂鳴器**。那 RGB 可以用 3、11 嗎？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | TMP36＋1 顆 LED（風扇） | 超過 25°C 開始亮，越熱越亮 | （不用改） |
| Step 2 | ＋1 顆旋鈕 | 旋鈕設定目標溫度 15～35°C | `useKnob = true` |
| Step 3 | ＋光敏電阻（含 10kΩ）＋RGB | 天黑時目標溫度自動降 5°C，狀態燈變藍 | `useNight = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinTemp = A0;                   // TMP36
int pinFan  = ?;                    // 風扇（LED 亮度代表轉速）
int pinKnob = ?;                    // 目標溫度旋鈕（A 腳）
int pinLDR  = ?;                    // 日夜偵測光敏電阻（A 腳）
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈 RGB
int darkLine = 300;                 // 光線低於這個數字算「夜間」
bool useKnob  = false;              // Step 2 接好後改成 true
bool useNight = false;              // Step 3 接好後改成 true
// ============================

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinFan, OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float t = readC(pinTemp);

  int target = 25;                                           // 預設目標 25°C
  if (useKnob) target = map(analogRead(pinKnob), 0, 1023, 15, 35);

  if (useNight) {
    bool night = analogRead(pinLDR) < darkLine;
    if (night) {
      target = target - 5;                                   // 夜間目標降 5°C
      setColor(0, 0, 255);                                   // 夜間：藍
    } else {
      setColor(255, 180, 0);                                 // 白天：黃
    }
  }

  // 超過目標幾度？每超過 1°C 風扇加 50，超過 5°C 全速
  float over = t - target;
  int fan = 0;
  if (over > 0) fan = constrain((int)(over * 50), 0, 255);
  analogWrite(pinFan, fan);

  Serial.print("Temp: ");
  Serial.print(t);
  Serial.print(" C   Target: ");
  Serial.print(target);
  Serial.print(" C   Fan: ");
  Serial.println(fan);
  delay(300);
}
```

### 情境思考

1. 風扇 LED 如果接在一般腳位，會看到什麼現象？對溫室來說，「只有全速或停止」的風扇有什麼缺點？
2. 光敏電阻要裝在溫室的哪裡才能正確判斷日夜？如果裝在溫室的補光燈旁邊會怎樣？
3. 💼 「越熱轉越快」和「超過就全速、沒超過就停」相比，在電費和作物品質上各有什麼影響？

### ✅ 專案二驗收

- [ ] 溫度超過目標時風扇 LED 亮起，溫度越高越亮
- [ ] 旋鈕能改變目標溫度（用 Serial Monitor 驗證 Target）
- [ ] 天黑時目標溫度自動降 5°C，狀態燈由黃轉藍
- [ ] 光敏電阻有接 10kΩ，TMP36 沒有接反

---

## 專案三：機房過熱保護

⭐⭐⭐ **難度**：TMP36 ＋ RGB 三段燈號 ＋ 蜂鳴器（注意 3、11）＋ 鎖定與人工解除

**客戶需求**：科技公司的伺服器機房要有過熱保護。溫度分三段，用 RGB 燈號與聲音提示。一旦進入「危險」，警報就**鎖定**：即使冷氣把溫度拉回來，警報也不會自己停，必須由值班工程師確認原因後按「復歸鈕」才能解除——而且**溫度還沒降回危險線以下時，不准解除**。系統還要記錄今天發生了幾次過熱事件。

| 溫度區間 | 燈號 | 聲音 |
| --- | --- | --- |
| 正常（低於 30°C） | 綠 | 安靜 |
| 警戒（30～35°C） | 黃 | 間歇短嗶 |
| 危險（35°C 以上）或已鎖定 | 紅 | 持續鳴叫 |

### Step 0：腳位規劃

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| TMP36 | A0 | — |
| 狀態燈 RGB（R / G / B） | | |
| 警報蜂鳴器 | | |
| 復歸鈕 | | |

> 🤔 **規劃提示**：有蜂鳴器，RGB 要避開哪兩隻腳？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | TMP36＋RGB | 綠／黃／紅三段燈號 | （不用改） |
| Step 2 | ＋蜂鳴器 | 警戒短嗶、危險長鳴 | `useBuzzer = true` |
| Step 3 | ＋1 顆按鈕 | 危險後鎖定，要按復歸鈕才解除，並記錄事件次數 | `useLatch = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinTemp   = A0;                 // TMP36
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈 RGB（避開 3、11）
int pinBuzzer = ?;                  // 警報蜂鳴器
int pinReset  = ?;                  // 復歸鈕
float warnLine   = 30.0;            // 警戒線 °C
float dangerLine = 35.0;            // 危險線 °C
bool useBuzzer = false;             // Step 2 接好後改成 true
bool useLatch  = false;             // Step 3 接好後改成 true
// ============================

bool latched = false;
int events = 0;
int lastButton = LOW;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(pinReset, INPUT);
  Serial.begin(9600);
}

void loop() {
  float t = readC(pinTemp);

  int level = 0;                              // 0=正常, 1=警戒, 2=危險
  if (t >= dangerLine)    level = 2;
  else if (t >= warnLine) level = 1;

  if (useLatch) {
    if (level == 2 && !latched) {             // 剛進入危險：鎖定並記一次
      latched = true;
      events++;
      Serial.print("OVERHEAT EVENT #");
      Serial.println(events);
    }
    int now = digitalRead(pinReset);
    if (now == HIGH && lastButton == LOW) {
      delay(50);
      if (latched && level < 2) {
        latched = false;
        Serial.println("Reset by engineer");
      } else if (latched) {
        Serial.println("Cannot reset: still too hot");
      }
    }
    lastButton = now;
  }

  int show = level;
  if (latched) show = 2;                      // 鎖定中一律顯示危險

  if (show == 0) {
    setColor(0, 255, 0);
    noTone(pinBuzzer);
  } else if (show == 1) {
    setColor(255, 180, 0);
    if (useBuzzer) tone(pinBuzzer, 1000, 100);
  } else {
    setColor(255, 0, 0);
    if (useBuzzer) tone(pinBuzzer, 2000);
  }

  Serial.print("Server room: ");
  Serial.print(t);
  Serial.println(" C");
  delay(300);
}
```

### 情境思考

1. 專案一的「靜音」和專案三的「鎖定＋復歸」有什麼不同？各適合什麼情境？
2. 機房溫度在 34.9°C 和 35.1°C 之間來回跳動時，如果沒有鎖定設計，警報會發生什麼事？
3. 💼 許多公司規定「復歸前必須填寫事件紀錄」。為什麼要多這一道手續？它和品質管理中的「矯正預防措施（CAPA）」有什麼關係？

### ✅ 專案三驗收

- [ ] 三段燈號正確，且 RGB 沒有接在 3、11
- [ ] 警戒短嗶、危險長鳴
- [ ] 進入危險後，即使降溫也持續紅燈與鳴叫
- [ ] 溫度還在危險線以上時按復歸無效；降溫後按復歸才解除
- [ ] Serial Monitor 的事件次數每次過熱只加 1

---

## 專案四：冷鏈物流雙溫層貨車

⭐⭐⭐⭐ **難度**：**2 顆 TMP36** ＋ 6 顆燈 ＋ RGB ＋ 蜂鳴器 ＋ 按鈕 → `~` 腳不夠分

**客戶需求**：冷鏈物流公司的貨車有兩個車廂：**冷藏室（0～7°C）**和**冷凍室（−18°C 以下）**。駕駛座前方要有一塊面板，兩個車廂各有一組「綠／黃／紅」三顆燈；正中間一顆 RGB 總狀態燈顯示兩車廂中**最糟**的狀況。任一車廂進入紅燈時蜂鳴器鳴叫，駕駛按「確認鈕」可暫停鳴叫，但兩車廂都恢復正常前燈號不會改變。

| 車廂 | 綠（正常） | 黃（偏離） | 紅（異常） |
| --- | --- | --- | --- |
| 冷藏室 | 0～7°C | −2～0°C 或 7～9°C | 其他 |
| 冷凍室 | −18°C 以下 | −18～−15°C | −15°C 以上 |

### Step 0：腳位規劃（本專案的重頭戲）

| 元件 | 需要幾隻腳 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- | --- |
| 冷藏室 TMP36 | 1（A 腳） | A0 | — |
| 冷凍室 TMP36 | 1（A 腳） | | — |
| 冷藏室燈（綠／黃／紅） | | | |
| 冷凍室燈（綠／黃／紅） | | | |
| 總狀態 RGB | | | |
| 蜂鳴器 | | | |
| 確認鈕 | | | |

> 🤔 **規劃提示**：
> - 客戶一開始想要「兩個車廂各一顆 RGB」。有蜂鳴器時，可用的 `~` 腳只剩幾隻？兩顆 RGB 放得下嗎？
> - 所以本專案改成「每車廂三顆單色燈＋一顆總狀態 RGB」。請在情境思考第 1 題說明這個取捨。

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 冷藏室 TMP36＋3 顆燈＋RGB＋蜂鳴器 | 冷藏室三段燈號，紅燈時鳴叫 | （不用改） |
| Step 2 | ＋冷凍室 TMP36＋3 顆燈 | 兩車廂各自顯示，RGB 顯示較糟的那一個 | `useFreezer = true` |
| Step 3 | ＋1 顆按鈕 | 按確認鈕暫停鳴叫，恢復正常後確認自動取消 | `useAck = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int tempChill  = A0;                // 冷藏室 TMP36
int tempFreeze = ?;                 // 冷凍室 TMP36（A 腳）
int ledChill[3]  = { ?, ?, ? };     // 冷藏室：綠、黃、紅
int ledFreeze[3] = { ?, ?, ? };     // 冷凍室：綠、黃、紅
int pinR = ?, pinG = ?, pinB = ?;   // 總狀態 RGB（避開 3、11）
int pinBuzzer = ?;                  // 蜂鳴器
int pinAck    = ?;                  // 駕駛確認鈕
bool useFreezer = false;            // Step 2 接好後改成 true
bool useAck     = false;            // Step 3 接好後改成 true
// ============================

bool acknowledged = false;
int lastButton = LOW;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

// 冷藏室：0=正常, 1=偏離, 2=異常
int chillLevel(float t) {
  if (t >= 0 && t <= 7)  return 0;
  if (t >= -2 && t <= 9) return 1;
  return 2;
}

// 冷凍室：0=正常, 1=偏離, 2=異常
int freezeLevel(float t) {
  if (t <= -18) return 0;
  if (t <= -15) return 1;
  return 2;
}

void showLevel(int leds[], int level) {
  for (int i = 0; i < 3; i++) {
    digitalWrite(leds[i], (i == level) ? HIGH : LOW);
  }
}

void setup() {
  for (int i = 0; i < 3; i++) {
    pinMode(ledChill[i], OUTPUT);
    pinMode(ledFreeze[i], OUTPUT);
  }
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(pinAck, INPUT);
  Serial.begin(9600);
}

void loop() {
  float tc = readC(tempChill);
  int lc = chillLevel(tc);
  showLevel(ledChill, lc);
  int worst = lc;

  float tf = 0;
  if (useFreezer) {
    tf = readC(tempFreeze);
    int lf = freezeLevel(tf);
    showLevel(ledFreeze, lf);
    if (lf > worst) worst = lf;
  }

  // 總狀態燈：顯示兩車廂中最糟的狀況
  if (worst == 0)      setColor(0, 255, 0);
  else if (worst == 1) setColor(255, 180, 0);
  else                 setColor(255, 0, 0);

  // 駕駛確認：只在異常時有效；恢復正常後自動取消
  if (worst < 2) acknowledged = false;
  if (useAck) {
    int now = digitalRead(pinAck);
    if (now == HIGH && lastButton == LOW) {
      delay(50);
      if (worst == 2) {
        acknowledged = true;
        Serial.println("Driver acknowledged");
      }
    }
    lastButton = now;
  }

  if (worst == 2 && !acknowledged) tone(pinBuzzer, 1800, 200);

  Serial.print("Chill: ");
  Serial.print(tc);
  if (useFreezer) {
    Serial.print(" C   Freeze: ");
    Serial.print(tf);
  }
  Serial.println(" C");
  delay(400);
}
```

### 情境思考

1. 用你的腳位規劃表說明：為什麼不能「每個車廂各一顆 RGB」？改用三顆單色燈，對駕駛來說是加分還是扣分？
2. 測試時把冷凍室的溫度滑桿拉高，卻是冷藏室的燈在變。可能是哪裡接錯？用哪個工具最快確認？
3. 💼 貨車在路上，駕駛確認了警報，但他沒辦法停車處理。從冷鏈管理的角度，這時候「誰」應該接手處理？這個面板還缺什麼功能？

### ✅ 專案四驗收

- [ ] 冷藏室、冷凍室各自依區間顯示綠／黃／紅
- [ ] 總狀態 RGB 正確顯示兩者中較糟的狀況，且沒有接在 3、11
- [ ] 任一車廂異常時鳴叫；按確認鈕後停止鳴叫，但燈號維持
- [ ] 兩車廂恢復後再次異常，警報會重新響起
- [ ] 兩顆 TMP36 方向正確，在麵包板上分區清楚

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：四個範例專案是「跟著做」，這一區換你「自己規劃」。每一關都是新情境、新的元件組合，你要先判斷需要什麼元件、接幾顆、接哪隻腳，**還要先想清楚溫度範圍怎麼訂**，再動手接線。
>
> 📈 **難度安排**：
>
> | 關卡 | TMP36 | 其他輸入 | 單色 LED | RGB | 蜂鳴器 | 配線重點 |
> | --- | --- | --- | --- | --- | --- | --- |
> | Lv.1 | 1 | — | — | 1 | — | TMP36 方向、無蜂鳴器時 RGB 可用 3、11 |
> | Lv.2 | 1 | — | 5 | — | 1 | 溫度長條圖 |
> | Lv.3 | 1 | 旋鈕＋按鈕 | 2 | — | — | 「開／關」兩條線的節能邏輯 |
> | Lv.4 | 2 | — | — | 1 | 1 | 兩顆 TMP36 比較溫差 |
> | Lv.5 | 3 | 按鈕 | 9 | 1 | 1 | 一般腳不夠，要借 A 腳 |
> | Lv.6 | 自訂 | 自訂 | 自訂 | 自訂 | 自訂 | 顧問提案 |
>
> 📌 **作答規則**：每一關先填腳位規劃表，再接線；接好後先用三個共用工具檢查，最後才上傳程式。程式只需要改「設定區」。

---

### 🟢 Lv.1 嬰兒房溫度提醒

**情境**：月子中心的每間嬰兒房要放一個溫度提醒燈，讓護理人員巡房時一眼看出房間溫度是否合適：太冷亮藍、舒適亮綠、太熱亮紅。

**元件**：1 顆 TMP36＋1 顆 RGB LED

**你要想的問題**：

- 這一關沒有蜂鳴器，RGB 可以接在哪些腳位？
- TMP36 平面朝向自己時，左中右分別接哪裡？接好後先用工具 1 確認。

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinTemp = A0;                   // TMP36
int pinR = ?, pinG = ?, pinB = ?;   // 提醒燈 RGB
float coldLine = 22.0;              // 低於這個溫度算太冷
float hotLine  = 26.0;              // 高於這個溫度算太熱
// ============================

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float t = readC(pinTemp);
  if (t < coldLine)      setColor(0, 0, 255);   // 太冷：藍
  else if (t > hotLine)  setColor(255, 0, 0);   // 太熱：紅
  else                   setColor(0, 255, 0);   // 舒適：綠

  Serial.print("Room: ");
  Serial.print(t);
  Serial.println(" C");
  delay(500);
}
```

**情境思考**：

1. 嬰兒房的舒適溫度 22～26°C 是誰決定的？你會去哪裡查資料確認？
2. 💼 TMP36 如果放在冷氣出風口正下方，量到的溫度能代表嬰兒床的溫度嗎？你會建議擺在哪裡？

---

### 🟢 Lv.2 咖啡機保溫顯示

**情境**：連鎖咖啡店的自助咖啡機，正面有 5 格長條燈顯示水溫（60～95°C 之間）。水溫第一次達到 **85°C 可沖泡**時，播放一段提示音；如果水溫掉到 80°C 以下再加熱回來，要再提示一次。

**元件**：1 顆 TMP36＋5 顆單色 LED＋1 顆蜂鳴器

**你要想的問題**：

- 5 顆長條燈只需要亮滅，蜂鳴器也一樣。這一關有用到任何 `~` 腳的必要嗎？
- 5 顆燈在麵包板上要依「由低溫到高溫」排好，陣列順序也要一致。

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinTemp = A0;                   // TMP36
int bar[5] = { ?, ?, ?, ?, ? };     // 水溫長條燈（低 → 高）
int pinBuzzer = ?;                  // 提示蜂鳴器
float readyLine = 85.0;             // 可沖泡溫度
float resetLine = 80.0;             // 掉到這以下，下次達標要再提示
// ============================

bool ready = false;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setup() {
  for (int i = 0; i < 5; i++) pinMode(bar[i], OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float t = readC(pinTemp);

  // 60~95°C 換算成亮 0~5 格
  int lit = map((int)t, 60, 95, 0, 5);
  lit = constrain(lit, 0, 5);
  for (int i = 0; i < 5; i++) {
    digitalWrite(bar[i], (i < lit) ? HIGH : LOW);
  }

  if (t >= readyLine && !ready) {          // 剛達到可沖泡
    ready = true;
    Serial.println("Ready to brew!");
    tone(pinBuzzer, 784, 150); delay(200);
    tone(pinBuzzer, 1047, 300); delay(350);
  }
  if (t < resetLine) ready = false;        // 掉溫，重設提示

  Serial.print("Water: ");
  Serial.print(t);
  Serial.println(" C");
  delay(300);
}
```

**情境思考**：

1. 為什麼「再提示」的條件設在 80°C，而不是同樣的 85°C？如果兩條線一樣，水溫在 84.9～85.1°C 跳動時會發生什麼事？
2. 💼 咖啡店要求「水溫不到 85°C 不准出杯」。除了提示音，這個系統還可以怎麼幫店長落實這條 SOP？

---

### 🟡 Lv.3 教室冷氣節能控制

**情境**：學校想讓教室冷氣更省電。老師用旋鈕設定溫度（20～28°C），按鈕是冷氣總開關。冷氣開著時「送風燈」常亮；「壓縮機燈」只在需要製冷時亮。為了避免壓縮機頻繁啟停（很耗電也傷機器），規則是：**高於設定 1°C 才啟動，低於設定 1°C 才停止**。

**元件**：1 顆 TMP36＋1 顆旋鈕＋1 顆按鈕＋2 顆單色 LED（送風燈、壓縮機燈）

**Step 0：腳位規劃**

| 元件 | 腳位 |
| --- | --- |
| TMP36 | A0 |
| 溫度設定旋鈕 | |
| 冷氣總開關 | |
| 送風燈 | |
| 壓縮機燈 | |

**你要想的問題**：

- TMP36 和旋鈕都是三隻腳、都接 A 腳，在麵包板上怎麼擺才不會搞混？哪一個接反會出問題？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinTemp  = A0;    // TMP36
int pinKnob  = ?;     // 溫度設定旋鈕（A 腳）
int pinPower = ?;     // 冷氣總開關
int ledFan   = ?;     // 送風燈
int ledComp  = ?;     // 壓縮機燈
// ============================

bool powerOn = false;
bool cooling = false;
int lastButton = LOW;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setup() {
  pinMode(pinPower, INPUT);
  pinMode(ledFan, OUTPUT);
  pinMode(ledComp, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int now = digitalRead(pinPower);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    powerOn = !powerOn;
    Serial.println(powerOn ? "AC ON" : "AC OFF");
  }
  lastButton = now;

  if (!powerOn) {
    cooling = false;
    digitalWrite(ledFan, LOW);
    digitalWrite(ledComp, LOW);
    delay(200);
    return;
  }

  int setTemp = map(analogRead(pinKnob), 0, 1023, 20, 28);
  float t = readC(pinTemp);

  if (t > setTemp + 1)      cooling = true;    // 太熱：啟動壓縮機
  else if (t < setTemp - 1) cooling = false;   // 夠冷：停止壓縮機
  // 介於中間：維持原本狀態

  digitalWrite(ledFan, HIGH);
  digitalWrite(ledComp, cooling ? HIGH : LOW);

  Serial.print("Set: ");
  Serial.print(setTemp);
  Serial.print(" C   Room: ");
  Serial.print(t);
  Serial.println(cooling ? " C   [COOLING]" : " C   [FAN ONLY]");
  delay(300);
}
```

**情境思考**：

1. 把設定溫度轉到 25°C，再把溫度滑桿慢慢從 27°C 拉到 23°C、再拉回 27°C，記錄壓縮機燈在哪些溫度亮、哪些溫度熄。它和「超過 25°C 就開、低於 25°C 就關」有什麼不同？
2. 💼 學校有 60 間教室。從電費、設備壽命、師生舒適度三個面向，說明這個「上下各留 1°C」的設計值不值得。

---

### 🟡 Lv.4 伺服器機櫃散熱監測

**情境**：資料中心的每座機櫃前後各裝一顆 TMP36：**前方量進風溫度、後方量出風溫度**。出風減進風的「溫差」越大，代表機器發熱越多或散熱越差。RGB 顯示狀態，異常時蜂鳴器提醒。

| 狀況 | RGB | 蜂鳴器 |
| --- | --- | --- |
| 溫差 8°C 以下 | 綠 | 安靜 |
| 溫差 8～12°C | 黃 | 安靜 |
| 溫差超過 12°C，或進風超過 27°C | 紅 | 嗶嗶 |

**元件**：2 顆 TMP36＋1 顆 RGB LED＋1 顆蜂鳴器

**你要想的問題**：

- 兩顆 TMP36 都要接 5V 與 GND，怎麼共用電源軌？哪一顆接 A0、哪一顆接 A1，要在麵包板上標清楚。
- RGB 要避開哪兩隻腳？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int tempIn  = A0;                   // 進風 TMP36（機櫃前方）
int tempOut = ?;                    // 出風 TMP36（機櫃後方，A 腳）
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈 RGB（避開 3、11）
int pinBuzzer = ?;                  // 蜂鳴器
float inletLimit = 27.0;            // 進風溫度上限
// ============================

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float tIn  = readC(tempIn);
  float tOut = readC(tempOut);
  float diff = tOut - tIn;

  if (diff > 12 || tIn > inletLimit) {
    setColor(255, 0, 0);
    tone(pinBuzzer, 1500, 200);
  } else if (diff > 8) {
    setColor(255, 180, 0);
  } else {
    setColor(0, 255, 0);
  }

  Serial.print("In: ");
  Serial.print(tIn);
  Serial.print(" C   Out: ");
  Serial.print(tOut);
  Serial.print(" C   Diff: ");
  Serial.println(diff);
  delay(500);
}
```

**情境思考**：

1. 為什麼要看「溫差」，而不是只看出風溫度？請舉一個「出風溫度很高但其實沒問題」的例子。
2. 測試時發現溫差永遠是負的。最可能的原因是什麼？要改接線還是改設定區？
3. 💼 資料中心的電費有很大比例花在冷卻。如果所有機櫃都長期亮綠燈、溫差很小，這代表什麼？可能有什麼可以省的地方？

---

### 🔴 Lv.5 食品工廠三區溫控中心

**情境**：食品工廠有三個溫控區，每區的正常範圍不同。中控室的面板上，每區各有三顆燈顯示「偏低／正常／偏高」；總狀態 RGB 在三區都正常時亮綠，任何一區偏離就亮紅並鳴叫。主管按下「確認鈕」可以暫停鳴叫，直到三區都恢復正常。

| 區域 | 正常範圍 |
| --- | --- |
| 麵糰發酵室 | 26～30°C |
| 成品冷卻室 | 15～20°C |
| 包裝區 | 18～24°C |

**元件**：3 顆 TMP36＋9 顆單色 LED＋1 顆 RGB LED＋1 顆蜂鳴器＋1 顆按鈕

**Step 0：腳位規劃（本關重點）**

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 3 顆 TMP36 | 3（A 腳） | A0、A1、A2 |
| 9 顆區域燈 | | |
| 總狀態 RGB | | |
| 蜂鳴器 | | |
| 確認鈕 | | |
| **一般腳位需求合計** | | |
| **Pin 2～13 可用數量** | 12 | |

> 🤔 **規劃提示**：不夠的腳位要從 A3～A5 借。借來的 A 腳適合接燈、按鈕還是蜂鳴器？RGB 能接在 A 腳嗎？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int tempPin[3] = { A0, A1, A2 };    // 發酵室、冷卻室、包裝區 TMP36
float lowLimit[3]  = { 26, 15, 18 };// 各區下限 °C
float highLimit[3] = { 30, 20, 24 };// 各區上限 °C
int led[3][3] = {
  { ?, ?, ? },                      // 發酵室：偏低、正常、偏高
  { ?, ?, ? },                      // 冷卻室：偏低、正常、偏高
  { ?, ?, ? }                       // 包裝區：偏低、正常、偏高
};
int pinR = ?, pinG = ?, pinB = ?;   // 總狀態 RGB（避開 3、11）
int pinBuzzer = ?;                  // 蜂鳴器
int pinAck    = ?;                  // 主管確認鈕
// ============================

bool acknowledged = false;
int lastButton = LOW;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  for (int z = 0; z < 3; z++) {
    for (int i = 0; i < 3; i++) pinMode(led[z][i], OUTPUT);
  }
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(pinAck, INPUT);
  Serial.begin(9600);
}

void loop() {
  bool allOK = true;

  for (int z = 0; z < 3; z++) {
    float t = readC(tempPin[z]);
    int level = 1;                          // 0=偏低, 1=正常, 2=偏高
    if (t < lowLimit[z])  level = 0;
    if (t > highLimit[z]) level = 2;
    for (int i = 0; i < 3; i++) {
      digitalWrite(led[z][i], (i == level) ? HIGH : LOW);
    }
    if (level != 1) allOK = false;

    Serial.print("Zone ");
    Serial.print(z + 1);
    Serial.print(": ");
    Serial.print(t);
    Serial.print(" C   ");
  }
  Serial.println();

  if (allOK) acknowledged = false;

  int now = digitalRead(pinAck);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    if (!allOK) {
      acknowledged = true;
      Serial.println("Supervisor acknowledged");
    }
  }
  lastButton = now;

  if (allOK) {
    setColor(0, 255, 0);
  } else {
    setColor(255, 0, 0);
    if (!acknowledged) tone(pinBuzzer, 1500, 200);
  }
  delay(500);
}
```

**情境思考**：

1. 9 顆區域燈在麵包板上怎麼排列，主管才看得出「哪一區、偏高還是偏低」？請截圖說明。
2. 你借用了哪幾隻 A 腳？接的是什麼元件？如果工廠要再加第四區，還放得下嗎？
3. 💼 三個區域的範圍寫在設定區的陣列裡。如果工廠換季要調整範圍，這種「把數字集中放在設定區」的寫法，對現場人員有什麼好處？

---

### 🏆 Lv.6 BOSS 關：物聯網顧問提案

**情境**：你的顧問公司接到新客戶。從下面三個案例中**任選一個**，自己完成需求分析、元件選擇、腳位規劃與 Tinkercad 實作。

> **案例 A：寵物旅館恆溫房**
> 兩間寵物房（狗房、貓房），各有不同的舒適溫度。每間有溫度燈號；任一間過熱或過冷時警報鳴叫。值班人員按按鈕確認後停止鳴叫。夜間（光敏電阻判斷）時警報音量改成短嗶，避免吵醒其他寵物。
>
> **案例 B：生鮮超市冷藏展示櫃**
> 展示櫃溫度用長條燈與 RGB 顯示，店長用旋鈕設定目標溫度。顧客開門太久導致溫度上升時提示；溫度超標超過一定時間要鎖定警報，需店長確認並在 Serial Monitor 記錄事件次數。
>
> **案例 C：自訂情境**
> 自己找一個生活或職場情境（例如：烘焙坊烤箱預熱提示、實驗室恆溫箱、魚缸水溫監控、長照中心體感溫度）。

**最低規格**：

- 至少 1 顆 TMP36、1 顆蜂鳴器、1 顆 RGB LED、1 顆按鈕
- 總共使用 10 隻以上腳位
- 程式可以從專案一～四或 Lv.1～Lv.5 改寫，說明你是從哪裡改來的、改了哪些設定

**繳交內容**：

1. **需求分析**：這個系統解決什麼問題？誰會操作？誰會看燈、聽聲音？（3～5 句）
2. **溫度規格表**：正常、警戒、危險各是幾度？資料來源或理由是什麼？
3. **警報處理設計**：異常時是「自動恢復」、「靜音」還是「鎖定＋人工復歸」？為什麼？
4. **腳位規劃表**：標註每隻腳接什麼、是否為 `~` 腳、還剩幾隻可擴充
5. **Tinkercad 實作**：電路截圖＋三個共用工具的檢查截圖

**驗收標準**：

- [ ] TMP36 方向正確，工具 1 讀數與滑桿一致
- [ ] 需要漸變或混色的燈接在 `~` 腳，且 RGB 避開 3、11
- [ ] 每條燈線都有 220Ω，按鈕與光敏電阻有 10kΩ，TMP36、旋鈕、蜂鳴器沒有多接電阻
- [ ] 溫度規格與警報處理方式有合理說明
- [ ] 麵包板依功能分區

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 溫度和滑桿差幾十度，或拖曳滑桿幾乎不變 | TMP36 是否接反（5V 和 GND 對調） | 平面朝自己：左 5V、中 A 腳、右 GND；滑鼠移到腳上確認名稱 |
| 溫度亂跳，或一直顯示 −50°C 左右 | 中間腳沒接到 A 腳，或 A 腳編號和程式不一致 | 用工具 1 確認 A 腳 |
| 溫度和滑桿差 1～2°C | 正常的感測誤差 | 不需修正；設定範圍時留一點緩衝 |
| 拉 A 車廂的滑桿，卻是 B 的燈在變 | 兩顆 TMP36 的 A 腳是否接反 | 用工具 1 逐一確認，調整設定區或接線 |
| 警報響時 RGB 顏色怪怪的 | RGB 是否接在 3 或 11 | 改接 5、6、9、10 |
| 風扇 LED 只有全亮或全滅 | 是否接在 `~` 腳 | 改接 `~` 腳 |
| 按復歸鈕沒反應 | 功能開關是否打開；溫度是否還在危險線以上 | 先降溫再按；檢查按鈕 10kΩ |
| 程式無法上傳，出現錯誤訊息 | 設定區是否還有 `?` 沒填 | 所有腳位都要先填，包含還沒接的元件 |

---

## 成果繳交與評分

### 範例專案繳交

每個專案各自建立一個 Tinkercad 檔案，命名格式：`W6-專案編號-學號`（例如 `W6-P3-B12345678`）。

| 繳交項目 | 專案一 | 專案二 | 專案三 | 專案四 |
| --- | --- | --- | --- | --- |
| Tinkercad 分享連結 | ✅ | ✅ | ✅ | ✅ |
| 腳位規劃表 | ✅ | ✅ | ✅ | ✅ |
| 完成 Step 3 的電路截圖 | ✅ | ✅ | ✅ | ✅ |
| 情境思考題作答 | ✅ | ✅ | ✅ | ✅ |

> 🎯 **最低完成標準**：專案一、二完成 Step 3，專案三、四至少完成 Step 2。

### 練習題繳交

| 關卡 | 繳交內容 | 建議定位 |
| --- | --- | --- |
| Lv.1～Lv.2 | Tinkercad 連結＋情境思考題 | 課堂暖身，全班必做 |
| Lv.3～Lv.4 | ＋腳位規劃表 | 課後作業，必做 |
| Lv.5 | ＋腳位規劃表＋麵包板擺放截圖 | 選做加分 |
| Lv.6 BOSS | 完整顧問提案（需求分析、溫度規格、警報設計、腳位規劃、實作） | 可分組 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 電路正確性 | 35% | 每個 Step 的功能都能正常運作，TMP36 方向正確 |
| 腳位規劃 | 25% | 規劃表完整，`~` 腳分配合理，RGB 避開 3、11 |
| 情境思考題 | 25% | 尤其是溫度規格、警報處理方式與 💼 管理面向論述 |
| 麵包板擺放 | 15% | 分區清楚，多顆感測器不混在一起 |

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 30 分鐘 | 認識 TMP36、溫度區間概念、配線口訣、工具示範 |
| 第 1 節後段 | 專案一（全班一起） |
| 第 2 節 | 專案二、專案三（分組進行） |
| 第 3 節前段 | 專案四 |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，Lv.3 之後帶回家 |

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 溫度感測器 TMP36 | 依溫度輸出不同數值的三腳元件，有方向性、不用電阻 |
| `readC()` 溫度翻譯機 | 已寫好的函式，把 A 腳讀值換算成攝氏溫度 |
| `float` 浮點數 | 可以有小數點的數字，例如 24.85 |
| 警戒線／危險線 | 把溫度切成正常、警戒、危險三段的分界 |
| 靜音（Mute／Acknowledge） | 只關掉聲音，異常燈號仍保留 |
| 鎖定與人工復歸（Latch & Reset） | 警報觸發後不會自己停，需要有人確認且狀況恢復才能解除 |
| 上下緩衝（遲滯，Hysteresis） | 「開」和「關」用兩條不同的線，避免在邊界來回切換 |
| 溫差 | 兩個位置的溫度相減，用來判斷散熱或保溫效果 |

---

## 反思與延伸討論

1. 本週出現了三種警報處理方式：**自動恢復**（嬰兒房、溫室）、**靜音但保留燈號**（藥品冷藏櫃、冷鏈貨車）、**鎖定＋人工復歸**（機房）。請說明你怎麼判斷一個情境該用哪一種，並各舉一個本週以外的例子。
2. Lv.2 咖啡機和 Lv.3 冷氣都用了「兩條線」避免在邊界來回切換。你能在生活中找到其他採用這種設計的例子嗎？（提示：熱水瓶、冰箱、恆溫烤箱）
3. 本週每個情境的溫度範圍都是「別人給的」。如果客戶自己也不知道該訂多少度，身為顧問，你會透過哪些管道（法規、產業標準、供應商、歷史資料）找出合理的規格？

---

## 本週檢核清單

- [ ] 能正確接上 TMP36（平面朝自己：左 5V、中 A 腳、右 GND）
- [ ] 能用溫度讀值檢查器確認讀數與 Tinkercad 滑桿一致
- [ ] 能依情境訂出正常／警戒／危險三段溫度範圍
- [ ] 能分辨「自動恢復」「靜音」「鎖定＋人工復歸」三種警報處理方式
- [ ] 知道有蜂鳴器時，RGB 要避開 `~3` 和 `~11`；沒有蜂鳴器時可以使用
- [ ] 能在 `~` 腳不足時，改用多顆單色燈表達狀態
- [ ] 完成 4 個獨立專案，並各自繳交 Tinkercad 檔案
- [ ] 完成練習題 Lv.1～Lv.4，挑戰 Lv.5 與 Lv.6

---

## 下週預告

第 7 週我們要讓系統「動起來」——加入 **伺服馬達**，打造「機械保全閘門」。你會學到如何讓馬達轉到指定角度，並把前幾週學過的按鈕、旋鈕、光敏電阻、溫度感測器接上去，做出會自動開關的機械裝置。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 5 週](week-05.md) | [下一週：第 7 週 — 機械保全閘門 ➡](week-07.md)
