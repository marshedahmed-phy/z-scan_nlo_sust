## ⚙️ ZScan Data Processing Algorithm: Preprocessing Unit

This document outlines the **9-step preprocessing algorithm** applied to the raw ZScan data to normalize signals, transform coordinates, and reduce the number of data points for final analysis.

---

### **1. Load and Preprocess Data**

The initial step involves loading the raw data and assigning column names.

The initial step involves loading the raw data and assigning column names.

* **Load Data:** Read the CSV file without headers.
* **Assign Columns:** Assign the column names **ca** to the first column and **oa** the second column.
    * *Initial Data Size:* $12000~\text{rows} \times 2~\text{columns}$.

---

### **2. Normalization of the Original Signal**

The raw signals are normalized using baseline averages.

* **Calculate Means:** Determine the mean value for each signal ($\text{ca}$ and $\text{oa}$) using the **first and last 300 values** (change with your data preferences).
* **Normalize Signals:** Normalize the $\text{ca}$ and $\text{oa}$ signals using these calculated means.
* **Filter Data:** Filter out any rows where normalized values are zero (if any).
    * *Normalized Data Size:* $12000~\text{rows} \times 4~\text{columns}$.

---

### **3. Visualize Original and Normalized Data**

Plots are generated to visually inspect the raw and normalized data, confirming the effectiveness of the normalization.

---

### **4. Identify Region of Interest (ROI)**

This step isolates the region containing the core signal feature (the Z-shaped portion) for precise analysis.

* **Select Subset:** Select a subset of the normalized data that encompasses the entire Z-shaped signal portion. Based on the current data setup, the range between **index 4000 and 7000** is typically effective.
* **Find Extrema:** Find the index positions of the **maximum and minimum** $\text{normalized\_ca}$ values within this selected range.
* **Extract Data:** Extract the data between these extrema to create a temporary DataFrame.
---

### **5. Linear Fit to Extract Transformation Parameters**

A linear regression determines the index value corresponding to the baseline ($T=1$), which serves as the center point for coordinate transformation.

* **Define Cropping Bounds:** Determine the ROI and add a **10% padding** around this region.
* **Extract Cropped Data:** Extract: $\text{y} = \text{normalized transmittance}$ (independent variable) and $\text{x} = \text{index values}$ (dependent variable).
* **Perform Linear Regression:** Fit a straight line: $\text{x} = m \cdot y + b$.
* **Predict x:** Predict the index value ($\text{x\_predict}$) where $\text{y} = 1$.

---

### **6. Coordinate Transformation**

The original index values are transformed into physical position coordinates ($\text{x\_position}$ in meters).

* **Define Transformation Function:** Compute the new position:
    $$\text{x\_position} = (\text{index} - \text{x\_pred}) \cdot 10^{-5}$$
* **Apply Transformation:** Apply the transformation and add $\text{x\_position}$ as a new column.
    * *Final Data Size:* $12000~\text{rows} \times 5~\text{columns}$.

---

### **7. Re-Visualize with Transformed Coordinates**

Generate new plots using the transformed $\text{Position x (in m)}$ on the x-axis.

---

### **8. Reduce Data Points Using Block-Wise Aggregation**

The dataset is reduced by aggregating data points into chunks for a cleaner analysis.

* **Define Aggregation Function ($\text{reduce\_data}$):**
    * Group data into chunks (e.g., every **10 rows**).
    * Compute the **mean and standard deviation** of selected columns within each group.
* **Apply Aggregation:** Apply the function to create a reduced DataFrame ($\text{reduced\_df}$).
    * *Reduced Data Size:* $1200~\text{rows} \times 5~\text{columns}$.

---

### **9. Filter Region of Interest (Final)**

The reduced data is filtered to isolate the central region for final plotting and fitting.

* **Filter Data:** Further filter the reduced data to a specific range, e.g., **$-0.03 < \text{x} < 0.03$**.
    * *Final Filtered Data Size:* $600~\text{rows} \times 5~\text{columns}$.
* **Plot Final Signals:** Plot the mean signals within this final window.
