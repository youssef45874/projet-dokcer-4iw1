FROM node:20-alpine AS source

WORKDIR /app
COPY . .

FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install --omit=dev

COPY --from=source /app/src ./src

ENV NODE_ENV=production
EXPOSE 3002

USER node

CMD ["npm", "start"]