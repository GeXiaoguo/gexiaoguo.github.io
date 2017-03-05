---
layout: post
title: ViewModel Should Be Coupled with The View
---
Josh Smith had <a href="https://groups.google.com/forum/#!topic/wpf-disciples/P-JwzRB_GE8">a great post</a> on this topic and suggested allow ViewModel to be coupled with the View can bring a lot of benefits. Although he seems to be in the minority in the following discussions, I have to say that I agree with him completely.

From a MVVM purist point of view, a ViewModel should always be agnostic to the View. This purist view bring problems

 - complex XAML, mixed with dynamic behaviors(ValueConverter, MultiBinding, Trigger)
 - difficulty to cover behaviors in XAML by unit test 
 - complex UI automation test cases to cover the complex XAML

One argument I often here about using View agnostic ViewModel is that you will be able to switch the view without affecting the ViewModel. But how much value it has to your project has to be evaluated. I have never seen any project that requires switching XAML view. And I would challenge anyone to try reusing the same ViewModel for web and desktop UI.

Allowing ViewModel to be coupled with the View is practically applying MVP to MVVM. 

And to follow the Single Responsibility Principle, we can also separate the Controller from the Presenter/ViewModel and end up with [MVC+MVP+MVVM The Complete Solution](http://gexiaoguo.github.io/MVC,-MVP-and-MVVM/ "MVC+MVP+MVVM The Complete Solution").
