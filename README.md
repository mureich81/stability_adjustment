# Analyzing trends in kinetic parameters of tRNA Selection by fitting N36-N39 segment stability to split-tRNA's decoding efficincy

The algorithm iteratively perturbs the stabilities within the N36-N39 segment, fitting them to experimentally obtained decoding efficiencies. This process approximates optimal stabilities within the constraints of kinetic equation for tRNA selection and identifies trends in regression coefficients. As these coefficients are defined by kinetic parameters, the trends allow general conclusions about the sensitivity of different selection steps to the stability of the codon-anticodon complex.

## Table of Contents
1. [Model Description](#Model_Descrition)
2. [Code Description](#Code_Descrition)
3. [Outputs](#Outputs)
4. [System Requirements](#System_Requirements)
5. [Installation Instructions](#Installation_Instructions)
6. [Running the Code](#Running_The_Code)
7. [License](#License)
 

## 1. Model Description
The algorithm is based on a system of 16 second-order polynomial equations to model the relationship between codon-anticodon complex stability and decoding efficiency. These equations are derived from the kinetic equation describing tRNA selection process: ${\frac{k_{cat}}{K_M}=\frac{k_1}{1+d_1(1+d_2(1+d_3))}}$ \
The model utilizes the estimated stabilities in the N36-N39 segments as [**independent variables**](split_tRNA_MANUSCRIPT/data/Data_1.xlsx). These stabilities are calculated as the sum of free energy changes for each stacking unit under the assumption that these units are independent, and the respective nucleotides are free to adopt optimal overlaps with each other (SI Table 3). During the regression analysis, they are iteratively perturbed to fit the experimentally determined products of decoding efficincy and the ratio of split-tRNA<sup>Ala</sup>GCG variant concentration to a concentration of competing tRNA<sup>Arg</sup>GCG. The decoding efficiencies of 16 N37N38 split-tRNA variants form a vector of [**dependent variables**](split_tRNA_MANUSCRIPT\data\Data_2.xlsx). 
The respective vectors of independent and dependent variables are also provided within the code's data overview section. 


## 2. Code Description
The algorithm comprises three hierarchical modules (A, B, C): function A is executed within cycle B for 56 iterations, and cycle B is nested within the main outer iteration cycle C, repeating 1000 times:
- The innermost **Function A** randomly selects the stability terms from the vector of independent variables and perturbs them one at a time over 100 trials. Following these 100 perturbations, it selects the vector variant that yields the best fit to the constant vector of dependent variables.
- **Cycle B** executes nested function A for 56 iterations, each time passing an updated vector of stability terms from the preceding iteration. The output of cycle B is a dataframe containing columns of predicted regression coefficients, scores, and updated stability vectors corresponding to each of 56 iterations. 
- **Cycle C** executes cycle B for 1000 times, generating as many independent dataframes which are used to extract the optimized regression coefficients ω<sub>0</sub>, ω<sub>1</sub>, and ω<sub>2</sub>.


## 3. Outputs
* The regression coefficients are extracted from the 1000 dataframes, and their distributions are plotted to identify trends in the values of the corresponding discard parameters.
* The 1000 dataframes are averaged row-wise to generate a single aggregated dataframe. This dataframe contains columns showing the score and the evolution of regression coefficients and stability terms within the N36-N39 segment as the score improves over 56 iteration cycles. The file is displayed within the notebook and saved in parallel in excel format
* A diagram displaying the evolution of the N36-N39 stability terms from the averaged dataframe.
* Evolution of the relationship between average stability and decoding efficiency over 56 cycles of stability vector optimization.
* Evolution of the regression curve over 56 cycles of stability vector optimization.

## 4. System Requirements

### 4-1. Hardware requirements
The algorithm requires only a standard computer with enough RAM to support the operations. 
For optimal performance, we recommend a computer with the following specs:
RAM: 16+ GB
CPU: 4 Cores, 1.9 GHz/core

### 4-2. Software requirements
- **Python 3.10.6** or higher
- **Jupyter Components** will be installed from the requirements.txt file (see below)

## 5. Installation Instructions

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
## 6. Running the Code
### 6-1. Open the Provided Notebook File:

* After running the command above, a new tab should open in your default web browser.
* Navigate to the provided notebook file (e.g., notebook.ipynb) and open it.

### 6-2. Set the Path for Exporting Results:

* Inside the notebook, in step **5-1**, set the path for exporting results by inserting the appropriate path referring to the location on your drive between the quotes.

### 6-3. Execute the Cells:

* Run each cell in the notebook sequentially. This will execute the code, perform the analysis, visualize the data, and export the results to the specified path.

**Estimated Run Time:** Running the code typically takes about 15 minutes (for 1000 cycle C iterations) to complete, depending on the size of the input data and system performance.

## 7. License
This project is licensed under the MIT License - see the [LICENSE](./OSI_License.ipynb) file for details. 