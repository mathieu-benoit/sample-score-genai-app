# Build stage
FROM golang:1.25-alpine AS builder

WORKDIR /app

# Copy go.mod first
COPY go.mod ./

# Initialize the module and download dependencies
RUN go mod tidy && go mod download

# Copy the source code
COPY . .

# Build the application
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

# Final stage
FROM gcr.io/distroless/static:nonroot

WORKDIR /app

# Copy the binary from builder stage
COPY --from=builder /app/main .

# Copy static files and templates
COPY static/ ./static/
COPY templates/ ./templates/

# Expose port 8080
EXPOSE 8080

# Run the application
CMD ["./main"]
