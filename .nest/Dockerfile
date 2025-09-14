# Nest.js 用 Dockerfile
FROM node:20-alpine

WORKDIR /app

COPY . .

RUN npm install

# ビルド
RUN npm run build

EXPOSE 3000
CMD ["npm", "run", "start:dev"]
