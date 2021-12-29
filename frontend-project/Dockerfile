FROM node:16.13.1-slim@sha256:230c2b1d462d7dcb61cb7141b16b19aba1844a3679ee5d99e2d94a327cad85ea AS env

RUN apt update && apt install -y g++ git make python3
WORKDIR /code

# PRODUCTION BUILD
FROM env AS builder
COPY ./ /code
RUN yarn && yarn build

FROM nginx:stable-alpine@sha256:74694f2de64c44787a81f0554aa45b281e468c0c58b8665fafceda624d31e556 AS prod
COPY ./nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /code/dist /usr/share/nginx/html
EXPOSE 8000
CMD ["nginx", "-g", "daemon off;"]

# DEVELOPMENT BUILD
FROM env
CMD ["bash", "-c", "yarn && yarn start"]
