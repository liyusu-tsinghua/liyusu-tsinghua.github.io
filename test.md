- [what is arbitradge](#what-is-arbitradge )
- [from the toy-model to a useful model : binomimal tree](#from-the-toy-model-to-a-useful-model-binomimal-tree )
- [apply the idea from the tree : <img src="https://latex.codecogs.com/gif.latex?&#x5C;Delta"/> hedging and the Black-Schole PDE](#apply-the-idea-from-the-tree-delta-hedging-and-the-black-schole-pde )
- [do some modification to apply the PDE in a more realistic area : Convertiable Bond in Chinese market](#do-some-modification-to-apply-the-pde-in-a-more-realistic-area-convertiable-bond-in-chinese-market )
  - [simplify assumption : coonstant parameters without soft-call or soft-put neither divident , ignoring the transaction cost](#simplify-assumption-coonstant-parameters-without-soft-call-or-soft-put-neither-divident-ignoring-the-transaction-cost )
    - [theory](#theory )
    - [code](#code )
  - [realistic assumption : local parameters and transaction cost as a ressult of special tax machinism in chinese market .](#realistic-assumption-local-parameters-and-transaction-cost-as-a-ressult-of-special-tax-machinism-in-chinese-market )
    - [theory](#theory-1 )
    - [code](#code-1 )
  - [somthing may be add later :](#somthing-may-be-add-later )
- [summary & persinal idea of LI YUSU](#summary-persinal-idea-of-li-yusu )
  
i'll show sth basic in quant's world --non-arbitradge-theory , through Convertiable Bond in Chinese market . this is also one of my homework , i hope this can help you understand the basic math structure in a quant's world . during the ilustration , i assume you hve never met this area , but you should have a basic understanding about PDE . if you just have an interest in this area and a little knowledge about math , but don't know where to start , follow me ,let's go !
  
  
##  what is arbitradge
  
  
in the first beginning , i'll start from a toy-model : from today on , after a week , god drop a coin , if the coin shows upside , tesla's share price will indrease \$100 , else the tesla's shareprice will decrease \$100
  
now you need to price such a chance : if tesla's share price rose , you get the money equle to the Tesla's share price , else you get 100 dollar .
  
intuitionally , the answer is <img src="https://latex.codecogs.com/gif.latex?Price(this%20chance)%20=%200.5%20*%20(S%20+%20100)%20+%200.5%20*%20100"/> where <img src="https://latex.codecogs.com/gif.latex?S"/> is Tesla's share price today ; if you are mor carefully , you may modify the the fomula a little into <img src="https://latex.codecogs.com/gif.latex?Price(this%20chance)%20=0.5%20*%20(S+100)%20+%200.5%20*%20&#x5C;frac{100}{1+r}"/> where <img src="https://latex.codecogs.com/gif.latex?r"/> is the interest rate a bank would pay you if you save \$100 in it for a week . 
  
but is taht true ? 
  
what if we sue a linear combination of tesla's share and some cash in the bank to replace that chance ? say <img src="https://latex.codecogs.com/gif.latex?replace%20=%20&#x5C;alpha%20*%20S%20+%20&#x5C;beta%20*%20cash"/>
  
we get :
  
<img src="https://latex.codecogs.com/gif.latex?replace%20=
&#x5C;begin{cases}
&#x5C;alpha%20*%20(S%20+%20100)%20+%20&#x5C;beta%20*%20cash%20*%20(1+r)%20,%20(god&#x27;s%20&#x5C;%20coin&#x5C;%20%20show%20&#x5C;%20upside%20)%20&#x5C;&#x5C;
&#x5C;alpha%20*%20(S%20-%20100)%20+%20&#x5C;beta%20*%20cash%20*%20(1+r)%20,%20(god&#x27;s%20&#x5C;%20coin&#x5C;%20%20show%20&#x5C;%20downside%20)
&#x5C;end{cases}"/>
  
let <img src="https://latex.codecogs.com/gif.latex?chance%20=%20replace"/> we get 
  
<img src="https://latex.codecogs.com/gif.latex?&#x5C;begin{cases}
&#x5C;alpha%20*%20(S%20+%20100)%20+%20&#x5C;beta%20*%20cash%20*%20(1+r)%20=%20S%20+%20100,%20(god&#x27;s%20&#x5C;%20coin&#x5C;%20%20show%20&#x5C;%20upside%20)%20&#x5C;&#x5C;
&#x5C;alpha%20*%20(S%20-%20100)%20+%20&#x5C;beta%20*%20cash%20*%20(1+r)%20=%20100,%20(god&#x27;s%20&#x5C;%20coin&#x5C;%20%20show%20&#x5C;%20downside%20)
&#x5C;end{cases}"/>
  
that is to say <img src="https://latex.codecogs.com/gif.latex?&#x5C;begin{cases}%20&#x5C;alpha%20=%20&#x5C;frac{S}{200}%20&#x5C;&#x5C;%20&#x5C;beta%20=%20&#x5C;frac{S%20+%20200%20-%20S^2&#x2F;100}{2*(1+r)%20*%20cash}%20&#x5C;end{cases}"/> and <img src="https://latex.codecogs.com/gif.latex?replace%20=%20&#x5C;frac{S^2}{200}%20+%20&#x5C;frac{S%20+%20200%20-%20S^2&#x2F;100}{2*(1+r)%20}"/> obviously that is different from our intuition answer . in fact if we use <img src="https://latex.codecogs.com/gif.latex?&#x5C;phi_{up}%20,%20&#x5C;phi_{down}"/> to denote <img src="https://latex.codecogs.com/gif.latex?S+100%20,%20100"/> , <img src="https://latex.codecogs.com/gif.latex?S_{up}%20,%20S_{down}"/> to denote <img src="https://latex.codecogs.com/gif.latex?S+100,S-100"/> , <img src="https://latex.codecogs.com/gif.latex?S%20*%20(1+r)"/> denoted by <img src="https://latex.codecogs.com/gif.latex?S_{-}"/> , we have:
  
<img src="https://latex.codecogs.com/gif.latex?replace%20=%20&#x5C;frac{S_{m}%20-%20S_{down}}{S_{up}%20-%20S{down}}%20*%20&#x5C;phi%20_{up}%20+%20&#x5C;frac{S_{up}%20-%20S_{m}}{S_{up}%20-%20S{down}}%20*%20&#x5C;phi%20_{down}"/>
  
instead of 
  
<img src="https://latex.codecogs.com/gif.latex?intuition%20=%20p_{up}%20*%20&#x5C;phi_{up}%20+%20%20p_{down}%20*%20&#x5C;phi_{down}"/>
  
in fact the difference between 'replace' and 'intuition' is that the replace is not influnced by the probebility of the coin showing upside . if i use the 'replace' strategy , no matter how our 'god' cheating druing the coin game , we won't lose anthing . in another world , once the price of the chance is different from the price of the <img src="https://latex.codecogs.com/gif.latex?replace"/> we calculate , we can meke profile with a probebility of 100% .
  
  
  
##  from the toy-model to a useful model : binomimal tree
  
  
now you may have a basic understand of arbidradge , here i add a more strict defination : 
if there is a porfolio in the market satisfy <img src="https://latex.codecogs.com/gif.latex?P(V_{t}&#x5C;geq%200)=1{&#x5C;text{%20and%20}}P(V_{t}&#x5C;neq%200)&gt;0&#x5C;,"/> where <img src="https://latex.codecogs.com/gif.latex?V_{0}=0"/> and <img src="https://latex.codecogs.com/gif.latex?V_{t}"/> denotes the portfolio value at time t , we say there is a arbitradge chance .
  
but the up side problem ls so naive , in real world the share price is changing continuiously , how that toy model make sence in reality ?
  
let's observe what if 'god' droping coins every second , and each time price add or lose 0.0001 dollar . then when we observe the price in a 'day' scale , the price just like sth continous , if you know a little about wiener process , that price is just a  --denote the tenor of droping coins <img src="https://latex.codecogs.com/gif.latex?&#x5C;delta%20t"/> , the add amount <img src="https://latex.codecogs.com/gif.latex?&#x5C;mu%20+%20&#x5C;Delta"/> , the lose amount <img src="https://latex.codecogs.com/gif.latex?&#x5C;mu%20-%20&#x5C;Delta"/> , and we have :
  
<img src="https://latex.codecogs.com/gif.latex?dS_t%20=%20&#x5C;frac{&#x5C;mu}{&#x5C;delta%20t}%20*%20dt%20+%20&#x5C;frac{&#x5C;Delta}{&#x5C;delta%20t}%20*%20dw^R"/>
  
where <img src="https://latex.codecogs.com/gif.latex?w^R"/> is a standard wiener process under real world measure. 
  
*a bit boring , i'll complete it later*
  
  
  
##  apply the idea from the tree : <img src="https://latex.codecogs.com/gif.latex?&#x5C;Delta"/> hedging and the Black-Schole PDE
  
  
from principle , a stock price is satisfied by a series of random event , because the amount of the events is so large , we can use a wiener process to describe , 
  
*a bit boring , i'll complete it later*
  
the idea is really similar as what we do in binominal tree (god drop coins in a very high frequency) , we asume in the continus world , we replace every state of our derevative with some cash and some let the none-arbitradge-brice of the derevative be a function of stock price <img src="https://latex.codecogs.com/gif.latex?S"/> and the time <img src="https://latex.codecogs.com/gif.latex?t"/> , apply ito's lamma:
if <img src="https://latex.codecogs.com/gif.latex?dS%20=%20adt%20+%20&#x5C;sigma%20dW"/> than
<img src="https://latex.codecogs.com/gif.latex?dF(S,t)%20=%20(&#x5C;partial_t%20+%20&#x5C;frac{1}{2}&#x5C;sigma^2(S,t)&#x5C;partial_{SS})F(S,t)%20*%20&#x5C;delta%20t%20+%20&#x5C;partial_S%20F(S,t)%20*%20&#x5C;delta%20S"/>
  
<img src="https://latex.codecogs.com/gif.latex?d(&#x5C;alpha(S,t)*S%20+%20&#x5C;beta(S,t)%20*%20cash)%20=%20&#x5C;beta(S,t)%20*%20cash%20*%20r(t)%20*%20&#x5C;delta%20t%20+%20&#x5C;alpha(S,t)*%20&#x5C;delta%20S"/>
  
<img src="https://latex.codecogs.com/gif.latex?&#x5C;delta%20t%20,%20&#x5C;delta%20S"/> can change indeependently , so <img src="https://latex.codecogs.com/gif.latex?&#x5C;begin{cases}&#x5C;alpha(S,t)%20=%20&#x5C;partial_S%20F(S,t)%20%20&#x5C;&#x5C;%20&#x5C;beta(S,t)%20=%20&#x5C;frac{(&#x5C;partial_t%20+%20&#x5C;frac{1}{2}&#x5C;sigma^2(S,t)&#x5C;partial_{SS})F(S,t)}{cash%20*%20r(t)}&#x5C;end{cases}"/> , which is 
  
<img src="https://latex.codecogs.com/gif.latex?F(S,t)%20=S%20&#x5C;partial_S%20F(S,t)%20+%20%20&#x5C;frac{(&#x5C;partial_t%20+%20&#x5C;frac{1}{2}&#x5C;sigma^2(S,t)&#x5C;partial_{SS})F(S,t)}{r(t)}"/>
  
aka Black-Schole fomula. thanks to Faymen , there is a fomula to connect parabolic partial differential equations (PDEs) and stochastic processes :
  
Consider the partial differential equation :
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?&#x5C;frac{&#x5C;partial%20u}{&#x5C;partial%20t}(x,t)%20+%20&#x5C;mu(x,t)%20&#x5C;frac{&#x5C;partial%20u}{&#x5C;partial%20x}(x,t)%20+%20&#x5C;tfrac{1}{2}%20&#x5C;sigma^2(x,t)%20&#x5C;frac{&#x5C;partial^2%20u}{&#x5C;partial%20x^2}(x,t)%20-V(x,t)%20u(x,t)%20+%20f(x,t)%20=%200,"/></p>  
  
  
defined for all <p align="center"><img src="https://latex.codecogs.com/gif.latex?x%20&#x5C;in%20&#x5C;mathbb{R}"/></p>  
 and <p align="center"><img src="https://latex.codecogs.com/gif.latex?t%20&#x5C;in%20[0,%20T]"/></p>  
, subject to the terminal condition
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?u(x,T)=&#x5C;psi(x),"/></p>  
  
  
where μ, σ, ψ, ''V'', ''f'' are known functions, ''T'' is a parameter and <p align="center"><img src="https://latex.codecogs.com/gif.latex?u:&#x5C;mathbb{R}&#x5C;times[0,T]&#x5C;to&#x5C;mathbb{R}"/></p>  
 is the unknown. Then the Feynman–Kac formula tells us that the solution can be written as a [[conditional expectation]]
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?u(x,t)%20=%20E^Q&#x5C;left[%20&#x5C;int_t^T%20e^{-%20%20&#x5C;int_t^r%20V(X_&#x5C;tau,&#x5C;tau)&#x5C;,%20d&#x5C;tau}f(X_r,r)dr%20+%20e^{-&#x5C;int_t^T%20V(X_&#x5C;tau,&#x5C;tau)&#x5C;,%20d&#x5C;tau}&#x5C;psi(X_T)%20&#x5C;,&#x5C;Bigg|&#x5C;,%20X_t=x%20&#x5C;right]"/></p>  
  
  
under the [[probability measure]] Q such that ''X'' is an [[Itô calculus|Itô process]] driven by the equation
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?dX%20=%20&#x5C;mu(X,t)&#x5C;,dt%20+%20&#x5C;sigma(X,t)&#x5C;,dW^Q,"/></p>  
  
  
with <img src="https://latex.codecogs.com/gif.latex?W^Q(t)"/> is a Wiener process (also called Brownian motion) under ''Q'', and the initial condition for X(t) is X(t) = x .
  
if we apply this trick to the B-S PDE , we find the measure Q is just function like the implied probablity we get from the analysis we've done in binominal tree . till here , theroitically we can sove any pricing problem using the idea that *there should not be arbitradge oppotunity* 
  
##  do some modification to apply the PDE in a more realistic area : Convertiable Bond in Chinese market
  
  
after going through this Nobel-level idea , you may wonder how such an obviously idea can win Nobel prize in 1997 , but the fact is that in this area , such an idea wasn't came up with until such a morden time . later i'll shoow some of my personal feeling about that idea . in the part 'summary'
  
but now let's go to a more technically area : how to use this idea to price chinese convertiable bonds.
  
###  simplify assumption : coonstant parameters without soft-call or soft-put neither divident , ignoring the transaction cost
  
  
####  theory
  
  
the first modification is change the terminal condition .
  
during BS model , they simply considering a executive in the final time , hence the terminal condition is just 
<p align="center"><img src="https://latex.codecogs.com/gif.latex?u(x,T)=&#x5C;psi(x),"/></p>  
  
but a convertiable bond can be execute at any time , which we call it American option . traditionally , teachers like to ptice an american option like this:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?A(x,t)%20=%20max&#x5C;{u(x,a)|_{&#x5C;partial_Tu(x,a)%20=%200},u(x,0),u(x,T)&#x5C;}"/></p>  
  
  
here <img src="https://latex.codecogs.com/gif.latex?a"/> is a optimal execute time , 0 denode execute immdietly and T denote execute at the meturity time . however it is not a proper strategy to define a execute time at the begining , because there could be a execute strategy : <img src="https://latex.codecogs.com/gif.latex?f(x,t)%20=%200"/> our , by which we can decide when and in which price we will execute . the valve between the execute noundry should be continue , 
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?F|_f%20=%20&#x5C;phi|_f"/></p>  
  
  
we bring this boundary condition into the BS fomula , to get the expression of the boundary <img src="https://latex.codecogs.com/gif.latex?f(x,t)%20=%200"/>
  
generally speaking , there should not be a analytical boundary , neither a analytical solution of the modified-B-S equation , however there will be a single solution to this equation (i don't have strict analytical provement this part , because i'm not a SDE exepert , however , i know how to solve this equation numercially , so i just say that there is a unique solution) , normally we apply a finite difference to numercially solve this equation . when problem get more complex , we can also use monte-calo to solve it . 
  
####  code
  
  
  
here is my functions file fun.py:
```python
def phi(x,t):
    """
        input two float , x denote price , t denote the time
        return one float , phi denote the execute payoff executing at (x,t)
    """
    r = 0
    # to be code here
    # the payoff of convertibal bond
    return r
  
def solve_the_boundary(f,g,R,Sigma):
    """
        input execute function 'f' and a 2d grids 'g'
        out put the boundary points in the grids
    """
  
    out = np.zeros(shape(g))
    # solve BS(f)|_{boundary} = 0 to get boundary
    # out[next to boundary] = 1
    return out
  
def r(g,R):
    """
        R is the interest rate function the model assumpted
        g is a 2d grade 
    """
    # discreat R to the same shape as g
    out = np.zeros(shape(g))
    #to code
    return out
  
  
def sigma(g,Sigma):
    """
        Sigma is the sigma function the model assumpted
        g is a 2d grade 
    """
    # discrate Sigma to the same shape as g
    out = np.zeros(shape(g))
    #to code
    return out
  
def FD_solve(fg,rg,sigmag,boundaryg):
    """
        fg is the grid should be out put ,
        rg is the input interest rate grids
        sigmag is the sigma grids
        boundaryg is the boundary grids
    """
    #to be code
    return fg
```
  
under the the simplist assumption:
we have simple.py:
```python
import numpy as np
import matplotlib.pyplot as plt
import fun
  
# argv to import
x0 = arg[0]
xi = arg[1]
  
#use 1000 time points and 10000 price points
fg = np.array((1000,10000))
st
# interest rate is constant
def R(x,y):
    return x0
  
# sigma is constant
def Sigma(x,y):
    return x1
  
#plot boundary 
## to be modify
boundaryg = solve_the_boundary(phi,fg,R,Sigma)
plt.plot()
plt.points(st[boundary==1])
plt.savefig(arg[3])
  
#price
rg = r(fg,R)
sigmag = sigma(fg,Sigma)
FD_solve(fg,rg,sigmag,boundaryg)
  
#3dplot
# to code
plt.savefig(arg[4])
```
  
  
  
###  realistic assumption : local parameters and transaction cost as a ressult of special tax machinism in chinese market .
  
  
####  theory
  
theoritically speaking , local r and local <img src="https://latex.codecogs.com/gif.latex?&#x5C;sigma"/> don't need specific modification , but soft-call soft-put and dividend need , transaction fees(tax) , require a strong modifycation .
  
we do things step by step . 
  
first we input the market interest rate and market sigma , instead of the constent we used in last part , and recalculate the price , to judge how much difference are there between our reimplied price and the market price .
  
second , let's consider transaction cost : if there is transaction cost , we can't apply <img src="https://latex.codecogs.com/gif.latex?&#x5C;Delta"/> hedging like before , because changing \Delta will cost additinaol money , so only if we can sell(/buy) the convertiable bond in a higher(/lower) price can we get arbitradge oppotunity .
  
####  code
  
  
```python
# replace R and Sigma with the market data
```
  
###  somthing may be add later :
  
  
1. Con-bond can add 20% a day in china , but the common share 10% .
2. shot stock cost more money in china
3. soft-call soft-put is not easy to be inclouded in the model , because , it's a strong path deependent . we may try monte-carlo later 
  
##  summary & persinal idea of LI YUSU
  
