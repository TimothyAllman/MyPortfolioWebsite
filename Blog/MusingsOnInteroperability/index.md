---
title: Musing On Interoperability In Python And C#
date: 2026-03-26
authors:
  - name: Timothy Allman

---

Sometimes you want to use C# code in python
Here are some thoughts on what that entails


## You Probably Shouldn't Do it
Although there are some valid reasons as to why you might *want* to use C# code in python or vice versa, in my opinion the maintanance 
and this often 
you probably haven't assessed your requirements thouroughly enough

the maintenance burden and time spent "making the two languages work together" will always be a imperfect experience and thus the solution will always be less optimal than if you just assess your requirements/ what your project *has to* be able to do and picking a language that meets or exceeds these requirement, i.e. the correct language for your problem

## You will probably let someone else do the heavy lifting
Making C# and python interact is no small undertaking. 
at most you probably just want to access something in C# via python.
i.e. you want to be able to do 
```{code-block} python
:filename: MyPythonScript.py
:linenos:
from MyCSharpCode.MyFunctionsNameSpace.MyFuncFile import MyFunc

athing = MyFunc(argument1 = 1, argument2 =2.0, argument3 = "three", argument4=True)
```
pythonet and IronPython are two projects that come up in this regard.
pythonet is quite good if you just want to use standard CPython and import a library. 
IronPython better if you want to you an alternative implementation of python (i.e. not the most common) that is more closely tied to C#.


## You cannot truly be in both worlds
As much as you want the two languages to exist in harmony, as mentioned earlier, there will be shortcomings.
In python

```{literalinclude} ../ruff.toml
:lineno-match:
``` 


## You can try wrap it



## But you should probably just expose as much as possible

##