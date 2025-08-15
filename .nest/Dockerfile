# Nest.js 用 Dockerfile
FROM node:20-alpine

WORKDIR /app

# package.json と lock ファイルだけ先にコピー
COPY src/package*.json ./

# 依存関係インストール
RUN npm install

# 残りのソースをコピー
COPY src/ .

# ビルド
RUN npm run build

EXPOSE 3000
CMD ["npm", "run", "start:dev"]
