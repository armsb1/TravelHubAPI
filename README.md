# TravelHub - Travel Management API

## **Project Description**
TravelHub is a RESTful API for managing trips, allowing you to create, update, and delete trip information. Additionally, the API provides integration with external services to fetch real-time city and exchange rate data. This project was developed as part of a portfolio and is ideal for demonstrations.

## **Features**

### **1. Trip Management**
- **GET /trips**: Lists all registered trips.
- **POST /trips**: Adds a new trip.
- **GET /trips/{id}**: Shows details of a specific trip.
- **PUT /trips/{id}**: Updates an existing trip.
- **DELETE /trips/{id}**: Deletes a trip.

### **2. Integration with External APIs**
- **GET /info/city/{cityName}**: Fetches city information using the GeoDB Cities API (synchronous).
- **GET /info/exchange/{currencyCode}**: Fetches the current exchange rate using the ExchangeRate-API (asynchronous).

### **3. API Documentation**
The API's interactive documentation is automatically generated using Swagger and OpenAPI. Access the Swagger UI at `http://localhost:8080/swagger-ui.html` after starting the application.

### **4. Data Cleanup and Initial Data**
- The application automatically deletes extra data every day at midnight (00:00).
- Initial data is recreated automatically to maintain a demonstration experience.
- Endpoint for manual data cleanup: **DELETE /admin/clear**.

## **Project Setup**

### **Prerequisites**
- Java 17+
- Maven 3.6+
- IntelliJ IDEA or another IDE

### **Installation**
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/travelhub.git
   cd travelhub
   ```
2. Set up dependencies:
   ```bash
   mvn clean install
   ```
3. Run the application:
   ```bash
   mvn spring-boot:run
   ```
4. Access the application in your browser: `http://localhost:8080`

### **H2 Database**
The API uses an H2 in-memory database to persist data. The H2 console can be accessed at `http://localhost:8080/h2-console`.

#### **H2 Access Credentials**
- URL: `jdbc:h2:mem:travelhub`
- User: `sa`
- Password: *(empty)*

### **Initial Data Setup**
The `data.sql` file inserts the following initial data into the database:
```sql
INSERT INTO trips (name, destination, start_date, end_date, budget, activities)
VALUES ('Trip to Paris', 'Paris', '2025-06-01', '2025-06-10', 1500, 'Museums, Shopping');
```

## **Integration with External APIs**

### **1. GeoDB Cities**
- **Base Endpoint:** https://wft-geo-db.p.rapidapi.com/v1/geo
- **Function:** Fetches information about a city.
- **Sample Response:**
  ```json
  {
      "data": {
          "name": "Paris",
          "country": "France",
          "latitude": 48.8566,
          "longitude": 2.3522
      }
  }
  ```
- **Authentication:** Use the key obtained from [RapidAPI](https://rapidapi.com/).

### **2. ExchangeRate-API**
- **Base Endpoint:** https://api.exchangerate.host/latest
- **Function:** Fetches updated exchange rates.
- **Sample Response:**
  ```json
  {
      "rates": {
          "USD": 1.0,
          "EUR": 0.85,
          "GBP": 0.75
      }
  }
  ```
- **Authentication:** Does not require an API key.

### **Consumption Modes**
- **GeoDB Cities** will be consumed **synchronously**.
- **ExchangeRate-API** will be consumed **asynchronously** using `WebClient` for non-blocking calls.

## **Scheduled Tasks**
The application deletes extra data every day at midnight (00:00) using the following schedule:
```java
@Scheduled(cron = "0 0 0 * * ?")
```
Additionally, sample data is automatically repopulated after the cleanup.

## **Tests**
The project includes basic unit tests for endpoints and validations.
Run tests with the following command:
```bash
mvn test
```

## **Deploy with Docker** (Optional)
1. Build the Docker image:
   ```bash
   docker build -t travelhub .
   ```
2. Run the container:
   ```bash
   docker run -p 8080:8080 travelhub
   ```

## **Contributions**
Contributions are welcome! Feel free to open issues or submit pull requests.

## **License**
This project is licensed under the MIT License. See the LICENSE file for more details.

---

### **Contact**
For questions or suggestions, contact:
- **Name:** [Your Name]
- **Email:** andre.sousabaptista@gmail.com
