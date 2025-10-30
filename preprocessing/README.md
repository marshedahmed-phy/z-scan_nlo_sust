## ⚙️ ZScan Data Processing Algorithm: Preprocessing Unit

This document outlines the **9-step preprocessing algorithm** applied to the raw ZScan data to normalize signals, transform coordinates, and reduce the number of data points for final analysis.

---

### **1. [cite_start]Load and Preprocess Data** [cite: 1]

The initial step involves loading the raw data and assigning column names.

* [cite_start]**Load Data:** Read the CSV file without headers[cite: 5].
* [cite_start]**Assign Columns:** Assign the column names **ca** and **oa**[cite: 2, 3, 5].
    * [cite_start]*Initial Data Size:* $12000~\text{rows} \times 2~\text{columns}$[cite: 15].

---

### **2. [cite_start]Normalization of the Original Signal** [cite: 17]

The raw signals are normalized using baseline averages.

* [cite_start]**Calculate Means:** Determine the mean value for each signal ($\text{ca}$ and $\text{oa}$) using the **first and last 500 rows**[cite: 18, 19, 20, 21].
* [cite_start]**Normalize Signals:** Normalize the $\text{ca}$ and $\text{oa}$ signals using these calculated means[cite: 21, 22].
* [cite_start]**Filter Data:** Filter out any rows where normalized values are zero (if any)[cite: 23, 24, 25].
    * [cite_start]*Normalized Data Size:* $12000~\text{rows} \times 4~\text{columns}$[cite: 26].

---

### **3. [cite_start]Visualize Original and Normalized Data** [cite: 28]

Plots are generated to visually inspect the raw and normalized data, confirming the normalization effect.

---

### **4. [cite_start]Identify Region of Interest (ROI)** [cite: 53]

This step isolates the region containing the core signal feature.

* [cite_start]**Select Subset:** Select a subset of the normalized data between **index 4000 and 7000**[cite: 54].
* [cite_start]**Find Extrema:** Find the index positions of the **maximum and minimum** values in this range[cite: 55].
* [cite_start]**Extract Data:** Extract the data between these extrema to create a temporary DataFrame[cite: 56].
    * [cite_start]*Temporary Data Size:* $350~\text{rows} \times 2~\text{columns}$[cite: 81].

---

### **5. [cite_start]Linear Fit to Extract Transformation Parameters** [cite: 82]

A linear regression determines the index value corresponding to the baseline ($T=1$), which serves as the center point for coordinate transformation.

* [cite_start]**Define Cropping Bounds:** Determine the ROI and add a **10% padding** around this region[cite: 83, 84].
* [cite_start]**Extract Cropped Data:** Extract: $\text{y} = \text{normalized transmittance}$ (independent variable) and $\text{x} = \text{index values}$ (dependent variable)[cite: 85, 87, 88].
* [cite_start]**Perform Linear Regression:** Fit a straight line: $\text{x} = m \cdot y + b$[cite: 103, 104, 105].
* [cite_start]**Predict x:** Predict the index value ($\text{x\_predict}$) where $\text{y} = 1$[cite: 106, 107].

---

### **6. [cite_start]Coordinate Transformation** [cite: 108]

The original index values are transformed into physical position coordinates ($\text{x\_position}$ in meters).

* **Define Transformation Function:** Compute the new position:
    [cite_start]$$\text{x\_position} = (\text{index} - \text{x\_pred}) \cdot 10^{-5}$$ [cite: 109, 110, 111, 112]
* [cite_start]**Apply Transformation:** Apply the transformation and add $\text{x\_position}$ as a new column[cite: 113, 115].
    * [cite_start]*Final Data Size:* $12000~\text{rows} \times 5~\text{columns}$[cite: 147].

---

### **7. [cite_start]Re-Visualize with Transformed Coordinates** [cite: 154]

Generate new plots using the transformed $\text{Position x (in m)}$ on the x-axis.

---

### **8. [cite_start]Reduce Data Points Using Block-Wise Aggregation** [cite: 176]

The dataset is reduced by aggregating data points into chunks for a cleaner analysis.

* **Define Aggregation Function ($\text{reduce\_data}$):**
    * [cite_start]Group data into chunks (e.g., every **10 rows**)[cite: 178].
    * [cite_start]Compute the **mean and standard deviation** of selected columns within each group[cite: 179].
* [cite_start]**Apply Aggregation:** Apply the function to create a reduced DataFrame ($\text{reduced\_df}$)[cite: 180].
    * [cite_start]*Reduced Data Size:* $1200~\text{rows} \times 5~\text{columns}$[cite: 182].

---

### **9. [cite_start]Filter Region of Interest (Final)** [cite: 199]

The reduced data is filtered to isolate the central region for final plotting and fitting.

* [cite_start]**Filter Data:** Further filter the reduced data to a specific range, e.g., **$-0.03 < \text{x} < 0.03$**[cite: 200].
    * [cite_start]*Final Filtered Data Size:* $600~\text{rows} \times 5~\text{columns}$[cite: 202].
* [cite_start]**Plot Final Signals:** Plot the mean signals within this final window[cite: 205].
