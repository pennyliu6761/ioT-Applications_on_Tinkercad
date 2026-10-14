[⬅ 回目錄](../README.md) | [⬅ 上一週：第 4 週](week-04.md) | [下一週：第 6 週 — 智慧溫控保險庫 ➡](week-06.md)

---

# 第 5 週：會發聲、會看光的智慧場域

## 主題專案：聽得到的警示，看得見的光線

> 🎯 **本週定位**：到目前為止，系統只會用「燈」跟人溝通，輸入也都要靠人動手（按、轉）。這一週加入兩個新元件：
>
> - **蜂鳴器**：讓系統「發出聲音」提醒人
> - **光敏電阻**：讓系統「自己感覺」環境變亮或變暗，不需要人操作
>
> 和第 4 週一樣，本週**重點不在寫程式**，而在於：
>
> 1. 讀懂情境需求，判斷需要哪些元件
> 2. 規劃腳位，在 Tinkercad 把電路一步步接起來
> 3. 想清楚感測器「要放在哪裡」才能真的偵測到該偵測的事
>
> 程式都已經準備好，你只需要填腳位、把功能開關從 `false` 改成 `true`。
>
> 🏁 **本週成果**：完成 **4 個獨立專案**，每個專案存成一個獨立的 Tinkercad 檔案繳交。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：蜂鳴器與光敏電阻](#認識新元件蜂鳴器與光敏電阻)
- [配線口訣卡](#配線口訣卡)
- [三個共用工具](#三個共用工具)
- [專案一：診所叫號服務鈴](#專案一診所叫號服務鈴)
- [專案二：社區智慧路燈](#專案二社區智慧路燈)
- [專案三：倉庫防盜光柵](#專案三倉庫防盜光柵)
- [專案四：產線計數與缺料警報](#專案四產線計數與缺料警報)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 冰箱門未關提醒](#-lv1-冰箱門未關提醒)
  - [Lv.2 辦公室盆栽光照監測](#-lv2-辦公室盆栽光照監測)
  - [Lv.3 雙車位停車偵測](#-lv3-雙車位停車偵測)
  - [Lv.4 貨車倒車雷達](#-lv4-貨車倒車雷達)
  - [Lv.5 智慧倉儲四區門禁](#-lv5-智慧倉儲四區門禁)
  - [🏆 Lv.6 BOSS 關：物聯網顧問提案](#-lv6-boss-關物聯網顧問提案)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～4 週）

- ✅ 單色 LED、RGB LED 的接法，每條燈線串一顆 220Ω 電阻
- ✅ 按鈕接法（一側 5V，另一側訊號線＋10kΩ 到 GND）
- ✅ 旋鈕接法（兩邊接電源，中間接 A 腳）
- ✅ 要調亮度、要混色的燈，接在 `~` 腳位；腳位不夠時借用 A 腳
- ✅ 用接線驗證器、旋鈕讀值檢查器確認接線

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 蜂鳴器（Piezo） | 讓系統發出「嗶」聲或簡單旋律 | 微波爐完成聲、倒車雷達、火災警報 |
| `tone()`／`noTone()` | 讓蜂鳴器發出某個音高／停止發聲 | 設定提示音的開關 |
| 光敏電阻（Photoresistor） | 會依照環境光線強弱，給出 0～1023 的數字 | 自動路燈、手機自動亮度、自動門光柵 |
| 「被遮住的那一瞬間」 | 只在狀態改變的那一刻記一次，而不是一直重複記 | 人流計數器、產線計件器 |

### 本週可用元件（只限這些）

| 元件 | 第幾週學的 |
| --- | --- |
| 單色 LED、220Ω 電阻 | 第 1 週 |
| 按鈕、10kΩ 電阻 | 第 2 週 |
| RGB LED（共陰極） | 第 3 週 |
| 旋鈕（Potentiometer） | 第 4 週 |
| 蜂鳴器（Piezo） | **本週新增** |
| 光敏電阻（Photoresistor） | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：顧問公司上週的四個案子都順利結案，客戶口碑不錯，這週又湧進四個新委託。這次的共同點是——**客戶希望系統能「自己察覺狀況」，並且「出聲提醒」**。
>
> | 客戶 | 委託內容 | 難度 |
> | --- | --- | --- |
> | 社區診所 | 叫號服務鈴 | ⭐ |
> | 社區管委會 | 智慧路燈 | ⭐⭐ |
> | 物流倉庫 | 防盜光柵警報 | ⭐⭐⭐ |
> | 食品工廠 | 產線計數與缺料警報 | ⭐⭐⭐⭐ |
>
> 每個專案都分成 3 個步驟，從最簡單的版本開始，每一步多接幾個元件、多一項功能。

---

## 認識新元件：蜂鳴器與光敏電阻

### 🔊 蜂鳴器（Piezo）

蜂鳴器有 **2 隻腳**，接法和單色 LED 很像，但**不需要電阻**：

| 蜂鳴器的腳 | 接到哪裡 |
| --- | --- |
| 正極（+，紅線） | 任何一隻數位腳位 |
| 負極（−，黑線） | GND |

- 蜂鳴器接在一般腳位就可以發聲，**不需要佔用 `~` 腳**。
- 程式用 `tone(腳位, 音高)` 讓它發聲，`noTone(腳位)` 讓它停止。音高數字越大，聲音越尖（例如 262 是 Do、523 是高八度的 Do）。

> ⚠️ **重要規則**：蜂鳴器發聲時，Arduino 內部會「借走」一部分計時資源，導致 **`~3` 和 `~11` 這兩隻腳的調光、混色失效**。所以本週只要同時用到蜂鳴器與 RGB LED，**RGB 請避開 3 和 11，從 5、6、9、10 裡面挑 3 隻**。

### 🌗 光敏電阻（Photoresistor）

光敏電阻有 **2 隻腳**，要搭配一顆 **10kΩ 電阻**，接法和第 2 週的按鈕幾乎一樣：

> **一腳接 5V，另一腳同時接「A 腳」和「10kΩ 到 GND」。**

```
  5V
   │
光敏電阻
   │
   ├──────── 接到 A0～A5
   │
10kΩ 電阻
   │
  GND
```

| 光線狀況 | 讀到的數字 |
| --- | --- |
| 很亮 | 數字大（接近 1023） |
| 很暗、被遮住 | 數字小（接近 0） |

- 在 Tinkercad 開始模擬後，**點一下光敏電阻**，會出現一條亮度滑桿，拖曳它就能模擬變亮或變暗。
- 每一顆光敏電阻都需要**自己的** 10kΩ 電阻和**自己的** A 腳。

### 🧭 三種「A 腳輸入元件」比一比

| 元件 | 誰來改變數值？ | 需要 10kΩ 嗎？ |
| --- | --- | --- |
| 按鈕（第 2 週） | 人按下 | 要 |
| 旋鈕（第 4 週） | 人轉動 | 不用 |
| 光敏電阻（本週） | **環境光線自己變** | 要 |

> 💡 旋鈕和光敏電阻讀到的都是 0～1023，所以在 Tinkercad 裡，你也可以先用旋鈕「假裝」是感測器測試程式，再換成光敏電阻。

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① 要調亮度、要混色 → 接 `~` 腳 | 只有 3、5、6、9、10、11 |
| ② **有蜂鳴器時，RGB 避開 3、11** | 從 5、6、9、10 裡挑 |
| ③ 只要亮或滅、蜂鳴器、按鈕 → 一般腳位就好 | 把 `~` 腳留給需要調光的燈 |
| ④ 每條燈線一顆 220Ω；蜂鳴器不用 | RGB LED 三個顏色腳各一顆 |
| ⑤ 光敏電阻：一腳 5V，一腳接 A 腳＋10kΩ 到 GND | 每顆都要自己的 10kΩ |
| ⑥ 旋鈕：兩邊接電源，中間接 A 腳 | 不用電阻 |
| ⑦ Pin 0、1 先不要用；腳位不夠時借 A 腳 | A 腳也能接燈、按鈕、蜂鳴器 |

---

## 三個共用工具

### 🔧 工具 1：類比讀值檢查器（旋鈕、光敏電阻都適用）

接好光敏電阻後，先用這支程式確認它有反應，並**記下這顆光敏電阻「最亮」和「最暗」各讀到多少**——後面設定門檻時會用到。

```cpp
int pinSensor = A0;   // ← 改成你接的 A 腳

void setup() {
  Serial.begin(9600);
}

void loop() {
  Serial.println(analogRead(pinSensor));
  delay(100);
}
```

**記錄表**：

| 光敏電阻 | 滑桿拉到最亮 | 滑桿拉到最暗 |
| --- | --- | --- |
| 第 1 顆（A?） | | |
| 第 2 顆（A?） | | |

### 🔧 工具 2：接線驗證器（沿用第 3、4 週）

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

### 🔧 工具 3：蜂鳴器測試器

接好蜂鳴器後，跑一次「Do Re Mi Fa Sol」，確認有聲音。（記得打開電腦喇叭）

```cpp
int pinBuzzer = 8;   // ← 改成你蜂鳴器接的腳位

int notes[] = { 262, 294, 330, 349, 392 };   // Do Re Mi Fa Sol

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
> 2. 把腳位填進程式的「設定區」
> 3. 照 Step 1 → 2 → 3 逐步接線，每完成一步就把對應的功能開關改成 `true`
> 4. 每一步都先用工具確認接線，再看功能是否正常

---

## 專案一：診所叫號服務鈴

⭐ **難度**：按鈕 ＋ 蜂鳴器，再逐步加燈和旋鈕

**客戶需求**：社區診所的櫃台要一顆叫號鈴。護理師按一下按鈕，就發出「叮咚」聲，叫號燈閃爍，同時在螢幕（Serial Monitor）顯示目前號碼。診所有三個診間，希望各診間的叮咚聲**音高不同**，病人一聽就知道是哪一間在叫號。

### Step 0：腳位規劃

| 元件 | 腳位 | 需要 `~` 腳嗎？為什麼？ |
| --- | --- | --- |
| 叫號按鈕 | | |
| 蜂鳴器 | | |
| 叫號燈（單色 LED） | | |
| 診間選擇旋鈕 | | — |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 按鈕＋蜂鳴器 | 按一下，發出「叮咚」，號碼加 1 | （不用改） |
| Step 2 | ＋1 顆單色 LED | 叮咚時叫號燈同步閃爍 | `useLight = true` |
| Step 3 | ＋1 顆旋鈕 | 旋鈕選診間，三間的叮咚音高不同 | `useKnob = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinCall   = ?;     // 叫號按鈕
int pinBuzzer = ?;     // 蜂鳴器
int pinLight  = ?;     // 叫號燈
int pinKnob   = A0;    // 診間選擇旋鈕
bool useLight = false; // Step 2 接好後改成 true
bool useKnob  = false; // Step 3 接好後改成 true
// ============================

int lastButton = LOW;
int number = 0;

// 叮咚：先高音再低音
void dingDong(int high) {
  if (useLight) digitalWrite(pinLight, HIGH);
  tone(pinBuzzer, high, 300);
  delay(350);
  if (useLight) digitalWrite(pinLight, LOW);
  tone(pinBuzzer, high * 4 / 5, 500);
  delay(550);
  if (useLight) {
    for (int k = 0; k < 2; k++) {
      digitalWrite(pinLight, HIGH); delay(150);
      digitalWrite(pinLight, LOW);  delay(150);
    }
  }
}

void setup() {
  pinMode(pinCall, INPUT);
  pinMode(pinLight, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int now = digitalRead(pinCall);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    number++;

    int room = 1;
    int high = 880;              // 預設音高
    if (useKnob) {
      room = map(analogRead(pinKnob), 0, 1024, 1, 4);   // 換算成 1、2、3 號診間
      if (room == 1) high = 660;
      else if (room == 2) high = 880;
      else high = 1047;
    }

    Serial.print("Now serving #");
    Serial.print(number);
    Serial.print("  -> Room ");
    Serial.println(room);

    dingDong(high);
  }
  lastButton = now;
}
```

### 情境思考

1. 蜂鳴器和叫號燈都只需要「開／關」，它們應該佔用 `~` 腳嗎？
2. 有一位聽力不好的長輩來看診。你的系統裡，哪一個設計是為了照顧這類病人？還可以怎麼加強？
3. 💼 很多醫院改用「螢幕叫號＋語音」而不是單純的叮咚聲。從病人體驗與營運成本兩個面向，比較兩種做法。

### ✅ 專案一驗收

- [ ] 按一下按鈕，發出叮咚聲，Serial Monitor 號碼加 1
- [ ] 叫號燈在叮咚時同步閃爍
- [ ] 旋鈕轉到不同位置，三個診間的音高明顯不同
- [ ] 腳位規劃表填寫完整

---

## 專案二：社區智慧路燈

⭐⭐ **難度**：第一次使用光敏電阻 ＋ 旋鈕調門檻 ＋ RGB 狀態燈

**客戶需求**：社區管委會想讓中庭路燈**天黑自動亮、天亮自動滅**，不用再請保全每天開關。不同季節天黑的程度不一樣，所以「多暗才開燈」要能讓管理員自己調整。管理室還想要一顆狀態燈，顯示現在是白天、黃昏還是夜間。

| 環境光線 | 路燈 | 管理室狀態燈 |
| --- | --- | --- |
| 白天（明顯比門檻亮） | 熄滅 | 綠 |
| 黃昏（接近門檻） | 熄滅 | 黃 |
| 夜間（低於門檻） | 點亮 | 藍 |

### Step 0：腳位規劃

| 元件 | 腳位 | 備註 |
| --- | --- | --- |
| 光敏電阻 | A1 | 記得搭配 10kΩ |
| 路燈（單色 LED） | | 只需亮或滅 |
| 門檻調整旋鈕 | | 要接哪一隻 A 腳？ |
| 狀態燈 RGB（R / G / B） | | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 光敏電阻（含 10kΩ）＋1 顆單色 LED | 拖曳光線滑桿，變暗時路燈亮起 | （不用改） |
| Step 2 | ＋1 顆旋鈕 | 轉旋鈕可以改變「多暗才開燈」 | `useKnob = true` |
| Step 3 | ＋1 顆 RGB LED | 狀態燈顯示白天／黃昏／夜間 | `useRGB = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinLDR  = A1;                   // 光敏電阻
int pinLamp = ?;                    // 路燈
int pinKnob = ?;                    // 門檻調整旋鈕（A 腳）
int pinR = ?, pinG = ?, pinB = ?;   // 管理室狀態燈 RGB
bool useKnob = false;               // Step 2 接好後改成 true
bool useRGB  = false;               // Step 3 接好後改成 true
// ============================

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinLamp, OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int light = analogRead(pinLDR);

  int threshold = 300;                 // 預設門檻
  if (useKnob) {
    threshold = analogRead(pinKnob);   // 旋鈕直接決定門檻（0~1023）
  }

  // 路燈：比門檻暗就亮
  digitalWrite(pinLamp, (light < threshold) ? HIGH : LOW);

  // 狀態燈
  if (useRGB) {
    if (light < threshold)            setColor(0, 0, 255);     // 夜間：藍
    else if (light < threshold + 200) setColor(255, 180, 0);   // 黃昏：黃
    else                              setColor(0, 255, 0);     // 白天：綠
  }

  Serial.print("Light: ");
  Serial.print(light);
  Serial.print("   Threshold: ");
  Serial.println(threshold);
  delay(200);
}
```

### 情境思考

1. 先用工具 1 測出你的光敏電阻「最亮／最暗」讀值。預設門檻 300 合理嗎？如果你的最暗值是 350，會發生什麼事？
2. 真實路燈上，光敏電阻如果裝在燈罩**正下方**，會被路燈自己的光照到。燈亮了 → 感測到很亮 → 燈熄了 → 又變暗 → 燈又亮……這種「一直閃」的問題，你會建議把光敏電阻裝在哪裡？
3. 💼 傍晚有一片烏雲飄過，天色暗了 30 秒，路燈就亮了又滅。從管委會的角度，這會造成什麼困擾？你會建議怎麼調整？

### ✅ 專案二驗收

- [ ] 拖曳光線滑桿，路燈在變暗時亮起、變亮時熄滅
- [ ] 旋鈕能改變開燈門檻（用 Serial Monitor 驗證 Threshold 有變化）
- [ ] 狀態燈正確顯示白天／黃昏／夜間三種顏色
- [ ] 光敏電阻有接 10kΩ 到 GND

---

## 專案三：倉庫防盜光柵

⭐⭐⭐ **難度**：光敏電阻 ＋ 蜂鳴器 ＋ RGB（要注意 3、11 規則）＋ 解除按鈕

**客戶需求**：物流倉庫的貴重品區入口，下班後要啟動「光柵」——一道光線照在光敏電阻上，有人經過擋住光線就觸發警報。保全室要知道今晚被觸發了幾次。而且警報**一旦響起就不能自己停**，必須由保全到現場確認後按「解除鈕」才會停止。

| 狀態 | 蜂鳴器 | 狀態燈 |
| --- | --- | --- |
| 警戒中（光線正常） | 安靜 | 綠 |
| 警報中（光線被擋或尚未解除） | 持續鳴叫 | 紅 |

### Step 0：腳位規劃

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| 光柵（光敏電阻） | A1 | — |
| 警報蜂鳴器 | | |
| 狀態燈 RGB（R / G / B） | | |
| 解除鈕 | | |

> 🤔 **規劃提示**：這一關同時有蜂鳴器和 RGB。回去看口訣 ②——RGB 的三隻腳能選哪些？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 光敏電阻＋蜂鳴器 | 光線被擋時警報響，Serial Monitor 記錄觸發次數 | （不用改） |
| Step 2 | ＋1 顆 RGB LED | 警戒綠燈、警報紅燈 | `useRGB = true` |
| Step 3 | ＋1 顆按鈕 | 警報會「鎖住」，要按解除鈕才停 | `useLatch = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinLDR    = A1;                 // 光柵（光敏電阻）
int pinBuzzer = ?;                  // 警報蜂鳴器
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈 RGB（避開 3、11）
int pinReset  = ?;                  // 解除鈕
int threshold = 300;                // 低於這個數字算「被擋住」（參考工具 1 的記錄調整）
bool useRGB   = false;              // Step 2 接好後改成 true
bool useLatch = false;              // Step 3 接好後改成 true
// ============================

bool wasBlocked = false;
bool alarmOn = false;
int breachCount = 0;
int lastButton = LOW;

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
  Serial.println("Guard mode ON");
}

void loop() {
  bool blocked = analogRead(pinLDR) < threshold;

  // 只在「剛被擋住的那一瞬間」記一次
  if (blocked && !wasBlocked) {
    breachCount++;
    Serial.print("BREACH! Count tonight: ");
    Serial.println(breachCount);
  }
  wasBlocked = blocked;

  if (useLatch) {
    if (blocked) alarmOn = true;               // 被擋過就鎖住
    int now = digitalRead(pinReset);
    if (now == HIGH && lastButton == LOW) {
      delay(50);
      if (!blocked) {
        alarmOn = false;                       // 光線恢復才允許解除
        Serial.println("Alarm cleared by guard");
      } else {
        Serial.println("Cannot clear: beam still blocked");
      }
    }
    lastButton = now;
  } else {
    alarmOn = blocked;                         // 沒有鎖住：光恢復就停
  }

  if (alarmOn) {
    tone(pinBuzzer, 1500);
    if (useRGB) setColor(255, 0, 0);
  } else {
    noTone(pinBuzzer);
    if (useRGB) setColor(0, 255, 0);
  }
  delay(50);
}
```

### 情境思考

1. 把 RGB 的藍色或紅色腳故意接到 `~11` 或 `~3`，觀察警報響起時燈號有什麼異常，再改回來。把你觀察到的現象寫下來。
2. 為什麼要設計成「光線還被擋著時，不准解除警報」？
3. 💼 如果倉庫裡有一隻貓每晚經過光柵三次，保全每晚都要跑三趟。你會建議客戶怎麼改善？（提示：光柵的高度、門檻、或觸發幾次才通報）

### ✅ 專案三驗收

- [ ] 擋住光線時警報響起，Serial Monitor 的觸發次數**只加 1**（不會一直跳）
- [ ] 狀態燈警戒綠、警報紅，且 RGB 沒有接在 3、11
- [ ] 警報響起後，即使光線恢復也持續響，按解除鈕才停
- [ ] 光線還被擋住時按解除鈕，警報不會停

---

## 專案四：產線計數與缺料警報

⭐⭐⭐⭐ **難度**：**2 顆光敏電阻** ＋ 長條燈 ＋ RGB ＋ 蜂鳴器 ＋ 按鈕

**客戶需求**：食品工廠的包裝線，每箱產品經過輸送帶上的光柵時要**自動計數**，每滿 5 箱算一批，批次進度用 5 顆燈顯示，滿一批時播放提示音。另外，原料箱底部也裝一顆光敏電阻：原料用完、箱底露出來照到光，就要亮紅燈並發出警告聲，提醒補料。班長交班時按一下按鈕，計數歸零。

| 區域 | 元件 | 功能 |
| --- | --- | --- |
| 輸送帶光柵 | 光敏電阻＋10kΩ | 每箱經過擋光一次 → 計數 +1 |
| 批次進度 | 5 顆單色 LED | 這一批已經完成幾箱 |
| 原料箱偵測 | 光敏電阻＋10kΩ | 箱底照到光 → 缺料 |
| 產線狀態 | RGB LED | 正常綠、缺料紅 |
| 提示音 | 蜂鳴器 | 計數嗶一聲、滿批旋律、缺料低音 |
| 交班歸零 | 按鈕 | 計數歸零 |

### Step 0：腳位規劃（本專案的重頭戲）

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| 輸送帶光柵 | A0 | — |
| 原料箱光敏電阻 | | — |
| 批次燈 1～5 | | |
| 產線狀態 RGB（R / G / B） | | |
| 蜂鳴器 | | |
| 交班歸零鈕 | | |

> 🤔 **規劃提示**：
> - 兩顆光敏電阻各需要一顆 10kΩ，在麵包板上怎麼擺，才不會兩組線混在一起？
> - RGB 要避開 3、11，剩下的 `~` 腳夠嗎？5 顆批次燈還能接 `~` 腳嗎？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 輸送帶光敏電阻＋5 顆 LED＋蜂鳴器 | 每擋光一次嗶一聲、亮一格，滿 5 格播旋律 | （不用改） |
| Step 2 | ＋原料箱光敏電阻＋RGB | 缺料時亮紅燈並發出低音警告 | `useBin = true` |
| Step 3 | ＋1 顆按鈕 | 按下交班鈕，計數歸零 | `useReset = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int ldrLine = A0;                   // 輸送帶光柵
int ldrBin  = ?;                    // 原料箱底部光敏電阻（A 腳）
int bar[5] = { ?, ?, ?, ?, ? };     // 批次進度燈
int pinR = ?, pinG = ?, pinB = ?;   // 產線狀態 RGB（避開 3、11）
int pinBuzzer = ?;                  // 蜂鳴器
int pinReset  = ?;                  // 交班歸零鈕
int threshold = 300;                // 低於這個數字算「被擋住」
bool useBin   = false;              // Step 2 接好後改成 true
bool useReset = false;              // Step 3 接好後改成 true
// ============================

bool wasBlocked = false;
bool wasEmpty = false;
int count = 0;
int lastButton = LOW;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void batchMelody() {
  int notes[] = { 523, 659, 784 };   // Do Mi Sol
  for (int i = 0; i < 3; i++) {
    tone(pinBuzzer, notes[i], 150);
    delay(200);
  }
}

void setup() {
  for (int i = 0; i < 5; i++) pinMode(bar[i], OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(pinReset, INPUT);
  Serial.begin(9600);
}

void loop() {
  // ---- 輸送帶計數：只在「剛被擋住」時算一箱 ----
  bool blocked = analogRead(ldrLine) < threshold;
  if (blocked && !wasBlocked) {
    count++;
    Serial.print("Box #");
    Serial.println(count);
    if (count % 5 == 0) {
      Serial.println("=== Batch complete ===");
      batchMelody();
    } else {
      tone(pinBuzzer, 2000, 80);
    }
  }
  wasBlocked = blocked;

  // ---- 批次進度燈 ----
  int inBatch = count % 5;
  if (count > 0 && inBatch == 0) inBatch = 5;   // 剛滿一批時顯示 5 格
  for (int i = 0; i < 5; i++) {
    digitalWrite(bar[i], (i < inBatch) ? HIGH : LOW);
  }

  // ---- 原料箱缺料偵測：箱底照到光 = 原料用完 ----
  if (useBin) {
    bool empty = analogRead(ldrBin) > threshold;
    if (empty && !wasEmpty) {
      Serial.println("WARNING: Material bin empty!");
      tone(pinBuzzer, 300, 600);
    }
    wasEmpty = empty;
    if (empty) setColor(255, 0, 0);
    else       setColor(0, 255, 0);
  }

  // ---- 交班歸零 ----
  if (useReset) {
    int now = digitalRead(pinReset);
    if (now == HIGH && lastButton == LOW) {
      delay(50);
      Serial.print("Shift total: ");
      Serial.println(count);
      count = 0;
      Serial.println("Counter reset");
    }
    lastButton = now;
  }

  delay(30);
}
```

### 情境思考

1. 兩顆光敏電阻的判斷剛好相反：輸送帶是「變暗＝有東西」，原料箱是「變亮＝沒東西」。為什麼？請用真實擺放位置說明。
2. 測試時發現擋住原料箱的光敏電阻，卻是計數在增加。可能是哪裡接錯了？用哪個工具可以快速確認？
3. 💼 如果兩箱產品在輸送帶上**貼得太近**、中間沒有空隙，光柵會把它們算成幾箱？這對庫存與出貨數量會造成什麼問題？你會建議產線怎麼調整？

### ✅ 專案四驗收

- [ ] 每擋一次輸送帶光柵，計數只加 1，批次燈亮一格
- [ ] 滿 5 箱時播放提示旋律
- [ ] 原料箱變亮時狀態燈轉紅並發出低音警告，恢復後轉綠
- [ ] 交班鈕能將計數歸零，並印出本班總數
- [ ] RGB 沒有接在 3、11；兩組光敏電阻在麵包板上分區清楚

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：四個範例專案是「跟著做」，這一區換你「自己規劃」。每一關都是新情境、新的元件組合，你要先判斷需要什麼元件、接幾顆、接哪隻腳，**還要想清楚感測器擺在哪裡**，再動手接線。
>
> 📈 **難度安排**：
>
> | 關卡 | 光敏電阻 | 旋鈕 | 單色 LED | RGB | 蜂鳴器 | 按鈕 | 配線重點 |
> | --- | --- | --- | --- | --- | --- | --- | --- |
> | Lv.1 | 1 | — | 1 | — | 1 | — | 感測器方向反過來（變亮才觸發） |
> | Lv.2 | 1 | — | — | 1 | 1 | — | RGB 避開 3、11 |
> | Lv.3 | 2 | — | 4 | — | 1 | — | 兩組光敏電阻分區 |
> | Lv.4 | — | 1 | 5 | 1 | 1 | — | 用旋鈕模擬沒學過的感測器 |
> | Lv.5 | 4 | 1 | 4 | 1 | 1 | 1 | A 腳幾乎用完、4 顆 10kΩ |
> | Lv.6 | 自訂 | 自訂 | 自訂 | 自訂 | 自訂 | 自訂 | 顧問提案 |
>
> 📌 **作答規則**：每一關先填腳位規劃表，再接線；接好後先用三個共用工具檢查，最後才上傳程式。程式只需要改「設定區」。

---

### 🟢 Lv.1 冰箱門未關提醒

**情境**：家電公司的新款冰箱，門打開時冰箱內燈要亮；如果門開著超過約 5 秒，就要「嗶嗶」提醒使用者關門。冰箱裡面平常是暗的，門一打開，外面的光就照進來。

**元件**：1 顆光敏電阻＋1 顆單色 LED（冰箱內燈）＋1 顆蜂鳴器

**你要想的問題**：

- 這一關是「**變亮**」才觸發，跟專案三的光柵剛好相反。程式要怎麼判斷？（看程式裡的 `>`）
- 光敏電阻要裝在冰箱的哪裡？如果裝在內燈旁邊，會發生什麼事？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinLDR    = A0;   // 冰箱內的光敏電阻
int pinLight  = ?;    // 冰箱內燈
int pinBuzzer = ?;    // 提醒蜂鳴器
int threshold = 500;  // 高於這個數字算「門打開」
// ============================

int openCount = 0;

void setup() {
  pinMode(pinLight, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  bool doorOpen = analogRead(pinLDR) > threshold;
  digitalWrite(pinLight, doorOpen ? HIGH : LOW);

  if (doorOpen) {
    openCount++;                      // 每 0.1 秒加 1
    if (openCount > 50) {             // 超過約 5 秒
      tone(pinBuzzer, 1000, 100);
      Serial.println("Please close the door!");
    }
  } else {
    openCount = 0;
  }
  delay(100);
}
```

**情境思考**：

1. 如果冰箱放在很暗的廚房，晚上開門時光線不夠，系統會判斷錯誤嗎？你會怎麼調整 `threshold`？
2. 💼 為什麼要等 5 秒才響，而不是一開門就響？從使用者體驗的角度說明。

---

### 🟢 Lv.2 辦公室盆栽光照監測

**情境**：公司在辦公室擺了很多綠色植物，總務想知道每盆植物的光照夠不夠。每盆放一個小監測器：一顆光敏電阻偵測光線、一顆 RGB 燈顯示狀態；光照不足時，蜂鳴器每隔幾秒輕輕嗶一聲提醒總務移盆。

| 光照狀況 | RGB 燈 | 蜂鳴器 |
| --- | --- | --- |
| 太暗 | 紫 | 每 2 秒嗶一聲 |
| 適中 | 綠 | 安靜 |
| 太強（日曬） | 黃 | 安靜 |

**元件**：1 顆光敏電阻＋1 顆 RGB LED＋1 顆蜂鳴器

**你要想的問題**：

- 這一關同時有 RGB 和蜂鳴器，RGB 三隻腳你選了哪些？為什麼？
- 先依照口訣接好，確認正常後，再**故意**把 RGB 的**藍色腳**改接到 `~11`（設定區的 `pinB` 也要改成 11），觀察「太暗」時紫色燈號在嗶聲響起時有什麼變化。

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinLDR    = A0;                 // 盆栽光敏電阻
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈 RGB
int pinBuzzer = ?;                  // 提醒蜂鳴器
int darkLine   = 300;               // 低於這個數字算「太暗」
int brightLine = 800;               // 高於這個數字算「太強」
// ============================

int tick = 0;

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
  int light = analogRead(pinLDR);
  Serial.println(light);

  if (light < darkLine) {
    setColor(180, 0, 120);                       // 太暗：紫
    tick++;
    if (tick >= 20) {                            // 約每 2 秒
      tone(pinBuzzer, 800, 100);
      tick = 0;
    }
  } else if (light > brightLine) {
    setColor(255, 180, 0);                       // 太強：黃
    tick = 0;
  } else {
    setColor(0, 255, 0);                         // 適中：綠
    tick = 0;
  }
  delay(100);
}
```

**情境思考**：

1. 你觀察到接 `~11` 時的異常是什麼？為什麼「適中」和「太強」時看不出問題，只有「太暗」時才出現？（提示：哪個狀態下蜂鳴器會響？）
2. 💼 辦公室有 30 盆植物，每盆都裝一個這樣的監測器，總務每天會被嗶幾十次。你會建議怎麼改，讓提醒更有效率？

---

### 🟡 Lv.3 雙車位停車偵測

**情境**：社區地下室有兩個訪客車位。每格地面裝一顆光敏電阻，車子停上去就會擋住光線。每格上方各有一紅一綠兩顆燈：空位亮綠、有車亮紅。有車**剛停進來**的那一刻，蜂鳴器嗶一聲，管理室也會收到訊息。

**元件**：2 顆光敏電阻＋4 顆單色 LED（每格一紅一綠）＋1 顆蜂鳴器

**Step 0：腳位規劃**

| 元件 | 腳位 |
| --- | --- |
| 車位 1 光敏電阻 | A0 |
| 車位 2 光敏電阻 | |
| 車位 1 紅燈／綠燈 | |
| 車位 2 紅燈／綠燈 | |
| 蜂鳴器 | |

**你要想的問題**：

- 兩顆光敏電阻各需要一顆 10kΩ，你在麵包板上怎麼排，讓「車位 1」和「車位 2」的元件一眼分得出來？
- 4 顆燈和蜂鳴器都只需要開關，你有沒有不小心把 `~` 腳用掉？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int ldr[2]   = { A0, ? };       // 車位 1、車位 2 光敏電阻
int red[2]   = { ?, ? };        // 車位 1、車位 2 紅燈
int green[2] = { ?, ? };        // 車位 1、車位 2 綠燈
int pinBuzzer = ?;              // 蜂鳴器
int threshold = 300;            // 低於這個數字算「有車」
// ============================

bool wasParked[2] = { false, false };

void setup() {
  for (int s = 0; s < 2; s++) {
    pinMode(red[s], OUTPUT);
    pinMode(green[s], OUTPUT);
  }
  Serial.begin(9600);
}

void loop() {
  int freeCount = 0;

  for (int s = 0; s < 2; s++) {
    bool parked = analogRead(ldr[s]) < threshold;

    if (parked && !wasParked[s]) {          // 剛停進來
      tone(pinBuzzer, 1200, 150);
      Serial.print("Car parked at spot ");
      Serial.println(s + 1);
    }
    if (!parked && wasParked[s]) {          // 剛開走
      Serial.print("Spot ");
      Serial.print(s + 1);
      Serial.println(" is free");
    }
    wasParked[s] = parked;

    digitalWrite(red[s],   parked ? HIGH : LOW);
    digitalWrite(green[s], parked ? LOW : HIGH);
    if (!parked) freeCount++;
  }

  delay(100);
}
```

**情境思考**：

1. 測試時發現車位 2 停車，卻是車位 1 的紅燈亮。這是電路問題還是程式設定問題？怎麼用工具 1 快速確認？
2. 地下室燈光整晚都開著，但某天停電改用緊急照明，光線變很暗。系統會發生什麼事？
3. 💼 如果要把這個系統擴充到 50 個車位，每格都用一顆光敏電阻和一個 A 腳，可行嗎？你會怎麼跟客戶說明限制？

---

### 🟡 Lv.4 貨車倒車雷達

**情境**：物流公司想幫舊款貨車加裝倒車輔助。真正的距離感測器我們還沒學到，所以先用**旋鈕模擬「與後方障礙物的距離」**。越靠近障礙物，嗶聲越急促、長條燈亮越多格、RGB 從綠變黃再變紅。

| 距離 | 嗶聲 | 長條燈 | RGB |
| --- | --- | --- | --- |
| 遠 | 間隔長 | 亮 1 格 | 綠 |
| 中 | 間隔中等 | 亮 2～3 格 | 黃 |
| 很近 | 幾乎連續 | 亮 5 格 | 紅 |

**元件**：1 顆旋鈕＋5 顆單色 LED＋1 顆 RGB LED＋1 顆蜂鳴器

**你要想的問題**：

- 這一關為什麼用旋鈕、不用光敏電阻來模擬距離？用光敏電阻可以嗎？
- RGB 要避開 3、11，那 5 顆長條燈可以接在 3 和 11 嗎？會有問題嗎？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinKnob = A0;                   // 模擬距離的旋鈕
int bar[5] = { ?, ?, ?, ?, ? };     // 距離長條燈（由遠到近）
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈 RGB（避開 3、11）
int pinBuzzer = ?;                  // 倒車蜂鳴器
// ============================

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  for (int i = 0; i < 5; i++) pinMode(bar[i], OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // 旋鈕 0~1023 換算成距離 200~10 公分（轉越多越近）
  int distance = map(analogRead(pinKnob), 0, 1023, 200, 10);

  // 越近亮越多格
  int lit = map(distance, 200, 10, 1, 5);
  for (int i = 0; i < 5; i++) {
    digitalWrite(bar[i], (i < lit) ? HIGH : LOW);
  }

  if (distance > 120)     setColor(0, 255, 0);     // 遠：綠
  else if (distance > 40) setColor(255, 180, 0);   // 中：黃
  else                    setColor(255, 0, 0);     // 近：紅

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // 越近嗶得越急
  int gap = map(distance, 200, 10, 800, 60);
  tone(pinBuzzer, 1000, 50);
  delay(gap);
}
```

**情境思考**：

1. 駕駛專心倒車時通常看後照鏡，不會看儀表板。在這個系統裡，哪一種輸出（嗶聲、長條燈、RGB）對駕駛最有用？為什麼還需要其他兩種？
2. 💼 這套加裝系統每台成本 800 元，車隊有 60 台車。你會用什麼理由說服物流公司老闆投資？（提示：一次擦撞的維修費、保險、停工時間）

---

### 🔴 Lv.5 智慧倉儲四區門禁

**情境**：大型倉庫分成四個區域：進貨區、出貨區、冷藏區、貴重品區。每區入口各有一道光柵（光敏電阻）。任何一區被觸發，該區指示燈亮起並**鎖住**，總狀態燈轉紅、蜂鳴器響，直到保全按下解除鈕。因為白天和晚上倉庫照明不同，光柵的**靈敏度要能用旋鈕統一調整**。

| 區域 | 元件 |
| --- | --- |
| 四區光柵 | 4 顆光敏電阻（各配 10kΩ） |
| 靈敏度調整 | 1 顆旋鈕 |
| 四區指示燈 | 4 顆單色 LED |
| 總狀態 | 1 顆 RGB LED |
| 警報 | 1 顆蜂鳴器 |
| 解除 | 1 顆按鈕 |

**Step 0：腳位規劃（本關重點）**

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 4 顆光敏電阻 | 4（A 腳） | |
| 靈敏度旋鈕 | 1（A 腳） | |
| **A 腳合計／可用** | ／ 6 | |
| 4 顆區域指示燈 | | |
| 總狀態 RGB | | |
| 蜂鳴器 | | |
| 解除鈕 | | |
| **一般腳位合計／可用** | ／ 12 | |

> 🤔 **規劃提示**：
> - 這一關的壓力不在一般腳位，而在 **A 腳**和**麵包板空間**——4 顆 10kΩ、1 顆旋鈕、5 組都要接 5V 和 GND。
> - 你打算怎麼利用麵包板上下兩條電源軌？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int ldr[4]     = { A0, ?, ?, ? };   // 進貨、出貨、冷藏、貴重品 光柵
int pinKnob    = ?;                 // 靈敏度旋鈕（A 腳）
int zoneLed[4] = { ?, ?, ?, ? };    // 四區指示燈
int pinR = ?, pinG = ?, pinB = ?;   // 總狀態 RGB（避開 3、11）
int pinBuzzer  = ?;                 // 警報蜂鳴器
int pinReset   = ?;                 // 解除鈕
// ============================

bool tripped[4] = { false, false, false, false };
int lastButton = LOW;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  for (int z = 0; z < 4; z++) pinMode(zoneLed[z], OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(pinReset, INPUT);
  Serial.begin(9600);
}

void loop() {
  // 靈敏度：旋鈕換算成門檻 100~800
  int threshold = map(analogRead(pinKnob), 0, 1023, 100, 800);

  bool anyAlarm = false;
  for (int z = 0; z < 4; z++) {
    if (analogRead(ldr[z]) < threshold && !tripped[z]) {
      tripped[z] = true;                   // 觸發後鎖住
      Serial.print("ZONE ");
      Serial.print(z + 1);
      Serial.println(" BREACH");
    }
    digitalWrite(zoneLed[z], tripped[z] ? HIGH : LOW);
    if (tripped[z]) anyAlarm = true;
  }

  if (anyAlarm) {
    setColor(255, 0, 0);
    tone(pinBuzzer, 1500);
  } else {
    setColor(0, 255, 0);
    noTone(pinBuzzer);
  }

  // 解除鈕：清除所有區域的鎖定
  int now = digitalRead(pinReset);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    for (int z = 0; z < 4; z++) tripped[z] = false;
    Serial.println("All zones cleared");
  }
  lastButton = now;

  delay(50);
}
```

**情境思考**：

1. 四區指示燈在麵包板上怎麼排，保全才看得出「是哪一區」被觸發？請截圖說明。
2. 這一關用了幾隻 A 腳？如果客戶要加第五區，還能加嗎？加第六區呢？
3. 💼 目前四區共用一個靈敏度旋鈕。但冷藏區的照明特別暗，常常誤觸發。你會建議怎麼改？這會讓腳位規劃發生什麼變化？

---

### 🏆 Lv.6 BOSS 關：物聯網顧問提案

**情境**：你的顧問公司接到新客戶。從下面三個案例中**任選一個**，自己完成需求分析、元件選擇、腳位規劃與 Tinkercad 實作。

> **案例 A：智慧教室**
> 教室窗邊的光敏電阻偵測自然光，光線不足時自動開燈（燈的亮度依照暗的程度調整）。老師有一顆按鈕可以切換「上課／下課」，下課時播放下課鈴旋律並關燈。另有一顆 RGB 燈顯示教室狀態。
>
> **案例 B：超商自動門迎賓**
> 門口光柵偵測客人進門，每位客人進門播放一段迎賓音，並在 Serial Monitor 記錄來客數。店長用旋鈕設定「同時在店人數上限」，超過時 RGB 轉紅並提醒。打烊時按按鈕，印出今日來客總數並歸零。
>
> **案例 C：自訂情境**
> 自己找一個生活或職場情境（例如：會議室使用中偵測、藥櫃開啟警示、寵物餵食提醒、圖書館閉館提示）。

**最低規格**：

- 至少 1 顆光敏電阻、1 顆蜂鳴器、1 顆 RGB LED、1 顆按鈕
- 總共使用 10 隻以上腳位
- 程式可以從專案一～四或 Lv.1～Lv.5 改寫，說明你是從哪裡改來的、改了哪些設定

**繳交內容**：

1. **需求分析**：這個系統解決什麼問題？誰會操作？誰會看燈、聽聲音？（3～5 句）
2. **感測器擺放說明**：光敏電阻在真實場地中要裝在哪裡？為什麼？
3. **腳位規劃表**：標註每隻腳接什麼、是否為 `~` 腳、還剩幾隻可擴充
4. **Tinkercad 實作**：電路截圖＋三個共用工具的檢查截圖
5. **擴充建議**：如果客戶明年想再加一個功能，你的配置還有空間嗎？

**驗收標準**：

- [ ] 需要漸變或混色的燈接在 `~` 腳，且 RGB 避開 3、11
- [ ] 每條燈線都有 220Ω，每顆按鈕與光敏電阻都有 10kΩ，蜂鳴器與旋鈕沒有多接電阻
- [ ] 計數類功能「只在狀態改變的瞬間」計一次
- [ ] 麵包板依功能分區
- [ ] 功能符合情境需求

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 蜂鳴器完全沒聲音 | 正極是否接到程式裡的腳位、負極是否接 GND；電腦喇叭是否打開 | 用工具 3 測試 |
| 蜂鳴器一直響停不下來 | 程式的功能開關、門檻設定 | 用工具 1 看讀值是否一直低於門檻 |
| 光敏電阻讀值一直是 0 或 1023 | 10kΩ 是否接到 GND、A 腳是否接在光敏電阻與 10kΩ 的交界處 | 照「一腳 5V，另一腳接 A 腳＋10kΩ 到 GND」重接 |
| 拖曳光線滑桿，讀值幾乎不變 | 是否點對了光敏電阻；A 腳編號是否和程式一致 | 用工具 1 逐一測 A0～A5 |
| 警報響時 RGB 顏色怪怪的 | RGB 是否接在 3 或 11 | 改接 5、6、9、10 |
| 擋一次光，計數卻一直跳 | 是否開啟了正確的功能、門檻是否剛好卡在讀值附近 | 依工具 1 記錄調整門檻，離最亮、最暗都有距離 |
| 擋 A 感測器，卻是 B 的功能在動 | 程式裡的 A 腳順序和實際接線是否一致 | 用工具 1 確認後，調整設定區或接線 |
| 程式無法上傳，出現錯誤訊息 | 設定區是否還有 `?` 沒填 | 所有腳位都要先填，包含還沒接的元件 |

---

## 成果繳交與評分

### 範例專案繳交

每個專案各自建立一個 Tinkercad 檔案，命名格式：`W5-專案編號-學號`（例如 `W5-P3-B12345678`）。

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
| Lv.6 BOSS | 完整顧問提案（需求分析、感測器擺放、腳位規劃、實作、擴充建議） | 可分組 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 電路正確性 | 35% | 每個 Step 的功能都能正常運作 |
| 腳位規劃 | 25% | 規劃表完整，`~` 腳分配合理，RGB 避開 3、11 |
| 情境思考題 | 25% | 尤其是感測器擺放與 💼 管理面向題目的論述 |
| 麵包板擺放 | 15% | 分區清楚，多組感測器不混在一起 |

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 30 分鐘 | 認識蜂鳴器與光敏電阻、配線口訣、三個工具示範 |
| 第 1 節後段 | 專案一（全班一起） |
| 第 2 節 | 專案二、專案三（分組進行） |
| 第 3 節前段 | 專案四 |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，Lv.3 之後帶回家 |

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 蜂鳴器（Piezo Buzzer） | 能發出嗶聲或簡單旋律的輸出元件，接一般腳位、不用電阻 |
| `tone()`／`noTone()` | 讓蜂鳴器以某個音高發聲／停止發聲 |
| 音高（頻率，Hz） | 數字越大聲音越尖，例如 262 是 Do、523 是高音 Do |
| 光敏電阻（Photoresistor, LDR） | 依環境亮暗給出 0～1023 數值的感測器，要搭配 10kΩ |
| 光柵 | 一道光照在感測器上，被擋住就代表有人或物體經過 |
| 門檻（Threshold） | 判斷「亮／暗」或「有／沒有」的分界數字 |
| 狀態改變偵測 | 只在「剛被擋住」那一瞬間做一次動作，避免重複計數 |
| 警報鎖定（Latch） | 警報觸發後不會自己停，必須有人確認解除 |

---

## 反思與延伸討論

1. 本週同一種光敏電阻，在不同情境裡有時「變暗＝有事」（光柵、停車格），有時「變亮＝有事」（冰箱門、原料箱）。決定方向的關鍵是什麼？以後遇到新感測器時，你會先問自己哪些問題？
2. 專案三和 Lv.5 都採用「警報鎖定，必須人工解除」的設計，專案二的路燈卻是「自動恢復」。什麼情境適合自動恢復？什麼情境一定要人工確認？請各舉一個本週以外的例子。
3. 本週好幾題都遇到「誤觸發」：貓經過光柵、烏雲讓路燈亮、緊急照明讓停車格誤判。誤觸發太多，使用者會開始「忽略警報」。從管理的角度，這個現象會帶來什麼風險？你會怎麼在「漏報」和「誤報」之間取得平衡？

---

## 本週檢核清單

- [ ] 能正確接上蜂鳴器（正極接數位腳、負極接 GND、不用電阻）
- [ ] 能正確接上光敏電阻（一腳 5V，另一腳接 A 腳＋10kΩ 到 GND）
- [ ] 能用類比讀值檢查器測出光敏電阻的最亮／最暗讀值，並據此設定門檻
- [ ] 知道有蜂鳴器時，RGB 要避開 `~3` 和 `~11`
- [ ] 能判斷情境是「變暗觸發」還是「變亮觸發」
- [ ] 能說明感測器在真實場地中應該擺放的位置與理由
- [ ] 完成 4 個獨立專案，並各自繳交 Tinkercad 檔案
- [ ] 完成練習題 Lv.1～Lv.4，挑戰 Lv.5 與 Lv.6

---

## 下週預告

第 6 週我們要加入 **TMP36 溫度感測器**，打造「智慧溫控保險庫」——玩家必須讓房間溫度維持在指定範圍內才能開啟保險庫。你會學到如何把感測器的原始數值換算成真正的物理單位（°C），並延續本週的「警報鎖定、人工解除」設計。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 4 週](week-04.md) | [下一週：第 6 週 — 智慧溫控保險庫 ➡](week-06.md)
