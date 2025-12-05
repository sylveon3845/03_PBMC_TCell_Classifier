# 03-04_PBMC_TCell_Classifier
## 👑 高準確度 T 細胞分類器與機器學習模型深度評估 (Random Forest & SVM)

本專案利用單細胞 RNA-seq 數據，成功訓練了高度準確的細胞分類器，並通過引入第二個模型 (SVM) 進行嚴謹的**模型評估與比較**，展現了完整的機器學習應用與模型選擇能力。

---

### 🚨 數據下載與專案重現性註記 (必讀)

由於核心數據集 `pbmc_processed_for_ml.h5ad` (約 60MB) 超過 GitHub 網頁上傳限制，為確保專案的可重現性，數據集已獨立託管。

**請點擊此處下載 Project 03/04 核心數據集 pbmc_processed_for_ml.h5ad:**
[點擊下載 60MB 數據檔案](https://drive.google.com/file/d/1BXmN9nO3_qcm6eM9OYgh_oJ52lIs4pAP/view?usp=sharing)

**重現步驟：**
1. 下載 `pbmc_processed_for_ml.h5ad` 檔案。
2. 將該檔案與 `03_PBMC_TCell_Classifier.ipynb` 放置於同一目錄。
3. 即可執行 Notebook 進行驗證。

---

### 🎯 專案目標與技術棧

* **目標：** 區分 **T 細胞 (T-cell)** 與 **非 T 細胞 (Non-T-cell)**，並通過多模型比較 (RF vs. SVM) 確立最佳分類策略。
* **技術棧：** Scikit-learn (Random Forest, SVM)、GridSearchCV、Feature Importance。

### 📊 核心分類器：Random Forest (RF)

| 階段 | 成果總結 | 核心指標 |
| :--- | :--- | :--- |
| **數據準備** | 使用 Project 02 清洗後的 2638 個細胞和 1826 個高變異基因。 | 標籤：`T-cell` (1) / `Non-T-cell` (0)。 |
| **GridSearch 優化** | 使用 5 折交叉驗證 (5-fold CV) 進行網格搜尋，確定最佳參數。 | 最佳交叉驗證準確度：`0.9924` |
| **優化後性能** | 模型具備強大且穩定的泛化能力。 | **測試集整體準確度：`0.9886`** |
| **T 細胞識別** | 成功找出所有 T 細胞，無遺漏。 | **T 細胞召回率 (Recall)：`1.00`** |

### 🔬 特徵重要性分析 (Feature Importance)

模型決策具有高度的**生物學可解釋性**。Top 10 基因反映了 T 細胞的核心免疫功能和代謝狀態。

| 排名 | 基因 (Gene) | 關鍵生物學功能 |
| :--- | :--- | :--- |
| 1 | B2M | MHC Class I 相關，廣泛參與免疫反應。 |
| 2 | HLA-DPA1 | 關鍵 MHC Class II 基因，與抗原呈遞細胞 (APCs) 相關。 |
| 3 | NKG7 | T 細胞/NK 細胞功能標誌物。 |
| 4 | HLA-DRA | 關鍵 MHC Class II 基因。 |
| 5 | CD74 | MHC Class II 相關蛋白。 |
| 6-10 | RPLs/CCL5 | 核糖體蛋白與趨化因子，反映細胞代謝活性與遷移能力。 |

### 🚀 階段三：模型比較與評估 (Project 04)

為驗證 Random Forest 的穩健性，我們引入了 **Support Vector Machine (SVM)** 進行比較。

| 模型 | 最佳交叉驗證準確度 | 測試集整體準確度 | 非 T 細胞召回率 (Recall) |
| :--- | :--- | :--- | :--- |
| **Random Forest (RF)** | **0.9924** | **0.9886** | **0.94** |
| **SVM (SVC)** | 0.8521 | 0.8523 | **0.00** (失效) |

#### 模型比較結論：

1.  **RF 優勢：** Random Forest 在整體準確度上遠超 SVM，且在所有指標上表現均衡。
2.  **SVM 的局限性：** SVM 模型在類別不平衡的數據中表現極差，**將所有樣本都預測為佔多數的 T 細胞**，導致 Non-T-cell 類別的召回率為 0.00。
3.  **最終選擇：** 最終確認 **Random Forest** 為最佳分類器，證明了其在單細胞數據分類問題上的優越性、穩健性及對少數類別的識別能力。

### 🖼️ 最終視覺化成果

上圖展示了 Top 10 關鍵基因在 UMAP 空間中的表現強度。可以清楚看到，這些高重要性基因的表現強度熱區，與 Project 02 專案中識別出的不同細胞群集高度吻合，從而佐證了機器學習模型的分類依據。

![Top 10 Gene Expression UMAP](umap_top10_gene_expression_umap.png)
