---
layout: post
title:  "Limiting WIP / Throughput of cars on a highway"
date:   2023-12-29 00:00:00 -0700
categories: 
---
In agile approaches to managing projects, when thinking about limiting WIP (work in progress) sometimes I think of the movement of traffic on a highway as a metaphor to reinforce my intuition for the practice. After all, we’ve all felt the frustration and the tedium of crawling along in bumper to bumper traffic.

That said, does limiting the number of cars on a highway actually improve the throughput of the system as a whole? By throughput, I mean that given a fixed point on the highway, does having fewer cars going faster mean that more cars pass the point in a given amount of time as opposed to packing a lot of cars in the same space that are going slower?

I did some quick, back-of-the-envelope calculations. I made some simplifying assumptions for a simplistic model that ignores the complications of real-life traffic.

- The highway has one lane going in each direction.
- The cars move at a constant uniform speed.
- The length of a car is 14 feet.
- Each driver maintains a gap, a.k.a. “headway”, of 3 seconds of travel time between themselves and the car ahead of them as a buffer.

Here are the calculations I got using approximate values.

<< need table here >>

Following is a scatter plot of (Speed in MPH) x (Number of cars per minute)

<< scatter plot here >>

At 3 MPH there are 196 cars in the space of one mile, so while space utilization is very high, throughput is only about 10 cars per minute.

At 60 MPH there are only 19 cars in the space of one mile. Space utilization is very low, but throughput is much better at 19 cars per minute.

It looks like broadly speaking, traffic throughput is a supportive metaphor for the practice of limiting WIP. Of course, it’s just a metaphor, but I think it’s one that helps build intuition for why limiting WIP increases productivity over a given unit of time, which might seem counter-intuitive.

Interestingly, when I posed this problem to ChatGPT, part of its response was:

 “In a real-world scenario, the specific speed at which the maximum number of cars pass a given point depends on various factors, including driver behavior, road conditions, vehicle type, and traffic control measures. In ideal conditions and ignoring external factors, the speed where the maximum flow happens is typically around 45-55 miles per hour for many highways.”
