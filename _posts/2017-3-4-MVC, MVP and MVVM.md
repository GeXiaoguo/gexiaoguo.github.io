---
layout: post
title: Using MVC, MVP and MVVM Together
---

MVC, MVP and MVVM are all design patterns trying to separate the concern of Business Logic and Presentation.  There are often confusions and debates about the when to use which. But, in fact,  these patterns do not have to be mutually exclusive. They each addresses different concerns and can be used together like below.
<p><img src="images\mvc+mvp+mvvm.png" alt="The Complete Solution"></p>
The component responsibilities are listed below    


1. View    
   Painting the UI effects as directed by the Presenter and(or) from data binding. View should be as thin as possible and unit testing shouldn't event be necessary.
   
    Some notes to XAML developers
    

    - stop using code behind
    - stop using advanced XAML features(triggers, converters, ...). Don't try to do everything with data binding, add a presenter.

2. Controller    
   Translating user actions into Business Logic commands. There should not be any business logic code in the controller.

3. Presenter
   Directly manipulating the View to paint UI effects.

4. ViewModel
   If data binding is available to you, helping the Presenter in controlling the display with databinding
   Otherwise, server as DTO(data transfer object) in passing data to the view(e.g. to Web front end)

5. Model
   Model here means business logic layer as a whole. It itself maybe comprised of complex layers and components.  

Let exam how this complete solution is different from each individual pattern and what do you miss for using a single pattern.

<p><img src="images\mvc.png" alt="MVC Diagram"></p>
With MVC, the Presenter and ViewModol are missing.

<p><img src="images\mvp.png" alt="MVP Diagram"></p>
With MVP, the Controller an ViewModel are missing.

<p><img src="images\mvvm.png" alt="MVVM Diagram"></p>
The Presenter and Controller are missing.