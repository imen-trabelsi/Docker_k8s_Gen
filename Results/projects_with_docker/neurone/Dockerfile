# NEURONE Dockerfile

# https://medium.com/@isohaze/how-to-dockerize-a-meteor-1-4-app-120a34089ddb
# https://projectricochet.com/blog/production-meteor-and-node-using-docker-part-i

FROM phusion/passenger-customizable:2.0.0

# Contact the maintainer in case of problems
MAINTAINER Daniel Gacitua <daniel.gacitua@usach.cl>

# Replace shell with bash so we can source files
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# Install basic dependencies
RUN apt-get -qq update \
  && apt-get -qq --no-install-recommends install wget curl ca-certificates libarchive-tools unzip make gcc python python-dev

# Install Node.js through nvm
# https://gist.github.com/remarkablemark/aacf14c29b3f01d6900d13137b21db3a
ENV NVM_DIR /nvm
ENV NODE_VERSION 8.17.0
RUN mkdir -p $NVM_DIR \
  && curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
RUN source $NVM_DIR/nvm.sh \
  && nvm install $NODE_VERSION \
  && nvm alias default $NODE_VERSION \
  && nvm use default
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH
RUN node -v && npm -v

# Set user
ARG username=user
ARG userid=9001
ENV LOCAL_USER_NAME $username
ENV LOCAL_USER_ID $userid
ADD ./.deploy/docker/createUser.sh /tmp/createUser.sh
RUN chmod +x /tmp/createUser.sh && ./tmp/createUser.sh

# Set working directory
RUN mkdir -p /app && chown -R $username:$username /app
WORKDIR /app
ENV HOME /home/$username

# Add all files
ADD . ./src
RUN chown -R $username:$username ./src

# Copy config files
RUN cp ./src/.deploy/docker/neurone.conf /etc/nginx/sites-enabled/neurone.conf \
  && cp ./src/.deploy/docker/meteor-env.conf /etc/nginx/main.d/meteor-env.conf \
  && cp ./src/.deploy/docker/meteorBuild.sh ./meteorBuild.sh \
  && cp ./src/.deploy/docker/fixPermissions.sh ./fixPermissions.sh \
  && chmod +x ./meteorBuild.sh \
  && chmod +x ./fixPermissions.sh

# Set Meteor Framework location as environment variable
ENV PATH $PATH:$HOME/.meteor

# Set Build Memory Limit
ENV TOOL_NODE_FLAGS --optimize_for_size --max_old_space_size=1536 --gc_interval=100

# Handle Let's Encrypt expired certificate
# https://docs.meteor.com/expired-certificate.html
ENV NODE_TLS_REJECT_UNAUTHORIZED 0

# Installation and packaging script
RUN /sbin/setuser $username ./meteorBuild.sh

# Create NEURONE assets directory
RUN mkdir -p /assets
RUN ./fixPermissions.sh /assets $username

# Set internal Meteor environment variables
ENV NEURONE_ASSET_PATH /assets
ENV HTTP_FORWARDED_COUNT 1

# Enable Nginx and Passenger
RUN rm -f /etc/nginx/sites-enabled/default \
  && rm -f /etc/service/nginx/down

# Set ports, data volumes and commands
EXPOSE 80
VOLUME ["/assets"]
CMD ["/sbin/my_init"]

# Clean up APT when done
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*