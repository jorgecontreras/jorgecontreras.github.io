# Profiling Go Applications with `pprof`: Addressing High CPU Usage in Production

In my years of experience as a software engineer, probably some of the most critical challenges I have faced are debugging and optimizing production applications. As part of the development process, I make sure I have a set of tools in place that allows me and my team to catch as many potential issues and bugs early in the process. Code linting, unit testing and performance testing as just some of the standard tasks that I always setup in my CI/CD pipelines. 

Nevertheless, there is always a possibility that some bugs and performance bottlenecks make their way into the production systems, especially in complex software with many components and dependencies. This was precisely the situation I faced recently in a live Go application, which was experiencing unexpectedly high CPU usage. My goal was to identify and resolve the source of this excessive usage. 

Fortunately, the Go ecosystem offers a robust solution with `pprof`, a built-in profiling tool that provides detailed insights into application performance. `pprof` emerged as a practical choice to tackle the performance bottlenecks in my application.

![Go pprof profile](/assets/images/goprof.webp)

## Understanding `pprof`: The Go Profiler

`pprof` is a profiler tool that is included in the Go standard library. It collects and visualizes statistics about a Go program to help developers identify performance issues. With `pprof`, you can analyze various aspects of your application such as CPU usage, memory allocation, and blocking profiles. This tool can be a game changer for performance optimization because it allows for detailed introspection into how Go routines are scheduled and run, and how memory is being used throughout the lifecycle of the application. The tool's integration into the Go language toolchain also means that it aligns seamlessly with the Go programming model, making it an indispensable resource for any Go developer looking to optimize their code.

## Using `pprof` to Profile Go Applications

Profiling an application in Go using `pprof` is a straightforward process that can yield deep insights into the performance characteristics of your code. Here are the steps I followed:

1. **Integration in Code**
   - I integrated `pprof` as middleware in my `main.go` file. This allowed `pprof` to start gathering profiling data from my running application.

```go
package main

import (
    "net/http"
    _ "net/http/pprof"
)

func main() {
    // Existing application code goes here

    // Start the pprof server
    go func() {
        log.Println(http.ListenAndServe("localhost:6060", nil))
    }()
}

```

> **Note on Production Use**: While `pprof` is an invaluable tool for profiling and debugging, it's important to exercise caution when using it in production environments. Exposing the `pprof` endpoint in a production environment can lead to potential security risks and unintended performance impacts. It's recommended to enable `pprof` only in development or controlled environments. In production, if absolutely necessary, ensure that access to the `pprof` endpoints is properly secured and restricted.


2. **Accessing the Profiling Endpoint**
   - I accessed the profiling endpoint by navigating to `localhost:6060/debug/pprof` in my web browser, which `pprof` exposes on my application's server. This endpoint provides access to the profiling data.

3. **Collecting Profile Data**
   - To collect specific profiling data, I returned to my terminal and executed the `go tool pprof` command with a targeted URL and a duration parameter: `go tool pprof localhost:6060/debug/pprof/profile?seconds=30`. This command specifies that pprof should collect 30 seconds of profiling data from the running application

4. **Generating Load**
   - With `pprof` collecting data, I used [Postman](https://www.postman.com/) to send requests to the API endpoints that were under performance scrutiny.

5. **Entering the `pprof` Tool**
   - After the 30 seconds elapsed, the `pprof` interactive interface was available for further analysis.

6. **Identifying Hotspots**
   - I used the `top` command in the `pprof` tool to identify the most CPU-intensive operations.

```
(pprof) top
Showing nodes accounting for 330ms, 80.49% of 410ms total
Dropped 19 nodes (cum <= 2.05ms)
Showing top 10 nodes out of 61
      flat  flat%   sum%        cum   cum%
     140ms 34.15% 34.15%      140ms 34.15%  runtime.cgocall
      50ms 12.20% 46.34%       50ms 12.20%  runtime.futex
      40ms  9.76% 56.10%       60ms 14.63%  runtime.mallocgc
      30ms  7.32% 63.41%       40ms  9.76%  runtime.heapBitsSetType
      20ms  4.88% 68.29%      180ms 43.90%  runtime.mcall
      20ms  4.88% 73.17%       20ms  4.88%  runtime.memmove
      10ms  2.44% 75.61%       10ms  2.44%  runtime.(*mheap).allocSpanLocked
      10ms  2.44% 78.05%       10ms  2.44%  runtime.nextFreeFast
      10ms  2.44% 80.49%       10ms  2.44%  runtime.scanobject

```

7. **Exploring Visualization Options**
   - The `help` command in `pprof` listed various visualization options that I could use to examine the profiling data.

8. **Exporting the Profile**
   - I selected a visualization type and exported the profiling data as a PDF for an in-depth examination.

9. **Analyzing the Data**
   - With the visualized data, I focused on the most CPU-intensive nodes to understand where optimizations were needed.


## Analyzing the Profiling Results

Once I had exported the profile data to a PDF, the next step was to decipher the call graph. A call graph is a directed graph that represents calling relationships between subroutines in a computer program. In the context of `pprof`, it visually maps function calls and the time spent in each function, allowing developers to pinpoint where optimizations can have the most impact.

To interpret the call graph generated by `pprof`, I referred to the official documentation provided by the developers of `pprof`. The documentation is available on GitHub and can be found here: [Interpreting the Callgraph](https://github.com/google/pprof/blob/main/doc/README.md#interpreting-the-callgraph).

I was able to understand the various metrics and representations within the graph, such as:

- **Nodes**: Represent functions or methods.
- **Edges**: Indicate the invocation between these functions.
- **Colors**: Denote the amount of time or memory consumed.

By carefully analyzing the graph, I identified the most CPU-intensive operations. These were the functions where my application spent the most time and were, therefore, the primary candidates for optimization.

## Conclusion

In retrospect, the challenge of optimizing Go applications is significantly streamlined by profiling tools like `pprof`. Such tools are indispensable for developers aiming to fine-tune their applications for optimal performance. `pprof` stands out for its native integration with Go, offering detailed insights across multiple dimensions like CPU and memory usage, Goroutine activity, and more.

While `pprof` is a robust option, the Go ecosystem and the broader landscape of performance profiling also offer other tools such as `trace`, `benchstat`, and integration with visualizers like Graphviz for comprehensive data analysis. Go developers are encouraged to explore these tools to find the right combination that suits their specific needs.

Profiling should be a regular part of the development lifecycle, beyond merely reacting to performance issues. Proactive profiling can prevent potential problems before they impact users, making it a best practice for serious application development. The path to performance excellence is ongoing, and with the right tools and practices, developers can ensure their applications are not only functional but also efficient and resilient.

## Links

[pprof](https://github.com/google/pprof)
