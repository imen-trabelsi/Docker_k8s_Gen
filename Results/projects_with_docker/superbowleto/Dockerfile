FROM 697525377503.dkr.ecr.us-east-1.amazonaws.com/pay-docker-base-images:pagarme-node8.9

ADD package.json /superbowleto/package.json
ADD package-lock.json /superbowleto/package-lock.json
WORKDIR /superbowleto

RUN npm install

EXPOSE 3000
