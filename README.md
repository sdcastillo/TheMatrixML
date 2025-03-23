# The Matrix and SciLab

https://www.youtube.com/watch?v=PSr5Q9KwIsg

# R CODE 
You can run this code using R and Rstudio.
place this dataset in your directory and load into 

# SciLab Code

​Scilab is a free and open-source software package for numerical computation, providing a powerful computing environment for engineering and scientific applications. citeturn0search0

To download the latest version of Scilab, follow these steps:

1. **Visit the Official Scilab Website**: Go to the [Scilab download page](https://www.scilab.org/download/scilab-2025.0.0).

2. **Select Your Operating System**: Choose the appropriate installer for your operating system:
   - **Windows 8, 10, 11**: Download the 64-bit executable.
   - **GNU/Linux**: Download the 64-bit tar.xz archive.
   - **macOS**:
     - For Intel-based Macs, download the 64-bit (Intel) dmg file.
     - For ARM-based Macs (e.g., those with M1 or M2 chips), download the 64-bit (ARM) dmg file.

3. **Install Scilab**:
   - **Windows**: Run the downloaded `.exe` file and follow the on-screen instructions.
   - **GNU/Linux**: Extract the tar.xz archive and follow the installation instructions provided in the package.
   - **macOS**: Open the `.dmg` file and follow the standard macOS installation process.

For detailed installation instructions and system requirements, refer to the [Scilab 2025.0.0 release notes](https://www.scilab.org/download/scilab-2025.0.0).

If you encounter any issues during installation or need further assistance, the [Scilab community forums](https://www.scilab.org/community) are a valuable resource. 

Below is a sample SciLab script that demonstrates how to implement and estimate the same Gaussian GLM (linear model) using the normal equations. This example reads a design matrix X (from model_matrix.csv) and a numeric response vector y (from y.csv). Each file is assumed to be purely numeric with no header row.
