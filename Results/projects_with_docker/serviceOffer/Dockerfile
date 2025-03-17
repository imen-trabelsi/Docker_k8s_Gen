FROM node:latest AS baseline
WORKDIR /usr/src/app/serviceoffer
COPY ./package.json /usr/src/app/serviceoffer

FROM baseline AS tester
RUN npm install
COPY . /usr/src/app/serviceoffer
RUN npm run lint
RUN npm run test

FROM baseline AS production
#RUN npm install package-lock.json -- production
RUN npm install
COPY ./src/ /usr/src/app/serviceoffer/src/
EXPOSE 4000
CMD ["npm", "run", "prod"]
