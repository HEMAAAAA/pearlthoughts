# Build stage
FROM golang:1.20-alpine AS builder

# Set working directory
WORKDIR /app

# Install git and other tools
RUN apk update && apk add --no-cache git

# Set Go environment (network-safe)
ENV GOPROXY=https://proxy.golang.org,direct
ENV GODEBUG=netdns=go,dns+ipv4only

# Copy go mod files
COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy source code
COPY main.go .

# Build the application with optimizations
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o shipping-service main.go

# Final stage
FROM alpine:3.18

# Add CA certificates for HTTPS
RUN apk --no-cache add ca-certificates

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set working directory
WORKDIR /app

# Copy binary from builder stage
COPY --from=builder /app/shipping-service .

# Use non-root user
USER appuser

# Expose API port
EXPOSE 8088

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -qO- http://localhost:8088/shipping-explanation || exit 1

# Run the binary
CMD ["./shipping-service"]
