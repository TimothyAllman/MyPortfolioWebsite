---
title: Why you really should componentize your frontend
date: 2025-12-18
authors:
  - name: Timothy Allman

---


## Some Background
At work today I found myself in an older legacy web application.
I had done most of the backend work and was testing the frontend to see if I had forgot anything or anything was breaking. 
True enough I a button wasn't showing up when it was supposed to and so I got out my wrench and dove into the html to fix.
But what I was met with was a mess of divs and attributes (some of which had been written by me earlier on in my lifespan) all held together .
on top of this the file was many lines long - too many lines. And so  

## The Problem
when one uses this "no-component" approach what I have found happens is that the fronent files get very, well... large.

```html
    <div>
    </div>
```

there is a lot going on here, layout stuff is doing spacing and structure and content things are 

## The Solution
Yes IDE support like colorisation and formatting can help this, but I still don't think that that is enough. the right solutions to any problem are elegant.


```html
    <div>
    </div>
```

just like a function (I am quite radical about this ) a file should do one thing and one thing only. this speaks to the locality of control argument when i look at `MyComponentFile.html`
I should only be looking at what is relavent to hit. I don't want to have to be interested in the fact that in the context of the main page I will need 5px padding at the top

similarly in my 

I just want to see this
```html
<html>
    <main>
        


        <MyComponentFile/>

    </main>
</html>
```
i.e. I just want to see htat I have a side bar and next to it is the content of my component i.e. I only want to care about layout or the relavent spacing 

## The Benefits
At first this doesn't seem to do much, we have merely taken one file that was fifty lines long and transformed it into two files that are 50
Not only does this

In any event even if you don't go the whole one file one thing mantra having less to things/html to look at is less to keep in your head and ultimately less places for a mistake to crop up
