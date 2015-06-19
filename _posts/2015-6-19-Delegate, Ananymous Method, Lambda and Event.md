---
layout: post
title: Notes on delegate, anonymous method, lambda expression and event
---

###1 The Basic
  Many people say that a delegate conceptually is a typed function pointer. It is not exactly accurate, even conceptually. A delegate type is actually a typed function group or list. It can contain multiple entries. Each entry points to a function with the conforming signature with the delegate.

  In the very beginning, there was no anonymous method, no lambda. The concept of delegate is clear and simple.
  The steps to use delegate are: 
  ..1 You defined a delegate type named myDelegate
  ..2 You instantiate an instance of it by invoking its constructor
  ..3 You invoke the instance.
  
```c#
  class DelegateDemo_TheVeryBasic
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
  Since delegate are actually function list, you can add an entry with += operator and remove entries with -= operator like below.
```c#
myDel += myFunction;
```
  In this case, there will be multiple entries of myFunction in the myDel delegate instance. Invoking the delegate now will invoke myFunciton twice. One thing to not is that if myFunction has a return type, the return value of the last invocation of myFunction will be the return value of the delegate.

```c#
myDel -= myFunction
```
  removes all occurrences of myFunction from the method list. 
  
###2 Method Group Conversion
  To make coding faster, Method Group Conversion was invented in C# 2.0. As a syntactic sugar, you don’t have to type as much. The compiler will invoke the delegate(meaning the typed method group) constructor for your.
   
  The new way of instantiating delegate instances is like below.  
```c#
myDelegate myDel = myFunction;//instantiating an instance with Method Group Conversion.
```       

###3 Anonymous Function
   To reducing typing even more and also to make the code cleaner(if myFunction is only intended to be used with delegate, giving it a name and exposing it to others is not clean), anonymous function is invented to to address this problem
   
   using anonymous function, the code can be written like below
   ```c#
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
   To further simplify the syntax when writing anonymous method, Lambda expression is introduced
   ```c#
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
  C# compiler implemented a lot of syntactic sugar to further simplify the lambda expression. How far you should with simplification depends on personal taste. But the golden standard is how easy it is to read the code, not how fast to type it.
  
##5 Delegate and Event
Delegate concept matches perfectly with event pattern. A delegate is a function list that each entries can be seen as an observer. Invoking the delegate calls each observer one by one. An sample implementation of event is given below.
```c#
class DelegateDemo_SimulateEvent
    {
        public delegate int myDelegate(int i);//defines a type myDelegate
        public static myDelegate Event;
        private static void RaiseMyEvent()
        {
            var myevent = Event;
            if (myevent != null)
            {
                myevent(0);
            }
        }
        public static void RunDemo()
        {
            //do something and signal to all the observers
            RaiseMyEvent();
        }
    }
    class Program
    {
        private static int myEventHandler(int i)
        {
            Console.WriteLine("event observed");
            return 0;
        }
        static void Main(string[] args)
        {
            DelegateDemo_SimulateEvent.Event += myEventHandler;
            DelegateDemo_SimulateEvent.RunDemo();
        }
    }
```
This way of implementing event pattern is very flexible. In fact, it is too flexible that Microsoft invented EventHandler<T> to reduce the flexibility and make everyone conform to the [event raising and handling guideline](https://msdn.microsoft.com/en-us/library/w369ty8x.aspx)

The real event implemntation with EventHandler<T> : where T is EventArgs
```c#
        private void RaiseMyEvent()
        {
            var myevent = Event;
            if (myevent != null)
            {
                myevent(this, new EventArgs());
            }
        }
        public void RunDemo()
        {
            //do something and signal to all the observers
            RaiseMyEvent();
        }
    }
    class Program
    {
        private static void myRealEventHandler(object sender, EventArgs args)
        {
            Console.WriteLine("event observed");
        }
        static void Main(string[] args)
        {
            var demo = new DelegateDemo_RealEvent();
            demo.Event += myRealEventHandler;
            demo.RunDemo();
        }
    }
```

below are some rants about the Microsoft guidelines
1. EventHandler<T> is actually a generic type which defines an event. If it actually defines an event, then why name it EventHandler. This only adds confusion to developer new to C#
2. The function which raises the event is named OnRaiseCustomEvent. Calling it RaiseCustomEvent maybe clearer. Adding an On only confuses developers because it sounds like the handler rather than the raiser of the event.
