FROM node:16.13.1 as dep1

WORKDIR /app

RUN  apt-get update -y && \
     apt-get upgrade -y && \
     apt-get dist-upgrade -y && \
     apt-get -y autoremove && \
     apt-get clean

RUN apt-get install -y zip \ 
  && rm -rf /var/lib/apt/lists/*

ENV PATH /app/node_modules/.bin:$PATH

COPY package.json yarn.lock ./

RUN yarn install --silent

COPY . .

RUN yarn run generate:prod



FROM node:16.13.1 as dep2

WORKDIR /app

COPY /static-build/package.json /static-build/yarn.lock  ./

RUN yarn install --silent



FROM node:16.13.1 as build-runner

WORKDIR /app

COPY --from=dep1 /app/static-build  ./
COPY --from=dep2 /app/node_modules  ./node_modules

ENV PORT=3333

CMD ["npm", "start"]
