[⬅ 回目錄](../README.md) | [⬅ 上一週：第 13 週](week-13.md) | [下一週：第 15 週 — 終極整合大專題（上）➡](week-15.md)

---

# 第 14 週：自動與手動的智慧場域

## 主題專案：感應、操作與「誰說了算」——工業控制盤的核心邏輯

> 🎯 **本週定位**：這是學習新元件的**最後一週**。我們加入三個元件，補齊工業控制盤最常見的拼圖：
>
> - **繼電器（Relay）**：讓 Arduino 控制「另一條電路」的開關，例如電燈、冷氣、抽水馬達
> - **PIR 人體感測器**：直接偵測「有沒有人在動」
> - **類比搖桿（Joystick）**：讓操作員用手推動，同時控制兩個方向
>
> 但本週真正的主角是一個**管理概念**：系統什麼時候自動運作？什麼時候交給人手動操作？兩者衝突時**誰說了算**？工業現場用一個三段選擇開關解決這件事——**手動／停止／自動（HOA）**。
>
> 和前幾週一樣，本週**重點不在寫程式**，而在於：
>
> 1. 判斷每個情境該用繼電器的**常開（NO）**還是**常閉（NC）**接點
> 2. 設計自動、手動、停止三種模式，以及**優先權順序**
> 3. 規劃腳位，在 Tinkercad 把電路一步步接起來
>
> 程式都已經準備好，你只需要填腳位、修改設定、把功能開關從 `false` 改成 `true`。
>
> 🏁 **本週成果**：完成 **4 個獨立專案**，每個專案存成一個獨立的 Tinkercad 檔案繳交。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件 ①：繼電器](#認識新元件-繼電器)
- [認識新元件 ②：PIR 人體感測器](#認識新元件-pir-人體感測器)
- [認識新元件 ③：類比搖桿](#認識新元件-類比搖桿)
- [本週核心觀念：HOA 三段模式與優先權](#本週核心觀念hoa-三段模式與優先權)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [專案一：樓梯間感應燈](#專案一樓梯間感應燈)
- [專案二：會議室空調節能控制](#專案二會議室空調節能控制)
- [專案三：倉庫天車操作台](#專案三倉庫天車操作台)
- [專案四：輸送線控制盤](#專案四輸送線控制盤)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 便利商店來客鈴](#-lv1-便利商店來客鈴)
  - [Lv.2 咖啡機保溫電源](#-lv2-咖啡機保溫電源)
  - [Lv.3 監視器雲台](#-lv3-監視器雲台)
  - [Lv.4 地下室抽水井](#-lv4-地下室抽水井)
  - [Lv.5 住家保全系統](#-lv5-住家保全系統)
  - [🏆 Lv.6 BOSS 關：期末專題預演提案](#-lv6-boss-關期末專題預演提案)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～13 週）

- ✅ 15 種元件的接線（含 74HC595、七段顯示器）
- ✅ 省腳位技巧：借 A 腳、L293D 省腳接法、I2C LCD、鍵盤 3 行、74HC595 串接
- ✅ 安全連鎖、急停、鎖定與復歸、優先權順序（期中專題）

### 這週要新學的

| 新東西 | 白話說明 | 跟哪個學過的最像 |
| --- | --- | --- |
| 繼電器 | 用電控制的開關：Arduino 一個小訊號，切換另一條電路 | L293D（第 10 週）：都是「以小控大」 |
| 常開（NO）／常閉（NC） | 沒通電時，開關是「斷開」還是「接通」 | 全新觀念 |
| PIR 人體感測器 | 偵測到有人移動就輸出 HIGH | 按鈕（第 2 週）：都是數位輸入 |
| 類比搖桿 | 兩顆旋鈕合體，一顆管左右、一顆管上下 | 旋鈕（第 4 週） |
| HOA 三段模式 | 手動（Hand）／停止（Off）／自動（Auto） | 全新管理觀念 |

> 💡 **這學期沒有任何元件是憑空冒出來的**——每個新元件都能在學過的東西裡找到「本質上相似」的對應。

### 本週可用元件（只限這些）

第 1～13 週學過的全部元件，加上本週的 **繼電器、PIR 人體感測器、類比搖桿**。

---

## 開場任務簡報

> 📖 **情境**：顧問公司的客戶開始要求「真正能上線」的系統：要能控制真實的電器、要能自動偵測有人，也要能讓現場人員隨時接手操作。這週的四個委託案，都在處理「自動」和「手動」怎麼共存。
>
> | 客戶 | 委託內容 | 新元件 | 難度 |
> | --- | --- | --- | --- |
> | 社區大樓 | 樓梯間感應燈 | PIR＋繼電器 | ⭐ |
> | 共享辦公室 | 會議室空調節能 | 繼電器＋PIR＋HOA | ⭐⭐ |
> | 物流倉庫 | 天車操作台 | 搖桿＋繼電器＋PIR | ⭐⭐⭐ |
> | 食品工廠 | 輸送線控制盤 | 全部整合＋優先權 | ⭐⭐⭐⭐ |
>
> 每個專案都分成 3 個步驟，從最簡單的版本開始，每一步多接幾個元件、多一項功能。

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

- **控制端**和**負載端**是兩條完全分開的電路（電氣隔離）。真實世界中，負載端可以是 110V 的電燈、冷氣——Arduino 完全碰不到高壓電，所以安全。
- 在 Tinkercad 本課程用 **LED＋220Ω** 模擬負載（例如電燈、冷氣），負載端的電源取自麵包板 5V。

### 接法

| 繼電器的腳 | 白話 | 接到哪裡 |
| --- | --- | --- |
| 線圈（Coil）兩腳 | 電磁鐵 | 一腳接 Arduino 數位腳、一腳接 GND |
| COM | 共同點 | 負載電源（5V） |
| NO（常開） | 沒通電時**斷開**，通電時接通 | 負載（LED＋220Ω → GND） |
| NC（常閉） | 沒通電時**接通**，通電時斷開 | 負載（LED＋220Ω → GND） |

> ⚠️ **真實硬體**上，Arduino 腳位的力氣不足以直接推動繼電器線圈，通常會使用內建驅動電路的「繼電器模組」（VCC、GND、IN 三腳）。Tinkercad 模擬可以直接接。

### ⭐ 本週最重要的配線決策：NO 還是 NC？

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

### 它在做什麼？

PIR 偵測人體發出的**紅外線變化**：有人在範圍內移動 → 輸出 HIGH；沒人或人靜止不動 → 輸出 LOW。

| 比較 | 光敏電阻（第 5 週） | 超音波（第 9 週） | PIR（本週） |
| --- | --- | --- | --- |
| 偵測什麼 | 光線被擋住 | 前方物體的距離 | **人或動物在移動** |
| 讀到什麼 | 0～1023 | 公分數 | HIGH／LOW |
| 優點 | 便宜 | 知道距離 | 範圍廣、只對溫熱的生物有反應 |
| 缺點 | 受環境光影響 | 只看正前方一條線 | **人不動就偵測不到** |

### 接法：3 隻腳，不用電阻

| PIR 的腳 | 接到哪裡 |
| --- | --- |
| 電源（+） | 5V |
| 訊號（OUT） | 任何數位腳 |
| 接地（−） | GND |

- 讀法和按鈕一樣：`digitalRead()`，但**不需要 10kΩ**。
- 在 Tinkercad 開始模擬後，**點一下 PIR**，前方會出現可以拖曳的物體，在偵測範圍內移動它就會觸發。

---

## 認識新元件 ③：類比搖桿

### 它在做什麼？

搖桿就是**兩顆旋鈕合體**：往左右推是 X 軸、往前後推是 Y 軸，各自輸出 0～1023，**放開時回到中間（約 512）**。

```
            Y 大（約 1023）
                 ↑
X 小（約 0）←   中間 512   → X 大（約 1023）
                 ↓
            Y 小（約 0）
```

### 接法

| 搖桿的腳 | 接到哪裡 |
| --- | --- |
| 電源（+5V） | 5V |
| 接地（GND） | GND |
| X 軸（VRx） | A 腳 |
| Y 軸（VRy） | 另一隻 A 腳 |

> 💡 **Tinkercad 若找不到搖桿元件**，用**兩顆旋鈕**代替：一顆接 X 軸的 A 腳、一顆接 Y 軸的 A 腳，接法和程式**完全相同**。只是放開時不會自動回中，要記得手動轉回中間。

### 死區（Deadband）

搖桿放開時很少剛好是 512，常常是 505、518 之類。如果程式把「不是 512」都當成有人在推，馬達會一直微微動。所以本週程式都設定一個**死區**：在中間 ±150 以內都當作「沒推」。

---

## 本週核心觀念：HOA 三段模式與優先權

### HOA：工業控制盤上一定有的三段開關

| 模式 | 英文 | 誰在控制 | 什麼時候用 |
| --- | --- | --- | --- |
| **自動** | Auto | 感測器與程式 | 平常運轉 |
| **手動** | Hand | 現場人員（按鈕、搖桿） | 維修、測試、感測器故障時 |
| **停止** | Off | 沒有人能啟動 | 保養、清潔、停機檢查 |

本週專案用一顆「模式鈕」循環切換：**AUTO → HAND → OFF → AUTO**，並用 RGB 顯示目前模式：

| 模式 | RGB |
| --- | --- |
| AUTO | 綠 |
| HAND | 黃 |
| OFF | 紅 |

### 優先權：不管哪個模式，安全永遠第一

```
每一輪依序檢查（越上面越優先）：
  ① 急停？                → 全部停止，什麼模式都沒用
  ② 安全感測（有人闖入）？ → 停止或降速
  ③ 目前模式是 OFF？       → 停止
  ④ 目前模式是 HAND？      → 聽操作員的
  ⑤ 目前模式是 AUTO？      → 聽感測器的
```

> 🏭 這和期中專題的「急停 → 溫度 → 照明 → 出入」是同一個思考方式，下週的期末專題也會用到。

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **繼電器：線圈一腳接 Arduino、一腳接 GND；COM 接負載電源** | 控制端和負載端是兩條電路 |
| ② **先問「壞掉時要停在哪裡」，再決定接 NO 或 NC** | 失效安全 |
| ③ **PIR：+ 接 5V、OUT 接數位腳、− 接 GND，不用電阻** | 讀法和按鈕一樣 |
| ④ **搖桿＝兩顆旋鈕：X、Y 各接一隻 A 腳** | 程式要設死區 |
| ⑤ 有伺服，9、10 不能調；有蜂鳴器，3、11 不能調 | 兩者都有時只剩 5、6 |
| ⑥ 每條燈線一顆 220Ω（負載 LED 也要） | 繼電器、PIR、搖桿不用電阻 |
| ⑦ 腳位不夠時：借 A 腳、I2C LCD、74HC595 | 參考第 12 週「十二週省腳位技巧總整理」 |

---

## 共用工具

### 🔧 工具 1：繼電器＋PIR＋搖桿綜合測試器

一次測三個新元件：繼電器每 2 秒切換一次（聽得到「喀」聲、負載 LED 亮滅交替）；Serial Monitor 同時顯示 PIR 狀態和搖桿 X、Y 數值。

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

> 📌 **每個專案的固定流程**
> 1. **Step 0**：決定繼電器接 NO 或 NC（寫下理由）、畫出優先權順序、填腳位規劃表
> 2. 用綜合測試器確認新元件
> 3. 照 Step 1 → 2 → 3 逐步接線，每完成一步就把對應的功能開關改成 `true`

---

## 專案一：樓梯間感應燈

⭐ **難度**：PIR ＋ LED，再改用繼電器控制，最後加光敏電阻

**客戶需求**：社區大樓的樓梯間，有人走過燈就亮，**最後一次偵測到人之後 10 秒**才熄滅（避免人走到一半燈就暗了）。燈泡是 110V 的，所以 Arduino 不能直接控制，要透過繼電器。白天光線充足時不需要開燈。

### Step 0：配線決策與腳位規劃

| 問題 | 你的答案 |
| --- | --- |
| 樓梯燈的繼電器要接 NO 還是 NC？如果 Arduino 壞了，燈應該亮還是滅？為什麼？ | |
| 光敏電阻要裝在哪裡？如果裝在樓梯燈正下方會怎樣？ | |

| 元件 | 腳位 |
| --- | --- |
| PIR 訊號 | |
| Step 1 直接控制的 LED | |
| 繼電器線圈 | |
| 光敏電阻 | A0 |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | PIR＋LED（直接接 Arduino） | 有人移動燈亮，最後一次偵測後 10 秒熄滅 | （不用改） |
| Step 2 | **改接**：LED 移到繼電器負載端（COM 接 5V、NO → 220Ω → LED → GND），線圈接 Arduino | 功能一樣，但改由繼電器切換（聽得到喀聲） | `useRelay = true` |
| Step 3 | ＋光敏電阻（含 10kΩ） | 白天不開燈 | `useDaylight = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinPIR   = ?;       // PIR 訊號
int ledLamp  = ?;       // Step 1：直接接 Arduino 的燈
int pinRelay = ?;       // Step 2：繼電器線圈
int pinLDR   = A0;      // Step 3：光敏電阻
int holdTime = 10000;   // 最後一次偵測後維持幾毫秒
int darkLine = 300;     // 光線低於這個數字才需要開燈
bool useRelay    = false;   // Step 2 改接繼電器後改成 true
bool useDaylight = false;   // Step 3 接好後改成 true
// ============================

bool everMotion = false;
unsigned long lastMotion = 0;
bool lampOn = false;

void setup() {
  pinMode(pinPIR, INPUT);
  pinMode(ledLamp, OUTPUT);
  pinMode(pinRelay, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  if (digitalRead(pinPIR) == HIGH) {
    everMotion = true;
    lastMotion = millis();
  }

  bool want = everMotion && (millis() - lastMotion < holdTime);
  if (useDaylight && analogRead(pinLDR) >= darkLine) want = false;   // 白天不開

  if (want != lampOn) {
    lampOn = want;
    Serial.println(lampOn ? "Lamp ON" : "Lamp OFF");
  }

  if (useRelay) digitalWrite(pinRelay, lampOn ? HIGH : LOW);
  else          digitalWrite(ledLamp,  lampOn ? HIGH : LOW);
  delay(50);
}
```

### 情境思考

1. Step 1 和 Step 2 的功能一模一樣，為什麼還要改用繼電器？在 Tinkercad 裡看不出差別，真實世界差在哪裡？
2. 有人在樓梯間停下來講電話 2 分鐘，燈會怎樣？這是 PIR 的什麼特性造成的？你會怎麼改善？
3. 💼 管委會想知道「改成感應燈一年可以省多少電費」。你需要收集哪些資料才能估算？

### ✅ 專案一驗收

- [ ] 有人移動時燈亮，最後一次偵測後約 10 秒熄滅
- [ ] Step 2 改由繼電器控制，負載接在 NO
- [ ] 白天（光線充足）時即使有人也不開燈
- [ ] 腳位規劃表寫出 NO／NC 的選擇理由

---

## 專案二：會議室空調節能控制

⭐⭐ **難度**：繼電器 ＋ HOA 模式鈕 ＋ PIR ＋ TMP36 ＋ RGB ＋ 蜂鳴器

**客戶需求**：共享辦公室的會議室冷氣常常「開會結束沒人關」。客戶希望：

- **AUTO**：有人在（PIR）**而且**溫度高於 26°C 才開冷氣；降到 24°C 以下關掉（第 6 週的「兩條線」）；**沒人超過 15 秒**自動關閉
- **HAND**：不管感測器，冷氣一直開（例如 PIR 偵測不到坐著不動的人時）
- **OFF**：冷氣關閉，誰也開不了（例如冷氣保養中）
- RGB 顯示目前模式，切換模式時嗶一聲

### Step 0：配線決策與腳位規劃

| 問題 | 你的答案 |
| --- | --- |
| 冷氣的繼電器接 NO 還是 NC？ | |
| RGB 只需要綠、黃、紅，需要 `~` 腳嗎？ | |

| 元件 | 腳位 |
| --- | --- |
| 繼電器線圈（冷氣） | |
| 模式鈕 | |
| PIR | |
| TMP36 | A0 |
| 模式 RGB | |
| 蜂鳴器 | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 繼電器（負載 LED 接 NO）＋模式鈕 | 按鈕循環 AUTO／HAND／OFF；HAND 時冷氣開、其他關 | （不用改） |
| Step 2 | ＋PIR＋TMP36 | AUTO 模式依「有人＋溫度」自動開關 | `useAuto = true` |
| Step 3 | ＋RGB＋蜂鳴器 | 綠／黃／紅顯示模式，切換時嗶一聲 | `useIndicator = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinRelay = ?;                   // 冷氣繼電器線圈
int btnMode  = ?;                   // 模式鈕（AUTO → HAND → OFF）
int pinPIR   = ?;                   // PIR
int pinTemp  = A0;                  // TMP36
int pinR = ?, pinG = ?, pinB = ?;   // 模式 RGB（全亮全滅）
int pinBuzzer = ?;                  // 蜂鳴器
float onTemp  = 26.0;               // 高於這個溫度開冷氣
float offTemp = 24.0;               // 低於這個溫度關冷氣
unsigned long emptyDelay = 15000;   // 沒人多久自動關（毫秒）
bool useAuto      = false;          // Step 2 接好後改成 true
bool useIndicator = false;          // Step 3 接好後改成 true
// ============================

int mode = 0;                       // 0=AUTO, 1=HAND, 2=OFF
String modeName[3] = { "AUTO", "HAND", "OFF" };
int lastButton = LOW;
unsigned long lastMotion = 0;
bool cooling = false;
bool acOn = false;
unsigned long lastReport = 0;

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
  pinMode(pinRelay, OUTPUT);
  pinMode(btnMode, INPUT);
  pinMode(pinPIR, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
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
    if (useIndicator) tone(pinBuzzer, 1200, 100);
  }
  lastButton = now;

  // ---- 依模式決定冷氣開關 ----
  float t = readC(pinTemp);
  if (mode == 1) {
    acOn = true;                                     // HAND：一直開
  } else if (mode == 2) {
    acOn = false;                                    // OFF：一律關
  } else if (useAuto) {                              // AUTO：有人＋溫度
    if (digitalRead(pinPIR) == HIGH) lastMotion = millis();
    bool occupied = (lastMotion > 0) && (millis() - lastMotion < emptyDelay);
    if (t > onTemp)  cooling = true;
    if (t < offTemp) cooling = false;
    acOn = occupied && cooling;
  } else {
    acOn = false;                                    // Step 1：AUTO 尚無感測器
  }
  digitalWrite(pinRelay, acOn ? HIGH : LOW);

  // ---- 模式指示 ----
  if (useIndicator) {
    if (mode == 0)      setColor(LOW, HIGH, LOW);    // AUTO：綠
    else if (mode == 1) setColor(HIGH, HIGH, LOW);   // HAND：黃
    else                setColor(HIGH, LOW, LOW);    // OFF：紅
  }

  if (millis() - lastReport > 1000) {
    lastReport = millis();
    Serial.print(modeName[mode]);
    Serial.print("  Temp ");
    Serial.print(t);
    Serial.print(" C  AC ");
    Serial.println(acOn ? "ON" : "OFF");
  }
  delay(50);
}
```

### 情境思考

1. 為什麼 AUTO 模式要等「沒人 15 秒」才關，而不是 PIR 一變 LOW 就關？
2. 有人覺得 AUTO 太麻煩，把模式永遠切在 HAND。這對節能有什麼影響？管理上怎麼避免？
3. 💼 OFF 模式「誰也開不了」，是為了保護誰？在真實工廠中，OFF 模式通常會搭配什麼制度？（提示：上鎖掛牌 LOTO）

### ✅ 專案二驗收

- [ ] 模式鈕能循環 AUTO → HAND → OFF
- [ ] HAND 冷氣一直開、OFF 一律關
- [ ] AUTO：有人且高於 26°C 才開，降到 24°C 以下關，沒人 15 秒後關
- [ ] RGB 正確顯示模式，切換時嗶一聲

---

## 專案三：倉庫天車操作台

⭐⭐⭐ **難度**：搖桿 ＋ 2 顆伺服 ＋ 繼電器（電磁鐵）＋ PIR 安全連鎖 ＋ RGB

**客戶需求**：倉庫的小型天車（吊車），操作員用搖桿控制：**左右推 → 吊臂旋轉、前後推 → 吊鉤升降**（兩顆伺服）。按「吸附鈕」切換吊鉤上的**電磁鐵**（繼電器控制，負載用 LED 模擬），吸起或放下鐵製貨物。

**安全規定**：作業區內有人走動（PIR）時，**天車立刻停止所有動作**，RGB 轉紅；但**電磁鐵不能放開**——放開的話，吊在半空中的貨物會砸下來。人離開 3 秒後才恢復操作。

### Step 0：配線決策與腳位規劃

| 問題 | 你的答案 |
| --- | --- |
| 電磁鐵的繼電器接 NO 還是 NC？如果 Arduino 突然斷電，貨物會怎樣？這安全嗎？ | |
| 系統有伺服，哪些 `~` 腳不能調光？RGB 需要調光嗎？ | |

| 元件 | 腳位 |
| --- | --- |
| 搖桿 X、Y（或兩顆旋鈕） | A0、A1 |
| 旋轉伺服、升降伺服 | |
| 吸附鈕 | |
| 電磁鐵繼電器線圈 | |
| PIR（作業區） | |
| 狀態 RGB | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 搖桿＋2 顆伺服 | 推搖桿，吊臂旋轉、吊鉤升降；放開就停在原位 | （不用改） |
| Step 2 | ＋吸附鈕＋繼電器（電磁鐵） | 按一下吸附、再按一下放開 | `useMagnet = true` |
| Step 3 | ＋PIR＋RGB | 有人進入作業區立刻停止動作（電磁鐵保持），人離開 3 秒後恢復 | `useSafety = true` |

### 程式（只改設定區）

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int joyX = A0, joyY = A1;           // 搖桿 X、Y（或兩顆旋鈕）
int pinRotate = ?;                  // 旋轉伺服
int pinLift   = ?;                  // 升降伺服
int btnGrab   = ?;                  // 吸附鈕
int pinRelay  = ?;                  // 電磁鐵繼電器線圈
int pinPIR    = ?;                  // 作業區 PIR
int pinR = ?, pinG = ?, pinB = ?;   // 狀態 RGB（全亮全滅）
int deadband = 150;                 // 搖桿死區
int resumeDelay = 3000;             // 人離開後幾毫秒恢復
bool useMagnet = false;             // Step 2 接好後改成 true
bool useSafety = false;             // Step 3 接好後改成 true
// ============================

Servo rotateArm, liftHook;
int angleR = 90, angleL = 90;
bool magnetOn = false;
int lastGrab = LOW;
unsigned long lastPerson = 0;
bool stopped = false;

void setColor(int r, int g, int b) {
  digitalWrite(pinR, r);
  digitalWrite(pinG, g);
  digitalWrite(pinB, b);
}

// 搖桿推到哪一邊：-1、0、+1
int direction(int value) {
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
  pinMode(pinPIR, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // ---- 安全連鎖：作業區有人 ----
  if (useSafety) {
    if (digitalRead(pinPIR) == HIGH) lastPerson = millis();
    bool personNear = (lastPerson > 0) && (millis() - lastPerson < resumeDelay);
    if (personNear != stopped) {
      stopped = personNear;
      Serial.println(stopped ? "PERSON IN ZONE - CRANE STOPPED" : "Zone clear - resumed");
    }
    if (stopped) {
      setColor(HIGH, LOW, LOW);       // 紅：停止中
      digitalWrite(pinRelay, magnetOn ? HIGH : LOW);   // 電磁鐵維持原狀，不放開貨物
      delay(50);
      return;
    }
    setColor(LOW, magnetOn ? LOW : HIGH, magnetOn ? HIGH : LOW);   // 綠：空鉤；藍：吊著貨
  }

  // ---- 搖桿控制：推多久就轉多少，放開就停 ----
  angleR = constrain(angleR + direction(analogRead(joyX)), 0, 180);
  angleL = constrain(angleL + direction(analogRead(joyY)), 20, 160);
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
    digitalWrite(pinRelay, magnetOn ? HIGH : LOW);
  }
  delay(15);
}
```

### 情境思考

1. 為什麼有人進入作業區時，天車「停止動作」但電磁鐵「保持吸住」？如果反過來（一律全部關掉）會發生什麼事？
2. 電磁鐵接 NO：Arduino 斷電時貨物會掉下來。真實天車怎麼解決這個問題？（提示：機械式的安全掛鉤、不斷電電源）
3. 💼 天車操作員需要證照。除了技術上的安全連鎖，企業在「人」的管理上還要做哪些事？

### ✅ 專案三驗收

- [ ] 搖桿左右推，吊臂旋轉；前後推，吊鉤升降；放開停在原位
- [ ] 吸附鈕能切換電磁鐵（負載 LED 亮滅）
- [ ] PIR 偵測到人時所有動作停止、RGB 轉紅，電磁鐵維持原狀
- [ ] 人離開 3 秒後恢復操作
- [ ] 腳位規劃表說明電磁鐵 NO／NC 的取捨

---

## 專案四：輸送線控制盤

⭐⭐⭐⭐ **難度**：HOA ＋ 急停 ＋ PIR 安全區 ＋ 搖桿手動 ＋ L293D 馬達 ＋ 繼電器警示燈 ＋ I2C LCD → **期末專題的預演**

**客戶需求**：食品工廠的輸送線控制盤，整合本週所有觀念：

| 模式／狀況 | 輸送帶行為 |
| --- | --- |
| **AUTO** | 入口超音波偵測到產品 → 輸送帶前進；最後一次偵測後 3 秒停止 |
| **HAND** | 操作員推搖桿 Y 軸：往前推前進、往後推倒退，推越多越快（排除卡料用） |
| **OFF** | 停止，任何操作無效 |
| **安全區有人**（PIR） | AUTO：立刻停止；HAND：限速 30%（操作員可能正在旁邊排除卡料） |
| **急停** | 最高優先：立刻停止，**再按一次急停才解除** |

- 輸送帶運轉時，**警示旋轉燈**（繼電器控制，負載 LED 模擬）亮起
- 有人進入安全區、或按下急停時，蜂鳴器提醒
- I2C LCD 第 0 行顯示模式，第 1 行顯示速度與方向或警告訊息

### Step 0：優先權流程圖與腳位規劃（本專案的重頭戲）

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

| 問題 | 你的答案 |
| --- | --- |
| 輸送帶要正反轉又要調速，L293D 用哪種接法？佔幾隻腳？ | |
| 有蜂鳴器，EN 要避開哪兩隻腳？ | |
| 警示旋轉燈的繼電器接 NO 還是 NC？ | |

| 元件 | 腳位 |
| --- | --- |
| L293D EN／IN1／IN2 | |
| HC-SR04 Trig／Echo | |
| 警示燈繼電器線圈 | |
| PIR（安全區） | |
| 模式鈕、急停鈕 | |
| 蜂鳴器 | |
| 搖桿 Y 軸 | A0 |
| I2C LCD | A4、A5 |
| **合計／還剩幾隻** | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | L293D（接法 D）＋馬達＋HC-SR04＋模式鈕＋I2C LCD | AUTO：偵測到產品就前進；OFF：停止 | （不用改） |
| Step 2 | ＋搖桿＋繼電器（警示燈） | 模式多了 HAND，搖桿控制前進倒退與速度；運轉時警示燈亮 | `useHand = true` |
| Step 3 | ＋PIR＋急停鈕＋蜂鳴器 | 安全區有人：AUTO 停止、HAND 限速；急停最優先 | `useSafety = true` |

### 程式（只改設定區）

```cpp
#include <Adafruit_LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
int pinEN = ?, pinIN1 = ?, pinIN2 = ?;   // L293D（接法 D；EN 為 ~ 腳，避開 3、11）
int pinTrig = ?, pinEcho = ?;            // 入口 HC-SR04
int pinRelay = ?;                        // 警示旋轉燈繼電器線圈
int pinPIR   = ?;                        // 安全區 PIR
int btnMode  = ?;                        // 模式鈕（AUTO → HAND → OFF）
int btnEStop = ?;                        // 急停鈕
int pinBuzzer = ?;                       // 蜂鳴器
int joyY = A0;                           // 搖桿 Y 軸（或旋鈕）
float productCM = 20;                    // 入口偵測距離
int autoSpeed = 180;                     // AUTO 速度
int runAfter  = 3000;                    // AUTO：最後一次偵測後再跑幾毫秒
int deadband  = 150;                     // 搖桿死區
int handLimitPct = 30;                   // HAND 時安全區有人的限速 %
bool useHand   = false;                  // Step 2 接好後改成 true
bool useSafety = false;                  // Step 3 接好後改成 true
// ============================
// I2C LCD：SDA 接 A4、SCL 接 A5

Adafruit_LiquidCrystal lcd(0);
int mode = 0;                            // 0=AUTO, 1=HAND, 2=OFF
String modeName[3] = { "AUTO", "HAND", "OFF" };
bool eStopped = false;
int lastMode = LOW, lastEStop = LOW;
unsigned long lastProduct = 0;
int tick = 0;

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void belt(int speed) {                   // 正數前進、負數倒退、0 停止
  digitalWrite(pinIN1, speed > 0 ? HIGH : LOW);
  digitalWrite(pinIN2, speed < 0 ? HIGH : LOW);
  analogWrite(pinEN, abs(speed));
  if (useHand) digitalWrite(pinRelay, speed != 0 ? HIGH : LOW);   // 運轉時警示燈亮
}

bool pressed(int pin, int &last) {
  int now = digitalRead(pin);
  bool edge = (now == HIGH && last == LOW);
  last = now;
  if (edge) delay(50);
  return edge;
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  pinMode(pinEN, OUTPUT);
  pinMode(pinIN1, OUTPUT);
  pinMode(pinIN2, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinRelay, OUTPUT);
  pinMode(pinPIR, INPUT);
  pinMode(btnMode, INPUT);
  pinMode(btnEStop, INPUT);
}

void loop() {
  tick++;

  // ---- 按鈕 ----
  if (pressed(btnMode, lastMode)) {
    mode = (mode + 1) % 3;
    if (mode == 1 && !useHand) mode = 2;          // Step 1 沒有 HAND
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
    showLine(0, "MODE OFF");
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
    if (person) speed = speed * handLimitPct / 100;          // ③ 安全區有人：限速
    info = person ? "PERSON! LIMITED" : "";
  } else {
    // ===== ⑤ AUTO：依超音波 =====
    if (readCM(pinTrig, pinEcho) < productCM) lastProduct = millis();
    bool hasWork = (lastProduct > 0) && (millis() - lastProduct < runAfter);
    speed = hasWork ? autoSpeed : 0;
    if (person) speed = 0;                                   // ③ 安全區有人：停止
    info = person ? "PERSON! STOPPED" : (hasWork ? "" : "WAITING PRODUCT");
  }

  belt(speed);
  showLine(0, "MODE " + modeName[mode]);
  if (info.length() > 0) showLine(1, info);
  else showLine(1, "SPD " + String(abs(speed)) + (speed > 0 ? " FWD" : speed < 0 ? " REV" : " STOP"));
  delay(50);
}
```

### 情境思考

1. 用你的優先權流程圖說明：安全區有人時，為什麼 AUTO 要「停止」，HAND 卻只「限速」？反過來設計會有什麼問題？
2. 急停設計成「再按一次才解除」，而不是「放開就恢復」。為什麼？
3. 💼 這個控制盤的所有判斷順序，就是一份「現場作業規則」。如果要把它寫成給操作員看的 SOP（標準作業程序），你會怎麼寫？請寫出 3～5 條。

### ✅ 專案四驗收

- [ ] AUTO：產品到入口時輸送帶前進，最後一次偵測後 3 秒停止
- [ ] HAND：搖桿前推前進、後推倒退，推越多越快；放開停止
- [ ] OFF：任何操作都無法啟動
- [ ] 安全區有人：AUTO 停止、HAND 限速 30%，蜂鳴器提醒
- [ ] 急停最優先，再按一次才解除
- [ ] 輸送帶運轉時警示燈（繼電器負載）亮起
- [ ] 優先權流程圖完整，與程式行為一致

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：四個範例專案是「跟著做」，這一區換你「自己規劃」。每一關先回答「**繼電器接 NO 還是 NC**」「**有哪些模式、誰優先**」，再規劃腳位、動手接線。
>
> 📈 **難度安排**：
>
> | 關卡 | 新元件 | 其他元件 | 配線重點 |
> | --- | --- | --- | --- |
> | Lv.1 | PIR | 蜂鳴器＋LED | PIR 當數位輸入 |
> | Lv.2 | 繼電器 | TMP36＋LED | 繼電器控制加熱器（兩條線） |
> | Lv.3 | 搖桿 | 2 伺服＋按鈕＋LED | 搖桿直接對應角度 |
> | Lv.4 | 繼電器 | HC-SR04＋按鈕＋RGB＋蜂鳴器 | HOA＋不受模式影響的安全警報 |
> | Lv.5 | PIR×2＋繼電器 | 鍵盤＋I2C LCD＋RGB＋蜂鳴器 | 布防／撤防與延遲 |
> | Lv.6 | 自訂 | 自訂 | 期末專題預演提案 |
>
> 📌 **作答規則**：每一關先填配線決策與腳位規劃表，再接線；接好後先用綜合測試器確認，最後才上傳程式。程式只需要改「設定區」。

---

### 🟢 Lv.1 便利商店來客鈴

**情境**：便利商店門口裝 PIR，有客人進門時播放「叮咚」並亮起歡迎燈 2 秒，Serial Monitor 記錄今日來客數。為了避免同一位客人在門口走來走去被重複計算，**兩次叮咚之間至少隔 3 秒**。

**元件**：PIR＋蜂鳴器＋1 顆 LED

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinPIR    = ?;     // PIR
int pinBuzzer = ?;     // 蜂鳴器
int ledWelcome = ?;    // 歡迎燈
int coolDown  = 3000;  // 兩次之間至少隔幾毫秒
// ============================

int visitors = 0;
unsigned long lastChime = 0;
bool wasMotion = false;

void setup() {
  pinMode(pinPIR, INPUT);
  pinMode(ledWelcome, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  bool motion = digitalRead(pinPIR) == HIGH;
  if (motion && !wasMotion && millis() - lastChime > coolDown) {
    visitors++;
    lastChime = millis();
    Serial.print("Visitor #");
    Serial.println(visitors);
    digitalWrite(ledWelcome, HIGH);
    tone(pinBuzzer, 880, 300); delay(350);
    tone(pinBuzzer, 700, 500); delay(1650);
    digitalWrite(ledWelcome, LOW);
  }
  wasMotion = motion;
  delay(50);
}
```

**情境思考**：

1. PIR 和第 9 週的超音波都能偵測「有人來」。這個情境用哪一個比較好？為什麼？
2. 💼 店員進出倉庫也會被算成來客。來客數失真會影響哪些經營決策？你會怎麼改善？

---

### 🟢 Lv.2 咖啡機保溫電源

**情境**：咖啡機的保溫盤是 110V 的加熱器，用繼電器控制。保溫盤溫度低於 80°C 開始加熱，高於 85°C 停止（第 6 週的「兩條線」）。加熱中指示燈亮。

**元件**：繼電器（負載 LED 代表加熱器）＋TMP36＋1 顆 LED（加熱中指示）

**你要想的問題**：

- 加熱器的繼電器接 NO 還是 NC？如果 Arduino 當機時繼電器停在「加熱」狀態，會發生什麼事？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinRelay = ?;      // 加熱器繼電器線圈
int pinTemp  = A0;     // TMP36（保溫盤）
int ledHeat  = ?;      // 加熱中指示燈
float heatOn  = 80.0;  // 低於這個溫度開始加熱
float heatOff = 85.0;  // 高於這個溫度停止加熱
// ============================

bool heating = false;

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setup() {
  pinMode(pinRelay, OUTPUT);
  pinMode(ledHeat, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float t = readC(pinTemp);
  if (t < heatOn)  heating = true;
  if (t > heatOff) heating = false;
  digitalWrite(pinRelay, heating ? HIGH : LOW);
  digitalWrite(ledHeat,  heating ? HIGH : LOW);

  Serial.print("Plate: ");
  Serial.print(t);
  Serial.println(heating ? " C  HEATING" : " C  idle");
  delay(500);
}
```

**情境思考**：

1. 繼電器每切換一次都會「喀」一聲，而且機械開關有使用次數上限。如果把兩條線改成同一條（例如 82°C），繼電器會怎樣？
2. 💼 咖啡店打烊後常忘記關保溫盤。你會建議加什麼元件、什麼規則，讓它「沒人時自動關」？

---

### 🟡 Lv.3 監視器雲台

**情境**：大樓保全室的監視器裝在可轉動的雲台上。保全用搖桿控制：左右推 → 水平轉動、前後推 → 上下俯仰，**搖桿推到哪、鏡頭就轉到哪**（放開回到正中間）。按「錄影鈕」切換錄影，錄影中紅燈閃爍。

**元件**：搖桿（或兩顆旋鈕）＋2 顆伺服＋1 顆按鈕＋1 顆 LED

**你要想的問題**：

- 專案三的天車是「推多久轉多少，放開停在原位」；這一關是「推到哪轉到哪，放開回中間」。哪一種適合天車？哪一種適合監視器？為什麼？

**程式**：

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int joyX = A0, joyY = A1;   // 搖桿 X、Y
int pinPan  = ?;            // 水平伺服
int pinTilt = ?;            // 俯仰伺服
int btnRec  = ?;            // 錄影鈕
int ledRec  = ?;            // 錄影燈
// ============================

Servo pan, tilt;
bool recording = false;
int lastButton = LOW;

void setup() {
  pan.attach(pinPan);
  tilt.attach(pinTilt);
  pinMode(btnRec, INPUT);
  pinMode(ledRec, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  pan.write(map(analogRead(joyX), 0, 1023, 0, 180));    // 推到哪，轉到哪
  tilt.write(map(analogRead(joyY), 0, 1023, 45, 135));  // 俯仰範圍較小

  int now = digitalRead(btnRec);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    recording = !recording;
    Serial.println(recording ? "REC START" : "REC STOP");
  }
  lastButton = now;

  digitalWrite(ledRec, (recording && (millis() / 500) % 2) ? HIGH : LOW);
  delay(15);
}
```

**情境思考**：

1. 俯仰範圍設成 45～135 度而不是 0～180 度，是為了避免什麼？
2. 💼 保全室有一位保全要同時看 20 支監視器。你會建議怎麼結合 PIR，讓保全只需要關注「有人移動」的畫面？

---

### 🟡 Lv.4 地下室抽水井

**情境**：大樓地下室的集水井（深 100 cm），超音波裝在井口往下量水位，繼電器控制抽水馬達（負載 LED 模擬）。

| 模式 | 抽水馬達 |
| --- | --- |
| AUTO | 水位高於 70% 開始抽，低於 20% 停止 |
| HAND | 一直抽（測試或緊急排水用） |
| OFF | 停止（維修馬達時） |

**不管哪個模式**，水位超過 **90%** 都要觸發**淹水警報**（蜂鳴器＋RGB 紅閃）——就算在 OFF 模式也一樣，因為淹水不等人。

**元件**：HC-SR04＋繼電器＋1 顆按鈕（模式）＋RGB＋蜂鳴器

**Step 0：優先權**

| 優先 | 狀況 | 動作 |
| --- | --- | --- |
| 1 | 水位 > 90% | ____________（任何模式都會發生） |
| 2 | OFF | ____________ |
| 3 | HAND | ____________ |
| 4 | AUTO | ____________ |

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinTrig = ?, pinEcho = ?;       // HC-SR04（井口往下量）
int pinRelay = ?;                   // 抽水馬達繼電器線圈
int btnMode  = ?;                   // 模式鈕
int pinR = ?, pinG = ?, pinB = ?;   // 模式／警報 RGB（全亮全滅）
int pinBuzzer = ?;                  // 蜂鳴器
float wellDepth = 100;              // 井口到井底深度（公分）
int startPct = 70, stopPct = 20, floodPct = 90;
// ============================

int mode = 0;                       // 0=AUTO, 1=HAND, 2=OFF
String modeName[3] = { "AUTO", "HAND", "OFF" };
bool pumping = false;
int lastButton = LOW;
int tick = 0;

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
  pinMode(pinRelay, OUTPUT);
  pinMode(btnMode, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  tick++;
  int now = digitalRead(btnMode);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    mode = (mode + 1) % 3;
  }
  lastButton = now;

  int level = constrain((int)((wellDepth - readCM(pinTrig, pinEcho)) / wellDepth * 100), 0, 100);

  if (mode == 2)      pumping = false;             // OFF
  else if (mode == 1) pumping = true;              // HAND
  else {                                           // AUTO（兩條線）
    if (level > startPct) pumping = true;
    if (level < stopPct)  pumping = false;
  }
  digitalWrite(pinRelay, pumping ? HIGH : LOW);

  bool flood = level > floodPct;                   // 不受模式影響
  if (flood) {
    setColor((tick / 5) % 2 ? HIGH : LOW, LOW, LOW);
    if (tick % 10 == 0) tone(pinBuzzer, 2000, 200);
  } else if (mode == 0) setColor(LOW, HIGH, LOW);
  else if (mode == 1)   setColor(HIGH, HIGH, LOW);
  else                  setColor(HIGH, LOW, LOW);

  if (tick % 10 == 0) {
    Serial.print(modeName[mode]);
    Serial.print("  Level ");
    Serial.print(level);
    Serial.print("%  Pump ");
    Serial.print(pumping ? "ON" : "OFF");
    Serial.println(flood ? "  !! FLOOD ALARM !!" : "");
  }
  delay(100);
}
```

**情境思考**：

1. 為什麼淹水警報「不受模式影響」，連 OFF 模式也會響？如果 OFF 模式下也不警報，會發生什麼事？
2. 維修人員把模式切到 OFF 修馬達，修完忘記切回 AUTO。這個系統有辦法提醒他嗎？你會怎麼加？
3. 💼 颱風夜抽水馬達故障，地下室淹水，損失數百萬。事後檢討時，除了技術，還要檢討哪些管理面向？

---

### 🔴 Lv.5 住家保全系統

**情境**：住家的保全系統，兩顆 PIR 分別裝在**大門口**和**客廳**：

- **撤防**（在家）：PIR 不觸發警報，RGB 綠燈
- 輸入密碼按 `A` → **布防倒數 10 秒**（讓屋主出門），RGB 黃燈閃，LCD 倒數
- **布防**：RGB 紅燈恆亮
  - 大門口偵測到人 → **進門延遲 10 秒**，LCD 倒數，屋主要在時間內輸入密碼按 `#` 撤防，否則觸發警報
  - 客廳偵測到人（沒經過大門，可能是翻窗）→ **立刻警報**
- **警報**：繼電器啟動警報燈／警笛（負載 LED），蜂鳴器響，RGB 紅閃；輸入密碼按 `#` 解除

**元件**：鍵盤（4 行）＋I2C LCD＋2 顆 PIR＋繼電器＋RGB＋蜂鳴器

**Step 0：腳位規劃（本關重點）**

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 鍵盤（要用 A 鍵 → 4 行） | 8 | |
| I2C LCD | 2 | A4、A5 |
| 大門 PIR、客廳 PIR | 2 | |
| 警報繼電器線圈 | 1 | |
| RGB | 3 | |
| 蜂鳴器 | 1 | |
| **合計／Arduino 可用 18** | | |

> 🤔 警報燈的繼電器接 NO 還是 NC？如果小偷剪斷電源，你希望警報燈怎樣？（提示：本關沒有標準答案，請說明你的取捨）

**程式**：

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C4
int pirDoor = ?;                    // 大門口 PIR
int pirLiving = ?;                  // 客廳 PIR
int pinRelay  = ?;                  // 警報燈／警笛繼電器線圈
int pinR = ?, pinG = ?, pinB = ?;   // 狀態 RGB（全亮全滅）
int pinBuzzer = ?;                  // 蜂鳴器
String password = "1357";
int exitDelay  = 10;                // 布防倒數秒數
int entryDelay = 10;                // 進門延遲秒數
// ============================
// I2C LCD：SDA 接 A4、SCL 接 A5

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);
Adafruit_LiquidCrystal lcd(0);

int state = 0;                      // 0=撤防, 1=布防倒數, 2=布防, 3=進門延遲, 4=警報
unsigned long stateStart = 0;
String input = "";

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void setColor(int r, int g, int b) {
  digitalWrite(pinR, r);
  digitalWrite(pinG, g);
  digitalWrite(pinB, b);
}

void go(int s) {
  state = s;
  stateStart = millis();
  input = "";
}

int secondsLeft(int total) {
  return total - (int)((millis() - stateStart) / 1000);
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  pinMode(pirDoor, INPUT);
  pinMode(pirLiving, INPUT);
  pinMode(pinRelay, OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  bool blink = (millis() / 300) % 2;
  String stars = "";
  for (int i = 0; i < input.length(); i++) stars += "*";

  // ---- 鍵盤 ----
  char k = keypad.getKey();
  if (k >= '0' && k <= '9' && input.length() < 4) input += k;
  else if (k == '*') input = "";
  else if (k == 'A' && state == 0) {
    if (input == password) go(1); else input = "";
  } else if (k == '#' && state >= 1) {
    if (input == password) go(0); else input = "";
  }

  // ---- 狀態機 ----
  if (state == 0) {                               // 撤防
    setColor(LOW, HIGH, LOW);
    digitalWrite(pinRelay, LOW);
    showLine(0, "DISARMED");
    showLine(1, "PIN+A TO ARM " + stars);
  } else if (state == 1) {                        // 布防倒數
    setColor(blink ? HIGH : LOW, blink ? HIGH : LOW, LOW);
    showLine(0, "LEAVE NOW " + String(secondsLeft(exitDelay)) + "s");
    showLine(1, "PIN+# CANCEL " + stars);
    if (secondsLeft(exitDelay) <= 0) go(2);
  } else if (state == 2) {                        // 布防
    setColor(HIGH, LOW, LOW);
    showLine(0, "ARMED");
    showLine(1, "PIN+# DISARM " + stars);
    if (digitalRead(pirLiving) == HIGH) go(4);    // 客廳：立刻警報
    else if (digitalRead(pirDoor) == HIGH) go(3); // 大門：進門延遲
  } else if (state == 3) {                        // 進門延遲
    setColor(blink ? HIGH : LOW, LOW, LOW);
    if (blink) tone(pinBuzzer, 1000, 50);
    showLine(0, "ENTER PIN " + String(secondsLeft(entryDelay)) + "s");
    showLine(1, "PIN+# " + stars);
    if (secondsLeft(entryDelay) <= 0) go(4);
  } else {                                        // 警報
    setColor(blink ? HIGH : LOW, LOW, LOW);
    digitalWrite(pinRelay, HIGH);
    tone(pinBuzzer, 2000, 100);
    showLine(0, "!!! ALARM !!!");
    showLine(1, "PIN+# STOP " + stars);
  }
  delay(30);
}
```

**情境思考**：

1. 附上你的腳位規劃表：一共用了幾隻？如果要再加「窗戶感測」（第三顆 PIR），還放得下嗎？
2. 大門用「進門延遲」、客廳用「立刻警報」。為什麼兩個位置的反應不一樣？
3. 💼 保全公司接到警報後要派人到場，但 90% 的警報都是屋主忘記撤防造成的「誤報」。保全公司該怎麼降低誤報的成本？

---

### 🏆 Lv.6 BOSS 關：期末專題預演提案

**情境**：下週開始期末整合大專題。這一關是**預演**：用本週學到的 HOA 模式與優先權觀念，提出一個完整的控制系統提案。

> **案例 A：智慧溫室控制盤**
> AUTO：依溫度控制天窗（伺服）、依光線控制補光燈（繼電器）、依土壤濕度（旋鈕模擬）控制澆水馬達；HAND：搖桿控制天窗；有人進入溫室（PIR）時暫停澆水。
>
> **案例 B：停車場出入口控制盤**
> AUTO：超音波偵測車輛自動開柵欄；HAND：保全按鈕開柵欄；OFF：柵欄鎖住；柵欄下方有人（PIR）時不准放下；夜間（光敏電阻）自動開啟照明（繼電器）。
>
> **案例 C：自訂情境**
> 自己找一個需要「自動＋手動」的控制系統（例如：自動門、捲門、抽風機、魚塭打水車、電梯）。

**最低規格**：

- 至少使用本週三個新元件中的兩個
- 必須有 HOA 三種模式，以及至少一個「不受模式影響」的安全機制
- 至少 1 個繼電器負載，並說明 NO／NC 的選擇
- 總共使用 12 隻以上腳位

**繳交內容**：

1. **需求分析**：這個系統解決什麼問題？誰會操作？誰會被影響？（3～5 句）
2. **模式表**：AUTO、HAND、OFF 三種模式下，每個輸出的行為
3. **優先權流程圖**：從急停（或最高安全條件）到 AUTO 的完整判斷順序
4. **失效安全分析**：每個繼電器負載接 NO 或 NC 的理由
5. **腳位規劃表**與 **Tinkercad 實作**
6. **作業規則**：給現場人員看的 3～5 條 SOP

**驗收標準**：

- [ ] 三種模式都正確運作，切換時有明確指示
- [ ] 安全機制在任何模式下都有效
- [ ] 優先權流程圖與實際行為一致
- [ ] 每個繼電器負載的 NO／NC 選擇有合理說明
- [ ] 麵包板依功能分區

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 繼電器有「喀」聲但負載 LED 不亮 | 負載端：COM 是否接 5V；LED 是否接在 NO（或 NC）；有沒有 220Ω、有沒有接回 GND | 照負載端接法重接 |
| 繼電器完全沒反應 | 線圈兩腳：一腳接 Arduino 腳位、一腳接 GND | 用綜合測試器確認 |
| 負載的亮滅和預期相反 | 接到 NO 還是 NC | NO 和 NC 對調 |
| PIR 一直是 HIGH 或一直是 LOW | 電源、GND、OUT 三隻腳是否接對；模擬時物體有沒有在範圍內**移動** | PIR 只偵測「移動」，物體要動 |
| PIR 偵測到後隔很久才變回 LOW | PIR 本身有維持時間 | 正常現象，程式的延遲時間要考慮這段 |
| 搖桿放開後馬達或伺服還在動 | 死區是否太小；用旋鈕代替時是否轉回中間 | 加大 `deadband`，或把旋鈕轉回中間 |
| 搖桿左右和上下顛倒 | X、Y 接的 A 腳 | 對調 X、Y 兩條線 |
| 調速無效 | EN 是否接在被蜂鳴器（3、11）或伺服（9、10）借走的 `~` 腳 | 查「~ 腳可用表」改接 |
| 程式無法上傳 | 設定區是否還有 `?` 沒填 | 填完所有腳位 |

---

## 成果繳交與評分

### 範例專案繳交

每個專案各自建立一個 Tinkercad 檔案，命名格式：`W14-專案編號-學號`（例如 `W14-P3-B12345678`）。

| 繳交項目 | 專案一 | 專案二 | 專案三 | 專案四 |
| --- | --- | --- | --- | --- |
| Tinkercad 分享連結 | ✅ | ✅ | ✅ | ✅ |
| NO／NC 選擇與理由 | ✅ | ✅ | ✅ | ✅ |
| 腳位規劃表 | ✅ | ✅ | ✅ | ✅ |
| 優先權流程圖 | — | — | ✅ | ✅ |
| 完成 Step 3 的電路截圖 | ✅ | ✅ | ✅ | ✅ |
| 情境思考題作答 | ✅ | ✅ | ✅ | ✅ |

> 🎯 **最低完成標準**：專案一、二完成 Step 3，專案三、四至少完成 Step 2。

### 練習題繳交

| 關卡 | 繳交內容 | 建議定位 |
| --- | --- | --- |
| Lv.1～Lv.2 | Tinkercad 連結＋情境思考題 | 課堂暖身，全班必做 |
| Lv.3～Lv.4 | ＋腳位規劃表＋優先權表 | 課後作業，必做 |
| Lv.5 | ＋腳位規劃表＋麵包板擺放截圖 | 選做加分 |
| Lv.6 BOSS | 完整提案（模式表、優先權流程圖、失效安全分析、SOP） | 期末專題預演，可分組 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 電路正確性 | 30% | 繼電器負載端、PIR、搖桿接線正確，每個 Step 功能正常 |
| 模式與優先權設計 | 25% | HOA 模式完整，優先權流程圖合理且與行為一致 |
| 失效安全分析 | 15% | NO／NC 選擇有合理理由 |
| 情境思考題 | 20% | 尤其是安全、SOP 與 💼 管理面向論述 |
| 麵包板擺放 | 10% | 控制端與負載端分區清楚 |

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 40 分鐘 | 認識三個新元件、NO／NC 與失效安全、HOA 與優先權；綜合測試器示範 |
| 第 1 節後段 | 專案一（Step 2「改接繼電器」是重點） |
| 第 2 節 | 專案二、專案三（分組進行） |
| 第 3 節前段 | 專案四（先畫優先權流程圖再接線） |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，其餘帶回家；預告期末專題 |

> 💡 **教學建議**：繼電器最容易卡在「負載端沒接成迴路」。建議在黑板上畫出「控制端」和「負載端」兩個框，強調它們是兩條完全分開的電路，學生接線時麵包板也分成兩區。

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 繼電器（Relay） | 用電控制的開關，讓 Arduino 切換另一條電路 |
| 線圈（Coil） | 繼電器內的電磁鐵，Arduino 控制這一端 |
| COM／NO／NC | 共同點／常開接點（平常斷開）／常閉接點（平常接通） |
| 電氣隔離 | 控制端和負載端是兩條分開的電路 |
| 失效安全（Fail-safe） | 系統故障時自動回到最安全的狀態 |
| PIR 人體感測器 | 偵測人體紅外線變化，有人移動輸出 HIGH |
| 類比搖桿 | 兩顆旋鈕合體，X、Y 兩軸各輸出 0～1023 |
| 死區（Deadband） | 搖桿中間一段範圍視為「沒推」，避免誤動作 |
| HOA | 手動（Hand）／停止（Off）／自動（Auto）三段模式 |
| 優先權 | 多個條件衝突時，誰先被判斷、誰說了算 |
| 上鎖掛牌（LOTO） | 設備維修時上鎖並掛牌，防止他人誤啟動的安全制度 |

---

## 反思與延伸討論

1. 本週的 NO／NC 選擇，本質上是在問：「**系統失敗時，我們寧可犯哪一種錯？**」（燈不亮 vs. 燈一直亮、門打不開 vs. 門關不上）。在管理決策中，你遇過哪些「兩種錯誤都有代價，只能選一種」的情況？
2. HOA 三種模式中，HAND 讓人可以「繞過自動化」。這是必要的彈性，也是風險的來源。企業應該如何管理「誰有權限切到手動」、「手動期間發生事故誰負責」？
3. 專案四的優先權流程圖，其實就是一份**決策規則**。下週的期末專題會整合更多元件、更多規則。你會用什麼方法確保團隊每個人對「誰優先」的理解一致？（提示：流程圖、規格書、測試案例）

---

## 本週檢核清單

- [ ] 能正確接上繼電器的控制端（線圈）與負載端（COM、NO、NC）
- [ ] 能依「失效安全」原則選擇 NO 或 NC，並說明理由
- [ ] 能正確接上 PIR，並理解它只偵測「移動」的特性
- [ ] 能正確接上搖桿（或用兩顆旋鈕代替），並理解死區的作用
- [ ] 能設計 HOA 三段模式，並說明各模式的使用時機
- [ ] 能畫出優先權流程圖，並讓系統行為與流程圖一致
- [ ] 完成 4 個獨立專案，並各自繳交 Tinkercad 檔案
- [ ] 完成練習題 Lv.1～Lv.4，挑戰 Lv.5 與 Lv.6

---

## 下週預告

從下週開始是為期兩週的**期末整合大專題**——不再學新元件，而是把這 14 週學過的**所有**元件與觀念，整合成一套完整的智慧場域系統。第 15 週是需求分析、系統設計與子系統開發；第 16 週是整合驗收與期末成果展演。本週 Lv.6 的提案格式，就是期末專題的起點。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 13 週](week-13.md) | [下一週：第 15 週 — 終極整合大專題（上）➡](week-15.md)
