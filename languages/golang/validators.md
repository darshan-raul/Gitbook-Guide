# Validators

JSON validation in Golang typically involves unmarshaling JSON data into a struct and then validating the struct fields according to your requirements. Here’s a step-by-step guide on how to perform JSON validation in Golang:

#### Step 1: Define Your Struct

First, define a struct that represents the JSON structure you expect. Use struct tags to specify JSON field names and validation rules.

```go
package main

import (
    "encoding/json"
    "fmt"
    "github.com/go-playground/validator/v10"
    "log"
)

type User struct {
    Name  string `json:"name" validate:"required"`
    Email string `json:"email" validate:"required,email"`
    Age   int    `json:"age" validate:"gte=0,lte=130"`
}
```

#### Step 2: Unmarshal JSON into the Struct

Next, unmarshal the JSON data into the struct. If there are any errors during unmarshaling, handle them appropriately.

```go
func main() {
    jsonData := []byte(`{
        "name": "Alice",
        "email": "alice@example.com",
        "age": 25
    }`)

    var user User
    if err := json.Unmarshal(jsonData, &user); err != nil {
        log.Fatalf("Error unmarshaling JSON: %v", err)
    }
```

#### Step 3: Validate the Struct

Use the `validator` package to validate the struct fields according to the rules specified in the struct tags. The `validator` package is a popular and robust validation library for Go.

```go
    validate := validator.New()
    if err := validate.Struct(user); err != nil {
        if _, ok := err.(*validator.InvalidValidationError); ok {
            log.Fatalf("Invalid validation error: %v", err)
        }
        
        // Iterate through validation errors
        for _, err := range err.(validator.ValidationErrors) {
            fmt.Printf("Validation error: Field '%s' failed on the '%s' tag\n", err.StructField(), err.Tag())
        }
    } else {
        fmt.Println("Validation successful!")
    }
}
```

#### Full Example

Here is the complete code with all steps combined:

```go
package main

import (
    "encoding/json"
    "fmt"
    "github.com/go-playground/validator/v10"
    "log"
)

type User struct {
    Name  string `json:"name" validate:"required"`
    Email string `json:"email" validate:"required,email"`
    Age   int    `json:"age" validate:"gte=0,lte=130"`
}

func main() {
    jsonData := []byte(`{
        "name": "Alice",
        "email": "alice@example.com",
        "age": 25
    }`)

    var user User
    if err := json.Unmarshal(jsonData, &user); err != nil {
        log.Fatalf("Error unmarshaling JSON: %v", err)
    }

    validate := validator.New()
    if err := validate.Struct(user); err != nil {
        if _, ok := err.(*validator.InvalidValidationError); ok {
            log.Fatalf("Invalid validation error: %v", err)
        }

        for _, err := range err.(validator.ValidationErrors) {
            fmt.Printf("Validation error: Field '%s' failed on the '%s' tag\n", err.StructField(), err.Tag())
        }
    } else {
        fmt.Println("Validation successful!")
    }
}
```

#### Additional Tips

* **Custom Validation**: You can add custom validation functions if needed using the `validator` package's `RegisterValidation` method.
* **Validation Tags**: The `validator` package supports a variety of validation tags like `required`, `email`, `gte`, `lte`, `min`, `max`, etc.
* **Error Handling**: Proper error handling is crucial. Ensure you handle JSON unmarshaling errors and validation errors separately and appropriately.

This approach allows you to validate JSON data against a defined schema easily, ensuring your data conforms to expected formats and constraints before further processing.
