# ベースイメージ
FROM node:20.16.0-alpine AS builder

# 作業ディレクトリの設定
WORKDIR /app

# package.jsonとpackage-lock.jsonをコピー
COPY package*.json ./

# 依存関係をインストール
RUN npm install

# アプリケーションのソースコードをコピー
COPY . .

# TypeScriptをJavaScriptにコンパイル
RUN npm run build

# ベースイメージを変更して、本稼働用の環境を作成
# production : 必要最小限の依存関係のみをインストール
FROM node:20.16.0-alpine AS production

# 作業ディレクトリの設定
WORKDIR /app

# 依存パッケージのみを再度インストール
COPY package*.json ./
RUN npm install --production

# ビルド済みのJavaScriptファイルをコピー
COPY --from=builder /app/dist ./dist

# エクスポートするポート番号の指定
EXPOSE 3001

# コンテナを起動したときに実行するコマンド
CMD ["npm", "run", "start"]