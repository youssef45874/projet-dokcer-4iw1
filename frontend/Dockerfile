FROM node:20-alpine AS build

WORKDIR /app
COPY package*.json ./
RUN npm install --no-audit --no-fund && npm cache clean --force

COPY . .
RUN npm run build

FROM node:20-alpine

WORKDIR /app
COPY package*.json ./
RUN npm install --omit=dev --no-audit --no-fund && npm cache clean --force

COPY server.cjs ./
COPY --from=build /app/dist ./dist

ENV NODE_ENV=production
EXPOSE 8080

USER node

CMD ["npm", "start"]