# Build stage
FROM golang:1.19-alpine AS build-stage
WORKDIR /app

# Copy go mod and sum files
COPY go.mod go.sum ./
RUN go mod download

# Copy source code
COPY . .

# Build the application
RUN CGO_ENABLED=0 GOOS=linux go build -o auth-api .

# Final stage
FROM alpine:3.16
WORKDIR /app
COPY --from=build-stage /app/auth-api .
# Copy config if it exists
RUN mkdir -p config
COPY --from=build-stage /app/config ./config/ || true

# Run as non-root user
RUN adduser -D appuser
USER appuser

EXPOSE 8000
CMD ["./auth-api"]
