ARG NGINX_IMAGE=nginx:1.27-alpine

FROM node:22-alpine AS build
WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
ARG VITE_PYTHON_ONLY=false
ENV VITE_PYTHON_ONLY=${VITE_PYTHON_ONLY}
RUN npm run build

FROM ${NGINX_IMAGE}

COPY docker/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html
COPY docker/entrypoint.sh /entrypoint.sh

RUN chmod +x /entrypoint.sh

EXPOSE 80
ENTRYPOINT ["/entrypoint.sh"]
