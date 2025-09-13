# Use Node.js LTS for backend API
FROM node:18

WORKDIR /app

# Install production dependencies
COPY package*.json ./
RUN npm ci --only=production

# Copy source code
COPY . .

EXPOSE 5000
CMD ["node", "server.js"]   # change to app.js if that's your entrypoint


