FROM node:alpine AS builder
WORKDIR /app
COPY ./package.json ./
RUN npm install
COPY . . 
RUN npm run build

FROM nginx
EXPOSE 3000
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
# Copy over the build assets from builder phase
COPY --from=builder /app/build /usr/share/nginx/html