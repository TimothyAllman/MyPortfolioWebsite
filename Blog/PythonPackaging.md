---
title: An Opinionated Guide On Consistent Python Package Structure 
date: 2025-11-11
authors:
  - name: Timothy Allman

---


Here are some opinionated recommendations on how to write consistent Python code

## Use uv (i.e. don't use pip)
uv is a python package manager written in rust. it is faster than pip

## Always use a pyproject.toml
it goes without saying that code must be reproducible. Code must work on my machine as well as yours. Thus dependencies for your code must be recorded somewhere, Do this in the pyproject.toml file. 

## Use ruff (formatting)
We don't want to have to manage The spacing and indentation of code ourselves. Instead use ruff to do this automatically on save. you can place a `ruff.toml` file at the root of your project to configure it with your preferences. 
Or just find a ruff.toml file used in other projects and copy and paste it into yours.
Here is the one this codebase uses.
```{literalinclude} ../ruff.toml
:lineno-match:
``` 

:::{aside}
I set the `line-length = 320` value of my ruff.toml to the maximum which is 320 because I prefer controlling newlines with magic trailing commas as is described next
:::

Storing this file in the git repo will mean that anyone who clones/works on the project will abide by the same formatting and changes to formatting will be versioned as well and therefore a consistent style will be maintained and applied throughout.



## Use (magic) trailing commas (like almost always)
When combined with the ruff formatter above we can use trailing commas to spill/newline long lines of code into multiline blocks/statements.

lets say I have this code where I am calling a function that has lots of arguments
```{code-block} python
:filename: FileThatShouldBeMultiline.py
:linenos:

athing = MyFunc(argument1 = 1, argument2 =2.0, argument3 = "three", argument4=True)

```

If I add a trailing comma at just before the last `)` 
```{code-block} python
:filename: FileThatShouldBeMultiline.py
:linenos:

athing = MyFunc(argument1 = 1, argument2 =2.0, argument3 = "three", argument4=True,)

```
and then format with ruff, I will get
```{code-block} python
:filename: FileThatShouldBeMultiline.py
:linenos:

athing = MyFunc(
    argument1 = 1, 
    argument2 =2.0, 
    argument3 = "three", 
    argument4=True,
)
```

the same goes with lists something like this

```{code-block} python
:filename: ExampleWithList.py
:linenos:

alist = [dt.datetime(2025, 11, 23), dt.datetime(2025, 11, 23), dt.datetime(2025, 11, 23), dt.datetime(2025, 11, 23), dt.datetime(2025, 11, 23)]

```

will be formatted into
```{code-block} python
:filename: ExampleWithList.py
:linenos:

alist = [
    dt.datetime(2025, 11, 23),
    dt.datetime(2025, 11, 23),
    dt.datetime(2025, 11, 23),
    dt.datetime(2025, 11, 23),
    dt.datetime(2025, 11, 23),
]
```

This also helps with diffs - for example if we had to add a new item 

with out the trailing comma we the diff we get this
```{code-block} python
:filename: Diff.py
:linenos:
:emphasize-lines: 6

alist = [
    dt.datetime(2025, 11, 23),
    dt.datetime(2025, 11, 24),
    dt.datetime(2025, 11, 25),
    dt.datetime(2025, 11, 26),
    dt.datetime(2025, 11, 27)
]
```

```{code-block} python
:filename: Diff.py
:linenos:
:emphasize-lines: 6,7

alist = [
    dt.datetime(2025, 11, 23),
    dt.datetime(2025, 11, 24),
    dt.datetime(2025, 11, 25),
    dt.datetime(2025, 11, 26),
    dt.datetime(2025, 11, 27),
    dt.datetime(2025, 11, 28)
]
```
the diff tool picks up both the new line and the addition fo the comma on the previous line

however with trailing commas
```{code-block} python
:filename: Diff.py
:linenos: 

alist = [
    dt.datetime(2025, 11, 23),
    dt.datetime(2025, 11, 24),
    dt.datetime(2025, 11, 25),
    dt.datetime(2025, 11, 26),
    dt.datetime(2025, 11, 27),
]
```

becomes
```{code-block} python
:filename: Diff.py
:linenos:
:emphasize-lines: 7

alist = [
    dt.datetime(2025, 11, 23),
    dt.datetime(2025, 11, 24),
    dt.datetime(2025, 11, 25),
    dt.datetime(2025, 11, 26),
    dt.datetime(2025, 11, 27),
    dt.datetime(2025, 11, 28),
]
```
Thus we don't have to change anything else i.e. one line changes are truly one line changes.

The main takeaway here is that you should use magic trailing commas to go from long lines to nice multi-line blocks of code and that, you should thus use them liberally/almost alwasy as they lead to essentially  "paragraphs" of code which each chunk following on from that which came before and aids the grouping of logical units.

## Use (as much a possible) "one file one function" thinking 
Code can get complicated very quickly. Let's say we have a file that defines two functions. 

```{code-block} python
:filename: OneBigFileOfEverything.py
:linenos:

import math

def MathSaysHi():
    print("Hello from math!")

def MySin(x):
    math.sin(x)
```

Although in this trivial example it is easy to see. Generally speaking, if there is a lot of code in the file, we would not know Which function depended on which imports/ i.e it can sometimes become confusing or difficult to know which function is using the math import. This is especially true if there are many imports in a file like when a function depends on many external packages. 

instead we should create two seperate files one tha looks like
```{code-block} python
:filename: MySinModule.py
:linenos:

import math

def MySin(x):
    math.sin(x)
```

and another file that looks like
```{code-block} python
:filename: MathSaysHiModule.py
:linenos:

def MathSaysHi():
    print("Hello from math!")
```


Keeping one thing (one function or class) In one file gives us the certainty that the imports we see at the top of the file are the imports we need for the function to work.
Said in another way, I can be certain that in the above example the `MathSaysHi()` function does not need any external imports to work - how do I know this - because there are no imports at the top of its file, how easy was that.




## Use the suffix `Module` for .py files
A `.py` file (In technical literature) is known as a Python module. This also helps with autocomplete/intellisense. And so I think that we should name our file, exactly like that - you may have noticed that in the examples above I have already started naming files in a certain way and adding a `Module` suffix to them. One reason for this is that naming things like this will help the IDE with autocomplete/intelisense/hints. More specifically, if I had a file like this 

```{code-block} python
:filename: ACoolFunc.py
:linenos:

def ACoolFunc():
    return "really cool"
``` 

and I wanted to use this file somewhere else
```{code-block} python
:filename: SomeOtherFile.py
message = ACoolFunc()
```
I have found that VSCode will suggest something like this 
```{code-block} python
:filename: SomeOtherFile.py
:linenos: 
:emphasize-lines: 1
from MyCoolCodePkg import ACoolFunc

message = ACoolFunc()
```
where we will import the file and the last line will throw and error because modules are not callable

whereas if I had a file like this 
```{code-block} python
:filename: ACoolFuncModule.py
:linenos: 

def ACoolFunc():
    return "really cool"
``` 
then VSCode suggestions give me what I want

```{code-block} python
:filename: SomeOtherFile.py
:emphasize-lines: 1
from MyCoolCodePkg.ACoolFuncModule import ACoolFunc

message = ACoolFunc()
```
where I am importing the function and no error will occur

In short i will spend less time fighting with my IDE and more time being in harmony, the IDE will know that I do not want to import the module because the function name does not have the suffix "Module" on it and so there is no ambiguity.  

furthermore A more important reason is that naming like this helps us humans distinguish between what is the function and what is the file (as the file wil be named e.g. `XyzModule.py`) 

What this also leads to is a very clear import syntax/conventions. 
```{code-block} python
:linenos:
from MyThingModule import MyThing
from ChatPageEndpointModule import ChatPageEndpoint
```

i.e. We import Xyz from an XyzModule. And everything is imported like this/abides by this convention. That sort of consistency and simplicity is very important as systems get complicated.




## Use the suffix `Pkg` for python packages/sub packages 
In pythonic code bases files Are organized into folders. These folders are known as packages and group related code together. For similar reasons to the above It is useful to add a `Pkg` suffix to these folder names. 

thus we would end up with something like this 
```
MathPkg/
├── __init__.py 
├── SinhModule.py
├── MySinModule.py
├── CoshModule.py
└── ExponentialModule.py
```

Naming the folders like this also makes it very clear that if we really needed to, we could pull this folder out of a larger project and create a separate package That could be imported on its own into very many projects. Ensuring that the idea of modularity is always front of mind.  

## Use the `src` layout
The src layout just means that you have a top level folder i.e `MyCoolCode` and inside of it we have a `src` folder that houses our main package folder and its sub folders.
The main package folder inside `src` is a packaged just like any other and so should be named with a `pkg` suffix as described above. 

```
MyCoolCode/
├── src/
│   └── mycoolcodepkg/
│       ├── __init__.py
│       ├── MathPkg/
│       │   ├── __init__.py
│       │   ├── SinhModule.py
│       │   ├── CoshModule.py
│       │   └── ExponentialModule.py
│       └── UsersPkg_b/
│           ├── __init__.py
│           ├── UserDataModule.py
│           ├── CreateUserModule.py
│           ├── DeleteUserModule.py
│           └── UserPermissionsModule.py
├── pyproject.toml
├── README.md
└── LICENSE
```

## Use fully qualified/absolute imports. 
Now that you have code written and organized into our src folder its time to talk about how we import it.
Most importantly we will avoid using dot/relative imports and we will also avoid using star imports. 

to emphasize 

we don't want to do

```{code-block} python
:linenos:
from stariodemo.DataStructsPkg import *
```
As it now becomes difficult to know What is being imported or from where a function originates? 

And we also don't want to do

```{code-block} python
:linenos:
from .DataStructsPkg.MessageModule import Message
```
Don't do this as this relies on relative referencing and could fail if the file structure is not the same. 

Instead, we import only the file you need. And exactly where you get it from
as is seen below

```{code-block} python
:linenos:
from stariodemo.DataStructsPkg.MessageModule import Message
from stariodemo.DataStructsPkg.UserModule import User
```

## Use `camelCase` and `PascalCase` over underscores and dashes
You may have noticed we have not been using underscores or dashes in our names In all the above examples. This is by design and mainly has to do with the top level package name But should be used in general. Underscores and dashes lead to confusing installation dynamics. 

More concretely A package named `my_cool_code` 
has to be installed via dashes

```{code} bash
pip install my-cool-code
```
or 
```{code} bash
uv add my-cool-code
```


But then unintuitively it will be imported via underscores 
```{code-block} python
:filename: myfile.py
:linenos:

import my_cool_code
```

This is sub optimal. We can avoid all this just by using camel case or pascal case for naming and not touching underscores or dashes (especially in when it comes to package names).


## Use a main.py
Now that we have a package defined in source, it is very easy for us to import this into an external file where we will do the work/calculation/thing we want it to. this file will exist outside of the src folder and for convention we will call it main. i.e. it is the "main" / first place to look to see how our code is used. So following on from our example our `main.py` file may look something like this

```{code-block} python
:filename: main.py
:linenos:

from mycoolcodepkg.MathPkg.MySinModule import MySin

result = MySin(x=0.3)

print(result)
```

or if you want to use alias it as a shorthand you can do
```{code-block} python
:filename: main.py
:linenos:

import mycoolcodepkg as mcc

result = mcc.MySin(x=0.3)

print(result)
```



