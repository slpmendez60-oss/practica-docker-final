# ─────────────────────────────────────────────
# ETAPA: dev
# ─────────────────────────────────────────────
FROM node:19-alpine3.15 AS dev
WORKDIR /app

COPY package.json yarn.lock ./
RUN yarn install

CMD [ "yarn","start:dev" ]


# ─────────────────────────────────────────────
# ETAPA: dev-deps
# ─────────────────────────────────────────────
FROM node:19-alpine3.15 AS dev-deps
WORKDIR /app

COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile


# ─────────────────────────────────────────────
# ETAPA: builder
# ─────────────────────────────────────────────
FROM node:19-alpine3.15 AS builder
WORKDIR /app

COPY --from=dev-deps /app/node_modules ./node_modules
COPY . .

RUN yarn build


# ─────────────────────────────────────────────
# ETAPA: prod-deps
# ─────────────────────────────────────────────
FROM node:19-alpine3.15 AS prod-deps
WORKDIR /app

COPY package.json yarn.lock ./
RUN yarn install --production --frozen-lockfile


# ─────────────────────────────────────────────
# ETAPA: prod
# ─────────────────────────────────────────────
FROM node:19-alpine3.15 AS prod

EXPOSE 3000
WORKDIR /app

ENV APP_VERSION=${APP_VERSION}

COPY --from=prod-deps /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist

CMD [ "node","dist/main.js" ]
