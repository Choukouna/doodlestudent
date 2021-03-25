#author : Kounadi OUATTARA
#builder ce dockerfile : docker build -t img_web_app:1 .
#launch container : docker run -p 8080:8080 img_web_app:1

FROM ubuntu:20.04

RUN apt-get update &&\
    apt-get install -y openjdk-8-jdk &&\
    apt-get install -y maven &&\
    apt-get install -yf libpng16-16 &&\
    apt-get install -yf libdc1394-22 &&\
    apt-get install -y git

WORKDIR /usr/src/

RUN git clone https://github.com/barais/TPDockerSampleApp

RUN mvn install:install-file -Dfile=./TPDockerSampleApp/lib/opencv-3410.jar \
     -DgroupId=org.opencv  -DartifactId=opencv -Dversion=3.4.10 -Dpackaging=jar

WORKDIR /usr/src/TPDockerSampleApp
RUN mvn package
EXPOSE 8080
CMD ["java", "-Djava.library.path=lib/ubuntuupperthan18/", "-jar", "/usr/src/TPDockerSampleApp/target/fatjar-0.0.1-SNAPSHOT.jar"]
