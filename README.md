# Exercise 16 · First Two Spring Framework Apps

Two independent Maven projects for the Spring Framework exercise. Both use Java 21 and Spring Boot 4.2.0.

## Run a project

Open the project's `pom.xml` in IntelliJ IDEA, select JDK 21, and run its application class. Alternatively, run the Maven Wrapper from that project's directory.

```powershell
.\mvnw.cmd spring-boot:run
```

On macOS or Linux, use `./mvnw spring-boot:run`. Both apps use port 8080 by default. Stop one before starting the other.

## Task 1 · Spring MVC and Thymeleaf

Open [first-project-java-spring](first-project-java-spring/).

- [Home](http://localhost:8080/) returns plain text using `@ResponseBody`.
- [Greeting](http://localhost:8080/greeting) displays `Hello, World!` and the Vistula logo.
- [Personalized greeting](http://localhost:8080/greeting?name=Vistula) displays `Hello, Vistula!`.

## Task 2 · Product REST API

Open [First-Rest-Api-Spring](First-Rest-Api-Spring/).

[Swagger UI](http://localhost:8080/swagger-ui/index.html) · [OpenAPI JSON](http://localhost:8080/v3/api-docs) · [H2 Console](http://localhost:8080/console/)

| Method | Path | Successful HTTP response |
| --- | --- | --- |
| POST | `/api/v1/products` | `201 Created` |
| GET | `/api/v1/products` | `200 OK` with a list |
| GET | `/api/v1/products/{id}` | `200 OK` with the product |
| PUT | `/api/v1/products/{id}` | `200 OK` with the updated product |
| DELETE | `/api/v1/products/{id}` | `204 No Content` |

Send a JSON body such as `{"name":"First product"}` when creating or updating a product. Requests for a missing product ID return `404 Not Found` with a JSON error message. These status codes are the observed HTTP responses.

To check the database, log in to the H2 Console with JDBC URL `jdbc:h2:mem:testdb`, user `sa`, and an empty password. Run the query after creating, updating, and deleting products.

```sql
SELECT ID, NAME FROM PRODUCTS ORDER BY ID;
```

### Slide 62 · How does an empty repository interface work?

`ProductRepository` extends `JpaRepository<Product, Long>` and inherits its CRUD method signatures. At startup, Spring Data JPA creates a repository proxy and registers it as a Spring bean. The proxy delegates standard CRUD calls to `SimpleJpaRepository`, which uses the JPA `EntityManager` to access the database.
