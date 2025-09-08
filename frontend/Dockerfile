FROM node:20.19-alpine AS deps

WORKDIR /app
COPY package*.json .
RUN npm install

# Stage 2: Build
FROM node:20.19-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY src ./src
COPY public ./public
COPY package.json tailwind.config.js ./

RUN npm run build

# Stage 3: run
FROM node:20.19-alpine AS dev
WORKDIR /app
COPY --from=build /app/node_modules ./node_modules
COPY . .

EXPOSE 3000
CMD ["npm", "start"]

