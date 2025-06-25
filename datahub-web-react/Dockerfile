FROM docker.nexon.com/node:20.12.0-alpine3.19 AS builder
WORKDIR /app

COPY package.json yarn.lock ./
RUN yarn config delete proxy
RUN yarn config delete https-proxy
RUN npm config rm proxy
RUN npm config rm https-proxy
RUN npm config set registry "http://registry.npmjs.org"
RUN yarn config set registry "http://registry.npmjs.org"
RUN yarn install --frozen-lockfile --network-timeout=1000000

COPY . .
ARG NPM_COMMAND=build
RUN npm run ${NPM_COMMAND}  # npm run build:dev

FROM docker.nexon.com/nginx:1.22.1-alpine AS server
COPY --from=builder /app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]