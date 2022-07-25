---
layout: post
title:  "Separate view and logic in a Blazor component"
date:   2022-07-25
---

## Two ways of creating Blazor components

In most of Blazor tutorials I've seen, Blazor components have been shown like this:

``` 
@page "/counter"

<PageTitle>Counter</PageTitle>

<h1>Counter</h1>

<p role="status">Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```
The code snippet comes from the default Blazor WebAssembly project template. If you look at it, you can see it is divided into two parts- one is the "html" part and the second one, the "code" part. It makes perfect sense in small components, you have your reference code and variables. 

However, let's imagine you have a large component, such as a list, or a form with many bindings, variables and services used - it can get quite messy the code in one file gets long.
In this case, you can choose to separate the html part and code part into two files - using partial classes.

## Separate code and HTML in Blazor components

- In a new Blazor WASM project in Visual Studio, create a new "Components" folder.
- Create a new Razor component and name it how you want - I used "CounterSeparate.razor". In the same folder create a new class and name it the same, but with a .cs extension - in my case, "CounterSeparate.cs".

![screenshot](https://user-images.githubusercontent.com/89458930/180850822-e29f21ff-914d-4762-8020-f9f02ca5dab5.png)

- Copy html contents from Counter.razor (in Pages folder) into your new CounterSeparate.razor file:

```
<PageTitle>Counter</PageTitle>

<h1>Counter</h1>

<p role="status">Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```
- Now, open your CounterSeparate.cs file
- First things first, change the class to partial and make it inherit after ComponentBase:
```
using Microsoft.AspNetCore.Components;

namespace BlazorComponents.Pages
{
    public partial class CounterSeparate : ComponentBase
    {
    }
}
```
Copy the code block directly into the file:

```
using Microsoft.AspNetCore.Components;

namespace BlazorComponents.Pages
{
    public partial class CounterSeparate : ComponentBase
    {
        private int currentCount = 0;

        private void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

Now, let's just insert the component on the main page - in Index.razor:

```
@page "/"
@using BlazorComponents.Components

<PageTitle>Index</PageTitle>

<h1>Hello, world!</h1>

Welcome to your new app.

<CounterSeparate />

<SurveyPrompt Title="How is Blazor working for you?" />
```

Finally, build and run the app - you will see the counter works as well as before:

![screenshot](https://user-images.githubusercontent.com/89458930/180850365-843422a1-5cb4-4b6a-b2c3-35d19967aafc.png)

## Summary
Personally, I always try to separate components if they are not really small. For me, this seems neater and has this regular-C#-class look that I like. It all boils down to personal preference and I can see it may be easier to navigate code or debug with all of the code in the same file. 

