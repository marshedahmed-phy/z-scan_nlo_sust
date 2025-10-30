# ZScan Data Processing Algorithm: Preprocessing Unit

This document outlines the 9-step preprocessing algorithm applied to the raw ZScan data to normalize signals, transform coordinates, and reduce the number of data points for final analysis.

1. Load and Preprocess DataThe initial step involves loading the raw data and assigning column names
          1. Load Data: Read the CSV file without headers
          2. Assign Columns: Assign the column names ca and oa3333.Initial Data Size: $12000~\text{rows} \times 2~\text{columns}$4.
