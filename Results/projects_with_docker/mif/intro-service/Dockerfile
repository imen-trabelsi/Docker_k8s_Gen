###
#  Created under not commercial project "Make it fine"
#
#  Copyright 2017-2021
###

FROM openjdk:11-jre
MAINTAINER Kores Mosto <koresmosto@gmail.com>

VOLUME /tmp

COPY target/intro-service-1.0-SNAPSHOT.jar /app.jar

CMD ["java", "-jar", "-Dspring.profiles.active=docker", "/app.jar"]
