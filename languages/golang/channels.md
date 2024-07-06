# Channels

Sure, channels are a powerful concurrency primitive in Go, used for communication between goroutines. Here are the common patterns for using channels in Go:

#### 1. **Basic Send and Receive**

A simple pattern where one goroutine sends data on a channel and another goroutine receives it.

```go
package main

import (
	"fmt"
)

func main() {
	ch := make(chan int)

	go func() {
		ch <- 42 // Send
	}()

	val := <-ch // Receive
	fmt.Println(val)
}
```

#### 2. **Buffered Channels**

Buffered channels allow sending and receiving without requiring the other side to be ready at the same time.

```go
package main

import (
	"fmt"
)

func main() {
	ch := make(chan int, 2) // Buffered channel

	ch <- 1
	ch <- 2

	fmt.Println(<-ch) // 1
	fmt.Println(<-ch) // 2
}
```

#### 3. **Range over Channels**

Using a `for` loop to receive values from a channel until it is closed.

```go
package main

import (
	"fmt"
)

func main() {
	ch := make(chan int)

	go func() {
		for i := 0; i < 5; i++ {
			ch <- i
		}
		close(ch)
	}()

	for val := range ch {
		fmt.Println(val)
	}
}
```

#### 4. **Select Statement**

The `select` statement lets a goroutine wait on multiple communication operations.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch1 := make(chan string)
	ch2 := make(chan string)

	go func() {
		time.Sleep(1 * time.Second)
		ch1 <- "one"
	}()

	go func() {
		time.Sleep(2 * time.Second)
		ch2 <- "two"
	}()

	for i := 0; i < 2; i++ {
		select {
		case msg1 := <-ch1:
			fmt.Println("received", msg1)
		case msg2 := <-ch2:
			fmt.Println("received", msg2)
		}
	}
}
```

#### 5. **Fan-out and Fan-in**

**Fan-out** is a pattern where multiple goroutines receive from a single channel. **Fan-in** is a pattern where multiple goroutines send to a single channel.

**Fan-out:**

```go
package main

import (
	"fmt"
	"sync"
)

func worker(id int, jobs <-chan int, results chan<- int) {
	for j := range jobs {
		results <- j * 2
		fmt.Println("worker", id, "processed job", j)
	}
}

func main() {
	jobs := make(chan int, 10)
	results := make(chan int, 10)

	for w := 1; w < 4; w++ {
		go worker(w, jobs, results)
	}

	for j := 1; j < 6; j++ {
		jobs <- j
	}
	close(jobs)

	for a := 1; a < 6; a++ {
		fmt.Println("result", <-results)
	}
}
```

**Fan-in:**

```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)

func producer(id int, ch chan<- int) {
	for {
		time.Sleep(time.Duration(rand.Intn(1000)) * time.Millisecond)
		ch <- id
	}
}

func main() {
	ch := make(chan int)

	for i := 1; i <= 3; i++ {
		go producer(i, ch)
	}

	for {
		fmt.Println(<-ch)
	}
}
```

#### 6. **Timeouts**

Using `select` with `time.After` to implement timeouts.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int)

	go func() {
		time.Sleep(2 * time.Second)
		ch <- 1
	}()

	select {
	case val := <-ch:
		fmt.Println("received", val)
	case <-time.After(1 * time.Second):
		fmt.Println("timeout")
	}
}
```

#### 7. **Closing Channels**

Close a channel to indicate that no more values will be sent on it.

```go
package main

import (
	"fmt"
)

func main() {
	ch := make(chan int)

	go func() {
		for i := 0; i < 5; i++ {
			ch <- i
		}
		close(ch)
	}()

	for val := range ch {
		fmt.Println(val)
	}

	_, ok := <-ch
	if !ok {
		fmt.Println("channel closed")
	}
}
```

#### 8. **Pipeline**

Connecting multiple stages of computation through channels, forming a pipeline.

```go
package main

import (
	"fmt"
)

func gen(nums ...int) <-chan int {
	out := make(chan int)
	go func() {
		for _, n := range nums {
			out <- n
		}
		close(out)
	}()
	return out
}

func sq(in <-chan int) <-chan int {
	out := make(chan int)
	go func() {
		for n := range in {
			out <- n * n
		}
		close(out)
	}()
	return out
}

func main() {
	nums := gen(2, 3, 4)
	squares := sq(nums)

	for n := range squares {
		fmt.Println(n)
	}
}
```

These patterns provide a solid foundation for effectively using channels in Go for various concurrency needs.
