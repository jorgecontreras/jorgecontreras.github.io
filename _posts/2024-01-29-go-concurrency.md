# Concurrency in Go: The basics

## What is concurrency and why does it matter?

Concurrency is handling several tasks within a given timeframe. Most modern applications, such as web servers, databases, and user interfaces, require real-time processing and responsiveness. Concurrency allows to handle multiple tasks, like API requests or database calls, simultaneously. This helps us developers improve the performance of the applications our users love. Concurrency allows for maximum optimization of computing resources, making our systems highly efficient while lowering infrastructure costs.

> Concurrency optimizes computing resources, maximizes performance and minimize infrastructure costs.

## Why is concurrency hard to implement?

Concurrency is challenging to implement for several reasons, largely due to the complexities and unpredictable nature of managing multiple tasks that run at overlapping times. Very often, these concurrent tasks need to share resources and it is very easy to make mistakes and interfere with other tasks. For example, if one task attempts to read from the memory while another task is writing. All these tasks being executed at the same time need an efficient way to synchronize and ensure there are no conflicts between them. Deadlocks, livelocks, starvation and race conditions are just a few examples of these complexities[1].

## How is concurrency implemented in different programming languages?

Concurrency is not a new concept. The idea of concurrency in computing began with the development of multiprogramming systems. The Atlas Computer, developed at the University of Manchester and operational in 1962, is often credited as one of the first machines to provide a form of multiprogramming. The semaphore, invented by Dijkstra and colleagues[2], is one of the first primitives for all things related to synchronization.

While it is possible to implement these primitives in languages like C or C++, it can be challenging, as these languages provide low-level control over hardware and memory, but it is up to the developer to control the threads and shared resources, keep track of memory usage, free up space and so on.

Over time, there has been a development of techniques to facilitate the implementation of concurrent systems. In the world of Javascript, NodeJS implements concurrency using a non-blocking, event-driven architecture, primarily centered around the event loop and callbacks. In PHP, the OpenSwoole framework enables PHP developers to write asynchronous, concurrent, and highly scalable applications without needing extensive knowledge of non-blocking I/O programming.

## What makes Go an excellent choice for concurrent applications?

Go's concurrency model, based on the Communicating Sequential Processes (CSP) theory, makes concurrent programming intuitive [3]. Go abstracts many of the complexities that arise from concurrent system architectures like distributed systems and cloud computing. The go runtime, via its scheduler, takes care of managing goroutines, which are Go's lightweight threads of execution [4].

Goroutines are much more lightweight than OS threads. They have a smaller memory footprint, and the overhead of creating and destroying a goroutine is significantly lower than that of an OS thread. This allows Go programs to easily handle thousands or even millions of goroutines.

Go by itself does not eliminate the complexities associated to concurrent programming, but it definitely makes them easier to manage. It provides composable primitives like goroutines, channels and waitgroups to facilitate synchronization. Its efficient garbage collector takes care of freeing up resources for you. Additionally, Go’s standard library includes a wealth of packages that are well-suited for building cloud-native applications. Additionally, Go provides the performance benefits of a compiled language, which is critical for cloud and network services that need to handle high loads efficiently.

## How is concurrency achieved in Go?

Let's take a look at how goroutines are created:

```go
package main

import (
    "fmt"
    "time"
)

func Pink() {
    fmt.Println("Hi, Pink Goroutine!")
}

func Purple() {
    fmt.Println("Hello, Purple Goroutine!")
}

func Green() {
    fmt.Println("Hello, Green Goroutine!")
}

func Orange() {
    fmt.Println("Hello, Orange Goroutine!")
}

func main() {
    // start the goroutines
    go Pink() 
    go Purple() 
    go Green()
    go Orange()
} 
```

The main process triggers the goroutines using the go keyword before the function call. These goroutines or threads start execution immediately and they run concurrently.

![](/assets/images/goroutines.gif)
_goroutines executing concurrently._

Just imagine all the time-consuming tasks that you could execute at the same time! Pulling from different external resources, reading from a database, calling an API. As long as they don't depend on each other, there is no need to wait for one task to complete to start working on the next.

In some cases though, you may need to actually wait for some goroutines to complete before starting the next one. For that, Go provides waitgroups:

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func Pink(wg *sync.WaitGroup) {
    defer wg.Done() // Decrement counter when done
    fmt.Println("Hi, Pink Goroutine!")
}

func Purple(wg *sync.WaitGroup) {
    defer wg.Done() // Decrement counter when done
    fmt.Println("Hello, Purple Goroutine!")
}

func Green(wg *sync.WaitGroup) {
    defer wg.Done() // Decrement counter when done
    fmt.Println("Hello, Green Goroutine!")
}

func Orange() {
    fmt.Println("Hello, Orange Goroutine!")
}

func main() {
    var wg sync.WaitGroup

    // Increment the WaitGroup counter for each goroutine
    wg.Add(3)

    // Start the first three goroutines
    go Pink(&wg)
    go Purple(&wg)
    go Green(&wg)

    // Wait for the first three goroutines to complete
    wg.Wait()

    // Now start the Orange goroutine
    go Orange()

} 
```
waitgroups: The process waits for the first 3 goroutines to complete.

Waitgroups give you the ability to wait for goroutines to be completed before moving on.

Why is synchronization important, you ask? Let's take a closer look the animation above. Did you notice the orange goroutine was not completed? That's because the process continues its execution and the goroutine may not get enough time to complete whatever it was doing. Adding that goroutine to the waitgroup would solve the issue. There could be many places in your system that requires this type of synchronization: use it wisely!

Finally, let's see what channels can do for us. Go's philosophy to prevent common issues like data races goes something like this:

> Do not communicate by sharing memory; instead, share memory by communicating.

What does this mean? Instead of explicitly using locks to mediate access to shared data, Go encourages the use of channels to pass references to data between goroutines. Let's see a code example:

```go
package main

import (
    "fmt"
)

func main() {
    messages := make(chan string)

    go func() { messages <- "Hello" }()
    go func() { messages <- "World" }()

    msg1 := <-messages
    msg2 := <-messages

    fmt.Println(msg1, msg2)
} 
```

In this example, two goroutines communicate strings through a channel. Instead of accessing a shared variable, they use the channel to send and receive messages.

This principle is one of the key reasons why Go's concurrency model is considered more straightforward and robust compared to traditional thread-based models.

## Wrapping up

Concurrency enables applications to handle multiple tasks efficiently, essential in applications driven by real-time data, high-performance demands, and the need for scalable systems that make the most optimal use of computing resources. Go's design principles, its simplicity and efficient communication over shared memory, makes it a great choice for distributed systems and cloud-native technologies. Go was designed with concurrency in mind since its inception, making it stand out from other programming languages.

## References

[1] https://www.cs.yale.edu/homes/aspnes/pinewiki/Deadlock.html

[2] https://pages.cs.wisc.edu/~remzi/OSTEP/threads-sema.pdf

[3] https://www.cs.cmu.edu/~crary/819-f09/Hoare78.pdf

[4] Cox-Buday, K. (2017). Concurrency in go: Tools and techniques for developers. O'Reilly Media.