# Virtuoso 實戰筆記

## 1. 放置元件（Instances）

### 快捷鍵
| 快捷鍵 | 功能 |
|--------|------|
| `I` | 放置元件（Instance） |
| `ESC` | 取消/結束目前操作 |

### 元件尺寸設定（Inverter 範例）
| 元件 | Width | Length |
|------|-------|--------|
| NMOS_VTG | 400nm | 50nm |
| PMOS_VTG | 600nm | 50nm |

---

## 2. MOS 電晶體基礎知識

### Width 與 Length 的定義
- **L（Length）**：Source 到 Drain 的距離，電流「流過」的路徑長度
- **W（Width）**：垂直於電流方向，電流可以「並排通過」的寬度

> 類比：把電流想像成水流
> - L = 水管長度 → 越長阻力越大，電流越小
> - W = 水管直徑 → 越寬通道越大，電流越大

### 電流公式
```
I_D ∝ μ × (W / L)
```
- W 越大 → 電流越大
- L 越大 → 電流越小

### 為什麼 PMOS 比 NMOS 寬？
| 載子類型 | 使用於 | 遷移率 μ |
|---------|--------|----------|
| 電子 (Electron) | NMOS | 較高（約 2~3 倍） |
| 電洞 (Hole) | PMOS | 較低 |

- **設計目標**：讓 Inverter 的 pull-up（PMOS）和 pull-down（NMOS）驅動能力相當
- **解法**：增加 PMOS 的 W 來補償較低的遷移率
- **常見比例**：Wp / Wn ≈ 1.5 ~ 3

### Drawn Length vs Effective Length
| 術語 | 數值 | 說明 |
|------|------|------|
| Drawn Length | 50nm | Virtuoso 中繪製的尺寸 |
| Effective Length | ≈ 45nm | 製造後實際的通道長度 |

- 差異原因：Source/Drain 的 n+ 區域會往 Gate 下方擴散
- 製程命名：FreePDK**45** → 指 effective length
- **設計原則**：使用製程的 minimum drawn length（50nm），獲得最大電流密度

---

## 3. 加入 Pins 與連線

### 快捷鍵
| 快捷鍵 | 功能 |
|--------|------|
| `P` | 放置 Pin（接腳） |
| `W` | 連線（Narrow Wire） |
| `U` | Undo（復原） |
| `Shift + U` | Redo（重做） |

### Pin 設定
| Pin 名稱 | Direction | 說明 |
|----------|-----------|------|
| A | input | 輸入訊號 |
| Y | output | 輸出訊號 |
| vdd | inputOutput | 電源正極 |
| gnd | inputOutput | 電源負極 |

### 連線技巧
- **單擊**：開始畫線 / 轉彎
- **雙擊**：結束畫線
- 線要接到元件的**紅色小方塊**才算連接成功
- 連接成功會出現**藍色圓點（Junction）**

### Body 端子 Warning
- MOS 有第四個端子叫 **Body（B）**
- Warning: "Pin B floating" 表示 Body 沒有明確連接
- **解法**：NMOS Body 接 GND，PMOS Body 接 VDD
- **可忽略**：Simulator 通常會自動處理

### Check and Save
- 點擊工具列的 **磁片 + 綠色勾勾** 圖示
- 會同時檢查錯誤並存檔
- 目標：0 errors, 0 warnings

---

## 4. 建立 Symbol（電路符號）

### 從 Schematic 自動生成 Symbol
1. 在 Schematic Editor：`Create` → `Cellview` → `From Cellview`
2. 設定：
   - From View Name = `schematic`
   - To View Name = `symbol`
   - Tool/Data Type = `schematicSymbol`
3. 點 OK，系統會自動生成方塊符號

### 編輯 Symbol 外觀（畫 Inverter 三角形）

#### Step 1：刪除原本的方塊
1. 選取綠色方塊（不是紅色框）
2. 按 `Delete` 刪除

#### Step 2：畫三角形
1. 選單：`Create` → `Shape` → `Polygon`
2. 依序點擊三個點形成三角形（尖端朝右）
3. 按 `ESC` 完成

#### Step 3：畫反相圈
1. 選單：`Create` → `Shape` → `Circle`
2. 在三角形尖端畫一個小圓

#### Step 4：調整 Pin 位置
- 選取 Pin 後拖曳到適當位置
- `in` 放左邊，`out` 放右邊

### Symbol 元素說明
| 元素 | 功能 |
|------|------|
| 綠色線條/形狀 | 純視覺圖形，讓人看懂這是什麼元件 |
| 紅色方塊（Pin） | 真正的電氣連接點 |
| 紅色大框 | Selection Box（選取區域） |

---

## 5. 建立 Testbench（測試平台）

### 為什麼需要 Testbench？
- Schematic 只有電路本身
- 模擬需要：輸入訊號源、電源供應、接地

### Testbench 元件
| 元件 | Library | Cell | 用途 |
|------|---------|------|------|
| 你的電路 | 你的 Library | inv (symbol) | 待測電路 |
| DC 電壓源 | analogLib | vdc | 提供 VDD |
| 脈衝訊號源 | analogLib | vpulse | 輸入測試訊號 |
| 接地符號 | analogLib | gnd | 定義 0V 參考點 |

### vpulse 參數設定（範例）
| 參數 | 值 | 說明 |
|------|-----|------|
| Voltage 1 | 0 | 低電位 |
| Voltage 2 | 1.1 | 高電位 |
| Period | 10n | 週期 |
| Delay time | 1n | 延遲 |
| Rise/Fall time | 20p | 上升/下降時間 |
| Pulse width | 5n | 高電位持續時間 |

> **單位提醒**：必須手動加上單位（n = nano, p = pico）

### Wire Label（網路標籤）
- 用途：讓同名的線自動連接，不需實際拉線
- 操作：按 `L`，輸入名稱，點擊線段
- 範例：所有標記 `VDD` 的線會自動連在一起

### Design Variable（設計變數）
- vdc 的 DC voltage 可填變數名稱（如 `vdd`）
- 實際數值在 ADE 模擬環境中定義
- 好處：方便做參數掃描（Parametric Sweep）

### gnd 符號
- 代表電路的 **0V 參考點**
- 不需要任何設定，放置即可
- 所有電壓源的負極都要連到 gnd

---

## 6. ADE 模擬設定

### 開啟 ADE
- 在 Testbench schematic 中：`Launch` → `ADE L`

### 設定 Simulator
1. `Setup` → `Simulator/Directory/Host`
2. Simulator 選 `spectre`
3. 點擊 OK

### 設定 Model Libraries
1. `Setup` → `Model Libraries`
2. 雙擊 `<Click here to add model file>`
3. 瀏覽到：`/vol/eecs391/FreePDK45/ncsu_basekit/models/hspice/hspice_nom.include`
4. 點擊 Open，然後 OK

> **Model File 的用途**：告訴模擬器 MOS 電晶體的物理特性（電流、閥值電壓、寄生電容等）

### 設定 Transient Analysis
1. 在 Analyses 區域右鍵 → Edit
2. 選擇 `tran`
3. Stop Time：`40n`
4. 確認 Enabled 勾選，點擊 OK

### 設定 Design Variables
1. 在 Design Variables 區域右鍵 → Edit
2. Name：`vdd`，Value：`1.1`
3. 點擊 Add/OK

### 執行模擬
- 點擊綠色播放按鈕（Netlist and Run）
- 等待 "Simulation completed successfully"

### 查看波形
1. `Results` → `Direct Plot` → `Transient Signal`
2. 回到 schematic，點擊要觀察的**線**（不是 pin）
3. 按 ESC 結束

### 儲存模擬設定
1. `Session` → `Save State`
2. Save State Option 選 `Cellview`
3. 下次可從 Library Manager 載入

---

## 7. 量測設定（Outputs）

### 設定要儲存的訊號
1. `Outputs` → `To Be Saved` → `Select On Design`
2. 點擊 schematic 上的線
3. 按 ESC 結束

### 設定量測表達式
1. `Outputs` → `Setup...`
2. 填入 Name 和 Expression
3. 勾選 Plotted/Evaluated
4. 點擊 Add

### Expression 語法說明

| 函數 | 意義 |
|------|------|
| `VT("/net")` | 取得某條線的**電壓波形** |
| `IT("/inst/pin")` | 取得某個 pin 的**電流波形** |
| `cross(波形, 電壓, 次數, 方向, nil, nil)` | 找出波形穿越某電壓的時間 |
| `integ(波形, 起始時間, 結束時間)` | 對波形做積分 |
| `VAR("變數名")` | 取得 Design Variable 的值 |

### 常用量測公式

**delay_rise（上升延遲）：**
```
(cross(VT("/out") 0.55 2 "rising" nil nil) - cross(VT("/in") 0.55 2 "falling" nil nil))
```

**delay_fall（下降延遲）：**
```
(cross(VT("/out") 0.55 2 "falling" nil nil) - cross(VT("/in") 0.55 2 "rising" nil nil))
```

**energy_rise_fall（切換能量）：**
```
(integ(IT("/I0/vdd") 2e-08 3e-08) * VAR("vdd"))
```

### 量測結果範例（Inverter）
| 項目 | 數值 | 單位 |
|------|------|------|
| delay_rise | 4.213p | ps（皮秒） |
| delay_fall | 4.157p | ps（皮秒） |
| energy_rise_fall | 2.155f | fJ（飛焦耳） |

---

## 8. 常見錯誤與解決

### Wire Label 沒有連接
- **症狀**：floating pin/net warning
- **原因**：Wire label 只是放在旁邊，沒有點擊到線上
- **解法**：先拉一段線（`W`），按 `L` 輸入名稱，**點擊那段線**

### Inverter 輸出電壓不對
- **症狀**：輸出不是 0V 和 1.1V
- **原因**：VDD 或 GND 沒有正確連接
- **解法**：檢查 wire label 名稱是否一致

---

## 9. Layout 設計

### 建立 Layout View
1. 在 Library Manager 中選擇你的 Cell
2. 在 View 欄位輸入 `layout`，按 Enter
3. 設定：
   - Type：`layout`
   - Application：`Layout L`
4. 點擊 OK 開啟 Layout Editor

### 初始設定（Options > Display）
| 參數 | 設定值 |
|------|--------|
| Minor Spacing | 0.1 |
| Major Spacing | 0.5 |
| X Snap Spacing | 0.0025 |
| Y Snap Spacing | 0.0025 |

### Workspace 設定
- 在工具列找到 Workspace 下拉選單
- 選擇 **basic**

### 放置 Transistor
1. 按 `I`（Create Instance）
2. Browse → **NCSU_TechLib_FreePDK45** → **pmos_vtg** 或 **nmos_vtg** → **layout**
3. 設定 Width（PMOS: 0.6, NMOS: 0.4）
4. 在編輯區點擊放置

### Layout 快捷鍵
| 快捷鍵 | 功能 |
|--------|------|
| `Shift + F` | 切換顯示 PyCells 內部結構 |
| `K` | 畫 Ruler（測量距離） |
| `Shift + K` | 清除所有 Ruler |
| `Z` | 縮放（畫框選擇區域） |
| `F` | Fit（顯示全部） |
| `Shift + Z` | 放大 2 倍 |
| `Ctrl + Z` | 縮小 2 倍 |

### Layout 顏色說明
不同顏色代表不同製程層（Layers）：
| 顏色 | 可能代表 |
|------|---------|
| 紅色 | Poly（閘極） |
| 藍色 | Metal（金屬連線） |
| 綠色 | Active（主動區） |

---

## 10. DRC（Design Rules Check）

### 什麼是 DRC？
- 檢查 Layout 是否符合製程的設計規則
- 例如：最小線寬、最小間距等

### 執行 DRC
1. 選單：`Calibre` → `Run DRC`
2. 設定 Rules：
   - DRC Rules File：`/vol/eecs391/FreePDK45/ncsu_basekit/techfile/calibre/calibreDRC.rul`
3. 確認 Inputs：
   - Run：DRC (Hierarchical)
   - Format：GDSII
   - Export from layout viewer：✅
   - Top Cell 和 Library 正確
4. 點擊 **Run DRC**

### DRC 結果
- ✅ 0 Results = 沒有違規，通過
- ❌ 有 Results = 需要修復違規

### PMOS/NMOS 間距
- pwell 和 nwell 最小間距：**0.225 μm**
- 使用 Ruler（`K`）測量並調整位置

---

## 11. Painting（連線繪製）

### 什麼是 Painting？
在 Layout 中畫圖形來連接元件。

### 連接 Gate（Poly 層）
1. 選擇 **poly** layer
2. 按 `R`（Rectangle）或 `Create` → `Shape` → `Rectangle`
3. 從 PMOS Gate 畫到 NMOS Gate，重疊連接

### 連接 Drain（Metal1 層）
1. 選擇 **metal1** layer
2. 按 `R` 畫矩形，連接 PMOS 和 NMOS 的 Drain
3. 這條線就是 **Output**

### Drain vs Source 判斷
| 位置 | 角色 | 說明 |
|------|------|------|
| 連在一起的那端 | Drain (Output) | PMOS/NMOS Drain 連接 |
| 分開的那端 | Source | PMOS 接 VDD，NMOS 接 GND |

---

## 12. Contact 與 Pin

### Well Contact（ntap/ptap）
用途：連接 MOS 的 Body 端到正確電位

| 元件 | 放置位置 | 連接到 |
|------|---------|--------|
| ntap | PMOS 上方 | n-well → VDD |
| ptap | NMOS 下方 | p-well → GND |

#### 放置方式
1. `Create` → `Via`（或按 `O`）
2. 選擇 **ntap** 或 **ptap**
3. 放置在對應 Transistor 旁邊
4. 按 `Q` 修改 Width 讓它延伸覆蓋整個區域

### M1_POLY Via（Gate 連接）
1. `Create` → `Contact`（或按 `O`）
2. Contact Type：**M1_POLY**
3. 放置在 Gate 的 Poly 上
4. 讓 Poly 可以被 Metal1 連接

### 建立 Pin
1. 選擇 **metal1 drawing** layer（很重要！）
2. `Create` → `Pin...`
3. 設定：
   - Terminal Names：`vdd gnd in out`
   - Pin Shape：rectangle
   - I/O Type：vdd/gnd/in = input，out = output
4. 放置 Pin 矩形（要覆蓋在 Metal1 上面）

### 建立 Pin Label（重要！）
> ⚠️ Create Pin 視窗內的 "Create Label" 選項**沒有用**！

需要**手動建立 Label**：
1. 選擇 **metal1 pin** 或 **text** layer
2. `Create` → `Label`
3. 輸入 Pin 名稱（例如 `vdd`）
4. 放置在對應的 Pin 矩形上
5. 對每個 Pin 重複此步驟

### Pin Label 大小
- 在 Create Label 視窗中設定 Height
- Height 設為 **0.1**（預設太大）

---

## 13. Layout 完成收尾

### 對齊原點
1. `Ctrl + A` 全選
2. 按 `M`（Move）
3. 點擊 Layout 左下角
4. 輸入 `0 0` 移動到原點

### 填滿 nwell
- 確保 nwell 層延伸到 cell 邊緣
- 方便之後和其他 cell 並排（abutment）

---

## 快捷鍵整理

### Schematic 快捷鍵
| 快捷鍵 | 功能 |
|--------|------|
| `I` | 放置元件 (Instance) |
| `W` | 拉線 (Wire) |
| `L` | Wire Name（標籤） |
| `P` | 建立 Pin |
| `M` | 移動 |
| `C` | 複製 |
| `Q` | 查看/編輯屬性 |
| `U` | Undo |
| `E` / `O` | Display Option |
| `F` | Fit（顯示全部） |
| `F3` | 進階選項 |

### Layout 快捷鍵
| 快捷鍵 | 功能 |
|--------|------|
| `I` | 放置元件 (Instance) |
| `M` | 移動 |
| `O` | Via / Contact |
| `P` | Path（畫路徑） |
| `R` | Rectangle（畫矩形） |
| `Q` | 查看/編輯屬性 |
| `K` | Ruler（量測距離） |
| `Shift + K` | 清除所有 Ruler |
| `Shift + F` | 切換 PyCells 內部結構 |
| `Z` | 縮放（畫框） |
| `F` | Fit（顯示全部） |
| `Shift + Z` | 放大 2 倍 |
| `Ctrl + Z` | 縮小 2 倍 |
| `Ctrl + A` | 全選 |

### Simulation 快捷鍵（波形視窗）
| 快捷鍵 | 功能 |
|--------|------|
| `A` | 加入 Marker A |
| `B` | 加入 Marker B（量測 A-B 距離） |
| `H` | 水平線 |
| `V` | 垂直線 |
| `M` | 加入新 Marker |
| `D` | 加入第二 Marker（量測距離） |

---

## 14. LVS（Layout vs Schematic）

### LVS 是什麼？
檢查 Layout 和 Schematic 是否**電氣上一致**：連線正確、Pin 名稱對應、電晶體參數一致。

### 執行 LVS
1. 在 Layout Editor 中：`Calibre` → `Run LVS`
2. 設定 **Rules**：
   - LVS Rules File：`/vol/eecs391/FreePDK45/ncsu_basekit/techfile/calibre/calibreLVS.rul`
3. 設定 **Inputs > Layout Tab**：
   - Hierarchical ✅
   - Layout vs Netlist ✅
   - Export from layout viewer ✅
   - Format：GDSII
4. 設定 **Inputs > Netlist Tab**：
   - **Export from schematic viewer ✅**（很重要！）
   - Format：SPICE
5. 點擊 **Run LVS**

### LVS 結果
- ✅ **CORRECT**（笑臉）= 通過
- ❌ **Discrepancies** = 需要修復

---

## 15. LVS 常見錯誤與解決方法

### EP=0（External Ports = 0）
**問題**：Layout 的 subcircuit 沒有識別到任何 port。

**原因**：
1. Pin 沒有正確放置在 Metal1 上
2. 使用了錯誤的 Create Pin 方式

**解決方法**：
- **不要用** Create Pin 視窗內的 "Create Label"（沒用！）
- **要用** 選單 `Create` → `Pin...`
- 確保選擇 **metal1 drawing** layer
- Pin 矩形要**覆蓋在 Metal1 上面**

### Pin 名稱大小寫不一致
**問題**：Layout 用 `vdd`，Schematic 用 `VDD`。

**解決方法**：統一大小寫（LVS 區分大小寫）。

### Body 未連接
**問題**：ntap/ptap 沒有和 VDD/GND 的 Metal1 連接。

**解決方法**：確保 ntap 的 Metal1 和 VDD Pin 的 Metal1 **重疊連接**。

---

## 16. Pin Names 顯示設定

如果 Layout 中的 Pin Label 看不到：

1. 選單：`Options` → `Display...`
2. 在 **Display Controls** 區域
3. 勾選 **Pin Names** ✅

這樣 Pin 的名稱才會顯示在 Layout 上。

---

## 17. Layout Extraction（PEX）

### PEX 是什麼？
萃取 Layout 中的**寄生電容和電阻**（Parasitic RC），讓模擬更接近實際晶片。

### 執行 PEX
1. 在 Layout Editor 中：`Calibre` → `Run PEX`
2. 如果有 "Load Runset File" popup，瀏覽到：
   `/vol/eecs391/FreePDK45/ncsu_basekit/techfile/calibre/runset.calibre.pex`

### Rules 設定
- PEX Rules File：`/vol/eecs391/FreePDK45/ncsu_basekit/techfile/calibre/calibrexRC.rul`
- PEX Run Directory：`/home/netID/cadence/LIBRARY/CELL`

### Inputs 設定
**Layout Tab：**
- File：`CELL.calibre.db`
- Format：GDSII
- **Export from layout viewer** ✅（很重要！）
- Top Cell、Library、View

**Netlist Tab：**
- Files：`CELL.src.net`
- Format：CALIBRVIEW
- **Export from schematic viewer** ✅（很重要！）

### Outputs 設定
- Extraction Type：**Transistor level, R+C+CC, No Inductance**
- Format：**CALIBRVIEW**
- Use Names From：**SCHEMATIC**
- File：`CELL.pex.netlist`
- **View netlist after PEX finishes** ✅

### Nets 設定
- Extract parasitics for：**All Nets**

### Reports 設定
- PEX Report：`CELL.pex.report`（box 應該填滿 pink）
- **SVDB** ✅
- SVDB Directory：`/home/netID/cadence/LIBRARY/CELL/svdb/`
- **Start RVE after PEX** ✅
- **Generate cross-reference data for RVE** ✅

### PEX Options 設定（重要！）
1. 點擊 `Setup`
2. 勾選 **PEX options**
3. 在右側出現的面板中，於 **Include** 區域輸入：
   - `LAYOUT CASE YES`
   - `SOURCE CASE YES`

### 執行
1. 點擊 **Run PEX**
2. 如果詢問是否 overwrite，點擊 OK
3. 成功後會彈出兩個視窗：
   - **Calibre View Setup**：用於設定 calibre view 以進行模擬
   - **RVE**：顯示萃取結果（寄生電容值）

---

## 18. Simulate Extracted Layout

### Step 1：設定 Calibre View Setup
1. 確認 **cellmap file** 路徑正確（應該在 cadence directory）
2. 點擊 **View** 確認 cellmap 內容
3. 點擊 **OK**
4. 會顯示 "Calibre View generation completed with 0 WARNINGs and 0 ERRORs"

### Step 2：建立 Config View
1. 關閉 extracted layout 圖和 ADE 視窗
2. 回到 **Library Manager**
3. 選擇 Library 和 **Sim_inv** cell
4. `File` → `New` → `Cell View`
5. Type 選擇 **config**
6. 點擊 OK

### Step 3：設定 New Configuration
1. **Top Cell** 區域：
   - Library：你的 Library
   - Cell：Sim_inv
   - View：schematic
2. 點擊 **Use Template** 按鈕
3. 選擇 **spectre**，點擊 OK
4. **Global Bindings** 會自動填入
5. 點擊 OK

### Step 4：設定使用 calibre view
1. 在 **Hierarchy Editor** 的 **Cell Bindings** 中找到 **inv**
2. **右鍵點擊** inv
3. 選擇 **Set Cell View**
4. View To Use 選擇 **calibre**
5. **儲存** Config

### Step 5：載入模擬設定並執行
1. 開啟 **Sim_inv**
2. `Launch` → `ADE L`
3. `Session` → `Load State`
4. Load State Option：**Cellview**
5. 選擇 Library、Cell、之前儲存的 State 名稱
6. 點擊 OK
7. 執行模擬

### 切換 schematic / calibre 模式
| View To Use | 說明 |
|-------------|------|
| **schematic** | 使用原始 Schematic（無寄生 RC）|
| **calibre** | 使用萃取後的 Layout（有寄生 RC）|

在 Hierarchy Editor 中右鍵點擊 inv，選擇 Set Cell View 即可切換。

---

## 19. Dealing with Locked Items

如果電腦凍結導致檔案被鎖定（可讀但無法編輯或重新儲存），使用以下步驟解鎖：

### 解鎖步驟
1. 確保 Cadence 已關閉，保持 terminal 開啟
2. 輸入：
   ```
   clsAdminTool
   ```
3. 查看可用命令：
   ```
   help
   ```
4. 列出被鎖定的檔案：
   ```
   ale Project_Dir
   ```
   （將 `Project_Dir` 替換為你的專案資料夾名稱）
5. 解鎖檔案：
   ```
   asre file_info
   ```
   （將 `file_info` 替換為上一步列出的完整資訊行）
6. 退出 admin tool：
   ```
   ctrl+c
   ```
7. 重新啟動 Cadence：
   ```
   virtuoso
   ```
