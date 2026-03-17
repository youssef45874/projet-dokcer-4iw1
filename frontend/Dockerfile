FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install --omit=dev

COPY server.cjs ./
COPY --from=build /app/dist ./dist

ENV NODE_ENV=production
EXPOSE 8080

USER node

CMD ["npm", "start"]