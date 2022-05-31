FROM node:18-alpine3.14
RUN mkdir -p /usr/src/frontendApp
WORKDIR /usr/src/frontendApp
COPY / /usr/src/frontendApp
RUN npm ci
EXPOSE 3000
CMD ["npm", "start"]