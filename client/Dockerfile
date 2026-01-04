 
# -----------------------------
# Stage 1: Build the frontend
# -----------------------------
    FROM node:18-alpine AS builder

    # Set working directory
    WORKDIR /build
    ENV NODE_OPTIONS="--max_old_space_size=4096"
    # Copy package files and install dependencies
    COPY package.json package-lock.json ./
    RUN npm ci
    
    # Copy the rest of the source code
    COPY . .
    
    # Build the project (Vite outputs to /build/dist by default)
    RUN npm run build
    
    # -----------------------------
    # Stage 2: Serve with Nginx
    # -----------------------------
    FROM nginx:alpine
    
    # Copy built files from builder stage to Nginx web root
    COPY --from=builder /build/dist /usr/share/nginx/html
    
    # Expose port 80
    EXPOSE 80
    
    # Run Nginx in the foreground
    CMD ["nginx", "-g", "daemon off;"]