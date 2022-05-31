FROM node:18-alpine3.14
RUN mkdir -p /usr/src/backendApp
WORKDIR /usr/src/backendApp
COPY * .
RUN npm ci
EXPOSE 8080
CMD ["npm", "start"]