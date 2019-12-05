---
layout: post
title:  "A Simple Example of Consumer-Producer Concurrency using the Python threading and queue modules"
date:   2019-12-05 00:00:00 -0700
categories: 
---
As part of my self-study of Python, I wanted to learn how to use concurrency in Python. It turns out there are a number of ways to do this. This post is about an exercise I did using the _threading_ and the _queue_ modules.

Concurrency can be tricky to think about correctly and is qualitatively far more difficult than sequential processes.

In this example a lot of the potential difficulty is handled and hidden under the hood by the thread-safe _queue_ module through its own implementation to ensure that the state of a queue is consistent and correct. 

From the Python documentation:
>The queue module implements multi-producer, multi-consumer queues. It is especially useful in threaded programming when information must be exchanged safely between multiple threads. The Queue class in this module implements all the required locking semantics.

So let's start with all the modules that are imported.

~~~~~ python
import threading
import queue
import time
import random
~~~~~


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
~~~~~

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
~~~~~

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

~~~~~ python
@time_it
def run_threads(num_producers: int = 1, num_consumers: int = 1) -> None:
    
    q = queue.Queue()
    
    ## Create the producer threads and get them running.
    ## Each producer produces a fixed number of items,
    ## but this could be randomized.
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

~~~~~ python
run_threads(20,  1)   ## 24.78 seconds
run_threads(20,  2)   ## 12.27 seconds
run_threads(20,  4)   ##  6.01 seconds
run_threads(20, 10)   ##  2.26 seconds
run_threads(20, 20)   ##  1.01 seconds
run_threads(20, 24)   ##  1.01 seconds
run_threads(20, 30)   ##  1.01 seconds
~~~~~
