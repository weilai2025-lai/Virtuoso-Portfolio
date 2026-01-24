30.11 Subthreshold Swing（逐步推導與模型簡化）

本節目標：
從指數型的漏電模型，推導出工程上最常用的 Subthreshold Swing（S）。
物理意義：「漏電流變化一個 decade（10 倍），Gate 電壓需要改變多少？」

⸻

Step 1：從 subthreshold 的指數模型出發
在 subthreshold 區域（V_{gs} < V_t），常用的漏電模型可寫成：

$$
I_{ds} = I_{ds0},\exp!\left(\frac{V_{gs}-V_t}{n,v_T}\right)
$$

其中：
	•	熱電壓（thermal voltage）
$$
v_T \equiv \frac{kT}{q}
$$
	•	n：下擺幅係數（subthreshold coefficient，通常 n>1）

⸻

Step 2：把式子變成「直線」以方便定義斜率
對兩邊取自然對數（\ln）：

$$
\ln I_{ds} = \ln I_{ds0} + \frac{V_{gs}-V_t}{n,v_T}
$$

把 v_T=\frac{kT}{q} 代入：

$$
\ln I_{ds} = \ln I_{ds0} + \frac{q}{n kT}(V_{gs}-V_t)
$$

展開並把「不隨 V_{gs} 變化」的項合併成常數 A：

$$
\begin{aligned}
\ln I_{ds}
&= \left(\ln I_{ds0}-\frac{q}{n kT}V_t\right) + \frac{q}{n kT}V_{gs} \
&\equiv A + \frac{q}{n kT}V_{gs}
\end{aligned}
$$

因此得到一條「\ln I_{ds} 對 V_{gs} 的直線」：

$$
\ln I_{ds} = A + m V_{gs},
\qquad
m \equiv \frac{q}{n kT}
$$

⸻

Step 3：定義「電流變 10 倍」所需的 \Delta V_{gs}
「電流增加 10 倍」定義為：

$$
I’{ds} = 10,I{ds}
$$

同時令 gate 電壓增加：

$$
V’{gs} = V{gs} + \Delta V_{gs}
$$

新狀態也必須落在同一條直線上：

$$
\ln(10 I_{ds}) = A + m,(V_{gs} + \Delta V_{gs})
$$

用對數性質：

$$
\ln(10 I_{ds}) = \ln 10 + \ln I_{ds}
$$

代入並與原本的 \ln I_{ds} = A + mV_{gs} 相減：

$$
\begin{aligned}
\ln 10 + \ln I_{ds} &= A + mV_{gs} + m\Delta V_{gs} \
\ln 10 + (A + mV_{gs}) &= A + mV_{gs} + m\Delta V_{gs} \
\ln 10 &= m\Delta V_{gs}
\end{aligned}
$$

所以：

$$
\Delta V_{gs} = \frac{\ln 10}{m}
= \ln 10 \cdot \frac{n kT}{q}
$$

⸻

Step 4：Subthreshold Swing 的最終公式（S）
定義 subthreshold swing S 為「電流變化一個 decade（10 倍）所需的 gate 電壓變化」：

$$
S \equiv \Delta V_{gs} = n,\frac{kT}{q}\ln 10
$$

⸻

Step 5：工程常用的「以 10 為底」表示式
由 S 的定義可得：

$$
n\frac{kT}{q} = \frac{S}{\ln 10}
$$

代回原本的指數模型：

$$
\exp!\left(\frac{V_{gs}-V_t}{n kT/q}\right)
= \exp!\left(\frac{(V_{gs}-V_t)\ln 10}{S}\right)
$$

使用恆等式：

$$
e^{x\ln 10} = 10^x
$$

得到常用的工程式：

$$
I_{ds} \approx I_{ds0}\cdot 10^{\frac{V_{gs}-V_t}{S}}
$$

⸻

Step 6：為什麼室溫常看到「60 mV/dec」？
在室溫 T\approx 300\text{ K}：
	•	\ln 10 \approx 2.3
	•	\dfrac{kT}{q} \approx 26\text{ mV}

因此：

$$
S = \ln(10)\cdot n \cdot \frac{kT}{q}
\approx 2.3 \cdot n \cdot 26\text{ mV}
\approx n \cdot 60\text{ mV/dec}
$$

⸻

Step 7：一句話抓住物理意義
S 越小，代表只要更小的 gate 電壓變化，就能讓漏電流改變一個 decade，電晶體在 cutoff 區就越「關得乾淨」。  ￼