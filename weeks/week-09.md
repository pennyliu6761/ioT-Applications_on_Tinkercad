[⬅ 回目錄](../README.md) | [⬅ 上一週：第 8 週](week-08.md) | [下一週：第 10 週 — 智慧輸送帶 ➡](week-10.md)

---

# 第 9 週：會量距離的智慧場域

## 主題專案：讓系統知道「東西離我多遠」

> 🎯 **本週定位**：期中專題結束，顧問公司接案能力大增。這一週加入新感官：**超音波測距感測器 HC-SR04**。前面學過的感測器只能回答「亮不亮」「熱不熱」，超音波可以直接回答「**離我幾公分**」。第 5 週用旋鈕「假裝」的倒車雷達，這週終於可以用真的了。
>
> 和前幾週一樣，本週**重點不在寫程式**，而在於：
>
> 1. 讀懂情境需求，判斷需要哪些元件
> 2. 規劃腳位，在 Tinkercad 把電路一步步接起來
> 3. 想清楚感測器**裝在哪裡、朝哪個方向**，量到的距離代表什麼意思
>
> 「發射超音波、換算公分」的程式已經寫好，你只需要填腳位、調整距離門檻、把功能開關從 `false` 改成 `true`。
>
> 🏁 **本週成果**：完成 **4 個獨立專案**，每個專案存成一個獨立的 Tinkercad 檔案繳交。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：超音波測距感測器 HC-SR04](#認識新元件超音波測距感測器-hc-sr04)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [專案一：感應式給皂機](#專案一感應式給皂機)
- [專案二：停車格倒車引導](#專案二停車格倒車引導)
- [專案三：智慧垃圾桶](#專案三智慧垃圾桶)
- [專案四：停車場入口管理](#專案四停車場入口管理)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 圖書館座位使用偵測](#-lv1-圖書館座位使用偵測)
  - [Lv.2 遊樂設施身高檢測](#-lv2-遊樂設施身高檢測)
  - [Lv.3 社區水塔水位監測](#-lv3-社區水塔水位監測)
  - [Lv.4 自動門防夾安全](#-lv4-自動門防夾安全)
  - [Lv.5 倉庫貨架缺貨偵測](#-lv5-倉庫貨架缺貨偵測)
  - [🏆 Lv.6 BOSS 關：物聯網顧問提案](#-lv6-boss-關物聯網顧問提案)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～8 週）

- ✅ LED、RGB、蜂鳴器、伺服四種輸出元件
- ✅ 按鈕、旋鈕、光敏電阻、TMP36 四種輸入元件
- ✅ 「~ 腳可用表」：伺服借走 9、10；蜂鳴器借走 3、11
- ✅ 狀態改變偵測、鎖定與復歸、安全連鎖、優先權
- ✅ 期中專題的模組化與腳位規劃

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 超音波感測器 HC-SR04 | 發出聲波、聽回音，算出前方物體距離 | 倒車雷達、感應水龍頭、水位計 |
| 距離翻譯機 `readCM()` | 已經寫好的函式，回傳幾公分 | 把儀器讀數換成報表單位 |
| 「安裝位置換算」 | 感測器量到的是「到我的距離」，要換算成真正想知道的數字 | 從天花板量身高、從桶頂量垃圾高度 |
| 多顆感測器輪流量 | 兩顆以上一起量會互相干擾，要一顆一顆來 | 會議中大家輪流發言 |

### 本週可用元件（只限這些）

| 元件 | 第幾週學的 |
| --- | --- |
| 單色 LED、220Ω 電阻 | 第 1 週 |
| 按鈕、10kΩ 電阻 | 第 2 週 |
| RGB LED（共陰極） | 第 3 週 |
| 旋鈕 | 第 4 週 |
| 蜂鳴器、光敏電阻 | 第 5 週 |
| TMP36 | 第 6 週 |
| 伺服馬達 | 第 7 週 |
| 超音波感測器 HC-SR04 | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：期中提案大獲好評，客戶開始帶朋友來。這週的四個新委託，都需要系統「知道東西有多遠」。
>
> | 客戶 | 委託內容 | 難度 |
> | --- | --- | --- |
> | 連鎖餐廳 | 感應式給皂機 | ⭐ |
> | 社區管委會 | 停車格倒車引導 | ⭐⭐ |
> | 科技園區 | 智慧垃圾桶 | ⭐⭐⭐ |
> | 購物中心 | 停車場入口管理 | ⭐⭐⭐⭐ |
>
> 每個專案都分成 3 個步驟，從最簡單的版本開始，每一步多接幾個元件、多一項功能。

---

## 認識新元件：超音波測距感測器 HC-SR04

### 它怎麼量距離？

HC-SR04 正面有兩個圓筒，像一對「眼睛」，其實是**一張嘴和一隻耳朵**：

```
   ┌──────────────────────┐
   │  (Trig)       (Echo)  │   ← 一個發出聲波，一個聽回音
   │   嘴巴          耳朵   │
   └──┬────┬────┬────┬────┘
     VCC  Trig Echo  GND
```

1. 「嘴巴」發出人耳聽不到的超音波
2. 聲波撞到前方物體反彈回來
3. 「耳朵」聽到回音，系統計算來回花了多久，換算成公分

> 🦇 蝙蝠、潛水艇的聲納、汽車倒車雷達都是同樣的原理。

### 接法：4 隻腳，不用電阻

| HC-SR04 的腳 | 接到哪裡 |
| --- | --- |
| VCC | 5V |
| Trig（發射） | 任何一隻數位腳位 |
| Echo（接收） | 任何一隻數位腳位 |
| GND | GND |

- Trig、Echo 接**一般腳位就可以**，不需要 `~` 腳。
- **一顆 HC-SR04 要佔 2 隻腳**。系統裡有兩顆，就要 4 隻——這是本週腳位壓力的來源。
- 在 Tinkercad 開始模擬後，**點一下感測器**，前方會出現一個可以拖曳的小圓點，代表「物體」。把它拉近拉遠，就能模擬距離變化。

### 程式裡的「距離翻譯機」

本週所有程式都附上同一個函式，你**不需要修改它**，只要知道它會回傳「幾公分」：

```cpp
// 距離翻譯機：輸入 Trig、Echo 腳位，回傳公分
float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);   // 發出一聲超音波
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);               // 等回音，最多等 30 毫秒
  if (t == 0) return 999;                            // 沒聽到回音：當作很遠
  return t / 58.0;                                   // 時間換算成公分
}
```

### ⭐ 本週最重要的觀念：安裝位置換算

感測器量到的永遠是「**物體到感測器**」的距離，但客戶想知道的常常是別的數字：

| 情境 | 感測器裝在哪 | 量到的距離 | 客戶真正想知道的 | 換算方式 |
| --- | --- | --- | --- | --- |
| 倒車雷達 | 車尾 | 車尾到牆 | 車尾到牆 | 不用換算 |
| 身高測量 | 天花板（200 cm 高） | 頭頂到天花板 | 身高 | 200 − 距離 |
| 垃圾桶 | 桶蓋內側（桶深 60 cm） | 垃圾表面到桶蓋 | 垃圾裝了多滿 | (60 − 距離) ÷ 60 |
| 水塔 | 水塔頂端（深 100 cm） | 水面到頂端 | 水位還有多高 | 100 − 距離 |

> 💡 **口訣：先想清楚感測器「站在哪裡、看向哪裡」，再決定要不要換算。**

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **HC-SR04：VCC 5V、Trig 數位腳、Echo 數位腳、GND** | 不用電阻，一顆佔 2 隻腳 |
| ② Trig、Echo 不要佔 `~` 腳 | 把 `~` 腳留給需要調光的燈 |
| ③ 多顆超音波要「輪流量」 | 每量完一顆，稍等一下再量下一顆 |
| ④ 有伺服，9、10 不能調光；有蜂鳴器，3、11 不能調光 | 兩者都有時只剩 5、6 |
| ⑤ 不需要漸變的 RGB，接哪都行 | 只用全亮全滅顏色時，`setColor` 改用 `digitalWrite` |
| ⑥ 每條燈線一顆 220Ω；按鈕、光敏電阻一顆 10kΩ | HC-SR04、伺服、蜂鳴器、旋鈕、TMP36 不用電阻 |
| ⑦ Pin 0、1 先不要用；腳位不夠時借 A 腳 | A 腳也能接 Trig、Echo、燈、按鈕 |

---

## 共用工具

### 🔧 工具 1：超音波測距檢查器

接好 HC-SR04 後先跑這支。拖曳 Tinkercad 前方的小圓點，確認 Serial Monitor 顯示的公分數**跟畫面上標示的距離差不多**。

```cpp
int pinTrig = 7;   // ← 改成你 Trig 接的腳位
int pinEcho = 8;   // ← 改成你 Echo 接的腳位

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  Serial.begin(9600);
}

void loop() {
  Serial.print("Distance: ");
  Serial.print(readCM(pinTrig, pinEcho));
  Serial.println(" cm");
  delay(200);
}
```

**檢查標準**：小圓點拉到 50 cm 附近，顯示約 48～52 cm。如果一直顯示 999，表示沒收到回音——先檢查 Trig、Echo 有沒有接反。

### 🔧 其他工具（沿用前幾週）

| 工具 | 用途 | 出處 |
| --- | --- | --- |
| 接線驗證器 | 檢查所有燈的腳位 | 第 3 週起 |
| 類比讀值檢查器 | 檢查旋鈕、光敏電阻 | 第 5 週 |
| 蜂鳴器測試器 | 檢查蜂鳴器 | 第 5 週 |
| 伺服測試器 | 找出伺服的開、關角度 | 第 7 週 |

> 📌 **每個專案的固定流程**
> 1. **Step 0**：讀完情境，先填好腳位規劃表，並畫出「感測器裝在哪、朝哪看」
> 2. 把腳位填進程式的「設定區」，並確認距離門檻符合情境
> 3. 照 Step 1 → 2 → 3 逐步接線，每完成一步就把對應的功能開關改成 `true`
> 4. 每一步都先用工具確認接線，再拖曳小圓點測試

---

## 專案一：感應式給皂機

⭐ **難度**：HC-SR04 ＋ LED，再逐步加蜂鳴器與旋鈕

**客戶需求**：連鎖餐廳的洗手台要裝感應式給皂機。手伸到出口下方，給皂一次（出皂燈亮 0.8 秒），而且**手不拿開就不會再給**，避免浪費。給皂時發出短促提示音。不同分店的洗手台高度不同，感應距離要能用旋鈕調整（5～30 cm）。

### Step 0：腳位規劃

| 元件 | 腳位 | 需要 `~` 腳嗎？為什麼？ |
| --- | --- | --- |
| HC-SR04 Trig | | |
| HC-SR04 Echo | | |
| 出皂燈（單色 LED） | | |
| 提示蜂鳴器 | | |
| 感應距離旋鈕 | | — |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | HC-SR04＋出皂燈 | 小圓點拉到 10 cm 內，出皂燈亮一下；不拉開不會再亮 | （不用改） |
| Step 2 | ＋蜂鳴器 | 給皂時嗶一聲 | `useBuzzer = true` |
| Step 3 | ＋旋鈕 | 旋鈕調整感應距離 5～30 cm | `useKnob = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinTrig   = ?;      // HC-SR04 Trig
int pinEcho   = ?;      // HC-SR04 Echo
int ledPump   = ?;      // 出皂燈
int pinBuzzer = ?;      // 提示蜂鳴器
int pinKnob   = A0;     // 感應距離旋鈕
float triggerCM = 10;   // 預設感應距離（公分）
bool useBuzzer = false; // Step 2 接好後改成 true
bool useKnob   = false; // Step 3 接好後改成 true
// ============================

bool wasNear = false;
int count = 0;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(ledPump, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  if (useKnob) triggerCM = map(analogRead(pinKnob), 0, 1023, 5, 30);

  float d = readCM(pinTrig, pinEcho);
  bool near = d < triggerCM;

  if (near && !wasNear) {                 // 手剛伸進來
    count++;
    digitalWrite(ledPump, HIGH);
    if (useBuzzer) tone(pinBuzzer, 1500, 80);
    delay(800);                           // 出皂 0.8 秒
    digitalWrite(ledPump, LOW);
    Serial.print("Dispense #");
    Serial.print(count);
    Serial.print("   (trigger < ");
    Serial.print(triggerCM);
    Serial.println(" cm)");
  }
  wasNear = near;
  delay(100);
}
```

### 情境思考

1. 為什麼要設計成「手不拿開就不會再給」？如果每 0.1 秒偵測到手就給一次，一天下來會浪費多少？
2. 感應距離設太遠（例如 30 cm）會發生什麼事？（提示：洗手台下方的水槽、旁邊經過的人）
3. 💼 Serial Monitor 記錄了給皂次數。對餐廳管理者來說，這個數字可以拿來做什麼？（提示：補貨、衛生稽核、尖峰時段）

### ✅ 專案一驗收

- [ ] 手（小圓點）靠近時給皂一次，不拿開不會重複給
- [ ] 給皂時嗶一聲
- [ ] 旋鈕能調整感應距離（Serial Monitor 顯示 trigger 數值變化）
- [ ] Trig、Echo 沒有佔用 `~` 腳

---

## 專案二：停車格倒車引導

⭐⭐ **難度**：HC-SR04 ＋ RGB ＋ 蜂鳴器 ＋ 5 顆長條燈

**客戶需求**：社區地下停車場的狹窄車位，牆上裝一套倒車引導燈。駕駛倒車時，燈號告訴他還能退多少：

| 車尾到牆距離 | RGB | 蜂鳴器 | 長條燈 |
| --- | --- | --- | --- |
| 60 cm 以上 | 綠（繼續倒） | 150 cm 內開始間歇嗶 | 越近亮越多格 |
| 20～60 cm | 黃（慢慢倒） | 越近嗶越急 | |
| 20 cm 以下 | 紅（停！） | 長鳴 | 5 格全亮 |

### Step 0：腳位規劃

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| HC-SR04 Trig／Echo | | |
| 狀態 RGB（R / G / B） | | |
| 蜂鳴器 | | |
| 長條燈 × 5 | | |

> 🤔 **規劃提示**：黃色用的是「紅全亮＋綠 180」，需要調光。有蜂鳴器時 RGB 要避開哪兩隻腳？長條燈可以接在那兩隻腳嗎？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | HC-SR04＋RGB | 依距離顯示綠／黃／紅 | （不用改） |
| Step 2 | ＋蜂鳴器 | 越近嗶越急，20 cm 內長鳴 | `useBuzzer = true` |
| Step 3 | ＋5 顆長條燈 | 越近亮越多格 | `useBar = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinTrig = ?, pinEcho = ?;       // HC-SR04
int pinR = ?, pinG = ?, pinB = ?;   // 狀態 RGB（避開 3、11）
int pinBuzzer = ?;                  // 蜂鳴器
int bar[5] = { ?, ?, ?, ?, ? };     // 長條燈（遠 → 近）
float stopCM = 20;                  // 停止距離
float slowCM = 60;                  // 減速距離
float farCM  = 150;                 // 開始提示距離
bool useBuzzer = false;             // Step 2 接好後改成 true
bool useBar    = false;             // Step 3 接好後改成 true
// ============================

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setColor(int r, int g, int b) {
  analogWrite(pinR, r);
  analogWrite(pinG, g);
  analogWrite(pinB, b);
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  for (int i = 0; i < 5; i++) pinMode(bar[i], OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float d = readCM(pinTrig, pinEcho);

  if (d <= stopCM)      setColor(255, 0, 0);     // 停：紅
  else if (d <= slowCM) setColor(255, 180, 0);   // 慢：黃
  else                  setColor(0, 255, 0);     // 繼續：綠

  if (useBar) {
    float dd = constrain(d, stopCM, farCM);
    int lit = map((int)dd, (int)farCM, (int)stopCM, 0, 5);   // 越近亮越多
    for (int i = 0; i < 5; i++) {
      digitalWrite(bar[i], (i < lit) ? HIGH : LOW);
    }
  }

  Serial.print("Distance: ");
  Serial.print(d);
  Serial.println(" cm");

  if (useBuzzer && d <= stopCM) {
    tone(pinBuzzer, 1500);                      // 長鳴
    delay(100);
  } else if (useBuzzer && d <= farCM) {
    int gap = map((int)d, (int)stopCM, (int)farCM, 80, 600);   // 越近間隔越短
    tone(pinBuzzer, 1000, 50);
    delay(gap);
  } else {
    noTone(pinBuzzer);
    delay(100);
  }
}
```

### 情境思考

1. 感測器要裝在牆上的什麼高度？太低（地板附近）或太高（天花板附近）各會量到什麼？
2. 長條燈只需要亮滅，如果接在 `~3` 或 `~11` 會有問題嗎？為什麼？
3. 💼 管委會問：「既然新車都有倒車雷達，為什麼還要在牆上裝？」請從「誰的車都能用」「維護成本」「事故糾紛」三個角度回答。

### ✅ 專案二驗收

- [ ] RGB 依距離顯示綠／黃／紅，且 RGB 沒有接在 3、11
- [ ] 越近嗶得越急，20 cm 內長鳴，150 cm 外安靜
- [ ] 長條燈越近亮越多格
- [ ] 腳位規劃表說明長條燈能不能用 3、11

---

## 專案三：智慧垃圾桶

⭐⭐⭐ **難度**：**2 顆 HC-SR04**（一顆朝外、一顆朝內）＋ 伺服 ＋ 滿桶連鎖

**客戶需求**：科技園區的公共區域要換成智慧垃圾桶：

- **桶外**一顆超音波：有人靠近（25 cm 內）時，桶蓋自動打開；人離開 2 秒後關上
- **桶內**一顆超音波（裝在桶蓋內側往下看）：量垃圾表面高度，換算「裝了幾成滿」，用三顆燈顯示：空、半滿、快滿
- **滿桶連鎖**：裝到 85% 以上時，桶蓋**不再打開**（避免垃圾滿出來），並發出提醒聲；清潔人員按「維護鈕」可以強制開蓋 5 秒

### Step 0：腳位規劃

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| 桶外 HC-SR04 Trig／Echo | | |
| 桶內 HC-SR04 Trig／Echo | | |
| 桶蓋伺服 | | |
| 容量燈（空／半滿／快滿） | | |
| 蜂鳴器 | | |
| 維護鈕 | | |

> 🤔 **規劃提示**：
> - 兩顆 HC-SR04 共要幾隻腳？麵包板上怎麼擺，才不會把「桶外」和「桶內」的線接錯？
> - 先畫出示意圖：一顆朝外看人、一顆朝下看垃圾。

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 桶外 HC-SR04＋伺服 | 有人靠近開蓋，離開 2 秒後關蓋 | （不用改） |
| Step 2 | ＋桶內 HC-SR04＋3 顆容量燈 | 依垃圾高度顯示空／半滿／快滿 | `useFill = true` |
| Step 3 | ＋蜂鳴器＋維護鈕 | 85% 以上不開蓋並提醒；維護鈕強制開蓋 5 秒 | `useFullLock = true` |

### 程式（只改設定區）

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int trigOut = ?, echoOut = ?;       // 桶外 HC-SR04（偵測人）
int trigIn  = ?, echoIn  = ?;       // 桶內 HC-SR04（偵測垃圾高度）
int pinServo  = ?;                  // 桶蓋伺服
int fillLed[3] = { ?, ?, ? };       // 容量燈：空、半滿、快滿
int pinBuzzer = ?;                  // 蜂鳴器
int btnService = ?;                 // 維護鈕
float openCM   = 25;                // 有人靠近的距離
float binDepth = 60;                // 桶蓋到桶底的深度（公分）
int closedAngle = 0, openAngle = 100;
bool useFill     = false;           // Step 2 接好後改成 true
bool useFullLock = false;           // Step 3 接好後改成 true
// ============================

Servo lid;
unsigned long lastNear = 0;
bool wasFull = false;
int lastService = LOW;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setup() {
  lid.attach(pinServo);
  lid.write(closedAngle);
  pinMode(trigOut, OUTPUT); pinMode(echoOut, INPUT);
  pinMode(trigIn, OUTPUT);  pinMode(echoIn, INPUT);
  for (int i = 0; i < 3; i++) pinMode(fillLed[i], OUTPUT);
  pinMode(btnService, INPUT);
  Serial.begin(9600);
}

void loop() {
  // ---- 桶內：垃圾裝了幾成？ ----
  bool full = false;
  if (useFill) {
    float dIn = readCM(trigIn, echoIn);
    int level = (int)((binDepth - dIn) / binDepth * 100);   // 安裝位置換算
    level = constrain(level, 0, 100);
    int idx = 0;                                             // 0=空, 1=半滿, 2=快滿
    if (level >= 85)      idx = 2;
    else if (level >= 50) idx = 1;
    for (int i = 0; i < 3; i++) {
      digitalWrite(fillLed[i], (i == idx) ? HIGH : LOW);
    }
    full = (level >= 85);
    delay(30);                                               // 輪流量，避免互相干擾
  }

  // ---- 滿桶連鎖 ----
  if (useFullLock) {
    if (full && !wasFull) {
      Serial.println("BIN FULL - please empty");
      tone(pinBuzzer, 800, 500);
    }
    wasFull = full;

    int now = digitalRead(btnService);
    if (now == HIGH && lastService == LOW) {
      delay(50);
      Serial.println("Service mode: lid open 5 s");
      lid.write(openAngle);
      delay(5000);
      lid.write(closedAngle);
    }
    lastService = now;

    if (full) {
      lid.write(closedAngle);                                // 滿了不開蓋
      delay(100);
      return;
    }
  }

  // ---- 桶外：有人靠近就開蓋 ----
  float dOut = readCM(trigOut, echoOut);
  if (dOut < openCM) {
    lid.write(openAngle);
    lastNear = millis();
  } else if (millis() - lastNear > 2000) {
    lid.write(closedAngle);                                  // 人離開 2 秒後關蓋
  }
  delay(100);
}
```

### 情境思考

1. 桶內感測器量到 9 cm，表示垃圾裝了幾成滿？請寫出換算過程。
2. 測試時，拖曳「桶外」的小圓點，卻是容量燈在變。最可能哪裡接錯？用哪個工具最快確認？
3. 💼 如果每個垃圾桶都能回報「幾成滿」，清潔公司的排班方式可以怎麼改變？（提示：固定路線 vs. 依需求派車）

### ✅ 專案三驗收

- [ ] 有人靠近時開蓋，離開 2 秒後關蓋
- [ ] 容量燈依垃圾高度正確顯示空／半滿／快滿
- [ ] 85% 以上時有人靠近也不開蓋，並響提醒聲（只響一次）
- [ ] 維護鈕能強制開蓋 5 秒
- [ ] 兩顆 HC-SR04 在麵包板上分區清楚，能說明哪顆朝外、哪顆朝內

---

## 專案四：停車場入口管理

⭐⭐⭐⭐ **難度**：入口、出口**各一顆 HC-SR04** ＋ 伺服柵欄 ＋ RGB ＋ 蜂鳴器 ＋ 4 顆剩餘車位燈 ＋ 旋鈕 → **腳位不夠，要借 A 腳**

**客戶需求**：購物中心停車場的入口和出口各裝一顆超音波感測器（朝向車道）：

- **入口**：車子開到柵欄前（50 cm 內）時，若還有空位，柵欄升起，等車子通過後 1 秒放下，場內車數 +1；若客滿，柵欄不開並發出拒絕聲
- **出口**：車子經過時，場內車數 −1
- **剩餘車位看板**：RGB 顯示綠（充足）／黃（剩 2 格以下）／紅（客滿）；4 顆燈顯示剩餘比例
- **總車位數**：管理員用旋鈕設定（1～20 格），因應部分車位施工封閉

### Step 0：腳位規劃（本專案的重頭戲）

| 元件 | 需要幾隻腳 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- | --- |
| 總車位旋鈕 | 1（A 腳） | A0 | — |
| 入口 HC-SR04 Trig／Echo | 2 | | |
| 出口 HC-SR04 Trig／Echo | 2 | | |
| 柵欄伺服 | | | |
| 看板 RGB | | | |
| 蜂鳴器 | | | |
| 剩餘車位燈 × 4 | | | |
| **一般腳位需求合計** | | | |
| **Pin 2～13 可用數量** | 12 | | |

> 🤔 **規劃提示**：
> - 一般腳位夠不夠？不夠的從 A1～A5 借，借來的 A 腳適合接什麼？
> - 伺服＋蜂鳴器同時存在，RGB 能調光嗎？本專案的黃色是「紅＋綠全亮」，需要調光嗎？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 入口 HC-SR04＋伺服＋蜂鳴器 | 車來柵欄升、通過後放下，計數；滿 8 台拒絕 | （不用改） |
| Step 2 | ＋出口 HC-SR04＋RGB | 出場車數 −1；看板顯示綠／黃／紅 | `useExit = true` |
| Step 3 | ＋旋鈕＋4 顆剩餘車位燈 | 旋鈕設定總車位；4 顆燈顯示剩餘比例 | `useBoard = true` |

### 程式（只改設定區）

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int trigIn  = ?, echoIn  = ?;       // 入口 HC-SR04
int trigOut = ?, echoOut = ?;       // 出口 HC-SR04
int pinServo  = ?;                  // 柵欄伺服
int pinR = ?, pinG = ?, pinB = ?;   // 看板 RGB（只用全亮全滅顏色）
int pinBuzzer = ?;                  // 蜂鳴器
int slotLed[4] = { ?, ?, ?, ? };    // 剩餘車位燈
int pinKnob = A0;                   // 總車位旋鈕
int capacity = 8;                   // 預設總車位
float carCM = 50;                   // 車子到感測器的距離小於這個值算「有車」
int closedAngle = 0, openAngle = 90;
bool useExit  = false;              // Step 2 接好後改成 true
bool useBoard = false;              // Step 3 接好後改成 true
// ============================

Servo barrier;
int cars = 0;
bool wasCarIn = false, wasCarOut = false;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setColor(int r, int g, int b) {      // 全亮全滅，不需要 ~ 腳
  digitalWrite(pinR, r);
  digitalWrite(pinG, g);
  digitalWrite(pinB, b);
}

void setup() {
  barrier.attach(pinServo);
  barrier.write(closedAngle);
  pinMode(trigIn, OUTPUT);  pinMode(echoIn, INPUT);
  pinMode(trigOut, OUTPUT); pinMode(echoOut, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  for (int i = 0; i < 4; i++) pinMode(slotLed[i], OUTPUT);
  Serial.begin(9600);
}

void loop() {
  if (useBoard) capacity = map(analogRead(pinKnob), 0, 1023, 1, 20);
  if (cars > capacity) cars = capacity;

  // ---- 入口：車剛開到柵欄前 ----
  bool carIn = readCM(trigIn, echoIn) < carCM;
  if (carIn && !wasCarIn) {
    if (cars >= capacity) {
      tone(pinBuzzer, 300, 600);
      Serial.println("FULL - entry refused");
    } else {
      tone(pinBuzzer, 1200, 100);
      barrier.write(openAngle);
      Serial.println("Barrier UP");
      while (readCM(trigIn, echoIn) < carCM) {   // 等車子通過
        delay(100);
      }
      delay(1000);
      barrier.write(closedAngle);
      cars++;
      carIn = false;
      Serial.print("Car IN. Parked: ");
      Serial.print(cars);
      Serial.print("/");
      Serial.println(capacity);
    }
  }
  wasCarIn = carIn;
  delay(30);                                     // 輪流量

  // ---- 出口：車剛經過 ----
  if (useExit) {
    bool carOut = readCM(trigOut, echoOut) < carCM;
    if (carOut && !wasCarOut && cars > 0) {
      cars--;
      Serial.print("Car OUT. Parked: ");
      Serial.print(cars);
      Serial.print("/");
      Serial.println(capacity);
    }
    wasCarOut = carOut;

    int left = capacity - cars;
    if (left <= 0)      setColor(HIGH, LOW, LOW);    // 客滿：紅
    else if (left <= 2) setColor(HIGH, HIGH, LOW);   // 快滿：黃
    else                setColor(LOW, HIGH, LOW);    // 充足：綠
  }

  // ---- 剩餘車位燈：剩越多亮越多 ----
  if (useBoard) {
    int left = capacity - cars;
    int lit = (left * 4 + capacity - 1) / capacity;  // 剩餘比例換算成 0~4 格（有剩就至少亮 1 格）
    for (int i = 0; i < 4; i++) {
      digitalWrite(slotLed[i], (i < lit) ? HIGH : LOW);
    }
  }
  delay(70);
}
```

### 情境思考

1. 用你的腳位規劃表說明：這個專案共需要幾隻一般腳？你借了哪幾隻 A 腳、接了什麼？
2. 入口柵欄打開後，程式會「等到車子離開感測器範圍」才放下。如果車子停在柵欄下不走（例如駕駛在找停車票），柵欄會怎樣？這是好的設計嗎？
3. 💼 如果出口感測器故障、一直沒偵測到車出去，系統會出現什麼問題？管理員要怎麼發現？你會建議加什麼「異常檢查」？

### ✅ 專案四驗收

- [ ] 入口有車且未客滿：柵欄升起 → 車通過後放下 → 車數 +1
- [ ] 客滿時柵欄不開並發出拒絕聲
- [ ] 出口有車經過時車數 −1，不會變負數
- [ ] 看板 RGB 依剩餘車位顯示綠／黃／紅
- [ ] 旋鈕能改變總車位，4 顆剩餘車位燈正確反映比例
- [ ] 腳位規劃表正確處理「一般腳不夠、借 A 腳」

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：四個範例專案是「跟著做」，這一區換你「自己規劃」。每一關都是新情境、新的元件組合，你要先判斷需要什麼元件、接幾顆、接哪隻腳，**還要先畫出感測器裝在哪、朝哪看**，再動手接線。
>
> 📈 **難度安排**：
>
> | 關卡 | HC-SR04 | 其他元件 | 配線重點 |
> | --- | --- | --- | --- |
> | Lv.1 | 1 | 2 LED | 「持續一段時間才算」的判斷 |
> | Lv.2 | 1 | RGB＋蜂鳴器 | 裝在天花板，距離要換算成身高 |
> | Lv.3 | 1 | 5 LED＋蜂鳴器 | 裝在水塔頂端，距離換算成水位 |
> | Lv.4 | 1 | 伺服＋蜂鳴器＋2 LED | 動作中持續偵測（防夾） |
> | Lv.5 | 3 | 6 LED＋蜂鳴器＋按鈕 | 一般腳不夠，三顆輪流量 |
> | Lv.6 | 自訂 | 自訂 | 顧問提案 |
>
> 📌 **作答規則**：每一關先填腳位規劃表、畫感測器安裝示意圖，再接線；接好後先用測距檢查器確認，最後才上傳程式。程式只需要改「設定區」。

---

### 🟢 Lv.1 圖書館座位使用偵測

**情境**：大學圖書館想知道自習座位的使用率。每張桌子底下裝一顆超音波，朝向椅子方向：有人坐著時距離很近。但經過的人、拉一下椅子也會被偵測到，所以規則是：**連續 3 秒都偵測到才算「有人使用」**。紅燈表示有人、綠燈表示空位。

**元件**：1 顆 HC-SR04＋2 顆單色 LED（紅、綠）

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinTrig = ?, pinEcho = ?;   // HC-SR04
int ledBusy = ?;                // 有人：紅燈
int ledFree = ?;                // 空位：綠燈
float seatCM = 60;              // 小於這個距離算「有人在座位上」
int confirmCount = 15;          // 連續幾次（每次 0.2 秒）才算有人
// ============================

int nearCount = 0;
bool occupied = false;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(ledBusy, OUTPUT);
  pinMode(ledFree, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float d = readCM(pinTrig, pinEcho);
  if (d < seatCM) nearCount++;
  else            nearCount = 0;

  bool nowOccupied = (nearCount >= confirmCount);
  if (nowOccupied != occupied) {
    occupied = nowOccupied;
    Serial.println(occupied ? "Seat TAKEN" : "Seat FREE");
  }
  digitalWrite(ledBusy, occupied ? HIGH : LOW);
  digitalWrite(ledFree, occupied ? LOW : HIGH);
  delay(200);
}
```

**情境思考**：

1. 把 `confirmCount` 改成 1 和 50，分別會發生什麼事？各有什麼缺點？
2. 💼 有人把書包放在椅子上「佔位」後離開兩小時，這個系統偵測得出來嗎？圖書館管理上可以怎麼搭配？

---

### 🟢 Lv.2 遊樂設施身高檢測

**情境**：遊樂園雲霄飛車入口要自動檢查身高，身高 **120 cm 以上**才能搭乘。感測器裝在入口上方 **200 cm** 高的橫桿上，往下量到頭頂的距離。有人站進來時量一次：符合亮綠燈、短嗶一聲；不符合亮紅燈、低音提示。

**元件**：1 顆 HC-SR04＋1 顆 RGB LED＋1 顆蜂鳴器

**你要想的問題**：

- 量到 85 cm，這個人身高幾公分？
- 有蜂鳴器，RGB 要避開哪兩隻腳？（本關只用紅、綠、藍三色，也可以接一般腳，請說明你的選擇）

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinTrig = ?, pinEcho = ?;       // HC-SR04（裝在橫桿上往下量）
int pinR = ?, pinG = ?, pinB = ?;   // 結果 RGB
int pinBuzzer = ?;                  // 蜂鳴器
float mountCM  = 200;               // 感測器離地高度
float minHeight = 120;              // 最低身高
// ============================

bool wasPerson = false;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setColor(int r, int g, int b) {
  digitalWrite(pinR, r);
  digitalWrite(pinG, g);
  digitalWrite(pinB, b);
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float d = readCM(pinTrig, pinEcho);
  float height = mountCM - d;                 // 安裝位置換算
  bool person = (height > 50);                // 高於 50 cm 才算有人站進來

  if (person && !wasPerson) {                 // 剛站進來，量一次
    delay(500);                               // 等站穩
    height = mountCM - readCM(pinTrig, pinEcho);
    Serial.print("Height: ");
    Serial.print(height);
    if (height >= minHeight) {
      Serial.println(" cm - OK, enjoy the ride!");
      setColor(LOW, HIGH, LOW);
      tone(pinBuzzer, 1500, 150);
    } else {
      Serial.println(" cm - Sorry, too short");
      setColor(HIGH, LOW, LOW);
      tone(pinBuzzer, 300, 500);
    }
  }
  if (!person) setColor(LOW, LOW, HIGH);      // 等待中：藍
  wasPerson = person;
  delay(150);
}
```

**情境思考**：

1. 小朋友戴了一頂 5 cm 高的帽子、或踮腳尖，系統會怎麼判斷？現場人員應該怎麼配合？
2. 💼 身高限制是安全規定。如果系統誤判讓不合格的小朋友上去，責任在誰？這告訴你自動化系統在「安全相關決策」上應該扮演什麼角色？

---

### 🟡 Lv.3 社區水塔水位監測

**情境**：社區頂樓水塔深 **100 cm**，超音波裝在水塔頂蓋往下量水面。管理室的面板用 5 顆燈顯示水位（每格 20%），水位低於 **20%** 時蜂鳴器每 2 秒提醒一次，請管委會聯絡補水。

**元件**：1 顆 HC-SR04＋5 顆單色 LED＋1 顆蜂鳴器

**你要想的問題**：

- 量到 30 cm，代表水位剩幾成？
- 這一關有需要 `~` 腳的元件嗎？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinTrig = ?, pinEcho = ?;       // HC-SR04（裝在頂蓋往下量）
int bar[5] = { ?, ?, ?, ?, ? };     // 水位燈（低 → 高）
int pinBuzzer = ?;                  // 低水位提醒
float tankDepth = 100;              // 頂蓋到塔底深度（公分）
int lowPercent  = 20;               // 低水位警戒（%）
// ============================

int tick = 0;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  for (int i = 0; i < 5; i++) pinMode(bar[i], OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float d = readCM(pinTrig, pinEcho);
  int level = (int)((tankDepth - d) / tankDepth * 100);   // 安裝位置換算
  level = constrain(level, 0, 100);

  int lit = (level + 19) / 20;                            // 每 20% 一格
  for (int i = 0; i < 5; i++) {
    digitalWrite(bar[i], (i < lit) ? HIGH : LOW);
  }

  tick++;
  if (level < lowPercent && tick % 10 == 0) {             // 約每 2 秒
    tone(pinBuzzer, 900, 200);
    Serial.println("LOW WATER - please refill");
  }
  if (tick % 5 == 0) {
    Serial.print("Water level: ");
    Serial.print(level);
    Serial.println(" %");
  }
  delay(200);
}
```

**情境思考**：

1. 水塔頂蓋到「水塔滿水時的水面」其實還有 10 cm 空隙。這樣換算會出現什麼誤差？要改設定區的哪個數字、怎麼改？
2. 💼 管委會想知道「每天用了多少水」。只靠這個系統做得到嗎？還需要記錄什麼？

---

### 🟡 Lv.4 自動門防夾安全

**情境**：診所的自動門。有人走近（50 cm 內）時門打開，停 3 秒後慢慢關上。**關門過程中只要偵測到有人在門口（30 cm 內），立刻重新打開**，並發出提醒聲——這就是「防夾」功能。開門燈表示門開著，警示燈在防夾觸發時閃一下。

**元件**：1 顆 HC-SR04＋1 顆伺服＋1 顆蜂鳴器＋2 顆單色 LED

**你要想的問題**：

- 伺服和蜂鳴器同時存在，這關的兩顆 LED 需要 `~` 腳嗎？
- 感測器要裝在門的哪裡、朝哪個方向，才能偵測到「站在門口」的人？

**程式**：

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int pinTrig = ?, pinEcho = ?;   // HC-SR04
int pinServo  = ?;              // 自動門伺服
int pinBuzzer = ?;              // 蜂鳴器
int ledOpen   = ?;              // 開門燈
int ledWarn   = ?;              // 防夾警示燈
float openCM  = 50;             // 有人靠近就開門
float safeCM  = 30;             // 關門時，門口有人就重開
int closedAngle = 0, openAngle = 90;
// ============================

Servo door;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void openDoor() {
  door.write(openAngle);
  digitalWrite(ledOpen, HIGH);
}

void setup() {
  door.attach(pinServo);
  door.write(closedAngle);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(ledOpen, OUTPUT);
  pinMode(ledWarn, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  if (readCM(pinTrig, pinEcho) < openCM) {
    openDoor();
    Serial.println("Door OPEN");
    delay(3000);

    // 慢慢關門，每一步都檢查門口有沒有人
    int a = openAngle;
    while (a > closedAngle) {
      if (readCM(pinTrig, pinEcho) < safeCM) {
        Serial.println("ANTI-PINCH: reopening");
        digitalWrite(ledWarn, HIGH);
        tone(pinBuzzer, 1200, 200);
        openDoor();
        delay(3000);
        digitalWrite(ledWarn, LOW);
        a = openAngle;                       // 從全開重新開始關
        continue;
      }
      a = a - 2;
      door.write(a);
      delay(40);
    }
    door.write(closedAngle);
    digitalWrite(ledOpen, LOW);
    Serial.println("Door CLOSED");
  }
  delay(100);
}
```

**情境思考**：

1. 在關門過程中把小圓點拉近 20 cm，門有沒有重新打開？如果把 `safeCM` 設成 5 cm，防夾還有效嗎？
2. 💼 防夾功能是法規要求的安全設計。如果你是診所老闆，除了這個自動功能，你還會在管理上做什麼？（提示：定期檢查、標示、員工訓練）

---

### 🔴 Lv.5 倉庫貨架缺貨偵測

**情境**：便利商店的後倉貨架有三格，每格深 **40 cm**，最深處各裝一顆超音波往外量：有貨時距離短，缺貨時會量到貨架外的空間（距離長）。每格有一紅一綠兩顆燈（綠＝有貨、紅＝缺貨）；某一格**剛變成缺貨**時蜂鳴器嗶一聲。店長按「盤點鈕」時，Serial Monitor 印出三格的狀態報表。

**元件**：3 顆 HC-SR04＋6 顆單色 LED＋1 顆蜂鳴器＋1 顆按鈕

**Step 0：腳位規劃（本關重點）**

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 3 顆 HC-SR04（Trig／Echo） | | |
| 6 顆格位燈 | | |
| 蜂鳴器 | | |
| 盤點鈕 | | |
| **合計** | | |
| **Pin 2～13 可用數量** | 12 | |

> 🤔 **規劃提示**：
> - 一般腳不夠時，A0～A5 都可以借來接 Trig、Echo、燈或按鈕。
> - 三顆感測器朝同一個方向，聲波會互相干擾。程式已經「輪流量」，但麵包板上的擺放也要清楚標示格位 1、2、3。

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int trig[3] = { ?, ?, ? };          // 三格的 Trig
int echo[3] = { ?, ?, ? };          // 三格的 Echo
int ledOK[3]    = { ?, ?, ? };      // 有貨：綠燈
int ledEmpty[3] = { ?, ?, ? };      // 缺貨：紅燈
int pinBuzzer = ?;                  // 蜂鳴器
int btnReport = ?;                  // 盤點鈕
float stockCM = 30;                 // 小於這個距離算「有貨」
// ============================

bool wasEmpty[3] = { false, false, false };
int lastButton = LOW;

float readCM(int t, int e) {
  digitalWrite(t, LOW);  delayMicroseconds(2);
  digitalWrite(t, HIGH); delayMicroseconds(10);
  digitalWrite(t, LOW);
  long us = pulseIn(e, HIGH, 30000);
  if (us == 0) return 999;
  return us / 58.0;
}

void setup() {
  for (int s = 0; s < 3; s++) {
    pinMode(trig[s], OUTPUT);
    pinMode(echo[s], INPUT);
    pinMode(ledOK[s], OUTPUT);
    pinMode(ledEmpty[s], OUTPUT);
  }
  pinMode(btnReport, INPUT);
  Serial.begin(9600);
}

void loop() {
  bool empty[3];
  for (int s = 0; s < 3; s++) {
    float d = readCM(trig[s], echo[s]);
    empty[s] = (d >= stockCM);
    digitalWrite(ledOK[s],    empty[s] ? LOW : HIGH);
    digitalWrite(ledEmpty[s], empty[s] ? HIGH : LOW);

    if (empty[s] && !wasEmpty[s]) {          // 剛變成缺貨
      tone(pinBuzzer, 1000, 150);
      Serial.print("Slot ");
      Serial.print(s + 1);
      Serial.println(" OUT OF STOCK");
    }
    wasEmpty[s] = empty[s];
    delay(40);                               // 輪流量，避免互相干擾
  }

  int now = digitalRead(btnReport);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    Serial.println("===== Stock Report =====");
    int lack = 0;
    for (int s = 0; s < 3; s++) {
      Serial.print("Slot ");
      Serial.print(s + 1);
      Serial.println(empty[s] ? ": EMPTY" : ": OK");
      if (empty[s]) lack++;
    }
    Serial.print("Slots to restock: ");
    Serial.println(lack);
  }
  lastButton = now;
}
```

**情境思考**：

1. 你的腳位規劃表一共用了幾隻腳？借了哪幾隻 A 腳？還能再加第四格嗎？
2. 某一格只剩最後面一盒貨（離感測器 5 cm），其他都賣光了。系統會顯示有貨還是缺貨？這對補貨有什麼影響？
3. 💼 「缺貨才補」和「快缺貨就補」哪一種比較好？要做到「快缺貨就補」，這個系統要怎麼改？（提示：第 4 週倉庫看板的「再訂購點」）

---

### 🏆 Lv.6 BOSS 關：物聯網顧問提案

**情境**：你的顧問公司接到新客戶。從下面三個案例中**任選一個**，自己完成需求分析、元件選擇、腳位規劃與 Tinkercad 實作。

> **案例 A：智慧停車格地鎖**
> 每個車位有一個地鎖（伺服）。車主按按鈕預約後地鎖降下，超音波偵測到車子停入後亮紅燈；車子離開後地鎖自動升起。若預約後 10 秒內沒有車停入，地鎖自動升回並發出提示。
>
> **案例 B：物流月台貨車對位**
> 貨車倒車靠月台時，超音波量車尾距離，RGB 與蜂鳴器引導；停妥（10 cm 內）後月台的升降板（伺服）放下。裝卸完成按按鈕，升降板收起、綠燈示意貨車可以離開。
>
> **案例 C：自訂情境**
> 自己找一個生活或職場情境（例如：電梯門口人潮偵測、無障礙廁所使用中顯示、超商排隊人數偵測、工地安全距離警示）。

**最低規格**：

- 至少 1 顆 HC-SR04、1 顆按鈕、1 顆 RGB LED，以及伺服或蜂鳴器至少一種
- 總共使用 10 隻以上腳位
- 必須說明感測器的**安裝位置與方向**，以及量到的距離如何換算成客戶要的數字
- 程式可以從專案一～四或 Lv.1～Lv.5 改寫，說明你是從哪裡改來的、改了哪些設定

**繳交內容**：

1. **需求分析**：這個系統解決什麼問題？誰會操作？誰會看燈、聽聲音？（3～5 句）
2. **感測器安裝示意圖**：畫出感測器在真實場地的位置、朝向，並標示距離換算方式
3. **腳位規劃表**：標註每隻腳接什麼、是否為 `~` 腳、還剩幾隻可擴充
4. **Tinkercad 實作**：電路截圖＋測距檢查器的截圖
5. **誤判分析**：你的系統在什麼情況下可能誤判？你怎麼降低誤判？

**驗收標準**：

- [ ] HC-SR04 四隻腳接對，測距檢查器讀數與 Tinkercad 標示一致
- [ ] Trig、Echo 沒有佔用需要的 `~` 腳；需要調光的燈避開被借走的腳
- [ ] 距離換算正確，符合安裝位置
- [ ] 有多顆超音波時有「輪流量」，且麵包板分區清楚
- [ ] 功能符合情境需求

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 距離一直顯示 999 | Trig、Echo 是否接反；程式腳位是否和實際一致 | 用測距檢查器確認後對調 |
| 距離一直是 0 或很小的固定值 | VCC、GND 是否接好 | 重新接 5V 與 GND |
| 拖曳小圓點，距離沒變化 | 是否點對感測器；程式腳位是否對應這一顆 | 逐顆用測距檢查器確認 |
| 有兩顆感測器，拖 A 卻是 B 的功能在動 | 設定區兩組 Trig／Echo 是否填反 | 對照麵包板重填 |
| 多顆感測器讀數偶爾亂跳 | 是否同時量（沒有輪流） | 每顆量完加短暫 `delay` |
| 換算後的身高、水位明顯不對 | 安裝高度、深度的設定值 | 修改設定區的 `mountCM`、`tankDepth`、`binDepth` |
| 調光燈只有全亮全滅 | 是否接在被伺服或蜂鳴器借走的 `~` 腳 | 查「~ 腳可用表」改接 |
| 程式無法上傳 | 設定區是否還有 `?` 沒填 | 填完所有腳位 |

---

## 成果繳交與評分

### 範例專案繳交

每個專案各自建立一個 Tinkercad 檔案，命名格式：`W9-專案編號-學號`（例如 `W9-P3-B12345678`）。

| 繳交項目 | 專案一 | 專案二 | 專案三 | 專案四 |
| --- | --- | --- | --- | --- |
| Tinkercad 分享連結 | ✅ | ✅ | ✅ | ✅ |
| 腳位規劃表 | ✅ | ✅ | ✅ | ✅ |
| 感測器安裝示意圖 | — | ✅ | ✅ | ✅ |
| 完成 Step 3 的電路截圖 | ✅ | ✅ | ✅ | ✅ |
| 情境思考題作答 | ✅ | ✅ | ✅ | ✅ |

> 🎯 **最低完成標準**：專案一、二完成 Step 3，專案三、四至少完成 Step 2。

### 練習題繳交

| 關卡 | 繳交內容 | 建議定位 |
| --- | --- | --- |
| Lv.1～Lv.2 | Tinkercad 連結＋情境思考題 | 課堂暖身，全班必做 |
| Lv.3～Lv.4 | ＋腳位規劃表＋安裝示意圖 | 課後作業，必做 |
| Lv.5 | ＋腳位規劃表＋麵包板擺放截圖 | 選做加分 |
| Lv.6 BOSS | 完整顧問提案（需求分析、安裝示意圖、腳位規劃、實作、誤判分析） | 可分組 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 電路正確性 | 30% | 每個 Step 的功能都能正常運作 |
| 腳位規劃 | 20% | 規劃表完整，正確處理多顆感測器與 `~` 腳限制 |
| 感測器安裝與換算 | 20% | 安裝示意圖清楚，距離換算正確 |
| 情境思考題 | 20% | 尤其是誤判分析與 💼 管理面向論述 |
| 麵包板擺放 | 10% | 多顆感測器分區清楚、標示明確 |

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 30 分鐘 | 認識 HC-SR04、安裝位置換算、測距檢查器示範 |
| 第 1 節後段 | 專案一（全班一起） |
| 第 2 節 | 專案二、專案三（分組進行） |
| 第 3 節前段 | 專案四 |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，Lv.3 之後帶回家 |

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 超音波感測器 HC-SR04 | 發出聲波、聽回音，算出前方物體距離的四腳元件 |
| Trig／Echo | Trig 負責「發出」聲波，Echo 負責「聽」回音 |
| `readCM()` 距離翻譯機 | 已寫好的函式，回傳物體到感測器的公分數 |
| 安裝位置換算 | 把「到感測器的距離」換成客戶真正想知道的數字（身高、水位、滿度） |
| 輪流量測 | 多顆超音波一顆一顆量，避免聲波互相干擾 |
| 持續確認 | 連續一段時間都偵測到才算數，降低誤判 |
| 防夾（Anti-pinch） | 門或柵欄關閉時偵測到障礙物就立刻停止或重開 |
| 誤判（False Alarm） | 系統判斷和實際狀況不一致 |

---

## 反思與延伸討論

1. 本週的超音波在不同情境中，量到的距離有時「直接就是答案」（倒車雷達），有時要「換算」（身高、水位、垃圾滿度）。遇到新情境時，你會先問自己哪些問題，來決定怎麼安裝、怎麼換算？
2. Lv.1 的「連續 3 秒才算」、專案一的「手不拿開就不再給」、Lv.5 的「剛變成缺貨才嗶」，都是為了減少誤判或重複觸發。請說明這三種做法各自解決了什麼問題。
3. 本週好幾個情境（垃圾桶、停車場、貨架、座位）都能產生「使用數據」。如果把這些數據集中起來，管理者可以做出哪些過去做不到的決策？這就是物聯網（IoT）真正的價值所在。

---

## 本週檢核清單

- [ ] 能正確接上 HC-SR04（VCC 5V、Trig、Echo、GND），並用測距檢查器確認
- [ ] 能在 Tinkercad 拖曳小圓點模擬距離變化
- [ ] 能依安裝位置把距離換算成身高、水位、滿度等數字
- [ ] 能安排兩顆以上超音波的腳位與擺放，並理解為什麼要輪流量
- [ ] 能在一般腳位不足時借用 A 腳接 Trig、Echo
- [ ] 能說明本週情境中的誤判來源與降低方法
- [ ] 完成 4 個獨立專案，並各自繳交 Tinkercad 檔案
- [ ] 完成練習題 Lv.1～Lv.4，挑戰 Lv.5 與 Lv.6

---

## 下週預告

第 10 週我們要加入**直流馬達與 L293D 馬達驅動晶片**，打造「智慧輸送帶」。伺服馬達只能轉到某個角度停住，直流馬達則可以**一直轉**，而且能控制轉速和方向。這是本學期最後一個新的動力元件，搭配這週的超音波，就能做出「前方有東西就停下」的自動輸送系統。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 8 週](week-08.md) | [下一週：第 10 週 — 智慧輸送帶 ➡](week-10.md)
