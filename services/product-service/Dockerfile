FROM node:20-alpine AS source

WORKDIR /app
COPY package*.json ./
COPY src ./src

FROM node:20-alpine

WORKDIR /app
COPY package*.json ./
RUN npm install --omit=dev --no-audit --no-fund && npm cache clean --force

COPY --from=source /app/src ./src

ENV NODE_ENV=production
EXPOSE 3000

USER node

CMD ["npm", "start"]