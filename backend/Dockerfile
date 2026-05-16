# Stage 1 - Build
FROM golang:1.26-alpine AS builder

WORKDIR /app

# Copy dependency files first
COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy project files
COPY . .

# Build optimized binary
RUN CGO_ENABLED=0 GOOS=linux \
go build -ldflags="-w -s" \
-o skillpulse .

# Stage 2 - Runtime
FROM alpine:3.23

# Install certificates
RUN apk add --no-cache ca-certificates

WORKDIR /app

# Copy built binary
COPY --from=builder /app/skillpulse .

# Security: non-root user
USER nobody

EXPOSE 8080

CMD ["./skillpulse"]
