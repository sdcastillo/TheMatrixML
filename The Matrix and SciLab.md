# The Matrix and SciLab

## R Code

You can run this code using **R** and **RStudio**. Place the **`train.csv`** file in your working directory, then load and fit a Gaussian GLM as follows:

```r
train <- read.csv("train.csv")

GLM <- glm(
  formula = Crash_Score ~ . + Work_Area * Rd_Class - Month, 
  family  = gaussian(),
  data    = train
)

summary(GLM)
