---
date: 2020-08-21T10:58:08-04:00
description: "Learn Go"
featured_image: "images/golang-logo.png"
tags: ["tech", "Go"]
title: "Learning a new language"
---

Learning Go has always been on my todo list for a few years. It has been gaining in popularity recently and is one of the top languages developers want to learn in 2020. Or maybe the Go song inspired me :laughing:

{{< youtube LJvEIjRBSDA >}}

I started with the [Tour of GO](https://tour.golang.org/) to learn the basic syntax and data structures. Its a great learning resource with a few exercises thrown in. My main motive was to learn about Concurrency in GO, which is where GO seems to score with developers. I have added my solutions to a repo [here](https://github.com/abhishekkh/learn-go)

Below is a sample program to create a worker pool that performs some concurrent tasks. The worker threads pick jobs in the job channel and update the results to the results channel once they are done. Finally we block until all the results are available in the results channel. 

```go
package main

import (
	"fmt"
	"time"
)

func worker (id int, jobs <-chan int, results chan<- int) {
	for j := range jobs {
		fmt.Println("worker", id, "started  job", j)
		time.Sleep(1 * time.Second)
		fmt.Println("worker", id, "finished job", j)
		results <- 2 * j
	}
}

func main() {
	numJobs := 5
	jobs := make(chan int, 5)
    results := make(chan int, 5)
    
	for w:=0; w < 3; w++ {
		go worker(w, jobs, results)
	}
	for i:=0; i < numJobs; i++ {
		jobs <- i
	}
	// close the job channel, let the receiver know there are no more jobs
	close(jobs)

	// wait for all the results to be completed
	for j:=0; j < numJobs; j++ {
		<- results
	}
}
```

> What did I learn?

- The code is definitely less verbose than **Java**. Kicking off go routines for the worker threads was very easy without any boilerplate. A good comparison of [Go vs Java](https://yourbasic.org/golang/go-vs-java/)
- Use of channels to communicate was something very unique which I havent seen before.
- Very quick to build and run
- The syntax is something to get used to. You have static typed variables and you can use := to get the implicit types during variable declaration. The type is always last in functions and variable declarations.
- Go built code as binaries which can be deployed easily. It has all the dependencies packaged in somewhat like java's _jar-with-dependencies.jar_
- Any unused package imports will lead to compile errors and they need to be cleaned up. This is a good to have especially for long running projects with many contributors and keeps the code clean.

## Resources
- A cool [blog](https://yourbasic.org/) with lots of examples.
- Learn Go with more [examples](https://gobyexample.com/)