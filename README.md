# Eureka Server

* eureka server is like a phone book

## running an instance of eureka server

* it is going to be the instance that's going to collect all the information about microservices
* add `@EnableEurekaServer` annotation that will provide a variety of auto configuration that will
be used to construct the Eureka Server 
* edit `application.properties`
  ```properties
    server.port=8761
    eureka.client.register-with-eureka=false
    eureka.client.fetch-registry=false
  ```
  * *8761* is a common port used for Eureka server
  * `eureka.client.register-with-eureka` will prevent that eureka server (this instance) registry to the eureka server bc is not a client
  * `eureka.client.fetch-registry=false` there is no need to fetch the registry (fetch all the information of other services)
    because it's going to housing them. There is no need to contact eureka server to get information already has
* run the application and enter to `http://localhost:8761` to see the administration management instance of eureka server
  * administration site will list instances registered with eureka

## redundant eureka servers

* with one eureka server there is one single point of failure
* it is common to set up another server to act as a replica to add some redundancy to the architecture
* having multiple eureka servers provides with fail over in case of an issue 
* steps to create a second eureka server
  * modify the `hosts` file, this file is going to map _host names_ to _IP_ addresses
    * `C:/Windows/System32/drivers/etc` in windows and `/etc/hosts` in linux and mac
    * associate 2 hosts
      ```
        127.0.0.1   peer1
        127.0.0.1   peer2
      ```
  * create one property file for each server (peer1 and peer2 for this demo) 
    * property `eureka.client.serviceUrl.defaultZone` is used to tell the instance to provide the value as a replica(s) if there are more than
      one, must go separated by comma
    * [official reference](http://cloud.spring.io/spring-cloud-netflix/single/spring-cloud-netflix.html#spring-cloud-eureka-server-peer-awareness)
    * [other reference](https://thepracticaldeveloper.com/2018/03/18/spring-boot-service-discovery-eureka/)
  * create a Run Configuration for each server
    * _VM arguments (eclipse)_: `-Dspring.profiles.active=[peer1|peer2]`
    * _Spring Boot settings - Active Profiles (intellij)_: `[peer1|peer2]`
* these servers will replicate any application instances or microservices that register with the spring eureka service and 
  that provide some redundancy within the architecture of the distributed system
  
## Related documentation

* [the mystery of eureka health monitoring](https://dzone.com/articles/the-mystery-of-eureka-health-monitoring)
* [the mystery of eurekas self preservation](https://dzone.com/articles/the-mystery-of-eurekas-self-preservation)