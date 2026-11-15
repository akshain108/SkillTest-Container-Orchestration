
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install --omit=dev

COPY app.js .

ENV NODE_ENV=production
ENV PORT=3002

EXPOSE 3002

CMD ["node", "app.js"]
