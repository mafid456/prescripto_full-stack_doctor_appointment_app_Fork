# Stage 1: Build React app
FROM node:18 AS build   # 👈 use uppercase "AS"

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Nginx to serve build
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80   # Nginx listens on port 80
CMD ["nginx", "-g", "daemon off;"]
