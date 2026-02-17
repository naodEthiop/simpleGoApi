# Book Inventory API

This is a simple RESTful API written in Go using the [Gin](https://github.com/gin-gonic/gin) web framework. The project maintains an in-memory list of books and allows you to perform basic operations such as listing books, retrieving a single book by ID, adding a new book, checking out a book, and returning a book.

## Prerequisites

- Go 1.20 or later installed on your system
- `go` command available in your PATH

## Getting Started

1. Clone or download the repository.
2. Navigate to the project directory:
   ```bash
   cd c:\Users\NaodEthiop\Desktop\Projects\APi-tutorial
   ```
3. Initialize Go modules (if not already done):
   ```bash
   go mod tidy
   ```
4. Run the server:
   ```bash
   go run main.go
   ```
   The API will listen on `localhost:8080`.

## Endpoints

| Method | URL            | Description                                       | Request Body                                    | Response      |
|--------|----------------|---------------------------------------------------|-------------------------------------------------|---------------|
| GET    | `/books`       | Get a list of all books                           | None                                            | JSON array    |
| GET    | `/books/:id`   | Get details of a single book by its ID            | None                                            | JSON object   |
| POST   | `/books`       | Create a new book entry                           | JSON object with `id`, `title`, `author`, `quantity` | JSON object (created book) |
| PATCH  | `/checkout`    | Decrement quantity for a book specified by query param | Query `id` (e.g. `/checkout?id=2`)          | JSON object   |
| PATCH  | `/return`      | Increment quantity for a book specified by query param | Query `id` (e.g. `/return?id=2`)            | JSON object   |

## Data Model

Books are represented using the following Go struct:

```go
type book struct {
    ID       string `json:"id"`
    Title    string `json:"title"`
    Author   string `json:"author"`
    Quantity int    `json:"quantity"`
}
```

The application stores a slice of `book` values in-memory, initialized with three example books.

## Code Walkthrough

* **Imports**
  * `github.com/gin-gonic/gin` – Gin framework for building HTTP handlers and routes.
  * `net/http` – Standard library constants for HTTP status codes.
  * `errors` – For creating error values.

* **Handlers**
  * `getBooks` – returns all books as indented JSON with status 200.
  * `bookById` – retrieves `:id` from the path, looks up a book using `getBookById`, returns 404 if not found.
  * `createBook` – binds a JSON request body to a new `book`, appends it to the slice, responds with status 201.
  * `checkoutBook` & `returnBook` – read an `id` query parameter, adjust the `Quantity` field up or down and return the updated book. They also handle error cases such as missing parameter or book not found.

* **Utility Functions**
  * `getBookById(id string)` loops through the `books` slice and returns a pointer to the matching book or an error if none is found.

* **main**
  * Creates a default Gin router, registers each route with its handler, and starts the server on `localhost:8080`.

## What You've Done

You've built a basic Go server that:

1. Defines a `book` type and stores sample data in a slice.
2. Exposes HTTP endpoints using Gin to interact with this data.
3. Handles JSON serialization/deserialization automatically through Gin's binding features.
4. Implements simple business logic (checkout and return operations) with error handling.

This kind of API is a foundation for a larger application; you could later connect it to a database, add validation, authentication, and deploy it as a service.

## Next Steps

- Persist books in a database (PostgreSQL, SQLite, etc.) instead of in-memory slice.
- Add input validation and return more informative errors.
- Include tests using Go's `testing` package.
- Consider using environment configuration and logging.

---

Feel free to ask if you'd like help extending or refactoring this API!