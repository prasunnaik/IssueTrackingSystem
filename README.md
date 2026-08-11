spring.application.name=api-gateway
server.port=8084

# Eureka
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
eureka.client.register-with-eureka=true
eureka.client.fetch-registry=true

# User Service
spring.cloud.gateway.server.webflux.routes[0].id=user-service
spring.cloud.gateway.server.webflux.routes[0].uri=lb://USER-SERVICE
spring.cloud.gateway.server.webflux.routes[0].predicates[0]=Path=/api/users/**

# Project Service
spring.cloud.gateway.server.webflux.routes[1].id=project-service
spring.cloud.gateway.server.webflux.routes[1].uri=lb://PROJECT-SERVICE
spring.cloud.gateway.server.webflux.routes[1].predicates[0]=Path=/api/projects/**

# Issue Service
spring.cloud.gateway.server.webflux.routes[2].id=issue-service
spring.cloud.gateway.server.webflux.routes[2].uri=lb://ISSUE-SERVICE
spring.cloud.gateway.server.webflux.routes[2].predicates[0]=Path=/api/issues/**
