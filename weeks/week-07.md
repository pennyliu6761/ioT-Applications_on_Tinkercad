[⬅ 回目錄](../README.md) | [⬅ 上一週：第 6 週](week-06.md) | [下一週：第 8 週 — 期中專題 ➡](week-08.md)

---

# 第 7 週：會動的智慧場域

## 主題專案：讓系統從「提醒」變成「動手」

> 🎯 **本週定位**：前六週的系統只會「亮燈、出聲」提醒人，真正的動作還是要靠人去做。這一週加入 **伺服馬達（Servo）**，系統第一次可以自己「動手」——開柵欄、轉天窗、推擋板、關門。
>
> 和第 4～6 週一樣，本週**重點不在寫程式**，而在於：
>
> 1. 讀懂情境需求，判斷需要哪些元件
> 2. 規劃腳位，在 Tinkercad 把電路一步步接起來
> 3. 想清楚「馬達要轉到幾度」「什麼情況下**不准**動」
>
> 控制馬達的程式都已經準備好，你只需要填腳位、調整角度、把功能開關從 `false` 改成 `true`。
>
> 🏁 **本週成果**：完成 **4 個獨立專案**，每個專案存成一個獨立的 Tinkercad 檔案繳交。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：伺服馬達](#認識新元件伺服馬達)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [專案一：停車場出入口柵欄](#專案一停車場出入口柵欄)
- [專案二：溫室自動天窗](#專案二溫室自動天窗)
- [專案三：展場自動門與人流管制](#專案三展場自動門與人流管制)
- [專案四：物流包裹分揀站](#專案四物流包裹分揀站)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 寵物自動餵食器](#-lv1-寵物自動餵食器)
  - [Lv.2 辦公室自動窗簾](#-lv2-辦公室自動窗簾)
  - [Lv.3 太陽能板追光支架](#-lv3-太陽能板追光支架)
  - [Lv.4 旅館保險箱密碼鎖](#-lv4-旅館保險箱密碼鎖)
  - [Lv.5 機械手臂夾取工作站](#-lv5-機械手臂夾取工作站)
  - [🏆 Lv.6 BOSS 關：物聯網顧問提案](#-lv6-boss-關物聯網顧問提案)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～6 週）

- ✅ 單色 LED、RGB LED、蜂鳴器的接法
- ✅ 按鈕、旋鈕、光敏電阻、TMP36 四種輸入元件
- ✅ 有蜂鳴器時 RGB 避開 3、11；腳位不夠時借 A 腳
- ✅ 狀態改變偵測、警報鎖定、人工復歸

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 伺服馬達（Servo） | 可以轉到指定角度（0～180°）並停住的馬達 | 停車場柵欄、自動門、機械手臂關節 |
| `Servo` 遙控器 | 程式裡的一個「遙控器」，綁定腳位後下指令「轉到幾度」 | 遙控器配對電視 |
| 平滑轉動 `sweepTo()` | 一度一度慢慢轉，而不是瞬間跳過去 | 柵欄緩緩升起，避免打到人 |
| 安全連鎖（Interlock） | 某個條件不安全時，馬達**不准動** | 微波爐門沒關不能加熱、電梯門沒關不能開動 |

### 本週可用元件（只限這些）

| 元件 | 第幾週學的 |
| --- | --- |
| 單色 LED、220Ω 電阻 | 第 1 週 |
| 按鈕、10kΩ 電阻 | 第 2 週 |
| RGB LED（共陰極） | 第 3 週 |
| 旋鈕（Potentiometer） | 第 4 週 |
| 蜂鳴器、光敏電阻 | 第 5 週 |
| 溫度感測器 TMP36 | 第 6 週 |
| 伺服馬達（Micro Servo） | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：顧問公司做了六週的「監控與提醒」系統，客戶開始問：「既然系統都知道該做什麼了，能不能直接幫我們做？」這週的四個委託案，都需要系統**自己動手**。
>
> | 客戶 | 委託內容 | 難度 |
> | --- | --- | --- |
> | 購物中心 | 停車場出入口柵欄 | ⭐ |
> | 有機農場 | 溫室自動天窗 | ⭐⭐ |
> | 展覽館 | 自動門與人流管制 | ⭐⭐⭐ |
> | 物流中心 | 包裹分揀站 | ⭐⭐⭐⭐ |
>
> 每個專案都分成 3 個步驟，從最簡單的版本開始，每一步多接幾個元件、多一項功能。

---

## 認識新元件：伺服馬達

### 外觀與接法

伺服馬達有 **3 條線**，靠**顏色**分辨，不需要電阻：

| 線的顏色 | 名稱 | 接到哪裡 |
| --- | --- | --- |
| 橘色（或黃色） | 訊號 | 任何一隻數位腳位 |
| 紅色 | 電源 | 5V |
| 棕色（或黑色） | 接地 | GND |

- 伺服的訊號線**接一般腳位就可以**，不需要 `~` 腳。
- 在 Tinkercad 元件庫搜尋「Micro Servo」，開始模擬後可以看到白色擺臂實際轉動。
- 如果系統裡有多顆伺服，每顆的紅線、棕線都接到麵包板電源軌，橘線各自接不同腳位。

### 程式裡的「遙控器」

本週每支程式的開頭都有這三行，你**不需要修改**，只要看懂它在做什麼：

```cpp
#include <Servo.h>    // 1. 拿出伺服馬達的說明書
Servo gate;           // 2. 準備一個遙控器，取名叫 gate
gate.attach(pin);     // 3. 把遙控器和某隻腳位配對（寫在 setup 裡）
```

之後只要下指令：

```cpp
gate.write(90);       // 轉到 90 度
```

> 💡 本週程式還附上一個寫好的 `sweepTo(起點, 終點)`，讓馬達一度一度慢慢轉過去，動作比較自然，也比較安全。

### ⚠️ 伺服馬達會「借走」9、10 的調光功能

第 5 週學過：蜂鳴器發聲時，`~3` 和 `~11` 的調光會失效。伺服馬達也有類似的情況——**只要程式用了伺服，`~9` 和 `~10` 就不能調光了**（不管伺服本身接在哪一隻腳）。

兩個規則疊在一起，就得到本週最重要的一張表：

| 系統裡有…… | 還能調光、混色的 `~` 腳 | 能放一顆完整漸變 RGB 嗎？ |
| --- | --- | --- |
| 都沒有 | 3、5、6、9、10、11 | 可以放兩顆 |
| 只有蜂鳴器 | 5、6、9、10 | 可以放一顆 |
| 只有伺服 | 3、5、6、11 | 可以放一顆 |
| **伺服＋蜂鳴器** | **只剩 5、6** | **不行** |

> 🤔 那「伺服＋蜂鳴器＋RGB」的系統怎麼辦？回想第 5 週 Lv.2：如果 RGB 只需要顯示**紅、綠、藍、黃、青、紫、白**這些「全亮／全滅組合出來的顏色」，就**不需要 `~` 腳**，接一般腳位也可以。本週的範例程式都只用這類顏色。

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **伺服：橘訊號、紅 5V、棕 GND** | 訊號接一般腳位即可，不用電阻 |
| ② 有伺服，9、10 不能調光 | 伺服本身可以接在 9、10 |
| ③ 有蜂鳴器，3、11 不能調光 | |
| ④ 伺服＋蜂鳴器同時存在，只剩 5、6 能調光 | RGB 改用「全亮全滅」的顏色，接哪都行 |
| ⑤ 每條燈線一顆 220Ω；伺服、蜂鳴器、旋鈕、TMP36 不用 | |
| ⑥ 按鈕、光敏電阻各配一顆 10kΩ | TMP36 平面朝自己：左 5V、中 A 腳、右 GND |
| ⑦ Pin 0、1 先不要用；腳位不夠時借 A 腳 | A 腳也能接燈、按鈕、蜂鳴器、伺服 |

---

## 共用工具

### 🔧 工具 1：伺服測試器

接好伺服後先跑這支，確認擺臂會依序轉到 0°、90°、180°，並觀察**哪個角度對應到哪個方向**——後面設定「開」「關」角度時會用到。

```cpp
#include <Servo.h>

int pinServo = 6;   // ← 改成你伺服訊號線接的腳位
Servo test;

void setup() {
  Serial.begin(9600);
  test.attach(pinServo);
}

void loop() {
  int angles[] = { 0, 90, 180, 90 };
  for (int i = 0; i < 4; i++) {
    Serial.print("Angle: ");
    Serial.println(angles[i]);
    test.write(angles[i]);
    delay(1200);
  }
}
```

### 🔧 工具 2：接線驗證器（沿用前幾週）

```cpp
int pins[] = { 9, 10, 11 };   // ← 填入所有「燈」的腳位（不含伺服、蜂鳴器）
int numPins = 3;

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

### 🔧 其他工具

- **類比讀值檢查器**（第 5 週）：檢查旋鈕、光敏電阻
- **溫度讀值檢查器**（第 6 週）：檢查 TMP36
- **蜂鳴器測試器**（第 5 週）：檢查蜂鳴器

> 📌 **每個專案的固定流程**
> 1. **Step 0**：讀完情境，先填好腳位規劃表（包含還沒接的元件）
> 2. 用工具 1 找出「開」「關」各是幾度，填進設定區
> 3. 照 Step 1 → 2 → 3 逐步接線，每完成一步就把對應的功能開關改成 `true`
> 4. 每一步都先用工具確認接線，再看功能是否正常

---

## 專案一：停車場出入口柵欄

⭐ **難度**：按鈕 ＋ 伺服，再逐步加紅綠燈與蜂鳴器

**客戶需求**：購物中心停車場出口的柵欄，收費員按一下按鈕，柵欄緩緩升起；再按一下，柵欄緩緩放下。柵欄旁要有紅綠燈告訴駕駛能不能通過，升降時蜂鳴器發出提示音，提醒行人注意。

### Step 0：腳位規劃

| 元件 | 腳位 | 需要 `~` 腳嗎？為什麼？ |
| --- | --- | --- |
| 柵欄伺服 | | |
| 收費員按鈕 | | |
| 紅燈（禁止通行） | | |
| 綠燈（可以通行） | | |
| 提示蜂鳴器 | | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 按鈕＋伺服 | 按一下柵欄升起，再按一下放下 | （不用改） |
| Step 2 | ＋紅燈＋綠燈 | 柵欄放下亮紅、升起亮綠 | `useLights = true` |
| Step 3 | ＋蜂鳴器 | 升起高音、放下低音提示 | `useBuzzer = true` |

### 程式（只改設定區）

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int pinServo  = ?;      // 柵欄伺服
int pinButton = ?;      // 收費員按鈕
int ledRed    = ?;      // 紅燈
int ledGreen  = ?;      // 綠燈
int pinBuzzer = ?;      // 提示蜂鳴器
int closedAngle = 0;    // 柵欄放下的角度（用工具 1 確認）
int openAngle   = 90;   // 柵欄升起的角度
bool useLights = false; // Step 2 接好後改成 true
bool useBuzzer = false; // Step 3 接好後改成 true
// ============================

Servo gate;
bool isOpen = false;
int lastButton = LOW;

// 一度一度慢慢轉過去
void sweepTo(int from, int to) {
  int step = (to > from) ? 1 : -1;
  for (int a = from; a != to; a += step) {
    gate.write(a);
    delay(10);
  }
  gate.write(to);
}

void setup() {
  gate.attach(pinServo);
  gate.write(closedAngle);
  pinMode(pinButton, INPUT);
  pinMode(ledRed, OUTPUT);
  pinMode(ledGreen, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  if (useLights) {
    digitalWrite(ledRed,   isOpen ? LOW : HIGH);
    digitalWrite(ledGreen, isOpen ? HIGH : LOW);
  }

  int now = digitalRead(pinButton);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    isOpen = !isOpen;
    if (useBuzzer) tone(pinBuzzer, isOpen ? 1200 : 600, 300);
    if (isOpen) {
      Serial.println("Gate OPENING");
      sweepTo(closedAngle, openAngle);
    } else {
      if (useLights) {                       // 放下前先轉紅燈
        digitalWrite(ledRed, HIGH);
        digitalWrite(ledGreen, LOW);
      }
      Serial.println("Gate CLOSING");
      sweepTo(openAngle, closedAngle);
    }
  }
  lastButton = now;
}
```

### 情境思考

1. 用工具 1 測試後，你的柵欄「水平放下」和「垂直升起」各是幾度？跟同學一樣嗎？為什麼可能不一樣？
2. 程式裡「放下前先轉紅燈」這個小細節，是為了避免什麼危險？
3. 💼 真實停車場的柵欄，常常在車子通過後**自動放下**，而不是等收費員再按一次。兩種做法各有什麼優缺點？（下一個專案會用到「自動」的概念）

### ✅ 專案一驗收

- [ ] 按一下柵欄平滑升起，再按一下平滑放下
- [ ] 柵欄放下亮紅、升起亮綠，且放下前會先轉紅
- [ ] 升起與放下的提示音不同
- [ ] 伺服三條線顏色接對，訊號線沒有佔用需要的 `~` 腳

---

## 專案二：溫室自動天窗

⭐⭐ **難度**：TMP36 ＋ 伺服 ＋ 光敏電阻 ＋ RGB（注意伺服借走 9、10）

**客戶需求**：有機農場的溫室天窗，要依溫度自動調整開啟角度：**20°C 以下全關，35°C 以上全開（90°），中間依比例半開**。夜間為了保溫，不論溫度一律關窗。管理室的狀態燈：關窗藍、半開琥珀色、全開綠。（琥珀色是「紅全亮＋綠半亮」，需要調光才能正確顯示。）

### Step 0：腳位規劃

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| TMP36 | A0 | — |
| 天窗伺服 | | |
| 光敏電阻（日夜偵測） | | — |
| 狀態燈 RGB（R / G / B） | | |

> 🤔 **規劃提示**：這一關有伺服、**沒有**蜂鳴器。查一下「~ 腳可用表」，RGB 能選哪些腳？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | TMP36＋伺服 | 拖曳溫度滑桿，天窗依溫度開到不同角度 | （不用改） |
| Step 2 | ＋光敏電阻（含 10kΩ） | 天黑時天窗一律關閉 | `useNight = true` |
| Step 3 | ＋RGB | 狀態燈顯示關（藍）／半開（琥珀）／全開（綠） | `useRGB = true` |

### 程式（只改設定區）

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int pinTemp  = A0;                  // TMP36
int pinServo = ?;                   // 天窗伺服
int pinLDR   = ?;                   // 日夜偵測光敏電阻（A 腳）
int pinR = ?, pinG = ?, pinB = ?;   // 狀態燈 RGB（避開 9、10）
int closeTemp = 20;                 // 這個溫度以下全關
int openTemp  = 35;                 // 這個溫度以上全開
int fullOpen  = 90;                 // 全開角度
int darkLine  = 300;                // 光線低於這個數字算夜間
bool useNight = false;              // Step 2 接好後改成 true
bool useRGB   = false;              // Step 3 接好後改成 true
// ============================

Servo window;

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
  window.attach(pinServo);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float t = readC(pinTemp);
  int angle = map((int)t, closeTemp, openTemp, 0, fullOpen);
  angle = constrain(angle, 0, fullOpen);

  bool night = false;
  if (useNight) {
    night = analogRead(pinLDR) < darkLine;
    if (night) angle = 0;                      // 夜間一律關窗
  }

  window.write(angle);

  if (useRGB) {
    if (angle == 0)             setColor(0, 0, 255);     // 關窗：藍
    else if (angle >= fullOpen) setColor(0, 255, 0);     // 全開：綠
    else                        setColor(255, 140, 0);   // 半開：琥珀（綠色要調光）
  }

  Serial.print("Temp: ");
  Serial.print(t);
  Serial.print(" C   Window: ");
  Serial.print(angle);
  Serial.println(night ? " deg (NIGHT)" : " deg");
  delay(300);
}
```

### 情境思考

1. 故意把 RGB 的**綠色腳**接到 `~9` 或 `~10`，觀察「半開」時的琥珀色有什麼異常，再改回來。為什麼「全關」「全開」看不出問題？
2. 溫度在 27～28°C 之間來回跳動時，天窗會一直微微擺動。真實天窗這樣動會有什麼問題？（提示：回想第 6 週冷氣「兩條線」的做法）
3. 💼 下雨天溫度高，但天窗不應該打開。如果客戶要加「下雨關窗」功能，你目前學過的元件中，有沒有能「假裝」雨量感測器的？

### ✅ 專案二驗收

- [ ] 天窗角度隨溫度比例變化，20°C 以下全關、35°C 以上全開
- [ ] 天黑時不論溫度天窗都關閉
- [ ] 狀態燈顯示關（藍）／半開（琥珀）／全開（綠），且 RGB 沒有接在 9、10
- [ ] TMP36 方向正確，光敏電阻有接 10kΩ

---

## 專案三：展場自動門與人流管制

⭐⭐⭐ **難度**：光敏電阻光柵 ＋ 伺服 ＋ 蜂鳴器 ＋ 離場按鈕 ＋ RGB → **伺服＋蜂鳴器，只剩 5、6 能調光**

**客戶需求**：展覽館的特展室入口，訪客走到門前擋住光柵，自動門打開 3 秒後自動關上。特展室最多只能容納 **5 人**，客滿時門不開，並發出低沉的「拒絕」聲。訪客離場時，出口工作人員按一下「離場鈕」，館內人數減 1。入口上方有一顆燈：可入場亮綠、客滿亮紅。

### Step 0：腳位規劃

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| 入口光柵（光敏電阻） | A0 | — |
| 自動門伺服 | | |
| 蜂鳴器 | | |
| 離場鈕 | | |
| 入口狀態 RGB（R / G / B） | | |

> 🤔 **規劃提示**：這一關伺服和蜂鳴器都有，能調光的 `~` 腳只剩 5、6。但入口燈只需要「綠」和「紅」——它還需要 `~` 腳嗎？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 光敏電阻（含 10kΩ）＋伺服 | 擋住光線，門開 3 秒後自動關 | （不用改） |
| Step 2 | ＋蜂鳴器 | 每進一人嗶一聲並計數；滿 5 人後門不開、發出拒絕聲 | `useCapacity = true` |
| Step 3 | ＋離場鈕＋RGB | 按離場鈕人數減 1；入口燈顯示綠／紅 | `useExit = true` |

### 程式（只改設定區）

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int pinLDR    = A0;                 // 入口光柵
int pinServo  = ?;                  // 自動門伺服
int pinBuzzer = ?;                  // 蜂鳴器
int pinExit   = ?;                  // 離場鈕
int pinR = ?, pinG = ?, pinB = ?;   // 入口狀態 RGB
int threshold = 300;                // 低於這個數字算「有人擋住」
int capacity  = 5;                  // 館內人數上限
int closedAngle = 0;                // 門關的角度
int openAngle   = 90;               // 門開的角度
bool useCapacity = false;           // Step 2 接好後改成 true
bool useExit     = false;           // Step 3 接好後改成 true
// ============================

Servo door;
bool wasBlocked = false;
int inside = 0;
int lastExit = LOW;

void sweepTo(int from, int to) {
  int step = (to > from) ? 1 : -1;
  for (int a = from; a != to; a += step) {
    door.write(a);
    delay(8);
  }
  door.write(to);
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  door.attach(pinServo);
  door.write(closedAngle);
  pinMode(pinExit, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // ---- 離場鈕 ----
  if (useExit) {
    int now = digitalRead(pinExit);
    if (now == HIGH && lastExit == LOW) {
      delay(50);
      if (inside > 0) inside--;
      Serial.print("Visitor left. Inside: ");
      Serial.println(inside);
    }
    lastExit = now;

    if (inside >= capacity) setColor(255, 0, 0);   // 客滿：紅
    else                    setColor(0, 255, 0);   // 可入場：綠
  }

  // ---- 入口光柵：只在剛被擋住時反應 ----
  bool blocked = analogRead(pinLDR) < threshold;
  if (blocked && !wasBlocked) {
    if (useCapacity && inside >= capacity) {
      Serial.println("FULL - entry refused");
      tone(pinBuzzer, 300, 600);
    } else {
      if (useCapacity) {
        inside++;
        tone(pinBuzzer, 1200, 100);
        Serial.print("Welcome! Inside: ");
        Serial.println(inside);
      }
      sweepTo(closedAngle, openAngle);
      delay(3000);                                  // 門開 3 秒
      sweepTo(openAngle, closedAngle);
    }
  }
  wasBlocked = blocked;
  delay(30);
}
```

### 情境思考

1. 完成 Step 2 後，連續擋光 5 次，門就再也打不開了。為什麼？Step 3 解決了什麼問題？
2. 入口燈接在一般腳位，顯示紅、綠正常嗎？如果客戶想要「人越多、顏色從綠慢慢變紅」的漸層效果，在目前的元件組合下做得到嗎？你會怎麼跟客戶說明？
3. 💼 用「離場鈕」計算人數，要靠工作人員記得按。如果工作人員忘記按，系統會出現什麼問題？你會建議怎麼改善？（提示：出口是不是也可以裝一道光柵？）

### ✅ 專案三驗收

- [ ] 擋住光線時門平滑打開，3 秒後自動關上
- [ ] 每進一人計數加 1，滿 5 人後門不開並發出拒絕聲
- [ ] 按離場鈕人數減 1，門又可以開
- [ ] 入口燈可入場綠、客滿紅
- [ ] 腳位規劃表正確說明為什麼 RGB 不需要 `~` 腳

---

## 專案四：物流包裹分揀站

⭐⭐⭐⭐ **難度**：**2 顆伺服** ＋ 光柵 ＋ 旋鈕 ＋ 3 顆燈 ＋ 蜂鳴器 ＋ 急停鈕 ＋ RGB

**客戶需求**：物流中心的輸送帶末端要自動分揀包裹。包裹經過光柵時，系統讀取重量（目前用旋鈕模擬磅秤），依重量推入三條分流道：

| 重量 | 分流道 | 動作 |
| --- | --- | --- |
| 10 公斤以下 | 輕件道 | 輕件擋板（伺服 A）推出 |
| 10～20 公斤 | 一般道 | 兩片擋板不動，直接通過 |
| 20 公斤以上 | 重件道 | 重件擋板（伺服 B）推出 |

每條分流道有一顆燈，包裹分到哪一道就閃一下，Serial Monitor 統計各道件數。現場有一顆**急停鈕**：按下後所有擋板回到原位、系統停止運作、狀態燈轉紅；再按一次恢復運轉。

### Step 0：腳位規劃（本專案的重頭戲）

| 元件 | 需要幾隻腳 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- | --- |
| 包裹光柵（光敏電阻） | 1（A 腳） | A0 | — |
| 重量旋鈕（模擬磅秤） | 1（A 腳） | | — |
| 輕件擋板伺服 A | | | |
| 重件擋板伺服 B | | | |
| 分流道燈 × 3 | | | |
| 蜂鳴器 | | | |
| 急停鈕 | | | |
| 系統狀態 RGB | | | |

> 🤔 **規劃提示**：
> - 兩顆伺服的紅線、棕線怎麼共用電源軌？
> - 伺服＋蜂鳴器同時存在，RGB 能調光嗎？本專案的狀態燈只用綠（運轉）和紅（急停），需要 `~` 腳嗎？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 光柵＋旋鈕＋伺服 A＋3 顆燈 | 擋光一次分揀一件：輕件推出、其他直走 | （不用改） |
| Step 2 | ＋伺服 B＋蜂鳴器 | 重件道啟用，每分揀一件嗶一聲 | `useHeavy = true` |
| Step 3 | ＋急停鈕＋RGB | 按急停全部停止、擋板歸位、亮紅；再按恢復 | `useStop = true` |

### 程式（只改設定區）

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int pinLDR    = A0;                 // 包裹光柵
int pinKnob   = ?;                  // 重量旋鈕（A 腳）
int pinArmA   = ?;                  // 輕件擋板伺服 A
int pinArmB   = ?;                  // 重件擋板伺服 B
int lane[3]   = { ?, ?, ? };        // 輕件道、一般道、重件道 燈
int pinBuzzer = ?;                  // 蜂鳴器
int pinStop   = ?;                  // 急停鈕
int pinR = ?, pinG = ?, pinB = ?;   // 系統狀態 RGB
int threshold = 300;                // 低於這個數字算「包裹經過」
int restAngle = 0;                  // 擋板平常的角度
int pushAngle = 70;                 // 擋板推出的角度
bool useHeavy = false;              // Step 2 接好後改成 true
bool useStop  = false;              // Step 3 接好後改成 true
// ============================

Servo armA, armB;
bool wasBlocked = false;
bool stopped = false;
int lastStop = LOW;
int count[3] = { 0, 0, 0 };

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  armA.attach(pinArmA);
  armB.attach(pinArmB);
  armA.write(restAngle);
  armB.write(restAngle);
  for (int i = 0; i < 3; i++) pinMode(lane[i], OUTPUT);
  pinMode(pinStop, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // ---- 急停鈕：按一下停機，再按一下恢復 ----
  if (useStop) {
    int now = digitalRead(pinStop);
    if (now == HIGH && lastStop == LOW) {
      delay(50);
      stopped = !stopped;
      if (stopped) {
        armA.write(restAngle);                // 擋板歸位
        armB.write(restAngle);
        tone(pinBuzzer, 400, 500);
        Serial.println("!!! EMERGENCY STOP !!!");
      } else {
        Serial.println("System resumed");
      }
    }
    lastStop = now;

    if (stopped) {
      setColor(255, 0, 0);                    // 急停：紅
      return;                                 // 安全連鎖：後面的分揀動作全部不執行
    }
    setColor(0, 255, 0);                      // 運轉：綠
  }

  // ---- 包裹分揀：只在剛擋住光柵時處理一件 ----
  bool blocked = analogRead(pinLDR) < threshold;
  if (blocked && !wasBlocked) {
    int weight = map(analogRead(pinKnob), 0, 1023, 0, 30);   // 0~30 公斤

    int target = 1;                           // 預設一般道
    if (weight < 10) target = 0;              // 輕件道
    else if (weight >= 20 && useHeavy) target = 2;   // 重件道

    if (target == 0) armA.write(pushAngle);
    if (target == 2) armB.write(pushAngle);

    digitalWrite(lane[target], HIGH);
    if (useHeavy) tone(pinBuzzer, 1500, 80);
    delay(800);                               // 等包裹被推走
    digitalWrite(lane[target], LOW);
    armA.write(restAngle);
    armB.write(restAngle);

    count[target]++;
    Serial.print(weight);
    Serial.print(" kg -> Lane ");
    Serial.print(target + 1);
    Serial.print("   [Light:");
    Serial.print(count[0]);
    Serial.print(" Normal:");
    Serial.print(count[1]);
    Serial.print(" Heavy:");
    Serial.print(count[2]);
    Serial.println("]");
  }
  wasBlocked = blocked;
  delay(30);
}
```

### 情境思考

1. 完成 Step 1 時，把旋鈕轉到 25 公斤再擋光，包裹會被分到哪一道？為什麼 Step 2 才會正確？
2. 急停時，程式用一行 `return` 讓後面的分揀動作完全不執行。這就是「安全連鎖」。請再舉兩個你在生活中看過的安全連鎖例子。
3. 💼 分揀錯誤（重件被推進輕件道）在物流中心會造成什麼後果？除了把系統做得更精準，管理上還可以設計什麼「防呆」機制？

### ✅ 專案四驗收

- [ ] 擋光一次只分揀一件，依重量進入正確分流道
- [ ] 分流道燈在分揀時閃一下，Serial Monitor 正確統計三道件數
- [ ] 按急停後擋板歸位、狀態燈轉紅，擋光也不會分揀；再按恢復
- [ ] 兩顆伺服共用電源軌，訊號線各自接不同腳位
- [ ] 腳位規劃表說明 RGB 為何接一般腳位也可以

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：四個範例專案是「跟著做」，這一區換你「自己規劃」。每一關都是新情境、新的元件組合，你要先判斷需要什麼元件、接幾顆、接哪隻腳，**還要先想清楚馬達轉幾度、什麼情況不准動**，再動手接線。
>
> 📈 **難度安排**：
>
> | 關卡 | 伺服 | 輸入元件 | 輸出（燈、蜂鳴器） | 配線重點 |
> | --- | --- | --- | --- | --- |
> | Lv.1 | 1 | 按鈕 | 1 LED | 伺服三色線 |
> | Lv.2 | 1 | 光敏電阻 | 1 LED（調光） | 調光燈避開 9、10 |
> | Lv.3 | 1 | 2 光敏電阻 | 2 LED | 兩顆光敏電阻左右擺放 |
> | Lv.4 | 1 | 旋鈕＋按鈕 | 3 LED＋RGB＋蜂鳴器 | 伺服＋蜂鳴器，RGB 不能漸變 |
> | Lv.5 | 3 | 2 旋鈕＋TMP36＋2 按鈕 | 3 LED＋RGB＋蜂鳴器 | 一般腳位剛好用滿 |
> | Lv.6 | 自訂 | 自訂 | 自訂 | 顧問提案 |
>
> 📌 **作答規則**：每一關先填腳位規劃表，再接線；接好後先用共用工具檢查，最後才上傳程式。程式只需要改「設定區」。

---

### 🟢 Lv.1 寵物自動餵食器

**情境**：上班族出門前設定好餵食器，寵物保母來訪時按一下按鈕，飼料盒蓋子打開 1 秒讓飼料落下，再自動關上；餵食時「餵食中」燈亮。Serial Monitor 記錄今天餵了幾次。

**元件**：1 顆伺服＋1 顆按鈕＋1 顆單色 LED

**你要想的問題**：

- 伺服三條線的顏色各接哪裡？
- 用工具 1 找出「蓋子關」和「蓋子開」各是幾度。

**程式**：

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int pinServo  = ?;    // 飼料盒蓋伺服
int pinButton = ?;    // 餵食按鈕
int ledFeed   = ?;    // 餵食中燈
int closedAngle = 0;  // 蓋子關
int openAngle   = 60; // 蓋子開
int openTime    = 1000; // 打開多久（毫秒）
// ============================

Servo lid;
int lastButton = LOW;
int feedCount = 0;

void setup() {
  lid.attach(pinServo);
  lid.write(closedAngle);
  pinMode(pinButton, INPUT);
  pinMode(ledFeed, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int now = digitalRead(pinButton);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    feedCount++;
    digitalWrite(ledFeed, HIGH);
    lid.write(openAngle);
    delay(openTime);
    lid.write(closedAngle);
    digitalWrite(ledFeed, LOW);
    Serial.print("Fed today: ");
    Serial.println(feedCount);
  }
  lastButton = now;
}
```

**情境思考**：

1. 寵物體型不同，需要的飼料量也不同。不改接線，你會調整設定區的哪個數字？
2. 💼 保母連按三次按鈕，寵物就吃了三份。你會建議怎麼避免過量餵食？（提示：今天已經餵幾次了？）

---

### 🟢 Lv.2 辦公室自動窗簾

**情境**：會議室窗邊裝一顆光敏電阻，陽光越強，窗簾（伺服）拉得越多，避免投影幕反光；傍晚光線變暗，窗簾自動打開，同時室內的輔助燈依暗的程度逐漸調亮。

**元件**：1 顆光敏電阻＋1 顆伺服＋1 顆單色 LED（輔助燈，要調光）

**你要想的問題**：

- 輔助燈要調光，必須接 `~` 腳。但這一關有伺服，哪些 `~` 腳不能用？
- 光敏電阻要裝在窗戶的「外側」還是「內側」？如果裝在輔助燈旁邊會怎樣？

**程式**：

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int pinLDR   = A0;    // 窗邊光敏電阻
int pinServo = ?;     // 窗簾伺服
int pinLamp  = ?;     // 輔助燈（要調光，避開 9、10）
int darkLine   = 200; // 低於這個數字開始補光
int brightLine = 900; // 高於這個數字窗簾全拉
// ============================

Servo curtain;

void setup() {
  curtain.attach(pinServo);
  pinMode(pinLamp, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int light = analogRead(pinLDR);

  // 越亮，窗簾拉越多（0=全開，180=全拉）
  int angle = map(light, darkLine, brightLine, 0, 180);
  angle = constrain(angle, 0, 180);
  curtain.write(angle);

  // 越暗，輔助燈越亮
  int lamp = map(light, 0, darkLine, 255, 0);
  lamp = constrain(lamp, 0, 255);
  analogWrite(pinLamp, lamp);

  Serial.print("Light: ");
  Serial.print(light);
  Serial.print("   Curtain: ");
  Serial.print(angle);
  Serial.print("   Lamp: ");
  Serial.println(lamp);
  delay(200);
}
```

**情境思考**：

1. 把輔助燈故意接到 `~9`，觀察亮度變化有什麼不同。為什麼？
2. 💼 開會時有人想要窗簾全開看風景，但系統一直自動拉上。你會建議加什麼元件、什麼功能，讓人可以「暫時手動控制」？

---

### 🟡 Lv.3 太陽能板追光支架

**情境**：屋頂的太陽能板裝在可以左右轉的支架上。支架左右兩側各裝一顆光敏電阻：左邊比較亮，就往左轉一點；右邊比較亮，就往右轉一點；兩邊差不多，就停住。兩顆指示燈顯示目前正在往哪邊轉。

**元件**：2 顆光敏電阻＋1 顆伺服＋2 顆單色 LED

**Step 0：腳位規劃**

| 元件 | 腳位 |
| --- | --- |
| 左側光敏電阻 | A0 |
| 右側光敏電阻 | |
| 支架伺服 | |
| 往左轉指示燈 | |
| 往右轉指示燈 | |

**你要想的問題**：

- 兩顆光敏電阻各需要一顆 10kΩ。在麵包板上要怎麼擺，才能清楚看出哪顆是「左」、哪顆是「右」？

**程式**：

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int ldrLeft  = A0;    // 左側光敏電阻
int ldrRight = ?;     // 右側光敏電阻（A 腳）
int pinServo = ?;     // 支架伺服
int ledLeft  = ?;     // 往左轉指示燈
int ledRight = ?;     // 往右轉指示燈
int tolerance = 50;   // 兩邊差距小於這個值就不動
// ============================

Servo panel;
int angle = 90;       // 從正中間開始

void setup() {
  panel.attach(pinServo);
  panel.write(angle);
  pinMode(ledLeft, OUTPUT);
  pinMode(ledRight, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int left  = analogRead(ldrLeft);
  int right = analogRead(ldrRight);
  int diff = left - right;

  digitalWrite(ledLeft, LOW);
  digitalWrite(ledRight, LOW);

  if (diff > tolerance) {          // 左邊比較亮：往左轉
    angle = angle + 1;
    digitalWrite(ledLeft, HIGH);
  } else if (diff < -tolerance) {  // 右邊比較亮：往右轉
    angle = angle - 1;
    digitalWrite(ledRight, HIGH);
  }
  angle = constrain(angle, 0, 180);
  panel.write(angle);

  Serial.print("L: ");
  Serial.print(left);
  Serial.print("  R: ");
  Serial.print(right);
  Serial.print("  Angle: ");
  Serial.println(angle);
  delay(30);
}
```

**情境思考**：

1. 在 Tinkercad 裡把左邊光敏電阻調亮，支架卻往「右」轉。**不改程式**，你可以怎麼修正？
2. `tolerance` 設成 0 會發生什麼事？設成 500 又會怎樣？
3. 💼 追光支架比固定支架貴，但發電量較高。如果你是屋主，你需要哪些數字才能判斷值不值得裝？

---

### 🟡 Lv.4 旅館保險箱密碼鎖

**情境**：旅館客房的保險箱，房客用旋鈕選數字（0～9），按「確認鈕」輸入一位，共三位。三顆燈顯示已輸入幾位。密碼正確，鎖舌（伺服）打開 3 秒，RGB 亮綠並播放提示音；密碼錯誤，RGB 亮紅並發出低音。**連續錯 3 次，鎖定 10 秒**，期間紅燈閃爍、按鈕無效。

**元件**：1 顆旋鈕＋1 顆按鈕＋1 顆伺服＋3 顆單色 LED＋1 顆 RGB LED＋1 顆蜂鳴器

**Step 0：腳位規劃**

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| 數字旋鈕 | A0 | — |
| 確認鈕 | | |
| 鎖舌伺服 | | |
| 輸入進度燈 × 3 | | |
| 狀態 RGB | | |
| 蜂鳴器 | | |

**你要想的問題**：

- 這一關有伺服又有蜂鳴器，能調光的 `~` 腳只剩幾隻？狀態燈只用藍、綠、紅，需要 `~` 腳嗎？

**程式**：

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int pinKnob   = A0;                 // 數字旋鈕
int pinButton = ?;                  // 確認鈕
int pinServo  = ?;                  // 鎖舌伺服
int progress[3] = { ?, ?, ? };      // 輸入進度燈
int pinR = ?, pinG = ?, pinB = ?;   // 狀態 RGB
int pinBuzzer = ?;                  // 蜂鳴器
int secret[3] = { 3, 7, 1 };        // 密碼
int lockedAngle = 0;                // 上鎖角度
int openAngle   = 90;               // 開鎖角度
// ============================

Servo bolt;
int entered = 0;
int digits[3];
int wrong = 0;
int lastButton = LOW;
int lastDigit = -1;

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  bolt.attach(pinServo);
  bolt.write(lockedAngle);
  pinMode(pinButton, INPUT);
  for (int i = 0; i < 3; i++) pinMode(progress[i], OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  setColor(0, 0, 255);                               // 待輸入：藍

  int d = map(analogRead(pinKnob), 0, 1024, 0, 10);  // 0~9
  if (d != lastDigit) {
    Serial.print("Digit: ");
    Serial.println(d);
    lastDigit = d;
  }

  for (int i = 0; i < 3; i++) {
    digitalWrite(progress[i], (i < entered) ? HIGH : LOW);
  }

  int now = digitalRead(pinButton);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    digits[entered] = d;
    entered++;
    tone(pinBuzzer, 1000, 60);

    if (entered == 3) {
      bool ok = (digits[0] == secret[0] && digits[1] == secret[1] && digits[2] == secret[2]);
      entered = 0;
      for (int i = 0; i < 3; i++) digitalWrite(progress[i], LOW);

      if (ok) {
        wrong = 0;
        Serial.println("UNLOCKED");
        setColor(0, 255, 0);
        tone(pinBuzzer, 1319, 300);
        bolt.write(openAngle);
        delay(3000);
        bolt.write(lockedAngle);
      } else {
        wrong++;
        Serial.print("WRONG CODE (");
        Serial.print(wrong);
        Serial.println("/3)");
        setColor(255, 0, 0);
        tone(pinBuzzer, 250, 600);
        delay(800);
        if (wrong >= 3) {
          Serial.println("LOCKED OUT for 10 s");
          for (int k = 0; k < 20; k++) {
            setColor(255, 0, 0); delay(250);
            setColor(0, 0, 0);   delay(250);
          }
          wrong = 0;
        }
      }
    }
  }
  lastButton = now;
}
```

**情境思考**：

1. 輸入進度燈為什麼對房客很重要？如果沒有它，房客會遇到什麼困擾？
2. 「連錯 3 次鎖 10 秒」是為了防什麼？如果改成「連錯 3 次永久鎖住，要叫櫃台」，對旅館營運有什麼影響？
3. 💼 房客忘記密碼時，旅館需要一個「萬能開鎖」機制。你會怎麼設計，同時兼顧方便與安全？

---

### 🔴 Lv.5 機械手臂夾取工作站

**情境**：工廠的教學用機械手臂有三顆伺服：**底座旋轉、手臂升降、夾爪開合**。操作員用兩顆旋鈕分別控制底座與手臂角度，按「夾爪鈕」切換夾緊／放開。手臂馬達旁裝一顆 TMP36，**溫度超過 45°C 時立刻進入過熱保護**：三顆伺服停在原位、所有操作無效、紅燈亮、蜂鳴器響；溫度降回 45°C 以下後，必須按「復歸鈕」才能恢復操作。

| 區域 | 元件 |
| --- | --- |
| 手臂動作 | 3 顆伺服（底座、手臂、夾爪） |
| 操作輸入 | 2 顆旋鈕、夾爪鈕 |
| 安全保護 | TMP36、復歸鈕、蜂鳴器 |
| 狀態顯示 | 運轉燈、夾爪燈、過熱燈（3 顆單色 LED）＋狀態 RGB |

**Step 0：腳位規劃（本關重點）**

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 底座旋鈕、手臂旋鈕 | 2（A 腳） | A0、A1 |
| TMP36 | 1（A 腳） | |
| 3 顆伺服 | | |
| 夾爪鈕、復歸鈕 | | |
| 蜂鳴器 | | |
| 3 顆狀態燈 | | |
| 狀態 RGB | | |
| **一般腳位合計** | | |
| **Pin 2～13 可用數量** | 12 | |

> 🤔 **規劃提示**：
> - 一般腳位是不是剛好用滿？如果不夠，A3～A5 可以借來接什麼？
> - 三顆伺服的紅線、棕線，加上兩顆旋鈕、TMP36 都要接 5V 與 GND。麵包板電源軌怎麼安排？

**程式**：

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int knobBase = A0;                  // 底座旋鈕
int knobArm  = A1;                  // 手臂旋鈕
int pinTemp  = ?;                   // 馬達溫度 TMP36（A 腳）
int pinBase  = ?;                   // 底座伺服
int pinArm   = ?;                   // 手臂伺服
int pinGrip  = ?;                   // 夾爪伺服
int btnGrip  = ?;                   // 夾爪鈕
int btnReset = ?;                   // 復歸鈕
int pinBuzzer = ?;                  // 蜂鳴器
int ledRun  = ?, ledGripOn = ?, ledHot = ?;   // 運轉燈、夾爪燈、過熱燈
int pinR = ?, pinG = ?, pinB = ?;   // 狀態 RGB
float hotLine = 45.0;               // 過熱溫度
int gripOpen = 80, gripClosed = 20; // 夾爪開／夾緊角度
// ============================

Servo base, arm, grip;
bool gripIsClosed = false;
bool tripped = false;
int lastGrip = LOW, lastReset = LOW;

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
  base.attach(pinBase);
  arm.attach(pinArm);
  grip.attach(pinGrip);
  grip.write(gripOpen);
  pinMode(btnGrip, INPUT);
  pinMode(btnReset, INPUT);
  pinMode(ledRun, OUTPUT);
  pinMode(ledGripOn, OUTPUT);
  pinMode(ledHot, OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float t = readC(pinTemp);
  int nowGrip  = digitalRead(btnGrip);
  int nowReset = digitalRead(btnReset);

  // ---- 過熱判斷 ----
  if (t >= hotLine && !tripped) {
    tripped = true;
    Serial.print("OVERHEAT at ");
    Serial.print(t);
    Serial.println(" C - all motion stopped");
  }

  // ---- 復歸：降溫後才有效 ----
  if (nowReset == HIGH && lastReset == LOW) {
    delay(50);
    if (tripped && t < hotLine) {
      tripped = false;
      Serial.println("Reset - motion enabled");
    } else if (tripped) {
      Serial.println("Cannot reset: still hot");
    }
  }
  lastReset = nowReset;

  // ---- 安全連鎖：過熱時所有操作無效 ----
  if (tripped) {
    digitalWrite(ledHot, HIGH);
    digitalWrite(ledRun, LOW);
    setColor(255, 0, 0);
    tone(pinBuzzer, 1000, 150);
    lastGrip = nowGrip;
    delay(300);
    return;
  }

  digitalWrite(ledHot, LOW);
  digitalWrite(ledRun, HIGH);
  setColor(0, 255, 0);

  // ---- 旋鈕控制底座與手臂 ----
  base.write(map(analogRead(knobBase), 0, 1023, 0, 180));
  arm.write(map(analogRead(knobArm), 0, 1023, 30, 150));

  // ---- 夾爪鈕：按一下切換夾緊／放開 ----
  if (nowGrip == HIGH && lastGrip == LOW) {
    delay(50);
    gripIsClosed = !gripIsClosed;
    grip.write(gripIsClosed ? gripClosed : gripOpen);
    Serial.println(gripIsClosed ? "Gripper CLOSED" : "Gripper OPEN");
  }
  lastGrip = nowGrip;
  digitalWrite(ledGripOn, gripIsClosed ? HIGH : LOW);

  delay(15);
}
```

**情境思考**：

1. 過熱保護時，夾爪如果正夾著零件，程式會讓它「停在原位」而不是「自動放開」。為什麼？
2. 你的一般腳位用了幾隻？如果客戶要再加一顆「急停鈕」，放得下嗎？放在哪裡？
3. 💼 手臂旋鈕的範圍是 30～150 度，而不是 0～180 度。這種「刻意限制活動範圍」的設計在工廠中有什麼意義？

---

### 🏆 Lv.6 BOSS 關：物聯網顧問提案

**情境**：你的顧問公司接到新客戶。從下面三個案例中**任選一個**，自己完成需求分析、元件選擇、腳位規劃與 Tinkercad 實作。

> **案例 A：智慧停車場出入口**
> 入口光柵偵測車輛，柵欄自動升起，車子通過後自動放下；場內車位上限用旋鈕設定，客滿時柵欄不開並顯示紅燈。出口有按鈕代表車輛離場。
>
> **案例 B：自動販賣機出貨口**
> 旋鈕選商品（4 種），按鈕確認後，對應的出貨擋板（伺服）打開一次。販賣機內有 TMP36 監測冷藏溫度，超過 10°C 時暫停販售冷飲並亮燈提示。
>
> **案例 C：自訂情境**
> 自己找一個生活或職場情境（例如：圖書館還書口、垃圾分類桶、智慧藥盒、捷運月台門）。

**最低規格**：

- 至少 1 顆伺服、1 種感測器（光敏電阻或 TMP36）、1 顆按鈕、1 顆蜂鳴器
- 總共使用 10 隻以上腳位
- 至少包含一個「安全連鎖」：某個條件下馬達**不准動**
- 程式可以從專案一～四或 Lv.1～Lv.5 改寫，說明你是從哪裡改來的、改了哪些設定

**繳交內容**：

1. **需求分析**：這個系統解決什麼問題？誰會操作？誰會被馬達動作影響？（3～5 句）
2. **動作規格表**：馬達的每個角度代表什麼狀態？轉動速度要快還是慢？為什麼？
3. **安全連鎖說明**：什麼條件下馬達不准動？如何恢復？
4. **腳位規劃表**：標註每隻腳接什麼、是否為 `~` 腳，並說明你如何處理「伺服借走 9、10」的限制
5. **Tinkercad 實作**：電路截圖＋共用工具的檢查截圖

**驗收標準**：

- [ ] 伺服三色線接對，角度符合動作規格
- [ ] 需要調光的燈避開被借走的 `~` 腳；不需要調光的燈不佔 `~` 腳
- [ ] 安全連鎖確實有效：條件成立時，任何操作都無法讓馬達轉動
- [ ] 每條燈線都有 220Ω，按鈕與光敏電阻有 10kΩ
- [ ] 麵包板依功能分區

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 伺服完全不動 | 橘線是否接到程式裡的腳位；紅線 5V、棕線 GND 是否接對 | 用工具 1 測試 |
| 伺服轉的方向和預期相反 | 「開」「關」角度設定 | 用工具 1 找出正確角度，改設定區 |
| 伺服一直微微抖動 | 輸入讀值在邊界跳動（旋鈕、光敏電阻、溫度） | 正常現象；可加大門檻差距或容許誤差 |
| 調光燈只有全亮全滅 | 是否接在 9、10（有伺服）或 3、11（有蜂鳴器） | 查「~ 腳可用表」改接 |
| RGB 顏色怪怪的 | 同上 | 改接可用的 `~` 腳，或改用全亮全滅顏色 |
| 擋一次光，動作執行好幾次 | 門檻是否剛好卡在讀值附近 | 用類比讀值檢查器重新設定門檻 |
| 多顆伺服時，動 A 卻是 B 在轉 | 程式裡的腳位和實際橘線是否對應 | 逐顆用工具 1 確認 |
| 程式無法上傳，出現錯誤訊息 | 設定區是否還有 `?` 沒填 | 所有腳位都要先填，包含還沒接的元件 |

---

## 成果繳交與評分

### 範例專案繳交

每個專案各自建立一個 Tinkercad 檔案，命名格式：`W7-專案編號-學號`（例如 `W7-P3-B12345678`）。

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
| Lv.6 BOSS | 完整顧問提案（需求分析、動作規格、安全連鎖、腳位規劃、實作） | 期中專題暖身，可分組 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 電路正確性 | 35% | 每個 Step 的功能都能正常運作，伺服角度正確 |
| 腳位規劃 | 25% | 規劃表完整，正確處理伺服與蜂鳴器借走的 `~` 腳 |
| 情境思考題 | 25% | 尤其是安全連鎖與 💼 管理面向論述 |
| 麵包板擺放 | 15% | 分區清楚，多顆伺服、感測器的電源線整齊 |

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 30 分鐘 | 認識伺服、「~ 腳可用表」、工具示範 |
| 第 1 節後段 | 專案一（全班一起） |
| 第 2 節 | 專案二、專案三（分組進行） |
| 第 3 節前段 | 專案四 |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，Lv.3 之後帶回家；預告期中專題 |

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 伺服馬達（Servo） | 能轉到 0～180° 指定角度並停住的馬達，三條線、不用電阻 |
| `Servo` 遙控器（物件） | 程式裡代表一顆伺服的「遙控器」，`.attach()` 配對腳位、`.write()` 下角度指令 |
| `sweepTo()` | 寫好的函式，讓伺服一度一度平滑轉到目標角度 |
| 計時資源衝突 | 伺服借走 9、10，蜂鳴器借走 3、11 的調光功能 |
| 安全連鎖（Interlock） | 不安全的條件成立時，強制禁止設備動作 |
| 急停（Emergency Stop） | 一按就讓所有動作停止、回到安全狀態的按鈕 |
| 容量管制 | 依人數或數量上限決定是否允許進入 |
| 活動範圍限制 | 刻意不讓馬達轉到極限角度，避免碰撞或損壞 |

---

## 反思與延伸討論

1. 本週的系統第一次「會動」。和只會亮燈、出聲的系統相比，設計時多了哪些必須考慮的風險？請從**人員安全、設備損壞、物品損失**三個角度各舉一例。
2. 專案四的急停、Lv.5 的過熱保護都是「安全連鎖」。如果你是工廠主管，你會怎麼確保員工不會為了趕工而「繞過」這些安全設計？（提示：制度、稽核、教育訓練）
3. 本週「伺服＋蜂鳴器」讓 RGB 只能顯示基本顏色。在真實的產品開發中，「元件之間互相搶資源」很常見。遇到這種限制時，除了犧牲功能，還有哪些解法？（例如：改用更大的控制板、改用其他元件、重新思考需求）

---

## 本週檢核清單

- [ ] 能正確接上伺服馬達（橘訊號、紅 5V、棕 GND）
- [ ] 能用伺服測試器找出情境需要的「開」「關」角度
- [ ] 能查「~ 腳可用表」，判斷有伺服、蜂鳴器時哪些腳還能調光
- [ ] 能在 `~` 腳不足時，改用全亮全滅的顏色或單色燈
- [ ] 能說明什麼是安全連鎖，並在專題中實作一個
- [ ] 能安排多顆伺服、多個感測器共用電源軌
- [ ] 完成 4 個獨立專案，並各自繳交 Tinkercad 檔案
- [ ] 完成練習題 Lv.1～Lv.4，挑戰 Lv.5 與 Lv.6

---

## 下週預告

第 8 週是**期中專題**，沒有新元件。你要以顧問身分，把前七週學過的所有元件——LED、按鈕、RGB、旋鈕、蜂鳴器、光敏電阻、TMP36、伺服馬達——整合成一個完整的「智慧場域監控站」。本週 Lv.6 的顧問提案格式，就是期中專題的暖身。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 6 週](week-06.md) | [下一週：第 8 週 — 期中專題 ➡](week-08.md)
