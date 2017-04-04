---
layout: post
title: Task, async, and await 
---
# Benefits of Asynchrony 
1. Unblocking the UI thread
   This only maters to WPF and WinForm applications but does not mean much for Asp.NET applications.
   Asp.NET applications are automatically multi-threaded. For details, please see [How Asp.Net process requests with Threadpool](https://docs.microsoft.com/en-us/aspnet/web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45 "How Asp.Net process requests with Threadpool") 
    <p><img src="/images/Asp-WPF.png" alt="Asp.NET vs WPF Threading Model"></p>
2. Increase parallelism (process a single request faster)
   Throw more CPU at the computation so that it can finish sooner.

2. Increase Throughput(process more request)
   Thread is a OS resource, it consumes space in the OS tables and ~1MB of memory. Blocking a thread is waiting all the resources that a thread allocates.     
   This normally, won't matter much for WinForm and WPF applications. But it does for Asp.Net applications. The default maximum number of thread pool threads for.NET 4.5 is 5,000. In case of thread starvation a.k.a all threads are busy, Asp.NET will first try to create more thread. And when the maximum limit is reached, it will start to queue HTTP requests. When the is full, it will return HTTP 503 (Server Too Busy).  
## some math about synchronous http requests  
- 1 sec duration, 5000 requests/sec without queuing
- 100 ms duration, 50000 requests/sec without queuing
- 10 s duration, 500 requests/sec without queuing

But this math is only true if we have unlimited amount of CPU. If 10% of the request process duration is CPU time, and we have 16 cores on a machine. Then above math will change to

- 1 sec duration, 100 ms CPU time, 160 requests/second without queuing
- 100 ms duration, 10 ms CPU time, 1600 request/second without queuing
- 10 s duration, 1 s CUP time, 16 requests/sec without queuing


# Asynchrony in Detail

## await
Split the code into different states in a state machine
The details of the threading model of the state machine are shown below
<p><image src="/images/async_await_threading_model.PNG"/></p>

## stack spilling
If await split code into multiple pieces and run in different threads, how can local variables be shared between different stages. .Net enables stack variable sharing by hoisting them into heap and restored them back onto stack   

## thread ambient information
If a single method is split into pieces and run in different thread, then do these different pieces of code see different thread ambient information (e.g. SecurityContext, CurrentCulture, Principle). Historically these information by design, before TPL, async/awiat are invented, are attached to the running thread.

Thread ambient information is captured into an ExecutionContext in the thread creating the calling the await, and restored to the thread executing the continuation.  

# Code Snippets for Reference

## SynchronizationContext #

        public virtual void Post(SendOrPostCallback d, Object state)
        {
            ThreadPool.QueueUserWorkItem(new WaitCallback(d), state);
        }


## AspNetSynchronizationContext #

        public override void Post(SendOrPostCallback callback, Object state)
        {
            _state.Helper.QueueAsynchronous(() => callback(state));
        }

## DispatcherSynchronizationContext 

        public override void Post(SendOrPostCallback d, Object state)
        {
            _dispatcher.BeginInvoke(_priority, d, state);
        }

## SynchronizationContextAwaitTaskContinuation
        private static void PostAction(object state)
        {
            var c = (SynchronizationContextAwaitTaskContinuation)state;
            c.m_syncContext.Post(s_postCallback, c.m_action);
        }

# References
- [ExecutionContext vs SynchronizationContext](https://blogs.msdn.microsoft.com/pfxteam/2012/06/15/executioncontext-vs-synchronizationcontext/) 
- [Microsoft Reference Source](https://referencesource.microsoft.com/)