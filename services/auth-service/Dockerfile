FROM node:20-alpine AS base
WORKDIR /app
COPY package*.json ./
RUN npm install --omit=dev && npm cache clean --force
COPY . .

FROM node:20-alpine AS prod
WORKDIR /app
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
COPY --from=base /app /app
ENV NODE_ENV=production
USER appuser
EXPOSE 3001
CMD ["npm", "start"]