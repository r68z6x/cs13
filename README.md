java c
Numerical Methods in Mathematical Finance.
Project:
Valuation and Sensitivities of a Bermudan Option
using a Monte-Carlo Simulation of
Local and Stochastic Volatility Models
1    IntroductionIn this project, we combine three aspects:  the valuation of Bermudan options, the approximation of partial derivatives (sensitivities), and stochastic volatility models. We consider combinations of these aspects exhibiting some interesting problems:
•  The valuation of a Bermudan option: Improving the valuation using a control variate. The valuation of a Bermudan option on an Asian underlying, where a sensible choice of basis functions should be made.
•  The calculation of sensitivities of a Bermudan option: Here, you may explore a neat trick to improve the accuracy.
•  The valuation of a Bermudan option under a Stochastic Volatility Model. Here again, a sensible choice of basis functions should be made.
You do not need to complete all tasks.
1.1   Aim of the Project
The aim of the project is to perform. a collaborative development of the algorithms, including theory, documentation, analysis and testing.
Your project solution should consider
•  writing clean, concise, elegant and easy to understand code, avoid codeduplica- tion (DRY)
•  write unit tests (e.g. using JUnit)
•  document the code (e.g. using JavaDoc).
It is not the primary aim to get an accurate solution. Designing the code, you may consider:
•  Your solution should try to follow the open-closed-principle: open for extentions, closed for modiﬁcation. Example: consider extending the model from one asset to many assets.
•  The type hierarchy of your models, products and test should follow the Liskov substitution principle: Examples: products can be valued with different models; tests can be run with different models and products.
1.2    Evaluation of the Project Results
To successfully pass the review of the project we may ask for a short presentation of a part of the solution and ask you for answering a set of project related questions.Note: The implementation part of the project can be solved very elegantly using object oriented implementation techniques requiring only few lines of new code. We encourage you to discuss your ideas in order to improve you solution.
1.3   Submission of the Solution
Each working group will receive its own password protected Git repository.
The solution has to be submitted to the repository 24 hours before the ﬁrst
presentation date that has been agreed.
We will ask each group to present some parts of their solution and perform. a short evaluation.
1.4   Working with the Git Repository
For an introduction to Git we recommend https://try.github.io/
Remark
DON’T PANIC. WE WILL TRY TO ASSIST YOU!
2   Theoretical Background
2.1    Bermudan OptionsA ﬁnancial product is called Bermudan if it has multiple exercise dates (options),i.e., there are times Ti  at which the holder of a Bermudan may choose between different payments or ﬁnancial products (underlyings).
A more formal deﬁnition of the Bermudan option is given in the following deﬁnition:Let {Ti}i=1;...;n denote a set of exercise dates and {Vunderl;i}i=1;...;n a corresponding set of ﬁnancial products, which we call underlyings. The Bermudan option is the right to receive at one and only one time Ti  the corresponding underlying Vunderl;i  (with i = 1, . . . , n) or receive nothing.Anticipating the result that the optimal exercise is to choose the maximum of the exercise and nonexercise value, we ﬁnd that at each exercise date Ti, the optimal strategy compares the value of the product upon exercise with the value of the product upon nonexercise and chooses the larger one. Thus the value of the Bermudan is given recursively 
exercise dates                                              exercise dates                  received upon
Ti ; . . . ; Tn                                                                       Ti+1 ; . . . ; Tn                          exercise in Ti
where  Vberm(Tn; Tn)  :=  0 and  Vunderl;i(Ti) denotes  the value  of the underlying Vunderl;i at exercise date Ti.
2.2    Bermudan Equity Options
A simple example of the Bermudan option on a stock S is given by Vunderl;i(Ti)  :=  Ni · (S(Ti) — Ki) ,
where Ni is the notional, here a unit less constant, and Ki is the strike price, a constant in currency unit.
This ﬁnancial product is implemented in ﬁnmath lib in
net .finmath .montecarlo .assetderivativevaluation .products . BermudanOption
2.3    Heston Model - Stochastic Volatility Model
The Heston model is an extension of the Black-Scholes model where the volatility parameter is stochastic. In the Heston model the stock S follows the SDE
with
dN(t)  =  rN(t)dt                                                N(0) = N0
under the corresponding martingale measure QN . The Heston model has the parameters
•  S0: initial value of the stock,
•  r: interest rate (constant),
•  σ: initial value of the volatility,
•  ξ: volatility of volatility,
•  κ: mean reversion speed of the volatility,
•  θ: mean reversion level of the volatility,
•  ρ: correlation of volatility (squared) and stock.Obviously, for ξ = κ = 0 the model is identical to the Black-Scholes model with constant volatility σ and for ξ = 0 the model is identical to the Black-Scholes model with time-dependent (but non-stochastic) volatility.
There is an implementation of the Heston model in the class HestonModel in the package net .finmath .montecarlo .assetderivativevaluation
2.4    Local Volatility ModelsA local volatility model is a model where the volatility is a function of time t and the current value of the underlying S(t). Since S(t) is a random variable, the volatility can be random (stochastic), but in contrast to a Heston model, the volatility does not have an independent Brownian driver dW2 .
The local volatility model is
dS(t)  =  rS(t)dt + σloc(t, S(t))dW (t),       S(0) = S0 .
2.4.1    Local Volatility and Implied VolatilityIt is important to distinguish local volatility and implied volatility: The local volatility σloc is a model parameter - it may be a function, a function of (t, S) (simulation time t and the state of the process S = S(t)).The implied volatility comes as "implied Black-Scholes volatility" or "implied Bachelier volatility" and is an alternative description of the value of a call option C(T, K), where T is the maturity and K is the strike. The implied volatility is a function of (T, K).For a given call option value C(T, K), the implied Black-Scholes volatility σL (T, K) is the value which has to be used as constant volatility in the Black-Scholes model (for- mula) to obtain the value C(T, K) (invert the Black-Scholes formula). The superscript. L is for lognormal.For a given call option value C(T, K), the implied Bachelier volatility σN (T, K)) is the value which has to be used as constant volatility in the Bachelier model (formula) to optain the value C(T, K) (invert the Bachelier formula). The superscript. N is for normal.
You can ﬁnd conversion from volatility to value and back in the class
net .finmath .functions .AnalyticFormulas .
2.5    Local Volatility Models
2.5.1   CEV Model
The CEV model assumes the local volatility to be
σloc(t, S)  =  Sβ,        for 0 ≤ β ≤ 1.
2.6    Difference of Local Volatility and Stochastic VolatilityNote that, if you calculate a local volatility function from option values C(T, K) which are generated from a (Heston) stochastic volatility model, then the local volatility model and the stochastic volatility model produce the same option values, but they are still different (in other aspects).
3   Software Tools and Libraries
3.1   ﬁnmath lib
•  You should implement your solution using the interfaces provided in ﬁnmath lib. This is just to make your solutions interoperable. Please checkout the version at
https://github.com/finmath/finmath-lib
•  You may use anything else from ﬁnmath lib and if your solution is very short due to heavy re-use of existing code, then this is good.
•  You may also re-write classes, if you see a good reason.
3.2   Apache commons-math
•  The Apache commons-math library contains a SobolSequenceGenerator. Since commons-mathis a dependency in ﬁmath-lib, including ﬁnmath-lib will make commons-math available to you.
3.3   Visualization of Results
•  We provide a small project which allows easily create plot in Java at https://    github.com/finmath/finmath-lib-plot-extensio代 写Numerical Methods in Mathematical FinanceJava
代做程序编程语言ns. The project provides wrapper code for JFreeChart, JFreeSVG JZY3D and iText).
4   Bermudan Options
4.1   Task: Control Variate for (Equity) Bermudan OptionsThe exercises in this section focus on the implementation of a control variate and  combine it with Bermudan options and sensitivities (partial derivatives). They do not  require special knowledge about the internal implementation of the Bermudan option. You can use the Bermudan option implemented in ﬁnmath lib in
net .finmath .montecarlo .assetderivativevaluation .products . BermudanOption
as a “blackbox” . The valuation gives you the random variable X for which you can deﬁne the control.
Exercise 1 (Control Variate for a Bermudan Option):
1.  Implement a generic control variate for a Bermudan option under the Black- Scholes model.
2.  Perform. numerical experiments illustrating the variance reduction (if any) of the method.
Notes:
•  This exercise is also a bit about software design. You could create a generic class for your control that just consumes the Bermudan option.
•  The constructor of your control could use the Bermudan option as an argu- ment, then ask that Bermudan option for the double[]  exerciseDates,
double[]  notionals, double[]  strikes via the corresponding getters (e.g., double[]  getExerciseDates()), then use that to build a control     variate.
•  If the class implementing the controlled Bermudan option holds a reference to the Bermudan option, it’s getValue-method may ﬁrst call the Bermudan’s getValue-method to build the uncontrolled random variable X , then calculate the controlled one.
Exercise 2 (Control Variate for a Bermudan Option under a Heston Model):In the lecture it was mentioned that the variance reduction via a control variate has the disadvantage of being model dependent. This appears obvious because the control requires knowledge of the analytic value of E(Y). However, the random variable Y could be the valuation of a different product under a different model, such that both, the product and the model are close to the original product and the original model.
1.  Implement a generic control variate for a Bermudan option under the Heston model, where the control performs the valuation under a Black-Scholes model, where the parameters of the Black-Scholes model are chosen such that the model is presumably close to the Heston model.
2.  Perform. numerical experiments illustrating the variance reduction (if any) of the method.
3.  Optional: Does the method work, if you calculate the volatility of the Black- Scholes model numerically by valuing a European option under the Heston  model (via Monte-Carlo), and then calculate the implied volatility from that  Monte-Carlo value? (If it does not work, why?)
Exercise 3 (Control Variate for the Delta of a Bermudan Option):
1.  Implement a generic control variate for the Delta-Sensitivity of a Bermudan
option under the Black-Scholes model.   The Delta-Sensitivity is the partial
derivative of the valuation with respect to the initial value of the stock,i.e.,  .
2.  Perform. numerical experiments illustrating the variance reduction (if any) of the method.
4.2   Task: Implement a Valuation of a Generalized Bermudan Option
Exercise 4 (Generalized Bermudan Option Valuation via Regression Method):
1.  Implement a Bermudan option valuation under an
AssetModelMonteCarloSimulationModel
using a general design that allows to pass different underlyings. Implement the interface
AssetMonteCarloProduct
with a Bermudan taking a List of exercise times {Ti  j i = 1, . . . , n} and a List of
underlying products {Vunderl.,i  j i = 1, . . . , n} implementing AssetMonteCarloProduct.
2.  Implement the backward algorithm using the conditional expectation estima- tor MonteCarloConditionalExpectationRegression with a set of basis functions.
3.  Compare your result with that of the BermudanOption that is already part of the library.
Notes:
•  You may consider different ways of providing suitable basis functions.  This might be useful for other exercises.
4.3   Task: Implement a Valuation of a Bermudan Option on a (Series) of Asian Options
Exercise 5 (Bermudan Option on a Series of Asian Options):
1.  Implement a Bermudan option valuation under an
AssetModelMonteCarloSimulationModel.
That is: the model simulates the value of a single stock.For that, implement a Bermudan option where the underlying products, in which  the Bermudan may exercise at one (at most one) exercise time), are Asian options. That is
•  the product constructor takes a List of exercise times {Tie  j i = 1, . . . , n}.
•  the product constructor takes a List of payment times {Tip  j i = 1, . . . , n}.
•  the product constructor takes a List of option strikes {Ki  j i = 1, . . . , n}.
•  the product constructor takes a List of List of averaging times {Tia;j  j j = 1, . . . , mi; i = 1, . . . , n}.
•  The Berumdan allows to exercise in time Tie into an Asian option paying
max(Ai - Ki, 0) in Tip , where A i  :=  ε1 S(Tia;j).
•  You may assume that Tie  = Tip  and that Tia;mi   ≤ Tip.
2.  Implement the interface
AssetMonteCarloProduct
that allows to value your product with a given
AssetModelMonteCarloSimulationModel.
3.  Implement a more general version that allows Tie  < Tip  (that is, Tia;mi   > Tie is allowed).
Notes:
•  If you struggle in creating an implementation as general as the one above, feel  free to implement a simpler version of the product. We will honour your efforts.
•  It you implement the more general version in Number 2, that counts as a solution of Number 1 (no need to do both).
•  You may try to reduce the coding and use the solution of the previous exercise (if you did that one) 
4.4   Task: Testing
Exercise 6 (Unit Test for a Bermudan Option):
1.  Write unit tests for your Bermudan option implementation.
Notes:
•  Think about good test cases. Do you know “limit cases” where the product agrees with some other product of which you have an independent implementation? Do you know “limit cases” where you known an analytic formula?
5   Stochastic Volatility and Bermudan Options
5.1   Task: Valuation of a Bermudan Option under a Stochastic Volatility
Model
Exercise 7 (Bermudan Option under a Stochastic Volatility Model):
1.  Value a Bermudan option under a stochastic volatility model, e.g., the Heston model. Analyse how the valuation depends on
•  the model parameters,
•  the choice of the regression basis functions of the conditional expectation estimator (using linear regression).
2.  (little bit more advances, optional): Value the same Bermudan option under a CEV model, where you try to choose the parameters of the CEV model such that the implied volatility of the European options is a similar a possible to the stochastic volatility model.  The idea here is that we have two models which generate (almost) the same values (prices) for European options, but different values for Bermudan options.
Remark:   You may use the following model parameters
S0 = 100, r = 5%, σ = 30%, κ = 0.2, θ = σ 2 , ξ = 20%, ρ = 0.
Notes:
•  There is a design issue here: What are good designs to provide the basis functions for different models?
6   Sensitivities and Bermudan Options
6.1   Task: Calculate Sensitivities of a Bermudan Option
Exercise 8 (Sensitivities of a Bermudan Option):
1.  Calculate the partial derivative of (Monte-Carlo) valuation of a Bermudan option with respect to some of the model parameters, e.g., the initial value of the stock (the delta).
2.  Think about improving the accuracy of the approximation.  Implement your ideas and analyse the accuracy of your numerical approximation of the partial derivative .
•  Do you know a random variable that should stay constant under an inﬁnites- imal shift of the model / product parameters, but that keeping it ﬁx could  result in a variance reduction?
3.  Calculate the vega sensitivity (here we mean the partial derivative with respect to the parameter σ in the Black-Scholes or Heston model). Analyse the dependency on the strike(s).
7    RemarksIn all exercises, try to analyse your solution by checking how the quantities (valuations, sensitivities) depend on model and product parameter. Visualize the result. Interpret your result. In your presentation - focus on plots and analysis from which you could draw some conclusion or which brings up some open questions.




         
加QQ：99515681  WX：codinghelp
