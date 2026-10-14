[⬅ 回目錄](../README.md) | [⬅ 上一週：第 11 週](week-11.md) | [下一週：第 13 週 — 產線數字看板升級站 ➡](week-13.md)

---

# 第 12 週：會輸入的智慧場域＋Arduino 單元總整理

## 主題專案：讓現場人員「輸入數字」，系統依指令行動

> 🎯 **本週定位**：到目前為止，人跟系統互動只能「按一下、轉一下」。這一週加入 **4×4 矩陣鍵盤**，現場人員第一次可以輸入**數字和指令**：密碼、員工編號、訂單號碼、量測數值。
>
> 這也是 Arduino 基礎元件的**最後一週**。鍵盤要佔 8 隻腳，加上上週的 LCD，腳位規劃會是整個單元最大的挑戰——正好檢驗你十二週來累積的配線功力。
>
> 和前幾週一樣，本週**重點不在寫程式**，而在於：
>
> 1. 讀懂情境需求，設計「操作流程」：使用者要按哪些鍵、看到什麼畫面
> 2. 規劃腳位——鍵盤要接幾行？LCD 用哪一版？還剩幾隻腳？
> 3. 在 Tinkercad 把電路一步步接起來
>
> 讀取鍵盤的程式都已經準備好，你只需要填腳位、修改設定、把功能開關從 `false` 改成 `true`。
>
> 🏁 **本週成果**：完成 **4 個獨立專案**，每個專案存成一個獨立的 Tinkercad 檔案繳交；並完成 **Arduino 單元總整理**。

---

## 📋 目錄

- [學習地圖](#學習地圖)
- [開場任務簡報](#開場任務簡報)
- [認識新元件：4×4 矩陣鍵盤](#認識新元件44-矩陣鍵盤)
- [操作流程設計原則](#操作流程設計原則)
- [配線口訣卡](#配線口訣卡)
- [共用工具](#共用工具)
- [專案一：員工打卡機](#專案一員工打卡機)
- [專案二：自動販賣機](#專案二自動販賣機)
- [專案三：辦公室智慧門鎖](#專案三辦公室智慧門鎖)
- [專案四：倉庫揀貨工作站](#專案四倉庫揀貨工作站)
- [練習題大補帖：情境配置挑戰](#練習題大補帖情境配置挑戰)
  - [Lv.1 社團選舉投票機](#-lv1-社團選舉投票機)
  - [Lv.2 健身房置物櫃](#-lv2-健身房置物櫃)
  - [Lv.3 品管抽檢輸入站](#-lv3-品管抽檢輸入站)
  - [Lv.4 自助洗衣機選單](#-lv4-自助洗衣機選單)
  - [Lv.5 金庫雙重驗證](#-lv5-金庫雙重驗證)
  - [🏆 Lv.6 BOSS 關：畢業機關提案](#-lv6-boss-關畢業機關提案)
- [🎓 Arduino 單元總整理](#-arduino-單元總整理)
- [配線除錯站](#配線除錯站)
- [成果繳交與評分](#成果繳交與評分)
- [📖 本週詞彙表](#-本週詞彙表)
- [反思與延伸討論](#反思與延伸討論)
- [本週檢核清單](#本週檢核清單)

---

## 學習地圖

### 你已經會的（第 1～11 週）

- ✅ 輸出：LED、RGB、蜂鳴器、伺服、直流馬達（L293D）、LCD（平行版、I2C 版）
- ✅ 輸入：按鈕、旋鈕、光敏電阻、TMP36、HC-SR04
- ✅ 「~ 腳可用表」、借用 A 腳、L293D 四種接法、LCD 版本選擇
- ✅ 狀態改變偵測、鎖定與復歸、安全連鎖、看板設計

### 這週要新學的

| 新東西 | 白話說明 | 在職場上像什麼 |
| --- | --- | --- |
| 4×4 矩陣鍵盤 | 16 顆按鍵排成 4 行 4 列，只用 8 條線 | 提款機、門禁、收銀機、設備參數面板 |
| `waitKey()`／`getKey()` | 已寫好的函式：讀到使用者按了哪個鍵 | 收銀員看顧客按了什麼 |
| 只接 3 行的省腳接法 | 不需要 A、B、C、D 鍵時，第 4 行不接，省 1 隻腳 | 只買需要的功能 |
| 操作流程設計 | 使用者每一步要按什麼、看到什麼 | 標準作業程序（SOP）、使用者體驗設計 |

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
| 4×4 矩陣鍵盤 | **本週新增** |

---

## 開場任務簡報

> 📖 **情境**：顧問公司成立滿一學期，這週接到四個需要「讓使用者輸入資料」的案子。客戶的共同需求是：現場人員要能**輸入號碼或密碼**，系統再依輸入內容做出反應。
>
> | 客戶 | 委託內容 | 難度 |
> | --- | --- | --- |
> | 中小企業 | 員工打卡機 | ⭐ |
> | 校園商店 | 自動販賣機 | ⭐⭐ |
> | 共享辦公室 | 智慧門鎖 | ⭐⭐⭐ |
> | 電商倉庫 | 揀貨工作站 | ⭐⭐⭐⭐ |
>
> 每個專案都分成 3 個步驟，從最簡單的版本開始，每一步多接幾個元件、多一項功能。

---

## 認識新元件：4×4 矩陣鍵盤

### 外觀與按鍵

```
       第1行 第2行 第3行 第4行
       (C1)  (C2)  (C3)  (C4)
第1列 R1 │ 1 │ 2 │ 3 │ A │
第2列 R2 │ 4 │ 5 │ 6 │ B │
第3列 R3 │ 7 │ 8 │ 9 │ C │
第4列 R4 │ * │ 0 │ # │ D │
```

- 16 顆按鍵，但只有 **8 隻腳**：4 條列線（R1～R4）＋ 4 條行線（C1～C4）。
- 按下一顆鍵，就是把某一條列線和某一條行線接通。Arduino 輪流檢查每一條線，就知道是哪顆鍵被按了——這叫**行列掃描**。
- 在 Tinkercad 元件庫搜尋「Keypad 4x4」，把滑鼠移到腳上會顯示 R1～R4、C1～C4。

### 接法：8 隻腳，不用電阻

| 鍵盤的腳 | 接到哪裡 |
| --- | --- |
| R1、R2、R3、R4 | 任何 4 隻數位腳（也可以借 A 腳） |
| C1、C2、C3、C4 | 任何 4 隻數位腳（也可以借 A 腳） |

- **不需要**任何電阻（和第 2 週的單顆按鈕不同）。
- 不需要 `~` 腳。

### ⭐ 省腳接法：只接 3 行

很多情境只需要 **0～9、\*、#** 這 12 顆鍵（例如輸入密碼、員工編號），用不到右邊那一行的 A、B、C、D。這時候 **C4 可以不接**，只佔 **7 隻腳**：

| 接法 | 可用的鍵 | 佔幾隻腳 | 適合情境 |
| --- | --- | --- | --- |
| 完整接法（4 行） | 全部 16 鍵 | 8 | 需要 A～D 當功能鍵（模式切換、管理選單） |
| 省腳接法（3 行） | 0～9、\*、# | 7 | 只需要輸入數字、確認、取消 |

### 鍵盤＋LCD 的腳位帳

| 組合 | 佔用腳位 | 18 隻中還剩 |
| --- | --- | --- |
| 鍵盤（4 行）＋平行版 LCD | 8 ＋ 6 ＝ 14 | 4 |
| 鍵盤（3 行）＋平行版 LCD | 7 ＋ 6 ＝ 13 | 5 |
| 鍵盤（4 行）＋I2C 版 LCD | 8 ＋ 2 ＝ 10 | **8** |
| 鍵盤（3 行）＋I2C 版 LCD | 7 ＋ 2 ＝ 9 | **9** |

> 💡 Arduino Uno 可用腳位：Pin 2～13（12 隻）＋ A0～A5（6 隻）＝ 18 隻。本週所有專案都建議用 **I2C 版 LCD**，其他元件才放得下。

### 程式裡的「讀鍵函式」

本週所有程式都附上這段，你**只需要填腳位**：

```cpp
#include <Keypad.h>

byte rowPins[4] = { ?, ?, ?, ? };   // R1, R2, R3, R4 接的腳位
byte colPins[4] = { ?, ?, ?, ? };   // C1, C2, C3, C4 接的腳位（只接 3 行時，C4 隨便填一隻沒用到的腳）

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);
```

之後只要：

```cpp
char k = keypad.getKey();   // 有人按鍵就回傳那個字元，沒人按就回傳 0
char k = waitKey();         // 一直等，直到有人按鍵為止
```

---

## 操作流程設計原則

鍵盤讓使用者可以「輸入」，但也讓使用者可能「按錯」。好的操作流程讓人**不用看說明書就會用**。

| 原則 | 說明 | 範例 |
| --- | --- | --- |
| ① 每一步都告訴使用者「現在要做什麼」 | LCD 第 0 行放提示 | `ENTER ID:`、`PRESS # TO OK` |
| ② 按下去要有回饋 | 每按一鍵 LCD 就顯示，或嗶一聲 | 輸入的數字立刻出現 |
| ③ 一定要能取消 | 統一用 `*` 清除重來 | `* = CLEAR` |
| ④ 統一用 `#` 確認 | 和提款機、電話一樣 | `# = OK` |
| ⑤ 密碼不能直接顯示 | 用 `*` 遮住 | `PIN: ****` |
| ⑥ 錯誤要說清楚 | 不只說「錯」，還說「怎麼辦」 | `WRONG (2/3)`、`TRY IN 30s` |

> 🏭 這些原則和**標準作業程序（SOP）**的精神一樣：讓不同的人做同一件事，都能得到一樣的結果。

---

## 配線口訣卡

| 口訣 | 說明 |
| --- | --- |
| ① **鍵盤 8 隻腳：R1～R4、C1～C4，不用電阻** | 滑鼠移上去確認腳位名稱 |
| ② **不需要 A～D 鍵，C4 不接，省 1 隻腳** | |
| ③ **有鍵盤就優先用 I2C 版 LCD** | 平行版會吃掉太多腳 |
| ④ 鍵盤、LCD 訊號線都不要佔 `~` 腳 | 把 `~` 腳留給調光、調速 |
| ⑤ 有伺服，9、10 不能調；有蜂鳴器，3、11 不能調 | 兩者都有時只剩 5、6 |
| ⑥ 每條燈線一顆 220Ω；按鈕、光敏電阻一顆 10kΩ | 鍵盤、LCD、伺服、蜂鳴器、HC-SR04 不用 |
| ⑦ Pin 0、1 不用；A 腳可借來接鍵盤、燈、按鈕 | 用 I2C 版時，A4、A5 已被佔用 |

---

## 共用工具

### 🔧 工具 1：鍵盤測試器

接好鍵盤後先跑這支，**16 顆鍵逐一按一遍**，確認 Serial Monitor 印出的字元和你按的位置一致。

```cpp
#include <Keypad.h>

byte rowPins[4] = { 2, 3, 4, 5 };   // ← R1~R4
byte colPins[4] = { 6, 7, 8, 9 };   // ← C1~C4

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);

void setup() {
  Serial.begin(9600);
  Serial.println("Press every key...");
}

void loop() {
  char k = keypad.getKey();
  if (k) {
    Serial.print("Key: ");
    Serial.println(k);
  }
}
```

**檢查標準**：按 `5` 印出 `5`、按 `D` 印出 `D`。如果按 `2` 卻印出 `4`，表示行線和列線接反了。

### 🔧 其他工具（沿用前幾週）

| 工具 | 用途 | 出處 |
| --- | --- | --- |
| LCD 測試器（平行版、I2C 版） | 檢查 LCD | 第 11 週 |
| 接線驗證器 | 檢查燈的腳位 | 第 3 週起 |
| 伺服測試器 | 找伺服角度 | 第 7 週 |
| 超音波測距檢查器 | 檢查 HC-SR04 | 第 9 週 |
| 馬達測試器 | 檢查 L293D 與馬達 | 第 10 週 |

> 📌 **每個專案的固定流程**
> 1. **Step 0**：畫出**操作流程圖**（使用者按什麼 → LCD 顯示什麼），再決定鍵盤接 3 行或 4 行、算腳位
> 2. 先接鍵盤和 LCD，分別用測試器確認
> 3. 照 Step 1 → 2 → 3 逐步接線，每完成一步就把對應的功能開關改成 `true`

---

## 專案一：員工打卡機

⭐ **難度**：鍵盤（先接 3 行）＋ I2C LCD，Step 3 再補接第 4 行

**客戶需求**：中小企業的員工打卡機。員工輸入 4 碼員工編號，按 `#` 打卡，LCD 顯示打卡結果與目前在公司的人數；按 `*` 清除重打。同一個人不能重複上班打卡。第二版老闆希望能**分上班、下班**：按 `A` 切換到上班模式、按 `B` 切換到下班模式。

### Step 0：操作流程與腳位規劃

**操作流程**（請補完）：

```
LCD: "IN  ENTER ID:"  →  按 1、2、3、4  →  LCD: "IN  ENTER ID:" / "1234"
  →  按 #  →  LCD: "ID 1234  IN OK" / "IN OFFICE: 3"   （嗶一聲、綠燈閃）
  →  2 秒後回到開頭
```

| 問題 | 你的答案 |
| --- | --- |
| Step 1～2 只需要數字、`*`、`#`，鍵盤接幾行？ | |
| Step 3 要用 `A`、`B`，要補接哪一條線？ | |

| 元件 | 腳位 |
| --- | --- |
| 鍵盤 R1～R4 | |
| 鍵盤 C1～C3（Step 1 接） | |
| 鍵盤 C4（Step 3 才接） | |
| I2C LCD | A4、A5 |
| 蜂鳴器 | |
| 綠燈 | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 鍵盤（只接 C1～C3）＋I2C LCD | 輸入 4 碼按 `#` 打卡，顯示在公司人數 | （不用改） |
| Step 2 | ＋蜂鳴器＋綠燈 | 打卡成功嗶一聲、綠燈閃；重複打卡低音提示 | `useFeedback = true` |
| Step 3 | **補接 C4** | 按 `A` 切上班、按 `B` 切下班 | `useMode = true` |

### 程式（只改設定區）

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C4（C4 Step 3 才接）
int pinBuzzer = ?;                  // 蜂鳴器
int ledOK     = ?;                  // 綠燈
bool useFeedback = false;           // Step 2 接好後改成 true
bool useMode     = false;           // Step 3 補接 C4 後改成 true
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

String input = "";
bool modeIn = true;                 // true=上班, false=下班
String present[20];                 // 目前在公司的員工編號
int presentCount = 0;

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

int findID(String id) {
  for (int i = 0; i < presentCount; i++) if (present[i] == id) return i;
  return -1;
}

void result(String msg, bool ok) {
  showLine(0, msg);
  showLine(1, "IN OFFICE: " + String(presentCount));
  if (useFeedback) {
    if (ok) { tone(pinBuzzer, 1500, 150); digitalWrite(ledOK, HIGH); }
    else    { tone(pinBuzzer, 300, 500); }
  }
  delay(2000);
  digitalWrite(ledOK, LOW);
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  pinMode(ledOK, OUTPUT);
}

void loop() {
  showLine(0, String(modeIn ? "IN " : "OUT") + " ENTER ID:");
  showLine(1, input);

  char k = keypad.getKey();
  if (!k) return;

  if (k >= '0' && k <= '9' && input.length() < 4) {
    input += k;
  } else if (k == '*') {
    input = "";
  } else if (useMode && k == 'A') {
    modeIn = true;
  } else if (useMode && k == 'B') {
    modeIn = false;
  } else if (k == '#') {
    if (input.length() != 4) {
      result("NEED 4 DIGITS", false);
    } else if (modeIn) {
      if (findID(input) >= 0) result("ID " + input + " ALREADY IN", false);
      else { present[presentCount++] = input; result("ID " + input + " IN OK", true); }
    } else {
      int i = findID(input);
      if (i < 0) result("ID " + input + " NOT IN", false);
      else {
        present[i] = present[presentCount - 1];
        presentCount--;
        result("ID " + input + " OUT OK", true);
      }
    }
    input = "";
  }
}
```

### 情境思考

1. Step 1 只接 3 行時，按 `A` 會發生什麼事？為什麼 Step 3 只要補接一條線，不用改其他接線？
2. 員工輸入 3 碼就按 `#`，系統怎麼回應？這符合「操作流程設計原則」的哪一條？
3. 💼 打卡資料是計算薪資的依據。目前系統關機後資料就消失了，這對人資部門有什麼風險？還缺什麼？

### ✅ 專案一驗收

- [ ] 輸入 4 碼按 `#` 打卡成功，人數加 1；同一人重複上班打卡會被拒絕
- [ ] 按 `*` 清除輸入
- [ ] 成功時嗶聲＋綠燈，失敗時低音
- [ ] 補接 C4 後，`A`／`B` 能切換上班／下班模式，下班打卡人數減 1

---

## 專案二：自動販賣機

⭐⭐ **難度**：鍵盤（3 行）＋ I2C LCD ＋ 投幣鈕 ＋ 伺服出貨口 ＋ 蜂鳴器

**客戶需求**：校園商店的飲料販賣機，有 4 種商品，編號 11～14。顧客輸入 2 位數商品編號按 `#`，LCD 顯示價格；按「投幣鈕」每次投入 10 元，LCD 顯示已投金額；金額足夠時出貨口（伺服）打開 1.5 秒，並顯示找零。按 `*` 取消並退回已投的錢。

| 編號 | 商品 | 價格 |
| --- | --- | --- |
| 11 | WATER | 20 |
| 12 | TEA | 25 |
| 13 | COLA | 30 |
| 14 | COFFEE | 35 |

### Step 0：操作流程與腳位規劃

```
"SELECT No.:"  →  按 1、2、#  →  "No.12 TEA $25" / "PAID $0"
  →  投幣 ×3  →  "PAID $30"  →  出貨  →  "ENJOY! CHANGE $5"
```

| 元件 | 腳位 | 是不是 `~` 腳？ |
| --- | --- | --- |
| 鍵盤 R1～R4、C1～C3 | | |
| I2C LCD | A4、A5 | — |
| 投幣鈕 | | |
| 出貨伺服 | | |
| 蜂鳴器 | | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 鍵盤（3 行）＋I2C LCD | 輸入編號顯示商品與價格；編號錯誤會提示 | （不用改） |
| Step 2 | ＋投幣鈕＋蜂鳴器 | 投幣累加金額，按 `*` 退幣 | `useCoin = true` |
| Step 3 | ＋伺服 | 金額足夠時出貨並找零 | `useDispense = true` |

### 程式（只改設定區）

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>
#include <Servo.h>

// ===== 設定區：只改這裡 =====
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C3（C4 不接，隨便填一隻空腳）
int btnCoin   = ?;                  // 投幣鈕（每按一次 10 元）
int pinServo  = ?;                  // 出貨口伺服
int pinBuzzer = ?;                  // 蜂鳴器
String names[4] = { "WATER", "TEA", "COLA", "COFFEE" };
int prices[4]   = { 20, 25, 30, 35 };
bool useCoin     = false;           // Step 2 接好後改成 true
bool useDispense = false;           // Step 3 接好後改成 true
// ============================

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);
Adafruit_LiquidCrystal lcd(0);
Servo door;

String input = "";
int selected = -1;
int paid = 0;
int lastCoin = LOW;

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  pinMode(btnCoin, INPUT);
  door.attach(pinServo);
  door.write(0);
}

void loop() {
  // ---- 畫面 ----
  if (selected < 0) {
    showLine(0, "SELECT No.:");
    showLine(1, input);
  } else {
    showLine(0, "No." + String(11 + selected) + " " + names[selected] + " $" + String(prices[selected]));
    showLine(1, "PAID $" + String(paid));
  }

  // ---- 投幣 ----
  if (useCoin) {
    int now = digitalRead(btnCoin);
    if (now == HIGH && lastCoin == LOW) {
      delay(50);
      paid += 10;
      tone(pinBuzzer, 2000, 60);
    }
    lastCoin = now;
  }

  // ---- 出貨 ----
  if (useDispense && selected >= 0 && paid >= prices[selected]) {
    int change = paid - prices[selected];
    showLine(0, "ENJOY YOUR " + names[selected]);
    showLine(1, "CHANGE $" + String(change));
    tone(pinBuzzer, 1319, 300);
    door.write(90);
    delay(1500);
    door.write(0);
    delay(1500);
    selected = -1;
    paid = 0;
    input = "";
  }

  // ---- 鍵盤 ----
  char k = keypad.getKey();
  if (!k) return;
  if (k >= '0' && k <= '9' && input.length() < 2 && selected < 0) {
    input += k;
  } else if (k == '*') {
    if (paid > 0) {
      showLine(0, "CANCELLED");
      showLine(1, "REFUND $" + String(paid));
      delay(1500);
    }
    selected = -1;
    paid = 0;
    input = "";
  } else if (k == '#' && selected < 0) {
    int code = input.toInt();
    if (code >= 11 && code <= 14) {
      selected = code - 11;
    } else {
      showLine(0, "NO SUCH ITEM");
      showLine(1, "TRY 11-14");
      delay(1500);
    }
    input = "";
  }
}
```

### 情境思考

1. 這個系統有伺服和蜂鳴器，但沒有需要調光的元件。`~` 腳被借走對這個專案有影響嗎？
2. 顧客投了 30 元後才發現選錯商品。目前的操作流程能處理嗎？顧客要按什麼？
3. 💼 販賣機業者想知道「哪個商品賣最好、什麼時段賣最多」。你會建議系統多記錄什麼資料？這些資料如何幫助補貨排程？

### ✅ 專案二驗收

- [ ] 輸入 11～14 按 `#` 顯示正確商品與價格；其他號碼提示錯誤
- [ ] 每按一次投幣鈕加 10 元，按 `*` 取消並顯示退款
- [ ] 金額足夠時伺服打開 1.5 秒出貨，並顯示正確找零
- [ ] 鍵盤只接 3 行

---

## 專案三：辦公室智慧門鎖

⭐⭐⭐ **難度**：鍵盤（4 行）＋ I2C LCD ＋ 伺服門鎖 ＋ RGB ＋ 蜂鳴器 → **一般腳不夠，要借 A 腳**

**客戶需求**：共享辦公室的門鎖。輸入 4 碼密碼按 `#`：

- 正確：門鎖（伺服）打開 3 秒，RGB 綠燈
- 錯誤：RGB 紅燈＋低音；**連錯 3 次鎖定 30 秒**，LCD 顯示倒數
- 管理員按 `A` 進入改密碼模式：先輸入舊密碼，再輸入新密碼
- 輸入密碼時 LCD 只顯示 `*`，不顯示數字

### Step 0：操作流程與腳位規劃（本專案的重頭戲）

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 鍵盤（4 行，要用 A 鍵） | 8 | |
| I2C LCD | 2 | A4、A5 |
| 門鎖伺服 | 1 | |
| 蜂鳴器 | 1 | |
| 狀態 RGB | 3 | |
| **A4、A5 以外的合計** | | |
| **Pin 2～13＋A0～A3 可用** | 16 | |

> 🤔 **規劃提示**：
> - 改用平行版 LCD 的話，一共要幾隻？放得下嗎？
> - 有伺服＋蜂鳴器，`~` 腳只剩 5、6。RGB 只顯示紅、綠、藍，需要 `~` 腳嗎？

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 鍵盤（4 行）＋I2C LCD＋伺服 | 正確密碼開門 3 秒，輸入時顯示 `****` | （不用改） |
| Step 2 | ＋RGB＋蜂鳴器 | 綠／紅回饋；連錯 3 次鎖定 30 秒並倒數 | `useAlarm = true` |
| Step 3 | （不用加元件） | 按 `A` 進入改密碼流程 | `useChange = true` |

### 程式（只改設定區）

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>
#include <Servo.h>

// ===== 設定區：只改這裡 =====
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C4
int pinServo  = ?;                  // 門鎖伺服
int pinBuzzer = ?;                  // 蜂鳴器
int pinR = ?, pinG = ?, pinB = ?;   // 狀態 RGB（全亮全滅）
String password = "1234";           // 初始密碼
int maxWrong = 3;                   // 連錯幾次鎖定
int lockSec  = 30;                  // 鎖定幾秒
bool useAlarm  = false;             // Step 2 接好後改成 true
bool useChange = false;             // Step 3 改成 true
// ============================

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);
Adafruit_LiquidCrystal lcd(0);
Servo lock;

String input = "";
int wrong = 0;
int mode = 0;                       // 0=一般, 1=輸入舊密碼, 2=輸入新密碼

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

String stars(int n) {
  String s = "";
  for (int i = 0; i < n; i++) s += "*";
  return s;
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  lock.attach(pinServo);
  lock.write(0);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  if (mode == 0)      showLine(0, "ENTER PIN:");
  else if (mode == 1) showLine(0, "OLD PIN:");
  else                showLine(0, "NEW PIN (4):");
  showLine(1, stars(input.length()));
  if (useAlarm) setColor(LOW, LOW, HIGH);          // 待機：藍

  char k = keypad.getKey();
  if (!k) return;

  if (k >= '0' && k <= '9' && input.length() < 4) {
    input += k;
    if (useAlarm) tone(pinBuzzer, 1000, 30);
  } else if (k == '*') {
    input = "";
    mode = 0;
  } else if (useChange && k == 'A' && mode == 0) {
    mode = 1;
    input = "";
  } else if (k == '#') {
    if (mode == 2) {                                // 設定新密碼
      if (input.length() == 4) {
        password = input;
        showLine(0, "PIN CHANGED");
      } else {
        showLine(0, "NEED 4 DIGITS");
      }
      showLine(1, "");
      delay(1500);
      mode = 0;
    } else if (input == password) {                 // 密碼正確
      wrong = 0;
      if (mode == 1) {
        mode = 2;                                   // 舊密碼正確，進入設定新密碼
      } else {
        showLine(0, "WELCOME!");
        showLine(1, "DOOR OPEN");
        if (useAlarm) { setColor(LOW, HIGH, LOW); tone(pinBuzzer, 1319, 300); }
        lock.write(90);
        delay(3000);
        lock.write(0);
      }
    } else {                                        // 密碼錯誤
      wrong++;
      mode = 0;
      showLine(0, "WRONG PIN");
      showLine(1, "(" + String(wrong) + "/" + String(maxWrong) + ")");
      if (useAlarm) { setColor(HIGH, LOW, LOW); tone(pinBuzzer, 250, 600); }
      delay(1500);
      if (useAlarm && wrong >= maxWrong) {          // 鎖定倒數
        for (int s = lockSec; s > 0; s--) {
          showLine(0, "!! LOCKED !!");
          showLine(1, "TRY IN " + String(s) + "s");
          setColor(s % 2 ? HIGH : LOW, LOW, LOW);
          delay(1000);
        }
        wrong = 0;
      }
    }
    input = "";
  }
}
```

### 情境思考

1. 用你的腳位規劃表說明：如果改用平行版 LCD，總共要幾隻腳？為什麼一定要用 I2C 版？
2. 鎖定期間有人一直亂按鍵盤，系統會有反應嗎？這樣的設計對「安全」和「使用者體驗」各有什麼影響？
3. 💼 共享辦公室有 50 位會員，如果大家共用同一組密碼，有人退租後要怎麼辦？「一人一組密碼」需要什麼條件？（提示：專案一的員工編號）

### ✅ 專案三驗收

- [ ] 輸入時 LCD 只顯示 `*`
- [ ] 正確密碼開門 3 秒、綠燈；錯誤紅燈低音並顯示錯誤次數
- [ ] 連錯 3 次鎖定 30 秒，LCD 倒數，期間按鍵無效
- [ ] 按 `A` → 輸入舊密碼 → 輸入新密碼後，新密碼生效、舊密碼失效
- [ ] 腳位規劃表正確借用 A 腳，並說明 RGB 為何不需要 `~` 腳

---

## 專案四：倉庫揀貨工作站

⭐⭐⭐⭐ **難度**：鍵盤 ＋ I2C LCD ＋ 3 顆儲位燈 ＋ 蜂鳴器 ＋ 輸送帶馬達 ＋ HC-SR04 → **電子標籤揀貨（Pick-to-Light）**

**客戶需求**：電商倉庫的揀貨員拿到訂單後，在工作站輸入**訂單編號**（3 碼），系統依訂單內容：

1. 依序點亮要揀的**儲位燈**（A、B、C 三個儲位），LCD 顯示「哪個儲位、拿幾個」
2. 揀貨員拿完按 `#`，系統換下一個儲位
3. 全部揀完後，LCD 提示「把箱子放上輸送帶」；超音波偵測到箱子，輸送帶（直流馬達）運轉 3 秒送往包裝區
4. 每完成一張訂單，LCD 顯示**這張單花了幾秒**和**今天完成幾張**

| 訂單 | 儲位 A | 儲位 B | 儲位 C |
| --- | --- | --- | --- |
| 101 | 2 | 0 | 1 |
| 102 | 1 | 1 | 0 |
| 103 | 0 | 3 | 2 |
| 104 | 1 | 1 | 1 |

> 🏭 **Pick-to-Light（電子標籤揀貨）** 是現代倉儲常見的做法：用燈號指引揀貨員「去哪裡、拿幾個」，減少找貨時間和揀錯率。

### Step 0：操作流程與腳位規劃

| 問題 | 你的答案 |
| --- | --- |
| 這個工作站需要 A～D 鍵嗎？鍵盤接幾行？ | |
| 輸送帶只往一個方向、固定速度，L293D 用哪種接法？佔幾隻腳？ | |

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 鍵盤 | | |
| I2C LCD | 2 | A4、A5 |
| 儲位燈 × 3 | 3 | |
| 蜂鳴器 | 1 | |
| 輸送帶（L293D） | | |
| HC-SR04 | 2 | |
| **A4、A5 以外的合計／可用 16 隻** | | |

### 逐步接線

| 步驟 | 接上哪些元件 | 完成後會看到 | 功能開關 |
| --- | --- | --- | --- |
| Step 1 | 鍵盤（3 行）＋I2C LCD | 輸入訂單編號，LCD 依序顯示每個儲位與數量，按 `#` 換下一個 | （不用改） |
| Step 2 | ＋3 顆儲位燈＋蜂鳴器 | 對應的儲位燈亮起，換儲位時嗶一聲 | `useLights = true` |
| Step 3 | ＋L293D（接法 A）＋馬達＋HC-SR04 | 箱子放上輸送帶自動送出，顯示單張耗時與完成張數 | `useConveyor = true` |

### 程式（只改設定區）

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C3（C4 不接，隨便填一隻空腳）
int binLed[3] = { ?, ?, ? };        // 儲位 A、B、C 燈
int pinBuzzer = ?;                  // 蜂鳴器
int pinMotor  = ?;                  // L293D IN1（接法 A：EN 接 5V、IN2 接 GND）
int pinTrig = ?, pinEcho = ?;       // 輸送帶起點 HC-SR04
int orderNo[4]  = { 101, 102, 103, 104 };
int qty[4][3]   = { {2, 0, 1}, {1, 1, 0}, {0, 3, 2}, {1, 1, 1} };
float boxCM = 15;                   // 箱子放上輸送帶的距離
bool useLights   = false;           // Step 2 接好後改成 true
bool useConveyor = false;           // Step 3 接好後改成 true
// ============================

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);
Adafruit_LiquidCrystal lcd(0);

String input = "";
int done = 0;
unsigned long totalSec = 0;
char binName[3] = { 'A', 'B', 'C' };

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

char waitKey() {
  char k = 0;
  while (!k) k = keypad.getKey();
  return k;
}

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void pickOrder(int o) {
  unsigned long start = millis();
  for (int b = 0; b < 3; b++) {
    if (qty[o][b] == 0) continue;
    showLine(0, "BIN " + String(binName[b]) + "  QTY " + String(qty[o][b]));
    showLine(1, "PICK, THEN #");
    if (useLights) {
      digitalWrite(binLed[b], HIGH);
      tone(pinBuzzer, 1200, 100);
    }
    while (waitKey() != '#') { }
    digitalWrite(binLed[b], LOW);
  }

  if (useConveyor) {
    showLine(0, "PUT BOX ON BELT");
    showLine(1, "WAITING...");
    while (readCM(pinTrig, pinEcho) >= boxCM) delay(100);
    showLine(1, "SENDING...");
    digitalWrite(pinMotor, HIGH);
    delay(3000);
    digitalWrite(pinMotor, LOW);
  }

  unsigned long sec = (millis() - start) / 1000;
  done++;
  totalSec += sec;
  showLine(0, "ORDER " + String(orderNo[o]) + " " + String(sec) + "s");
  showLine(1, "DONE " + String(done) + " AVG " + String(totalSec / done) + "s");
  tone(pinBuzzer, 1568, 300);
  delay(2500);
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  for (int b = 0; b < 3; b++) pinMode(binLed[b], OUTPUT);
  pinMode(pinMotor, OUTPUT);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
}

void loop() {
  showLine(0, "ORDER No.:");
  showLine(1, input);

  char k = keypad.getKey();
  if (!k) return;

  if (k >= '0' && k <= '9' && input.length() < 3) {
    input += k;
  } else if (k == '*') {
    input = "";
  } else if (k == '#') {
    int found = -1;
    for (int o = 0; o < 4; o++) if (orderNo[o] == input.toInt()) found = o;
    if (found >= 0) {
      pickOrder(found);
    } else {
      showLine(0, "ORDER NOT FOUND");
      showLine(1, "");
      tone(pinBuzzer, 300, 500);
      delay(1500);
    }
    input = "";
  }
}
```

### 情境思考

1. 用你的腳位規劃表說明：這個工作站一共用了幾隻腳？鍵盤接 3 行和輸送帶用接法 A，各省了幾隻腳？
2. 揀貨員在儲位 B 拿錯數量，系統發現得了嗎？真實的 Pick-to-Light 系統怎麼防止揀錯？（提示：每個儲位旁的確認鈕、秤重）
3. 💼 LCD 顯示「平均每單 38 秒」。倉庫主管要如何利用這個數字安排人力？如果某位揀貨員明顯比較慢，你會怎麼分析原因？

### ✅ 專案四驗收

- [ ] 輸入 101～104 按 `#`，LCD 依序顯示需要揀的儲位與數量（數量 0 的儲位自動跳過）
- [ ] 對應儲位燈亮起，按 `#` 換下一個儲位
- [ ] 揀完後箱子放上輸送帶（HC-SR04 偵測），馬達運轉 3 秒
- [ ] LCD 顯示單張耗時、完成張數與平均時間
- [ ] 鍵盤接 3 行、L293D 用接法 A，腳位規劃表說明省了幾隻腳

---

## 練習題大補帖：情境配置挑戰

> 🎯 **本區定位**：四個範例專案是「跟著做」，這一區換你「自己規劃」。每一關先**畫操作流程圖**、決定鍵盤接 3 行或 4 行、**算腳位**，再動手接線。
>
> 📈 **難度安排**：
>
> | 關卡 | 鍵盤 | 其他元件 | 配線重點 |
> | --- | --- | --- | --- |
> | Lv.1 | 3 行 | I2C LCD | 最基本的鍵盤＋LCD |
> | Lv.2 | 3 行 | I2C LCD＋伺服＋LED | 使用者自訂密碼 |
> | Lv.3 | 3 行 | I2C LCD＋RGB＋蜂鳴器 | 輸入量測值做品管判定 |
> | Lv.4 | 3 行 | I2C LCD＋L293D 馬達＋蜂鳴器 | 鍵盤選單控制馬達 |
> | Lv.5 | 4 行 | I2C LCD＋伺服＋RGB＋蜂鳴器＋按鈕＋HC-SR04 | **18 隻腳全部用滿** |
> | Lv.6 | 自訂 | 自訂 | 畢業機關提案 |
>
> 📌 **作答規則**：每一關先畫操作流程、填腳位規劃表，再接線；接好後先用鍵盤測試器、LCD 測試器確認，最後才上傳程式。程式只需要改「設定區」。

---

### 🟢 Lv.1 社團選舉投票機

**情境**：社團幹部選舉，有 4 位候選人（編號 1～4）。投票者按候選人編號，LCD 顯示「確認投給 X 號？」，按 `#` 確認、按 `*` 重選。主持人按 `0` 可以看目前票數（3 秒後自動回到投票畫面）。

**元件**：鍵盤（3 行）＋I2C LCD

**程式**：

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C3（C4 不接）
int numCandidates = 4;
// ============================

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);
Adafruit_LiquidCrystal lcd(0);

int votes[4] = { 0, 0, 0, 0 };
int choice = 0;

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
}

void loop() {
  if (choice == 0) {
    showLine(0, "VOTE: PRESS 1-" + String(numCandidates));
    showLine(1, "");
  } else {
    showLine(0, "VOTE FOR No." + String(choice) + "?");
    showLine(1, "#=YES  *=BACK");
  }

  char k = keypad.getKey();
  if (!k) return;

  if (k >= '1' && k <= '0' + numCandidates) {
    choice = k - '0';
  } else if (k == '*') {
    choice = 0;
  } else if (k == '#' && choice > 0) {
    votes[choice - 1]++;
    showLine(0, "THANK YOU!");
    showLine(1, "");
    delay(1500);
    choice = 0;
  } else if (k == '0') {
    showLine(0, "1:" + String(votes[0]) + " 2:" + String(votes[1]));
    showLine(1, "3:" + String(votes[2]) + " 4:" + String(votes[3]));
    delay(3000);
  }
}
```

**情境思考**：

1. 為什麼要多一步「確認」，而不是按下編號就直接投票？
2. 💼 任何人都可以按 `0` 看票數，也可以一直重複投票。如果是正式選舉，這兩個問題要怎麼解決？

---

### 🟢 Lv.2 健身房置物櫃

**情境**：健身房的置物櫃沒有鑰匙。櫃子空著時，會員**自己設定一組 4 碼密碼**按 `#`，門鎖（伺服）鎖上、使用中燈亮；取物時輸入同一組密碼按 `#` 開鎖，密碼清除，櫃子恢復可用。

**元件**：鍵盤（3 行）＋I2C LCD＋伺服＋1 顆 LED

**程式**：

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>
#include <Servo.h>

// ===== 設定區：只改這裡 =====
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C3（C4 不接）
int pinServo = ?;                   // 門鎖伺服
int ledBusy  = ?;                   // 使用中燈
int openAngle = 0, lockAngle = 90;
// ============================

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);
Adafruit_LiquidCrystal lcd(0);
Servo lock;

String input = "";
String code = "";                   // 空字串代表櫃子沒人用

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  lock.attach(pinServo);
  lock.write(openAngle);
  pinMode(ledBusy, OUTPUT);
}

void loop() {
  bool inUse = (code != "");
  digitalWrite(ledBusy, inUse ? HIGH : LOW);
  showLine(0, inUse ? "IN USE. PIN:" : "SET 4-DIGIT PIN:");
  String mask = "";
  for (int i = 0; i < input.length(); i++) mask += "*";
  showLine(1, mask);

  char k = keypad.getKey();
  if (!k) return;

  if (k >= '0' && k <= '9' && input.length() < 4) {
    input += k;
  } else if (k == '*') {
    input = "";
  } else if (k == '#' && input.length() == 4) {
    if (!inUse) {
      code = input;
      lock.write(lockAngle);
      showLine(0, "LOCKED");
      showLine(1, "REMEMBER YOUR PIN");
    } else if (input == code) {
      code = "";
      lock.write(openAngle);
      showLine(0, "UNLOCKED");
      showLine(1, "THANK YOU");
    } else {
      showLine(0, "WRONG PIN");
      showLine(1, "");
    }
    delay(1500);
    input = "";
  }
}
```

**情境思考**：

1. 這一關沒有蜂鳴器，只有伺服。如果要把 LED 換成「可調亮度」的燈，哪些 `~` 腳不能用？
2. 💼 會員忘記密碼是健身房櫃台最常見的困擾。管理員需要一個「萬能開鎖」功能，你會怎麼設計？需要接第 4 行嗎？

---

### 🟡 Lv.3 品管抽檢輸入站

**情境**：食品工廠包裝線的品管站，品管員把抽檢的產品放上磅秤，**用鍵盤輸入重量**（公克）按 `#`。規格是 **495～505 公克**：合格 RGB 亮綠，不合格亮紅並嗶一聲。LCD 第 0 行顯示本次判定，第 1 行顯示累計的合格數、不合格數與**良率**。

**元件**：鍵盤（3 行）＋I2C LCD＋RGB＋蜂鳴器

**程式**：

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C3（C4 不接）
int pinR = ?, pinG = ?, pinB = ?;   // 判定 RGB
int pinBuzzer = ?;                  // 蜂鳴器
int lowSpec  = 495;                 // 規格下限（公克）
int highSpec = 505;                 // 規格上限（公克）
// ============================

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);
Adafruit_LiquidCrystal lcd(0);

String input = "";
int okCount = 0, ngCount = 0;
String lastResult = "ENTER WEIGHT g";

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

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
}

void loop() {
  int total = okCount + ngCount;
  int yieldPct = (total > 0) ? okCount * 100 / total : 0;
  showLine(0, (input.length() > 0) ? "WEIGHT: " + input + " g" : lastResult);
  showLine(1, "OK" + String(okCount) + " NG" + String(ngCount) + " Y" + String(yieldPct) + "%");

  char k = keypad.getKey();
  if (!k) return;

  if (k >= '0' && k <= '9' && input.length() < 4) {
    input += k;
  } else if (k == '*') {
    input = "";
  } else if (k == '#' && input.length() > 0) {
    int w = input.toInt();
    if (w >= lowSpec && w <= highSpec) {
      okCount++;
      lastResult = String(w) + "g  OK";
      setColor(LOW, HIGH, LOW);
    } else {
      ngCount++;
      lastResult = String(w) + "g  NG " + (w < lowSpec ? "LOW" : "HIGH");
      setColor(HIGH, LOW, LOW);
      tone(pinBuzzer, 400, 400);
    }
    input = "";
  }
}
```

**情境思考**：

1. 品管員手滑把 500 打成 5000，系統會怎麼判定？你會在操作流程上加什麼防呆？
2. 💼 良率從 98% 掉到 90%，而且不合格的大多是「偏輕」。品管主管下一步應該檢查什麼？這個輸入站還可以多記錄什麼，幫助找出原因？

---

### 🟡 Lv.4 自助洗衣機選單

**情境**：自助洗衣店的洗衣機面板。顧客按 `1`、`2`、`3` 選擇洗程（快洗、標準、強力），LCD 顯示洗程名稱與輪數，按 `#` 開始。洗衣過程馬達**正轉 3 秒 → 停 1 秒 → 反轉 3 秒 → 停 1 秒**為一輪，LCD 顯示目前第幾輪；洗完蜂鳴器播放提示。

| 按鍵 | 洗程 | 輪數 |
| --- | --- | --- |
| 1 | QUICK | 1 |
| 2 | NORMAL | 2 |
| 3 | HEAVY | 3 |

**元件**：鍵盤（3 行）＋I2C LCD＋L293D＋直流馬達＋蜂鳴器

**Step 0：腳位規劃**

| 問題 | 你的答案 |
| --- | --- |
| 洗衣機要反轉、不用調速，L293D 用哪種接法？佔幾隻腳？ | |
| 鍵盤＋LCD＋L293D＋蜂鳴器，一共幾隻腳？ | |

**程式**：

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>

// ===== 設定區：只改這裡 =====
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C3（C4 不接）
int pinIN1 = ?, pinIN2 = ?;         // L293D 方向腳（接法 C，EN 接 5V）
int pinBuzzer = ?;                  // 蜂鳴器
String progName[3] = { "QUICK", "NORMAL", "HEAVY" };
int progRounds[3]  = { 1, 2, 3 };
// ============================

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);
Adafruit_LiquidCrystal lcd(0);

int prog = -1;

void showLine(int row, String text) {
  while (text.length() < 16) text += " ";
  lcd.setCursor(0, row);
  lcd.print(text.substring(0, 16));
}

void motor(int in1, int in2, int ms) {
  digitalWrite(pinIN1, in1);
  digitalWrite(pinIN2, in2);
  delay(ms);
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  pinMode(pinIN1, OUTPUT);
  pinMode(pinIN2, OUTPUT);
}

void loop() {
  if (prog < 0) {
    showLine(0, "1QUICK 2NORMAL");
    showLine(1, "3HEAVY");
  } else {
    showLine(0, progName[prog] + " x" + String(progRounds[prog]));
    showLine(1, "#=START *=BACK");
  }

  char k = keypad.getKey();
  if (!k) return;

  if (k >= '1' && k <= '3') {
    prog = k - '1';
  } else if (k == '*') {
    prog = -1;
  } else if (k == '#' && prog >= 0) {
    for (int r = 1; r <= progRounds[prog]; r++) {
      showLine(0, "WASHING " + progName[prog]);
      showLine(1, "ROUND " + String(r) + "/" + String(progRounds[prog]));
      motor(HIGH, LOW, 3000);
      motor(LOW, LOW, 1000);
      motor(LOW, HIGH, 3000);
      motor(LOW, LOW, 1000);
    }
    showLine(0, "DONE!");
    showLine(1, "PLEASE TAKE OUT");
    int notes[] = { 523, 659, 784, 1047 };
    for (int i = 0; i < 4; i++) { tone(pinBuzzer, notes[i], 200); delay(250); }
    delay(2000);
    prog = -1;
  }
}
```

**情境思考**：

1. 第 10 週的洗衣機用旋鈕選輪數，這週改用鍵盤選洗程。對顧客來說哪一種比較好用？對店家來說呢？
2. 💼 洗衣店想推出「會員價」：會員輸入手機末 4 碼享 9 折。操作流程要怎麼改？需要接第 4 行嗎？

---

### 🔴 Lv.5 金庫雙重驗證

**情境**：銀行金庫的進入管制，必須通過**三道關卡**才能開門：

1. **有人站在門前**：超音波偵測到 50 cm 內有人，系統才啟動，否則 LCD 顯示 `STEP CLOSER`
2. **輸入密碼**：4 碼密碼正確
3. **轉動實體鑰匙**：密碼正確後 10 秒內，必須按下「鑰匙鈕」（模擬插入實體鑰匙）

三關都通過，金庫門（伺服）打開 5 秒，RGB 綠燈。任何一關失敗 RGB 紅燈＋警報；**連續失敗 3 次鎖定 60 秒**。主管按 `D` 可以查看今日成功與失敗次數。

**元件**：鍵盤（4 行）＋I2C LCD＋HC-SR04＋伺服＋RGB＋蜂鳴器＋1 顆按鈕

**Step 0：腳位規劃（本關重點）**

| 元件 | 需要幾隻腳 | 腳位 |
| --- | --- | --- |
| 鍵盤（4 行） | 8 | |
| I2C LCD | 2 | A4、A5 |
| HC-SR04 | 2 | |
| 金庫門伺服 | 1 | |
| RGB | 3 | |
| 蜂鳴器 | 1 | |
| 鑰匙鈕 | 1 | |
| **總計** | | |
| **Arduino 可用總數** | 18 | |

> 🤔 **規劃提示**：這一關會用掉**全部 18 隻腳**。請把 Pin 2～13、A0～A5 畫成一張表，逐一填上元件。如果客戶還想加一顆「門已開」指示燈，你會建議拿掉什麼，或改變什麼設計？

**程式**：

```cpp
#include <Keypad.h>
#include <Adafruit_LiquidCrystal.h>
#include <Servo.h>

// ===== 設定區：只改這裡 =====
byte rowPins[4] = { ?, ?, ?, ? };   // 鍵盤 R1~R4
byte colPins[4] = { ?, ?, ?, ? };   // 鍵盤 C1~C4
int pinTrig = ?, pinEcho = ?;       // 門前 HC-SR04
int pinServo  = ?;                  // 金庫門伺服
int pinR = ?, pinG = ?, pinB = ?;   // 狀態 RGB（全亮全滅）
int pinBuzzer = ?;                  // 蜂鳴器
int btnKey    = ?;                  // 實體鑰匙鈕
String password = "2580";
float nearCM = 50;
int keyWindow = 10;                 // 密碼正確後幾秒內要轉鑰匙
// ============================

char keys[4][4] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, 4, 4);
Adafruit_LiquidCrystal lcd(0);
Servo vault;

String input = "";
int fails = 0, okToday = 0, failToday = 0;

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

float readCM(int trig, int echo) {
  digitalWrite(trig, LOW);  delayMicroseconds(2);
  digitalWrite(trig, HIGH); delayMicroseconds(10);
  digitalWrite(trig, LOW);
  long t = pulseIn(echo, HIGH, 30000);
  if (t == 0) return 999;
  return t / 58.0;
}

void fail(String why) {
  fails++;
  failToday++;
  showLine(0, "DENIED: " + why);
  showLine(1, "(" + String(fails) + "/3)");
  setColor(HIGH, LOW, LOW);
  tone(pinBuzzer, 250, 800);
  delay(2000);
  if (fails >= 3) {
    for (int s = 60; s > 0; s--) {
      showLine(0, "!! VAULT LOCKED");
      showLine(1, "WAIT " + String(s) + "s");
      setColor(s % 2 ? HIGH : LOW, LOW, LOW);
      delay(1000);
    }
    fails = 0;
  }
}

void setup() {
  lcd.begin(16, 2);
  lcd.setBacklight(1);
  pinMode(pinTrig, OUTPUT);
  pinMode(pinEcho, INPUT);
  pinMode(pinR, OUTPUT);
  pinMode(pinG, OUTPUT);
  pinMode(pinB, OUTPUT);
  pinMode(btnKey, INPUT);
  vault.attach(pinServo);
  vault.write(0);
}

void loop() {
  // ---- 第 1 關：有人在門前 ----
  if (readCM(pinTrig, pinEcho) > nearCM) {
    showLine(0, "STEP CLOSER");
    showLine(1, "");
    setColor(LOW, LOW, LOW);
    input = "";
    delay(200);
    return;
  }

  setColor(LOW, LOW, HIGH);
  showLine(0, "VAULT PIN:");
  String mask = "";
  for (int i = 0; i < input.length(); i++) mask += "*";
  showLine(1, mask);

  char k = keypad.getKey();
  if (!k) return;

  if (k >= '0' && k <= '9' && input.length() < 4) {
    input += k;
  } else if (k == '*') {
    input = "";
  } else if (k == 'D') {
    showLine(0, "TODAY OK: " + String(okToday));
    showLine(1, "TODAY FAIL: " + String(failToday));
    delay(3000);
  } else if (k == '#') {
    // ---- 第 2 關：密碼 ----
    if (input != password) {
      fail("PIN");
    } else {
      // ---- 第 3 關：限時轉鑰匙 ----
      bool turned = false;
      unsigned long start = millis();
      while (millis() - start < keyWindow * 1000UL) {
        int left = keyWindow - (millis() - start) / 1000;
        showLine(0, "TURN KEY NOW");
        showLine(1, String(left) + "s");
        if (digitalRead(btnKey) == HIGH) { turned = true; break; }
        delay(50);
      }
      if (!turned) {
        fail("KEY");
      } else {
        fails = 0;
        okToday++;
        showLine(0, "ACCESS GRANTED");
        showLine(1, "DOOR OPEN 5s");
        setColor(LOW, HIGH, LOW);
        tone(pinBuzzer, 1319, 400);
        vault.write(90);
        delay(5000);
        vault.write(0);
      }
    }
    input = "";
  }
}
```

**情境思考**：

1. 附上你的 18 隻腳位分配表。哪些元件你放在 A 腳？為什麼這樣安排？
2. 「密碼＋實體鑰匙」叫做**雙重驗證**。為什麼只有密碼不夠安全？生活中還有哪些雙重驗證的例子？
3. 💼 銀行稽核要求「每次開金庫都要知道是誰開的」。目前的系統做得到嗎？要怎麼改？（提示：專案一的員工編號）

---

### 🏆 Lv.6 BOSS 關：畢業機關提案

**情境**：這是 Arduino 單元的**畢業作品**。你的顧問團隊要從零開始，為一個真實場域設計一套完整的智慧系統。

> **案例 A：自助圖書借還站**
> 讀者用鍵盤輸入借書證號碼與書號，系統確認後開啟還書口（伺服）；超音波偵測書本放入；LCD 顯示借閱紀錄；逾期時 RGB 亮紅並提示罰金。
>
> **案例 B：社區包裹智慧櫃**
> 物流人員輸入住戶門牌號碼，對應的櫃門（伺服）打開放包裹；系統產生 4 碼取件碼；住戶輸入取件碼取件；冷藏格用 TMP36 監控溫度。
>
> **案例 C：自訂情境**
> 自己找一個需要「輸入」的生活或職場場域（例如：餐廳點餐機、停車繳費機、醫院報到機、工廠換線設定面板）。

**最低規格**：

- 必須使用矩陣鍵盤＋LCD
- 另外至少使用**三類**前面學過的元件（例如：感測器、馬達、聲光）
- 至少一個安全或防呆機制（錯誤鎖定、雙重確認、安全連鎖等）
- 總共使用 12 隻以上腳位
- 程式可以從本週或前幾週的範例改寫，說明你是從哪裡改來的、改了哪些設定

**繳交內容**：

1. **需求分析**：這個系統解決什麼問題？有哪些使用者角色？（3～5 句）
2. **操作流程圖**：每一種使用者，從開始到結束按了什麼、LCD 顯示什麼
3. **腳位規劃表**：Pin 2～13、A0～A5 逐一列出，說明鍵盤接幾行、LCD 用哪一版、L293D 用哪種接法（如果有）
4. **Tinkercad 實作**：電路截圖＋各元件測試器截圖
5. **防呆與安全設計說明**
6. **成果短文**（150～200 字）：說明你最引以為傲的一個設計決策

**驗收標準**：

- [ ] 鍵盤＋LCD 操作流程完整，符合「操作流程設計原則」
- [ ] 腳位規劃合理，有說明省腳位的設計選擇
- [ ] 防呆或安全機制確實有效
- [ ] 每條燈線有 220Ω，按鈕與光敏電阻有 10kΩ，其他元件沒有多接電阻
- [ ] 麵包板依功能分區

---

## 🎓 Arduino 單元總整理

恭喜你完成了 Arduino 基礎元件的全部內容！在進入第 13～16 週的整合階段前，用下面兩張表做一次完整盤點。

### 📋 全元件配線速查表（建議印出來貼在桌上）

| 元件 | 腳數 | 接 Arduino 幾隻 | 要哪種腳 | 需要電阻嗎 | 最容易犯的錯 |
| --- | --- | --- | --- | --- | --- |
| 單色 LED | 2 | 1 | 一般；要調光接 `~` | 220Ω | 長短腳接反 |
| RGB LED（共陰極） | 4 | 3 | 一般；要混色接 `~` | 每色 220Ω | 有伺服、蜂鳴器時接到被借走的 `~` 腳 |
| 按鈕 | 4 | 1 | 一般 | 10kΩ 下拉 | 訊號線接錯側 |
| 旋鈕 | 3 | 1 | A 腳 | 不用 | 中間腳沒接 A 腳 |
| 蜂鳴器 | 2 | 1 | 一般 | 不用 | 會借走 3、11 的調光 |
| 光敏電阻 | 2 | 1 | A 腳 | 10kΩ | 忘了 10kΩ |
| TMP36 | 3 | 1 | A 腳 | 不用 | 平面方向接反 |
| 伺服馬達 | 3 | 1 | 一般 | 不用 | 會借走 9、10 的調光 |
| HC-SR04 | 4 | 2 | 一般 | 不用 | Trig、Echo 接反 |
| L293D＋直流馬達 | 16＋2 | 1～3（每顆馬達） | EN 要調速接 `~` | 不用 | 電源腳 8、16 或 GND 漏接 |
| LCD 平行版 | 16 | 6 | 一般 | 不用 | RW 沒接 GND |
| LCD I2C 版 | 4 | 2 | 固定 A4、A5 | 不用 | SDA、SCL 接反 |
| 4×4 鍵盤 | 8 | 7～8 | 一般 | 不用 | 行列接反 |

### 📋 十二週省腳位技巧總整理

| 技巧 | 出處 | 省多少 |
| --- | --- | --- |
| 只要亮滅的燈不佔 `~` 腳 | 第 3、4 週 | 保留 `~` 腳給需要的元件 |
| A0～A5 可以借來當一般腳 | 第 4 週 | 最多多 6 隻 |
| 只用全亮全滅顏色的 RGB 接一般腳 | 第 5、7 週 | 不搶 `~` 腳 |
| L293D 接法 A、B 只佔 1 隻 | 第 10 週 | 每顆馬達省 1～2 隻 |
| LCD 改用 I2C 版 | 第 11 週 | 省 4 隻 |
| 鍵盤只接 3 行 | 第 12 週 | 省 1 隻 |

### 📋 技能自評表（1～5 分）

| 週次 | 核心能力 | 自評 |
| --- | --- | --- |
| 第 1～2 週 | LED、按鈕接線；狀態改變偵測 | ___ / 5 |
| 第 3～4 週 | `~` 腳與 PWM；旋鈕與換算 | ___ / 5 |
| 第 5～6 週 | 蜂鳴器、光敏電阻、TMP36；警報處理方式 | ___ / 5 |
| 第 7 週 | 伺服馬達；安全連鎖 | ___ / 5 |
| 第 8 週 | 需求分析、模組化、QA 測試、提案簡報 | ___ / 5 |
| 第 9 週 | 超音波；安裝位置換算 | ___ / 5 |
| 第 10 週 | L293D 四種接法；緩啟動 | ___ / 5 |
| 第 11 週 | LCD 兩種版本；看板設計 | ___ / 5 |
| 第 12 週 | 鍵盤；操作流程設計 | ___ / 5 |
| 全學期 | **腳位規劃與資源取捨** | ___ / 5 |

> 自評 3 分以下的週次，建議在第 13 週前回頭重做該週的專案一、二。

### 銜接第 13～16 週

| 你學過的 | 接下來會用在 |
| --- | --- |
| 省腳位技巧（第 10～12 週） | 第 13 週：位移暫存器——用 3 隻腳控制 8 顆燈，把省腳位推到極致 |
| L293D「以小控大」（第 10 週） | 第 14 週：繼電器——用 Arduino 控制家電等大功率設備 |
| 光敏電阻、超音波的「偵測有人」（第 5、9 週） | 第 14 週：PIR 人體感測器 |
| 期中專題的模組化與優先權（第 8 週） | 第 15～16 週：期末整合大專題 |

---

## 配線除錯站

| 現象 | 先檢查這裡 | 修正方式 |
| --- | --- | --- |
| 按鍵完全沒反應 | 鍵盤 8 條線是否都接上；程式腳位是否和實際一致 | 用鍵盤測試器逐鍵確認 |
| 按 `2` 卻印出 `4` | 行線（C）和列線（R）整組接反 | 把 rowPins、colPins 兩組對調 |
| 某一整排按鍵都沒反應 | 那一條列線或行線沒接好 | 檢查對應的 R 或 C 腳 |
| 按 A～D 沒反應 | 是否只接了 3 行（C4 沒接） | 需要 A～D 時補接 C4 |
| 按一下卻輸入兩次 | 程式是否在 `loop()` 裡重複讀取 | 本週程式已處理；若自行修改要保留 `getKey()` 寫法 |
| LCD 沒有字 | I2C 版 SDA→A4、SCL→A5；是否有 `setBacklight(1)` | 用 LCD 測試器確認 |
| 加了 I2C LCD 後，原本接在 A4、A5 的元件失效 | A4、A5 已被 LCD 佔用 | 把該元件改接其他腳 |
| 腳位不夠 | 鍵盤是否可以只接 3 行？LCD 是否改用 I2C？L293D 是否能改用省腳接法？ | 參考「十二週省腳位技巧總整理」 |
| 程式無法上傳 | 設定區是否還有 `?` 沒填 | 填完所有腳位（只接 3 行時 C4 也要填一隻空腳） |

---

## 成果繳交與評分

### 範例專案繳交

每個專案各自建立一個 Tinkercad 檔案，命名格式：`W12-專案編號-學號`（例如 `W12-P3-B12345678`）。

| 繳交項目 | 專案一 | 專案二 | 專案三 | 專案四 |
| --- | --- | --- | --- | --- |
| Tinkercad 分享連結 | ✅ | ✅ | ✅ | ✅ |
| 操作流程圖 | ✅ | ✅ | ✅ | ✅ |
| 腳位規劃表 | ✅ | ✅ | ✅ | ✅ |
| 完成 Step 3 的電路截圖 | ✅ | ✅ | ✅ | ✅ |
| 情境思考題作答 | ✅ | ✅ | ✅ | ✅ |

> 🎯 **最低完成標準**：專案一、二完成 Step 3，專案三、四至少完成 Step 2。

### 練習題與單元總整理繳交

| 項目 | 繳交內容 | 建議定位 |
| --- | --- | --- |
| Lv.1～Lv.2 | Tinkercad 連結＋操作流程＋情境思考題 | 課堂暖身，全班必做 |
| Lv.3～Lv.4 | ＋腳位規劃表 | 課後作業，必做 |
| Lv.5 | ＋18 隻腳位分配表＋麵包板擺放截圖 | 選做加分 |
| Lv.6 畢業機關 | 完整提案（需求、流程、腳位、實作、安全設計、成果短文） | 單元成績，可分組 |
| 單元總整理 | 技能自評表 | 全班必交 |

### 評分比重建議

| 項目 | 比重 | 說明 |
| --- | --- | --- |
| 電路正確性 | 30% | 鍵盤、LCD 與其他元件正確運作 |
| 腳位規劃 | 25% | 正確運用省腳位技巧，能說明取捨 |
| 操作流程設計 | 20% | 符合操作流程設計原則，有防呆 |
| 情境思考題 | 15% | 尤其是 💼 管理面向論述 |
| 麵包板擺放 | 10% | 鍵盤、LCD 線路整齊，分區清楚 |

### 課堂時間建議（3 節課）

| 時段 | 內容 |
| --- | --- |
| 第 1 節前 30 分鐘 | 認識矩陣鍵盤、3 行省腳接法、操作流程設計原則、鍵盤測試器示範 |
| 第 1 節後段 | 專案一（Step 3 補接第 4 行是重點） |
| 第 2 節 | 專案二、專案三（分組進行） |
| 第 3 節前段 | 專案四 |
| 第 3 節後段 | Arduino 單元總整理（自評表、速查表）；練習題 Lv.1～Lv.2 當堂完成，其餘帶回家 |

---

## 📖 本週詞彙表

| 名稱 | 白話解釋 |
| --- | --- |
| 4×4 矩陣鍵盤 | 16 顆按鍵排成 4 列 4 行，只用 8 條線 |
| 行列掃描 | Arduino 輪流檢查每一條線，找出哪顆鍵被按下 |
| 列線 R1～R4／行線 C1～C4 | 鍵盤的 8 隻腳 |
| 3 行省腳接法 | 不需要 A～D 鍵時不接 C4，只佔 7 隻腳 |
| `getKey()`／`waitKey()` | 讀取按鍵；`waitKey()` 會一直等到有人按 |
| 操作流程 | 使用者每一步按什麼、看到什麼的完整順序 |
| 防呆（Poka-yoke） | 讓人不容易犯錯、犯錯也能被發現的設計 |
| 錯誤鎖定 | 連續輸入錯誤達上限，暫時停止接受輸入 |
| 雙重驗證 | 同時需要「知道的東西」（密碼）和「擁有的東西」（鑰匙）才能通過 |
| 電子標籤揀貨（Pick-to-Light） | 用燈號指引揀貨員去哪個儲位、拿幾個 |
| 良率（Yield） | 合格品占總檢驗數的比例 |

---

## 反思與延伸討論

1. 本週每個專案都要先畫「操作流程圖」。這和你在其他管理課程中畫過的**流程圖、SOP、顧客旅程地圖**有什麼共通點？為什麼「先想清楚使用者怎麼操作」比「先接線」更重要？
2. 專案三、Lv.5 的「錯誤鎖定」提高了安全性，但也可能讓真正的使用者被鎖在門外。請以提款機或手機解鎖為例，討論「安全」和「方便」之間的取捨，以及企業如何決定這條線要畫在哪裡。
3. 回顧十二週，你從「每個元件都要一隻腳」，到學會借 A 腳、選 L293D 接法、改用 I2C、只接 3 行鍵盤。這些「在有限資源下達成需求」的思考，和管理中的**資源規劃、產能配置**有什麼相似之處？

---

## 本週檢核清單

- [ ] 能正確接上 4×4 鍵盤，並用鍵盤測試器確認 16 顆鍵
- [ ] 能依需求決定鍵盤接 3 行或 4 行
- [ ] 能在鍵盤＋LCD 的系統中，選擇適合的 LCD 版本並說明理由
- [ ] 能畫出操作流程圖，並應用操作流程設計原則（提示、回饋、取消、確認、遮蔽、錯誤說明）
- [ ] 能完成 18 隻腳全部用滿的腳位規劃
- [ ] 完成 4 個獨立專案，並各自繳交 Tinkercad 檔案
- [ ] 完成練習題 Lv.1～Lv.4，挑戰 Lv.5 與 Lv.6
- [ ] 完成 Arduino 單元技能自評表

---

## 下週預告

恭喜完成 Arduino 基礎元件階段！第 13 週進入整合階段，第一個新元件是**位移暫存器（74HC595）與七段顯示器**，打造「產線數字看板升級站」。你會看到省腳位技巧的極致：**只用 3 隻腳就能控制 8 顆燈**。第 14 週接著學繼電器與 PIR 人體感測器，第 15～16 週則把整學期所學收斂成期末整合大專題。

---

[⬅ 回目錄](../README.md) | [⬅ 上一週：第 11 週](week-11.md) | [下一週：第 13 週 — 產線數字看板升級站 ➡](week-13.md)
