---
layout: post
title: Is It Ok to Couple ViewModel with View
---
Josh Smith had <a href="https://groups.google.com/forum/#!topic/wpf-disciples/P-JwzRB_GE8">a great post</a> on this topic. Although he seems to be in the minority in the following discussions, I have to say that I agree with him completely. A lot of people seem to believe that having a View agnostic ViewModel is automatically a good thing. I think it should all boils down to benefit/cost analysis. I just can't justify the extra cost in trying to develop a completely View agnostic ViewModel

The costs/benefits I can think of for each camp are listed below.


Cost/Benefit of loosely coupled ViewModel


 - minus: complex xaml, mixed with dynamic behaviors(ValueConverters, Behaviors)
 - minus: complex UI automation test cases to cover the complex view
 - plus: being able to switch view without affecting the ViewModel. But the real benefit has to be aligned with business need. Is there any demand, plan to switch views


Benefit of a tightly coupled ViewModel


- plus: a much less lightweight View(e.g. reduce ValueConverters, easier to eliminate code behind)
- plus: less UI Automation testing 
- plus: it is much easier to unit test ViewModel than UI automation testing the View. So, if putting view specific code( e.g. a visibility property) does not affect the unit testability of the ViewModel, it should be encouraged.

The decision should be based on how much each point costs/values to your project rather than how well each camp fits your philosophical belief.  
