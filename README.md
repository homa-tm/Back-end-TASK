
## **Task Overview**

This task involves building two Nest.js services: 
1. **Wallet Service**: Ingests data from a JSON file, stores it in a database using Sequelize, and provides endpoints for wallet analysis.
2. **Caller Service**: Simulates high-load conditions by making frequent requests to the Wallet Service.
3. Use best practices for setting up services, creating APIs, and handling high-load scenarios.

### **Requirements**
- Use **Nest.js** and **Sequelize**.
- Plan a database schema. 
- Employ **RabbitMQ** for inter-service communication.
- Include basic **logging** (e.g., with Winston) and **error handling**.
- Set Up **Swagger** For API documentation.
- Utilize **environment configuration** (.env files).
- **Bonus**: Dockerize the application.
- if you have Implemented a **scalablity** solution make sure you briefly explain it in the README file.

---

## **Section 1: Wallet Service**

### **Step 1: Database Schema**

- Use **Sequelize** with PostgreSQL to create a `wallets` table. The schema should include the following columns but leaves room for creativity:
  - **`address`**: (Primary Key) - The wallet's address, used for searching and identification.
  - **`total_profit`**: The total profit/loss of the wallet.
  - **Other columns**: You may include columns like `total_profit`, `num_tokens_traded`, `num_active_days`, `avg_trade_volume`, `risk_assessment`, `last_updated` and others as needed for your analysis.


### **Step 2: Data Ingestion**
- Implement a method to **parse the JSON file** and populate the `wallets` table. Use streaming or batching techniques for efficient processing of large files.

### **Step 3: Endpoints**
Implement the following, simpler set of endpoints:

1. **`GET /wallets`**:
   - **Description**: Fetch a list of wallets with optional sorting.
   - **Query Parameters**:
     - `sort_by`: Can be `total_profit`, `num_tokens_traded`, or `num_active_days`.
     - `order`: `asc` or `desc`.
   - **Example**: `/wallets?sort_by=total_profit&order=desc`.

2. **`GET /wallets/:address`**:
   - **Description**: Fetch detailed information for a specific wallet using its address.
   - **Response**: Returns the characteristics of the specified wallet.

3. **`POST /wallets/analyze`**:
   - **Description**: Analyzes the JSON data to update the wallet information in the database.
   - **Body Parameters**: None. The service automatically processes the JSON file.
   - **Response**: Returns a status message indicating the success of the analysis.

4. **`GET /wallets/summary`**:
   - **Description**: Retrieve basic statistics about the wallets (e.g., average profit, count of wallets analyzed).
   - **Response**: Returns a summary with key statistics.


### **Step 4: Data Analysis**
- In the `POST /wallets/analyze` endpoint, implement logic to:
  - Calculate total profit/loss, identify most/least profitable tokens.
  - Determine the number of unique tokens traded and active trading days.
  - Assess risk levels based on diversification.
- Save results to the database.

### **Step 5: Integration with RabbitMQ**
- Implement RabbitMQ to queue and process requests from the Caller Service.
- Optimize RabbitMQ settings for high throughput (e.g., persistent messages, acknowledgment, retries).

---

## **Section 2: Caller Service**

### **Objective**
Simulate high-load conditions by making randomized requests to the Wallet Service via RabbitMQ.

### **Step 1: Implementation**
- Set up the `CallerService` to:
  - Use a cron job or interval to **send 50 random requests per minute** to the Wallet Service.
  - Randomly choose between the Wallet Service endpoints (`/wallets`, `/wallets/:id`, `/wallets/top-tokens/:id`).
  - Utilize RabbitMQ for sending requests, allowing for asynchronous processing.
  - Log the the latency of each response.

### **Step 2: Logging and Error Handling**
- Implement logging using **Winston** to capture the status of each request (success/failure, latency).
- Handle errors using retry logic for failed requests and manage timeouts gracefully.

---

## **Additiona Tasks**

- **Scalability Discussion**: Include a section in this `README.md` on how you would scale this system to handle higher traffic or larger data volumes.
- **Environment Configuration**: Store all configurations in `.env` files. Include a sample `.env.example` file.
- **Scalability**: Document in the `README.md` how you would scale the service (e.g., using caching, database indexing, and optimizing RabbitMQ settings).

## **Additiona Tasks**

1. **Docker**: Implement Docker to containerize both services for easier deployment.
2. **Caching**: Implement caching for wallet data to minimize database reads and improve response times.

---


## **Submission Guidelines**
1. Implement the solution as a node.js (preferedly Nest.js) application with the above services and endpoints.
2. Structure your project with clear separation between services (`CallerService` and `WalletService`).
3. Include a `README.md` with instructions for setting up, running, and testing the application.
4. Compress the project folder (excluding large data files) and email it, or send to the telegram ID that contacted you, or provide a downloadable link for the entire project.

---

By completing this task, you will demonstrate your ability to:
- Design and implement efficient data processing for large JSON files.
- Create a robust database schema for storing and querying analytical data.
- Develop scalable and reliable API endpoints in a Nest.js application.
- Handle high-frequency API requests with proper concurrency and error-handling techniques.

