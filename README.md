# api-gateway
spring cloud gateway for routing purpose and managing cross cutting concerns -  part of Currency Application currency-exchange-service , currency-conversion-service , git-localconfig-repo and spring-cloud-config-server

---
### Spring Cloud API Gateway - Step 22 to Step 25
---

(-2) Give these settings a try individually in application.properties of all microservices (currency-exchange, currency-conversion, api-gateway) to see if they help

```
eureka.instance.prefer-ip-address=true
```

OR

```
eureka.instance.hostname=localhost
```

(0) Make sure you are using the right URLs?

Discovery
- http://localhost:8765/CURRENCY-EXCHANGE/currency-exchange/from/USD/to/INR
- http://localhost:8765/CURRENCY-CONVERSION/currency-conversion/from/USD/to/INR/quantity/10
- http://localhost:8765/CURRENCY-CONVERSION/currency-conversion-feign/from/USD/to/INR/quantity/10

LowerCase
- http://localhost:8765/currency-exchange/currency-exchange/from/USD/to/INR
- http://localhost:8765/currency-conversion/currency-conversion/from/USD/to/INR/quantity/10
- http://localhost:8765/currency-conversion/currency-conversion-feign/from/USD/to/INR/quantity/10

Discovery Disabled and Custom Routes Configured
- http://localhost:8765/currency-exchange/from/USD/to/INR
- http://localhost:8765/currency-conversion/from/USD/to/INR/quantity/10
- http://localhost:8765/currency-conversion-feign/from/USD/to/INR/quantity/10
- http://localhost:8765/currency-conversion-new/from/USD/to/INR/quantity/10


(1) Are you using right configuration?

```
spring.application.name=api-gateway
server.port=8765

eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka

#spring.cloud.gateway.discovery.locator.enabled=true
#spring.cloud.gateway.discovery.locator.lowerCaseServiceId=true
```

(2) Compare against the code for ApiGatewayConfiguration below?

(3) Compare against the code for LoggingFilter below?

(4) Ensure that all the three services are registered with Eureka at http://localhost:8761/.

(5) Try if it works when you include the following property in application.properties for currency-conversion-service and currency-exchange-service

```
eureka.instance.hostname=localhost
```

(6) Some student reported success when using lower-case-service-id instead of spring.cloud.gateway.discovery.locator.lowerCaseServiceId. See if it helps!

```
spring.cloud.gateway.discovery.locator.enabled=true

spring.cloud.gateway.discovery.locator.lower-case-service-id=true

#spring.cloud.gateway.discovery.locator.lowerCaseServiceId=true


```

(7) Compare code against the complete list of components below.


If everything is fine

(1) Make sure you start the services in this order (a) netflix-eureka-naming-server (b) netflix-zuul-api-gateway-server (c)currency-exchange-service (d)currency-conversion-service

(2) Make sure all the components are registered with naming server.

(3) Give a minute of warm up time!

(4) If you get an error once, execute it again after 5 minutes

If you still have a problem, post a question including all the details:

(1) Screenshot of services registration with Eureka

(2) Responses from all the 5 URLs - http://localhost:8100/currency-conversion-feign/from/EUR/to/INR/quantity/10000, http://localhost:8000/currency-exchange/from/EUR/to/INR, http://localhost:8100/currency-conversion/from/USD/to/INR/quantity/10 and the URLs listed above!

(3) Start up logs of the each of the components to understand what's happening in the background!

(4) If you still get an error, post the logs of the each of the components to understand what's happening in the background!

(5) What was the last working state of the application? Explain in Detail.

(6) Post the version of Spring Boot and Spring Cloud You are Using.

(7) Post Code for all the components listed below:



---
### Step 22
---

Step 22 - Setting up Spring Cloud API Gateway

On Spring Initializr, choose:
- Group Id: com.hanil.microservices
- Artifact Id: api-gateway
- Dependencies
	- DevTools
	- Actuator
	- Config Client
	- Eureka Discovery Client
	- Gateway (Spring Cloud Routing)

#### /api-gateway/src/main/resources/application.properties Modified

```properties
spring.application.name=api-gateway
server.port=8765

eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka
```

---
### Step 23
---

Step 23 - Enabling Discovery Locator with Eureka for Spring Cloud Gateway

Initial
- http://localhost:8765/CURRENCY-EXCHANGE/currency-exchange/from/USD/to/INR
- http://localhost:8765/CURRENCY-CONVERSION/currency-conversion/from/USD/to/INR/quantity/10
- http://localhost:8765/CURRENCY-CONVERSION/currency-conversion-feign/from/USD/to/INR/quantity/10

Intermediate
- http://localhost:8765/currency-exchange/currency-exchange/from/USD/to/INR
- http://localhost:8765/currency-conversion/currency-conversion/from/USD/to/INR/quantity/10
- http://localhost:8765/currency-conversion/currency-conversion-feign/from/USD/to/INR/quantity/10


#### /api-gateway/src/main/resources/application.properties Modified

```properties
spring.application.name=api-gateway
server.port=8765

eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka

spring.cloud.gateway.discovery.locator.enabled=true
spring.cloud.gateway.discovery.locator.lowerCaseServiceId=true
```

