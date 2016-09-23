---
layout: post
title: TPL, async, await guidelines
---

1. When using await, always explicity specify ConfigAwait
because: 


  - It prevents potential deadlocks.
  - It reduces future maintance effort. It is always easier to decide wether the continuation code can be put to the thread pool thread when you are the author of the code. It would much harder to do several years from now, and by someone who is reading your code the first time.
  - It removes more work from the current thread. After all, this is why we use await in the first place.
  - It improves overall system performance by reducing the number of thread contaxt switching
  
2. When using Task.Factory.StartNew and Task.ContinueWith, alwys specify a TaskScheduler
  Because, if no TaskSchedler is specified, StartNew will use the GetDefaultScheduler to locate a TaskScheduler. And return result of GetDefaultScheduler is highly dependent on the current running thread, which makes the threading model very difficult to reason with.
  {% highlight csharp %}
        private TaskScheduler GetDefaultScheduler(Task currTask)
        {
            if (m_defaultScheduler != null) return m_defaultScheduler;
            else if ((currTask != null)
                && ((currTask.CreationOptions & TaskCreationOptions.HideScheduler) == 0)
                )
                return currTask.ExecutingTaskScheduler;
            else return TaskScheduler.Default;
        }
  {% endhighlight %}

3. Prefer Task.GetAwaiter().GetResult() over Task.Result() or Task.Wait()
  Because the exception will be unwraped by GetAwaiter().GetResult() while Task.Result() and Task.Wait() will throw an AggregateException instead of unwrapping it.
