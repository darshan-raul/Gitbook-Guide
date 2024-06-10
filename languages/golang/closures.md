# Closures

**What is a Closure?**

In essence, a closure is a function's ability to "remember" and access variables from its surrounding scope (lexical environment), even after the outer function has finished executing. This remembered environment is "enclosed" with the inner function, forming the closure.

Think of it like a backpack that a function carries with it. The backpack contains the variables and values from the function's birthplace. Even if the function travels to a new location (gets called later), it still has access to the contents of its backpack (those remembered variables).

**What Are Closures Useful For?**

1. **Data Encapsulation:** Closures can create private variables within a function, protecting them from being modified directly from outside. This helps maintain data integrity.
2. **State Preservation:** They allow functions to maintain a state between calls, like a counter or a cached result. This can be useful for optimization and creating functions with memory.
3. **Partial Application and Currying:** Closures enable techniques where a function with multiple arguments is transformed into a sequence of functions, each taking a single argument. This is helpful for creating more modular and reusable code.
4. **Callbacks and Event Handling:** In asynchronous programming, closures capture the necessary context (variables) for a callback function to work correctly when it's executed later.

***



**Example 1: Basic Closure**

Go

```
package main

import "fmt"

func intSeq() func() int {
    i := 0
    return func() int {
        i++
        return i
    }
}

func main() {
    nextInt := intSeq()
    fmt.Println(nextInt()) // Output: 1
    fmt.Println(nextInt()) // Output: 2
    fmt.Println(nextInt()) // Output: 3

    newInts := intSeq()
    fmt.Println(newInts()) // Output: 1
}
```

**Explanation:**

* The `intSeq` function returns a closure (an anonymous function) that has access to and increments the variable `i` within its enclosing scope.
* Each call to the returned function `nextInt` increments and returns the current value of `i`, effectively maintaining its state between calls.

**Example 2: Closure with State**

Go

```
package main

import "fmt"

func makeAdder(x int) func(int) int {
    return func(y int) int {
        return x + y
    }
}

func main() {
    add5 := makeAdder(5)
    fmt.Println(add5(2)) // Output: 7

    add10 := makeAdder(10)
    fmt.Println(add10(2)) // Output: 12
}
```

**Explanation:**

* The `makeAdder` function returns a closure that "remembers" the value of `x` passed to it.
* Each returned closure (e.g., `add5` and `add10`) adds its captured `x` value to the argument `y` passed during invocation.

**Example 3: Data Encapsulation**

Go

```
package main

import "fmt"

func newCounter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}

func main() {
    counter := newCounter()
    fmt.Println(counter()) // Output: 1
    fmt.Println(counter()) // Output: 2

    counter2 := newCounter() // A new closure with its own 'count'
    fmt.Println(counter2())  // Output: 1
}
```

**Explanation:**

* The `newCounter` function creates a closure with a private variable `count`.
* The returned closure provides controlled access to `count` through increment and retrieval.
* Each call to `newCounter` creates a separate closure with its own isolated `count` variable.

