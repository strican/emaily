FROM node:21.7.1-alpine as node

##################################################
# Build Stage
##################################################
FROM node as Build

WORKDIR /usr/src/app

COPY .yarn ./.yarn
COPY .yarnrc ./

COPY package.json yarn.lock ./
COPY server/package.json ./server/

RUN yarn install --frozen-lockfile

COPY server ./server

RUN yarn workspace server build

##################################################
# Final Stage
##################################################
FROM node

EXPOSE 5000

WORKDIR /usr/src/app

COPY .yarn ./.yarn
COPY .yarnrc ./

COPY package.json yarn.lock ./
COPY server/package.json ./server/

RUN yarn install --frozen-lockfile

COPY --from=Build /usr/src/app/server/build ./build

ENTRYPOINT ["node"]

# Command to run the application
CMD ["./build/index.js"]