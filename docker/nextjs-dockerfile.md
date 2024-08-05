# Next Js DockerFile

Berikut adalah contoh penulisan **DockerFile** untuk project next js.

```DockerFile
FROM node:18-alpine AS base
WORKDIR /app

COPY package.json ./
RUN apk add --no-cache bash curl git && \
    curl https://pyenv.run | bash && \
    apk add --no-cache make build-base libffi-dev openssl-dev zlib-dev bzip2-dev readline-dev sqlite-dev && \
    export PATH="/root/.pyenv/bin:$PATH" && \
    eval "$(pyenv init -)" && \
    eval "$(pyenv virtualenv-init -)" && \
    pyenv install 3.9.10 && \
    pyenv global 3.9.10 && \
    ln -s "$(pyenv which python3)" /usr/local/bin/python

FROM base AS builder
WORKDIR /app
RUN npm install
RUN npm ci --no-audit;
COPY . .
RUN npm run build; 


FROM base as runner
WORKDIR /app
COPY --from=builder /app/next.config.js .
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static
COPY --from=builder /app/setup.sh ./setup.sh

ENV NODE_ENV development
EXPOSE 3000

ENV PORT 3000
RUN chmod +x ./setup.sh 
CMD ./setup.sh && node server.js
```