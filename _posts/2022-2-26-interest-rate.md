---
layout: post
read_time: true
show_date: true
title:  "some basic idea in quant finance world learning from swaps ."
date:   2022-02-23 
img: posts/tobeupload.png
tags: [QF],[basic idea],[homework],[qf5202],[ma4269]
author: LI YUSU
github:  liyusu-tsinghua/github.io
mathjax: yes
description: i'll show sth basic in quant's world --interest rate ,through a kind of instrument called swap . this is also one of my homework , i hope this can help you understand the basic math structure in a quant's world . during the ilustration , i assume you hve never met this area , but you should have a basic understanding about PDE . if you just have an interest in this area and a little knowledge about math , but don't know where to start , follow me ,let's go !
---

[toc]

## background

interest rate is probably the most important part in finance , maybe even more important than none-arbitradge . because interest rate means the money in different time means differently .

if you still ask why to every answer , after 5 or 6 times , you will come to an area , where there is a same question , and that question is probably the main idea of that subject . 

in physics , you will meet dirac equation , they will tell you every thing is about to find a sammitry . in finance , it will be how to balance different resource from different time . the relation ship between different time is interest rate . 

i will add some more personal understanding at the end of this essay , now let's start from a more specific spot : bonds.

## theory 

### calculate from bonds
theoritically , the goveronment or central bank will offer long-term bonds or , which in fact is a serious of cashalow , for example :

pay you \$10 every year , at the tenth year , pay you $110 .

this kind of bond may have a price today $p$, if we mark the \$ in time t as $Z_t$ , the cash flow at time t as $c_t$ simply we have :

$$p*Z_0 = \sum_{t_i}c_{ti}Z_{t_i}$$

mormally , we have a series of bond , that means if we use index j to denote No.j bond , we have :

$$p_jZ_0 = \sum_{t_i} c_{t_i,j}Z_{t_i}$$

which is a linear equation , if the number of j is less than the number of $t_i$ , there will be infinit solution , if the number of j is larger than the ti , that would mean the bonds are linear deependent , or there would be arbitradge oppotunity . by defination , \$ at same time chan exchange with each other . 

than there comes a question : what if i want to exchange today's money with some time isn't exist in the time series ? 

we will do some interpolation between the $\{Z_{t_i}\}$ , for some different reason , people will choose different interpolation method , but i think the interpolation details is decided by some statastic theory , and it also depend's on what is the interest rate you are calculating . 

if you are calculating the cost of borrowing money for your self , you should considering calculate a higher one , if you are calculating your profit for stoving , you should considering a lower one .

### calculate from the libor

in fact , except the bonds market , there is another market called money market , where between bank they calculate lobar everyday , that is a flout market . 

## technical details

the theory is really easy but the details is not , so i must give some realistic example to illustrate what a bond is :

in fact , a bond is really easy , you can simply say how much there will be in which day , but in fact people follow a series of rules to define a bond , because of my ugly homeworrk , i'll show these details with the package : quantlib for python , this is a package written by c++ , but can be import by python , honestly speaking , idon't think this package outstanding at all . but i have to use it , because of my th=eacher .  



