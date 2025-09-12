# Use Node.js LTS
FROM node:18

# Set working directory
WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm ci --only=production

# Copy source code
COPY . .

# Expose backend port
EXPOSE 5000

# Run backend
CMD ["node", "server.js"]   # or app.js depending on your entry file

