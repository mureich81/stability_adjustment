# Analyzing trends in kinetic parameters of tRNA Selection

Within the constraints of the kinetic equation of tRNA selection (Eq. 3), the regression model is fitted to decoding efficiencies with simultaneous iterative perturbation of initially estimated N36-N39 stability values for each N37N38 split-tRNA variant. This process is repeated 1000 times to identify trends in the regression coefficients. Since these coefficients are proportional to discard parameters for hypothetical split-tRNA devoid of stacking interactions within N36-N39 segment, the observed trends provide insights into how different selection stages are influenced by the metastable codon-anticodon complex. 

## Table of Contents
1. [Model Description](#1-model-description)
2. [Code Description](#2-code-description)
3. [Outputs](#3-outputs)
4. [System Requirements](#4-system-requirements)
5. [Installation Instructions](#5-installation-instructions)
6. [Running the Code](#6-running-the-code)
7. [License](#7-License)
 

## 1 Model Description
The algorithm models the relationship between codon-anticodon complex stability and decoding efficiency using a system of 16 second-order polynomial equations. These equations are derived from the kinetic equation describing the tRNA selection process: ${\frac{k_{cat}}{K_M}=\frac{k_1}{1+d_1(1+d_2(1+d_3))}}$ \
The model utilizes estimated stabilities within the N36-N39 segments as [**independent variables**](split_tRNA_MANUSCRIPT/data/Data_1.xlsx). These stabilities are calculated as the sum of free energy changes for each stacking unit, under the assumption that these units are independent and that the respective nucleotides are free to adopt optimal overlaps (table S3). During regression analysis, the stabilities are iteratively perturbed to fit the experimentally determined products of decoding efficincy and the ratio of the concentration of split-tRNA<sup>Ala</sup>GCG variants to the concentration of competing tRNA<sup>Arg</sup>GCG. The decoding efficiencies of 16 N37N38 split-tRNA variants form a vector of [**dependent variables**](split_tRNA_MANUSCRIPT\data\Data_2.xlsx). 
The respective vectors of independent and dependent variables are also provided within the code's data overview section. 

[Back to Table of Contents](#table-of-contents)

## 2 Code Description
The algorithm is structured into three hierarchical modules (A, B, C):
- **Function A** (innermost loop) randomly selects the stability terms from the vector of independent variables and perturbs them individually over 100 trials. After 100 perturbations, it selects the vector variant that yields the best fit to the constant vector of dependent variables.
- **Cycle B** runs **Function A** for 56 iterations, each time passing an updated vector of stability terms from the previous iteration. The output is a dataframe containing columns with predicted regression coefficients, scores, and updated stability vectors for each of the 56 iterations. 
- **Cycle C** (outer loop) executes **Cycle B** 1000 times, generating as many independent dataframes. These dataframes are then used to extract the optimized regression coefficients ω<sub>0</sub>, ω<sub>1</sub>, and ω<sub>2</sub>.

[Back to Table of Contents](#table-of-contents)

## 3 Outputs
* The regression coefficients are extracted from the 1000 dataframes, and their distributions are plotted to identify trends in the corresponding discard parameter values.
* The 1000 dataframes are averaged row-wise to generate a single aggregated dataframe, which contains columns showing the score and the evolution of regression coefficients and stability terms within the N36-N39 segment as the score improves over 56 iteration cycles. This dataframe is displayed within the notebook and saved in Excel format.
* A diagram showing the evolution of the N36-N39 stability terms from the averaged dataframe.
* A plot illustrating the evolution of the relationship between average stability and decoding efficiency over 56 cycles of stability vector optimization.
* A plot showing the evolution of the regression curve over 56 cycles of stability vector optimization.

[Back to Table of Contents](#table-of-contents)

## 4 System Requirements

### 4-1. Hardware requirements
The algorithm runs efficiently on a standard computer with sufficient RAM to support the operations. 
For optimal performance, the following specifications are recommended:
RAM: 16+ GB
CPU: 4 Cores, 1.9 GHz/core

### 4-2. Software requirements
- **Python 3.10.6** or higher
- **Jupyter Components** will be installed from the requirements.txt file (see below)

[Back to Table of Contents](#table-of-contents)

## 5 Installation Instructions

### 5-1. Install Python:
* Download Python 3.10.6 or higher from the official website: [Python Downloads](https://www.python.org/downloads/)

### 5-2. Clone the Repository

* **Open a terminal or Command Prompt:**

    - *On Windows:*
      - Press `Win + R`, type `cmd`, and press Enter.
      - Alternatively, search for "Command Prompt" in the Start menu and open it.

    - *On macOS:*
      - Press `Cmd + Space`, type `Terminal`, and press Enter.
      - Alternatively, go to `Applications` > `Utilities` > `Terminal`.

    - *On Linux:*
      - Press `Ctrl + Alt + T`.
      - Alternatively, search for "Terminal" in your applications menu.


* **Clone the Repository:**
```sh
git clone https://github.com/USERNAME/REPOSITORY_NAME.git
cd REPOSITORY_NAME
```

### 5-3. Create and Activate Virtual Environment
* **On macOS/Linux:**
```sh
python3 -m venv venv
source venv/bin/activate
```
* **On Windows:**
```sh
python -m venv venv
venv\Scripts\activate
```
### 5-4. Install Dependencies
```sh
pip install -r requirements.txt
```
**Estimated Installation Time:** Approximately 5-10 minutes, depending on your internet speed and system performance.

### 5-5. Run Jupiter Notebook 
```sh
jupyter notebook
```
[Back to Table of Contents](#table-of-contents)

## 6 Running the Code
### 6-1. Open the Provided Notebook File:

* After running the command above, a new tab should open in your default web browser.
* Navigate to the provided notebook file (e.g., notebook.ipynb) and open it.

### 6-2. Set the Path for Exporting Results:

* Inside the notebook, in step **5-1**, set the path for exporting results by inserting the appropriate path referring to the location on your drive between the quotes.

### 6-3. Execute the Cells:

* Run each cell in the notebook sequentially. This will execute the code, perform the analysis, visualize the data, and export the results to the specified path.

**Estimated Run Time:** Running the code typically takes about 15 minutes (for 1000 cycle C iterations) to complete, depending on the size of the input data and system performance.

[Back to Table of Contents](#table-of-contents)

## 7 License
This project is licensed under the MIT License - see the [LICENSE](./OSI_License.ipynb) file for details. 

[Back to Table of Contents](#table-of-contents)
