# The Matrix and SciLab

# R CODE 
#you can run this code using R and Rstudio.
#place this dataset in your directory and load into 

train = read.csv("train.csv")
GLM = glm(formula = Crash_Score ~ . + Work_Area * Rd_Class - Month, 
    family = gaussian(), data = train)

summary(GLM)

///////////////////////////////////////////////////////////////////////////////
// SCILAB CODE
//  1. LOAD DATA
///////////////////////////////////////////////////////////////////////////////

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

fileX = "model_matrix.csv";
fileY = "y.csv";

// If your CSV files have no header row, simply do:
X = csvRead(fileX, ",", ".", "double");  // design matrix: includes intercept as first col
y = csvRead(fileY, ",", ".", "double");  // numeric outcome: Crash_Score

// Optional: check dimensions
[N, p] = size(X);  // N = number of observations, p = number of predictors (including intercept)
disp("Size of X = "+string(N)+" x "+string(p));
disp("Size of y = "+string(size(y,1))+" x 1");

///////////////////////////////////////////////////////////////////////////////
// 2. FIT THE GAUSSIAN GLM (LINEAR MODEL) VIA NORMAL EQUATIONS
///////////////////////////////////////////////////////////////////////////////
//
//   We solve for beta_hat = (X^T X)^(-1) X^T y
//
// IMPORTANT: This direct inverse approach is fine for demonstration,
// but for numerical stability, an alternative like QR decomposition is recommended.
// For moderate data sizes, normal equations are typically acceptable.

XT = X';
XT_X = XT * X;        // (p x p)
XT_y = XT * y;        // (p x 1)

// invert (X^T X)
inv_XT_X = inv(XT_X);

// compute beta estimates
beta_hat = inv_XT_X * XT_y;  // p x 1

///////////////////////////////////////////////////////////////////////////////
// 3. COMPUTE FITTED VALUES, RESIDUALS, RSS, etc.
///////////////////////////////////////////////////////////////////////////////

y_hat = X * beta_hat;       // fitted values
resid = y - y_hat;          // residuals (N x 1)
RSS   = sum(resid.^2);      // residual sum of squares

// degrees of freedom = N - p
df = N - p;

// unbiased estimate of variance, i.e. mean squared error (MSE)
sigma2_hat = RSS / df; 
sigma_hat  = sqrt(sigma2_hat);

// standard errors of coefficients: sqrt( diag( sigma^2 * (X^T X)^(-1) ) )
var_beta_hat = sigma2_hat * inv_XT_X;       // p x p
SE_beta      = sqrt(diag(var_beta_hat));    // p x 1

///////////////////////////////////////////////////////////////////////////////
// 4. T-STATISTICS AND P-VALUES
///////////////////////////////////////////////////////////////////////////////
//
// t_j = beta_hat_j / SE_beta_j
// p-value = 2 * [1 - CDF_t(|t_j|, df)] for two-sided test
// SciLab does not have a built-in for the Student-t CDF in base. 
// If you have the Statistics or DSP toolbox, you can import the t-distribution CDF. 
// Or, for large N, a z approximation might suffice.

t_stats = beta_hat ./ SE_beta;     // p x 1

// If we want approximate p-values using normal distribution for large df
// (like R does if df is large). For a more rigorous approach, you need a t-distribution CDF.
function pVal = pValueApprox(t)
    pVal = 2 * (1 - cdfnor(abs(t), 0, 1));  // normal approx
endfunction

p_values = arrayfun(pValueApprox, t_stats);

///////////////////////////////////////////////////////////////////////////////
// 5. DISPLAY RESULTS
///////////////////////////////////////////////////////////////////////////////

disp("================== GAUSSIAN GLM RESULTS ==================");
for j = 1:p
    mprintf("Coefficient %d: beta_hat=%.5f  SE=%.5f  t=%.3f  p=%.6f\n", ...
             j, beta_hat(j), SE_beta(j), t_stats(j), p_values(j));
end
mprintf("\nResidual Sum of Squares (RSS) = %.5f\n", RSS);
mprintf("Degrees of Freedom = %d\n", df);
mprintf("Residual Std. Error = %.5f\n", sigma_hat);

// Optionally compute AIC for Gaussian model: AIC = N*ln(RSS/N) + 2p + constant
// (constant = N*(1+ln(2pi)) is the same across models, so often omitted for comparisons)
AIC = N*log(RSS/N) + 2*p;
mprintf("Approx AIC (ignoring constant) = %.3f\n", AIC);

disp("===========================================================");
