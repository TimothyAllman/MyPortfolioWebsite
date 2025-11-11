---
title: Yield Curves
date: 2023-05-11
authors:
  - name: Timothy Allman
exports:
  - format: pdf
    template: lapreprint-typst
    # output: exports/my-document.pdf
#     id: my-document-export
# downloads:
#   - id: my-document-export
#     title: A PDF of this document
---

## Interesst Rate Derivatives

yield curves are fundamental to most pricing in fincial markets 
as they provide a mechanism for us to take a future cashflow and present value it to our current time t_0

## Recall
primary assets -> we know their prices they are freely available, e.g. bond
derivative assets -> we  do not know there price e.g. insurance option contract
goal is to find them -> how we replicate and we are able to price and hedge at the same time
probability measure based on prices of all assets -> fake 
i.e. we can price via replication or via expectation under the risk neutral measure (but they are basically the same)

## Linear Interest Rate Derivatives


:::{attention}
linear derivatives, like  the linear interest rate derivatives we speak about here, have the special property that they are model independet. i.e. the prices we arrive 

other derivatives (non linear derivates), like calls and puts are model dependent i.e. the price we obtain through them will change based on the underlying model we use to evaluate them. i.e. we could use blakc sholes to obtain the price of call option on a share and the price would be $x$ e.g.  and we could at the same time use heston model to obtain the price of call option on a share and the price would be $y$. i.e. the prices would be different  
:::

hence because of this we do not need models to obtain a price for linear derivatives
linear as a function of primary assets (mainly zcb)


we will make some very idealised assumptions
- we have a infinite collection of ZCB (zero coupon bonds) i.e we can find a 

a zcb you buy for less than 1 rand and it pays you one rand sometime in the future

we shall denote this with $P(t,T)$ 
for any maturity we pick we can find a ZCB i.e. if I have time 5yr I am able to find P(0,5)
from this we can see that ZCB give us a way to discount 1 rand to today and thus could also me used as disocunt factors

from this we can also define its counsin a capitalisation function
$C(t,T)$
which closely links to how interest is accumulated in a market and can come in different flavours

but for this we need to define interest

## Spot rates
for Simple interest we get
$$
C(t,T) = (1+ L(t,T)\times\tau(t,T))
$$

where $\tau(t,T)$ is used to account for day count conventions (e.g. actual/365) if we are being pedantic and is roughly equivalnt to $(T-t) 

for example if we had a picture like this 
diagram

we would arrive at this
$$
C(t,T) = (1+ 5\times \frac{45}{365})
$$

comopound interest

nominal annula compounded P / NACP
$$
C(t,T)=(1+\frac{R_p(t,T)}{p})^{\tau(T,t)\times p} \sim  C(t,T)=(1+\frac{R_p(t,T)}{p})^{(T-t)\times p}
$$
where $R_P$ means compounded p times a year




nominal annula compounded annually / NACA
$$
C(t,T)=(1+R_a(t,T))^{\tau(T,t)} \sim  C(t,T)=(1+R(t,T))^{(T-t)}
$$

nominal annula compounded semi annually / NACS
$$
C
$$


nominal annula compounded quarterly / NACQ
$$
C
$$


nominal annula compounded monthly / NACM
$$
C
$$


if
p --> $\infty$ 

we know

as 
$$
x_p \rightarrow x
$$

$$
\lim_{p\rightarrow\infty} (1+\frac{x_p}{p})^{p} = e^{x}
$$

i.e.
$$
x_p \rightarrow x_{\infty}
$$

$$
\lim_{p\rightarrow\infty} (1+\frac{x_p}{p})^{p} = e^{x_{\infty}}
$$

is awell known result

so taking x_p = r_p
this is NACC

$$
\lim_{p\rightarrow\infty} (1+\frac{R_p(t,T)}{p})^{p} = e^{R_{\infty}(t,T)} = u
% &= [ \lim_{p\rightarrow\infty} (1+\frac{x_p}{p})^{p} ]^{(T-t)} 
% &= \lim_{p\rightarrow\infty} ((1+\frac{x_p}{p})^{p})^{(T-t)} 
$$

thus
$$
u^{(T-t)} = (e^{R_{\infty}(t,T)})^{(T-t)} = e^{R_{\infty}(t,T)\times (T-t)}
$$

these to relations when multiplied together give you 1
$$
C(t,T) \times P(t,T) = 1
$$

thats it for spot rates


## Forward rates

t-----T-------A
$$
C(t,A) = C(t,T) \times C(T,A)
$$

so for 
NACC this looks like
$$
e^{R(t,A)(A-t)}=e^{R(t,T)(T-t)}\times e^{F(t,T,A)(A-t)}
$$
taking logs to solve for $F$
$$
\begin{align}
R(t,A)(A-t) &=R(t,T)(T-t) + F(t,T,A)(A-t)
F(t,T,A)  &= \frac{R(t,A)(A-t) - R(t,T)(T-t)}{A-T}
\end{align}
$$

which is a weighted difference between the spot rates of some time


if you take
$$
\lim_{A\rightarrow T} F(t,T,A) = \frac{d}{d_{T_1}} R(t,T_1)(T_1-t) |_{T_1=T} = \frac{d}{d_{T_1}} \log(C(t,T_1))
$$

$$
\lim_{A\rightarrow T} F(t,T,A) = \frac{d}{d_{T_1}} R(t,M)(M-t) |_{M=T} = \frac{d}{d_{T_1}} \log(C(t,T_1))
$$
lim of forward rate is rate of change of log capitalisation function


i.e. the derivative

$$
f'(a) = \lim_{x\rightarrow a} \frac{f(x)-f(a)}{x-a}
$$

the forward rates are a discretizaation of the rate of change log capitalisation

the instantaeous forward rate is limit quantity
$$
\lim_{A\rightarrow T} F(t,T,A)
$$
often shown with a littel f as 
$$
f(t,T) -> \text{the forward rate over the next instant (1 day forward rate roughly an overnight rate)}
$$


in general 

t-------------------T1----------------------T2-------------.....---------Tn

the spot rate can be written as a weighted average of all the forward rates
i.e.
$$
R(t,T_n) = \frac{1}{T_n-t} \Sigma_{k=1}^n F(t,T_{k-1}, T_{k}) \times(T_k-T_{k-1})
$$
where we agree that $T_0 =t$


in the limit we get

$$
R(t,T) = \frac{1}{T-t} \int_{k}^T f(t,s) ds = \frac{1}{T-t} \int_{k}^T f(t,x) dx
$$
thus  the spot rate can also be recovered by integrating the instantaneous forward rate

##Govi bonds


