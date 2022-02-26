---
layout: post
read_time: true
show_date: true
title:  "some basic idea in quant finance world learning from Convertiable Bond in Chinese market ."
date:   2022-02-23 
img: posts/tobeupload.png
tags: [QF],[basic idea],[homework],[qf5202],[ma4269]
author: LI YUSU
github:  liyusu-tsinghua/github.io
mathjax: yes
description: i'll show sth basic in quant's world --non-arbitradge-theory , through Convertiable Bond in Chinese market . this is also one of my homework , i hope this can help you understand the basic math structure in a quant's world . during the ilustration , i assume you hve never met this area , but you should have a basic understanding about PDE . if you just have an interest in this area and a little knowledge about math , but don't know where to start , follow me ,let's go !
---

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

the first modification is change the terminal condition .

during BS model , they simply considering a executive in the final time , hence the terminal condition is just 
$$u(x,T)=\psi(x), $$
but a convertiable bond can be execute at any time , which we call it American option . 



### realistic assumption : local parameters and transaction cost as a ressult of special tax machinism in chinese market .

## summary & persinal idea of LI YUSU













































