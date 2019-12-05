---
layout: post
title:  "A Simple Example of Consumer-Producer Concurrency with the Python threading and queue modules"
date:   2019-12-05 00:00:00 -0700
categories: 
---
As part of my self-study of Python, I wanted to learn how to use concurrency. It turns out there are a number of ways to do this. This post is about an exercise I did using the _threading_ and the _queue_ modules.

Concurrency can be tricky to think about correctly and is qualitatively far more difficult to get right than sequential processing.

In this example a lot of the potential difficulty is handled and hidden under the hood by the thread-safe _queue_ module which ensures that the state of a queue object is kept consistent and correct. 

From the Python documentation:
>The queue module implements multi-producer, multi-consumer queues. It is especially useful in threaded programming when information must be exchanged safely between multiple threads. The Queue class in this module implements all the required locking semantics.

<br />
Let's start with the modules that are imported.

~~~~~ python
import threading
import queue
import time
import random
~~~~~

<br />
The class Producer contains the method that does the producing along with a couple of attributes. "Producing" means adding items to the queue. In this exercise all instances of Producer are producing the same number of items, but that could be easily varied or randomized by the code that creates the instances. A thread terminates when the _produce()_ method completes its loop and then exits.

The _idnum_ attribute is used in the _print_ statement, which gives an indication of the interleaved execution of the threads in an order that is different from the order in which they are created.

~~~~~ python
class Producer:

    def __init__(self, idnum: int, nitems: int):
        self.idnum = idnum
        self.num_items = nitems  ## Number of items to produce.
        
    def produce(self, q: queue.Queue) -> None:
        ## Note that each producer sleeps for 0.20 second to produce an item
        ## This amount of time could be randomized within some interval
        produce_time = 0.20

        while self.num_items > 0:
            item = random.randint(1, 50)
            q.put(item)
            self.num_items -= 1
            time.sleep(produce_time)
            
        print(f'Producer thread {self.idnum} is terminating \n')
~~~~~

<br />
The class Consumer contains the method that does the consuming along with attributes. "Consuming" means removing items from the queue.

Again, the _idnum_ attribute is used in the _print_ statement, which indicates the interleaved execution order of the threads.

But how to terminate the consumer threads? At first, I thought of having the main thread just kill these sub-threads. After some reading, that seems like a really bad idea. As a general policy, a thread should be _requested_ to end itself, giving itself a chance to release any resources it is holding and doing any other desired cleanup.

In this example, the _stop_consuming_ attribute is initialized to _False_, then set to _True_ by the main thread so that the sub-thread knows when to stop accessing the queue for items to consume.

Note that queue.Queue.get() raises an exception if the queue is empty.

Also note that when removing an item from _Queue_ you also have to call _task_done()_ so that the main thread's _join()_ on the Queue will succeed and not continue to wait. The _join()_ will hang otherwise. 

~~~~~ python
class Consumer:

    def __init__(self, idnum: int):
        self.idnum = idnum
        self.stop_consuming = False
        
    def consume(self, q: queue.Queue) -> None:
        ## Note that each consumer sleeps for 0.25 second to consume an item
        ## This amount of time could be randomized within some interval
        consume_time = 0.25
        while not self.stop_consuming:
            try:
                item = q.get(block=False)
                q.task_done()  ## Don't forget to call task_done() on the queue
            except queue.Empty:
                pass
            time.sleep(consume_time)
    
        print(f'Consumer thread {self.idnum} is terminating \n')
~~~~~

<br />
The _time_it()_ function is just a decorator for timing the main thread and displayed its elapsed execution time.

~~~~~ python
def time_it(func):
    def wrapper(*args, **kwargs):
        start_time = time.time()
        func(*args, **kwargs)
        stop_time = time.time()
        print(f'\n Elapsed time for {func.__name__}', \
              f'was {round(stop_time - start_time, 2)} seconds \n')
    return wrapper
~~~~~

<br />
And the function for the main thread, which creates the producer  and the consumer threads and manages them. In particular, it tells the consumer threads when to stop.

~~~~~ python
@time_it
def run_threads(num_producers: int = 1, num_consumers: int = 1) -> None:
    
    q = queue.Queue()
    
    ## Create the producer threads and get them running.
    ## Each producer produces a fixed number of items,
    ## but this could be varied or randomized.
    producer_threads = []
    num_of_items = 5
    for id in range(0, num_producers):
        prod = Producer(idnum=id, nitems=num_of_items)
        t = threading.Thread(target=prod.produce, args=[q])
        t.start()
        producer_threads.append(t)
        
    ## Create the consumer threads and get them running.
    consumers = []
    for id in range(0, num_consumers):
        cons = Consumer(idnum=id)
        t = threading.Thread(target=cons.consume, args=[q])
        t.start()
        consumers.append(cons)
        
    ## Wait for all the producer threads to complete
    for th in producer_threads:
        th.join()

    ## Wait for all items in the queue to be consumed.
    q.join()
    
    ## Tell all the consumers to stop.
    for cons in consumers:
        cons.stop_consuming = True
    
~~~~~

<br />
Some sample runs with 20 producer threads and an increasing number of consumer threads. This shows how overall execution time is dramatically reduced by adding consumer threads, but only up to the point where there are enough consumers to handle the load being created by the producers.

~~~~~ python
run_threads(20,  1)   ## 24.78 seconds
run_threads(20,  2)   ## 12.27 seconds
run_threads(20,  4)   ##  6.01 seconds
run_threads(20, 10)   ##  2.26 seconds
run_threads(20, 20)   ##  1.01 seconds
run_threads(20, 24)   ##  1.01 seconds
run_threads(20, 30)   ##  1.01 seconds
~~~~~

<br />
**Sources:**

&nbsp;&nbsp;&nbsp;&nbsp;_queue - A synchronized queue class_  
&nbsp;&nbsp;&nbsp;&nbsp;<https://docs.python.org/3/library/queue.html>

&nbsp;&nbsp;&nbsp;&nbsp;_threading - Thread-based parallelism_  
&nbsp;&nbsp;&nbsp;&nbsp;<https://docs.python.org/3/library/threading.html>

&nbsp;&nbsp;&nbsp;&nbsp;_Terminating a thread_  
&nbsp;&nbsp;&nbsp;&nbsp;<https://www.oreilly.com/library/view/python-cookbook/0596001673/ch06s03.html>
