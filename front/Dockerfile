#dockerfile to generate frontend image
#step 1 : compile angular source code
FROM node:10-alpine AS build-step
WORKDIR /app
COPY package.json /app
RUN npm install
COPY . /app
RUN npm run build

#step 2 : copy compilation's result to bunkerized-nginx www dir
FROM bunkerity/bunkerized-nginx AS prod
COPY --from=build-step /app/dist/ /www