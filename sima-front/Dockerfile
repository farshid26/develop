FROM node:20.9-alpine

WORKDIR /app

COPY . .

RUN npm install -g pnpm && pnpm install

RUN pnpm run build

EXPOSE 5000

CMD ["pnpm", "run", "start"]
