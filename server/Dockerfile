
# Stage 1: Build the project
FROM node:18-alpine AS builder

WORKDIR /build

# Copy only package files first (to leverage cache)
COPY package.json package-lock.json ./

RUN npm ci

# Copy the rest of the source code
COPY . .

# Build the project
RUN npm run build
RUN npm prune --omit=dev

# Stage 2: Runner image
FROM node:18-alpine AS runner

WORKDIR /app

# Copy dist, public, node_modules, and package.json separately
COPY --from=builder /build/dist ./dist/
COPY --from=builder /build/public ./public/
COPY --from=builder /build/node_modules ./node_modules/
COPY --from=builder /build/package.json ./           
COPY --from=builder /build/package-lock.json ./

EXPOSE 3000

CMD ["node", "dist/server.js"]
