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
      $ vi dockerfile
      ```
      FROM alpine/git AS vcs
      RUN cd / && git clone https://github.com/DevProjectsForDevOps/StudentCoursesRestAPI.git && \
      pwd && ls /StudentCoursesRestAPI
      FROM python:3.8.3-alpine As Builder
      LABEL author="Supriya" organization="qt" project="learning"
      COPY --from=vcs /StudentCoursesRestAPI /StudentCoursesRestAPI
      ARG DIRECTORY=StudentCourses
      RUN cd / StudentCoursesRestAPI cp requirements.txt /StudentCourses
      ADD . ${DIRECTORY}
      EXPOSE 8080
      WORKDIR StudentCoursesRestAPI
      RUN pip install --upgrade pip
      RUN pip install -r requirements.txt
      ENTRYPOINT ["python", "app.py"]
      ```
    ```
    $ docker image build -t scr .
    $ docker container run -d --name nani -P scr
    $ docker container ls
    ```
    ![preview](images/dockerimage48.jpg)
    open the port 
    ![preview](images/dockerimage49.jpg)
* 2. Push these images to  
      a. Azure ACR
      create a virtual machine and login into a terminal
    * install azure-cli
     * $ sudo apt update
     * $ curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
    * $ az --version    (to check version)
    * Now install docker 
    *  $ sudo apt update
    *  $ curl -fsSL https://test.docker.com -o test-docker.sh
    *  $ sh test-docker.sh
    *  $ docker --version (to check version)
      ![preview](images/dockerimage1.jpg)
    * $ docker info     (it will not work)
    * $ whoami  (to check present user name)
    * $ sudo usermod -aG docker azureuser
      ![preview](images/dockerimage2.jpg)
    *  $ exit
    * then again login to the machine
    *  $ docker info (docker will run)
    *  login to azure-cli
    *  $ az login
    *  now create ACR (search container registries)
    *  ![preview](images/dockerimage50.jpg)
    *  ![preview](images/dockerimage51.jpg)
    *  ![preview](images/dockerimage52.jpg)
    *  now login into docker 
    *  $ docker login <loginservername>
    *  give the user name and password of ACR
    *  ![preview](images/dockerimage53.jpg)
    *  we have login successfully
    *  Now manually we try to push image of nginix
    *  $ docker image pull nginx
    *  $ docker tag <image id> <ACR username>
    *  $ docker tag <image id> <ACR username>/nginx
    *  ![preview](images/dockerimage54.jpg)
    *  ![preview](images/dockerimage55.jpg)
    *  $ docker tag <ACR loginuser>/nginx
    *  now push the image
    *  $ docker push <ACR username>/nginx
    *  ![preview](images/dockerimage57.jpg)
    * Now push the images given below :-
       *  a. student courses register (SCR)
       *  In the same vm create a docker file for scr
       *  $ sudo vi Dockerfile  (write a docker file for scr)
       *  $ docker image build -t scr .
       *  $ docker image ls
       *  ![preview](images/dockerimage58.jpg)
       *  check in the ACR 
       *  ![preview](images/dockerimage56.jpg)

       * b. spring petclinic
       * 
   



   






