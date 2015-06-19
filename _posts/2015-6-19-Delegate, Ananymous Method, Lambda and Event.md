---
layout: post
title: notes on delegate, ananymous method, lambda and event
---

###1 In the very begining
In the very beginning, there was no ananymous method, no lambda. The concept of delegate is clear and simple, but you will have to type more code.

```class DelegateDemo_TheVeryBasic
    {
        delegate int myDelegate(int i);//defines a type myDelegate
        static int myFunction(int i) //this function is only referenced when instantiating the myDel instance. Defining it explicitly here is an overhead.
        {
            return i + 1;
        }
        public static void RunDemo()
        {
            myDelegate myDel = new myDelegate(myFunction);//using the delegate by instantiating an instance of it.
            int res = myDel(0);//invoking the delegate instance
        }
    }
```
    
  Like it is noted in the code comments above, you defined a delegate type named myDelegate. Instantiate an instance of it. And invoke the instance.
  Many people say that a delegate type is a typed function pointer. It is not exactly accurate. A delegate type is actually a typed function group or list. Calling it function list probably makes the understanding much more straight forward.
  More instances can be added to the list with the += operator as below

```
myDel += myFunction;
```

  In this case, there will be multiple entires of myFunction in the myDel delegate instance. Invoking the delegate now will invoke myFunciton twice. One thing to not is that if myFunction has a return type, the return value of the last invocation of myFunction will be the return value of the delegate.
  
  entries can also be removed from the delegate with -= operator

```
myDel -= myFunction
```

  removes all entries of the myFunction from the method list. 
  
###2 Method Group Conversion
  To make the coding faster, then came along Method Group Conversion in C# 2.0. As a syntatic sugur, you dont have to type as much. The compiler will invoke the delegate(meaning the typed method group) constuctor for your.
   
  The new way of instantiating delegate instances is like below.  
```
myDelegate myDel = myFunction;//instantiating an instance with Method Group Conversion.
```       

###3 Ananymous Function
   To reducing typeing even more and also to make the code cleaner(if myFunction is only intended to be used with delegate, giving it a name and exposing it to others is really not clean), ananymous function is invented to to address this problem
   
   using ananymous function, the code can be written like below
   ```
   class DelegateDemo_WithAnanymousFunction
    {
        delegate int myDelegate(int i);//defines a type myDelegate
        public static void RunDemo()
        {
            myDelegate myDel = delegate(int i)
            {
                return i + 1;
            };
            int res = myDel(0);
        }
    }
   ```
   
###4 Lambda Expression
   To further simplify the syntax when writing ananymous method, Lambda expression is introduced
   ```
  class DelegateDemo_WithLambdaExpression
    {
        delegate int myDelegate(int i);//defines a type myDelegate
        public static void RunDemo()
        {
            //define a ananymous method with a statement lambda
            myDelegate myDel = (int i)=>
            {
                return i + 1;
            };
            
            //simplify the statement lambda to a expression lambda
            myDelegate myDel1 = (int i) => i + 1;

            //further simplify the expression lambda by removing the 'int' because the compiler knows the funciton takes an int parameter
            myDelegate myDel2 =  (i) => i + 1;

            //further simplify the expression lambda by removing the ()
            myDelegate myDel3 = i => i + 1;

            int res = myDel(0);
        }
    }
   ```
  C# compiler implemented a lot of syntatic sugar to further simplify the lambda expression. How far you should with simplification depends on personal taste. But the goldan standard is how easy it is to read the code, not how fast to type it.
