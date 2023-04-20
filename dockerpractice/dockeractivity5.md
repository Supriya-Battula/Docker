# Create a jenkins image by creating your own docker.
* $ vi dockerfile
```
FROM ubuntu:22.04
LABEL author="Supriya" organization="qt" project="learning"
RUN apt update && apt install openjdk-11-jdk maven curl -y
RUN curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null
RUN echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
RUN apt-get update && apt-get install jenkins -y
EXPOSE 8080
CMD ["/usr/bin/jenkins"]
```
Results
 ```
  $ docker image build -t jenkins .
  $ docker container run -d --name ubuntu -P jenkins
  $ docker container ls
 ```
 * ![preview](images/dockerimage26.jpg)
Open the port that have given (49513)
 *   ![preview](images/dockerimage27.jpg)
  Login into jenkins
  ![preview](images/dockerimage30.jpg)
  ![preview](images/dockerimage31.jpg)
  ![preview](images/dockerimage32.jpg)
* Jenkins will getting started
* ![preview](images/dockerimage33.jpg)
* ![preview](images/dockerimage34.jpg)
* ![preview](images/dockerimage35.jpg)


# Create nop commerce and mysql server container and try to make them work by configure

* 1. create network
    $ docker network
    $ docker network create -d bridge --subnet "10.0.0.0/24" mysqlnetwork
    ![preview](images/dockerimage38.jpg)
* 2. create volume
     $  docker volume create sqlvolume
     ![preview](images/dockerimage39.jpg)
* 3. create sql (container)
   $ docker container run -d --name sqldb -v MYVOL:/var/libsqldb --network mysqlnetwork -e MYSQL_USER=supriya -e MYSQL_ROOT_PASSWORD=nani123 -e MYSQL_DATABASE=employees -e -P mysql:8
   ![preview](images/dockerimage40.jpg)
* 4. create npo commerse docker file
  $ vi docker (write dockerfile)
  ```
  FROM mcr.microsoft.com/dotnet/sdk:7.0
  LABEL author="Prakash" organization="qt" project="learning"
  ADD https://github.com/nopSolutions/nopCommerce/releases/download/release-4.60.2/nopCommerce_4.60.2_NoSource_linux_x64.zip /nop/nopCommerce_4.60.2_NoSource_linux_x64.zip
  WORKDIR /nop
  RUN apt update && apt install unzip -y && \
    unzip /nop/nopCommerce_4.60.2_NoSource_linux_x64.zip && \
    mkdir /nop/bin && mkdir /nop/logs
  EXPOSE 5000
  CMD [ "dotnet", "Nop.Web.dll","--urls", "http://0.0.0.0:5000" ]
  ```
  $  docker container run -d --name mynop -P --network mysqlnetwork -e MTSQL_SERVER=sqldb nani
  $ docker container ls
  ![preview](images/dockerimage41.jpg)
open the port 
![preview](images/dockerimage36.jpg)
install
![preview](images/dockerimage37.jpg)
after install web will be get slow down , we have restart the web page
$ docker container start "container id/container name"
![preview](images/dockerimage42.jpg)
Final Results
![preview](images/dockerimage43.jpg)
