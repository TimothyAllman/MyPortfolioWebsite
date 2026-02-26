---
title: An Opinionated Guide On Consistent Python Package Structure 
date: 2023-05-11
authors:
  - name: Timothy Allman

---


Here are some opinionated recommendations on how to write consistent Python code

## Use uv (i.e. don't use pip)
uv is a python package manager written in rust. it is faster than pip

## Always use a pyproject.toml
it goes without saying that code must be reproducible. Code must work on my machine as well as yours. Thus dependencies for your code must be recorded somewhere, Do this in the pyproject.toml file. 

## Use ruff (formatting)
We don't want to have to manage The spacing and indentation of code ourselves. Instead use ruff to do this automatically on save. 
Find a ruff.toml file used in other projects and copy and paste it into yours. 

## Use (magic) trailing commas
When combined with the ruff formatter above we can use trailing commas to shorten long lines or control newlines.
This also helps with diffs, i.e. one line changes are truly one line changes

## Use (as much a possible) "one file one function" thinking 
Code can get complicated very quickly. Let's say we have a file that defines two functions. 

```.py
import math

def main():
    print("Hello from myportfoliowebsite!")

def mysin(x):
    math.sin(x)
```

Although in this trivial example it is easy to see. Generally speaking, if there is a lot of code in the file, we would not know Which function depended on which imports/ i.e it can sometimes become confusing or difficult to know which function is using the math import. This is especially true if there are many imports in a file like when a function depends on many external packages. 

Keeping one thing (one function or class) In one file gives us the certainty that the imports we see in the file are the only imports we need for this function. 

e.g. The below code would be vastly more complicated to understand If it weren't in its own file. 

```.py
import uuid

from stario import Context
from stario import Writer

from stariodemo.DataStructsPkg.GenerateColorModule import generate_color
from stariodemo.DataStructsPkg.GenerateUserNameModule import generate_username
from stariodemo.HtmlComponentsPkg.PageModule import page
from stariodemo.HtmlViewsPkg.ChatViewModule import chat_view
from stariodemo.HtmlViewsPkg.NavBarAndFooterViewModule import NavBarAndFooterView


def ChatPageEndpoint(
    noDeps: None,
):
    async def handler(c: Context, w: Writer) -> None:
        """
        Serve the home page
        """
        user_id = str(uuid.uuid4())[:8]
        username = generate_username()
        color = generate_color()

        # Pass empty collections - user will get real data after subscribing
        w.html(
            page(
                NavBarAndFooterView(
                    chat_view(
                        user_id,
                        username,
                        color,
                        messages=[],
                        users={},
                    )
                )
            )
        )

    return handler
```


## Use the suffix `Module` for .py files
A `.py` file (In technical literature) is known as a Python module. This also helps with autocomplete/intellisense. If I import a function 9e,g, `ChatPageEndpoint`) into a separate file to use. The IDE will know that I do not want to import the module. Because the function name does not have the suffix "Module" on it.  
What this also leads to is a very clean import syntax/conventions. 

```.py
from MyThingModule import MyThing
from ChatPageEndpointModule import ChatPageEndpoint
```

i.e. We import Xyz from an XyzModule.  




## Use the suffix `Pkg` for python packages/sub packages 
In pythonic code bases files Are organized into folders. These folders are known as packages and group related code together. For similar reasons to the above It is useful to add a `Pkg` suffix to these folder names. 

thus we would end up with something like this 
```.py
MathPkg/
├── __init__.py 
├── SinhModule.py
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
```
from stariodemo.DataStructsPkg import *
```
As it now becomes difficult to know What is being imported or from where a function originates? 

And we also don't want to do
```
from .DataStructsPkg.MessageModule import Message
```
Don't do this as this relies on relative referencing and could fail if the file structure is not the same. 

Instead, we import only the file you need. And exactly where you get it from
as is seen below
```
from stariodemo.DataStructsPkg.MessageModule import Message
from stariodemo.DataStructsPkg.UserModule import User
```

## Use `camelCase` and `PascalCase` over underscores and dashes
You may have noticed we have not been using underscores or dashes in our names In all the above examples. This is by design and mainly has to do with the top level package name But should be used in general. Underscores and dashes lead to confusing installation dynamics. 

More concretely A package named `my_cool_code` 
has to be installed via `install my-cool-code`/`uv add my-cool-code`
But then it's imported via `import my_cool_code`

This is sub optimal. We can avoid all this just by using camel case or pascal case for naming. 


## Use a main.py
Now that we have a package defined in source, it is very easy for us to import this into an external file where we will do the work/calculation/thing we want it to. this file will exist outside of the src folder and for convention we will call it main. i.e. it is the "main" / first place to look to see how our code is used





