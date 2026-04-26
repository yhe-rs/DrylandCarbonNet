# DrylandCarbonNet Methodology Workflow

### **Phase 1: Knowledge-Guided Synthetic Data Generation (Task A)**
* **1.1 Data Collection & Parameterization**
    * Collect high-resolution geospatial drivers: Climate (**NLDAS-2**), Soil (**gSSURGO**), Land Cover (**Landsat/NLCD**), and Topography (**TanDEM-X/SRTM**).
* **1.2 EcoSIM Process-Based Simulation**
    * Run the **EcoSIM** model to simulate daily carbon fluxes: Autotrophic Respiration ($Ra$), Heterotrophic Respiration ($Rh$), Net Ecosystem Exchange ($NEE$), and Gross Primary Production ($GPP$).
    * Period: 2000–2025 across major dryland land types.
* **1.3 ML-Ready Dataset Creation**
    * Aggregate simulation outputs into a synthetic dataset to provide the deep learning model with "prior" biogeochemical knowledge.

---

### **Phase 2: Model Architecture & Constraint-Based Pre-training (Task B)**
* **2.1 Modular GRU Design**
    * **`GRU_backbone`**: Extracts temporal features from climate and soil drivers.
    * **`GRU_Ra`**: Specifically predicts autotrophic respiration.
    * **`GRU_Rh`**: Predicts heterotrophic respiration using a residue layer ($GPP - Ra$).
    * **`GRU_NEE`**: Final estimation of net ecosystem exchange.
* **2.2 Hierarchical Training Strategy**
    1. **Step 1**: Train `backbone` and `Ra` (Plant-related modules); others frozen.
    2. **Step 2**: Train `Rh` alongside the plant modules; NEE frozen.
    3. **Step 3**: Full model training using **Biogeochemical Constraints**:
        * *Mass Balance*: Ensure $GPP - Ra - Rh = -NEE$.
        * *Response Control*: Ensure $Rh$ increases realistically with soil organic carbon.

---

### **Phase 3: Observational Fine-Tuning & Upscaling (Task C)**
* **3.1 In-situ Data Integration**
    * Process Eddy Covariance flux data from the **AmeriFlux** database (cleaning, gap-filling, and flux partitioning).
* **3.2 Model Refinement**
    * Fine-tune the pre-trained model using real-world measurements.
    * Freeze the `backbone` to preserve temporal patterns while allowing flux submodules to adapt to site-level reality.
* **3.3 Regional Application**
    * Upscale the model to **250m resolution** across the Western U.S. by assimilating **SLOPE GPP** and climate forcing.

---

### **Phase 4: Impact Analysis & Future Forecasting (Tasks D & E)**
* **4.1 Wildfire Impact Assessment**
    * Quantify carbon flux changes during immediate (1-yr), short-term (5-yr), and long-term (10-yr) post-fire recovery stages.
* **4.2 Future Scenario Forecasting**
    * Predict future $GPP$ using a dedicated GRU model.
    * Simulate carbon dynamics under **RCP 4.5 and 8.5** scenarios.
* **4.3 Tipping Point Identification**
    * Identify critical thresholds where dryland ecosystems transition from carbon sinks to carbon sources.

<img width="1536" height="1024" alt="DrylandCarbonNet" src="https://github.com/user-attachments/assets/af1f98a8-e704-4beb-97b9-bcbf24617d3f" />
