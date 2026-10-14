[⬅ 回目錄](../README.md) | [⬅ 上一週：第 12 週](week-12.md) | [下一週：第 14 週 — 智慧感應與操作台 ➡](week-14.md)

---

# 第 13 週：用 3 隻腳點亮整面看板

## 主題專案：位移暫存器與七段顯示器——省腳位的極致

> 🎯 **本週定位**：第 12 週的金庫把 18 隻腳全部用滿了。如果客戶還要 8 顆工作站指示燈、兩位數的計數器怎麼辦？這一週加入兩個新元件：
>
> - **位移暫存器 74HC595**：只用 **3 隻 Arduino 腳**，就能控制 **8 個輸出**；把好幾顆串在一起，**還是只用 3 隻腳**，就能控制 16、24、32 個輸出
> - **七段顯示器**：產線、電梯、計時器上最常見的數字顯示
>
> 和前幾週一樣，本週**重點不在寫程式**，而在於：
>
> 1. 讀懂情境需求，算出「需要幾個輸出」，決定要用幾顆 74HC595
> 2. 把 74HC595 的 16 隻腳接對，並學會把多顆晶片**串接**
> 3. 用「1 代表亮、0 代表滅」的方式，設計燈號圖案
>
> 送資料給 74HC595、顯示數字的程式都已經準備好，你只需要填腳位、修改設定、把功能開關從 `false` 改成 `true`。
>
> 🏁 **本週成果**：完成 **4 個獨立專案**，每個專案存成一個獨立的 Tinkercad 檔案繳交。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件 ①：位移暫存器 74HC595](#認識新元件-位移暫存器-74hc595)
- [認識新元件 ②：七段顯示器](#認識新元件-七段顯示器)
- [燈號圖案：用 1 和 0 畫圖](#燈號圖案用-1-和-0-畫圖)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [專案一：商店招牌燈效](#專案一商店招牌燈效)
- [專案二：行人號誌倒數器](#專案二行人號誌倒數器)
- [專案三：兩位數產量計數器](#專案三兩位數產量計數器)
- [專案四：產線安燈總看板](#專案四產線安燈總看板)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 電梯樓層顯示](#-lv1-電梯樓層顯示)
  - [Lv.2 音響音量燈條](#-lv2-音響音量燈條)
  - [Lv.3 週年慶抽獎機](#-lv3-週年慶抽獎機)
  - [Lv.4 機房兩位數溫度顯示](#-lv4-機房兩位數溫度顯示)
  - [Lv.5 產線節拍時間看板](#-lv5-產線節拍時間看板)
  - [🏆 Lv.6 BOSS 關：物聯網顧問提案](#-lv6-boss-關物聯網顧問提案)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～12 週）

- ✅ 13 種元件的接線（見第 12 週「全元件配線速查表」）
- ✅ 省腳位技巧：借 A 腳、L293D 省腳接法、I2C 版 LCD、鍵盤只接 3 行
- ✅ 狀態改變偵測、鎖定與復歸、安全連鎖、看板設計、操作流程設計

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 位移暫存器 74HC595 | 3 條線進、8 條線出的「擴充器」 | 延長線、網路分享器 |
| 串接（Daisy Chain） | 把多顆 74HC595 頭尾相連，還是只用 3 隻腳 | 一條延長線再接一條 |
| 七段顯示器 | 7 條小燈管拼出 0～9 | 電梯樓層、計時器、加油機 |
| 燈號圖案（位元） | 用 8 個 0 或 1 表示 8 顆燈的亮滅 | 開關面板上的一排撥桿 |
| `sendOut()`／`showNumber()` | 已寫好的函式，把圖案或數字送出去 | 按下「送出」鍵 |

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
| 直流馬達、L293D | 第 10 週 |
| LCD 16×2（平行版、I2C 版） | 第 11 週 |
| 4×4 矩陣鍵盤 | 第 12 週 |
| 位移暫存器 74HC595、七段顯示器（共陰極） | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：顧問公司進入下半學期，客戶的需求越來越「大」：要很多燈、要數字看板，但 Arduino 的腳就那麼多。這週的四個委託案，都要用 74HC595 解決「腳位不夠」的問題。
>
> | 客戶 | 委託內容 | 需要的輸出 | 74HC595 數量 | 難度 |
> | --- | --- | --- | --- | --- |
> | 手搖飲店 | 招牌燈效 | 8 顆燈 | 1 | ⭐ |
> | 市政府 | 行人號誌倒數器 | 1 位數字 | 1 | ⭐⭐ |
> | 食品工廠 | 兩位數產量計數器 | 2 位數字 | 2（串接） | ⭐⭐⭐ |
> | 電子工廠 | 產線安燈總看板 | 8 顆燈＋2 位數字 | 3（串接） | ⭐⭐⭐⭐ |
>
> 每個專案都分成 3 個步驟，從最簡單的版本開始，每一步多接幾個元件、多一項功能。

---

## 認識新元件 ①：位移暫存器 74HC595

### 它在做什麼？

想像一台**電梯**：乘客（資料）一個一個走進去排好（位移），等 8 個人都進去了，**電梯門一打開**（鎖存），8 個人同時出現在樓層上（8 個輸出同時改變）。

```
Arduino                74HC595                     8 個輸出
資料線 ─────────────► 一個一個排進去 ────┐
時脈線 ─────────────► 「下一個！」        ├──► Q0 Q1 Q2 Q3 Q4 Q5 Q6 Q7
鎖存線 ─────────────► 「開門！」全部出現 ─┘
```

所以只要 **3 條線**：

| 名稱 | 白話 | 晶片腳 | 接 Arduino |
| --- | --- | --- | --- |
| 資料（DS） | 這一位是 1 還是 0 | Pin 14 | 任何數位腳 |
| 時脈（SHCP） | 「收下這一位，換下一位」 | Pin 11 | 任何數位腳 |
| 鎖存（STCP） | 「8 位都到齊了，一起顯示」 | Pin 12 | 任何數位腳 |

### 74HC595 接線地圖

晶片要**跨在麵包板中間的凹槽上**，有半圓缺口的一端朝上。左排由上往下 1～8，右排由下往上 9～16。

```
          ┌───∪───┐
   Q1  1 ─┤       ├─ 16  VCC（5V）
   Q2  2 ─┤       ├─ 15  Q0
   Q3  3 ─┤       ├─ 14  DS 資料 ◄── Arduino
   Q4  4 ─┤74HC595├─ 13  OE（接 GND）
   Q5  5 ─┤       ├─ 12  STCP 鎖存 ◄── Arduino
   Q6  6 ─┤       ├─ 11  SHCP 時脈 ◄── Arduino
   Q7  7 ─┤       ├─ 10  MR（接 5V）
  GND  8 ─┤       ├─  9  Q7' 串接輸出 ──► 下一顆的 DS
          └───────┘
```

| 晶片腳 | 名稱 | 白話說明 | 接到哪裡 |
| --- | --- | --- | --- |
| 16 | VCC | 晶片電源 | 5V |
| 8 | GND | 接地 | GND |
| 10 | MR | 清除全部（不要清除） | **5V** |
| 13 | OE | 輸出開關（一直開著） | **GND** |
| 14 | DS | 資料 | Arduino 數位腳 |
| 11 | SHCP | 時脈 | Arduino 數位腳 |
| 12 | STCP | 鎖存 | Arduino 數位腳 |
| 15、1～7 | Q0～Q7 | 8 個輸出 | 燈或七段顯示器（**每條串 220Ω**） |
| 9 | Q7' | 串接輸出 | 下一顆 74HC595 的 DS（Pin 14） |

> ⚠️ **最常見的錯誤**：Q0 在**右邊**的 Pin 15，不是左邊！Q1～Q7 才在左排的 Pin 1～7。

### ⭐ 串接：再多燈也只要 3 隻腳

把第 1 顆的 **Q7'（Pin 9）** 接到第 2 顆的 **DS（Pin 14）**，兩顆的**時脈**和**鎖存**接在一起，就變成 16 個輸出：

```
Arduino ──資料──► [第 1 顆] Q7' ──► [第 2 顆] Q7' ──► [第 3 顆]
        ──時脈──► 三顆的 SHCP（Pin 11）全部接在一起
        ──鎖存──► 三顆的 STCP（Pin 12）全部接在一起
```

| 74HC595 數量 | 輸出數量 | 佔 Arduino 幾隻腳 |
| --- | --- | --- |
| 1 顆 | 8 | 3 |
| 2 顆串接 | 16 | **還是 3** |
| 3 顆串接 | 24 | **還是 3** |

> 💡 **第 1 顆**＝最靠近 Arduino 的那顆（資料線直接接進來的那顆）。

### 74HC595 的限制

| 可以 | 不行 |
| --- | --- |
| 控制 LED 亮／滅 | 調光（沒有 `~` 功能） |
| 控制七段顯示器 | 讀取按鈕、感測器（它只能「輸出」） |
| 控制 RGB 的全亮全滅顏色 | 直接推動馬達（要經過 L293D） |

---

## 認識新元件 ②：七段顯示器

### 段位名稱

```
     ─a─
    │   │
    f   b
    │   │
     ─g─
    │   │
    e   c
    │   │
     ─d─   ● dp（小數點）
```

- 7 條燈管（a～g）＋1 個小數點（dp），一共 8 個段位——**剛好對應 74HC595 的 8 個輸出**。
- 本課程使用**共陰極**：共同腳接 GND，段位腳給「1」就亮。在 Tinkercad 點選七段顯示器，確認屬性是「Cathode」。

### 接法（經過 74HC595）

| 七段顯示器腳 | 接到哪裡 |
| --- | --- |
| a | 74HC595 Q0（經 220Ω） |
| b | Q1（經 220Ω） |
| c | Q2（經 220Ω） |
| d | Q3（經 220Ω） |
| e | Q4（經 220Ω） |
| f | Q5（經 220Ω） |
| g | Q6（經 220Ω） |
| dp | Q7（經 220Ω） |
| 共同腳（COM） | GND |

> 💡 **為什麼每一段都要電阻？** 每一段都是一顆小 LED，口訣「每條燈線一顆 220Ω」照樣適用。（有些教材只在共同腳串一顆電阻，那樣顯示「1」和「8」的亮度會不一樣。）

### 不用 74HC595 會怎樣？

| 需求 | 直接接 Arduino | 用 74HC595 |
| --- | --- | --- |
| 1 位數字 | 8 隻腳 | 3 隻腳 |
| 2 位數字 | 16 隻腳 | 3 隻腳（2 顆串接） |
| 8 顆燈＋2 位數字 | 24 隻腳（**超過 18 隻，不可能**） | 3 隻腳（3 顆串接） |

---

## 燈號圖案：用 1 和 0 畫圖

本週程式用 `0b` 開頭的 8 位數字表示 8 顆燈：**1 代表亮、0 代表滅**，**最右邊是 Q0**。

```
0b 0 0 0 0 0 1 1 1
   ↑             ↑
   Q7            Q0

→ Q0、Q1、Q2 亮，其他滅
```

| 圖案 | 意思 |
| --- | --- |
| `0b00000001` | 只有 Q0 亮 |
| `0b10000000` | 只有 Q7 亮 |
| `0b11111111` | 全亮 |
| `0b10101010` | 隔一顆亮一顆 |
| `0b00001111` | 右半邊亮 |

> 🎨 你不需要會二進位換算，只要把它當成「一排 8 個開關」，想亮哪顆就在那個位置寫 1。

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **74HC595 跨凹槽、缺口朝上；16 接 5V、8 接 GND** | |
| ② **MR（10）接 5V、OE（13）接 GND** | 這兩隻漏接，晶片不會動 |
| ③ **DS（14）資料、SHCP（11）時脈、STCP（12）鎖存** 接 Arduino | 3 隻一般腳即可，不用 `~` |
| ④ **Q0 在右邊 Pin 15**，Q1～Q7 在左邊 Pin 1～7 | |
| ⑤ **串接：Q7'（9）→ 下一顆 DS（14）；時脈、鎖存全部並接** | 最靠近 Arduino 的是第 1 顆 |
| ⑥ 七段顯示器：a～dp 依序接 Q0～Q7，每段一顆 220Ω，COM 接 GND | 屬性設成共陰極 |
| ⑦ 74HC595 不能調光、不能讀輸入 | 需要調光、讀感測器的元件仍接 Arduino |

---

## 共用工具

### 🔧 工具 1：74HC595 測試器

接好 74HC595 後先跑這支：依序只亮 Q0、Q1……Q7，Serial Monitor 同步顯示。串接多顆時，把 `chips` 改成顆數，會從第 1 顆的 Q0 一路測到最後一顆的 Q7。

```cpp
// ===== 改成你的腳位 =====
int dataPin  = 2;    // DS（Pin 14）
int clockPin = 3;    // SHCP（Pin 11）
int latchPin = 4;    // STCP（Pin 12）
int chips    = 1;    // 串接了幾顆 74HC595
// =======================

void sendOut(byte chip[], int n) {
  digitalWrite(latchPin, LOW);
  for (int i = n - 1; i >= 0; i--) shiftOut(dataPin, clockPin, MSBFIRST, chip[i]);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  byte chip[3];
  for (int c = 0; c < chips; c++) {
    for (int q = 0; q < 8; q++) {
      for (int i = 0; i < chips; i++) chip[i] = 0;
      chip[c] = 1 << q;
      sendOut(chip, chips);
      Serial.print("Chip ");
      Serial.print(c + 1);
      Serial.print("  Q");
      Serial.println(q);
      delay(400);
    }
  }
}
```

**檢查標準**：燈依 Q0 → Q7 的順序亮，和 Serial Monitor 一致。如果順序亂跳，檢查 Q0 是不是接到 Pin 15。

### 🔧 工具 2：七段顯示器測試器

接好七段顯示器後先跑這支：依序顯示 0～9，每個數字停 0.8 秒。

```cpp
int dataPin = 2, clockPin = 3, latchPin = 4;   // ← 改成你的腳位

byte font[10] = {
  0b00111111, 0b00000110, 0b01011011, 0b01001111, 0b01100110,
  0b01101101, 0b01111101, 0b00000111, 0b01111111, 0b01101111
};

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
}

void loop() {
  for (int n = 0; n < 10; n++) {
    digitalWrite(latchPin, LOW);
    shiftOut(dataPin, clockPin, MSBFIRST, font[n]);
    digitalWrite(latchPin, HIGH);
    delay(800);
  }
}
```

**檢查標準**：數字形狀正確。如果「1」看起來像別的形狀，表示 a～g 的接線順序錯了。

> 📌 **每個專案的固定流程**
> 1. **Step 0**：算出需要幾個輸出 → 決定幾顆 74HC595 → 填腳位規劃表
> 2. 先接 74HC595 和燈（或七段顯示器），用測試器確認
> 3. 照 Step 1 → 2 → 3 逐步接線，每完成一步就把對應的功能開關改成 `true`

---

## 專案一：商店招牌燈效

⭐ **難度**：1 顆 74HC595 ＋ 8 顆 LED，再加按鈕與旋鈕

**客戶需求**：手搖飲店的招牌要裝 8 顆燈做燈效。店員按按鈕可以切換四種燈效（跑馬燈、交錯閃、堆疊、全閃），旋鈕調整速度。老闆特別交代：Arduino 的腳要省著用，以後還要加其他功能。

### Step 0：腳位規劃

| 問題 | 你的答案 |
| --- | --- |
| 8 顆燈直接接 Arduino 要幾隻腳？用 74HC595 要幾隻？ | |
| 74HC595 的 MR、OE 各接哪裡？ | |

| 元件 | 腳位 |
| --- | --- |
| 74HC595 DS／SHCP／STCP | |
| 燈效切換鈕 | |
| 速度旋鈕 | A0 |
| **用了幾隻？還剩幾隻？** | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 74HC595＋8 顆 LED（各串 220Ω） | 跑馬燈效果 | （不用改） |
| Step 2 | ＋1 顆按鈕 | 按一下切換下一種燈效 | `useButton = true` |
| Step 3 | ＋1 顆旋鈕 | 旋鈕調整燈效速度 | `useKnob = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int dataPin  = ?;      // 74HC595 DS（Pin 14）
int clockPin = ?;      // 74HC595 SHCP（Pin 11）
int latchPin = ?;      // 74HC595 STCP（Pin 12）
int btnMode  = ?;      // 燈效切換鈕
int pinKnob  = A0;     // 速度旋鈕
bool useButton = false; // Step 2 接好後改成 true
bool useKnob   = false; // Step 3 接好後改成 true
// ============================

int effect = 0;
int step = 0;
int lastButton = LOW;

void sendOut(byte chip[], int n) {
  digitalWrite(latchPin, LOW);
  for (int i = n - 1; i >= 0; i--) shiftOut(dataPin, clockPin, MSBFIRST, chip[i]);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(btnMode, INPUT);
  Serial.begin(9600);
}

void loop() {
  if (useButton) {
    int now = digitalRead(btnMode);
    if (now == HIGH && lastButton == LOW) {
      delay(50);
      effect = (effect + 1) % 4;
      step = 0;
      Serial.print("Effect: ");
      Serial.println(effect);
    }
    lastButton = now;
  }

  byte pattern = 0;
  if (effect == 0)      pattern = 1 << (step % 8);                        // 跑馬燈
  else if (effect == 1) pattern = (step % 2) ? 0b10101010 : 0b01010101;   // 交錯閃
  else if (effect == 2) pattern = (1 << (step % 9)) - 1;                  // 堆疊
  else                  pattern = (step % 2) ? 0b11111111 : 0b00000000;   // 全閃

  byte chip[1] = { pattern };
  sendOut(chip, 1);

  int speed = 200;
  if (useKnob) speed = map(analogRead(pinKnob), 0, 1023, 50, 500);
  step++;
  delay(speed);
}
```

### 情境思考

1. 本專案一共用了幾隻 Arduino 腳？如果沒有 74HC595，同樣的功能要幾隻？
2. 想讓 8 顆燈「由左往右」跑，卻是「由右往左」。**不改程式**要怎麼修？
3. 💼 招牌燈效太花俏會不會反而讓顧客不舒服？你會建議老闆在什麼時段用什麼燈效？

### ✅ 專案一驗收

- [ ] 74HC595 測試器確認 Q0～Q7 依序亮起
- [ ] 跑馬燈效果正確
- [ ] 按鈕能切換四種燈效
- [ ] 旋鈕能調整速度
- [ ] 8 顆 LED 各有 220Ω，MR 接 5V、OE 接 GND

---

## 專案二：行人號誌倒數器

⭐⭐ **難度**：74HC595 ＋ 七段顯示器 ＋ 紅綠燈 ＋ 按鈕 ＋ 蜂鳴器

**客戶需求**：市政府要在學校前的路口裝「按鈕式行人號誌」。平常行人紅燈；行人按下按鈕，2 秒後轉綠燈，七段顯示器從 9 倒數到 0，**最後 3 秒綠燈閃爍**提醒加快腳步，倒數結束回到紅燈。為了照顧視障者，綠燈期間蜂鳴器每秒「嗶」一聲，最後 3 秒改為急促的嗶聲（**無障礙號誌**）。

### Step 0：腳位規劃

| 元件 | 腳位 |
| --- | --- |
| 74HC595 DS／SHCP／STCP | |
| 行人紅燈、綠燈 | |
| 行人按鈕 | |
| 蜂鳴器 | |

> 🤔 紅燈、綠燈要接 Arduino，還是接 74HC595 的 Q7（七段的小數點用不到時）？兩種都可以，請說明你的選擇。本專案範例程式把紅綠燈接 Arduino。

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到／聽到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 74HC595＋七段顯示器（a～dp 各串 220Ω） | 七段顯示器 9 → 0 循環倒數展示 | （不用改） |
| Step 2 | ＋紅燈＋綠燈＋按鈕 | 平時紅燈；按下按鈕 2 秒後轉綠倒數，最後 3 秒綠燈閃 | `useButton = true` |
| Step 3 | ＋蜂鳴器 | 綠燈每秒嗶一聲，最後 3 秒急促嗶 | `useSound = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int dataPin  = ?;      // 74HC595 DS
int clockPin = ?;      // 74HC595 SHCP
int latchPin = ?;      // 74HC595 STCP
int ledRed   = ?;      // 行人紅燈
int ledGreen = ?;      // 行人綠燈
int btnWalk  = ?;      // 行人按鈕
int pinBuzzer = ?;     // 蜂鳴器
int walkTime = 9;      // 綠燈秒數（最多 9）
bool useButton = false; // Step 2 接好後改成 true
bool useSound  = false; // Step 3 接好後改成 true
// ============================

byte font[10] = {
  0b00111111, 0b00000110, 0b01011011, 0b01001111, 0b01100110,
  0b01101101, 0b01111101, 0b00000111, 0b01111111, 0b01101111
};

void sendOut(byte chip[], int n) {
  digitalWrite(latchPin, LOW);
  for (int i = n - 1; i >= 0; i--) shiftOut(dataPin, clockPin, MSBFIRST, chip[i]);
  digitalWrite(latchPin, HIGH);
}

void showDigit(int n) {               // n = -1 代表熄滅
  byte chip[1] = { (n < 0) ? (byte)0 : font[n] };
  sendOut(chip, 1);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(ledRed, OUTPUT);
  pinMode(ledGreen, OUTPUT);
  pinMode(btnWalk, INPUT);
}

void loop() {
  if (!useButton) {                   // Step 1：倒數展示
    for (int s = 9; s >= 0; s--) { showDigit(s); delay(800); }
    return;
  }

  digitalWrite(ledRed, HIGH);
  digitalWrite(ledGreen, LOW);
  showDigit(-1);

  if (digitalRead(btnWalk) == HIGH) {
    delay(2000);                      // 等車道轉紅
    digitalWrite(ledRed, LOW);
    for (int s = walkTime; s >= 0; s--) {
      showDigit(s);
      if (s > 3) {                    // 一般綠燈
        digitalWrite(ledGreen, HIGH);
        if (useSound) tone(pinBuzzer, 1000, 100);
        delay(1000);
      } else {                        // 最後 3 秒：綠燈閃、急促嗶
        for (int k = 0; k < 4; k++) {
          digitalWrite(ledGreen, k % 2 == 0 ? HIGH : LOW);
          if (useSound) tone(pinBuzzer, 1500, 60);
          delay(250);
        }
      }
    }
    digitalWrite(ledGreen, LOW);
  }
}
```

### 情境思考

1. 七段顯示器的小數點（dp）這個專案沒用到，接在 Q7 上的那條線可以省略嗎？省略的話，Q7 可以拿來接什麼？
2. 為什麼按下按鈕後要「等 2 秒」才轉綠燈，而不是立刻轉？
3. 💼 學校前的路口，上下學時段行人多、其他時段行人少。號誌的綠燈秒數應該固定嗎？你會建議交通局怎麼調整？

### ✅ 專案二驗收

- [ ] 七段顯示器能正確顯示 9～0
- [ ] 平時紅燈；按下按鈕 2 秒後轉綠並倒數
- [ ] 最後 3 秒綠燈閃爍
- [ ] 綠燈期間每秒嗶一聲，最後 3 秒急促嗶
- [ ] 七段顯示器每段都有 220Ω，屬性為共陰極

---

## 專案三：兩位數產量計數器

⭐⭐⭐ **難度**：**2 顆 74HC595 串接** ＋ 2 顆七段顯示器 ＋ 超音波 ＋ 旋鈕 ＋ 按鈕 ＋ 蜂鳴器

**客戶需求**：食品工廠包裝線末端，要一個大大的兩位數計數器（00～99）。產品經過超音波時自動加 1。班長用旋鈕設定本班目標產量，**達標時蜂鳴器播放提示，個位數的小數點開始閃爍**。交班時按「歸零鈕」重新計數。

### Step 0：腳位規劃（本專案的重頭戲）

| 問題 | 你的答案 |
| --- | --- |
| 兩位數字需要幾個段位輸出？直接接 Arduino 要幾隻腳？ | |
| 2 顆 74HC595 怎麼串接？第 1 顆的哪隻腳接到第 2 顆的哪隻腳？ | |
| 哪一顆接十位數、哪一顆接個位數？ | 本專案：第 1 顆接十位、第 2 顆接個位 |

| 元件 | 腳位 |
| --- | --- |
| 74HC595 DS（只接第 1 顆） | |
| SHCP、STCP（兩顆並接） | |
| HC-SR04 Trig／Echo | |
| 目標旋鈕 | A0 |
| 歸零鈕 | |
| 蜂鳴器 | |
| **一共用了幾隻？** | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 2 顆 74HC595 串接＋2 顆七段＋HC-SR04 | 產品每經過一次，計數 00 → 01 → … → 99 | （不用改） |
| Step 2 | ＋旋鈕＋蜂鳴器 | 達到目標時播放提示，個位小數點閃爍 | `useTarget = true` |
| Step 3 | ＋歸零鈕 | 按下歸零，Serial Monitor 印出本班產量 | `useReset = true` |

### 程式（只改設定區）

```cpp
// ===== 設定區：只改這裡 =====
int dataPin  = ?;                   // 第 1 顆 74HC595 的 DS
int clockPin = ?;                   // 兩顆的 SHCP（並接）
int latchPin = ?;                   // 兩顆的 STCP（並接）
int pinTrig = ?, pinEcho = ?;       // HC-SR04
int pinKnob = A0;                   // 目標旋鈕
int btnReset = ?;                   // 歸零鈕
int pinBuzzer = ?;                  // 蜂鳴器
float passCM = 10;                  // 小於這個距離算「產品經過」
bool useTarget = false;             // Step 2 接好後改成 true
bool useReset  = false;             // Step 3 接好後改成 true
// ============================

byte font[10] = {
  0b00111111, 0b00000110, 0b01011011, 0b01001111, 0b01100110,
  0b01101101, 0b01111101, 0b00000111, 0b01111111, 0b01101111
};
int count = 0;
bool wasPassing = false;
bool reached = false;
int lastReset = LOW;
int tick = 0;

void sendOut(byte chip[], int n) {
  digitalWrite(latchPin, LOW);
  for (int i = n - 1; i >= 0; i--) shiftOut(dataPin, clockPin, MSBFIRST, chip[i]);
  digitalWrite(latchPin, HIGH);
}

// 第 1 顆＝十位、第 2 顆＝個位；dot=true 時個位小數點亮
void showNumber(int n, bool dot) {
  byte chip[2];
  chip[0] = font[(n / 10) % 10];
  chip[1] = font[n % 10] | (dot ? 0b10000000 : 0);
  sendOut(chip, 2);
}

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(btnReset, INPUT);
  Serial.begin(9600);
}

void loop() {
  tick++;
  bool passing = readCM(pinTrig, pinEcho) < passCM;
  if (passing && !wasPassing && count < 99) count++;
  wasPassing = passing;

  int target = map(analogRead(pinKnob), 0, 1023, 5, 99);
  if (useTarget && count >= target && !reached) {
    reached = true;
    Serial.println("TARGET REACHED");
    tone(pinBuzzer, 1047, 200); delay(250);
    tone(pinBuzzer, 1319, 400);
  }
  if (count < target) reached = false;

  if (useReset) {
    int now = digitalRead(btnReset);
    if (now == HIGH && lastReset == LOW) {
      delay(50);
      Serial.print("Shift output: ");
      Serial.println(count);
      count = 0;
      reached = false;
    }
    lastReset = now;
  }

  bool dot = useTarget && reached && (tick / 5) % 2;
  showNumber(count, dot);
  delay(50);
}
```

### 情境思考

1. 用你的腳位規劃表說明：本專案一共用了幾隻 Arduino 腳？如果兩顆七段顯示器直接接 Arduino，還放得下 HC-SR04 和其他元件嗎？
2. 接好後發現十位和個位顯示相反（例如計數 12 卻顯示 21）。**不改程式**要怎麼修？
3. 💼 「達標」之後，產線應該停下來還是繼續生產？超產和缺產各有什麼成本？

### ✅ 專案三驗收

- [ ] 2 顆 74HC595 正確串接（Q7' → 第 2 顆 DS；時脈、鎖存並接）
- [ ] 每經過一個產品計數只加 1，能從 00 數到 99
- [ ] 達到旋鈕設定的目標時播放提示、個位小數點閃爍
- [ ] 歸零鈕能將計數歸零並印出本班產量
- [ ] 腳位規劃表說明串接後仍只用 3 隻腳

---

## 專案四：產線安燈總看板

⭐⭐⭐⭐ **難度**：**3 顆 74HC595 串接**（8 顆站點燈＋2 位數字）＋ 鍵盤 ＋ 蜂鳴器 ＋ I2C LCD

**客戶需求**：電子工廠有 8 個工作站，現場要一塊**安燈（Andon）總看板**：

- 8 顆紅燈代表 8 個工作站：某站出問題時，作業員到看板旁的鍵盤輸入**站號（1～8）＋ `#`**，該站紅燈亮起；問題排除後再輸入一次，紅燈熄滅
- 兩位數七段顯示器顯示**今天累計的異常次數**
- 只要有任何一站異常，蜂鳴器每秒提醒一次
- I2C LCD 顯示**目前停最久的是哪一站、已經停了幾秒**（讓主管知道優先處理誰）

> 🏭 **安燈（Andon）** 是豐田生產系統的核心工具：任何人發現問題都能「拉繩停線」，讓問題立刻被看見、被處理。第 3 週我們做過單站的安燈，這週升級成全廠的總看板。

### Step 0：腳位規劃（本專案的重頭戲）

**先算一次不用 74HC595：**

| 元件 | 需要幾隻腳 |
| --- | --- |
| 8 顆站點燈 | 8 |
| 2 位七段顯示器 | 16 |
| 鍵盤（3 行） | 7 |
| 蜂鳴器 | 1 |
| I2C LCD | 2 |
| **合計** | |
| **Arduino 可用** | 18 |

**再算一次用 3 顆 74HC595 串接：**

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 74HC595 × 3（串接） | 3 | |
| 鍵盤（3 行） | 7 | |
| 蜂鳴器 | 1 | |
| I2C LCD | 2 | A4、A5 |
| **合計** | | |
| **還剩幾隻？** | | |

| 74HC595 | 接什麼 |
| --- | --- |
| 第 1 顆（最靠近 Arduino） | 8 顆站點燈（Q0＝第 1 站 … Q7＝第 8 站） |
| 第 2 顆 | 十位數七段顯示器 |
| 第 3 顆 | 個位數七段顯示器 |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 第 1 顆 74HC595＋8 顆紅燈＋鍵盤（3 行） | 輸入站號＋`#`，該站燈亮；再輸入一次熄滅 | （不用改） |
| Step 2 | ＋第 2、3 顆 74HC595（串接）＋2 顆七段＋蜂鳴器 | 顯示累計異常次數；有異常時每秒嗶 | `useCount = true` |
| Step 3 | ＋I2C LCD | 顯示停最久的站與秒數、目前異常站數 | `useLCD = true` |

> 💡 Step 1 只接 1 顆，程式也只送 1 顆的資料；Step 2 加上兩顆後，把 `useCount` 打開，程式就會送 3 顆的資料。

### 程式（只改設定區）

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
int dataPin  = ?;                   // 第 1 顆 74HC595 的 DS
int clockPin = ?;                   // 三顆的 SHCP（並接）
int latchPin = ?;                   // 三顆的 STCP（並接）
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C3（C4 不接，隨便填一隻空腳）
int pinBuzzer = ?;                  // 蜂鳴器
bool useCount = false;              // Step 2 接好後改成 true
bool useLCD   = false;              // Step 3 接好後改成 true
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

byte font[10] = {
  0b00111111, 0b00000110, 0b01011011, 0b01001111, 0b01100110,
  0b01101101, 0b01111101, 0b00000111, 0b01111111, 0b01101111
};
bool down[8];
unsigned long downSince[8];
int events = 0;
String input = "";
unsigned long lastBeep = 0;

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

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  for (int s = 0; s < 8; s++) down[s] = false;
  if (useLCD) {
    lcd.begin(16, 2);
    lcd.setBacklight(1);
  }
  Serial.begin(9600);
}

void loop() {
  // ---- 鍵盤：站號 + # 切換異常／排除 ----
  char k = keypad.getKey();
  if (k >= '1' && k <= '8') {
    input = String(k);
  } else if (k == '*') {
    input = "";
  } else if (k == '#' && input.length() == 1) {
    int s = input.toInt() - 1;
    down[s] = !down[s];
    if (down[s]) {
      events++;
      downSince[s] = millis();
      Serial.print("STATION ");
      Serial.print(s + 1);
      Serial.println(" DOWN");
    } else {
      Serial.print("STATION ");
      Serial.print(s + 1);
      Serial.print(" RECOVERED after ");
      Serial.print((millis() - downSince[s]) / 1000);
      Serial.println(" s");
    }
    input = "";
  }

  // ---- 站點燈圖案：第 s 站異常 → 第 s 位為 1 ----
  byte stations = 0;
  int downCount = 0;
  int oldest = -1;
  for (int s = 0; s < 8; s++) {
    if (down[s]) {
      stations |= (1 << s);
      downCount++;
      if (oldest < 0 || downSince[s] < downSince[oldest]) oldest = s;
    }
  }

  // ---- 送出到 74HC595 ----
  if (useCount) {
    byte chip[3] = { stations, font[(events / 10) % 10], font[events % 10] };
    sendOut(chip, 3);
    if (downCount > 0 && millis() - lastBeep > 1000) {
      tone(pinBuzzer, 1500, 100);
      lastBeep = millis();
    }
  } else {
    byte chip[1] = { stations };
    sendOut(chip, 1);
  }

  // ---- LCD：停最久的站 ----
  if (useLCD) {
    if (oldest >= 0) {
      showLine(0, "ST" + String(oldest + 1) + " DOWN " + String((millis() - downSince[oldest]) / 1000) + "s");
    } else {
      showLine(0, "ALL RUNNING");
    }
    showLine(1, "DOWN NOW: " + String(downCount) + (input.length() ? "  >" + input : ""));
  }
  delay(30);
}
```

### 情境思考

1. 用你的兩張腳位計算表說明：沒有 74HC595 時這個看板做得出來嗎？用了之後還剩幾隻腳？
2. LCD 顯示「停最久的站」而不是「最新停的站」。為什麼主管更需要知道「停最久」的？
3. 💼 每次站點恢復時，Serial Monitor 印出「停了幾秒」。如果把一個月的紀錄整理起來，生產主管可以做哪些分析？（提示：哪一站最常停、平均修復時間 MTTR）

### ✅ 專案四驗收

- [ ] 3 顆 74HC595 正確串接，用 74HC595 測試器（`chips = 3`）確認 24 個輸出依序亮
- [ ] 輸入站號＋`#`，對應的站點燈亮；再輸入一次熄滅
- [ ] 兩位數七段顯示器顯示累計異常次數
- [ ] 有任何一站異常時，蜂鳴器每秒提醒一次
- [ ] LCD 顯示停最久的站與秒數、目前異常站數
- [ ] 腳位規劃表比較「有／沒有 74HC595」兩種方案

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：四個範例專案是「跟著做」，這一區換你「自己規劃」。每一關先**算出需要幾個輸出**，決定用幾顆 74HC595，再規劃腳位、動手接線。
>
> 📈 **難度安排**：
>
> | 關卡 | 74HC595 | 顯示 | 其他元件 | 配線重點 |
> | --- | --- | --- | --- | --- |
> | Lv.1 | 1 | 1 位數字 | 2 按鈕 | 七段顯示器基本接法 |
> | Lv.2 | 1 | 8 顆燈 | 旋鈕 | 燈條圖案 |
> | Lv.3 | 1 | 1 位數字 | 按鈕＋蜂鳴器 | 用 Q7 小數點當指示燈 |
> | Lv.4 | 2 | 2 位數字 | TMP36＋RGB | 串接＋RGB 調光 |
> | Lv.5 | 3 | 8 顆燈＋2 位數字 | HC-SR04＋旋鈕＋按鈕＋蜂鳴器 | 3 顆串接 |
> | Lv.6 | 自訂 | 自訂 | 自訂 | 顧問提案 |
>
> 📌 **作答規則**：每一關先填腳位規劃表（含「有／沒有 74HC595」的腳位比較），再接線；接好後先用測試器確認，最後才上傳程式。程式只需要改「設定區」。

---

### 🟢 Lv.1 電梯樓層顯示

**情境**：三層樓以上的小型電梯（1～9 樓），車廂內有一顆七段顯示器。按「上樓鈕」電梯每 1 秒上一層，按「下樓鈕」每 1 秒下一層，一次移動一層，顯示器同步更新。到頂樓或一樓後按鈕無效。

**元件**：1 顆 74HC595＋1 顆七段顯示器＋2 顆按鈕

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int dataPin = ?, clockPin = ?, latchPin = ?;   // 74HC595
int btnUp = ?, btnDown = ?;                    // 上樓鈕、下樓鈕
int topFloor = 9;                              // 最高樓層
// ============================

byte font[10] = {
  0b00111111, 0b00000110, 0b01011011, 0b01001111, 0b01100110,
  0b01101101, 0b01111101, 0b00000111, 0b01111111, 0b01101111
};
int floorNow = 1;

void showDigit(int n) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, font[n]);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(btnUp, INPUT);
  pinMode(btnDown, INPUT);
}

void loop() {
  showDigit(floorNow);
  if (digitalRead(btnUp) == HIGH && floorNow < topFloor) {
    delay(1000);
    floorNow++;
  }
  if (digitalRead(btnDown) == HIGH && floorNow > 1) {
    delay(1000);
    floorNow--;
  }
}
```

**情境思考**：

1. 本關只需要 a～g 七段，小數點沒用到。Q7 那條線可以不接嗎？
2. 💼 很多大樓沒有「4 樓」或「13 樓」。如果客戶要求顯示時跳過 4 樓，需要改接線還是改程式？

---

### 🟢 Lv.2 音響音量燈條

**情境**：音響面板上 8 顆燈排成一條，用旋鈕調音量時，燈條即時顯示音量大小（0～8 格）。第 4 週我們用 Arduino 直接接 5 顆燈做過類似的長條燈，這次改用 74HC595。

**元件**：1 顆 74HC595＋8 顆 LED＋1 顆旋鈕

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int dataPin = ?, clockPin = ?, latchPin = ?;   // 74HC595
int pinKnob = A0;                              // 音量旋鈕
// ============================

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
}

void loop() {
  int lit = map(analogRead(pinKnob), 0, 1024, 0, 9);   // 0~8 格
  byte pattern = (1 << lit) - 1;                       // 亮 lit 格
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, pattern);
  digitalWrite(latchPin, HIGH);
  delay(30);
}
```

**情境思考**：

1. 和第 4 週的 5 格長條燈相比，這次燈變多了，Arduino 的腳反而用得更少。各用了幾隻？
2. 真實音響的音量燈條，最上面兩格通常是紅色、其他是綠色。不改程式，你要怎麼做到？

---

### 🟢 Lv.3 週年慶抽獎機

**情境**：百貨公司週年慶的抽獎機。顧客按下按鈕，七段顯示器快速滾動數字，越滾越慢，最後停在 0～9 其中一個數字。抽到 **7** 是大獎：播放慶祝音樂，**小數點閃爍**；其他數字是普獎，嗶一聲。

**元件**：1 顆 74HC595＋1 顆七段顯示器＋1 顆按鈕＋1 顆蜂鳴器

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int dataPin = ?, clockPin = ?, latchPin = ?;   // 74HC595
int btnDraw = ?;                               // 抽獎鈕
int pinBuzzer = ?;                             // 蜂鳴器
int bigPrize = 7;                              // 大獎號碼
// ============================

byte font[10] = {
  0b00111111, 0b00000110, 0b01011011, 0b01001111, 0b01100110,
  0b01101101, 0b01111101, 0b00000111, 0b01111111, 0b01101111
};
int lastButton = LOW;
int result = 0;

void show(byte pattern) {
  digitalWrite(latchPin, LOW);
  shiftOut(dataPin, clockPin, MSBFIRST, pattern);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(btnDraw, INPUT);
  randomSeed(analogRead(A5));
  show(font[0]);
}

void loop() {
  int now = digitalRead(btnDraw);
  if (now == HIGH && lastButton == LOW) {
    delay(50);
    for (int i = 0; i < 25; i++) {               // 越滾越慢
      result = random(0, 10);
      show(font[result]);
      tone(pinBuzzer, 800, 20);
      delay(30 + i * 12);
    }
    if (result == bigPrize) {
      int notes[] = { 523, 659, 784, 1047 };
      for (int k = 0; k < 8; k++) {
        show(font[result] | ((k % 2) ? 0b10000000 : 0));   // 小數點閃爍
        tone(pinBuzzer, notes[k % 4], 150);
        delay(200);
      }
    } else {
      tone(pinBuzzer, 600, 300);
    }
    show(font[result]);
  }
  lastButton = now;
}
```

**情境思考**：

1. 小數點本來是顯示數字用的，這裡拿來當「中大獎指示燈」。還有哪些情境可以這樣「一物多用」？
2. 💼 抽中大獎的機率是 1/10。如果百貨公司預算只夠送出 5 個大獎，但預計有 200 人來抽，應該怎麼調整？這是技術問題還是管理問題？

---

### 🟡 Lv.4 機房兩位數溫度顯示

**情境**：機房門口要一個大大的兩位數溫度顯示器（00～99°C），讓巡檢人員不用進門就看得到。旁邊一顆 RGB 燈用**漸層**顯示溫度：越冷越藍、越熱越紅。

**元件**：2 顆 74HC595（串接）＋2 顆七段顯示器＋TMP36＋RGB

**你要想的問題**：

- RGB 要漸層變色，需要調光。它可以接在 74HC595 上嗎？為什麼？
- 本關沒有伺服、蜂鳴器，RGB 可以用哪些 `~` 腳？

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int dataPin = ?, clockPin = ?, latchPin = ?;   // 74HC595（2 顆串接）
int pinTemp = A0;                              // TMP36
int pinR = ?, pinG = ?, pinB = ?;              // 漸層 RGB（~ 腳）
int coldTemp = 18, hotTemp = 35;               // 藍色與紅色對應的溫度
// ============================

byte font[10] = {
  0b00111111, 0b00000110, 0b01011011, 0b01001111, 0b01100110,
  0b01101101, 0b01111101, 0b00000111, 0b01111111, 0b01101111
};

float readC(int pin) {
  float voltage = analogRead(pin) * 5.0 / 1023.0;
  return (voltage - 0.5) * 100.0;
}

void sendOut(byte chip[], int n) {
  digitalWrite(latchPin, LOW);
  for (int i = n - 1; i >= 0; i--) shiftOut(dataPin, clockPin, MSBFIRST, chip[i]);
  digitalWrite(latchPin, HIGH);
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  int t = constrain((int)readC(pinTemp), 0, 99);
  byte chip[2] = { font[t / 10], font[t % 10] };   // 第 1 顆十位、第 2 顆個位
  sendOut(chip, 2);

  int red = map(constrain(t, coldTemp, hotTemp), coldTemp, hotTemp, 0, 255);
  analogWrite(pinR, red);
  analogWrite(pinG, 0);
  analogWrite(pinB, 255 - red);
  delay(300);
}
```

**情境思考**：

1. 兩位數顯示器看不到小數（例如 24.7°C 顯示成 24）。對機房巡檢來說夠用嗎？如果要顯示一位小數，需要幾顆七段顯示器？小數點要亮哪一顆的？
2. 💼 巡檢人員每天要記錄溫度。這個顯示器讓他不用進門，但還是要「人去看」。下一步可以怎麼省掉人工記錄？

---

### 🔴 Lv.5 產線節拍時間看板

**情境**：汽車零件組裝線，每一件產品都要在**節拍時間（Takt Time）**內完成。產線旁的看板：

- **兩位數七段顯示器**：倒數這一件剩下幾秒；產品完成（超音波偵測到產品送出）就重新開始倒數
- 倒數到 0 還沒完成 → 蜂鳴器提醒、小數點閃爍，並記錄一次「延遲」
- **8 顆燈**：本班進度，每完成目標的 1/8 多亮一格
- 旋鈕設定節拍時間（10～60 秒），按鈕交班歸零，Serial Monitor 印出完成數與延遲次數

> 🏭 **節拍時間（Takt Time）** ＝ 可用生產時間 ÷ 顧客需求數量。它是精實生產的「心跳」：每隔多久就必須完成一件，才能剛好滿足需求。

**元件**：3 顆 74HC595（串接）＋8 顆 LED＋2 顆七段顯示器＋HC-SR04＋旋鈕＋按鈕＋蜂鳴器

**Step 0：腳位規劃（本關重點）**

| 74HC595 | 接什麼 |
| --- | --- |
| 第 1 顆 | 8 顆進度燈 |
| 第 2 顆 | 十位數七段 |
| 第 3 顆 | 個位數七段 |

| 元件 | 不用 74HC595 | 用 74HC595 |
| --- | --- | --- |
| 8 顆燈＋2 位數字 | 24 | 3 |
| HC-SR04 | 2 | 2 |
| 旋鈕 | 1（A 腳） | 1（A 腳） |
| 按鈕、蜂鳴器 | 2 | 2 |
| **合計** | | |

**程式**：

```cpp
// ===== 設定區：只改這裡 =====
int dataPin = ?, clockPin = ?, latchPin = ?;   // 74HC595（3 顆串接）
int pinTrig = ?, pinEcho = ?;                  // HC-SR04（產品送出偵測）
int pinKnob = A0;                              // 節拍時間旋鈕
int btnReset = ?;                              // 交班歸零鈕
int pinBuzzer = ?;                             // 蜂鳴器
int shiftTarget = 40;                          // 本班目標件數
float doneCM = 10;                             // 小於這個距離算「產品送出」
// ============================

byte font[10] = {
  0b00111111, 0b00000110, 0b01011011, 0b01001111, 0b01100110,
  0b01101101, 0b01111101, 0b00000111, 0b01111111, 0b01101111
};
int done = 0, late = 0;
unsigned long cycleStart = 0;
bool wasPassing = false, lateMarked = false;
int lastReset = LOW;

void sendOut(byte chip[], int n) {
  digitalWrite(latchPin, LOW);
  for (int i = n - 1; i >= 0; i--) shiftOut(dataPin, clockPin, MSBFIRST, chip[i]);
  digitalWrite(latchPin, HIGH);
}

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void setup() {
  pinMode(dataPin, OUTPUT);
  pinMode(clockPin, OUTPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(btnReset, INPUT);
  Serial.begin(9600);
  cycleStart = millis();
}

void loop() {
  int takt = map(analogRead(pinKnob), 0, 1023, 10, 60);
  int elapsed = (millis() - cycleStart) / 1000;
  int remain = max(takt - elapsed, 0);

  // ---- 產品完成：重新開始倒數 ----
  bool passing = readCM(pinTrig, pinEcho) < doneCM;
  if (passing && !wasPassing) {
    done++;
    Serial.print("Unit ");
    Serial.print(done);
    Serial.print(" done in ");
    Serial.print(elapsed);
    Serial.println(" s");
    cycleStart = millis();
    lateMarked = false;
    remain = takt;
  }
  wasPassing = passing;

  // ---- 超過節拍時間 ----
  if (remain == 0 && !lateMarked) {
    late++;
    lateMarked = true;
    tone(pinBuzzer, 500, 600);
    Serial.println("LATE!");
  }

  // ---- 交班歸零 ----
  int now = digitalRead(btnReset);
  if (now == HIGH && lastReset == LOW) {
    delay(50);
    Serial.print("SHIFT END  Done: ");
    Serial.print(done);
    Serial.print("  Late: ");
    Serial.println(late);
    done = 0;
    late = 0;
    cycleStart = millis();
    lateMarked = false;
  }
  lastReset = now;

  // ---- 看板 ----
  int lit = min(done * 8 / shiftTarget, 8);
  byte progress = (1 << lit) - 1;
  bool blink = (remain == 0) && ((millis() / 300) % 2);
  byte chip[3] = {
    progress,
    font[(remain / 10) % 10],
    (byte)(font[remain % 10] | (blink ? 0b10000000 : 0))
  };
  sendOut(chip, 3);
  delay(50);
}
```

**情境思考**：

1. 附上你的腳位規劃表：用 3 顆 74HC595 後一共用了幾隻腳？還剩幾隻？
2. 如果節拍時間是 30 秒，但作業員平均要 35 秒才完成一件，會發生什麼事？要解決這個問題，可以從哪些方向下手？（提示：增加人力、改善工法、調整需求）
3. 💼 「延遲次數」是要拿來「懲罰作業員」還是「找出改善機會」？兩種用法會對現場文化造成什麼不同影響？

---

### 🏆 Lv.6 BOSS 關：物聯網顧問提案

**情境**：你的顧問公司接到新客戶。從下面三個案例中**任選一個**，自己完成需求分析、元件選擇、腳位規劃與 Tinkercad 實作。

> **案例 A：餐廳取餐號碼牌**
> 廚房按按鈕叫下一號，兩位數七段顯示器顯示目前取餐號碼，蜂鳴器提示；8 顆燈顯示目前排隊人數（每顆代表 1 人）；顧客取號用另一顆按鈕。
>
> **案例 B：停車場 16 格車位燈**
> 2 顆 74HC595 串接控制 16 顆車位燈（紅＝有車），管理員用鍵盤輸入車位號碼切換狀態；I2C LCD 顯示剩餘車位與最久停放的車位。
>
> **案例 C：自訂情境**
> 自己找一個需要「很多燈」或「數字顯示」的場域（例如：球場計分板、會議室預約燈牆、藥櫃取藥指示、考場座位燈）。

**最低規格**：

- 至少 2 顆 74HC595 串接
- 至少 1 種輸入元件
- 總共控制 16 個以上的輸出（燈或七段段位）
- 程式可以從專案一～四或 Lv.1～Lv.5 改寫，說明你是從哪裡改來的、改了哪些設定

**繳交內容**：

1. **需求分析**：這個系統解決什麼問題？誰會看這塊看板？（3～5 句）
2. **輸出數量計算**：需要幾個輸出？幾顆 74HC595？每顆接什麼？
3. **腳位規劃表**：「有／沒有 74HC595」兩種方案比較
4. **Tinkercad 實作**：電路截圖＋74HC595 測試器截圖（`chips` 設成你的顆數）
5. **擴充建議**：如果客戶要再加一倍的燈，你的設計要怎麼改？Arduino 腳位會增加嗎？

**驗收標準**：

- [ ] 74HC595 的 MR 接 5V、OE 接 GND，串接正確
- [ ] 每個燈、每個七段段位都有 220Ω
- [ ] 74HC595 測試器確認所有輸出依序亮
- [ ] 需要調光的元件、輸入元件仍接 Arduino
- [ ] 麵包板依功能分區，晶片跨在凹槽上

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 所有燈都不亮 | 16 接 5V、8 接 GND；**MR（10）接 5V、OE（13）接 GND** | 逐腳核對接線地圖 |
| 燈全亮或亂亮，不受控制 | DS、SHCP、STCP 三條線是否接對；程式腳位是否一致 | 用 74HC595 測試器確認 |
| 順序亂跳（Q0 不是第一個亮） | Q0 是否接到 Pin 15（右排） | Q0 在右邊，Q1～Q7 在左邊 |
| 七段顯示的數字形狀怪怪的 | a～g 是否依序接 Q0～Q6 | 用七段測試器，對照段位圖調整 |
| 七段完全不亮 | 共同腳是否接 GND；屬性是否為共陰極 | 改成 Cathode |
| 串接後第 2 顆不動 | 第 1 顆 Q7'（Pin 9）是否接第 2 顆 DS（Pin 14）；時脈、鎖存是否並接 | 檢查三條串接線 |
| 兩位數顯示相反（12 變 21） | 十位、個位接在哪一顆 | 對調兩顆七段，或交換兩顆 74HC595 的輸出線 |
| 接在 74HC595 上的燈不能調光 | 74HC595 沒有調光功能 | 需要調光的燈改接 Arduino 的 `~` 腳 |
| 程式無法上傳 | 設定區是否還有 `?` 沒填 | 填完所有腳位 |

---

## 成果繳交與評分

### 範例專案繳交

每個專案各自建立一個 Tinkercad 檔案，命名格式：`W13-專案編號-學號`（例如 `W13-P3-B12345678`）。

| 繳交項目 | 專案一 | 專案二 | 專案三 | 專案四 |
| --- | --- | --- | --- | --- |
| Tinkercad 分享連結 | ✅ | ✅ | ✅ | ✅ |
| 輸出數量計算與 74HC595 顆數 | ✅ | ✅ | ✅ | ✅ |
| 腳位規劃表（有／沒有 74HC595 比較） | ✅ | ✅ | ✅ | ✅ |
| 完成 Step 3 的電路截圖 | ✅ | ✅ | ✅ | ✅ |
| 情境思考題作答 | ✅ | ✅ | ✅ | ✅ |

> 🎯 **最低完成標準**：專案一、二完成 Step 3，專案三、四至少完成 Step 2。

### 練習題繳交

| 關卡 | 繳交內容 | 建議定位 |
| --- | --- | --- |
| Lv.1～Lv.3 | Tinkercad 連結＋情境思考題 | 課堂暖身，全班必做 |
| Lv.4 | ＋腳位規劃表 | 課後作業，必做 |
| Lv.5 | ＋腳位規劃表＋麵包板擺放截圖 | 選做加分 |
| Lv.6 BOSS | 完整顧問提案 | 可分組，作為期末專題暖身 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 電路正確性 | 35% | 74HC595 接線與串接正確，每個 Step 功能正常 |
| 腳位規劃 | 25% | 正確計算輸出數量與 74HC595 顆數，能比較兩種方案 |
| 情境思考題 | 25% | 尤其是節拍時間、安燈等 💼 管理面向論述 |
| 麵包板擺放 | 15% | 晶片位置正確、電阻排列整齊、分區清楚 |

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 40 分鐘 | 認識 74HC595（電梯比喻）、接線地圖、燈號圖案；全班一起接 1 顆＋8 顆燈並跑測試器 |
| 第 1 節後段 | 專案一 |
| 第 2 節 | 七段顯示器、專案二、專案三（串接是重點） |
| 第 3 節前段 | 專案四 |
| 第 3 節後段 | 練習題 Lv.1～Lv.3 當堂完成，其餘帶回家 |

> 💡 **教學建議**：74HC595 最常卡在「Q0 在右邊」和「MR、OE 沒接」。建議第 1 節全班一起接到 74HC595 測試器能讓 8 顆燈依序亮，再開始專案。

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 位移暫存器 74HC595 | 用 3 條線控制 8 個輸出的晶片 |
| 資料（DS）／時脈（SHCP）／鎖存（STCP） | 74HC595 接 Arduino 的 3 條線 |
| 串接（Daisy Chain） | 把多顆 74HC595 頭尾相連，仍只用 3 隻 Arduino 腳 |
| Q7'（串接輸出） | 第 9 腳，接到下一顆的 DS |
| 七段顯示器 | 7 條燈管（a～g）＋小數點拼出數字 |
| 共陰極 | 七段顯示器的共同腳接 GND，段位給 1 就亮 |
| 燈號圖案 | 用 `0b` 開頭的 8 個 0/1 表示 8 顆燈的亮滅 |
| 安燈（Andon） | 讓任何人都能立刻通報異常的目視化管理工具 |
| 節拍時間（Takt Time） | 為了滿足需求，每多久必須完成一件產品 |
| 平均修復時間（MTTR） | 從異常發生到恢復，平均花多少時間 |

---

## 反思與延伸討論

1. 本週用 3 隻腳控制 24 個輸出，靠的是「一次送一位、送完再一起顯示」。這種「把很多事情排隊、批次處理」的做法，在管理上有沒有類似的例子？（提示：批次結帳、集中採購、排程）
2. 專案四的安燈看板和 Lv.5 的節拍時間看板，都把「異常」和「延遲」變成數字。這些數字可以用來**找出改善機會**，也可能被用來**責怪個人**。身為管理者，你會怎麼設計看板與制度，讓現場人員願意主動回報問題？
3. 74HC595 讓我們幾乎不再受限於「腳位不夠」。但它也有代價：不能調光、不能讀輸入、多了晶片和線路。回顧整學期的省腳位技巧，你會怎麼總結「什麼時候該用哪一招」？

---

## 本週檢核清單

- [ ] 能正確接上 74HC595（電源、MR、OE、DS、SHCP、STCP、Q0～Q7）
- [ ] 能用 74HC595 測試器確認 8 個輸出依序亮
- [ ] 能把 2～3 顆 74HC595 正確串接
- [ ] 能正確接上共陰極七段顯示器，並用測試器確認 0～9
- [ ] 能用「1 代表亮、0 代表滅」設計燈號圖案
- [ ] 能依輸出數量決定 74HC595 顆數，並比較「有／沒有 74HC595」的腳位
- [ ] 能說明安燈、節拍時間等看板指標的管理意義
- [ ] 完成 4 個獨立專案，並各自繳交 Tinkercad 檔案
- [ ] 完成練習題 Lv.1～Lv.4，挑戰 Lv.5 與 Lv.6

---

## 下週預告

第 14 週要學**繼電器**和 **PIR 人體感測器**，打造「智慧感應與操作台」。繼電器是 L293D 之後的另一種「以小控大」元件，能讓 Arduino 控制電燈、電扇這類家電；PIR 則能直接偵測「有沒有人」，比光敏電阻、超音波更適合做節能照明與保全系統。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 12 週](week-12.md) | [下一週：第 14 週 — 智慧感應與操作台 ➡](week-14.md)
