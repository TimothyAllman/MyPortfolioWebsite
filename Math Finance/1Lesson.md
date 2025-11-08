---
title: Some Notes On Mathematical Finance 
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


## Some Thanks
I made these notes while working under a 
alot of this is taken by Melusi Mavuso, he is one of the best teachers I have ever had the previllege of learning from
if anything is correct it is only due to his intelligence, patience, wonderful teaching style and his wealth of knowledge with respect to this subject. 
and if anything is wrong it is probably due to my own incorrect understanding

so I just would like to say thanks to him and all of those I have worked with and learnt from 

## Intro
hopefully after reading this you will be able to value "fincial produucts" so for an insurance comapny this may be things like a IGR / and for banking these may be derivatives or other such complex products


## The big Idea
How do you price a product that does not exist in the market
How do I hedge such a an instrument?

e.g cos(tesla sahre)
how do I find the price of this

## A motivating example
consider a world with oonly two time points

```{math}
t=0 \hdots t=T

```
where t=0 is today and some random point in the future t=T

we will also aumme that ther are only  two possible "states" at time t=T,
a "good" state and a "bad" state

```{math}
\mathcal{P}(g)=0.7 
\mathcal{P}(b)=0.3
```

we assume a "primary/trivial assets" of a share and a bank account
```{math}
S\arrow \text{share}
B\arrow \text{bank account}
```

initiall the share is 100 bucks
```{math}
S_{0} = 100
```

and the future value of the share could be one of 2 possibilities
```{math}
S_{1} = 140 if g
=90 if b
```
and the bank account earns 6%
mathmatically we can denote this by 
```{math}
B_{0} = 1
```
and 

```{math}
B_{T} = 1.06
```

all together we can represent this in a tree diagram like so
```{mermaid}
graph TD
    A[Root Node] --> B[Child 1]
    A --> C[Child 2]
    B --> D[Sub-Child 1]
    C --> E[Sub-Child 2]
    C --> F[Sub-Child 3]
````

from this we can now answer some questions

what is the expected return of the bank account?
this is obviously 6%

a harder one woul be what is the expected return of the share
```{math}
expected value of share  &=  40\mul 0.7 \plus (-10) \mul 0.3 \\
                          &=  25

```
therfore 
```{math}
expected return of share  &=  25/100 \\
                          &=  25%

```

now let us  consider a nother non primary asset, a derivative, i.e. a complicate instrument bbased on primary assets
in this scenario we will introduce a call option with a strike price of {math}K=110

:::{aside} A Call Option
is a fincial contract where the holder has the right but not the obligation to buy a thing at a future point in time for a prespecified strike price $K$
:::

So if the future the price of the share is 2000 then we would obviously excercise our right to but it at 110. 
Conversely if the price of the share in the future is 20 then we wwould not excercise the right to buy it at 110 because we can just go to the open market and buy it for 20

mathematically we can express this terminal payoof condition as 
```{math}
C_{T} &= \max{S_{T}-110,0}
      &= S_{T} -K if S_{T}\geq K or 0 otherwise 
```

so in the simplistic two state example we have described above

```{math}
C_{T} = 30 if good
```
or
```{math}
C_{T} = 0 if bad
```

our goal in life is to answer the question
1. what is {math} C_0?
2. How do you hedge {math}C?

Let us try to find C_0 using expected present value 
```{math}
:label: epv
C_{0} &= 30 \times 1.06^{-1} \times 0.7 + 0 \times 1.066{-1} \times 0.3
      &= 19.81
```

this seems reasonable 
however using expected present value on the sahre i.e. discounting back S_{T} back to find S_0 does not give you S_0 = 100
hence if EPV does not hold for the share we cannot expect it to hold for the Call

even worse, the probabilities we used were completely made up out of thin air. these are the real world possibilities and hence any person you ask what the proability of the good state would be would give you different answers (i.e. some might say 0.7 like us but others would say 0.65 or 0.95)

so it is clear that we need a different approach
and we are lucky becasue blacka and merton hyponthesied what we should do in this case
we will create a replicating portfolio. the only thing that this replicating portfolio must do is make sure that it that no matter what the stae is the value of the replicating portfolio must equal the value of the option
so you can think of the replicating portfolio  as a basket of primary assets which has the value of the option  in all states of world 
```{math}
\psi \arrow number of shares
\beta  \arrow number of bank
```

so in order to determine these values and in order to abbide by the above condition we form the below equations

in the good state
```{math}
140\phi + 1.06\beta =30
```
and in for the bad state we construct
```{math}
90\phi + 1.06\beta = 0
```
now that we have two equations with two unknowns
we can solve for this (the determenant is positive etc)


solve
solve
solve

thus we end up with {math} \phi = 0.6 and \beta=-50.94

(will suspend disbelief about buying 0.6 shares/fractional shares)

```{math}
V_0 &=S_0\times\phi + B_0\times\beta = 
    &=100\times\phi + 1\times\beta = 
    &=100\times 0.6 + 1\times\-50.94 = 9,06
```

so we have a portfolio {math} V

and we know now that 
```{math}
V_0  = 9,06
```

```{math}
V_T  = 30
```
in the good state


```{math}
V_T  = 0
```
in the bad state


thus by design the replicating portfolio V has the same value as the call option/derivative at expiry
hence C_0 should be equal to V_0 or else there would be arbitrage, etc

so we have answred question one 
we now know that C_0 should be 9.06
and even more cool we have actaully also answered question 2
because in order to hedge C we can jut trade in the V i.e. the underlying assets

but what about those probablities, they didnt even feature even if we changes the probablit of g changed to 0.99
it would not make a difference and the price would stil be 9.06
thus this pricing process does not care about the real world probabilities


but we can still use probabilities they just have to be somewhat "special"

imagine we have
```{math}
\mathcal{Q}(g) = q
```
and
```{math}
\mathcal{Q}(b) = 1-q
```
and now we just have to "choose/find" q so that the replicating porfolio price V_0 is still consistent and V_0 = 9.06

thus we construct
```{math}
\mathcal{E}^{\mathcal{Q}}[1.06^{-1} \times S_{T}] 
```
which we call the expectation **under the probability measure {math}\mathcal{Q}**

if we do construct this such that it equals the sahre price at time t=0
```{math}
\mathcal{E}^{\mathcal{Q}}[1.06^{-1} \times S_{T}] = S0
```
then we can use this in the way we tried to in {eq}`epv` and udo expected present value

so lets try that
```{math}
 S_0  &= \mathcal{E}^{\mathcal{Q}}[1.06^{-1} \times S_{T}]
      &= (140\times 1.06^-1)\times q \plus (90\times1.06)^{-1})\times (1-q)
      &= (140\times 1.06^-1)\times q \plus (90\times1.06)^{-1})\times (1-q)
```

thus for the probability of being in a good state, we get
```{math}
q = 0.32 
```

and for the probability of being in a bad state we get
```{math}
1-q =0.68
```

finally we can apply this to the call option /derivati
```{math}
\mathcal{E}^{\mathcal{Q}}[1.06^{-1} \times C_{T}] &= solve
                                                  &= solve
                                                  &= 9.06
                                                  &= C_0
```
and we see tht indeed 

So in summary the main points are
- real world probabilites dont work on shares or options based on them and 


this new probability measure {math}\mathcal{Q} has sme interesting properties

```{math}
\mathcal{E}^{mathcal{Q}}(\frac{C_T]{C_0}) = \mathcal{E}^{mathcal{Q}}(\frac{S_T]{S_0}) = \mathcal{E}^{mathcal{Q}}(\frac{B_T]{B_0}) = 1.06
```
thus all things/ assets grow at the risk free rate and hence this probability measure Q is know as the Risk-Neutral Measure

it is also known as the the pricing measure or equivalent martinggale measure 



:::{attention}
the probabilitys are fake and made up they are not real world proabbilitys and exist as a mathematical construct to allow us to do expected present value  
:::

but the P and Q are equivalent in that they:
- assign non zero probabilities to the same sets even though those probabilitys may be different
- assign zero probability to the same sets

Also any asset or "thing" that abides byt the discounted expectation under Q
like 

the share 
```{math}
\mathcal{E}^{\mathcal{Q}}[1.06^{-1} \times S_{T}] &= S_0
                      
```

the call option
```{math}
\mathcal{E}^{\mathcal{Q}}[1.06^{-1} \times C_{T}] &= C_0
```

any other thing
```{math}
\mathcal{E}^{\mathcal{Q}}[1.06^{-1} \times G_{T}] &= G_0
```
is called a martingale

and similarly if we have a thing J and we know J_T and we are told/ know its a martingale. we can do

```{math}
\mathcal{E}^{\mathcal{Q}}[1.06^{-1} \times J_{T}] &= J_0
```
to find J_0

hence the equivalent martinggale measure naming
hence you could say measure Q turns discounted asset prices into martingales

does this measure always exist
the answer is no sometiems it doesnt

but then what do you do
well you can't do anything

the fundamental theorem of asset prcinging part 1 states
and EMM means there is NO ARBITRGE (Harriosn And Krepps)

so when there is no EMM there is arbitrage
but when there is more than one EMM you can easily show that there is infinitely  many EMMS
and this is termed an incomplete market and it means not all derivates can be replicated so some derivates can but not all 

the fundamental theorem of asset prcinging part 2 states
if NA is true then a market is complete complete and every derivative can be hedge if and only iff there is 1 EMM

## Black Scholes

squigggely line

continuous stochastic process
bachalier phd







```{math}
:label: mymath
(a + b)^2 = a^2 + 2ab + b^2

(a + b)^2  &=  (a + b)(a + b) \\
           &=  a^2 + 2ab + b^2

\sum{}

\sigma

\Sigma
```

## 



## 





