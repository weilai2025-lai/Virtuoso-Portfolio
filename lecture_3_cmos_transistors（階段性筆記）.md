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

### Gate 加正電時（Vg > 0）：

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
- **NMOS**：V<sub>G</sub> < 0 ｜ **PMOS**：V<sub>G</sub> > 0
- Gate 電壓讓多數載子聚集在表面
- 但 **沒有形成導通通道**

### Depletion（耗盡）
- **NMOS**：0 < V<sub>G</sub> < V<sub>th</sub> ｜ **PMOS**：V<sub>th</sub> < V<sub>G</sub> < 0
- Gate 電壓推走多數載子
- 表面留下固定離子 → 耗盡區

### Inversion（反轉）
- **NMOS**：V<sub>G</sub> > V<sub>th</sub> ｜ **PMOS**：V<sub>G</sub> < V<sub>th</sub>
- Gate 電壓夠大，吸引少數載子形成反轉層
- **形成可導通的 channel**

---

## 5. Threshold Voltage（Vth）

- **Vth = 剛好在表面形成穩定反轉層的 Gate 電壓**
- 物理上可定義、理論上可推導
- 工程上：
  - 由 **量測 Id–Vg 曲線** 萃取

**重要直覺：**
> Gate 並不是一加電就能控制通道，
> 必須先「付掉 Vth 這個入場費」。

---

## 6. Terminal Voltages（端點電壓定義）

- $V_{GS} = V_G - V_S$
- $V_{GD} = V_G - V_D$
- $V_{DS} = V_D - V_S = V_{GS} - V_{GD}$

### Source / Drain 的定義

- 結構上對稱
- **由電壓決定角色**：
  - NMOS：電壓較低者 = Source
  - PMOS：電壓較高者 = Source

---

## 7. 操作區域（Operating Regions, NMOS）

### 7.1 Cutoff
- $V_{GS} < V_{th}$
- 無反轉層，MOS 關閉

### 7.2 Linear（Triode）
$$
\begin{cases}
V_{GS} > V_{th} \\
V_{DS} < V_{GS} - V_{th}
\end{cases}
$$

**物理意義：**
- 通道「整條都存在」
- Gate 在 Source 與 Drain 端都壓得住
- MOS 行為近似為 **受控電阻**

### 7.3 Saturation
$$
\begin{cases}
V_{GS} > V_{th} \\
V_{DS} \ge V_{GS} - V_{th}
\end{cases}
$$

**物理意義：**
- Drain 端通道被拉斷（pinch-off）
- 再增加 Vds，電流不再明顯增加

---

## 8. Vds 的關鍵直覺（重要）

> **Vgs 決定「有沒有路」**  
> **Vds 決定「通道會不會在 Drain 端被 pinch-off（夾斷）」**

- Vds 是加在 **整條通道上的電位差**
- 分界條件中的 $-V_{th}$ 代表：
  - Gate 必須先超過 Vth
  - 剩下的 $V_{GS} - V_{th}$ 才是真正能對抗 Drain 端電位的有效控制力

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

- **電路定義角度（Id, Ids）**：
  - 一律定義為 **Drain → Source 為正**

**結論：**
> 談物理機制 → 用電子 / 電洞想  
> 寫公式、算電流 → 用傳統正電流定義

---

## 10. Vds 與 Ids 正負號的實際對齊（CMOS Inverter 實例）

> 本節以**實際電壓數字**，將 Vds 與 Ids 的正負號一次對齊說清楚。

### 10.0 三個不能動的定義（錨點）

在開始之前，先鎖死這三個定義：

1. **Ids 定義**：傳統正電流，Drain → Source 為正
2. **Vds 定義**：V<sub>DS</sub> = V<sub>D</sub> - V<sub>S</sub>
3. **Source 定義**：
   - NMOS：電壓較低的一端是 Source
   - PMOS：電壓較高的一端是 Source

---

### 10.1 PMOS 充電階段（0 → 1）

#### 設定條件
- V<sub>DD</sub> = 1.1 V
- PMOS source 接 VDD
- PMOS n-well 接 VDD
- Gate = 0（PMOS 打開）

#### 切換瞬間（真正有電流的時候）

假設某一個切換瞬間：
- Source：V<sub>S</sub> = 1.1 V
- Drain（輸出端，還在被充電中）：V<sub>D</sub> = 0.6 V

#### Vds 的正負號

$$V_{DS} = V_D - V_S = 0.6 - 1.1 = -0.5 \text{ V}$$

✅ **Vds 是負的**

這個負號的物理意義：
> Drain 的電位比 Source 低
> → 這正是 PMOS 正在「由 Source 往 Drain 充電」的狀態

#### Ids 的正負號

真實物理行為：
- 正電流（電洞）方向：Source → Drain

但 Ids 的定義方向是：
- Drain → Source 為正

所以此時：I<sub>DS</sub> < 0

✅ **Ids 是負的**

> **關鍵觀察：** Vds 與 Ids 在 PMOS 導通時，符號會「同時為負」。
> 這不是巧合，是因為電壓與電流的參考方向一致，符合被動符號慣例（passive sign convention）。

#### 充電完成（穩態）

充完電後：
- V<sub>D</sub> → 1.1 V，V<sub>S</sub> = 1.1 V
- V<sub>DS</sub> = 1.1 - 1.1 = 0（沒有壓差）
- I<sub>DS</sub> → 0（電流停止）

---

### 10.2 NMOS 放電階段（1 → 0）

#### 設定條件
- V<sub>DD</sub> = 1.1 V
- NMOS source 接 GND（0 V）
- NMOS body（p-substrate）接 GND
- Gate = 1.1 V（NMOS 打開）
- 輸出端原本被 PMOS 拉到高電位，準備放電

#### 切換瞬間（真正有電流的時候）

假設某一個放電瞬間：
- Source（接地）：V<sub>S</sub> = 0 V
- Drain（輸出端，還沒放完電）：V<sub>D</sub> = 0.6 V

#### Vds 的正負號

$$V_{DS} = V_D - V_S = 0.6 - 0 = +0.6 \text{ V}$$

✅ **Vds 是正的**

這個正號的物理意義：
> Drain 的電位比 Source 高
> → 電場方向會把電子往 Drain 拉
> → 但電子是負電，實際移動方向是 Source → Drain 的反方向
> → 這正是 NMOS 正在把電容往地放電

#### Ids 的正負號

真實物理行為：
- 電子方向：Source → Drain
- 傳統正電流方向：Drain → Source

對照 Ids 定義（正方向 = Drain → Source）：

I<sub>DS</sub> > 0

✅ **Ids 是正的**

> **關鍵觀察：** NMOS 導通時，Vds 與 Ids 會同時是正的。

#### 放電完成（穩態）

放電完成後：
- V<sub>D</sub> → 0 V，V<sub>S</sub> = 0 V
- V<sub>DS</sub> = 0 - 0 = 0（沒有壓差）
- I<sub>DS</sub> → 0（電流停止）

---

### 10.3 總結對照表

| 元件 | 切換方向 | $V_{DS}$ | $I_{DS}$ | 有電流的時機 |
|------|----------|----------|----------|---------------|
| NMOS | 1 → 0（放電） | 正 | 正 | Vout 尚未到 0 |
| PMOS | 0 → 1（充電） | 負 | 負 | Vout 尚未到 VDD |

---

### 10.4 總結段落

**PMOS 版本：**
> 以 PMOS 為例，在輸出由 0 充電至 $V_{DD}$ 的切換期間，Source 端維持在 $V_{DD}$，而 Drain 端電壓尚未上升完成，因此 $V_D < V_S$，使得 $V_{DS} = V_D - V_S < 0$。
>
> 此時實際正電流由 Source 流向 Drain，但由於 Ids 的正方向定義為 Drain → Source，因此量測到的 $I_{DS}$ 為負值。
> 當輸出電壓上升至與 Source 對齊後，V<sub>DS</sub> → 0，電流隨即消失，系統進入穩態。

**NMOS 版本：**
> 以 NMOS 為例，在輸出由 $V_{DD}$ 放電至 0 的切換期間，Source 端維持在 0 V，而 Drain 端電壓尚未下降完成，因此 $V_D > V_S$，使得 $V_{DS} = V_D - V_S > 0$。
>
> 此時電子實際由 Source 流向 Drain，而傳統正電流方向為 Drain → Source；由於 Ids 的正方向亦定義為 Drain → Source，因此量測到的 $I_{DS}$ 為正值。
> 當輸出電壓下降至與 Source 對齊後，V<sub>DS</sub> → 0，電流隨即消失，系統進入穩態。

---

### 10.5 一句話封印整個觀念

> **只要輸出還沒和 Source 對齊，就一定有電流；**
> **Source 是低端（NMOS）→ Vds、Ids 為正；**
> **Source 是高端（PMOS）→ Vds、Ids 為負。**

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

> 本段目標：把 MOS 當成「平行板電容」，用 $Q = C \cdot V$ 估算通道電荷。  
> 你當初卡住的核心是兩個：  
> 1) 為什麼會出現 $V_{gs}-V_{ds}/2$（中點近似的空間直覺）  
> 2) 為什麼還要扣 $V_t$（「有沒有通道」 vs 「通道有多少電荷」是不同層級）

---

### 13.1 模型骨架：先把 $Q = C \cdot V$ 立起來

本頁從

$$Q_{\text{channel}} = C \cdot V$$

出發，其中把 gate–oxide–channel 視為平行板電容：

- 單位面積氧化層電容：
  $$C_{ox} = \frac{\varepsilon_{ox}}{t_{ox}}$$

- gate 覆蓋面積約為 $W \cdot L$，所以整體 gate 電容（本頁用來近似 gate 對 channel 的耦合）為：
  $$C = C_g = C_{ox}WL = \frac{\varepsilon_{ox}WL}{t_{ox}}$$

**這裡 $C_g$ 的角色：**  
> 它是「比例尺」：告訴你 gate 的有效控制電壓（$V$）能拉出多少 channel charge（$Q$）。

---

### 13.2 你一開始最卡的點：$V$ 到底是哪個電壓？為什麼是 $V_{gs}-V_{ds}/2$？

#### (a) 先抓住事實：channel 電位沿著 $x$ 方向會變
當 $V_{ds}\neq 0$ 時，通道由 source 到 drain 的電位大致從 $V_s$ 漸變到 $V_d$。  
因此 gate-to-channel 電壓 $V_{gc}$ 不是單一值，而是會隨位置改變。

#### (b) 老師採用的近似：用「channel 中點」代表平均狀態
定義中點 channel 電位

$$V_{\text{ch,mid}} \approx \frac{V_s + V_d}{2}$$

中點的 gate-to-channel 電壓為

$$V_{gc,\text{mid}} = V_g - V_{\text{ch,mid}} = V_g - \frac{V_s + V_d}{2}$$

改寫成端點電壓：

- $V_{gs} = V_g - V_s$
- $V_{ds} = V_d - V_s$

先改寫平均項：

$$\frac{V_s+V_d}{2} = V_s + \frac{V_d - V_s}{2} = V_s + \frac{V_{ds}}{2}$$

代回去：

$$
\begin{aligned}
V_{gc,\text{mid}}
&= V_g - \left( V_s + \frac{V_{ds}}{2} \right) \\
&= (V_g - V_s) - \frac{V_{ds}}{2} \\
&= V_{gs} - \frac{V_{ds}}{2}
\end{aligned}
$$

**空間直覺（當初卡住的點）：**  
> Gate 在「上方」覆蓋整條 channel；channel 在「下方」沿著 source→drain 電位形成斜坡。  
> $V_{gc,\text{mid}} = V_g - V_{\text{ch,mid}}$ 是「上下方向」的電壓差，不是左右接觸的概念。  
> 用中點近似，是用「平均 channel 電位」來代表 gate 對 channel 的平均控制力。

---

### 13.3 為什麼還要扣 $V_t$？以及最常見的誤會怎麼解

這裡要分清楚兩個層級（這是你後來真正想通的關鍵）：

- **開關層級（有沒有通道）**：  
  $$V_{gs} \ge V_t$$  
  這回答的是「MOS 有沒有被打開」的 yes/no 問題。

- **電荷層級（通道有多少反轉電荷）**：  
  反轉層「開始形成」所需的那段 gate 電壓可以視為入場成本，因此有效能增加反轉電荷的部分要扣掉 $V_t$：

  $$V = V_{gc} - V_t \approx \left( V_{gs} - \frac{V_{ds}}{2} \right) - V_t$$

**關鍵澄清：**  
> 這不是在說 $V_{gs} = V_{gs}-V_{ds}/2$。  
> $V_{gs}$ 用來判斷「是否開通」，而 $V_{gs}-V_{ds}/2$ 是用來估算「平均 gate 控制力（進而估平均 charge）」。

---

### 13.4 邊界條件：Bulk charge model 什麼時候才「站得住腳」？

本頁的中點平均近似隱含假設：

> $V_{gs}-V_t$ 要「夠大」，使得 channel 大部分區域都能維持 strong inversion。

因此：
- 當 $V_{gs}$ 只比 $V_t$ 大一點點時，$V = (V_{gs}-V_{ds}/2)-V_t$ 可能很快變成負值。  
- 這不代表「實際完全沒有電流」，更合理的解讀是：**這個平均模型在該操作點開始失效，不應硬套**。

---

### 13.5 一句話封印 Page 12（回想用）

> **Page 12 做的事情：**  
> 用 $Q = C\cdot V$，其中 $C = C_{ox}WL$ 是比例尺，  
> $V \approx (V_{gs}-V_{ds}/2)-V_t$ 是 gate 對 channel 的平均有效控制力（中點近似 + 扣入場費）。


---

> 本筆記為 Lecture 3 **第一階段版本**。
> 後續可隨課程進度補充：
> - Id–Vg / Id–Vd 模型
> - Short-channel effects
> - Delay / Energy / Power 的正式分析