
## **Task Overview**

This task involves building two services:
1. **Caller Service**: Simulates a high-load scenario by making random requests to the wallet analysis endpoints.
2. **Wallet Service**: Processes wallet data from the provided JSON file, stores analyzed data in a database, and provides endpoints for querying and sorting wallet information.

### **General Instructions**
1. You will use the provided JSON file to create a database schema to store wallet data and their analysis.
2. Implement a Nest.js application with two primary services:
   - **Caller Service**: Simulates frequent requests to the Wallet Service.
   - **Wallet Service**: Reads, analyzes, stores, and provides an interface to interact with wallet data.
3. Use best practices for setting up services, creating APIs, and handling high-load scenarios.

---

## **Section 1: Wallet Service**
### **Step 1: Database Schema**
- Create a database schema (use PostgreSQL for this task) with the following table:

#### **Table: `wallets`**
| Column Name       | Data Type        | Description                                 |
|-------------------|------------------|---------------------------------------------|
| `id`              | `UUID`           | Unique identifier for the wallet (Primary Key). |
| `address`         | `VARCHAR(255)`   | The wallet's address.                      |
| `total_profit`    | `NUMERIC`        | The total profit/loss of the wallet.       |
| `num_tokens_traded` | `INTEGER`      | Number of different tokens traded by the wallet. |
| `num_active_days` | `INTEGER`        | Number of days with at least one transaction. |
| `max_profit_token`| `VARCHAR(255)`   | Name of the most profitable token for the wallet. |
| `min_profit_token`| `VARCHAR(255)`   | Name of the least profitable token for the wallet. |
| `avg_trade_volume`| `NUMERIC`        | Average transaction volume for the wallet. |
| `risk_assessment` | `VARCHAR(50)`    | Categorizes the risk (e.g., High, Medium, Low) based on token diversification and volatility. |
| `last_updated`    | `TIMESTAMP`      | Timestamp of the last data analysis update. |

### **Step 2: Data Ingestion**
- **Parse the JSON file** and populate the `wallets` table with relevant information. Ensure efficient handling of the large file, possibly using streaming or batching techniques to avoid memory overload.

### **Step 3: Endpoints**
Implement the following endpoints:

1. **`GET /wallets`**:
   - **Description**: Fetch a list of wallets with optional sorting and filtering.
   - **Query Parameters**:
     - `sort_by`: Can be `total_profit`, `num_tokens_traded`, `num_active_days`, `avg_trade_volume`.
     - `order`: `asc` or `desc`.
     - `filter_by`: Optional filters (e.g., `min_profit` to return wallets with profit above a threshold).
   - **Example**: `/wallets?sort_by=total_profit&order=desc`

2. **`GET /wallets/:id`**:
   - **Description**: Fetch detailed information for a specific wallet by its ID.
   - **Response**: Returns all characteristics of the wallet from the database.

3. **`POST /wallets/analyze`**:
   - **Description**: Trigger analysis of the JSON data to update wallet information in the database.
   - **Body Parameters**: None. The service should automatically read from the JSON file.
   - **Response**: Returns a status message confirming the analysis and update of the database.
   
4. **`GET /wallets/top-tokens/:id`**:
   - **Description**: Get a list of the top 3 most profitable and least profitable tokens for a given wallet.
   - **Parameters**: Wallet `id`.
   - **Response**: JSON containing details of the tokens' profit/loss.

5. **`GET /wallets/stats`**:
   - **Description**: Retrieve statistics about wallets (e.g., number of wallets with high risk, average profit across all wallets).
   - **Response**: JSON with key statistics.

### **Step 4: Data Analysis**
- Implement logic in the `POST /wallets/analyze` endpoint to:
  - Calculate each wallet's total profit/loss.
  - Determine the most and least profitable tokens.
  - Count the number of different tokens traded and active trading days.
  - Assess risk based on token diversification and market volatility.
- Store the results in the database for quick retrieval.

---

## **Section 2: Caller Service**
### **Objective**
Simulate real-world high-load conditions by calling the Wallet Service endpoints randomly and logging responses.

### **Step 1: Implementation**
- Create a `CallerService` that performs the following actions:
  - Uses an interval or cron job to make **50 random requests per minute** to the Wallet Service.
  - Randomly choose from the following endpoints:
    - `/wallets` with random sorting parameters.
    - `/wallets/:id` using randomly selected wallet IDs.
    - `/wallets/top-tokens/:id` for a random wallet ID.
  - Log the success and failure of each request, including the latency of each response.

### **Step 2: Concurrency and Error Handling**
- The service should handle concurrency efficiently using techniques like asynchronous operations (e.g., `async/await`).
- Implement retry logic for failed requests and handle timeouts gracefully.

### **Step 3: Testing**
- Write unit tests for key functionalities, including database operations, data analysis, and API endpoints.

---

## **Optional Bonus Tasks**
1. **Caching**: Implement caching for wallet data to minimize database reads and improve response times.
2. **Scalability Discussion**: Include a section in this `README.md` on how you would scale this system to handle higher traffic or larger data volumes.
3. **Security**: Add JWT-based authentication for the Wallet Service endpoints.

---

## **Submission Guidelines**
1. Implement the solution as a Node.js Nest.js application with the above services and endpoints.
2. Structure your project with clear separation between services (`CallerService` and `WalletService`).
3. Include a `README.md` with instructions for setting up, running, and testing the application.
4. Compress the project folder (excluding large data files) and email it, or provide a downloadable link for the entire project.

---

By completing this task, you will demonstrate your ability to:
- Design and implement efficient data processing for large JSON files.
- Create a robust database schema for storing and querying analytical data.
- Develop scalable and reliable API endpoints in a Nest.js application.
- Handle high-frequency API requests with proper concurrency and error-handling techniques.
