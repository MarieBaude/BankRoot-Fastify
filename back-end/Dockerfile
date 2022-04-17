FROM node:12-alpine AS build
WORKDIR /app 
COPY package*.json ./
COPY prisma ./prisma/
RUN npm ci
COPY . ./
RUN npm run build:ts

FROM node:alpine
WORKDIR /app 
COPY --from=build /app/dist /app/dist
COPY --from=build /app/node_modules /app/node_modules
COPY --from=build /app/prisma /app/prisma

RUN ls && cd dist && ls
RUN npx prisma generate 
#RUN npx prisma migrate deploy

EXPOSE 3000

CMD ["node", "dist/app.js"]
