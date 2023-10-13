## Documentation for Generali SpringPL1 training

### Services 
- [user-auth-service](https://github.com/maciejburzynski/generali-user-auth-service)
- [mail-service](https://github.com/maciejburzynski/generali-mail-service)

Each service consists of:
- Exposed Rest API
- Database: H2
- Spring Security (with JWT/Basic authorization)
- Spring Data JPA
- Hibernate Validator 
- Flyway 
- Swagger UI

<p align="center">
    <img src="/Generali-diagram.drawio.png" >
</p>

### user-auth-service

Service to store systems' users. Exposes following endpoints:
- `POST` `/users/login` - to receive JWT to be authenticated to work with other services. Token is valid for 10 mins. The endpoint does not require to be authenticated.
- `POST` `/users/register` - to register users. Requires authentication before.
- `GET` `/users` - returns all registered users users. Requires increased priviliges. 
- `PUT` `/users/{id}`- for updating particular user. Requires increased priviliges.

#### User registration process 

- To register new User, send `POST` on `/users/register` endpoint is required. In body there should be `username` and `password` fields. 
- Newly registered User is inactive as flags: `isAccountNonExpired`, `isAccountNonLocked`, `isCredentialsNonExpired`, `isEnabled` are set to false.
- Activation link is sent via [mail-service](https://github.com/maciejburzynski/generali-mail-service)
- Once link is clicked, appropriate GET method is executed which is responsible for user activation.
- If all fine - User is activated then all flags(`isAccountNonExpired`, `isAccountNonLocked`, `isCredentialsNonExpired`, `isEnabled`) are set to true.

- Pattern for Activation link is following: `baseUrl` `/users/<user-uuid>=<true/false>`. This link can be user for user activation as well as user blocking/inactivation.

### mail-service

Service responsible for user notification/communication. Exposes following endpoints:
- `POST` `/api/mails` - to send an e-mail. 
- `GET` `/api/mails` - to get all mails.

Usage of the service requires:
- for other application - Basic Auth 
- for users - JWT

*`send email` - save at local machine in home directory, folder e.g. `~/user/Desktop/Mails`.

Mail naming convention: `<day>-<month>-<year><hour><minute><second>-<receiver>.txt`.

Content of example mail:
```console

To: <receiver(s) address>
Subject: <subject>
Content: <mail content>


Best Regards,
Generali's Mail Service :)

```

Todo:
- Zamodeluj dane w order-service używając klasy abstrakcyjnej Product
- Dziedzicz po klasie Product następujące klasy: Laptop, Monitor

