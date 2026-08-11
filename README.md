spring.application.name=api-gateway
server.port=8080

# Eureka Server
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
eureka.client.register-with-eureka=true
eureka.client.fetch-registry=true

# Gateway Routes

spring.cloud.gateway.routes[0].id=user-service
spring.cloud.gateway.routes[0].uri=lb://USER-SERVICE
spring.cloud.gateway.routes[0].predicates[0]=Path=/api/users/**

spring.cloud.gateway.routes[1].id=project-service
spring.cloud.gateway.routes[1].uri=lb://PROJECT-SERVICE
spring.cloud.gateway.routes[1].predicates[0]=Path=/api/projects/**

spring.cloud.gateway.routes[2].id=issue-service
spring.cloud.gateway.routes[2].uri=lb://ISSUE-SERVICE
spring.cloud.gateway.routes[2].predicates[0]=Path=/api/issues/**


package com.its.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ApiGatewayApplication {

    public static void main(String[] args) {
        SpringApplication.run(ApiGatewayApplication.class, args);
    }
}
