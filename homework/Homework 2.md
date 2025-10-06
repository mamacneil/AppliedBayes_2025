Using the Waffle House data set


You can use the below code to load the data set in R. For python users, best to download the CSV from our github repo and load it using pandas.
```{r}
library(rethinking)
data(WaffleDivorce)
waffle_data <- WaffleDivorce
```

1. (12 points) Develop two statistical models, one to predict divorce rates using median age at marriage as a predictor, and another using marriage rates as a predictor. Using ***unstandardized*** data.
   - For each model, provide:
      - (2 points) A DAG explaining the causal relationship between the two variables. 
      - (2 points) Bayesian notation of each model.
        - Explain, what is the significance of your intercept and slope parameters in each model?
      - (2 points) Prior prediction plots to justify your choice of priors. (why did you choose these priors?)
      - (2 points) A summary of the posterior distributions of the parameters. (mean, sd, HPDI)
        - Both a table output and a plot of the posterior distributions for each parameter
      - (2 points) Posterior predictive plots for each model.
      - (2 points) Final regressions plots with your data.

2. (6 points) Now, combine both predictors into a single multivariate linear regression model.
   - For this model, provide:
     - (1 point) A DAG explaining the causal relationship between the three variables.
     - (1 point) Bayesian notation of the model.
       - Explain, what is the significance of your intercept and slope parameters in this model?
     - (1 point) Prior prediction plots to justify your choice of priors. (why did you choose these priors?)
     - (1 point) A summary of the posterior distributions of the parameters. (mean, sd, HPDI)
       - Both a table output and a plot of the posterior distributions for each parameter
     - (1 point) Posterior predictive plots for the model.
     - (1 point) Final regression plots with your data.

3. (4 points) Below, I have given you two models, represented in different ways.
    - (a) A coffee sales model in Bayesian notation.
    - (b) A plant growth model in R/Python code.
    - Your task is to translate each model into the other notation, and explain the following.
      - What does each line in the model represent?
      - What is the significance of each parameter in the model?
      - What are the prior distributions for each parameter, and why were they chosen?
      - What sort of values are expected from each prior? What effect (positive/negative) would this have one the response variable?
      - What is the likelihood function for the data, and why was it chosen?
    - You do NOT need to fit either model, just translate them.
    - If you need to visualise the distributions being used in the priors, you can always use Probability Playground https://www.acsu.buffalo.edu/~adamcunn/probability/probability.html or simulate from them (but you don't need to show this)


Model (a) Coffee sales model in Bayesian notation:
```{latex}
\begin{align*}
Sales_i &\sim \text{Normal}(\mu_i, \sigma) \\
\mu_i &= \alpha + \beta_1 \cdot Outside_temperature_i + \beta_2 \cdot Number_of_students_i \\
\alpha &\sim \text{Normal}(50, 20) \\
\beta_1 &\sim \text{Normal}(-5, 1) \\
\beta_2 &\sim \text{Normal}(10, 1) \\
\sigma &\sim \text{Exponential}(1)
\end{align*}
```

Model (b) Plant growth model in R/Python code:
```{r}
plant_growth_model <- ulam(
  alist(
    Growth ~ dnorm(mu, sigma),
    mu <- alpha + beta_1 * Sunlight + beta_2 * Water,
    alpha ~ dnorm(50, 10),
    beta_1 ~ dnorm(3, 1),
    beta_2 ~ dnorm(2, 1),
    sigma ~ dexp(1)
  ),
  data = plant_data,
)
```

```{python}
with pm.Model() as plant_growth_model:
    alpha = pm.Normal('alpha', mu=50, sigma=10)
    beta_1 = pm.Normal('beta_1', mu=3, sigma=1)
    beta_2 = pm.Normal('beta_2', mu=2, sigma=1)
    sigma = pm.Exponential('sigma', lam=1)
    mu = alpha + beta_1 * Sunlight + beta_2 * Water   
    Growth = pm.Normal('Growth', mu=mu, sigma=sigma, observed=plant_data['Growth'])
```
