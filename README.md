1. Introduction

API automation ensures that RESTful APIs function correctly by validating status codes, response payloads, headers, and error handling. This project automates the Bookstore API using Rest Assured, TestNG, and Maven, ensuring comprehensive test coverage.

2. Technologies Used

	•	Java 11+ → Programming language for writing tests.
	•	Rest Assured → Java library for API testing.
	•	TestNG → Test framework to organize and run test cases.
	•	Maven → Build tool for dependency management.
	•	JSON → Data format for API requests/responses.

4. Project Structure

bookstore-api-automation/
│-- src/
│   ├── main/
│   │   ├── java/
│   │   ├── resources/
│   ├── test/
│   │   ├── java/
│   │   │   ├── base/
│   │   │   │   ├── BaseTest.java
│   │   │   ├── tests/
│   │   │   │   ├── BookstoreTests.java
│   │   │   ├── utils/
│   │   │   │   ├── ConfigReader.java
│   │   ├── resources/
│   │   │   ├── testng.xml
│-- pom.xml  (Maven dependencies)
│-- README.md 

4. Prerequisites

✅ Install Java 11+
✅ Install Maven
✅ Install Git
✅ Install IDE (IntelliJ IDEA / Eclipse)

5.Clone or Create the Project

Clone from GitHub

git clone https://github.com/yourusername/bookstore-api-automation.git
cd bookstore-api-automation

Or Create a New Maven Project

	1.	Open IntelliJ IDEA / Eclipse
	2.	Create a New Maven Project
	3.	Add the following dependencies in pom.xml:

 <dependencies>
    <dependency>
        <groupId>io.rest-assured</groupId>
        <artifactId>rest-assured</artifactId>
        <version>5.3.0</version>
    </dependency>
    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.6.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>

       4.      Run mvn clean install to download dependencies.

6. Configure API Base URL

Create a ConfigReader.java utility class:

public class ConfigReader {
    public static final String BASE_URL = "http://127.0.0.1:8000"; // Change if needed
}

7. Run API Tests

Create a BaseTest.java file for setup:

import io.restassured.RestAssured;
import org.testng.annotations.BeforeClass;

public class BaseTest {
    @BeforeClass
    public void setup() {
        RestAssured.baseURI = ConfigReader.BASE_URL;
    }
}

8. API Endpoints & Test Cases

📌 Create a New Book

import io.restassured.http.ContentType;
import org.testng.annotations.Test;
import static io.restassured.RestAssured.given;
import static org.hamcrest.Matchers.equalTo;

public class BookstoreTests extends BaseTest {

    @Test
    public void createBook() {
        String requestBody = "{ \"title\": \"API Testing Book\", \"author\": \"John Doe\" }";

        given()
            .contentType(ContentType.JSON)
            .body(requestBody)
        .when()
            .post("/books")
        .then()
            .statusCode(201)
            .body("title", equalTo("API Testing Book"));
    }
}

📌 Get a Book by ID

@Test
public void getBookById() {
    given()
    .when()
        .get("/books/1")
    .then()
        .statusCode(200)
        .body("id", equalTo(1));
}

📌 Update a Book

@Test
public void updateBook() {
    String requestBody = "{ \"title\": \"Updated Book\", \"author\": \"Jane Doe\" }";

    given()
        .contentType(ContentType.JSON)
        .body(requestBody)
    .when()
        .put("/books/1")
    .then()
        .statusCode(200)
        .body("title", equalTo("Updated Book"));
}

📌 Delete a Book

@Test
public void deleteBook() {
    given()
    .when()
        .delete("/books/1")
    .then()
        .statusCode(204);
}

9. Running Tests Using TestNG

To run all test cases, create a testng.xml file:

 <suite name="APITests">
    <test name="Bookstore API Tests">
        <classes>
            <class name="tests.BookstoreTests"/>
        </classes>
    </test>
</suite>

Run the tests:

 mvn test
 
 10. Generate & View Test Reports

Generate TestNG Reports
	
 1.	Run the tests:
 
 mvn test

2.	Open test-output/index.html to view reports.

11. Running API Requests Manually (Using cURL)

Create a New Book

curl -X POST "http://127.0.0.1:8000/books" -H "Content-Type: application/json" -d '{ "title": "API Book", "author": "John" }'

 Get a Book

 curl -X GET "http://127.0.0.1:8000/books/1"
 
 Update a Book

 curl -X PUT "http://127.0.0.1:8000/books/1" -H "Content-Type: application/json" -d '{ "title": "Updated Title", "author": "Jane" }'

 Delete a Book
 
 curl -X DELETE "http://127.0.0.1:8000/books/1"

 curl -X DELETE "http://127.0.0.1:8000/books/1"

Troubleshooting:

API Not Responding:

1.Ensure the FastAPI server is running locally (uvicorn main:app --reload).

2.Check your config.properties to ensure the base URL is correct.

Dependency Issues:

1.Make sure all dependencies are correctly added to pom.xml.

2.Run mvn clean install to refresh dependencies.


