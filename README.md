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
   cd A-Web-Server-With-Golang
   ```

## Running the Server

1. **Build the Executable**: First, build the executable binary using:
   ```bash
   go build
   ```

2. **Run the Server**: Execute the built binary to start the server:
   ```bash
   go run main.go
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

### `http.FileServer(http.Dir("./static"))`:

The `http.FileServer()` function creates a handler that serves HTTP requests with the contents of the file system rooted at a given directory. Here's what happens internally:

1. It takes a parameter of type `http.FileSystem`. In this case, `http.Dir("./static")` returns an `http.Dir` object representing the directory "./static".
  
2. The `http.Dir` type implements the `http.FileSystem` interface, which allows it to serve files from the specified directory.

3. When a request is received, the server checks if the requested file exists in the specified directory. If it does, the server reads the contents of the file and sends it as the response body. If not, it returns a "404 Not Found" error.

4. Additionally, if the requested path is a directory (e.g., "/"), the server attempts to serve an index file such as "index.html" by default if it exists in that directory.

### `http.Handle("/", fileServer)`:

The `http.Handle()` function registers a handler for the given pattern (in this case, "/"). Here's what happens internally:

1. It takes two parameters: the URL pattern and the handler function or object to be registered for that pattern.

2. In this case, `"/"` is the URL pattern, which represents the root path. The `fileServer` variable, created by `http.FileServer()`, is the handler object that serves static files from the "./static" directory.

3. When a request is received with a path matching "/", the server invokes the handler function (`fileServer`) registered for that pattern.

4. As a result, the static files from the "./static" directory, including any default index file like "index.html", are served to the client.

### `http.HandleFunc("/form", formHandler)`:

This line registers the `formHandler` function to handle requests to the "/form" route using `http.HandleFunc()`. Here's what happens internally:

1. `http.HandleFunc()` is a convenience method for registering a function to handle HTTP requests for a specific route pattern.
  
2. It takes two parameters: the route pattern ("/form" in this case) and the handler function (`formHandler`).

3. When a client makes a request to the "/form" route, the server invokes the `formHandler` function to process that request.

4. Inside the `formHandler` function, the server parses form data from the request, handles any errors, and sends a response back to the client.

### `http.HandleFunc("/hello", helloHandler)`:

Similarly, this line registers the `helloHandler` function to handle requests to the "/hello" route using `http.HandleFunc()`. Here's what happens internally:

1. It associates the "/hello" route pattern with the `helloHandler` function, which is called whenever a client requests the "/hello" route.

2. The `helloHandler` function checks if the request path is "/hello" and if the request method is GET. If both conditions are met, it sends "hello!" as the response to the client.

### `fmt.Printf("starting server at port 8080\n")`:

This line prints a message to the console indicating that the server is starting at port 8080. It's a helpful message for developers to know that the server is up and running.

### `if err := http.ListenAndServe(":8080", nil); err != nil { ... }`:

Finally, this line starts the HTTP server on port 8080 using `http.ListenAndServe()`. Here's what happens internally:

1. `http.ListenAndServe()` starts an HTTP server with the specified address (":8080" in this case) and a given handler.

2. The second argument is `nil`, indicating that the default router provided by `http.DefaultServeMux` should be used.

3. If an error occurs while starting the server (for example, if the specified port is already in use), `http.ListenAndServe()` returns an error. In that case, the `if` statement logs the error using `log.Fatal()` and exits the program.

By registering handlers for specific routes and starting the server, this main function ensures that incoming HTTP requests are properly routed and handled by the appropriate functions.

## Conclusion

This readme provides detailed instructions on setting up and running the Go server, along with explanations of the main source code file. Feel free to explore the code further and experiment with building upon it for your projects. If you have any questions or feedback, don't hesitate to reach out!
