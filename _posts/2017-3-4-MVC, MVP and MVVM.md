---
layout: post
title: Using MVC, MVP and MVVM Together
---

There are often confusions about MVC, MVP and MVVM. These 3 patterns are all trying to address different concerns and do not have to be mutually exclusive. In a complex real world application, it is often beneficial to apply a combined pattern like below. 
<p><img src="/images/MVC+MVP+MVVM.png" alt="The Complete Solution"></p>

To exam the benefits of this combined pattern, let's start from each individual pattern and exam what they are missing. 

##The MVC Pattern
<p><img src="images\MVC.png" alt="MVC Diagram"></p>
With MVC, the Presenter and ViewModol are missing.
  
The MVC pattern dictates that a feature should be divided into there components based on three separate concerns. That is the presentation logic(a.k.a. the View), the control flow(a.k.a. the Controller), and the business logic(a.k.a the Model).
###View
The View handles presentation logic
###Controller
The Controller handles the control flow logic and updates the Model
###Model
The Model handles the business logic concern. In a generalized sense, it represents the whole business logic Layer.  

 The problem with MVC is that it does not specify how the View and the Model should be structured internally. If taken literally, one could and usually does structure the View into a single class. We will then end up with a fat view which has the following problems:

1. View is tightly coupled to the Model  
   What's worse is that because View is tightly coupled to the Model, feature requirement to the View can easily drip down to the Model and pollute the Business Logic layer.
2. View is difficult to test.  
   Because the View is monolithic, and it usually couples tightly with the UI framework, unit testing the View becomes really hard. Just think how will you paint the screen and then verify that the screen is correctly painted in a unit test.

##The MVP Pattern
<p><img src="images\MVP.png" alt="MVP Diagram"></p>
With MVP, the Controller and ViewModel are missing

MVP is not a pattern that specifies how to structure the whole system. It only dictates how to structure the View. MVP completely separates the View from the Model and also solves the unit test problem of the View.

###Model
Same as the Model in MVC, the Model should be the generalized Business Logic Layer instead of a single class. 
###Presenter
The Presenter reads data from the Model and drives the View through an interface. This makes testing of the Presenter much easier. All presentation logic should be captured in the Presenter so that there should be nothing left in the View that worth testing. 
###View
The View in MVP is an implementation of the IView interface. And the view should be so thin that it should be acceptable not to unit testing it.   

The problems with MVP is that if taken literally

1. Controller is often omitted  
   Because of lack of Controller, control flow has also to be handled by the Presenter. This makes the Presenter responsible for two concerns: updating the Model and presenting the Model. 
2. Data Binding is not utilized  
   If binding is possible with the UI framework, it should be utilized to simplify the presenter. I've seen cases that people claiming that since they are using MVP, data binding should not be allowed. The Presenter should only drive the View through the IView interface. There is no ground for this argument. The IView interface is for solving the unit test problem. Utilizing data binding and is not harming the testability of the Presenter.

###The MVVM Pattern
<p><img src="images\MVVM.png" alt="MVVM Diagram"></p>
The Presenter and Controller are missing.

With MVVM the ViewModel replaces the Presenter driving the View

1. View    
   Bind to the ViewModel properties and update itself when ViewModel properties change.
2. ViewModel
   Instead of driving the View through an interface, the ViewModel drives the view through changing its own properties and allow data binding to handle the rest

Problems with MVVM are that   
 
1. Controller is often omitted  
   Because of lack of Controller, control flow has also to be handled by the ViewModel. This makes the ViewModel responsible for two concerns: updating the Model and presenting the Model.
2. The View becomes complex
   Because ViewModel can only drive the View with data binding, often complex binding tricks has to be applied(multi-binding, triggers, code behind, ...). 

The problems with MVVM is partly due to the fact that, in XAML community, there is wide belief that the ViewModel should be **decoupled** to the View. Because of this belief, ViewModel properties often made reflect the business logic rather than what the View needs. And the View needs to do additional magic to convert ViewModel properties, which is made more business logic oriented, into something that is displayable.  
   
For example, if for an error indicator on the UI which needs to turn red in case of error, it is widely believed that 

Defining a boolean HasError property on the ViewModel and then View converts the boolean flag into color


    ```csharp
    public class ViewModel 
    {
       bool HasError { get; set; }
    }   
    ```  


is better than to simply define a ColorOfErrorIndicator property on the ViewModel.


    ```csharp  
    public class ViewModel 
    {
       Color ColorOfErrorIndicator { get; set; }
    }   
    ```  
   

There is [a length discussion on this by Josh Smith](https://groups.google.com/forum/#!topic/wpf-disciples/P-JwzRB_GE8). But it seemed that he was not able to convince the community that this is a bad and costly belief. 


