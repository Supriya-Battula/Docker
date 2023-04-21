# Docker Workshop-3 Activities (21/APR/2023) - Khaja Sir   
------------------------------------------------------------------------ 

* 1. Create a multi-stage docker file to build  
    a. nop commerce  
  In docker playdroung , login into terminal
  $ vi docker file
  (Docker File)
    ```
    FROM ubuntu:22.04 As builder
    RUN apt update && apt install unzip -y
    ADD https://github.com/nopSolutions/nopCommerce/releases/download/release-4.40.2/nopCommerce_4.40.2_NoSource_linux_x64.zip /nop/nopCommerce_4.40.2_NoSource_linux_x64.zip
    RUN cd nop && unzip nopCommerce_4.40.2_NoSource_linux_x64.zip && rm nopCommerce_4.40.2_NoSource_linux_x64.zip
    FROM mcr.microsoft.com/dotnet/sdk:7.0
    LABEL author="Supriya" organization="qt" project="learning"
    COPY --from=builder /nop /nop-bin
    WORKDIR /nop-bin
    EXPOSE 5000
    CMD [ "dotnet", "Nop.Web.dll", "--urls", "http://0.0.0.0:5000" ]
    ```
    $ docker image build -t nop .
    $ docker container run -d --name nani -P nop
    $ docker container ls
    ![preview](images/dockerimage45.jpg)
    give port and refresh the page until its work
    Results
    ![preview](images/dockerimage44.jpg)

  b. spring petclinic 
  $ vi docker file
  (Docker File)
    ```
    FROM alpine/git AS vcs
    RUN cd / && git clone https://github.com/spring-projects/spring-petclinic.git && \
    pwd && ls /spring-petclinic
    FROM maven:3-amazoncorretto-17 AS builder
    COPY --from=vcs /spring-petclinic /spring-petclinic
    RUN ls /spring-petclinic
    RUN cd /spring-petclinic && mvn package
    FROM amazoncorretto:17-alpine-jdk
    LABEL author="Prakash Reddy" organization="qt" project="learning"
    EXPOSE 8080
    ARG HOME_DIR=/spc
    WORKDIR ${HOME_DIR}
    COPY --from=builder /spring-petclinic/target/spring-*.jar ${HOME_DIR}/spring-petclinic.jar
    EXPOSE 8080
    CMD ["java", "-jar", "spring-petclinic.jar"]
    ```
    after buil the docker file 
    ```
    $ docker image build -t nop .
    $ docker container run -d --name nani -P nop
    $ docker container ls
    ```
    ![preview](images/dockerimage47.jpg)
    Results
    ![preview](images/dockerimage46.jpg)

    c. student courses register

