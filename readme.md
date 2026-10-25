# Feature selection based on whale optimization algorithm [with MATLAB code]

For general classification problems, it is generally required to ensure accuracy and improve algorithm running speed. This necessitates maximizing the use of feature values. The lower the dimensionality of features, the faster the algorithm can run. Therefore, this paper proposes a whale optimization-based feature selection method. The specific theory and code are as follows:
MATLAB Code:
main
%---------------------------------------------------------------------%\n%  Whale Optimization Algorithm (WOA) source codes demo version       %\n%---------------------------------------------------------------------%\n​\n​\n%---Inputs-----------------------------------------------------------\n% feat     : feature vector ( Instances x Features )\n% label    : label vector ( Instances x 1 )\n% N        : Number of whales\n% max_Iter : Maximum number of iterations\n% b        : Constant \n​\n%---Output-----------------------------------------------------------\n% sFeat    : Selected features (instances x features)\n% Sf       : Selected feature index\n% Nf       : Number of selected features\n% curve    : Convergence curve\n%--------------------------------------------------------------------\n​\n​\n%% Whale Optimization Algorithm\nclc, clear, close; \n% Benchmark data set \nload ionosphere.mat; \n​\n% Set 20% data as validation set\nho = 0.2; \n% Hold-out method\nHO = cvpartition(label,'HoldOut',ho); \n​\n% Parameter setting\nN        = 10; \nmax_Iter = 100; \n​\n% Whale Optimization Algorithm\n[sFeat,Sf,Nf,curve] = jWOA(feat,label,N,max_Iter,HO); \n​\n% Accuracy\nAcc = jKNN(sFeat,label,HO); \nfprintf('\n Accuracy: %g %%',Acc); \n​\n% Plot convergence curve\nplot(1:max_Iter,curve); \nxlabel('Number of Iterations'); \nylabel('Fitness Value'); \ntitle('WOA'); grid on; WOA function
​\nfunction [sFeat,Sf,Nf,curve] = jWOA(feat,label,N,max_Iter,HO)\n% Parameters\nlb  = 0; \nub  = 1; \nb   = 1; \n​\nfun = @jFitnessFunction; \ndim = size(feat,2); \nX   = zeros(N,dim); \nfor i = 1:N\n  for d = 1:dim\n    X(i,d) = lb + (ub - lb) * rand(); \n  end\nend\nfit  = zeros(1,N); \nfitG = inf; \nfor i = 1:N\n  fit(i) = fun(feat,label,(X(i,:) > 0.5),HO); \n  if fit(i) < fitG\n    fitG = fit(i); \n    Xgb  = X(i,:); \n  end\nend\n​\ncurve = inf; \nt = 1; \n%---Iteration start---------------------------------------------------\nwhile t <= max_Iter\n  a = 2 - t * (2 / max_Iter); \n  for i = 1:N\n    A = 2 * a * rand() - a; \n    C = 2 * rand(); \n    p = rand(); \n    l = -1 + 2 * rand(); \n    if p  < 0.5\n      if abs(A) < 1\n        for d = 1:dim\n          Dx     = abs(C * Xgb(d) - X(i,d)); \n          X(i,d) = Xgb(d) - A * Dx; \n        end\n      elseif abs(A) >= 1\n        for d = 1:dim\n          k      = randi([1,N]); \n          Dx     = abs(C * X(k,d) - X(i,d)); \n          X(i,d) = X(k,d) - A * Dx; \n        end\n      end\n    elseif p >= 0.5\n      for d = 1:dim\n        dist   = abs(Xgb(d) - X(i,d)); \n        X(i,d) = dist * exp(b * l) * cos(2 * pi * l) + Xgb(d); \n      end\n    end\n    XB = X(i,:); XB(XB > ub) = ub; XB(XB < lb) = lb; \n    X(i,:) = XB; \n  end\n  for i = 1:N\n    fit(i) = fun(feat,label,(X(i,:) > 0.5),HO); \n    if fit(i) < fitG\n      fitG = fit(i); \n      Xgb  = X(i,:); \n    end\n  end\n  curve(t) = fitG; \n  fprintf('\nIteration %d Best (WOA)= %f',t,curve(t))\n  t = t + 1; \nend\nPos   = 1:dim; \nSf    = Pos((Xgb > 0.5) == 1); \nNf    = length(Sf); \nsFeat = feat(:,Sf); Simulation results
The simulation results show that the original data dimension is 351*34, and after WOA feature selection, the data dimension becomes 351*3. According to the KNN classification results, it is evident that after WOA optimization algorithm feature selection, the remaining features can still ensure an accuracy of 94.2857%, greatly reducing the complexity of the algorithm.

## Images

![Feature selection based on whale optimization algorithm [with MATLAB code]](01.jpg)
![Feature selection based on whale optimization algorithm [with MATLAB code]](02.jpg)
![Feature selection based on whale optimization algorithm [with MATLAB code]](03.jpg)
![Feature selection based on whale optimization algorithm [with MATLAB code]](04.jpg)
![Feature selection based on whale optimization algorithm [with MATLAB code]](05.jpg)
![Feature selection based on whale optimization algorithm [with MATLAB code]](06.jpg)
![Feature selection based on whale optimization algorithm [with MATLAB code]](07.jpg)
![Feature selection based on whale optimization algorithm [with MATLAB code]](08.jpg)
![Feature selection based on whale optimization algorithm [with MATLAB code]](09.jpg)
![Feature selection based on whale optimization algorithm [with MATLAB code]](10.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
