# Extracted from http://blog.bejanalex.com/2017/03/running-mongodb-in-a-docker-container-with-authentication/

FROM mongo:3.4
 
MAINTAINER Daniel Gacitua <daniel.gacitua@usach.cl>
 
ENV AUTH yes
ENV STORAGE_ENGINE wiredTiger
ENV JOURNALING yes
 
ADD mongodb_run.sh /run.sh
ADD mongodb_password.sh /set_mongodb_password.sh
 
CMD ["/run.sh"]
