# Greenlight API

A JSON API for managing movie information, built while working through
*Let's Go Further* by Alex Edwards.

> **Status:** work in progress. Currently through Chapter 3 (Sending JSON responses).

## Requirements

- Go 1.27 or newer (the project uses `encoding/json/v2`)

## Running the API

```bash
go run ./cmd/api
```

The server starts on port `4000` in the `development` environment by default.

### Command-line flags

| Flag    | Default       | Description                                        |
|---------|---------------|----------------------------------------------------|
| `-port` | `4000`        | Port the API server listens on                     |
| `-env`  | `development` | Environment (`development`, `staging`, `production`) |

Example:

```bash
go run ./cmd/api -port=8080 -env=staging
```

## Endpoints

| Method | Path              | Description                                   |
|--------|-------------------|-----------------------------------------------|
| GET    | `/v1/healthcheck` | Returns the API status, environment and version |
| POST   | `/v1/movies`      | Create a new movie *(placeholder for now)*    |
| GET    | `/v1/movies/:id`  | Show a movie by ID *(returns sample data for now)* |

### Examples

```bash
curl -i localhost:4000/v1/healthcheck
```

```json
{"environment":"development","status":"available","version":"1.0.0"}
```

```bash
curl -i localhost:4000/v1/movies/123
```

```json
{"id":123,"title":"Casablanca","year":0,"genres":["romance","drama","war"],"version":1}
```

An `id` that is not a positive integer currently returns a plain-text `404`.
This will become a JSON error response later in the book.

## Project structure

```
cmd/api/           Application code: main, routes, handlers and helpers
internal/models/   Data types such as Movie
migrations/        Database migrations (not used yet)
remote/            Deployment files (not used yet)
bin/               Compiled binaries
```

## Dependencies

- [`julienschmidt/httprouter`](https://github.com/julienschmidt/httprouter) for routing
