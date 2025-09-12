# Use Node.js LTS
FROM node:18

WORKDIR /app
COPY package*.json ./
# Install only production dependencies
RUN npm ci --only=production

COPY . .

EXPOSE 5000
CMD ["node", "server.js"]   # change to app.js if that's your entry


