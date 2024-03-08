# Go Server

This repository contains a basic Go web server that serves static files and handles form submissions. Below, you'll find detailed explanations of each step involved in setting up and running this server, along with an explanation of the main `main.go` file.

## Setup

1. **Install Golang**: Ensure that you have Golang installed on your system. You can find installation instructions for various platforms [here](https://golang.org/doc/install).

2. **Create Project Directory**: Navigate to the directory where you want to create your Go project.

3. **Clone Repository**: Clone this repository into your project directory using the following command:
   ```bash
   git clone <repository_url>
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

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

// formHandler handles form submissions.
func formHandler(w http.ResponseWriter, r *http.Request) {
    // Parse the form data from the request.
    if err := r.ParseForm(); err != nil {
        fmt.Fprintf(w, "ParseForm() err: %v", err)
        return
    }
    // Print a message indicating successful POST request.
    fmt.Fprintf(w, "POST request successful\n")
    // Retrieve the value of the form field named "favoritePackage".
    favoritePackage := r.FormValue("favoritePackage")
    // Print the value of the form field to the response.
    fmt.Fprintf(w, "favoritePackage = %s\n", favoritePackage)
}

// helloHandler handles requests to the "/hello" route.
func helloHandler(w http.ResponseWriter, r *http.Request) {
    // Check if the request path is not "/hello".
    if r.URL.Path != "/hello" {
        // Return a 404 error if the path is not "/hello".
        http.Error(w, "404 not found", http.StatusNotFound)
        return
    }
    // Check if the request method is not GET.
    if r.Method != "GET" {
        // Return a 405 error if the method is not GET.
        http.Error(w, "method is not supported", http.StatusMethodNotAllowed)
        return
    }
    // Print "hello!" to the response.
    fmt.Fprintf(w, "hello!")
}

func main() {
    // Create a file server to serve static files from the "static" directory.
    fileServer := http.FileServer(http.Dir("./static"))
    // Handle requests to the root path ("/") by serving static files.
    http.Handle("/", fileServer)
    // Handle requests to the "/form" route by calling the formHandler function.
    http.HandleFunc("/form", formHandler)
    // Handle requests to the "/hello" route by calling the helloHandler function.
    http.HandleFunc("/hello", helloHandler)

    // Print a message indicating that the server is starting.
    fmt.Printf("starting server at port 8080\n")
    // Start the HTTP server on port 8080.
    if err := http.ListenAndServe(":8080", nil); err != nil {
        // Log any errors that occur while starting the server.
        log.Fatal(err)
    }
}
```

### Explanation

- **Imports**: Import necessary packages (`fmt`, `log`, `net/http`) required for the server.
- **`formHandler` Function**: Handles form submissions by parsing the form data from the request and printing it to the response.
- **`helloHandler` Function**: Handles requests to the "/hello" route by printing "hello!" to the response.
- **`main` Function**: The entry point of the program. It sets up routes for serving static files and handling form submissions, then starts the HTTP server on port 8080.

## Conclusion

This readme provides detailed instructions on setting up and running the Go server, along with explanations of the main source code file. Feel free to explore the code further and experiment with building upon it for your projects. If you have any questions or feedback, don't hesitate to reach out!
