# The Bias-Variance Tradeoff: Choosing How Flexible a Model Should Be

## Objective
I used a simulated world with a known true curve to see how a polynomial's degree trades off bias against variance, and to pick a degree with cross-validation instead of a single split.

## Methodology
- Simulated data from the curve sin(1.4x) + 0.25x with noise (standard deviation 0.45), and drew one training sample of 60 points.
- Split the error of polynomial fits into bias squared and variance by fitting 200 fresh samples at degrees 1, 2, 3, 5, 9 and 15 and comparing the predictions with the true curve.
- Fit degrees 1 to 15 on the 60 training points and compared each one's training error with its error on 2,000 new points from the same world.
- Chose a degree with 5-fold cross-validation over degrees 1 to 11 and looked at how much the error varied from fold to fold.
- Ran nested cross-validation to check whether the smallest CV error is a fair estimate of the error on new data.
- Wrote a function, kfold_mse(), that gives the k-fold CV error for any degree and number of folds, and used it to compare 5-fold and 10-fold results for degrees 4 and 5.

## Key Findings
Variance stayed low through degree 5 at 0.0180 and then climbed to 0.0994 at degree 15, while bias squared dropped close to zero, so the extra flexibility past degree 5 mostly added variance. The held-out error was lowest at degree 5, and 5-fold CV chose degree 4 with a CV MSE of 0.1401, which is close to the same answer without needing 2,000 extra points. The nested CV estimate came out at 0.2070, higher than the 0.1401 from Step 6, which shows that the smallest CV error is too optimistic once you've used it to pick the degree. With 10 folds, the CV MSE was 0.1409 for degree 4 and 0.1457 for degree 5, so degree 4 still came out slightly ahead, the same ranking that 5-fold CV gave.
