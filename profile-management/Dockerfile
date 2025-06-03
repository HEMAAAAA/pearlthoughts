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

ENV NODE_ENV=production

# Create a non-root user
RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup

# Set working directory
WORKDIR /app

# Copy node_modules and app files from builder
COPY --from=builder --chown=appuser:appgroup /app/node_modules ./node_modules
COPY --from=builder --chown=appuser:appgroup /app/package.json ./package.json
COPY --from=builder --chown=appuser:appgroup /app/auth_api.js ./auth_api.js

# Set proper permissions
USER appuser

# Expose the port that the API listens on
EXPOSE 3003

# Run the Node.js application when the container starts
CMD ["node", "auth_api.js"]