FROM node:16.15.0 as node
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
EXPOSE 4100
CMD ["npm", "start"]
