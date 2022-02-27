---
layout: post
read_time: true
show_date: true
title:  "some basic idea in quant finance world learning from Convertiable Bond in Chinese market ."
date:   2022-02-23 
img: posts/20220223/tobeupload.png  #posts/20210402/post7-header.webp
tags: [QF],[basic idea],[homework],[qf5202],[ma4269]
author: LI YUSU
github:  liyusu-tsinghua/github.io
mathjax: yes
description: i'll show sth basic in quant's world --non-arbitradge-theory , through Convertiable Bond in Chinese market . this is also one of my homework , i hope this can help you understand the basic math structure in a quant's world . during the ilustration , i assume you hve never met this area , but you should have a basic understanding about PDE . if you just have an interest in this area and a little knowledge about math , but don't know where to start , follow me ,let's go !
---

[toc]

i'll show sth basic in quant's world --non-arbitradge-theory , through Convertiable Bond in Chinese market . this is also one of my homework , i hope this can help you understand the basic math structure in a quant's world . during the ilustration , i assume you hve never met this area , but you should have a basic understanding about PDE . if you just have an interest in this area and a little knowledge about math , but don't know where to start , follow me ,let's go !


## what is arbitradge

in the first beginning , i'll start from a toy-model : from today on , after a week , god drop a coin , if the coin shows upside , tesla's share price will indrease \$100 , else the tesla's shareprice will decrease \$100

now you need to price such a chance : if tesla's share price rose , you get the money equle to the Tesla's share price , else you get 100 dollar .

intuitionally , the answer is $Price(this chance) = 0.5 * (S + 100) + 0.5 * 100$ where $S$ is Tesla's share price today ; if you are mor carefully , you may modify the the fomula a little into $Price(this chance) =0.5 * (S+100) + 0.5 * \frac{100}{1+r} $ where $r$ is the interest rate a bank would pay you if you save \$100 in it for a week . 

but is taht true ? 

what if we sue a linear combination of tesla's share and some cash in the bank to replace that chance ? say $replace = \alpha * S + \beta * cash$

we get :

$
replace =
\begin{cases}
\alpha * (S + 100) + \beta * cash * (1+r) , (god's \ coin\  show \ upside ) \\
\alpha * (S - 100) + \beta * cash * (1+r) , (god's \ coin\  show \ downside )
\end{cases}
$

let $chance = replace$ we get 

$
\begin{cases}
\alpha * (S + 100) + \beta * cash * (1+r) = S + 100, (god's \ coin\  show \ upside ) \\
\alpha * (S - 100) + \beta * cash * (1+r) = 100, (god's \ coin\  show \ downside )
\end{cases}
$

that is to say $\begin{cases} \alpha = \frac{S}{200} \\ \beta = \frac{S + 200 - S^2/100}{2*(1+r) * cash} \end{cases}$ and $replace = \frac{S^2}{200} + \frac{S + 200 - S^2/100}{2*(1+r) } $ obviously that is different from our intuition answer . in fact if we use $\phi_{up} , \phi_{down} $ to denote $S+100 , 100$ , $S_{up} , S_{down}$ to denote $S+100,S-100$ , $ S * (1+r)$ denoted by $S_{-}$ , we have:

$
replace = \frac{S_{m} - S_{down}}{S_{up} - S{down}} * \phi _{up} + \frac{S_{up} - S_{m}}{S_{up} - S{down}} * \phi _{down}
$

instead of 

$
intuition = p_{up} * \phi_{up} +  p_{down} * \phi_{down}
$

in fact the difference between 'replace' and 'intuition' is that the replace is not influnced by the probebility of the coin showing upside . if i use the 'replace' strategy , no matter how our 'god' cheating druing the coin game , we won't lose anthing . in another world , once the price of the chance is different from the price of the $replace$ we calculate , we can meke profile with a probebility of 100% .



## from the toy-model to a useful model : binomimal tree

now you may have a basic understand of arbidradge , here i add a more strict defination : 
if there is a porfolio in the market satisfy $P(V_{t}\geq 0)=1{\text{ and }}P(V_{t}\neq 0)>0\,$ where $V_{0}=0$ and $V_{t}$ denotes the portfolio value at time t , we say there is a arbitradge chance .

but the up side problem ls so naive , in real world the share price is changing continuiously , how that toy model make sence in reality ?

let's observe what if 'god' droping coins every second , and each time price add or lose 0.0001 dollar . then when we observe the price in a 'day' scale , the price just like sth continous , if you know a little about wiener process , that price is just a  --denote the tenor of droping coins $\delta t$ , the add amount $\mu + \Delta$ , the lose amount $\mu - \Delta$ , and we have :

$dS_t = \frac{\mu}{\delta t} * dt + \frac{\Delta}{\delta t} * dw^R$

where $w^R$ is a standard wiener process under real world measure. 

*a bit boring , i'll complete it later*



## apply the idea from the tree : $\Delta$ hedging and the Black-Schole PDE

from principle , a stock price is satisfied by a series of random event , because the amount of the events is so large , we can use a wiener process to describe , 

*a bit boring , i'll complete it later*

the idea is really similar as what we do in binominal tree (god drop coins in a very high frequency) , we asume in the continus world , we replace every state of our derevative with some cash and some let the none-arbitradge-brice of the derevative be a function of stock price $S$ and the time $t$ , apply ito's lamma:
if $dS = adt + \sigma dW$ than
$
dF(S,t) = (\partial_t + \frac{1}{2}\sigma^2(S,t)\partial_{SS})F(S,t) * \delta t + \partial_S F(S,t) * \delta S
$

$
d(\alpha(S,t)*S + \beta(S,t) * cash) = \beta(S,t) * cash * r(t) * \delta t + \alpha(S,t)* \delta S 
$

$\delta t , \delta S $ can change indeependently , so $\begin{cases}\alpha(S,t) = \partial_S F(S,t)  \\ \beta(S,t) = \frac{(\partial_t + \frac{1}{2}\sigma^2(S,t)\partial_{SS})F(S,t)}{cash * r(t)}\end{cases}$ , which is 

$
F(S,t) =S \partial_S F(S,t) +  \frac{(\partial_t + \frac{1}{2}\sigma^2(S,t)\partial_{SS})F(S,t)}{r(t)}
$

aka Black-Schole fomula. thanks to Faymen , there is a fomula to connect parabolic partial differential equations (PDEs) and stochastic processes :

Consider the partial differential equation :

$$
\frac{\partial u}{\partial t}(x,t) + \mu(x,t) \frac{\partial u}{\partial x}(x,t) + \tfrac{1}{2} \sigma^2(x,t) \frac{\partial^2 u}{\partial x^2}(x,t) -V(x,t) u(x,t) + f(x,t) = 0, 
$$

defined for all $$x \in \mathbb{R}$$ and $$t \in [0, T]$$, subject to the terminal condition

$$u(x,T)=\psi(x), $$

where μ, σ, ψ, ''V'', ''f'' are known functions, ''T'' is a parameter and $$ u:\mathbb{R}\times[0,T]\to\mathbb{R}$$ is the unknown. Then the Feynman–Kac formula tells us that the solution can be written as a [[conditional expectation]]

$$ u(x,t) = E^Q\left[ \int_t^T e^{-  \int_t^r V(X_\tau,\tau)\, d\tau}f(X_r,r)dr + e^{-\int_t^T V(X_\tau,\tau)\, d\tau}\psi(X_T) \,\Bigg|\, X_t=x \right] $$

under the [[probability measure]] Q such that ''X'' is an [[Itô calculus|Itô process]] driven by the equation

$$dX = \mu(X,t)\,dt + \sigma(X,t)\,dW^Q,$$

with $W^Q(t)$ is a Wiener process (also called Brownian motion) under ''Q'', and the initial condition for X(t) is X(t) = x .

if we apply this trick to the B-S PDE , we find the measure Q is just function like the implied probablity we get from the analysis we've done in binominal tree . till here , theroitically we can sove any pricing problem using the idea that *there should not be arbitradge oppotunity* 

## do some modification to apply the PDE in a more realistic area : Convertiable Bond in Chinese market

after going through this Nobel-level idea , you may wonder how such an obviously idea can win Nobel prize in 1997 , but the fact is that in this area , such an idea wasn't came up with until such a morden time . later i'll shoow some of my personal feeling about that idea . in the part 'summary'

but now let's go to a more technically area : how to use this idea to price chinese convertiable bonds.

### simplify assumption : coonstant parameters without soft-call or soft-put neither divident , ignoring the transaction cost

#### theory

the first modification is change the terminal condition .

during BS model , they simply considering a executive in the final time , hence the terminal condition is just 
$$u(x,T)=\psi(x), $$
but a convertiable bond can be execute at any time , which we call it American option . traditionally , teachers like to ptice an american option like this:

$$
A(x,t) = max\{u(x,a)|_{\partial_Tu(x,a) = 0},u(x,0),u(x,T)\}
$$

here $a$ is a optimal execute time , 0 denode execute immdietly and T denote execute at the meturity time . however it is not a proper strategy to define a execute time at the begining , because there could be a execute strategy : $f(x,t) = 0$ our , by which we can decide when and in which price we will execute . the valve between the execute noundry should be continue , 

$$
F|_f = \phi|_f
$$

we bring this boundary condition into the BS fomula , to get the expression of the boundary $f(x,t) = 0$

generally speaking , there should not be a analytical boundary , neither a analytical solution of the modified-B-S equation , however there will be a single solution to this equation (i don't have strict analytical provement this part , because i'm not a SDE exepert , however , i know how to solve this equation numercially , so i just say that there is a unique solution) , normally we apply a finite difference to numercially solve this equation . when problem get more complex , we can also use monte-calo to solve it . 

#### code


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



### realistic assumption : local parameters and transaction cost as a ressult of special tax machinism in chinese market .

#### theory
theoritically speaking , local r and local $\sigma$ don't need specific modification , but soft-call soft-put and dividend need , transaction fees(tax) , require a strong modifycation .

we do things step by step . 

first we input the market interest rate and market sigma , instead of the constent we used in last part , and recalculate the price , to judge how much difference are there between our reimplied price and the market price .

second , let's consider transaction cost : if there is transaction cost , we can't apply $\Delta$ hedging like before , because changing \Delta will cost additinaol money , so only if we can sell(/buy) the convertiable bond in a higher(/lower) price can we get arbitradge oppotunity .

#### code

```python
# replace R and Sigma with the market data
```

### somthing may be add later :

1. Con-bond can add 20% a day in china , but the common share 10% .
2. shot stock cost more money in china
3. soft-call soft-put is not easy to be inclouded in the model , because , it's a strong path deependent . we may try monte-carlo later 

## summary & persinal idea of LI YUSU













































