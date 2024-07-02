# Interfaces

***







***

### Advanced



#### Struct having extra functions than interface

In Go, if a struct implements more methods than defined in an interface, you can still call the extra methods, but you cannot call them directly on a variable of the interface type without type assertion or type conversion. Here’s an example to illustrate this:

#### Example

Let's define an interface `Animal` and a struct `Dog` that implements `Animal` and has an extra method `Bark`.

```go
package main

import (
	"fmt"
)

// Animal interface with one method
type Animal interface {
	Speak() string
}

// Dog struct implementing Animal interface
type Dog struct {
	Name string
}

// Speak method required by the Animal interface
func (d Dog) Speak() string {
	return "Woof!"
}

// Bark is an extra method not defined in the Animal interface
func (d Dog) Bark() string {
	return fmt.Sprintf("%s is barking!", d.Name)
}

func main() {
	var a Animal
	d := Dog{Name: "Buddy"}

	a = d

	// Call the Speak method from the Animal interface
	fmt.Println(a.Speak())

	// Attempting to call Bark method directly on interface variable 'a' will result in an error
	// fmt.Println(a.Bark()) // This won't compile

	// Type assertion to access the extra method
	if dog, ok := a.(Dog); ok {
		fmt.Println(dog.Bark())
	} else {
		fmt.Println("Type assertion failed")
	}

	// Another way to access the extra method by converting interface back to struct type
	dog := a.(Dog)
	fmt.Println(dog.Bark())
}
```

#### Explanation

1. **Interface Definition**: The `Animal` interface has a single method `Speak`.
2. **Struct Definition**: The `Dog` struct implements the `Animal` interface by providing the `Speak` method. It also has an extra method `Bark`.
3. **Assignment**: A `Dog` instance is assigned to an `Animal` interface variable. Since `Dog` implements `Animal`, this assignment is valid.
4. **Calling Methods**:
   * You can call the `Speak` method directly on the interface variable `a`.
   * You cannot call the `Bark` method directly on the interface variable `a` because `Animal` does not have a `Bark` method.
5. **Type Assertion**:
   * Using type assertion, you can access the underlying `Dog` type and call the `Bark` method.
   * The syntax `a.(Dog)` converts the interface variable back to the `Dog` type.
6. **Type Conversion**:
   * You can also use a type conversion directly to call the `Bark` method, but make sure that the conversion is safe and the type assertion succeeds.

#### Summary

If a struct implements more methods than defined in an interface, you can call the extra methods by asserting the interface back to the struct type. Directly calling the extra methods on the interface variable without type assertion is not possible.
