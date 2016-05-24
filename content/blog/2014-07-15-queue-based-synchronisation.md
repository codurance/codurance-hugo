---
title: Queue Based Synchronisation
author: Mashooq Badar
date: 2014-07-15T12:00:00Z

image:
  src: /assets/img/custom/blog/queue-based-synchronisation.jpg
tags:
  - threading
  - design

type: blog
layout: post
slug: queue-based-synchronisation
aliases: 
  - /2014/07/15/queue-based-synchronisation/
---

The first rule of using locks for thread synchronisation is, **"Do NOT use them!"**. Recently I saw an implementation that made heavy use of locks to synchronise access to a shared cache between two threads. The overall approach is explained in the diagram below:

{{< blog_image src="/assets/img/custom/blog/lock-based-synchronisation.jpg" title="Lock based synchronisation" >}}

Why not do the whole thing in a single thread? Well! the operations to the External Store are very time consuming and Thread-1 does not need to wait for them. So how do you solve this without using lock-based synchronisation?

The operations to the cache are very quick and can be done in a single thread. These operations are coming from multiple threads. We can funnel them through a single thread by using a thread-safe queue as explained in the following diagram:

{{< blog_image src="/assets/img/custom/blog/queue-based-synchronisation.jpg" title="Queue based synchronisation" >}}

Although this solution looks more complicated, the key advantage is that no low-level thread synchronisation is needed. Most good programming languages already provide thread-safe queues. Also, you can scale up using a thread pool for the operations to the external store. 

*Note: in both of the above approaches we need to ensure that the cache does not grow indefinitely. In case of the queue based approach we can use a a queue that blocks after a maximum capacity is reached. In case of the lock based approach the cache itself will need to block.*