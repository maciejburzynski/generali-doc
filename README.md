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

Service to store systems' users. Exposing following endpoints:
- `/users/login` - to receive JWT to be authenticated to work with other services. Token is valid for 10 mins. The endpoint does not require to be authenticated.
- `/users/register` - to register users. Requires authentication before.
- `/users` - returns all registered users users. Requires increased priviliges. 
- `/users/{id}`- for updating particular user. Requires increased priviliges.
