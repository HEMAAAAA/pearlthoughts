# Use the official Node.js image as the base image
ARG BASE_IMAGE=node:14-alpine
FROM ${BASE_IMAGE} AS builder

# Set the working directory inside the container
WORKDIR /app

# ---------- Build server ----------
# Copy server package files first (better caching)
COPY server/package*.json ./server/

# Install server dependencies
WORKDIR /app/server
RUN npm install --production

# ---------- Build client ----------
# Copy client package files first (better caching)
WORKDIR /app
COPY client/package*.json ./client/

# Install client dependencies
WORKDIR /app/client
RUN npm install --production

# ---------- Copy source code ----------
# Now copy the rest of the application code
WORKDIR /app
COPY server ./server
COPY client ./client

# ---------- Stage 2: Final Stage ----------
FROM ${BASE_IMAGE} AS final

# Create non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Set working directory
WORKDIR /app

# Copy only what's needed from builder stage
COPY --from=builder --chown=appuser:appgroup /app/client /app/client
COPY --from=builder --chown=appuser:appgroup /app/server /app/server

# Set permissions
USER appuser

# Change to the server directory
WORKDIR /app/server

# Expose the port that the server listens on
EXPOSE 4000

# Run the Node.js server when the container starts
CMD ["node", "server.js"]