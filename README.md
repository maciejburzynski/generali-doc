## Documentation for Generali SpringPL1 training

### Table of contents 
- [Multiple Flyways](https://github.com/maciejburzynski/generali-mail-service/tree/master/src/main/java/com/generali/mailservice/flyway)
- [Spring Data JPA](https://github.com/maciejburzynski/generali-mail-service/blob/master/src/main/java/com/generali/mailservice/mail/Mail.java)
- [Spring Data JDBC](https://github.com/maciejburzynski/generali-attachment-service/tree/main/attachment-service/src/main/java/com/generali/attachmentservice)
- [Multiple data sources](https://github.com/maciejburzynski/generali-mail-service/tree/master/src/main/java/com/generali/mailservice/multipledatasources)
- [Active-MQ / JMS publisher](https://github.com/maciejburzynski/generali-order-service/tree/main/Spring_2/src/main/java/pl/generali/Spring/activemq)
- [Active-MQ / JMS consumer](https://github.com/maciejburzynski/generali-attachment-service/tree/main/attachment-service/src/main/java/com/generali/attachmentservice/activemq)
- [Batch jobs - @Scheduled](https://github.com/maciejburzynski/generali-mail-service/blob/master/src/main/java/com/generali/mailservice/mail/MailScheduler.java)
- [Api Consuming](https://github.com/maciejburzynski/generali-order-service/tree/main/Spring_2/src/main/java/pl/generali/Spring/apiconsuming)
- [Api Exposure](https://github.com/maciejburzynski/generali-order-service/blob/main/Spring_2/src/main/java/pl/generali/Spring/order/product/laptop/LaptopRestController.java)
- [Rest Clients](https://github.com/maciejburzynski/generali-user-auth-service/tree/master/src/main/java/com/generali/userauthservice/restclients)
- [JWT](https://github.com/maciejburzynski/generali-user-auth-service/tree/master/src/main/java/com/generali/userauthservice/jwt)
- [User Entity (roles, authorities)](https://github.com/maciejburzynski/generali-user-auth-service/tree/master/src/main/java/com/generali/userauthservice/user)
- [Basic Spring User](https://github.com/maciejburzynski/generali-order-service) - Readme
- [Optimistic locking](https://github.com/maciejburzynski/generali-attachment-service)
- [Specification](https://github.com/maciejburzynski/generali-user-auth-service/tree/master/src/main/java/com/generali/userauthservice/user/jpaspecification)
- [Global Exception Handler](https://github.com/maciejburzynski/generali-attachment-service)
- [XML to Java Conversion / Jaxb](https://github.com/maciejburzynski/generali-user-auth-service/tree/master/src/main/java/com/generali/userauthservice/user/xml)
- [Dto validator/ Hibernate validator](https://github.com/maciejburzynski/generali-user-auth-service/blob/master/src/main/java/com/generali/userauthservice/user/UserDto.java)
- [Pagination + Path variable + Request param](https://github.com/maciejburzynski/generali-order-service/blob/main/Spring_2/src/main/java/pl/generali/Spring/order/product/laptop/LaptopRestController.java)
- [Swagger](https://github.com/maciejburzynski/generali-mail-service/tree/master/src/main/java/com/generali/mailservice/swagger)

### Services 
- [user-auth-service](https://github.com/maciejburzynski/generali-user-auth-service)
- [mail-service](https://github.com/maciejburzynski/generali-mail-service)
- [order-service](https://github.com/maciejburzynski/generali-order-service)
- [attachment-service](https://github.com/maciejburzynski/generali-attachment-service)

Each service consists of:
- Exposed Rest API
- Database: H2
- Spring Security (with JWT/Basic authorization)
- Spring Data JPA/ Spring Data JDBC
- Hibernate Validator 
- Flyway 
- Swagger UI

<p align="center">
    <img src="/Generali.drawio.png" >
</p>

## user-auth-service

Service to store systems' users. Exposes following endpoints:
- `POST` `/users/login` - to receive JWT to be authenticated to work with other services. Token is valid for 10 mins. The endpoint does not require to be authenticated.
- `POST` `/users/register` - to register users. Requires authentication before.
- `GET` `/users` - returns all registered users users. Requires increased priviliges. 
- `PUT` `/users/{id}`- for updating particular user. Requires increased priviliges.

#### User registration process 

- To register new user, send `POST` request on `/users/register` endpoint. In body there should be `username` and `password` fields. 
- Newly registered user is inactive as flags: `isAccountNonExpired`, `isAccountNonLocked`, `isCredentialsNonExpired`, `isEnabled` are set to false.
- Activation link is sent via [mail-service](https://github.com/maciejburzynski/generali-mail-service)
- Once link is clicked, appropriate GET method is executed which is responsible for user activation.
- If all fine - user is activated then all flags(`isAccountNonExpired`, `isAccountNonLocked`, `isCredentialsNonExpired`, `isEnabled`) are set to true.

- Pattern for Activation link is following: `<baseUrl>/users/<user-uuid>`. This link is used for user activation.
  
#### User login process

- To login user, send `POST` on `/users/login` endpoint is required. In body there should be `username` and `password` fields.
- If user exists, credentials are correct, flags(`isAccountNonExpired`, `isAccountNonLocked`, `isCredentialsNonExpired`, `isEnabled`) are set to true, JWT to authenticate is returned in response.
- If username/password is not correct, user doesn't exist or is inactive - appropriate message is returned to the client, based on thrown `Exception` or validation performed.

## mail-service

Service responsible for user notification/communication. Exposes following endpoints:
- `POST` `/api/mails` - to send an e-mail. 
- `GET` `/api/mails` - to get all mails.

Usage of the service requires:
- for other application - Basic Auth 
- for users - JWT

*`send email` - save at local machine in home directory, folder e.g. `~/user/Desktop/Mails`.

Mail naming convention: `<day>-<month>-<year><hour><minute><second><miliseconds>-<endoded-in-base64-receiver(s)>.txt`.

Content of example mail:
```console

To: <receiver(s) address>
Subject: <subject>
Content: <mail content>


Best Regards,
Generali's Mail Service :)

```
## order-service 

Service responsible for order processing. Exposes lots of endpoints. The most functional ones are:
- `POST` `/api/orders` - to add order. 
- `GET` `/api/orders` - to get all orders.
  
## attachment-service

Service responsible for attachment processing. Exposes 2 endpoints:
- `POST` `/api/attachments` - to add attachment. 
- `GET` `/api/attachments` - to get all attachments.


