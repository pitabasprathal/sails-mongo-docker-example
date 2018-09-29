FROM node

ARG HTTP_PROXY=""
ENV http_proxy $HTTP_PROXY
ENV https_proxy $HTTP_PROXY

RUN apt-get update -qq && apt-get install -y build-essential
RUN npm install -g npm
# use changes to package.json to force Docker not to use the cache
# when we change our application's nodejs dependencies:
ADD package.json /tmp/package.json
RUN cd /tmp && npm install -g sails && npm install

ENV DIR /usr/src/sails-app
RUN mkdir -p $DIR && cp -a /tmp/node_modules $DIR

# From here we load our application's code in, therefore the previous docker
# "layer" thats been cached will be used if possible
WORKDIR $DIR
ADD . $DIR

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
