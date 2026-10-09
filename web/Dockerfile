# syntax=docker/dockerfile:1

# ---- Stage 1: build the static site (npm run build -> dist/) ----
FROM node:20-alpine AS build
WORKDIR /app
COPY package.json build.js ./
COPY public ./public
RUN npm run build

# ---- Stage 2: serve dist/ with nginx (runs as non-root, uid 101) ----
FROM nginxinc/nginx-unprivileged:1.27-alpine AS runtime
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80

HEALTHCHECK --interval=15s --timeout=3s --start-period=5s --retries=3 \
  CMD wget -q --spider http://127.0.0.1:80/ || exit 1
