# Statistical Analysis
##  Regression
### Linear regression on  groups





```r
kable(mtcars %>%  group_by(as.factor(gear)) %>%
summarise(mean = mean(qsec), sd = sd(qsec))) %>%
  kable_styling(full_width = F) %>%
  kable_minimal()

#Run the same linear regression model by group levels? 
#Instead of running #summary(lm(y~x)) for the number of levels 
#you have, you can use the R package “broom” along with dplyr.

# Run the same regression model for gears ##
kable(mtcars%>% group_by(gear) %>%
  do(fitgear = glance(lm(hp~qsec, data = .))) %>% 
  unnest(fitgear),digits=2) %>%   kable_styling(full_width = F) %>% 
  kable_minimal()
```

\begin{table}
\centering
\begin{tabular}{l|r|r}
\hline
as.factor(gear) & mean & sd\\
\hline
3 & 17.692 & 1.349916\\
\hline
4 & 18.965 & 1.613880\\
\hline
5 & 15.640 & 1.130487\\
\hline
\end{tabular}
\end{table}

\begin{table}
\centering
\begin{tabular}{r|r|r|r|r|r|r|r|r|r|r|r|r}
\hline
gear & r.squared & adj.r.squared & sigma & statistic & p.value & df & logLik & AIC & BIC & deviance & df.residual & nobs\\
\hline
3 & 0.66 & 0.63 & 28.87 & 25.19 & 0.00 & 1 & -70.65 & 147.31 & 149.43 & 10837.12 & 13 & 15\\
\hline
4 & 0.10 & 0.01 & 25.72 & 1.15 & 0.31 & 1 & -54.90 & 115.80 & 117.26 & 6616.46 & 10 & 12\\
\hline
5 & 0.88 & 0.83 & 41.95 & 21.03 & 0.02 & 1 & -24.50 & 55.00 & 53.83 & 5279.95 & 3 & 5\\
\hline
\end{tabular}
\end{table}
[Reference](https://stackoverflow.com/questions/22713325/fitting-several-regression-models-with-dplyr)

## Logistic Regression
### to be completed


## Survival Analysis and Visualisation
### To be completed

## Receiver Operated Curves (ROC)
### To be completed


