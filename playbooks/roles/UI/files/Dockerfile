# Stage 1: Build the application
FROM node:22-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application source code
COPY . .

# Build the application
# The --openssl-legacy-provider might be needed for older dependencies
RUN npm run build

# Stage 2: Production image
FROM node:22-alpine

WORKDIR /app

# Copy built assets from the builder stage
COPY --from=builder /app/build ./build
COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./package.json
COPY --from=builder /app/node_modules ./node_modules

# Copy the .env file that Ansible will provide
COPY .env ./.env

# Expose the port the app runs on (assuming 3000 for Next.js)
EXPOSE 3000

# Command to start the application
CMD ["npm", "start"]