# context

{% embed url="https://www.youtube.com/watch?v=Q0BdETrs1Ok&t=127s" %}

The `context` package in Go is used for passing request-scoped values, cancellation signals, and deadlines across API boundaries to all the goroutines involved in handling a particular request. It provides a way to manage and cancel long-running operations, propagate values across API boundaries, and handle timeouts. Here's a detailed explanation of the `context` package and some practical scenarios where you can use it.

**Understanding the `context` package**

The `context` package revolves around the `Context` interface, which defines four methods:

1. `Deadline()` returns the time when the work should be canceled.
2. `Done()` returns a channel that is closed when the work should be canceled.
3. `Err()` returns an error indicating why the work was canceled.
4. `Value()` returns a key-value pair associated with the `Context`.

The `context` package provides two main functions for creating `Context` values:

1. `context.Background()` returns an empty `Context` that is typically used as the root of a context tree.
2. `context.WithCancel(parent)` returns a new `Context` with a cancel function that can be used to cancel the `Context` and any derived `Context` values.

Additionally, there are other functions like `context.WithDeadline()`, `context.WithTimeout()`, and `context.WithValue()` that create derived `Context` values with additional properties like deadlines, timeouts, and key-value pairs.

**Practical Scenarios for Using `context`**

1.  **Canceling Long-running Operations**

    One of the most common use cases for the `context` package is canceling long-running operations, such as HTTP requests, database queries, or goroutines that may take a long time to complete. By passing a `Context` value to these operations, you can signal them to cancel their work when the `Context` is canceled.

    ```go
    func longRunningOperation(ctx context.Context) error {
        // Perform long-running operation
        // Check for cancellation periodically
        select {
        case <-ctx.Done():
            // Clean up and return an error
            return ctx.Err()
        default:
            // Continue the operation
        }
    }

    func main() {
        ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
        defer cancel()

        err := longRunningOperation(ctx)
        if err != nil {
            fmt.Println("Operation canceled:", err)
        }
    }
    ```
2.  **Propagating Values Across API Boundaries**

    The `context` package allows you to propagate values across API boundaries using `context.WithValue()`. This can be useful for passing request-scoped data like authentication tokens, user IDs, or request metadata through multiple layers of your application.

    ```go
    type authKey string

    func middlewareHandler(next http.Handler) http.Handler {
        return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            ctx := r.Context()
            authToken := r.Header.Get("Authorization")
            ctx = context.WithValue(ctx, authKey("auth"), authToken)
            next.ServeHTTP(w, r.WithContext(ctx))
        })
    }

    func endpointHandler(w http.ResponseWriter, r *http.Request) {
        ctx := r.Context()
        authToken, ok := ctx.Value(authKey("auth")).(string)
        if !ok {
            // Handle error
            return
        }
        // Use authToken for processing the request
    }
    ```
3.  **Enforcing Timeouts and Deadlines**

    The `context` package provides functions like `context.WithTimeout()` and `context.WithDeadline()` to create `Context` values with timeouts and deadlines. These can be used to enforce timeouts for operations like database queries, network requests, or any other long-running tasks.

    ```go
    func fetchData(ctx context.Context, url string) ([]byte, error) {
        req, err := http.NewRequestWithContext(ctx, http.MethodGet, url, nil)
        if err != nil {
            return nil, err
        }

        resp, err := http.DefaultClient.Do(req)
        if err != nil {
            return nil, err
        }
        defer resp.Body.Close()

        return io.ReadAll(resp.Body)
    }

    func main() {
        ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
        defer cancel()

        data, err := fetchData(ctx, "https://example.com/data")
        if err != nil {
            fmt.Println("Error fetching data:", err)
            return
        }

        // Process the fetched data
    }
    ```
4.  **Preventing Goroutine Leaks**

    The `context` package can be used to prevent goroutine leaks in concurrent applications. By creating a `Context` value with a cancellation function and passing it to goroutines, you can ensure that all goroutines are properly cleaned up when the operation is canceled or completed.

    ```go
    func processData(ctx context.Context, data []byte) {
        // Process data in a goroutine
        ch := make(chan struct{})
        go func() {
            defer close(ch)
            // Process data
            // ...
        }()

        // Wait for the goroutine to complete or context to be canceled
        select {
        case <-ctx.Done():
            // Context was canceled, clean up and return
            return
        case <-ch:
            // Goroutine completed successfully
        }
    }

    func main() {
        ctx, cancel := context.WithCancel(context.Background())
        defer cancel()

        data := []byte("some data")
        go processData(ctx, data)

        // Wait for some time or trigger cancellation
        // ...
        cancel()
    }
    ```

These are just a few examples of how the `context` package can be used in Go. The package is widely used in many parts of the Go standard library and third-party libraries, providing a consistent way of managing and canceling long-running operations, propagating request-scoped values, and enforcing timeouts and deadlines across different components of an application.

***

Here's a breakdown of `context.TODO()` and `context.Background()` in Go, their use cases, and when to choose one over the other:

**context.Background()**

* **Purpose:** The root of all contexts. Represents the basis for which other contexts get derived.
* **Use Cases:**
  * **Main function:** Typically used in the `main` function to initiate top-level contexts.
  * **Initialization:** Used during initialization phases where further context propagation isn't required.
  * **Tests:** Commonly used in simple unit tests that don't need sophisticated context handling.
  * **Top-level Requests:** Use for incoming requests at API boundaries where the internal chain of function calls might create and propagate more derived contexts.

**context.TODO()**

* **Purpose:** Acts as a placeholder. It stands out in code, signaling a missing context.
* **Use Cases:**
  * **Temporary During Development:** Use when you haven't implemented the functionality to propagate proper contexts, but still need to compile and run code.
  * **Placeholders for Future Refinement:** Mark code sections where adding a more appropriate context is intended.
  * **Prototyping:** Useful during prototyping when the context strategy isn't yet finalized.

**Key Differences**

* **Values & Deadlines:** Both `context.Background()` and `context.TODO()` return an empty context with no associated values or deadlines.
* **Intent:** `context.Background()` is usually a final choice for top-level contexts. `context.TODO()` is a clear signal that a context needs attention.

**When to Choose Which**

1. **Start with `context.Background()`:** Begin with `context.Background()` at the initial points of your program (often your `main`) and as the base for incoming requests.
2. **Use `context.TODO()` Sparingly:** Use `context.TODO()` cautiously and temporarily during development. Avoid leaving `context.TODO()` calls in final production code.
3. **Propagate Derived Contexts:** When you need to pass deadlines, cancellation signals, or values down a call chain, create derived contexts using functions like:
   * `context.WithDeadline()`
   * `context.WithTimeout()`
   * `context.WithCancel()`
   * `context.WithValue()`

**Important Note:** Never use `context.TODO()` for production code without replacing it with a context suitable for the situation.

**Example**

Go

```
func processRequest(ctx context.Context) {
    // If you don't have a more appropriate context yet:
    ctx = context.TODO() 

    // ... do some work ...

    // Replace with derived context before calling other functions:
    ctx, cancel := context.WithTimeout(ctx, 5*time.Second)
    defer cancel()
    subResult := doSomethingElse(ctx) 
    // ...
}
```

