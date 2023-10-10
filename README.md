## Documentation for Generali SpringPL1 training

### Services 
- [user-auth-service](https://github.com/maciejburzynski/generali-user-auth-service)
- [mail-service](https://github.com/maciejburzynski/GeneraliJavaPL1) 

Each service consists of:
- Exposed Rest API
- Database: H2
- Spring Security (with JWT/Basic authorization)
- Spring Data JPA
- Hibernate Validator 
- Flyway 
- Swagger UI

<p align="center">
    <img src="/service-example.png" alt="example-service">
</p>

### user-auth-service

Service to store systems' users. Exposes following endpoints:
- `POST` `/users/login` - to receive JWT to be authenticated to work with other services. Token is valid for 10 mins. The endpoint does not require to be authenticated.
- `POST` `/users/register` - to register users. Requires authentication before.
- `GET` `/users` - returns all registered users users. Requires increased priviliges. 
- `PUT` `/users/{id}`- for updating particular user. Requires increased priviliges.

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


