[⬅ 回目錄](../README.md) | [⬅ 上一週：第 8 週](week-08.md) | [下一週：第 10 週 — 會持續運轉的智慧場域 ➡](week-10.md)

---

# 第 9 週：會量距離的智慧場域

## 主題專案：讓系統知道「東西離我多遠」

> 🎯 **本週定位**：期中專題結束，顧問公司接案能力大增。這一週加入新感官：**超音波測距感測器 HC-SR04**。前面學過的感測器只能回答「亮不亮」「熱不熱」，超音波可以直接回答「**離我幾公分**」。
>
> 超音波一顆就佔 **2 隻腳**，系統裡有兩顆就是 4 隻，**一般腳位不夠、要借 A 腳**是本週最大的配線挑戰。另外，感測器「裝在哪裡、朝哪裡看」決定了量到的數字代表什麼——這是本週最重要的情境思考。
>
> 每一個關卡照「五個固定環節」進行：抽**客戶限制卡** → 回答**接線前決策題** → 照**接線任務**一步步接 → 套用**程式模板**（只改腳位區、參數區） → 完成**配線變化挑戰**（程式一個字都不改，只改線）。
>
> 🏁 **本週成果**：完成 **4 個關卡** ＋ **練習題 Lv.1～Lv.6**（套用本週的程式模板 T17～T20）。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：超音波測距感測器 HC-SR04](#認識新元件超音波測距感測器-hc-sr04)
- [本週配線新技巧](#本週配線新技巧)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [關卡怎麼玩：五個固定環節](#關卡怎麼玩五個固定環節)
- [關卡 1：感應式給皂機（模板 T17）](#關卡-1感應式給皂機模板-t17)
- [關卡 2：停車格倒車引導（模板 T18）](#關卡-2停車格倒車引導模板-t18)
- [關卡 3：智慧垃圾桶（模板 T19）](#關卡-3智慧垃圾桶模板-t19)
- [關卡 4：停車場出入口管理（模板 T20）](#關卡-4停車場出入口管理模板-t20)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 圖書館座位使用偵測](#-lv1-圖書館座位使用偵測)
  - [Lv.2 遊樂設施身高檢測](#-lv2-遊樂設施身高檢測)
  - [Lv.3 社區水塔水位面板](#-lv3-社區水塔水位面板)
  - [Lv.4 診所自動門防夾](#-lv4-診所自動門防夾)
  - [Lv.5 展覽館人流管制](#-lv5-展覽館人流管制)
  - [🏆 Lv.6 BOSS 關：顧問提案](#-lv6-boss-關顧問提案)
  - [📝 配線設計單](#-配線設計單)
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
- ✅ 配線技巧：兩燈共腳、光敏電阻反相、全亮全滅顏色接一般腳、逐腳判斷 `~` 腳、兩顆伺服共用訊號線
- ✅ 「~ 腳可用表」：伺服借走 9、10；蜂鳴器借走 3、11
- ✅ 安全連鎖、鎖定與復歸、期中專題的模組化與腳位總表

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 超音波感測器 HC-SR04 | 發出聲波、聽回音，算出前方物體距離 | 倒車雷達、感應水龍頭、水位計 |
| 安裝位置換算 | 感測器量到的是「到我的距離」，要換算成客戶真正想知道的數字 | 從天花板量身高、從桶頂量垃圾高度 |
| 兩顆感測器角色互換 | 入口、出口裝反了，對調兩組線就好 | 現場施工裝錯位置的快速修正 |
| 程式模板 T17～T20 | 本週四個新模板 | 公司的標準表單 |

### 本週可用元件（只限這些）

| 元件 | 第幾週學的 |
| --- | --- |
| 單色 LED、RGB LED、220Ω | 第 1、3 週 |
| 按鈕、10kΩ | 第 2 週 |
| 旋鈕 | 第 4 週 |
| 蜂鳴器、光敏電阻 | 第 5 週 |
| TMP36 | 第 6 週 |
| 伺服馬達 | 第 7 週 |
| 超音波感測器 HC-SR04 | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：期中提案大獲好評，客戶開始帶朋友來。這週的四個新委託，都需要系統「知道東西有多遠」。
>
> | 關卡 | 客戶 | 委託內容 | 套用模板 | 難度 |
> | --- | --- | --- | --- | --- |
> | 1 | 連鎖餐廳 | 感應式給皂機 | T17 近接觸發 | ⭐ |
> | 2 | 社區管委會 | 停車格倒車引導 | T18 距離分段指示 | ⭐⭐ |
> | 3 | 科技園區 | 智慧垃圾桶 | T19 雙感測開蓋＋容量 | ⭐⭐⭐ |
> | 4 | 購物中心 | 停車場出入口管理 | T20 進出計數柵欄 | ⭐⭐⭐⭐ |

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
| Trig（發射） | 任何一隻數位腳位（也可以借 A 腳） |
| Echo（接收） | 任何一隻數位腳位（也可以借 A 腳） |
| GND | GND |

- Trig、Echo 接**一般腳位就可以**，不需要 `~` 腳——把 `~` 腳留給要調光的燈。
- **一顆 HC-SR04 佔 2 隻腳**。系統裡有兩顆，就要 4 隻。
- 在 Tinkercad 元件庫搜尋「Ultrasonic Distance Sensor」，選 **4 隻腳**的版本（HC-SR04）。開始模擬後**點一下感測器**，前方會出現一個可以拖曳的小圓點，代表「物體」。把它拉近拉遠，就能模擬距離變化。

### 程式裡的「距離翻譯機」

本週所有模板都附上同一個函式，你**不需要修改它**，只要知道它會回傳「幾公分」：

```cpp
float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);   // 發出一聲超音波
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);               // 等回音，最多等 30 毫秒
  if (t == 0) return 999;                            // 沒聽到回音：當作很遠
  return t / 58.0;                                   // 時間換算成公分
}
```

---

## 本週配線新技巧

### 技巧 1：先想「站在哪、看向哪」——安裝位置換算

感測器量到的永遠是「**物體到感測器**」的距離，但客戶想知道的常常是別的數字：

| 情境 | 感測器裝在哪 | 量到的距離 | 客戶真正想知道的 | 換算方式 |
| --- | --- | --- | --- | --- |
| 倒車雷達 | 牆上，朝向車尾 | 車尾到牆 | 車尾到牆 | 不用換算 |
| 身高測量 | 天花板（200 cm 高），朝下 | 頭頂到天花板 | 身高 | 200 − 距離 |
| 垃圾桶 | 桶蓋內側（桶深 60 cm），朝下 | 垃圾表面到桶蓋 | 裝了幾成滿 | (60 − 距離) ÷ 60 |
| 水塔 | 水塔頂蓋（深 100 cm），朝下 | 水面到頂蓋 | 水位高度 | 100 − 距離 |

> 💡 和第 7 週的伺服一樣：**換算是參數區的事，不是改線的事**。本週模板都有「安裝高度」或「深度」參數，填對就會自動換算。

### 技巧 2：兩顆感測器角色互換 → 對調兩組線

系統有兩顆 HC-SR04（例如入口、出口），現場卻發現裝反了：

```
原本：入口那顆 → Pin 7、8（程式當作入口）    出口那顆 → Pin 12、13（程式當作出口）
修正：把兩顆感測器的 Trig、Echo 線「整組」對調，程式一個字都不用改
```

> ⚠️ 要**整組**對調（Trig 跟 Trig 換、Echo 跟 Echo 換），只換一條會兩顆都量不到。

### 技巧 3：Trig、Echo 可以借 A 腳

A0～A5 也能當數位腳用。兩顆超音波＋伺服＋燈號時，一般腳常常不夠，把 Trig、Echo 搬到 A 腳是最常見的解法——但**旋鈕、光敏電阻、TMP36 只能接 A 腳**，借之前先保留它們的位置。

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **HC-SR04：VCC 5V、Trig、Echo、GND** | 不用電阻，一顆佔 2 隻腳 |
| ② Trig、Echo 不要佔 `~` 腳 | 把 `~` 腳留給需要調光的燈 |
| ③ 先想「站在哪、看向哪」，換算填在參數區 | 量身高、水位、垃圾高度都要換算 |
| ④ 兩顆感測器裝反 → 整組對調 Trig、Echo 線 | 程式不用改 |
| ⑤ 有伺服，9、10 不能調光；有蜂鳴器，3、11 不能調光 | 兩者都有時只剩 5、6 |
| ⑥ 每條燈線一顆 220Ω；按鈕、光敏電阻一顆 10kΩ | HC-SR04、伺服、蜂鳴器、旋鈕、TMP36 不用 |
| ⑦ Pin 0、1 不用；腳位不夠時借 A 腳 | 先保留旋鈕、光敏電阻、TMP36 的 A 腳 |

---

## 共用工具

### 🔧 工具 1：超音波測距檢查器

接好 HC-SR04 後先跑這支。拖曳 Tinkercad 前方的小圓點，確認 Serial Monitor 顯示的公分數**跟畫面上標示的距離差不多**。有兩顆感測器時，**每一顆都要各測一次**。

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

**檢查標準**：小圓點拉到 50 cm 附近，顯示約 48～52 cm。如果一直顯示 999，先檢查 Trig、Echo 有沒有接反。

**距離記錄表**（每顆感測器都要填）：

| 感測器 | Trig／Echo 接在 | 小圓點 20 cm 時顯示 | 小圓點 100 cm 時顯示 | 朝向（看什麼？） |
| --- | --- | --- | --- | --- |
| 第 1 顆 | | | | |
| 第 2 顆 | | | | |

### 🔧 其他工具（沿用前幾週）

| 工具 | 用途 | 出處 |
| --- | --- | --- |
| 接線驗證器 | 檢查所有燈的腳位 | 第 3 週起 |
| 類比讀值檢查器 | 檢查旋鈕、光敏電阻 | 第 5 週 |
| 蜂鳴器測試器 | 檢查蜂鳴器 | 第 5 週 |
| 伺服測試器（角度記錄表） | 找出伺服的開、關角度 | 第 7 週 |

---

## 關卡怎麼玩：五個固定環節

| 環節 | 你要做什麼 | 產出 |
| --- | --- | --- |
| 🃏 **① 抽客戶限制卡** | 依座號尾數找到你的限制卡 | 圈出你的限制卡 |
| ✏️ **② 接線前決策題** | 回答配線判斷題，畫出**感測器安裝示意圖**，填好**腳位規劃表** | 決策題答案＋示意圖＋規劃表 |
| 🔌 **③ 接線任務** | 照 Step 1 → 2 → 3 接線，每一步都用工具檢查 | 檢查截圖＋距離記錄表 |
| 💻 **④ 套用程式模板** | 只改「腳位區」和「參數區」，再把功能開關逐一打開 | 可運作的 Tinkercad 檔案 |
| 🔄 **⑤ 配線變化挑戰** | **程式一個字都不能改**，只靠改線完成客戶的臨時需求 | 修改後的截圖＋說明 |

| 座號尾數 | 抽到 |
| --- | --- |
| 1、4、7 | 限制卡 A |
| 2、5、8 | 限制卡 B |
| 3、6、9、0 | 限制卡 C |

---

## 關卡 1：感應式給皂機（模板 T17）

⭐ **難度**：HC-SR04 ＋ LED，再逐步加蜂鳴器與旋鈕

### 📖 情境

連鎖餐廳的洗手台要裝感應式給皂機。手伸到出口下方，給皂一次（出皂燈亮 0.8 秒），而且**手不拿開就不會再給**，避免浪費。給皂時發出短促提示音。不同分店的洗手台高度不同，感應距離要能用旋鈕調整（5～30 cm）。

### 🎯 本關配線學習目標

- 第一次接 HC-SR04，用測距檢查器確認讀值
- 判斷 Trig、Echo 要不要 `~` 腳，練習借 A 腳

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：一個出皂口、一顆出皂燈 |
| **B** | 洗手台控制盒已佔用 **Pin 7～13**；**兩個出皂口同步**出皂，兩顆出皂燈只能用 **1 隻腳** |
| **C** | 客戶要「可使用」**綠色待機燈**（沒有手時亮）＋**藍色出皂燈**；蜂鳴器必須接 **A1**，HC-SR04 必須接 **A2（Trig）、A3（Echo）** |

### ✏️ ② 接線前決策題

1. HC-SR04 需要電阻嗎？Trig、Echo 需要 `~` 腳嗎？
2. 給皂機的感測器要裝在出皂口的哪裡、朝哪個方向？畫出示意圖。
3. 限制卡 B：Pin 7～13 不能用，剩下哪些腳？夠不夠接所有元件？兩顆出皂燈怎麼接？
4. 限制卡 C：待機燈要填在模板的哪個腳位？旋鈕還能接 A0 嗎？

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| HC-SR04 Trig | |
| HC-SR04 Echo | |
| 出皂燈（B：兩顆共用） | |
| 待機燈（限制卡 C） | |
| 蜂鳴器 | |
| 感應距離旋鈕 | |

### 🔌 ③ 接線任務

**Step 1：HC-SR04＋出皂燈**

1. HC-SR04 的 VCC 接 5V 軌、GND 接 GND 軌，Trig、Echo 接你規劃的腳位。
2. 出皂燈（限制卡 C 再加待機燈）各串 220Ω。
3. ✅ 檢查：**工具 1**（填距離記錄表）、接線驗證器。

**Step 2：蜂鳴器**

1. 接好蜂鳴器。
2. ✅ 檢查：蜂鳴器測試器。

**Step 3：旋鈕**

1. 旋鈕兩側接 5V、GND，中間接 A 腳。
2. ✅ 檢查：類比讀值檢查器，確認 0～1023。

### 💻 ④ 套用程式模板 T17

```cpp
// ===== 程式模板 T17：近接觸發 =====

// ----- 腳位區（沒接的元件填 -1）-----
int pinTrig   = ?;                  // HC-SR04 Trig
int pinEcho   = ?;                  // HC-SR04 Echo
int ledAction = ?;                  // 動作燈（偵測到時亮）
int ledIdle   = -1;                 // 待機燈（沒偵測到時亮）
int pinBuzzer = ?;                  // 提示蜂鳴器
int pinKnob   = -1;                 // 感應距離旋鈕

// ----- 參數區 -----
float triggerCM = 10;               // 小於這個距離算「偵測到」（有接旋鈕時由旋鈕決定）
int knobMinCM = 5, knobMaxCM = 30;  // 旋鈕可調範圍（公分）
int confirmTime = 0;                // 連續偵測多久才算數（毫秒）；0：立刻算
int actionTime  = 800;              // 動作燈亮多久（毫秒）；0：東西在就一直亮
int beepTone    = 1500;             // 提示音高
String eventName = "Dispense";      // 顯示名稱（英文）

// ----- 功能開關 -----
bool useBuzzer = false;             // Step 2：蜂鳴器
bool useKnob   = false;             // Step 3：旋鈕

// ===== 以下不用改 =====
bool wasNear = false, counted = false;
unsigned long nearSince = 0;
int count = 0;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setLights(bool on) {
  if (ledAction >= 0) digitalWrite(ledAction, on ? HIGH : LOW);
  if (ledIdle >= 0)   digitalWrite(ledIdle,   on ? LOW : HIGH);
}

void setup() {
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  if (ledAction >= 0) pinMode(ledAction, OUTPUT);
  if (ledIdle >= 0)   pinMode(ledIdle, OUTPUT);
  setLights(false);
  Serial.begin(9600);
}

void loop() {
  if (useKnob && pinKnob >= 0) {
    triggerCM = map(analogRead(pinKnob), 0, 1023, knobMinCM, knobMaxCM);
  }

  float d = readCM(pinTrig, pinEcho);
  bool near = d < triggerCM;
  if (near && !wasNear) nearSince = millis();               // 剛偵測到，開始計時
  bool confirmed = near && (millis() - nearSince >= (unsigned long)confirmTime);

  if (confirmed && !counted) {                              // 一次靠近只算一次
    counted = true;
    count++;
    if (useBuzzer && pinBuzzer >= 0) tone(pinBuzzer, beepTone, 80);
    Serial.print(eventName + " #");
    Serial.print(count);
    Serial.print("   distance ");
    Serial.print(d);
    Serial.print(" cm (trigger < ");
    Serial.print(triggerCM);
    Serial.println(")");
    if (actionTime > 0) {
      setLights(true);
      delay(actionTime);
      setLights(false);
    }
  }
  if (!near) counted = false;                               // 離開後才能再觸發
  if (actionTime == 0) setLights(counted);

  wasNear = near;
  delay(100);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表填入；限制卡 C 的 `pinTrig = A2`、`pinEcho = A3`、`pinBuzzer = A1`，`ledIdle` 填綠燈腳位 |
| 參數區 | 本關不用改（`actionTime = 800` 就是出皂 0.8 秒） |
| 功能開關 | Step 2 → `useBuzzer = true`；Step 3 → `useKnob = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「隔壁洗手台也要一顆出皂燈，跟這台同步亮」（限制卡 B 的同學：改成拿掉一顆） | |
| 「旋鈕往右轉應該是感應距離**變短**」 | |
| 「給皂機一直沒反應」——老師會動手腳，找出來並修好 | （提示：看 Serial Monitor 顯示的距離） |

### 💼 情境思考

1. 為什麼要設計成「手不拿開就不會再給」？如果每 0.1 秒偵測到手就給一次，一天下來會浪費多少？
2. 感應距離設太遠（例如 30 cm）會發生什麼事？（提示：洗手台下方的水槽、旁邊經過的人）
3. Serial Monitor 記錄了給皂次數。對餐廳管理者來說，這個數字可以拿來做什麼？（提示：補貨、衛生稽核、尖峰時段）

### ✅ 關卡 1 驗收

- [ ] 距離記錄表已填，Trig、Echo 沒有佔用 `~` 腳
- [ ] 手（小圓點）靠近時給皂一次，不拿開不會重複給；給皂時嗶一聲
- [ ] 旋鈕能調整感應距離（Serial Monitor 的 trigger 數值會變）
- [ ] 依限制卡完成（B：兩燈同步；C：待機綠燈、A 腳配置正確）
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 2：停車格倒車引導（模板 T18）

⭐⭐ **難度**：HC-SR04 ＋ RGB ＋ 蜂鳴器 ＋ 長條燈（注意蜂鳴器借走 3、11）

### 📖 情境

社區地下停車場的狹窄車位，牆上裝一套倒車引導燈。駕駛倒車時，燈號告訴他還能退多少：

| 車尾到牆距離 | RGB | 蜂鳴器 | 長條燈 |
| --- | --- | --- | --- |
| 60 cm 以上 | 綠（繼續倒） | 150 cm 內開始間歇嗶 | 越近亮越多格 |
| 20～60 cm | 黃 (255,180,0)（慢慢倒） | 越近嗶越急 | |
| 20 cm 以下 | 紅（停！） | 長鳴 | 全亮 |

### 🎯 本關配線學習目標

- 逐腳判斷 RGB 哪一腳需要 `~`，並避開蜂鳴器借走的 3、11
- 用參數讓三顆單色燈「當成」RGB 使用

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：RGB ＋ 5 格長條燈 |
| **B** | 管委會想要**紅、黃、綠三顆單色燈**（像紅綠燈）取代 RGB；長條燈 5 格 |
| **C** | 長條燈只要 **3 格**；**~5、~6** 已被車位照明佔用 |

### ✏️ ② 接線前決策題

1. 逐腳判斷：綠 (0,255,0)、黃 (255,180,0)、紅 (255,0,0) 三種顏色中，R、G、B 哪一腳會出現中間值？
2. 有蜂鳴器時，需要 `~` 的那一腳可以接哪些腳位？限制卡 C 再扣掉 5、6 呢？
3. 長條燈只需要亮滅，接在 `~3` 或 `~11` 會有問題嗎？
4. 限制卡 B：三顆單色燈分別接在 `pinR`、`pinG`、`pinB` 的哪一個？三種顏色參數要怎麼改？需要 `~` 腳嗎？
5. 感測器要裝在牆上的什麼高度？太低（地板附近）或太高（天花板附近）各會量到什麼？

**腳位規劃表**：

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| HC-SR04 Trig／Echo | | |
| RGB 的 R／G／B（B：紅、綠、黃三顆燈） | | |
| 蜂鳴器 | | |
| 長條燈 × 5（C：× 3），由遠到近 | | |

### 🔌 ③ 接線任務

**Step 1：HC-SR04＋RGB（或三顆單色燈）**

1. HC-SR04 接法同關卡 1。
2. RGB 三條腳各串 220Ω（限制卡 B：三顆單色燈各串 220Ω）。
3. ✅ 檢查：工具 1、接線驗證器。

**Step 2：蜂鳴器**

1. 接好蜂鳴器。
2. ✅ 檢查：蜂鳴器測試器。

**Step 3：長條燈**

1. 長條燈由**遠到近**排成一列，各串 220Ω。
2. ✅ 檢查：接線驗證器，確認亮的順序和排列順序一致。

### 💻 ④ 套用程式模板 T18

```cpp
// ===== 程式模板 T18：距離分段指示 =====

// ----- 腳位區（沒接的元件填 -1）-----
int pinTrig = ?, pinEcho = ?;       // HC-SR04
int pinR = ?, pinG = ?, pinB = ?;   // 狀態 RGB（或三顆單色燈）
int pinBuzzer = ?;                  // 蜂鳴器
int bar[5] = { ?, ?, ?, ?, ? };     // 長條燈（由遠到近；沒接的填 -1）
int numBars = 5;                    // 實際接了幾格

// ----- 參數區 -----
float mountCM  = 0;                 // 0：數值 = 距離；大於 0：數值 = mountCM − 距離（量高度、水位）
float stopLine = 20;                // 數值 ≤ 這個 → 停止區
float slowLine = 60;                // 數值 ≤ 這個 → 減速區
float beepLine = 150;               // 數值 ≤ 這個 → 開始嗶（越小越急）；0：不嗶
float barMax   = 150;               // 長條燈的量程上限
bool barMoreWhenSmall = true;       // true：數值越小亮越多格（倒車）；false：越大亮越多格（水位）
int colorStop[3] = { 255, 0, 0 };   // 停止區顏色（紅）
int colorSlow[3] = { 255, 180, 0 }; // 減速區顏色（黃）
int colorGo[3]   = { 0, 255, 0 };   // 安全區顏色（綠）
int beepTone = 1000;                // 嗶聲音高
String valueName = "Distance";      // 顯示名稱（英文）

// ----- 功能開關 -----
bool useBuzzer = false;             // Step 2：蜂鳴器
bool useBar    = false;             // Step 3：長條燈

// ===== 以下不用改 =====
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
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  if (pinR >= 0) pinMode(pinR, OUTPUT);
  if (pinG >= 0) pinMode(pinG, OUTPUT);
  if (pinB >= 0) pinMode(pinB, OUTPUT);
  for (int i = 0; i < numBars; i++) if (bar[i] >= 0) pinMode(bar[i], OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float d = readCM(pinTrig, pinEcho);
  float v = (mountCM > 0) ? (mountCM - d) : d;             // 安裝位置換算
  if (mountCM > 0 && v < 0) v = 0;

  if (v <= stopLine)      setColor(colorStop);
  else if (v <= slowLine) setColor(colorSlow);
  else                    setColor(colorGo);

  if (useBar) {
    float vv = constrain(v, stopLine, barMax);
    int lit;
    if (barMoreWhenSmall) lit = map((int)vv, (int)barMax, (int)stopLine, 0, numBars);
    else                  lit = map((int)vv, (int)stopLine, (int)barMax, 0, numBars);
    for (int i = 0; i < numBars; i++) {
      if (bar[i] >= 0) digitalWrite(bar[i], (i < lit) ? HIGH : LOW);
    }
  }

  Serial.print(valueName + ": ");
  Serial.print(v);
  Serial.println(" cm");

  if (useBuzzer && pinBuzzer >= 0 && beepLine > 0 && v <= stopLine) {
    tone(pinBuzzer, beepTone);                              // 停止區：長鳴
    delay(100);
  } else if (useBuzzer && pinBuzzer >= 0 && v <= beepLine) {
    int gap = map((int)v, (int)stopLine, (int)beepLine, 80, 600);   // 越接近停止區越急
    tone(pinBuzzer, beepTone, 50);
    delay(gap);
  } else {
    if (pinBuzzer >= 0) noTone(pinBuzzer);
    delay(100);
  }
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表填入；限制卡 C 的 `numBars = 3`，`bar[]` 後兩格填 `-1` |
| 參數區 | 本關距離規格和模板預設相同；限制卡 B 依決策題 4 改三種顏色（提示：黃燈接在哪一腳，`colorSlow` 就只讓那一腳是 255） |
| 功能開關 | Step 2 → `useBuzzer = true`；Step 3 → `useBar = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「駕駛座那一側的柱子上也要一顆『停』紅燈，跟主燈的紅色同時亮」 | |
| 「長條燈裝反了：車子越近，亮的是靠牆那一端」——讓亮燈方向反過來 | |
| 「顏色亂了：該綠的時候亮紅」——老師會動手腳，找出來並修好 | |

### 💼 情境思考

1. 長條燈只需要亮滅，如果接在 `~3` 或 `~11` 會有問題嗎？為什麼？
2. 管委會問：「既然新車都有倒車雷達，為什麼還要在牆上裝？」請從「誰的車都能用」「維護成本」「事故糾紛」三個角度回答。

### ✅ 關卡 2 驗收

- [ ] RGB（或三顆單色燈）依距離顯示綠／黃／紅；需要 `~` 的那一腳沒有接在 3、11（限制卡 C：也不在 5、6）
- [ ] 越近嗶得越急，20 cm 內長鳴，150 cm 外安靜
- [ ] 長條燈越近亮越多格（限制卡 C：3 格）
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 3：智慧垃圾桶（模板 T19）

⭐⭐⭐ **難度**：**2 顆 HC-SR04**（一顆朝外、一顆朝內）＋ 伺服 ＋ 容量燈 ＋ 滿桶連鎖

### 📖 情境

科技園區的公共區域要換成智慧垃圾桶：

- **桶外**一顆超音波：有人靠近（25 cm 內）時，桶蓋自動打開；人離開 2 秒後慢慢關上。**關蓋途中有人再靠近，立刻重新打開**（防夾手）。
- **桶內**一顆超音波（裝在桶蓋內側往下看）：量垃圾表面高度，換算「裝了幾成滿」，三顆燈顯示：少、半滿（50% 以上）、快滿（85% 以上）。
- **滿桶連鎖**：裝到 85% 以上時桶蓋**不再打開**，並發出提醒聲；清潔人員按「維護鈕」可以強制開蓋 5 秒。

### 🎯 本關配線學習目標

- 兩顆 HC-SR04 在麵包板上分區擺放，不混淆
- 用「深度」參數完成安裝位置換算
- 一般腳不夠時，決定把誰搬到 A 腳

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：桶深 **60 cm** |
| **B** | 大型桶，桶深 **80 cm**；園區監視系統已佔用 **Pin 2～5** |
| **C** | **雙開蓋**（兩顆伺服同步，只能用 1 隻腳）；客戶要求滿桶時**不鎖蓋、只提醒**（避免民眾把垃圾丟在地上） |

### ✏️ ② 接線前決策題

1. 畫出垃圾桶示意圖：兩顆感測器各裝在哪裡、朝哪裡看？
2. 桶內感測器量到 9 cm，表示垃圾裝了幾成滿？請寫出換算過程（限制卡 B 用 80 cm 算）。
3. 本關一般腳位需要幾隻？（兩顆 HC-SR04、伺服、3 顆燈、蜂鳴器、按鈕）限制卡 B 的可用腳位夠嗎？不夠要借幾隻 A 腳？
4. 麵包板上怎麼擺，才不會把「桶外」和「桶內」的線接錯？
5. 限制卡 C：「不鎖蓋、只提醒」要改哪個參數？兩顆伺服怎麼接？

**腳位規劃表**：

| 元件 | 腳位 |
| --- | --- |
| 桶外 HC-SR04 Trig／Echo | |
| 桶內 HC-SR04 Trig／Echo | |
| 桶蓋伺服（C：兩顆共用） | |
| 容量燈：少／半滿／快滿 | |
| 蜂鳴器 | |
| 維護鈕 | |

### 🔌 ③ 接線任務

**Step 1：桶外 HC-SR04＋伺服**

1. 桶外感測器放在麵包板**左側**，伺服的紅線接 5V 軌、棕線接 GND 軌、橘線接你規劃的腳位。
2. ✅ 檢查：工具 1（桶外那顆）、伺服測試器（填角度記錄表：蓋子「關」和「開」各是幾度？）。

**Step 2：桶內 HC-SR04＋容量燈**

1. 桶內感測器放在麵包板**右側**，和桶外那顆分開。
2. 三顆容量燈各串 220Ω。
3. ✅ 檢查：工具 1（桶內那顆）、接線驗證器。

**Step 3：蜂鳴器＋維護鈕**

1. 接好蜂鳴器、維護鈕（配 10kΩ）。
2. ✅ 檢查：蜂鳴器測試器。

### 💻 ④ 套用程式模板 T19

```cpp
// ===== 程式模板 T19：雙感測開蓋＋容量 =====
#include <Servo.h>

// ----- 腳位區（沒接的元件填 -1）-----
int trigOut = ?, echoOut = ?;       // 外側 HC-SR04（偵測人）
int trigIn  = ?, echoIn  = ?;       // 內側 HC-SR04（偵測容量）
int pinServo = ?;                   // 蓋子（門）伺服
int fillLed[3] = { ?, ?, ? };       // 容量燈：少、半滿、快滿
int pinBuzzer  = ?;                 // 蜂鳴器
int btnService = ?;                 // 維護鈕

// ----- 參數區 -----
float openCM = 25;                  // 外側：有人靠近的距離
int holdTime = 2000;                // 人離開後幾毫秒關蓋
float depthCM = 60;                 // 內側感測器到底部的距離（安裝位置換算）
int midLine  = 50;                  // 幾 % 以上算半滿
int fullLine = 85;                  // 幾 % 以上算快滿
int closedAngle = 0, openAngle = 100;   // 依角度記錄表
int sweepDelay = 8;                 // 每轉 1 度等幾毫秒（越大越慢）
bool fullLock = true;               // true：快滿時不開蓋；false：只提醒
int serviceTime = 5000;             // 維護鈕開蓋幾毫秒
int alertTone = 800;                // 提醒音高
String deviceName = "Bin";          // 顯示名稱（英文）

// ----- 功能開關 -----
bool useFill    = false;            // Step 2：內側感測器＋容量燈
bool useService = false;            // Step 3：蜂鳴器＋維護鈕＋滿桶處理

// ===== 以下不用改 =====
Servo lid;
bool lidOpen = false, wasFull = false;
unsigned long lastNear = 0;
int lastService = LOW, tick = 0;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void sweepTo(int from, int to) {
  int step = (to > from) ? 1 : -1;
  for (int a = from; a != to; a += step) { lid.write(a); delay(sweepDelay); }
  lid.write(to);
}

void closeLid() {                                          // 關蓋途中有人靠近就重開（防夾）
  int step = (closedAngle > openAngle) ? 1 : -1;
  for (int a = openAngle; a != closedAngle; a += step) {
    lid.write(a);
    delay(sweepDelay);
    if (a % 10 == 0 && readCM(trigOut, echoOut) < openCM) {
      sweepTo(a, openAngle);
      lastNear = millis();
      if (useService && pinBuzzer >= 0) tone(pinBuzzer, 1500, 200);
      Serial.println("Anti-pinch: reopen");
      return;
    }
  }
  lid.write(closedAngle);
  lidOpen = false;
  Serial.println(deviceName + " CLOSED");
}

void setup() {
  lid.attach(pinServo);
  lid.write(closedAngle);
  pinMode(trigOut, OUTPUT); pinMode(echoOut, INPUT);
  if (trigIn >= 0) { pinMode(trigIn, OUTPUT); pinMode(echoIn, INPUT); }
  for (int i = 0; i < 3; i++) if (fillLed[i] >= 0) pinMode(fillLed[i], OUTPUT);
  if (btnService >= 0) pinMode(btnService, INPUT);
  Serial.begin(9600);
}

void loop() {
  tick++;

  // ---- 內側：裝了幾成滿？ ----
  bool full = false;
  if (useFill) {
    float dIn = readCM(trigIn, echoIn);
    int level = constrain((int)((depthCM - dIn) / depthCM * 100), 0, 100);   // 安裝位置換算
    int idx = 0;
    if (level >= fullLine)     idx = 2;
    else if (level >= midLine) idx = 1;
    for (int i = 0; i < 3; i++) {
      if (fillLed[i] >= 0) digitalWrite(fillLed[i], (i == idx) ? HIGH : LOW);
    }
    full = (level >= fullLine);
    if (tick % 20 == 0) {
      Serial.print("Level: ");
      Serial.print(level);
      Serial.println(" %");
    }
    delay(30);                                             // 兩顆輪流量，避免互相干擾
  }

  // ---- 維護與滿桶處理 ----
  if (useService) {
    if (full && !wasFull) {
      Serial.println(deviceName + " FULL - please empty");
      if (pinBuzzer >= 0) tone(pinBuzzer, alertTone, 500);
    }
    wasFull = full;

    int now = (btnService >= 0) ? digitalRead(btnService) : LOW;
    if (now == HIGH && lastService == LOW) {
      delay(50);
      Serial.println("Service mode: open");
      if (!lidOpen) sweepTo(closedAngle, openAngle);
      delay(serviceTime);
      lidOpen = true;
      closeLid();
    }
    lastService = now;

    if (full && fullLock) {                                // 滿桶連鎖：不開蓋
      if (lidOpen) closeLid();
      delay(100);
      return;
    }
  }

  // ---- 外側：有人靠近就開蓋 ----
  float dOut = readCM(trigOut, echoOut);
  if (dOut < openCM) {
    lastNear = millis();
    if (!lidOpen) {
      sweepTo(closedAngle, openAngle);
      lidOpen = true;
      Serial.println(deviceName + " OPEN");
    }
  } else if (lidOpen && millis() - lastNear > (unsigned long)holdTime) {
    closeLid();
  }
  delay(100);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位規劃表填入（限制卡 B 借用的 A 腳寫 `A0`、`A1`…） |
| 參數區 | `closedAngle`、`openAngle` 依角度記錄表；限制卡 B 的 `depthCM = 80`；限制卡 C 的 `fullLock = false` |
| 功能開關 | Step 2 → `useFill = true`；Step 3 → `useService = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「拖曳桶外的小圓點，卻是容量燈在變」——老師把兩顆感測器的線動了手腳，找出來並修好 | （提示：技巧 2） |
| 「清潔站也要看到『快滿』燈，跟桶上的快滿燈同步」 | |
| 「改成雙開蓋」（限制卡 C 的同學：改回單蓋） | |

### 💼 情境思考

1. 本關的防夾設計：關蓋途中有人靠近就重開。如果是你設計，還有哪些「蓋子（門）會傷人」的情況要考慮？
2. 限制卡 A、B 滿桶時鎖蓋，限制卡 C 不鎖蓋只提醒。兩種設計各自的風險是什麼？你會建議客戶選哪一種？
3. 如果每個垃圾桶都能回報「幾成滿」，清潔公司的排班方式可以怎麼改變？（提示：固定路線 vs. 依需求派車）

### ✅ 關卡 3 驗收

- [ ] 兩顆感測器分區擺放，距離記錄表與角度記錄表已填
- [ ] 有人靠近時開蓋，離開 2 秒後平滑關蓋；關蓋途中靠近會重開
- [ ] 容量燈依垃圾高度正確顯示（限制卡 B 用 80 cm 換算）
- [ ] 快滿時依限制卡鎖蓋或只提醒（提醒聲只響一次）；維護鈕能強制開蓋
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 關卡 4：停車場出入口管理（模板 T20）

⭐⭐⭐⭐ **難度**：入口、出口**各一顆 HC-SR04** ＋ 伺服柵欄 ＋ 看板燈 ＋ 蜂鳴器 ＋ 剩餘車位燈 ＋ 旋鈕 → **一般腳不夠，要借 A 腳**

### 📖 情境

購物中心停車場的入口和出口各裝一顆超音波感測器（朝向車道）：

- **入口**：車子開到柵欄前（50 cm 內）時，若還有空位，柵欄升起，等車子通過後 1 秒放下，場內車數 +1；若客滿，柵欄不開並發出拒絕聲。**車子停在柵欄下不走時，柵欄絕對不放下**，改為每 10 秒提醒一次。
- **出口**：車子經過時，場內車數 −1
- **看板燈**：綠（充足）／黃（剩 2 格以下）／紅（客滿）；**剩餘車位燈**顯示剩餘比例
- **總車位數**：管理員用旋鈕設定（1～20 格），因應部分車位施工封閉

### 🎯 本關配線學習目標

- 完整的腳位預算：一般腳、A 腳、`~` 腳一起算
- 兩顆感測器角色互換、柵欄共用訊號線的綜合應用

### 🃏 ① 客戶限制卡

| 限制卡 | 現場條件 |
| --- | --- |
| **A** | 標準款：一支柵欄、RGB 看板燈、4 顆剩餘車位燈 |
| **B** | **雙車道入口**，兩支柵欄同步（只能用 1 隻腳）；**Pin 12、13** 已被收費機佔用 |
| **C** | 看板改用**紅、綠、黃三顆單色燈**；剩餘車位燈只要 **2 顆** |

### ✏️ ② 接線前決策題

1. 看板燈只顯示綠、黃 (1,1,0)、紅這種「全亮全滅」顏色，需要 `~` 腳嗎？
2. 填寫下方腳位預算表。一般腳位夠嗎？不夠要借哪幾隻 A 腳？（旋鈕已經佔 A0）
3. 限制卡 C：三顆單色燈要怎麼對應 `pinR`、`pinG`、`pinB`？`colorFew`（快滿）要改成什麼？
4. 入口柵欄打開後，車子停在柵欄下不走，柵欄會怎樣？為什麼這樣設計？

**腳位預算表**：

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 總車位旋鈕 | 1（A 腳） | A0 |
| 入口 HC-SR04 Trig／Echo | 2 | |
| 出口 HC-SR04 Trig／Echo | 2 | |
| 柵欄伺服（B：兩顆共用） | 1 | |
| 看板燈 R／G／B（C：三顆單色燈） | 3 | |
| 蜂鳴器 | 1 | |
| 剩餘車位燈（A、B：4 顆；C：2 顆） | | |
| **一般腳位需求合計** | | |
| **可用的一般腳位（扣掉限制卡）** | | |
| **要借幾隻 A 腳？借哪幾隻？** | | |

### 🔌 ③ 接線任務

**Step 1：入口 HC-SR04＋伺服＋蜂鳴器**

1. 入口感測器放在麵包板**左側**；柵欄伺服（限制卡 B：兩顆橘線接同一隻腳）。
2. ✅ 檢查：工具 1（入口那顆）、伺服測試器（填角度記錄表）、蜂鳴器測試器。

**Step 2：出口 HC-SR04＋看板燈**

1. 出口感測器放在麵包板**右側**。
2. 看板燈各串 220Ω。
3. ✅ 檢查：工具 1（出口那顆）、接線驗證器。

**Step 3：旋鈕＋剩餘車位燈**

1. 旋鈕接 A0；剩餘車位燈各串 220Ω。
2. ✅ 檢查：類比讀值檢查器、接線驗證器。

### 💻 ④ 套用程式模板 T20

```cpp
// ===== 程式模板 T20：進出計數柵欄 =====
#include <Servo.h>

// ----- 腳位區（沒接的元件填 -1）-----
int trigIn  = ?, echoIn  = ?;       // 入口 HC-SR04
int trigOut = ?, echoOut = ?;       // 出口 HC-SR04
int pinServo  = ?;                  // 柵欄伺服
int pinBuzzer = ?;                  // 蜂鳴器
int pinR = ?, pinG = ?, pinB = ?;   // 看板燈（只用全亮全滅顏色）
int slotLed[4] = { ?, ?, ?, ? };    // 剩餘車位燈（沒接的填 -1）
int numSlots = 4;                   // 實際接了幾顆
int pinKnob = A0;                   // 總車位旋鈕

// ----- 參數區 -----
int capacity = 8;                   // 總車位（有接旋鈕時由旋鈕決定）
int knobMax  = 20;                  // 旋鈕最大可設幾格
float carCM  = 50;                  // 小於這個距離算「有車」
int closedAngle = 0, openAngle = 90;   // 依角度記錄表
int closeDelay  = 1000;             // 車通過後幾毫秒放下
int passTimeout = 10000;            // 車停在柵欄下超過幾毫秒就提醒
int fewLeft = 2;                    // 剩幾格以下算「快滿」
int colorPlenty[3] = { 0, 1, 0 };   // 充足（R, G, B 開關：1 亮、0 滅）
int colorFew[3]    = { 1, 1, 0 };   // 快滿
int colorFull[3]   = { 1, 0, 0 };   // 客滿
int welcomeTone = 1200, refuseTone = 300;
String placeName = "Parking";       // 顯示名稱（英文）

// ----- 功能開關 -----
bool useExit  = false;              // Step 2：出口感測器＋看板燈
bool useBoard = false;              // Step 3：旋鈕＋剩餘車位燈

// ===== 以下不用改 =====
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

void setColor(int c[]) {
  if (pinR >= 0) digitalWrite(pinR, c[0] ? HIGH : LOW);
  if (pinG >= 0) digitalWrite(pinG, c[1] ? HIGH : LOW);
  if (pinB >= 0) digitalWrite(pinB, c[2] ? HIGH : LOW);
}

void report(String what) {
  Serial.print(what);
  Serial.print("  Parked: ");
  Serial.print(cars);
  Serial.print("/");
  Serial.println(capacity);
}

void setup() {
  barrier.attach(pinServo);
  barrier.write(closedAngle);
  pinMode(trigIn, OUTPUT);  pinMode(echoIn, INPUT);
  if (trigOut >= 0) { pinMode(trigOut, OUTPUT); pinMode(echoOut, INPUT); }
  if (pinR >= 0) pinMode(pinR, OUTPUT);
  if (pinG >= 0) pinMode(pinG, OUTPUT);
  if (pinB >= 0) pinMode(pinB, OUTPUT);
  for (int i = 0; i < numSlots; i++) if (slotLed[i] >= 0) pinMode(slotLed[i], OUTPUT);
  Serial.begin(9600);
  Serial.println(placeName + " READY");
}

void loop() {
  if (useBoard && pinKnob >= 0) capacity = map(analogRead(pinKnob), 0, 1023, 1, knobMax);
  if (cars > capacity) cars = capacity;

  // ---- 入口 ----
  bool carIn = readCM(trigIn, echoIn) < carCM;
  if (carIn && !wasCarIn) {
    if (cars >= capacity) {
      tone(pinBuzzer, refuseTone, 600);
      report("FULL - entry refused");
    } else {
      tone(pinBuzzer, welcomeTone, 100);
      barrier.write(openAngle);
      unsigned long t0 = millis();
      while (readCM(trigIn, echoIn) < carCM) {             // 車還在柵欄下：絕對不放下
        if (millis() - t0 > (unsigned long)passTimeout) {
          tone(pinBuzzer, refuseTone, 300);
          Serial.println("Car not moving - please check");
          t0 = millis();
        }
        delay(100);
      }
      delay(closeDelay);
      barrier.write(closedAngle);
      cars++;
      carIn = false;
      report("Car IN.");
    }
  }
  wasCarIn = carIn;
  delay(30);                                               // 兩顆輪流量

  // ---- 出口與看板燈 ----
  if (useExit) {
    bool carOut = readCM(trigOut, echoOut) < carCM;
    if (carOut && !wasCarOut && cars > 0) {
      cars--;
      report("Car OUT.");
    }
    wasCarOut = carOut;

    int left = capacity - cars;
    if (left <= 0)            setColor(colorFull);
    else if (left <= fewLeft) setColor(colorFew);
    else                      setColor(colorPlenty);
  }

  // ---- 剩餘車位燈：剩越多亮越多 ----
  if (useBoard) {
    int left = capacity - cars;
    int lit = (left * numSlots + capacity - 1) / capacity;   // 有剩就至少亮 1 顆
    for (int i = 0; i < numSlots; i++) {
      if (slotLed[i] >= 0) digitalWrite(slotLed[i], (i < lit) ? HIGH : LOW);
    }
  }
  delay(70);
}
```

**本關要改的地方**：

| 區塊 | 要做什麼 |
| --- | --- |
| 腳位區 | 依腳位預算表填入（借用的 A 腳寫 `A1`、`A2`…）；限制卡 C 的 `numSlots = 2`，`slotLed[]` 後兩格填 `-1` |
| 參數區 | 角度依角度記錄表；限制卡 C 依決策題 3 改三種顏色 |
| 功能開關 | Step 2 → `useExit = true`；Step 3 → `useBoard = true` |

### 🔄 ⑤ 配線變化挑戰（程式一個字都不能改）

| 客戶臨時要求 | 你要改哪幾條線？ |
| --- | --- |
| 「施工隊把入口和出口的感測器裝反了」——車子進場時車數卻減少 | （提示：技巧 2） |
| 「管理室也要一顆『客滿』紅燈，跟看板的紅色同時亮」 | |
| 「總車位旋鈕往右轉應該是**變少**」 | |

### 💼 情境思考

1. 用你的腳位預算表說明：你借了哪幾隻 A 腳、接了什麼？為什麼選這些元件去借？
2. 「車子停在柵欄下就絕對不放下」是安全優先的設計，但如果感測器故障一直量到「有車」，柵欄會永遠開著。管理上要怎麼發現這種故障？
3. 如果出口感測器故障、一直沒偵測到車出去，系統會出現什麼問題？你會建議加什麼「異常檢查」？

### ✅ 關卡 4 驗收

- [ ] 入口有車且未客滿：柵欄升起 → 車通過後放下 → 車數 +1；客滿時拒絕
- [ ] 車停在柵欄下時柵欄不放下，超過 10 秒會提醒
- [ ] 出口有車經過時車數 −1，不會變負數
- [ ] 看板燈依剩餘車位顯示（限制卡 C：三顆單色燈正確）；旋鈕與剩餘車位燈正確
- [ ] 腳位預算表正確處理「一般腳不夠、借 A 腳」與限制卡
- [ ] 完成 3 題配線變化挑戰

**驗收同學簽名**：＿＿＿＿＿＿

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：每一關都是**新的客戶情境**，從 T17～T20 挑一個模板，**自己畫感測器安裝示意圖、算元件、規劃腳位、接線**，並填寫**參數修改表**。**不寫新程式**。
>
> | 關卡 | 情境 | 套用模板 | 元件 | 配線重點 |
> | --- | --- | --- | --- | --- |
> | Lv.1 | 圖書館座位使用偵測 | T17 | HC-SR04＋2 LED | 連續偵測才算數、指定 A 腳 |
> | Lv.2 | 遊樂設施身高檢測 | T18 | HC-SR04＋RGB | 安裝位置換算、逐腳判斷 |
> | Lv.3 | 社區水塔水位面板 | T18 | HC-SR04＋10 LED＋蜂鳴器 | 換算＋長條燈方向＋兩燈共腳 |
> | Lv.4 | 診所自動門防夾 | T19 | HC-SR04＋2 伺服＋蜂鳴器 | 只用一顆感測器、雙開門 |
> | Lv.5 | 展覽館人流管制 | T20 | 2 HC-SR04＋2 伺服＋RGB＋蜂鳴器＋4 LED＋旋鈕 | **一般腳嚴重不夠** |
> | Lv.6 | 顧問提案 | 自選 | 自訂 | 自己設計情境 |
>
> 📌 **作答規則**：先畫「感測器安裝示意圖」，再填「腳位規劃表」「參數修改表」，然後接線；接好後先用工具檢查，最後才上傳程式。Lv.3 之後每關要附**配線設計單**。

---

### 🟢 Lv.1 圖書館座位使用偵測

**情境**：大學圖書館想知道自習座位的使用率。每張桌子底下裝一顆超音波，朝向椅子方向：有人坐著時距離很近（40 cm 內）。但經過的人、拉一下椅子也會被偵測到，所以規則是：**連續 3 秒都偵測到才算「有人使用」**。紅燈表示有人、綠燈表示空位。**圖書館要安靜，不能有聲音**。

**元件**：1 顆 HC-SR04＋2 顆單色 LED（紅、綠）

**套用模板**：T17

**限制條件**：桌下線槽只接到 **A1～A3**，HC-SR04 的 Trig、Echo 必須接在這裡面。

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `pinTrig`、`pinEcho` | `?` | | |
| `ledAction` | `?` | | 紅燈還是綠燈？ |
| `ledIdle` | `-1` | | |
| `pinBuzzer`、`pinKnob` | `?`、`-1` | | |
| `triggerCM` | 10 | | |
| `confirmTime` | 0 | | |
| `actionTime` | 800 | | （提示：有人坐著就要一直亮） |
| `eventName` | `"Dispense"` | | |
| 功能開關 | | | |

**情境思考**：有人放了書包佔位、人卻不在。這套系統會怎麼判斷？要怎麼改善「佔位」問題？

---

### 🟢 Lv.2 遊樂設施身高檢測

**情境**：遊樂園雲霄飛車入口要自動檢查身高，**120 cm 以上**才能搭乘。感測器裝在入口上方 **200 cm** 高的橫桿上，往下量到頭頂的距離。身高符合亮綠燈，不符合亮紅燈。

**元件**：1 顆 HC-SR04＋1 顆 RGB LED（**不需要**蜂鳴器、長條燈）

**套用模板**：T18

**你要想的問題**：

- 安裝位置換算：`mountCM` 填多少？Serial Monitor 顯示的數值代表什麼？
- 只有綠、紅兩種顏色，`slowLine` 要怎麼設，才不會出現黃色？
- 逐腳判斷：B 腳會用到嗎？可以不接嗎？

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `pinB` | `?` | | |
| `pinBuzzer`、`bar[]` | `?` | | |
| `mountCM` | 0 | | |
| `stopLine`、`slowLine` | 20、60 | | |
| `colorStop[]`、`colorGo[]` | 紅、綠 | | |
| `valueName` | `"Distance"` | | |
| 功能開關 | | | |

**情境思考**：沒有人站在下面時，系統會量到地板（約 200 cm），顯示的身高是多少？燈會亮什麼顏色？這樣合理嗎？現場人員要怎麼使用才不會誤會？

---

### 🟡 Lv.3 社區水塔水位面板

**情境**：社區頂樓水塔深 **100 cm**，超音波裝在水塔頂蓋往下量水面。用 5 顆燈顯示水位（**水越多亮越多格**），RGB 不需要。水位低於 **20 cm** 時蜂鳴器開始提醒，越低嗶越急，低於 **10 cm** 長鳴。**管理室和頂樓機房各一組 5 格水位燈**，兩組同步顯示。

**元件**：1 顆 HC-SR04＋10 顆單色 LED＋1 顆蜂鳴器

**套用模板**：T18

**限制條件**：10 顆水位燈最多只能用 **5 隻腳**。

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| `pinR`、`pinG`、`pinB` | `?` | | |
| `bar[]` | `?` | | 兩組燈怎麼接？ |
| `mountCM` | 0 | | |
| `stopLine`、`slowLine`、`beepLine` | 20、60、150 | | |
| `barMax` | 150 | | |
| `barMoreWhenSmall` | `true` | | |
| `valueName` | `"Distance"` | | |
| 功能開關 | | | |

**情境思考**：水位感測器量到 999（沒收到回音）時，系統會判斷水位是多少？會不會誤發「缺水警報」？這種「感測器故障」要怎麼讓管理員知道？

---

### 🟡 Lv.4 診所自動門防夾

**情境**：診所的雙開自動門。有人走近（50 cm 內）時兩扇門同步打開，人離開 3 秒後慢慢關上。**關門過程中只要偵測到有人，立刻重新打開**並發出提醒聲。這扇門**不需要**容量偵測。

**元件**：1 顆 HC-SR04＋2 顆伺服＋1 顆蜂鳴器

**套用模板**：T19

**你要想的問題**：

- 本關只有一顆感測器，要接在 `trigOut／echoOut` 還是 `trigIn／echoIn`？另一組填什麼？
- 提醒聲要響，哪一個功能開關要打開？維護鈕沒接，模板會出錯嗎？
- 門關得太快，防夾來不及反應。要改哪個參數讓門關得慢一點？

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 | 為什麼 |
| --- | --- | --- | --- |
| 腳位區（含不用的元件） | | | |
| `openCM` | 25 | | |
| `holdTime` | 2000 | | |
| `closedAngle`、`openAngle` | 0、100 | | 依角度記錄表 |
| `sweepDelay` | 8 | | |
| `fullLock` | `true` | | 本關用不到，填什麼都可以嗎？ |
| `deviceName` | `"Bin"` | | |
| 功能開關 | | | |

**情境思考**：超音波看得到「站著的人」，但看得到「輪椅、嬰兒車的輪子」嗎？從感測器的安裝高度思考，真實自動門還需要什麼安全設計？

---

### 🔴 Lv.5 展覽館人流管制

**情境**：展覽館入口和出口各裝一顆超音波（朝向通道，**80 cm 內**算有人）。入口是**雙開閘門**（兩顆伺服同步）：有人且未額滿時開門，人通過後 1.5 秒關上；額滿時不開並發出拒絕聲。看板 RGB 顯示綠（充足）／黃（剩 5 人以下）／紅（額滿）；4 顆燈顯示剩餘名額比例。館方用旋鈕設定上限（1～60 人）。

**元件**：2 顆 HC-SR04＋2 顆伺服＋1 顆 RGB＋1 顆蜂鳴器＋4 顆單色 LED＋1 顆旋鈕

**套用模板**：T20

**限制條件**：**Pin 2～4** 已被售票系統佔用。

**Step 0：腳位預算（本關重點）**

| 元件 | 需要幾隻腳 | 需要 `~` 腳嗎？ |
| --- | --- | --- |
| 旋鈕 | 1（A 腳） | — |
| 2 顆 HC-SR04 | | |
| 2 顆伺服 | | |
| RGB（綠、黃、紅） | | |
| 蜂鳴器 | | |
| 4 顆剩餘名額燈 | | |
| **一般腳位需求合計** | | |
| **Pin 5～13 可用** | 9 | |
| **要借幾隻 A 腳？借哪幾隻？還剩幾隻？** | | |

**參數修改表**（請填寫）：

| 參數 | 模板原本的值 | 改成 |
| --- | --- | --- |
| `knobMax` | 20 | |
| `carCM` | 50 | |
| `closeDelay` | 1000 | |
| `fewLeft` | 2 | |
| `closedAngle`、`openAngle` | 0、90 | 依角度記錄表 |
| `placeName` | `"Parking"` | |
| 腳位區（含借用的 A 腳） | | |
| 功能開關 | | |

**情境思考**：

1. 附上你的腳位預算表。如果館方要再加一顆「出口感謝燈」，還放得下嗎？
2. 一群人一起走過入口，超音波只算一次。這個誤差對人流管制有什麼影響？管理上可以怎麼彌補？

---

### 🏆 Lv.6 BOSS 關：顧問提案

**情境**：自己找一個生活或職場情境，**從 T17～T20 選一個模板**，設計一套「會量距離」的系統。

| 情境靈感 | 可能適合的模板 |
| --- | --- |
| 自助加油站「車停到定位」提示 | T18 |
| 貨架缺貨偵測（貨越少距離越遠） | T17（想想怎麼讓「遠」觸發） |
| 寵物飼料桶剩量＋自動開蓋 | T19 |
| 圖書館入口人數管制 | T20 |

**最低規格**：

- 至少 1 顆 HC-SR04，並畫出**感測器安裝示意圖**
- 至少用到 8 隻腳位
- 至少修改模板參數區的 **5 個參數**
- 至少用到一個本週或前幾週的配線技巧（安裝位置換算、角色互換、兩燈共腳、共用訊號線、借 A 腳）
- 自己設計 1 題「配線變化挑戰」並附解答

**繳交內容**：情境說明、感測器安裝示意圖、配線設計單、參數修改表、配線變化挑戰與解答、Tinkercad 連結

---

### 📝 配線設計單

| 項目 | 內容 |
| --- | --- |
| 情境名稱 | |
| 套用模板 | T17／T18／T19／T20 |
| 感測器安裝示意圖 | 每顆 HC-SR04 裝在哪、朝哪看、量到的距離代表什麼 |
| 元件清單 | 元件名稱 × 數量（含 220Ω、10kΩ） |
| 腳位分配表 | 元件 → 腳位（標註 `~` 腳、借用 A 腳） |
| `~` 腳預算 | 有伺服？有蜂鳴器？哪些燈需要 `~` 腳？ |
| 距離／角度記錄表 | 每顆感測器、每顆伺服的實測紀錄 |
| 遇到的問題 | 接線過程中出過什麼錯？怎麼修正的？ |

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 距離一直顯示 999 | Trig、Echo 是否接反；VCC、GND 是否接上 | 用工具 1 逐顆測試 |
| 距離一直是 0 或亂跳 | 是否選到 3 隻腳的感測器版本 | 換成 4 隻腳的 HC-SR04 |
| 拖曳 A 感測器，卻是 B 的功能在動 | 兩顆感測器的線是否接反 | 整組對調 Trig、Echo（技巧 2） |
| 換算後的數字是負的或怪怪的 | `mountCM`、`depthCM` 是否填對 | 重新量安裝高度或深度 |
| 一靠近就觸發好幾次 | 距離門檻是否剛好卡在讀值附近 | 依距離記錄表調整門檻 |
| 調光燈只有全亮全滅 | 是否接在 9、10（有伺服）或 3、11（有蜂鳴器） | 查 ~ 腳可用表改接 |
| 程式無法上傳 | 腳位區是否還有 `?` 沒填 | 用到的填腳位，沒接的填 `-1` |

---

## 成果繳交與評分

### 繳交清單

每個關卡、每個練習題各自建立一個 Tinkercad 檔案，命名格式：`W9-關卡或Lv編號-限制卡-學號`。

| 項目 | 繳交內容 |
| --- | --- |
| 關卡 1～4 | Tinkercad 連結＋決策題答案＋安裝示意圖＋腳位規劃表＋距離／角度記錄表＋配線變化挑戰說明＋同學驗收簽名 |
| Lv.1～Lv.2 | Tinkercad 連結＋參數修改表＋情境思考 |
| Lv.3～Lv.5 | ＋配線設計單 |
| Lv.6 | 完整顧問提案 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 腳位規劃與 A 腳借用 | 25% | 符合限制卡；Trig、Echo 不佔 `~` 腳；一般腳不夠時借用合理 |
| 電路正確性 | 25% | 每個 Step 通過工具檢查，兩顆感測器分區清楚 |
| 配線變化挑戰 | 20% | 正確運用角色互換、兩燈共腳、共用訊號線等技巧 |
| 參數修改表與安裝換算 | 15% | 換算參數正確並說明理由 |
| 情境思考 | 15% | 💼 管理面向論述 |

> 🎯 **最低完成標準**：關卡 1～4 全部完成（含配線變化挑戰）、Lv.1～Lv.3 完成。

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 30 分鐘 | 認識 HC-SR04、三個新技巧、測距檢查器 |
| 第 1 節後段 | 關卡 1 |
| 第 2 節 | 關卡 2、關卡 3 |
| 第 3 節前段 | 關卡 4 |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，其餘帶回家 |

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 超音波感測器（HC-SR04） | 發出聲波、聽回音來量距離，4 隻腳、不用電阻 |
| Trig／Echo | 發射腳／接收腳，各佔一隻數位腳 |
| 安裝位置換算 | 把「到感測器的距離」換成客戶想知道的數字（身高、水位、容量） |
| 角色互換 | 兩顆感測器裝反時，整組對調 Trig、Echo 線 |
| 輪流量 | 多顆超音波一顆一顆量，避免互相干擾 |
| 防夾 | 關閉途中偵測到人就重新打開 |
| 距離記錄表 | 用測距檢查器實測每顆感測器的紀錄 |

---

## 反思與延伸討論

1. 本週每個情境都要先想「感測器站在哪、看向哪」。換算錯一個數字，整個系統的判斷都會錯。這和工廠的「量具校正」有什麼關係？
2. 本週的兩顆感測器裝反，只要對調兩組線就好；伺服的方向卻只能改參數。請整理目前學過的元件：哪些問題靠改線解決、哪些靠改參數？
3. 超音波看不到軟的東西（布料、毛絨玩具）、斜面也會量不準。如果你是顧問，要怎麼在提案裡誠實說明這些限制，又不讓客戶失去信心？

---

## 本週檢核清單

- [ ] 能正確接上 HC-SR04（VCC、Trig、Echo、GND），並用測距檢查器填好距離記錄表
- [ ] 能畫出感測器安裝示意圖，並用參數完成安裝位置換算
- [ ] 能用「整組對調」修正兩顆感測器角色裝反的問題
- [ ] 能在一般腳不夠時合理借用 A 腳
- [ ] 完成關卡 1～4，並取得同學驗收簽名
- [ ] 完成練習題 Lv.1～Lv.3，挑戰 Lv.4～Lv.6

---

## 下週預告

第 10 週我們要讓系統**持續運轉**——加入**直流馬達與 L293D 驅動晶片**。馬達要不要調速、要不要反轉，會決定 L293D 要佔 Arduino 幾隻腳：需求越簡單，接法越省腳。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 8 週](week-08.md) | [下一週：第 10 週 — 會持續運轉的智慧場域 ➡](week-10.md)
