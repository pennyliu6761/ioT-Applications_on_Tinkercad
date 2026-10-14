[⬅ 回目錄](../README.md) | [⬅ 上一週：第 9 週](week-09.md) | [下一週：第 11 週 — 產線戰情看板 ➡](week-11.md)

---

# 第 10 週：會持續運轉的智慧場域

## 主題專案：讓系統「一直轉」，而且能控制快慢與方向

> 🎯 **本週定位**：第 7 週的伺服馬達只能「轉到某個角度停住」。真實世界裡更多設備需要**一直轉**：風扇、輸送帶、捲門、洗衣機、搬運車輪子。這一週加入 **直流馬達（DC Motor）** 和它的「司機」——**馬達驅動晶片 L293D**。
>
> 本週是全學期**接線最複雜的一週**，正好符合我們的重點：
>
> 1. 讀懂情境需求，判斷馬達要不要**調速**、要不要**反轉**
> 2. 依需求選擇 L293D 的**接法**——需求越簡單，佔用的 Arduino 腳位越少
> 3. 規劃腳位，在 Tinkercad 把電路一步步接起來
>
> 控制馬達的程式都已經準備好，你只需要填腳位、調整參數、把功能開關從 `false` 改成 `true`。
>
> 🏁 **本週成果**：完成 **4 個獨立專案**，每個專案存成一個獨立的 Tinkercad 檔案繳交。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：直流馬達與 L293D](#認識新元件直流馬達與-l293d)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [專案一：廁所自動抽風扇](#專案一廁所自動抽風扇)
- [專案二：電動鐵捲門](#專案二電動鐵捲門)
- [專案三：智慧輸送帶](#專案三智慧輸送帶)
- [專案四：倉庫無人搬運車（AGV）](#專案四倉庫無人搬運車agv)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 果汁機三段速](#-lv1-果汁機三段速)
  - [Lv.2 伺服器散熱風扇](#-lv2-伺服器散熱風扇)
  - [Lv.3 洗衣機正反轉洗程](#-lv3-洗衣機正反轉洗程)
  - [Lv.4 三層樓貨梯](#-lv4-三層樓貨梯)
  - [Lv.5 雙輸送帶包裹分流站](#-lv5-雙輸送帶包裹分流站)
  - [🏆 Lv.6 BOSS 關：物聯網顧問提案](#-lv6-boss-關物聯網顧問提案)
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
- ✅ 「~ 腳可用表」：伺服借走 9、10；蜂鳴器借走 3、11
- ✅ 狀態改變偵測、鎖定與復歸、安全連鎖、安裝位置換算

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 直流馬達（DC Motor） | 通電就一直轉，電給得多轉得快，電反過來就反轉 | 風扇、輸送帶、捲門、車輪 |
| 馬達驅動晶片 L293D | Arduino 力氣太小推不動馬達，L293D 是「司機」，聽 Arduino 指揮、負責出力 | 用遙控器指揮冷氣：遙控器出指令，冷氣自己出力 |
| 油門腳 EN | 控制轉多快（接 `~` 腳可調速） | 油門踏板 |
| 方向腳 IN1、IN2 | 控制往哪轉 | 排檔（D 檔／R 檔） |
| 緩啟動 | 慢慢加速，不要一下衝到全速 | 電梯起步、輸送帶啟動 |

### 本週可用元件（只限這些）

| 元件 | 第幾週學的 |
| --- | --- |
| 單色 LED、RGB LED、220Ω | 第 1、3 週 |
| 按鈕、10kΩ | 第 2 週 |
| 旋鈕 | 第 4 週 |
| 蜂鳴器、光敏電阻 | 第 5 週 |
| TMP36 | 第 6 週 |
| 伺服馬達 | 第 7 週 |
| 超音波 HC-SR04 | 第 9 週 |
| 直流馬達、L293D | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：顧問公司這週接到四個需要「持續運轉」設備的委託案。客戶的需求各不相同：有的只要開關、有的要調速、有的要正反轉、有的要同時控制兩顆馬達。
>
> | 客戶 | 委託內容 | 馬達需求 | 難度 |
> | --- | --- | --- | --- |
> | 連鎖餐廳 | 廁所自動抽風扇 | 單向、開關→可調速 | ⭐ |
> | 五金行 | 電動鐵捲門 | 正反轉 | ⭐⭐ |
> | 食品工廠 | 智慧輸送帶 | 單向、調速、緩啟動 | ⭐⭐⭐ |
> | 物流倉庫 | 無人搬運車（AGV） | 兩顆馬達、調速 | ⭐⭐⭐⭐ |
>
> 每個專案都分成 3 個步驟，從最簡單的版本開始，每一步多接幾個元件、多一項功能。

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
- 在 Tinkercad 模擬時，馬達上會顯示轉速（RPM）。

### L293D：一顆晶片可以帶兩顆馬達

L293D 是一顆 16 隻腳的晶片，**一邊（左半）管馬達 A，另一邊（右半）管馬達 B**。

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

### L293D 接線地圖（白話版）

| 晶片腳 | 名稱 | 白話說明 | 接到哪裡 |
| --- | --- | --- | --- |
| 16 | VCC1 | 晶片自己的電源 | 5V（**一定要接**） |
| 8 | VCC2 | 給馬達用的電源 | 5V（**一定要接**） |
| 4、5、12、13 | GND | 接地 | GND（**四隻都要接**） |
| 1 | EN1 | 馬達 A 的**油門** | `~` 腳（要調速）或 5V（全速） |
| 2、7 | IN1、IN2 | 馬達 A 的**方向** | 數位腳，或固定接 5V／GND |
| 3、6 | OUT1、OUT2 | 馬達 A 的兩隻腳 | 馬達 A |
| 9 | EN2 | 馬達 B 的油門 | 同 EN1 |
| 10、15 | IN3、IN4 | 馬達 B 的方向 | 同 IN1、IN2 |
| 11、14 | OUT3、OUT4 | 馬達 B 的兩隻腳 | 馬達 B |

> ⚠️ 真實硬體上，馬達電源（Pin 8）通常會接**獨立電源**（例如電池盒），避免馬達啟動時拖垮 Arduino。Tinkercad 模擬可以直接接 Arduino 的 5V。

### 方向怎麼控制？

| IN1 | IN2 | 馬達 |
| --- | --- | --- |
| HIGH | LOW | 正轉 |
| LOW | HIGH | 反轉 |
| LOW | LOW | 停止 |

### ⭐ 本週最重要的觀念：依需求選接法，省腳位

不是每個情境都需要「調速＋正反轉」。**需求越簡單，L293D 可以把越多腳固定接到 5V 或 GND，Arduino 就省越多腳**：

| 接法 | 情境需求 | EN（油門） | IN1 | IN2 | 佔 Arduino 幾隻腳 |
| --- | --- | --- | --- | --- | --- |
| **A** | 只要開／關，固定全速、單一方向 | 5V | 數位腳 | GND | **1 隻**（一般腳） |
| **B** | 單一方向，但要調速 | `~` 腳 | 5V | GND | **1 隻**（`~` 腳） |
| **C** | 要正反轉，固定全速 | 5V | 數位腳 | 數位腳 | **2 隻**（一般腳） |
| **D** | 要正反轉，也要調速 | `~` 腳 | 數位腳 | 數位腳 | **3 隻**（含 1 隻 `~` 腳） |

> 🤔 **每個專案的 Step 0，第一件事就是決定用哪一種接法。**

### ~ 腳可用表（更新版）

直流馬達要調速時，油門腳 EN 必須接 `~` 腳。它和 RGB、調光燈一起搶下面這些 `~` 腳：

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
| ② **先決定接法 A／B／C／D，再規劃腳位** | 不需要的功能就固定接 5V 或 GND |
| ③ 要調速 → EN 接 `~` 腳；不調速 → EN 接 5V | |
| ④ 方向不對 → 對調馬達兩條線 | 不用改程式 |
| ⑤ 有伺服，9、10 不能調速；有蜂鳴器，3、11 不能調速 | 兩者都有時只剩 5、6 |
| ⑥ 每條燈線一顆 220Ω；按鈕、光敏電阻一顆 10kΩ | L293D、馬達、HC-SR04、伺服、蜂鳴器、旋鈕、TMP36 不用 |
| ⑦ Pin 0、1 先不要用；腳位不夠時借 A 腳 | A 腳也能接 IN 腳、Trig、Echo、燈、按鈕 |

---

## 共用工具

### 🔧 工具 1：馬達測試器（接法 D）

接好 L293D 和馬達後先跑這支：正轉 2 秒 → 停 1 秒 → 反轉 2 秒 → 停 1 秒 → 半速正轉 2 秒。觀察 Tinkercad 馬達上的轉速顯示。

```cpp
int pinEN  = 5;   // ← 油門腳（~ 腳）
int pinIN1 = 7;   // ← 方向腳 1
int pinIN2 = 8;   // ← 方向腳 2

void run(int in1, int in2, int speed, String label) {
  Serial.println(label);
  digitalWrite(pinIN1, in1);
  digitalWrite(pinIN2, in2);
  analogWrite(pinEN, speed);
  delay(2000);
  analogWrite(pinEN, 0);
  digitalWrite(pinIN1, LOW);
  digitalWrite(pinIN2, LOW);
  Serial.println("Stop");
  delay(1000);
}

void setup() {
  pinMode(pinEN, OUTPUT);
  pinMode(pinIN1, OUTPUT);
  pinMode(pinIN2, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  run(HIGH, LOW, 255, "Forward, full speed");
  run(LOW, HIGH, 255, "Reverse, full speed");
  run(HIGH, LOW, 128, "Forward, half speed");
}
```

> 若你用的是接法 A、B、C，部分腳位固定接 5V 或 GND，測試器裡對應的行沒有作用是正常的——只要確認「該轉的時候會轉」即可。

### 🔧 其他工具（沿用前幾週）

| 工具 | 用途 | 出處 |
| --- | --- | --- |
| 接線驗證器 | 檢查燈的腳位 | 第 3 週起 |
| 類比讀值檢查器 | 檢查旋鈕、光敏電阻 | 第 5 週 |
| 溫度讀值檢查器 | 檢查 TMP36 | 第 6 週 |
| 伺服測試器 | 找伺服角度 | 第 7 週 |
| 超音波測距檢查器 | 檢查 HC-SR04 | 第 9 週 |

> 📌 **每個專案的固定流程**
> 1. **Step 0**：讀完情境，決定 L293D 接法（A／B／C／D），再填腳位規劃表
> 2. 先只接 L293D＋馬達，用馬達測試器確認會轉
> 3. 照 Step 1 → 2 → 3 逐步接線，每完成一步就把對應的功能開關改成 `true`

---

## 專案一：廁所自動抽風扇

⭐ **難度**：L293D 最省腳的接法，再逐步升級

**客戶需求**：連鎖餐廳的廁所抽風扇。第一版只要一顆開關：按一下開、再按一下關。第二版店長希望能**調整風量**（冬天開小一點）。第三版要**自動化**：廁所燈一亮（有人使用）風扇就轉，燈關掉後**再多轉 5 秒**把異味抽乾淨才停。

### Step 0：決定接法與腳位規劃

| 步驟 | 需求 | 你選哪種接法？ | 為什麼？ |
| --- | --- | --- | --- |
| Step 1 | 開／關、全速 | | |
| Step 2 | 加上調速 | | |

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| L293D IN1（Step 1 用） | | |
| L293D EN1（Step 2 用） | | |
| 風扇開關按鈕 | | |
| 風量旋鈕 | A0 | — |
| 光敏電阻（偵測廁所燈） | A1 | — |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | L293D（**接法 A**：EN1→5V、IN1→Arduino、IN2→GND）＋馬達＋按鈕 | 按一下風扇轉，再按一下停 | （不用改） |
| Step 2 | **改接成接法 B**：EN1 從 5V 改接 `~` 腳、IN1 從 Arduino 改接 5V；＋旋鈕 | 旋鈕調整風量 | `useSpeed = true` |
| Step 3 | ＋光敏電阻（含 10kΩ） | 燈亮自動轉，燈滅後再轉 5 秒 | `useAuto = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinIN1  = ?;        // 接法 A 用：L293D IN1
int pinEN   = ?;        // 接法 B 用：L293D EN1（~ 腳）
int btnFan  = ?;        // 風扇開關按鈕
int pinKnob = A0;       // 風量旋鈕
int pinLDR  = A1;       // 光敏電阻（偵測廁所燈）
int lightLine = 500;    // 光線高於這個數字算「燈亮著」
int afterRun  = 5000;   // 燈滅後多轉幾毫秒
bool useSpeed = false;  // Step 2 改接成接法 B 後改成 true
bool useAuto  = false;  // Step 3 接好後改成 true
// ============================

bool manualOn = false;
int lastButton = LOW;
bool everLit = false;
unsigned long lastLit = 0;

void setup() {
  pinMode(pinIN1, OUTPUT);
  pinMode(pinEN, OUTPUT);
  pinMode(btnFan, INPUT);
  Serial.begin(9600);
}

void loop() {
  // ---- 手動開關 ----
  int now = digitalRead(btnFan);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    manualOn = !manualOn;
    Serial.println(manualOn ? "Fan ON (manual)" : "Fan OFF (manual)");
  }
  lastButton = now;

  // ---- 自動：燈亮就轉，燈滅後多轉一段時間 ----
  bool autoOn = false;
  if (useAuto) {
    if (analogRead(pinLDR) > lightLine) {
      everLit = true;
      lastLit = millis();
    }
    autoOn = everLit && (millis() - lastLit < afterRun);
  }

  bool running = manualOn || autoOn;

  if (useSpeed) {                                     // 接法 B：用油門控制
    int speed = map(analogRead(pinKnob), 0, 1023, 80, 255);
    analogWrite(pinEN, running ? speed : 0);
  } else {                                            // 接法 A：用方向腳開關
    digitalWrite(pinIN1, running ? HIGH : LOW);
  }
  delay(50);
}
```

### 情境思考

1. 接法 A 和接法 B 都只佔 1 隻 Arduino 腳，差別在哪裡？為什麼接法 B 一定要用 `~` 腳？
2. Step 2 的旋鈕範圍是 80～255，而不是 0～255。為什麼最小值不設成 0？（提示：觀察馬達在很低的油門時有沒有在轉）
3. 💼 「燈滅後多轉 5 秒」在電費和顧客體驗之間是怎麼取捨的？如果你是店長，你會設幾秒？

### ✅ 專案一驗收

- [ ] Step 1 按鈕能切換風扇開關，且 Arduino 只用了 1 隻腳控制馬達
- [ ] Step 2 改接成接法 B 後，旋鈕能調整風量
- [ ] Step 3 燈亮自動轉、燈滅後約 5 秒才停
- [ ] L293D 的 VCC1、VCC2、四隻 GND 都有接

---

## 專案二：電動鐵捲門

⭐⭐ **難度**：正反轉（接法 C）＋ 停止鈕 ＋ 下降防壓

**客戶需求**：五金行的電動鐵捲門。按「上升鈕」捲門往上捲、按「下降鈕」往下放，約 4 秒到底自動停。門在動的時候，門口的燈號顯示上升中／下降中，蜂鳴器間歇提醒路人注意。**下降時門下方若偵測到東西（30 cm 內），立刻改為上升 1 秒**，避免壓到人或車。

### Step 0：決定接法與腳位規劃

| 問題 | 你的答案 |
| --- | --- |
| 鐵捲門需要調速嗎？需要反轉嗎？ | |
| 所以選哪一種接法？佔幾隻腳？ | |

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| L293D IN1／IN2 | | |
| 上升鈕、下降鈕、停止鈕 | | |
| 上升中燈、下降中燈 | | |
| 蜂鳴器 | | |
| HC-SR04 Trig／Echo（門下方） | | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | L293D（接法 C）＋馬達＋上升鈕＋下降鈕 | 按上升正轉、按下降反轉，4 秒自動停 | （不用改） |
| Step 2 | ＋停止鈕＋2 顆燈 | 隨時可按停止；燈號顯示上升中／下降中 | `useStop = true` |
| Step 3 | ＋HC-SR04＋蜂鳴器 | 移動中間歇嗶聲；下降時偵測到障礙物立刻改上升 1 秒 | `useSafety = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinIN1 = ?, pinIN2 = ?;          // L293D 方向腳（接法 C，EN1 接 5V）
int btnUp = ?, btnDown = ?;          // 上升鈕、下降鈕
int btnStop = ?;                     // 停止鈕
int ledUp = ?, ledDown = ?;          // 上升中燈、下降中燈
int pinBuzzer = ?;                   // 蜂鳴器
int pinTrig = ?, pinEcho = ?;        // HC-SR04（門下方）
int travelTime = 4000;               // 捲到底需要幾毫秒
float safeCM = 30;                   // 下降時小於這個距離就停
bool useStop   = false;              // Step 2 接好後改成 true
bool useSafety = false;              // Step 3 接好後改成 true
// ============================

int state = 0;                       // 0=停止, 1=上升, 2=下降
unsigned long startT = 0;
unsigned long runLimit = 0;
int lastUp = LOW, lastDown = LOW, lastStop = LOW;
int tick = 0;

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

void go(int s, unsigned long limit) {
  state = s;
  startT = millis();
  runLimit = limit;
}

void setup() {
  pinMode(pinIN1, OUTPUT);
  pinMode(pinIN2, OUTPUT);
  pinMode(btnUp, INPUT);
  pinMode(btnDown, INPUT);
  pinMode(btnStop, INPUT);
  pinMode(ledUp, OUTPUT);
  pinMode(ledDown, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  Serial.begin(9600);
}

void loop() {
  tick++;
  if (pressed(btnUp, lastUp))     { go(1, travelTime); Serial.println("Door UP"); }
  if (pressed(btnDown, lastDown)) { go(2, travelTime); Serial.println("Door DOWN"); }
  if (useStop && pressed(btnStop, lastStop)) { state = 0; Serial.println("STOP"); }

  // 到底自動停
  if (state != 0 && millis() - startT > runLimit) {
    Serial.println(state == 1 ? "Reached TOP" : "Reached BOTTOM");
    state = 0;
  }

  // 下降防壓：偵測到障礙物，改上升 1 秒
  if (useSafety && state == 2 && readCM(pinTrig, pinEcho) < safeCM) {
    Serial.println("OBSTACLE! Reversing up");
    tone(pinBuzzer, 2000, 500);
    go(1, 1000);
  }

  // 馬達方向
  digitalWrite(pinIN1, state == 1 ? HIGH : LOW);
  digitalWrite(pinIN2, state == 2 ? HIGH : LOW);

  // 燈號與提醒聲
  if (useStop) {
    digitalWrite(ledUp,   state == 1 ? HIGH : LOW);
    digitalWrite(ledDown, state == 2 ? HIGH : LOW);
  }
  if (useSafety && state != 0 && tick % 10 == 0) tone(pinBuzzer, 1000, 80);

  delay(50);
}
```

### 情境思考

1. 用馬達測試器確認後，按「上升」馬達卻是往下的方向。**不改程式**要怎麼修？
2. 為什麼「防壓」只在**下降**時檢查，上升時不用？
3. 💼 鐵捲門壓傷人的事故時有所聞。除了防壓感測器，五金行老闆在管理上還可以做什麼？（提示：操作權限、定期檢查、張貼警示）

### ✅ 專案二驗收

- [ ] 按上升正轉、按下降反轉，約 4 秒自動停
- [ ] 停止鈕隨時有效，燈號正確顯示移動方向
- [ ] 移動中間歇嗶聲
- [ ] 下降時把 HC-SR04 小圓點拉近 30 cm 內，門立刻改為上升
- [ ] 腳位規劃表說明為什麼選接法 C，EN1 接 5V

---

## 專案三：智慧輸送帶

⭐⭐⭐ **難度**：調速（接法 B）＋ 緩啟動 ＋ 超音波 ＋ RGB ＋ 蜂鳴器 → **`~` 腳剛好用完**

**客戶需求**：食品工廠的包裝輸送帶，只往一個方向跑。

- 旋鈕調整速度；啟動和減速都要**慢慢變化**，避免產品倒下
- 輸送帶末端裝超音波：產品接近末端（30 cm 內）自動**減速**；到達末端（10 cm 內）**停止並鎖住**，等作業員把產品拿走、按「繼續鈕」才再啟動
- RGB 顯示狀態：運轉綠、減速黃、停止紅；停止時蜂鳴器提醒

### Step 0：決定接法與腳位規劃

| 問題 | 你的答案 |
| --- | --- |
| 輸送帶需要調速嗎？需要反轉嗎？選哪種接法？ | |
| 這個系統有蜂鳴器，`~` 腳剩哪幾隻？ | |
| EN 要 1 隻 `~` 腳、RGB（黃色需要調光）要 3 隻 `~` 腳，夠嗎？ | |

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| L293D EN1 | | |
| 速度旋鈕 | A0 | — |
| HC-SR04 Trig／Echo（末端） | | |
| 繼續鈕 | | |
| 狀態 RGB（R / G / B） | | |
| 蜂鳴器 | | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | L293D（接法 B）＋馬達＋旋鈕 | 旋鈕調速，速度變化是慢慢加減的 | （不用改） |
| Step 2 | ＋HC-SR04＋繼續鈕 | 產品接近末端減速、到末端停止，按繼續鈕才再走 | `useSensor = true` |
| Step 3 | ＋RGB＋蜂鳴器 | 綠／黃／紅狀態燈，停止時提醒 | `useStatus = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int pinEN = ?;                       // L293D EN1（~ 腳；接法 B：IN1 接 5V、IN2 接 GND）
int pinKnob = A0;                    // 速度旋鈕
int pinTrig = ?, pinEcho = ?;        // HC-SR04（末端）
int btnResume = ?;                   // 繼續鈕
int pinR = ?, pinG = ?, pinB = ?;    // 狀態 RGB（避開 3、11）
int pinBuzzer = ?;                   // 蜂鳴器
float slowCM = 30;                   // 減速距離
float stopCM = 10;                   // 停止距離
int rampStep = 5;                    // 每次加減速的幅度（越小越平順）
bool useSensor = false;              // Step 2 接好後改成 true
bool useStatus = false;              // Step 3 接好後改成 true
// ============================

int current = 0;                     // 目前油門
bool halted = false;
int lastResume = LOW;

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
  pinMode(pinEN, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(btnResume, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int base = map(analogRead(pinKnob), 0, 1023, 80, 255);
  int target = base;
  bool slowing = false;

  if (useSensor) {
    float d = readCM(pinTrig, pinEcho);
    if (d < stopCM && !halted) {
      halted = true;
      Serial.println("Product at END - conveyor STOPPED");
      if (useStatus) tone(pinBuzzer, 800, 400);
    } else if (d < slowCM) {
      target = base / 3;                         // 接近末端：降到三分之一
      slowing = true;
    }

    int now = digitalRead(btnResume);
    if (now == HIGH && lastResume == LOW) {
      delay(50);
      if (halted && d >= stopCM) {
        halted = false;
        Serial.println("Resumed");
      } else if (halted) {
        Serial.println("Remove the product first");
      }
    }
    lastResume = now;
    if (halted) target = 0;
  }

  // ---- 緩啟動／緩停止：每次只往目標靠近一點 ----
  if (current < target) current = min(current + rampStep, target);
  if (current > target) current = max(current - rampStep, target);
  analogWrite(pinEN, current);

  if (useStatus) {
    if (halted)       setColor(255, 0, 0);       // 停止：紅
    else if (slowing) setColor(255, 180, 0);     // 減速：黃
    else              setColor(0, 255, 0);       // 運轉：綠
  }
  delay(40);
}
```

### 情境思考

1. 把 `rampStep` 改成 255（等於沒有緩啟動），觀察馬達轉速變化。對輸送帶上的玻璃瓶裝產品會有什麼影響？
2. 你的 EN 和 RGB 共用了哪 4 隻 `~` 腳？如果客戶要再加一顆「調光照明燈」，還放得下嗎？
3. 💼 「到末端就停、要人按繼續」會不會讓產線變慢？從產能和安全的角度，討論這個設計值不值得。

### ✅ 專案三驗收

- [ ] 旋鈕調速時，速度是慢慢變化而不是瞬間跳
- [ ] 產品接近末端減速、到末端停止，產品沒拿走前按繼續無效
- [ ] RGB 顯示綠／黃／紅，停止時蜂鳴器提醒
- [ ] EN 與 RGB 都避開 3、11，腳位規劃表說明 `~` 腳分配

---

## 專案四：倉庫無人搬運車（AGV）

⭐⭐⭐⭐ **難度**：**一顆 L293D 帶兩顆馬達** ＋ 循線 ＋ 避障 ＋ 伺服貨叉 → **伺服＋蜂鳴器，`~` 腳只剩 5、6**

**客戶需求**：物流倉庫的地上貼了一條黑色引導線，無人搬運車（AGV）要沿著線走：

- 車底左右各一顆光敏電阻看地面：**左邊看到黑線 → 往左修正；右邊看到黑線 → 往右修正**；都沒看到 → 直走
- 兩邊**同時**看到黑線（站點的橫線）→ 停下來，貨叉（伺服）升起、放下完成裝卸，再繼續走
- 前方有超音波：20 cm 內有人或障礙物就停車並鳴叫，障礙消失後繼續
- 一顆啟動／停止鈕

> 🚗 **轉彎原理**：AGV 沒有方向盤，靠「左右輪速度不同」轉彎——要往左轉，就讓左輪慢、右輪快。

### Step 0：決定接法與腳位規劃（本專案的重頭戲）

| 問題 | 你的答案 |
| --- | --- |
| 兩顆輪子要調速嗎？要倒退嗎？選哪種接法？ | 本專案只前進、要調速 → 接法 B（每顆馬達 1 隻 `~` 腳） |
| 系統有伺服＋蜂鳴器，`~` 腳只剩哪兩隻？ | |
| 兩顆馬達的 EN 各接哪一隻？ | |

| 元件 | 需要幾隻腳 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- | --- |
| 左輪 L293D EN1 | 1 | | |
| 右輪 L293D EN2 | 1 | | |
| 左、右循線光敏電阻 | 2（A 腳） | A0、A1 | — |
| HC-SR04 Trig／Echo | 2 | | |
| 蜂鳴器 | 1 | | |
| 啟動／停止鈕 | 1 | | |
| 貨叉伺服 | 1 | | |

> ⚠️ **陷阱**：如果 Step 1 時把 EN 接在 9 或 10，到了 Step 3 加上伺服，輪子就不能調速了，必須全部重新接線。**Step 0 就要把最後的元件考慮進去。**

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | L293D（兩側都用接法 B）＋2 顆馬達＋2 顆光敏電阻＋按鈕 | 按啟動後循線前進；調整光線滑桿模擬偏左偏右，兩輪轉速改變 | （不用改） |
| Step 2 | ＋HC-SR04＋蜂鳴器 | 前方有障礙停車鳴叫，障礙消失繼續 | `useObstacle = true` |
| Step 3 | ＋伺服（貨叉） | 兩邊都看到黑線時停車、貨叉升降、再出發 | `useFork = true` |

### 程式（只改設定區）

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int enLeft  = ?;                     // 左輪 L293D EN1（~ 腳）
int enRight = ?;                     // 右輪 L293D EN2（~ 腳）
int ldrLeft = A0, ldrRight = A1;     // 左、右循線光敏電阻
int pinTrig = ?, pinEcho = ?;        // 前方 HC-SR04
int pinBuzzer = ?;                   // 蜂鳴器
int btnStart  = ?;                   // 啟動／停止鈕
int pinFork   = ?;                   // 貨叉伺服
int lineDark = 300;                  // 光線低於這個數字算「看到黑線」
int cruise   = 180;                  // 直走速度
int slowSide = 60;                   // 修正方向時，內側輪的速度
float stopCM = 20;                   // 前方障礙停車距離
int forkDown = 0, forkUp = 70;       // 貨叉角度
bool useObstacle = false;            // Step 2 接好後改成 true
bool useFork     = false;            // Step 3 接好後改成 true
// ============================

Servo fork;
bool running = false;
int lastStart = LOW;
int tick = 0;

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
  fork.attach(pinFork);
  fork.write(forkDown);
  pinMode(enLeft, OUTPUT);
  pinMode(enRight, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(btnStart, INPUT);
  Serial.begin(9600);
}

void loop() {
  tick++;
  int now = digitalRead(btnStart);
  if (now == HIGH && lastStart == LOW) {
    delay(50);
    running = !running;
    Serial.println(running ? "AGV START" : "AGV STOP");
  }
  lastStart = now;

  if (!running) { drive(0, 0); delay(50); return; }

  // ---- 避障：最優先 ----
  if (useObstacle && readCM(pinTrig, pinEcho) < stopCM) {
    drive(0, 0);
    if (tick % 10 == 0) {
      tone(pinBuzzer, 1500, 150);
      Serial.println("OBSTACLE - waiting");
    }
    delay(50);
    return;
  }

  bool L = analogRead(ldrLeft)  < lineDark;
  bool R = analogRead(ldrRight) < lineDark;

  if (L && R) {                                  // 站點橫線
    drive(0, 0);
    Serial.println("STATION reached");
    if (useFork) {
      tone(pinBuzzer, 1000, 100);
      fork.write(forkUp);   delay(1500);         // 貨叉升起
      fork.write(forkDown); delay(1000);         // 貨叉放下
      Serial.println("Load transfer done");
    }
    drive(cruise, cruise);                       // 往前開，離開橫線
    delay(800);
  } else if (L) {
    drive(slowSide, cruise);                     // 往左修正
  } else if (R) {
    drive(cruise, slowSide);                     // 往右修正
  } else {
    drive(cruise, cruise);                       // 直走
  }
  delay(30);
}
```

### 情境思考

1. 用你的腳位規劃表說明：兩顆馬達的 EN 為什麼只能接 5 和 6？如果客戶要求 AGV 能**倒車**，需要再多幾隻腳？還放得下嗎？
2. 在 Tinkercad 裡把**左邊**光敏電阻調暗，AGV 應該往左修正。如果你看到的是右輪變慢，問題在哪？**不改程式**要怎麼修？
3. 💼 AGV 遇到障礙物會停下來等。如果有員工把貨物堆在走道上，AGV 會一直卡著。管理上應該怎麼處理？AGV 系統還需要什麼功能？（提示：等太久要通報誰？）

### ✅ 專案四驗收

- [ ] 按啟動後兩輪轉動；左側看到黑線時左輪變慢、右側看到時右輪變慢
- [ ] 前方 20 cm 內有障礙物時停車鳴叫，障礙消失後繼續
- [ ] 兩側同時看到黑線時停車、貨叉升降後繼續前進
- [ ] 一顆 L293D 同時帶兩顆馬達，兩組 EN 接在 5、6
- [ ] 腳位規劃表說明「Step 0 就要考慮伺服」的理由

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：四個範例專案是「跟著做」，這一區換你「自己規劃」。每一關先回答「**要調速嗎？要反轉嗎？**」，選好 L293D 接法，再規劃腳位、動手接線。
>
> 📈 **難度安排**：
>
> | 關卡 | 馬達 | L293D 接法 | 其他元件 | 配線重點 |
> | --- | --- | --- | --- | --- |
> | Lv.1 | 1 | B | 按鈕＋3 LED | 單向調速只佔 1 隻 `~` 腳 |
> | Lv.2 | 1 | B | TMP36＋LED | 溫度決定轉速 |
> | Lv.3 | 1 | C | 按鈕＋旋鈕＋LED＋蜂鳴器 | 正反轉、不調速 |
> | Lv.4 | 1 | D | 3 按鈕＋3 LED＋蜂鳴器 | 正反轉又調速，佔 3 隻腳 |
> | Lv.5 | 2 | D＋B | 伺服＋超音波＋旋鈕＋RGB＋蜂鳴器＋2 按鈕 | `~` 腳只剩 5、6，一般腳不夠 |
> | Lv.6 | 自訂 | 自訂 | 自訂 | 顧問提案 |
>
> 📌 **作答規則**：每一關先寫出「選哪種接法、為什麼」，再填腳位規劃表；接好後先用馬達測試器確認會轉，最後才上傳程式。程式只需要改「設定區」。

---

### 🟢 Lv.1 果汁機三段速

**情境**：手搖飲店的果汁機有一顆按鈕，每按一次切換：**停止 → 低速 → 中速 → 高速 → 停止**。三顆燈顯示目前段數。

**元件**：L293D＋直流馬達＋1 顆按鈕＋3 顆單色 LED

**你要想的問題**：

- 果汁機不需要反轉，要調速。選哪種接法？IN1、IN2 各接到哪裡？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinEN   = ?;                 // L293D EN1（~ 腳）
int btnMode = ?;                 // 段數按鈕
int led[3]  = { ?, ?, ? };       // 低、中、高速燈
int speeds[4] = { 0, 100, 170, 255 };   // 停止、低、中、高
// ============================

int level = 0;
int lastButton = LOW;

void setup() {
  pinMode(pinEN, OUTPUT);
  pinMode(btnMode, INPUT);
  for (int i = 0; i < 3; i++) pinMode(led[i], OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int now = digitalRead(btnMode);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    level = (level + 1) % 4;
    Serial.print("Speed level: ");
    Serial.println(level);
  }
  lastButton = now;

  analogWrite(pinEN, speeds[level]);
  for (int i = 0; i < 3; i++) {
    digitalWrite(led[i], (i < level) ? HIGH : LOW);
  }
}
```

**情境思考**：

1. 三顆燈可以接在 `~` 腳嗎？需要嗎？
2. 💼 果汁機為什麼通常設計「段數」而不是「無段旋鈕」？從店員操作與產品一致性來想。

---

### 🟢 Lv.2 伺服器散熱風扇

**情境**：小型機房的散熱風扇依溫度自動調整：**25°C 以下停止**，25～40°C 之間溫度越高轉越快，40°C 以上全速並亮警示燈。

**元件**：L293D＋直流馬達＋TMP36＋1 顆單色 LED

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinEN   = ?;          // L293D EN1（~ 腳）
int pinTemp = A0;         // TMP36
int ledHot  = ?;          // 過熱警示燈
int startTemp = 25;       // 開始轉的溫度
int fullTemp  = 40;       // 全速的溫度
int minSpeed  = 80;       // 剛開始轉的最低油門
// ============================

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void setup() {
  pinMode(pinEN, OUTPUT);
  pinMode(ledHot, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  float t = readC(pinTemp);
  int speed = 0;
  if (t >= startTemp) {
    speed = map((int)t, startTemp, fullTemp, minSpeed, 255);
    speed = constrain(speed, minSpeed, 255);
  }
  analogWrite(pinEN, speed);
  digitalWrite(ledHot, t >= fullTemp ? HIGH : LOW);

  Serial.print("Temp: ");
  Serial.print(t);
  Serial.print(" C   Fan: ");
  Serial.println(speed);
  delay(300);
}
```

**情境思考**：

1. 第 6 週專案二用「LED 亮度」代表風扇轉速，這週換成真的馬達。電路上改了什麼？程式上改了什麼？
2. 💼 風扇「越熱轉越快」比「永遠全速」省電，但機房主管擔心風扇轉太慢會來不及散熱。你會怎麼說服他？需要什麼數據？

---

### 🟡 Lv.3 洗衣機正反轉洗程

**情境**：自助洗衣店的洗衣機，按「開始鈕」後執行洗程：**正轉 3 秒 → 停 1 秒 → 反轉 3 秒 → 停 1 秒**，算一輪。旋鈕設定要洗幾輪（1～5 輪）。洗衣中燈亮，洗完播放提示旋律。

**元件**：L293D＋直流馬達＋1 顆按鈕＋1 顆旋鈕＋1 顆 LED＋1 顆蜂鳴器

**你要想的問題**：

- 洗衣機要反轉，但不需要調速。選哪種接法？EN 接哪裡？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinIN1 = ?, pinIN2 = ?;      // L293D 方向腳（接法 C，EN1 接 5V）
int btnStart  = ?;               // 開始鈕
int pinKnob   = A0;              // 輪數旋鈕
int ledWash   = ?;               // 洗衣中燈
int pinBuzzer = ?;               // 蜂鳴器
int spinTime  = 3000;            // 每次轉幾毫秒
int pauseTime = 1000;            // 每次停幾毫秒
// ============================

int lastButton = LOW;

void motor(int in1, int in2, int ms) {
  digitalWrite(pinIN1, in1);
  digitalWrite(pinIN2, in2);
  delay(ms);
}

void setup() {
  pinMode(pinIN1, OUTPUT);
  pinMode(pinIN2, OUTPUT);
  pinMode(btnStart, INPUT);
  pinMode(ledWash, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int rounds = map(analogRead(pinKnob), 0, 1024, 1, 6);   // 1~5 輪

  int now = digitalRead(btnStart);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    Serial.print("Wash start: ");
    Serial.print(rounds);
    Serial.println(" rounds");
    digitalWrite(ledWash, HIGH);

    for (int r = 1; r <= rounds; r++) {
      Serial.print("Round ");
      Serial.println(r);
      motor(HIGH, LOW, spinTime);    // 正轉
      motor(LOW, LOW, pauseTime);    // 停
      motor(LOW, HIGH, spinTime);    // 反轉
      motor(LOW, LOW, pauseTime);    // 停
    }

    digitalWrite(ledWash, LOW);
    Serial.println("Wash DONE");
    int notes[] = { 523, 659, 784, 1047 };
    for (int i = 0; i < 4; i++) {
      tone(pinBuzzer, notes[i], 200);
      delay(250);
    }
  }
  lastButton = now;
}
```

**情境思考**：

1. 為什麼正轉和反轉之間要「停 1 秒」，而不是直接切換？（提示：想想急速倒車對車子的傷害）
2. 💼 洗衣店老闆想推出「快洗」方案。在不改接線的情況下，你會調整哪些數字？快洗要收比較便宜還是比較貴？

---

### 🟡 Lv.4 三層樓貨梯

**情境**：小型倉庫的三層樓貨梯。每層一顆呼叫鈕、一顆樓層燈。貨梯每上（下）一層需要 2 秒；為了讓貨物不會晃倒，**每次出發都要緩啟動、抵達前要緩停止**。抵達時蜂鳴器「叮」一聲。

**元件**：L293D＋直流馬達＋3 顆按鈕＋3 顆 LED＋1 顆蜂鳴器

**Step 0：決定接法與腳位規劃**

| 問題 | 你的答案 |
| --- | --- |
| 貨梯要上下（反轉），也要緩啟動（調速）。選哪種接法？佔幾隻腳？ | |
| 有蜂鳴器，EN 要避開哪兩隻腳？ | |
| 全部加起來幾隻腳？12 隻夠嗎？ | |

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int pinEN  = ?;                      // L293D EN1（~ 腳，避開 3、11）
int pinIN1 = ?, pinIN2 = ?;          // L293D 方向腳（接法 D）
int btnFloor[3] = { ?, ?, ? };       // 1F、2F、3F 呼叫鈕
int ledFloor[3] = { ?, ?, ? };       // 1F、2F、3F 樓層燈
int pinBuzzer = ?;                   // 蜂鳴器
int floorTime = 2000;                // 每層樓移動時間（毫秒）
// ============================

int floorNow = 0;                    // 0=1F, 1=2F, 2=3F
int lastBtn[3] = { LOW, LOW, LOW };

// 緩啟動 → 等速 → 緩停止，總共約 floorTime 毫秒
void travel(bool up, int floors) {
  digitalWrite(pinIN1, up ? HIGH : LOW);
  digitalWrite(pinIN2, up ? LOW : HIGH);
  int total = floorTime * floors;
  for (int s = 0; s <= 255; s += 15) { analogWrite(pinEN, s); delay(20); }   // 緩啟動
  delay(total - 700);
  for (int s = 255; s >= 0; s -= 15) { analogWrite(pinEN, s); delay(20); }   // 緩停止
  digitalWrite(pinIN1, LOW);
  digitalWrite(pinIN2, LOW);
}

void setup() {
  pinMode(pinEN, OUTPUT);
  pinMode(pinIN1, OUTPUT);
  pinMode(pinIN2, OUTPUT);
  for (int f = 0; f < 3; f++) {
    pinMode(btnFloor[f], INPUT);
    pinMode(ledFloor[f], OUTPUT);
  }
  Serial.begin(9600);
}

void loop() {
  for (int f = 0; f < 3; f++) {
    digitalWrite(ledFloor[f], (f == floorNow) ? HIGH : LOW);
  }

  for (int f = 0; f < 3; f++) {
    int now = digitalRead(btnFloor[f]);
    if (now == HIGH && lastBtn[f] == LOW) {
      delay(50);
      if (f != floorNow) {
        Serial.print("Going to ");
        Serial.print(f + 1);
        Serial.println("F");
        digitalWrite(ledFloor[floorNow], LOW);
        travel(f > floorNow, abs(f - floorNow));
        floorNow = f;
        tone(pinBuzzer, 1319, 300);
        Serial.print("Arrived ");
        Serial.print(floorNow + 1);
        Serial.println("F");
      }
    }
    lastBtn[f] = now;
  }
}
```

**情境思考**：

1. 這部貨梯「靠時間」判斷到了哪一層，而不是真的量位置。這樣做有什麼風險？你學過的哪個感測器可以用來真正確認樓層？
2. 貨梯移動中有人按了其他樓層的按鈕，會發生什麼事？真實電梯是怎麼處理的？
3. 💼 貨梯的「緩啟動、緩停止」讓每趟多花一點時間。對倉庫來說，這個時間成本換到了什麼？

---

### 🔴 Lv.5 雙輸送帶包裹分流站

**情境**：物流中心有一條**主輸送帶**和一條**支線輸送帶**，接在同一顆 L293D 上。主輸送帶上的包裹經過分流點時（超音波偵測），主輸送帶暫停、讀取重量（旋鈕模擬磅秤）：

- **10 公斤以上**：分流擋板（伺服）轉向支線，支線輸送帶啟動 2 秒把包裹送走
- **10 公斤以下**：擋板維持直行，主輸送帶繼續

另有兩顆按鈕：**急停鈕**（全部停止、RGB 紅燈、再按恢復）、**卡件回退鈕**（按住時主輸送帶反轉，把卡住的包裹退回來）。RGB 狀態：運轉綠、分流中黃、急停紅。

| 元件 | 說明 |
| --- | --- |
| 主輸送帶馬達 | 要調速、要反轉（卡件回退）→ 接法 D |
| 支線輸送帶馬達 | 單向、要調速 → 接法 B |
| 分流擋板 | 伺服 |
| 分流點偵測 | HC-SR04 |
| 重量 | 旋鈕（A0） |
| 狀態 | RGB＋蜂鳴器 |
| 操作 | 急停鈕、卡件回退鈕 |

**Step 0：腳位規劃（本關重點）**

| 元件 | 需要幾隻腳 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- | --- |
| 主輸送帶 EN1、IN1、IN2 | 3 | | |
| 支線輸送帶 EN2 | 1 | | |
| 分流伺服 | 1 | | |
| HC-SR04 Trig／Echo | 2 | | |
| 蜂鳴器 | 1 | | |
| 狀態 RGB | 3 | | |
| 急停鈕、回退鈕 | 2 | | |
| **一般腳位合計** | | | |
| **Pin 2～13 可用數量** | 12 | | |

> 🤔 **規劃提示**：
> - 伺服＋蜂鳴器 → `~` 腳只剩 5、6，剛好給兩顆馬達的 EN。那 RGB 的黃色怎麼辦？（提示：黃＝紅＋綠全亮）
> - 一般腳不夠時，A1～A5 可以借來接什麼？

**程式**：

```cpp
#include <Servo.h>

// ===== 設定區：只改這裡 =====
int enMain = ?, inMain1 = ?, inMain2 = ?;   // 主輸送帶（接法 D）
int enBranch = ?;                           // 支線輸送帶 EN2（接法 B）
int pinGate  = ?;                           // 分流伺服
int pinTrig = ?, pinEcho = ?;               // 分流點 HC-SR04
int pinKnob = A0;                           // 重量旋鈕
int pinR = ?, pinG = ?, pinB = ?;           // 狀態 RGB（全亮全滅）
int pinBuzzer = ?;                          // 蜂鳴器
int btnEStop = ?, btnBack = ?;              // 急停鈕、卡件回退鈕
int mainSpeed = 180, branchSpeed = 200;
float parcelCM = 15;                        // 小於這個距離算「包裹到達分流點」
int heavyKg = 10;
int straightAngle = 0, divertAngle = 60;
// ============================

Servo gate;
bool eStopped = false;
bool wasParcel = false;
int lastEStop = LOW;
int counts[2] = { 0, 0 };                   // 主線、支線件數

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

void mainBelt(int speed) {                  // 正數前進、負數後退、0 停止
  digitalWrite(inMain1, speed > 0 ? HIGH : LOW);
  digitalWrite(inMain2, speed < 0 ? HIGH : LOW);
  analogWrite(enMain, abs(speed));
}

void setup() {
  gate.attach(pinGate);
  gate.write(straightAngle);
  pinMode(enMain, OUTPUT);
  pinMode(inMain1, OUTPUT);
  pinMode(inMain2, OUTPUT);
  pinMode(enBranch, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(btnEStop, INPUT);
  pinMode(btnBack, INPUT);
  Serial.begin(9600);
}

void loop() {
  // ---- 急停：最優先 ----
  int now = digitalRead(btnEStop);
  if (now == HIGH && lastEStop == LOW) {
    delay(50);
    eStopped = !eStopped;
    Serial.println(eStopped ? "!!! E-STOP !!!" : "Resumed");
    if (eStopped) tone(pinBuzzer, 400, 500);
  }
  lastEStop = now;

  if (eStopped) {
    mainBelt(0);
    analogWrite(enBranch, 0);
    setColor(HIGH, LOW, LOW);
    delay(50);
    return;
  }

  // ---- 卡件回退：按住時主輸送帶反轉 ----
  if (digitalRead(btnBack) == HIGH) {
    mainBelt(-120);
    setColor(HIGH, HIGH, LOW);
    delay(50);
    return;
  }

  setColor(LOW, HIGH, LOW);
  mainBelt(mainSpeed);

  // ---- 分流：包裹剛到達分流點 ----
  bool parcel = readCM(pinTrig, pinEcho) < parcelCM;
  if (parcel && !wasParcel) {
    mainBelt(0);
    int kg = map(analogRead(pinKnob), 0, 1023, 0, 30);
    Serial.print("Parcel ");
    Serial.print(kg);
    Serial.print(" kg -> ");
    if (kg >= heavyKg) {
      Serial.println("BRANCH");
      setColor(HIGH, HIGH, LOW);
      tone(pinBuzzer, 1200, 100);
      gate.write(divertAngle);
      delay(400);
      analogWrite(enBranch, branchSpeed);
      mainBelt(mainSpeed);                  // 主線把包裹推進支線
      delay(2000);
      analogWrite(enBranch, 0);
      gate.write(straightAngle);
      counts[1]++;
    } else {
      Serial.println("MAIN");
      counts[0]++;
    }
    Serial.print("Totals  Main: ");
    Serial.print(counts[0]);
    Serial.print("  Branch: ");
    Serial.println(counts[1]);
  }
  wasParcel = parcel;
  delay(40);
}
```

**情境思考**：

1. 你的腳位規劃表用了幾隻一般腳？借了哪幾隻 A 腳、接了什麼？
2. 「卡件回退」設計成**按住才動、放開就停**，而急停是**按一下就鎖住**。為什麼兩者的操作方式不同？
3. 💼 分流站每天處理上萬件包裹。Serial Monitor 的件數統計，對物流中心主管有什麼用？還應該記錄什麼？

---

### 🏆 Lv.6 BOSS 關：物聯網顧問提案

**情境**：你的顧問公司接到新客戶。從下面三個案例中**任選一個**，自己完成需求分析、元件選擇、腳位規劃與 Tinkercad 實作。

> **案例 A：自動洗車機**
> 車子開進洗車道（超音波偵測停妥）後，按開始鈕：刷子馬達正反轉刷洗、風乾馬達依序運轉。過程中車子移動（距離改變）立刻全部停止並鳴叫。
>
> **案例 B：迴轉壽司輸送台**
> 輸送台馬達用旋鈕調速，尖峰時段轉快、離峰轉慢。輸送台旁的光敏電阻偵測盤子被拿走，計算每小時取盤數。廚師按按鈕可暫停輸送台補盤，暫停時 RGB 亮黃。
>
> **案例 C：自訂情境**
> 自己找一個生活或職場情境（例如：自動旋轉門、跑步機、電動窗簾、機場行李轉盤、捲線機）。

**最低規格**：

- 至少 1 顆直流馬達（經 L293D 控制）、1 種感測器、1 顆按鈕
- 總共使用 10 隻以上腳位
- 至少包含一個「安全停止」機制
- 程式可以從專案一～四或 Lv.1～Lv.5 改寫，說明你是從哪裡改來的、改了哪些設定

**繳交內容**：

1. **需求分析**：這個系統解決什麼問題？誰會操作？誰會被馬達動作影響？（3～5 句）
2. **馬達需求表**：每顆馬達要不要調速、要不要反轉？選擇哪一種 L293D 接法？省了幾隻腳？
3. **腳位規劃表**：標註每隻腳接什麼、是否為 `~` 腳，並說明如何處理伺服、蜂鳴器借走 `~` 腳的限制
4. **Tinkercad 實作**：電路截圖＋馬達測試器的檢查截圖
5. **安全設計說明**：什麼情況下馬達必須停止？如何恢復？

**驗收標準**：

- [ ] L293D 電源腳（16、8）、四隻 GND 全部接妥
- [ ] 接法選擇合理：不需要的功能固定接 5V 或 GND，沒有浪費腳位
- [ ] 需要調速的 EN 接在可用的 `~` 腳
- [ ] 安全停止機制確實有效
- [ ] 麵包板依功能分區，L293D 跨在凹槽上

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 馬達完全不轉 | L293D 的 16、8 是否接 5V；4、5、12、13 是否都接 GND | 逐腳核對「接線地圖」 |
| 馬達完全不轉（電源都接了） | EN 是否接到 5V 或 `~` 腳；晶片是否跨在凹槽上 | 接法 A、C 的 EN 要接 5V |
| 馬達方向相反 | 馬達兩條線的接法 | 對調馬達兩條線 |
| 只能轉一個方向 | IN2 是否接到 Arduino（接法 C、D） | 接法 A、B 本來就只能單向 |
| 調速沒效果，只有全速或停止 | EN 是否接在 `~` 腳；是否被伺服（9、10）或蜂鳴器（3、11）借走 | 查「~ 腳可用表」改接 |
| 低油門時馬達不轉 | 正常現象：油門太小推不動 | 設定最低速度（例如 80） |
| 兩顆馬達，動 A 卻是 B 在轉 | 左右兩側腳位是否接反 | 對照晶片圖：左半管 A、右半管 B |
| 晶片腳位對不上 | 缺口方向是否朝上 | 缺口朝上，左上角是 Pin 1 |
| 程式無法上傳 | 設定區是否還有 `?` 沒填 | 填完所有腳位 |

---

## 成果繳交與評分

### 範例專案繳交

每個專案各自建立一個 Tinkercad 檔案，命名格式：`W10-專案編號-學號`（例如 `W10-P3-B12345678`）。

| 繳交項目 | 專案一 | 專案二 | 專案三 | 專案四 |
| --- | --- | --- | --- | --- |
| Tinkercad 分享連結 | ✅ | ✅ | ✅ | ✅ |
| L293D 接法選擇與理由 | ✅ | ✅ | ✅ | ✅ |
| 腳位規劃表 | ✅ | ✅ | ✅ | ✅ |
| 完成 Step 3 的電路截圖 | ✅ | ✅ | ✅ | ✅ |
| 情境思考題作答 | ✅ | ✅ | ✅ | ✅ |

> 🎯 **最低完成標準**：專案一、二完成 Step 3，專案三、四至少完成 Step 2。

### 練習題繳交

| 關卡 | 繳交內容 | 建議定位 |
| --- | --- | --- |
| Lv.1～Lv.2 | Tinkercad 連結＋接法選擇＋情境思考題 | 課堂暖身，全班必做 |
| Lv.3～Lv.4 | ＋腳位規劃表 | 課後作業，必做 |
| Lv.5 | ＋腳位規劃表＋麵包板擺放截圖 | 選做加分 |
| Lv.6 BOSS | 完整顧問提案（需求分析、馬達需求表、腳位規劃、實作、安全設計） | 可分組 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 電路正確性 | 30% | L293D 接線正確，每個 Step 功能正常 |
| 接法選擇與腳位規劃 | 30% | 依需求選擇合理接法，`~` 腳分配正確 |
| 情境思考題 | 25% | 尤其是安全設計與 💼 管理面向論述 |
| 麵包板擺放 | 15% | L293D 位置正確、電源線整齊、分區清楚 |

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 40 分鐘 | 認識 L293D、接線地圖、四種接法、馬達測試器示範（全班一起接一次） |
| 第 1 節後段 | 專案一（Step 1 → Step 2 的「改接法」是重點） |
| 第 2 節 | 專案二、專案三（分組進行） |
| 第 3 節前段 | 專案四 |
| 第 3 節後段 | 練習題 Lv.1～Lv.2 當堂完成，Lv.3 之後帶回家 |

> 💡 **教學建議**：L293D 是全學期最容易接錯的元件。建議第 1 節讓全班先一起接一次「接法 D＋馬達測試器」，確認每個人的馬達都會正轉、反轉、半速，再開始專案。

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 直流馬達（DC Motor） | 通電就持續轉動的馬達，兩條線接反就反轉 |
| L293D 馬達驅動晶片 | Arduino 的「司機」，聽指令並提供馬達需要的大電流；一顆可帶兩顆馬達 |
| H 橋（H-Bridge） | L293D 內部讓電流可以正反向流動的電路，是馬達能反轉的原因 |
| 油門腳 EN（Enable） | 決定馬達轉多快；接 `~` 腳可調速，接 5V 固定全速 |
| 方向腳 IN1／IN2 | 決定馬達往哪轉 |
| 接法 A／B／C／D | 依「要不要調速、要不要反轉」選擇的四種接線方式 |
| 緩啟動／緩停止 | 速度慢慢加減，保護設備與貨物 |
| 差速轉向 | 左右輪速度不同來轉彎，AGV、掃地機器人都是這樣 |

---

## 反思與延伸討論

1. 本週的「接法 A／B／C／D」告訴我們：**需求越清楚，資源用得越省**。在管理工作中，你能想到哪些「先釐清需求，才不會浪費資源」的例子？
2. 專案四的 AGV 只要 Step 1 把 EN 接在 9 或 10，到 Step 3 就得全部重接。這和專案管理中「前期規劃不足，後期變更成本很高」的道理有什麼關係？
3. 本週好幾個情境（鐵捲門、輸送帶、貨梯、洗衣機）都刻意讓馬達**慢慢啟動、慢慢停止**，或在換方向前**先停一下**。這些設計犧牲了一點速度，換到了什麼？在產線上，你會怎麼向主管說明這個取捨？

---

## 本週檢核清單

- [ ] 能正確擺放 L293D（跨凹槽、缺口朝上），並接妥所有電源與接地腳
- [ ] 能說明 EN、IN1、IN2 的功能，並用馬達測試器確認正轉、反轉、調速
- [ ] 能依情境需求選擇接法 A／B／C／D，並說明省了幾隻腳
- [ ] 能在有伺服、蜂鳴器的系統中，正確分配 EN 的 `~` 腳
- [ ] 能用一顆 L293D 同時控制兩顆馬達
- [ ] 能說明緩啟動、防壓、急停等安全設計的目的
- [ ] 完成 4 個獨立專案，並各自繳交 Tinkercad 檔案
- [ ] 完成練習題 Lv.1～Lv.4，挑戰 Lv.5 與 Lv.6

---

## 下週預告

第 11 週我們要學習「產線資訊可視化」——**LCD 16x2 顯示器**，打造「產線戰情看板」。到目前為止，我們都靠燈號和 Serial Monitor 看資訊；下週起，系統可以直接在螢幕上顯示文字和數字，把十週來學過的各種感測器資料，整合到一塊真正的看板上。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 9 週](week-09.md) | [下一週：第 11 週 — 產線戰情看板 ➡](week-11.md)
