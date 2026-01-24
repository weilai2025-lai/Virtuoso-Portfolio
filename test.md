# Lecture 3 — CMOS Transistors（階段性筆記）

> 本筆記整理自 Lecture 3 目前為止的內容，並融合課堂逐字稿導讀、物理直覺、以及常見迷思澄清。
> 目標是：**不用背公式，也能清楚判斷 MOS 的狀態與行為**。

---

## 1. Intrinsic Silicon（本質矽）

- 矽（Si）為 **第四族元素**，最外層有 **4 個價電子**。
- 在晶體中，每個 Si 會與 **4 個鄰居形成共價鍵**。
- 這些專價鍵讓「最外層看起來有 8 個電子」，但：
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
- 物理上可定義、理論上可推導入
- 工程上：
  - 由 **量測 $I_d - V_g$ 曲線** 萃取

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

這個正號的物理意義：
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

> **關鍵觀察：** NMOS 導通時， $V_{ds}$ 與 $I_{ds}$ 會同時是正的。

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

---

## 11. CMOS Inverter 的穩態理解（總結）

### Vin = High
- NMOS on, PMOS off
- n-channel 將輸出拉到 GND
- Vout ≈ 0

### Vin = Low
- PMOS on, NMOS off
- p-channel 將輸出拉到 VDD
- Vout ≈ VDD

**關鍵句：**
> 輸出電位由「哪一顆 MOS 的通道導通」決定，
> body / well 與 PN 接面負責隔離與正確偏壓。

---

## 13. Bulk Charge Model（Page 12）— 用電容近似通道電荷的第一步

> **開宗明義（先說這個模型在幹嘛）**  
> Bulk charge model 的目的，不是在判斷 MOS「有沒有打開」，  
> 而是在 **MOS 已經打開（strong inversion）之後**，  
> 用一個 *好算但不失直覺* 的方式，估算：
>
> 👉 **整條 channel 裡「大約有多少反轉電荷」**  
>
> 因此它回答的是「多少（how much）」的問題，  
> 而不是你前面已經熟悉的「有沒有（on/off）」問題。  
> 這也是它和 $V_{gs} \ge V_t$ 那套開關判斷**層級不同**的地方。

---

### 13.1 模型骨架：先把 Q = C · V 立起來

本頁從

$$Q_{\text{channel}} = C \cdot V$$

出發，其中把 gate–oxide–channel 視為**平行板電容**。

這裡的想像非常具體：

- 上方：gate（金屬）
- 中間：oxide（SiO₂，絕緣）
- 下方：channel（反轉層，導電）

只要你接受這個幾何結構， Q = C · V 就是最自然的第一步。

---

### 13.1.1 為什麼會有 $C_{ox}$？——「材料與距離」的係數

單位面積的氧化層電容定義為：

$$C_{ox} = \frac{\varepsilon_{ox}}{t_{ox}}$$

這一項只跟兩件事有關：

- $\varepsilon_{ox}$：氧化層材料的介電常數 → **材料本身有多容易被極化**
- $t_{ox}$：氧化層厚度 → **gate 跟 channel 隔多遠**

因此 $C_{ox}$ 的物理意義可以直接讀成：

> **「每一單位面積，gate 對 channel 的電容有多強」**

這是一個**純材料 + 製程**決定的係數，  
與 MOS 有沒有打開、電壓多大 **完全無關**。

---

### 13.1.2 為什麼要乘上 W × L？——「面積真的會線性放大電容」

gate 在上方覆蓋整條 channel，而 channel 的幾何尺寸為：

- 寬度：W（橫向，決定一次能排幾條電荷）
- 長度：L（縱向，從 source 到 drain）

因此 gate 覆蓋的有效面積約為：

$$A \approx W \cdot L$$

對平行板電容而言：

> **面積放大幾倍，能儲存的電荷就放大幾倍**

所以整體 gate 電容自然寫成：

$$C = C_g = C_{ox} \cdot W L = \frac{\varepsilon_{ox}WL}{t_{ox}}$$

你可以這樣記這一行：

- $C_{ox}$：**每單位面積的能力（強度）**
- WL：**你實際用掉多少面積**
- $C_{g}$：**這顆 MOS gate 對 channel 的總耦合能力**

---

### 13.1.3 小總結： $C_{g}$ 在這一頁「只負責一件事」

在 Page 12， $C_{g}$ **沒有任何動態、沒有任何非線性角色**，  
它只做一件事：

> 👉 把「gate 的有效控制電壓」  
> 轉成「channel 裡能被拉出來的電荷量」

也就是：

$$Q_{\text{channel}} = C_{g} \cdot V$$

---

### 13.2 你一開始最卡的點：V 到底是哪個電壓？為什麼是 $V_{gs} - V_{ds}/2$？

#### (a) 先抓住事實：channel 電位沿著 x 方向會變
當 $V_{ds} \neq 0$ 時，通道由 source 到 drain 的電位：

- 左端（source 端）約為 $V_{s}$
- 右端（drain 端）約為 $V_{d}$

因此 channel **不是等電位**，  
gate-to-channel 電壓 $V_{gc}$ 也就不是單一數值。

---

#### (b) 中點近似的真正幾何意義（幫你之後快速回想）

定義：

$$V_{\text{ch,mid}} \approx \frac{V_s + V_d}{2}$$

這個 $V_{ch,mid}$ 指的是：

> **channel 在「幾何正中央」那一點的電位**

它的位置是：

- 左右方向：source 與 drain 的正中間  
- 上下方向：在 gate 正下方、channel 內部

而 $V_{g}$ 則是：

- gate 那整片金屬的電位
- 對整片 gate 來說是同一個數值

所以在「同一個橫向位置（中點）」上：

$$V_{gc,\text{mid}} = V_g - V_{\text{ch,mid}}$$

這是一個**純上下方向**的電壓差，  
不是 gate 去「碰」source 或 drain 的左右概念。

---

(c) 改寫成端點電壓後的結果（完整推導，不省略）
從中點定義開始：

$$V_{\text{ch,mid}} \approx \frac{V_s + V_d}{2}$$

以及中點的 gate-to-channel 電壓定義：

$$V_{gc,\text{mid}} = V_g - V_{\text{ch,mid}}$$

將第一式代入第二式：

$$V_{gc,\text{mid}} = V_g - \frac{V_s + V_d}{2}$$

接著把平均項改寫成「以 V_s 為基準」的形式（這一步是關鍵）：

$$
\begin{aligned}
\frac{V_s + V_d}{2} &= \frac{V_s + (V_s + (V_d - V_s))}{2} \\
&= \frac{2V_s + (V_d - V_s)}{2} \\
&= V_s + \frac{V_d - V_s}{2}
\end{aligned}
$$

將上式再代回 $V_{gc,\text{mid}}$ ：

$$
\begin{aligned}
V_{gc,\text{mid}} &= V_g - \left( V_s + \frac{V_d - V_s}{2} \right) \\
&= (V_g - V_s) - \frac{V_d - V_s}{2}
\end{aligned}
$$

最後用端點電壓定義：
- $V_{gs} = V_g - V_s$
- $V_{ds} = V_d - V_s$

代入得：

$$V_{gc,\text{mid}} = V_{gs} - \frac{V_{ds}}{2}$$

這一項的直覺可以讀成：

> **「在平均意義下，gate 對 channel 還剩多少控制力」**

---

### 13.3 為什麼還要扣 $V_{t}$？以及最常見的誤會怎麼解

這裡一定要分清楚兩個層級（也是你一開始混在一起的地方）：

- **開關層級（有沒有通道）**  
  $V_{gs} \ge V_t$  
  → 判斷 MOS 是否打開（yes / no）

- **電荷層級（通道裡有多少反轉電荷）**  
  $V_{t}$ 那一段 gate 電壓，只是「剛好把反轉層建立起來的成本」，  
  **不會增加反轉層的厚度或電荷量**。

因此在算電荷時，有效電壓要寫成：

$$V = V_{gc} - V_t \approx \left( V_{gs} - \frac{V_{ds}}{2} \right) - V_t$$

**關鍵澄清（避免之後再卡一次）：**

> 這不是在說  
> $V_{gs} = V_{gs} - V_{ds}/2$  
> 而是在說：
>
> - $V_{gs}$：用來判斷「MOS 有沒有開」
> - $V_{gs} - V_{ds}/2$：用來估算「平均 gate 控制力」
> - 再扣掉 $V_{t}$：得到「真正能增加反轉電荷的部分」

---

### 13.4 邊界條件：Bulk charge model 什麼時候才「站得住腳」？

本頁的中點平均近似隱含一個很重要的前提：

> **$V_{gs} - V_{t}$ 要夠大**，  
> 使得 channel 大部分區域都能維持 strong inversion。

因此：

- 當 $V_{gs}$ 只比 $V_{t}$ 大一點點時  
  $V = (V_{gs} - V_{ds}/2) - V_{t}$ 很容易被算成負值
- 這不代表「實際完全沒有電流」
- 更合理的解讀是：  
  **這個「整條 channel 平均」的模型在該操作點開始失效**

---

### 13.5 Bulk Charge Model 核心概念總結

> **核心思路：**  
> 在 strong inversion 前提下，用平行板電容模型  
> 把 gate 的「平均有效控制電壓」  
> 轉成「channel 裡的總反轉電荷量」。

## 14. Carrier velocity（載子速度）— 電子在通道裡是怎麼被拉著走的？

> **本節在做什麼（先對齊角色）**  
> 在前一節（Bulk charge model）中，我們已經用電容近似，
> 估算出「通道裡大約有多少反轉電荷 $Q_{channel}$」。
>
> 接下來要回答的問題是：
>
> 👉 **這些電荷是怎麼移動的？移動得快還是慢？**
>
> 因此，本節只討論「載子的運動行為」，  
> 尚未引入電流公式。

---

### 14.1 橫向電場 E：推電子往前的來源

當 NMOS 的 drain 與 source 之間存在電壓差 $V_{ds}$ 時，
在通道內沿著 **source → drain** 方向會形成一個**橫向電場**。

在這裡採用的是平均近似：

$$E \approx \frac{V_{ds}}{L}$$

其中各符號的中文意義為：

- E：橫向電場（electric field） → **每單位通道長度上，電子所感受到的平均推力大小**
- $V_{ds}$：汲極對源極電壓 → **沿著通道施加的總電壓差**
- L：通道長度（channel length） → **source 到 drain 的距離**

**直覺理解：**

> 把整個 $V_{ds}$ 平均分攤在長度 L 上，  
> 得到「每單位通道長度，電位下降多少」，  
> 也就是「每單位長度，對電子施加多大的推力」。

這裡的 E 是**平均電場**，  
並不表示通道中每一點的電場都完全相同。

---

### 14.2 載子速度 v：電子實際跑多快？

電子在電場作用下會產生漂移運動，其平均漂移速度定義為：

$$v = \mu \cdot E$$

各符號的物理意義為：

- v：載子速度（carrier velocity） → **電子在通道裡沿著 source → drain 移動的平均速度**
- $\mu$：遷移率（mobility） → **材料允許電子移動快慢的能力係數**
- E：橫向電場 → **推動電子前進的驅動來源**

**物理直覺：**

> 電場越強，電子被拉得越用力；  
> 但電子實際能跑多快，取決於材料本身（ $\mu$ ）。

---

### 14.3 隱含假設：低電場（low-field）載子運動模型

在本節中直接使用

$$v = \mu \cdot E$$

且將 $\mu$ 視為常數，隱含了一個重要前提：

> **低電場假設（low-field regime）**

其意義為：

-