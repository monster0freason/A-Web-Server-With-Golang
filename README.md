# Go Server

This repository contains a basic Go web server that serves static files and handles form submissions. Below, you'll find detailed explanations of each step involved in setting up and running this server, along with an explanation of the main `main.go` file.

## Setup

1. **Install Golang**: Ensure that you have Golang installed on your system. You can find installation instructions for various platforms [here](https://golang.org/doc/install).

2. **Create Project Directory**: Navigate to the directory where you want to create your Go project.

3. **Clone Repository**: Clone this repository into your project directory using the following command:
   ```bash
   git clone https://github.com/monster0freason/A-Web-Server-With-Golang
   ```

4. **Navigate to Project Directory**: Change into the project directory using:
   ```bash
   cd go-server
   ```

## Running the Server

1. **Build the Executable**: First, build the executable binary using:
   ```bash
   go build
   ```

2. **Run the Server**: Execute the built binary to start the server:
   ```bash
   ./go-server
   ```

3. **Access the Server**: Open your web browser and navigate to `http://localhost:8080` to access the server.

## File Structure

- **`index.html`**: Contains the HTML markup for the home page of the server.
- **`form.html`**: Contains the HTML markup for the form page of the server.
- **`main.go`**: The main Go source code file that defines the server logic.

## Detailed Explanation of `main.go`

### Part 1: Import Statements

```go
import (
    "fmt"
    "log"
    "net/http"
)
```

**Explanation:**

In Go, import statements are used to bring in functionality from other packages. Here's what each imported package does:

- `fmt`: This package provides functions for formatted input and output. It's commonly used for printing messages to the console and formatting strings.
  
- `log`: The `log` package is used for logging messages, particularly useful for debugging and error handling. It provides functions for writing log messages to various destinations, such as standard error or a file.

- `net/http`: This package provides HTTP client and server implementations. It's used for handling HTTP requests and responses, setting up HTTP servers, and making HTTP requests to external servers.

### Part 2: formHandler Function

```go
func formHandler(w http.ResponseWriter, r *http.Request) {
    if err := r.ParseForm(); err != nil {
        fmt.Fprintf(w, "ParseForm() err: %v", err)
        return
    }
    fmt.Fprintf(w, "POST request successful\n")
    favoritePackage := r.FormValue("favoritePackage")
    fmt.Fprintf(w, "favoritePackage = %s\n", favoritePackage)
}
```

**Explanation:**

This function, `formHandler`, is responsible for handling form submissions. Let's break down each part:

- `func formHandler(w http.ResponseWriter, r *http.Request)`: This line defines a function named `formHandler` that takes two parameters: `w`, which is an `http.ResponseWriter`, and `r`, which is an `http.Request`. The `http.ResponseWriter` is used to write the response back to the client, and the `http.Request` contains information about the incoming request.
  
- `if err := r.ParseForm(); err != nil { ... }`: This line parses the form data from the incoming request (`r`) and populates the `r.Form` and `r.PostForm` fields. If there's an error during parsing, such as invalid form data, it returns an error, which is handled in the `if` statement.

- `fmt.Fprintf(w, "POST request successful\n")`: This line writes a message to the response indicating that the POST request was successful. It uses `fmt.Fprintf` to format and write the message to the `http.ResponseWriter`.

- `favoritePackage := r.FormValue("favoritePackage")`: This line retrieves the value of the form field named "favoritePackage" from the parsed form data (`r.Form`) and assigns it to the variable `favoritePackage`.

- `fmt.Fprintf(w, "favoritePackage = %s\n", favoritePackage)`: Finally, this line writes the value of `favoritePackage` to the response. It uses `fmt.Fprintf` to format the string with the value of `favoritePackage` and write it to the `http.ResponseWriter`.

### Part 3: helloHandler Function

```go
func helloHandler(w http.ResponseWriter, r *http.Request) {
    if r.URL.Path != "/hello" {
        http.Error(w, "404 not found", http.StatusNotFound)
        return
    }
    if r.Method != "GET" {
        http.Error(w, "method is not supported", http.StatusMethodNotAllowed)
        return
    }
    fmt.Fprintf(w, "hello!")
}
```

**Explanation:**

This function, `helloHandler`, handles requests to the "/hello" route. Here's the breakdown:

- `func helloHandler(w http.ResponseWriter, r *http.Request)`: Similar to `formHandler`, this line defines a function named `helloHandler` that takes two parameters: `w`, which is an `http.ResponseWriter`, and `r`, which is an `http.Request`.

- `if r.URL.Path != "/hello" { ... }`: This line checks if the requested URL path is not "/hello". If it's not, it returns a 404 Not Found error using `http.Error()` and stops further execution of the function.

- `if r.Method != "GET" { ... }`: This line checks if the request method is not GET. If it's not, it returns a 405 Method Not Allowed error using `http.Error()` and stops further execution of the function.

- `fmt.Fprintf(w, "hello!")`: Finally, if the request path is "/hello" and the method is GET, this line writes "hello!" to the response using `fmt.Fprintf()`.

### Part 4: Main Function

```go
func main() {
    fileServer := http.FileServer(http.Dir("./static"))
    http.Handle("/", fileServer)
    http.HandleFunc("/form", formHandler)
    http.HandleFunc("/hello", helloHandler)

    fmt.Printf("starting server at port 8080\n")
    if err := http.ListenAndServe(":8080", nil); err != nil {
        log.Fatal(err)
    }
}
```

**Explanation:**

This is the main function of the program, responsible for setting up routes and starting the HTTP server. Let's break it down:

- `fileServer := http.FileServer(http.Dir("./static"))`: This line creates a file server to serve static files from the "static" directory. It uses `http.FileServer()` to create the file server.

- `http.Handle("/", fileServer)`: This line registers the file server to handle requests to the root path "/" using `http.Handle()`.

- `http.HandleFunc("/form", formHandler)`: This line registers the `formHandler` function to handle requests to the "/form" route using `http.HandleFunc()`.

- `http.HandleFunc("/hello", helloHandler)`: Similarly, this line registers the `helloHandler` function to handle requests to the "/hello" route using `http.HandleFunc()`.

- `fmt.Printf("starting server at port 8080\n")`: This line prints a message indicating that the server is starting.

- `if err := http.ListenAndServe(":8080", nil); err != nil { ... }`: Finally, this line starts the HTTP server on port 8080 using `http.ListenAndServe()`. If there's an error starting the server, it logs the error and exits the program using `log.Fatal()`.

Each part of the code is explained thoroughly, assuming no prior knowledge, to help you understand the program better. If you have any further questions, feel free to ask!

## Conclusion

This readme provides detailed instructions on setting up and running the Go server, along with explanations of the main source code file. Feel free to explore the code further and experiment with building upon it for your projects. If you have any questions or feedback, don't hesitate to reach out!
