# Lecture 3 — CMOS Transistors（階段性筆記）

> 本筆記整理自 Lecture 3 目前為止的內容，並融合課堂逐字稿導讀、物理直覺、以及常見迷思澄清。
> 目標是：**不用背公式，也能清楚判斷 MOS 的狀態與行為**。

---

## 1. Intrinsic Silicon（本質矽）

- 矽（Si）為 **第四族元素**，最外層有 **4 個價電子**。
- 在晶體中，每個 Si 會與 **4 個鄰居形成共價鍵**。
- 這些共價鍵讓「最外層看起來有 8 個電子」，但：
  - 電子都被**鍵結綁住**
  - **沒有自由載子**（free carriers）

**結論：**
> 純矽（intrinsic silicon）幾乎不導電。

---

## 2. Doping（摻雜）與載子

### 2.1 P-type（受體摻雜）

- 用 **第三族元素**（例如 Boron, B）取代 Si
- 少一個電子 → 形成 **電洞（hole）**
- Boron 稱為 **acceptor**
- 多數載子：**hole**

### 2.2 N-type（施體摻雜）

- 用 **第五族元素**（例如 Phosphorus, Arsenic）取代 Si
- 多一個電子 → 形成 **自由電子**
- 這些原子稱為 **donor**
- 多數載子：**electron**

---

## 3. MOS 結構的物理直覺（以 NMOS 為例）

- 基板（body）是 **p-type substrate**，透過 **p+ tap 接地（GND）**
- Source / Drain 為 **n+ diffusion**
- n+ 與 p-substrate 之間形成 **PN junction（反向偏壓）**，產生耗盡區

### Gate 加正電時（$V_g > 0$）：

1. 吸引電子到 Si–SiO₂ 表面
2. 先推走 hole → **depletion（耗盡）**
3. 再吸引電子 → **inversion（反轉）**
4. 形成 **n-channel**，連接 source 與 drain

**關鍵觀念：**
> 輸出電壓是由「哪一顆 MOS 的通道導通」決定，
> 耗盡區的角色是隔離與防止接面導通，而不是直接設定電壓。

![CMOS 橫截面示意圖](assets/cmos_cross_section.png)

---

## 4. Accumulation / Depletion / Inversion

### Accumulation（累積）
- **NMOS**：$V_g < 0$ ｜ **PMOS**：$V_g > 0$ 
- Gate 電壓讓多數載子聚集在表面
- 但 **沒有形成導通通道**

### Depletion（耗盡）
- **NMOS**：$0 < V_g < V_{th}$ ｜ **PMOS**：$V_{th} < V_g < 0$ 
- Gate 電壓推走多數載子
- 表面留下固定離子 → 耗盡區

### Inversion（反轉）
- **NMOS**：$V_g > V_{th}$ ｜ **PMOS**：$V_g < V_{th}$ 
- Gate 電壓夠大，吸引少數載子形成反轉層
- **形成可導通的 channel**

---

## 5. Threshold Voltage（$V_{th}$）

- **$V_{th}$ = 剛好在表面形成穩定反轉層的 Gate 電壓**
- 物理上可定義、理論上可推導
- 工程上：
  - 由 **量測 $I_d – V_g$ 曲線** 萃取

**重要直覺：**
> Gate 並不是一加電就能控制通道，
> 必須先「付掉 $V_{th}$ 這個入場費」。

---

## 6. Terminal Voltages（端點電壓定義）

- $V_{gs} = V_g - V_s$ 
- $V_{gd} = V_g - V_d$ 
- $V_{ds} = V_d - V_s = V_{gs} - V_{gd}$ 

### Source / Drain 的定義

- 結構上對稱
- **由電壓決定角色**：
  - NMOS：電壓較低者 = Source
  - PMOS：電壓較高者 = Source

---

## 7. 操作區域（Operating Regions, NMOS）

### 7.1 Cutoff
- $V_{gs} < V_{th}$ 
- 無反轉層，MOS 關閉

### 7.2 Linear（Triode）

$$
\begin{cases}
V_{gs} > V_{th} \\
V_{ds} < V_{gs} - V_{th}
\end{cases}
$$

**物理意義：**
- 通道「整條都存在」
- Gate 在 Source 與 Drain 端都壓得住
- MOS 行為近似為 **受控電阻**

### 7.3 Saturation

$$
\begin{cases}
V_{gs} > V_{th} \\
V_{ds} \ge V_{gs} - V_{th}
\end{cases}
$$

**物理意義：**
- Drain 端通道被拉斷（pinch-off）
- 再增加 $V_{ds}$ ，電流不再明顯增加

---

## 8. $V_{ds}$ 的關鍵直覺（重要）

> **$V_{gs}$ 決定「有沒有路」**  
> **$V_{ds}$ 決定「通道會不會在 Drain 端被 pinch-off（夾斷）」**

- $V_{ds}$ 是加在 **整條通道上的電位差**
- 分界條件中的 $- V_{th}$ 代表：
  - Gate 必須先超過 $V_{th}$ 
  - 剩下的 $V_{gs} - V_{th}$ 才是真正能對抗 Drain 端電位的有效控制力

**常見誤解澄清：**

| 誤解 | 實際情況 |
|------|----------|
| 通道真的斷成兩截？ | ❌ 不是。通道只是在 Drain 端「收縮到消失」，電流還是在流 |
| 電流停止？ | ❌ 不是。電子靠電場漂移穿越那一小段耗盡區 |

---

## 9. 電子流 vs. 傳統電流（符號觀念）

- **物理載子角度**：
  - NMOS：電子由 Source → Drain
  - PMOS：電洞由 Source → Drain

- **電路定義角度（$I_d$, $I_{ds}$）**：
  - 一律定義為 **Drain → Source 為正**

**結論：**
> 談物理機制 → 用電子 / 電洞想  
> 寫公式、算電流 → 用傳統正電流定義

---

## 10. $V_{ds}$ 與 $I_{ds}$ 正負號的實際對齊（CMOS Inverter 實例）

> 本節以 **實際電壓數字** ，將 $V_{ds}$ 與 $I_{ds}$ 的正負號一次對齊說清楚。

### 10.0 三個不能動的定義（錨點）

在開始之前，先鎖死這三個定義：

1. **$I_{ds}$ 定義** ：傳統正電流，Drain → Source 為正
2. **$V_{ds}$ 定義** ：$V_{ds} = V_d - V_s$ 
3. **Source 定義** ：
   - NMOS：電壓較低的一端是 Source
   - PMOS：電壓較高的一端是 Source

---

### 10.1 PMOS 充電階段（0 → 1）

#### 設定條件
- $V_{DD} = 1.1 \text{ V}$ 
- PMOS source 接 $V_{DD}$ 
- PMOS n-well 接 $V_{DD}$ 
- Gate = 0 （PMOS 打開）

#### 切換瞬間（真正有電流的時候）

假設某一個切換瞬間：
- Source：$V_s = 1.1 \text{ V}$ 
- Drain（輸出端，還在被充電中）：$V_d = 0.6 \text{ V}$ 

#### $V_{ds}$ 的正負號

$$V_{ds} = V_d - V_s = 0.6 - 1.1 = -0.5 \text{ V}$$

✅ **$V_{ds}$ 是負的**

這個負號的物理意義：
> Drain 的電位比 Source 低
> → 這正是 PMOS 正在「由 Source 往 Drain 充電」的狀態

#### $I_{ds}$ 的正負號

真實物理行為：
- 正電流（電洞）方向：Source → Drain

但 $I_{ds}$ 的定義方向是：
- Drain → Source 為正

所以此時：$I_{ds} < 0$ 

✅ **$I_{ds}$ 是負的**

> **關鍵觀察：** $V_{ds}$ 與 $I_{ds}$ 在 PMOS 導通時，符號會「同時為負」。
> 這不是巧合，是因為電壓與電流的參考方向一致，符合被動符號慣例（passive sign convention）。

#### 充電完成（穩態）

充完電後：
- $V_d \rightarrow 1.1 \text{ V}$ ， $V_s = 1.1 \text{ V}$ 
- $V_{ds} = 1.1 - 1.1 = 0$ （沒有壓差）
- $I_{ds} \rightarrow 0$ （電流停止）

---

### 10.2 NMOS 放電階段（1 → 0）

#### 設定條件
- $V_{DD} = 1.1 \text{ V}$ 
- NMOS source 接 GND（0 V）
- NMOS body（p-substrate）接 GND
- Gate = $1.1 \text{ V}$ （NMOS 打開）
- 輸出端原本被 PMOS 拉到高電位，準備放電

#### 切換瞬間（真正有電流的時候）

假設某一個放電瞬間：
- Source（接地）：$V_s = 0 \text{ V}$ 
- Drain（輸出端，還沒放完電）：$V_d = 0.6 \text{ V}$ 

#### $V_{ds}$ 的正負號

$$V_{ds} = V_d - V_s = 0.6 - 0 = +0.6 \text{ V}$$

✅ **$V_{ds}$ 是正的**

這個負號的物理意義：
> Drain 的電位比 Source 高
> → 電場方向會把電子往 Drain 拉
> → 但電子是負電，實際移動方向是 Source → Drain 的反方向
> → 這正是 NMOS 正在把電容往地放電

#### $I_{ds}$ 的正負號

真實物理行為：
- 電子方向：Source → Drain
- 傳統正電流方向：Drain → Source

對照 $I_{ds}$ 定義（正方向 = Drain → Source）：

$I_{ds} > 0$ 

✅ **$I_{ds}$ 是正的**

> **關鍵觀察：** NMOS 導通時，$V_{ds}$ 與 $I_{ds}$ 會同時是正的。

#### 放電完成（穩態）

放電完成後：
- $V_d \rightarrow 0 \text{ V}$ ， $V_s = 0 \text{ V}$ 
- $V_{ds} = 0 - 0 = 0$ （沒有壓差）
- $I_{ds} \rightarrow 0$ （電流停止）

---

### 10.3 總結對照表

| 元件 | 切換方向 | $V_{ds}$ | $I_{ds}$ | 有電流的時機 |
|------|----------|----------|----------|---------------|
| NMOS | 1 → 0（放電） | 正 | 正 | $V_{out}$ 尚未到 0 |
| PMOS | 0 → 1（充電） | 負 | 負 | $V_{out}$ 尚未到 $V_{DD}$ |

---

### 10.4 總結段落

**PMOS 版本：**
> 以 PMOS 為例，在輸出由 0 充電至 $V_{DD}$ 的切換期間，Source 端維持在 $V_{DD}$ ，而 Drain 端電腳尚未上升完成，因此 $V_d < V_s$ ，使得 $V_{ds} = V_d - V_s < 0$ 。
>
> 此時實際正電流由 Source 流向 Drain，但由於 $I_{ds}$ 的正方向定義為 Drain → Source，因此量測到的 $I_{ds}$ 為負值。
> 當輸出電壓上升至與 Source 對齊後， $V_{ds} \rightarrow 0$ ，電流隨即消失，系統進入穩態。

**NMOS 版本：**
> 以 NMOS 為例，在輸出由 $V_{DD}$ 放電至 0 的切換期間，Source 端維持在 0 V，而 Drain 端電壓尚未下降完成，因此 $V_d > V_s$ ，使得 $V_{ds} = V_d - V_s > 0$ 。
>
> 此時電子實際由 Source 流向 Drain，而傳統正電流方向為 Drain → Source；由於 $I_{ds}$ 的正方向亦定義為 Drain → Source，因此量測到的 $I_{ds}$ 為正值。
> 當輸出電壓下降至與 Source 對齊後， $V_{ds} \rightarrow 0$ ，電流隨即消失，系統進入穩態。

---

### 10.5 一句話封印整個觀念

> **只要輸出還沒和 Source 對齊，就一定有電流；**
> **Source 是低端（NMOS）→ $V_{ds}$ 、 $I_{ds}$ 為正；**
> **Source 是高端（PMOS）→ $V_{ds}$ 、 $I_{ds}$ 為負。**