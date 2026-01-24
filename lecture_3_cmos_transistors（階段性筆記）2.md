### 30.11 Subthreshold Swing（逐步推導與模型簡化）

> **本節目標：**  
> 從指數型的漏電模型，推導出工程上最常用的 **Subthreshold Swing（S）**。  
> 它的物理意義是：**「要讓漏電流下降一個量級（10 倍），Gate 電壓需要多關掉多少？」**

---

#### Step 1：回歸指數模型

在 subthreshold 區域（ $V_{gs} < V_t$ ），漏電流公式如下：

$$
I_{ds} = I_{ds0} \cdot \exp \left( \frac{V_{gs} - V_t}{n v_T} \right)
$$

其中：
- $v_T = \frac{kT}{q} \approx 26 \text{ mV}$ (在室溫 300K 下)
- $n$ 是下擺幅係數（subthreshold coefficient），通常 $n > 1$

---

#### Step 2：定義 Subthreshold Swing ($S$)

我們想知道當電流改變 10 倍時，$V_{gs}$ 的變化量（ $\Delta V_{gs}$ ）。這相當於對 $I_{ds}$ 取以 10 為底的對數後，求其變化的倒數。

定義 $S$ 為：

$$
S = \ln(10) \cdot n \cdot \frac{kT}{q}
$$

---

#### Step 3：常數帶入（為什麼會有 60 mV 這個數字？）

讓我們帶入室溫下的數值來算算看：
- $\ln(10) \approx 2.3$
- $v_T = \frac{kT}{q} \approx 26 \text{ mV}$

$$
S = 2.3 \cdot n \cdot 26 \text{ mV} \approx n \cdot 60 \text{ mV/decade}
$$

**物理直覺：**
> 如果是一個「完美的電晶體」（ $n=1$ ），每關掉 **60 mV** 的 Gate 電壓，漏電流就會下降 **10 倍**。
> 在現實中，$n$ 通常大於 1（例如 1.3 ~ 1.5），所以 $S$ 通常落在 **80 ~ 100 mV/decade** 之間。

---

#### Step 4：改寫成以 10 為底的工程公式

為了方便計算，我們利用 $e^{\ln(10) \cdot x} = 10^x$ 的性質，將原本的自然指數公式改寫。

已知 $\frac{1}{n v_T} = \frac{\ln(10)}{S}$，代入原式：

$$
I_{ds} = I_{ds0} \cdot \exp \left( \ln(10) \cdot \frac{V_{gs} - V_t}{S} \right)
$$

得到最終最常用的工程形式：

$$
\boxed{I_{ds} = I_{ds0} \cdot 10^{\frac{V_{gs} - V_t}{S}}}
$$

---

#### Step 5：這個公式怎麼讀？（一眼判定漏電量）

這個形式非常直覺，它告訴我們：

1. **當 $V_{gs} = V_t$ 時**：
   $I_{ds} = I_{ds0} \cdot 10^0 = I_{ds0}$。這再次驗證了 $I_{ds0}$ 是基準電流。

2. **當 $V_{gs}$ 比 $V_t$ 低了 3 個 $S$ 時**：
   (例如 $V_{gs} - V_t = -300 \text{ mV}$, 而 $S = 100 \text{ mV}$)
   $I_{ds} = I_{ds0} \cdot 10^{-3}$。代表漏電流縮小了 1000 倍。

---

### 30.12 總結：如何減少漏電？

根據上述公式，要減少 OFF-state 的漏電（ $V_{gs}=0$ 時的電流 ），設計者有三個手段：

1. **提高 $V_t$**：
   讓指數項的分子更負（最直接，但會犧牲切換速度）。
2. **減小 $S$（優化製程結構）**：
   讓 $n$ 靠近 1（例如使用 FinFET 或 GAA 結構讓 Gate 控制力變強），讓電流在關閉時下降得更快。
3. **降低溫度 ($T$)**：
   $v_T$ 變小，$S$ 就會隨之變小（變陡），漏電也隨之下降。這就是為什麼晶片過熱時，靜態功耗會飆升。