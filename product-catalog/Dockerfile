# Build stage
FROM node:14-alpine AS builder

# Set the working directory
WORKDIR /app

# Copy package files for dependency installation
COPY package*.json ./

# Install dependencies with exact versions for production
RUN npm install --production

# Copy application code
COPY . .

# Runtime stage
FROM node:14-alpine AS final

# Create a non-root user
RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup

# Set working directory
WORKDIR /app

# Copy node_modules and app files from builder
COPY --from=builder --chown=appuser:appgroup /app/node_modules ./node_modules
COPY --from=builder --chown=appuser:appgroup /app/package.json ./package.json
COPY --from=builder --chown=appuser:appgroup /app/index.js ./index.js
COPY --from=builder --chown=appuser:appgroup /app/products.json ./products.json

# Set proper permissions
USER appuser

# Expose the application port
EXPOSE 3001

# Define healthcheck
  CMD curl -f http://localhost:3001/health || exit 1
  CMD wget -q -O - http://localhost:3001/health || exit 1

# Run the application
CMD ["node", "index.js"]