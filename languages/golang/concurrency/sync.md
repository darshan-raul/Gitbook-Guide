# Sync

The `sync` package in Go provides synchronization primitives that are commonly needed in concurrent programming. It includes tools for mutual exclusion (mutexes), wait groups, once execution, and other synchronization needs. Here’s an overview of some of the key components and their use cases:

#### 1. **Mutex (sync.Mutex)**

A `Mutex` is used to ensure that only one goroutine can access a critical section of code at a time. It is useful for protecting shared resources.

**Example:**

```go
package main

import (
	"fmt"
	"sync"
)

var (
	counter int
	mu      sync.Mutex
)

func increment(wg *sync.WaitGroup) {
	defer wg.Done()
	mu.Lock()
	counter++
	mu.Unlock()
}

func main() {
	var wg sync.WaitGroup

	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go increment(&wg)
	}

	wg.Wait()
	fmt.Println("Counter:", counter)
}
```

#### 2. **RWMutex (sync.RWMutex)**

An `RWMutex` is a reader/writer mutual exclusion lock. It allows multiple readers to hold the lock simultaneously but only one writer at a time. This is useful when you have a resource that is read frequently but written infrequently.

**Example:**

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

var (
	counter int
	rwMu    sync.RWMutex
)

func read(wg *sync.WaitGroup) {
	defer wg.Done()
	rwMu.RLock()
	defer rwMu.RUnlock()
	fmt.Println("Read counter:", counter)
}

func write(wg *sync.WaitGroup) {
	defer wg.Done()
	rwMu.Lock()
	counter++
	rwMu.Unlock()
}

func main() {
	var wg sync.WaitGroup

	for i := 0; i < 3; i++ {
		wg.Add(1)
		go write(&wg)
	}

	for i := 0; i < 5; i++ {
		wg.Add(1)
		go read(&wg)
	}

	wg.Wait()
}
```

#### 3. **WaitGroup (sync.WaitGroup)**

A `WaitGroup` waits for a collection of goroutines to finish executing. It is useful for ensuring that a set of concurrent tasks have completed before proceeding.

**Example:**

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func worker(id int, wg *sync.WaitGroup) {
	defer wg.Done()
	fmt.Printf("Worker %d starting\n", id)
	time.Sleep(time.Second)
	fmt.Printf("Worker %d done\n", id)
}

func main() {
	var wg sync.WaitGroup

	for i := 1; i <= 5; i++ {
		wg.Add(1)
		go worker(i, &wg)
	}

	wg.Wait()
	fmt.Println("All workers completed")
}
```

#### 4. **Once (sync.Once)**

A `Once` ensures that a piece of code is executed only once, even if called from multiple goroutines.

**Example:**

```go
package main

import (
	"fmt"
	"sync"
)

var once sync.Once

func initialize() {
	fmt.Println("Initializing...")
}

func worker(wg *sync.WaitGroup) {
	defer wg.Done()
	once.Do(initialize)
	fmt.Println("Worker done")
}

func main() {
	var wg sync.WaitGroup

	for i := 0; i < 3; i++ {
		wg.Add(1)
		go worker(&wg)
	}

	wg.Wait()
}
```

#### 5. **Cond (sync.Cond)**

A `Cond` is used for waiting and signaling between goroutines. It allows goroutines to wait for some condition to be met.

**Example:**

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

var (
	sharedRsc = false
	mu        sync.Mutex
	cond      = sync.NewCond(&mu)
)

func waitForResource(id int, wg *sync.WaitGroup) {
	defer wg.Done()
	cond.L.Lock()
	for !sharedRsc {
		cond.Wait()
	}
	cond.L.Unlock()
	fmt.Printf("Goroutine %d proceeding\n", id)
}

func main() {
	var wg sync.WaitGroup

	for i := 0; i < 3; i++ {
		wg.Add(1)
		go waitForResource(i, &wg)
	}

	time.Sleep(1 * time.Second)

	cond.L.Lock()
	sharedRsc = true
	cond.L.Unlock()
	cond.Broadcast()

	wg.Wait()
}
```

These are just a few of the many use cases for the `sync` package in Go. It is a powerful tool for handling concurrency and ensuring that your programs run correctly and efficiently.
