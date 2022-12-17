FROM gradle:jdk12 AS build

# tools
RUN apt-get update && \
    apt-get install -y curl && \
    apt-get install -y gcc && \
    apt-get install -y make

RUN rm -rf /configure

# copy project files
COPY ml-configure/build.gradle /configure/
COPY ml-configure/src /configure/src
COPY ml-configure/gradle.properties /configure/

# wait script
COPY ml-configure/wait-for-ml.sh /configure/wait-for-ml.sh
RUN chmod +x /configure/wait-for-ml.sh
WORKDIR /configure
CMD /configure/wait-for-ml.sh $MLHOST 8001 $MLUSER $MLPWD && \
	gradle -PmlHost=$MLHOST -PmlUsername=$MLUSER -PmlPassword=$MLPWD mlDeployServers