### **Task Overview**

The objective of this task is to develop two services: **Caller Service** and **Wallet Service**, using the provided JSON data. The project must include database schema creation, API endpoints, inter-service communication through **RabbitMQ**, environment configuration, and basic logging and error-handling mechanisms. The candidate should also provide explanations for any scalability considerations used in the project.

### **Project Requirements**

#### **1. Database Setup**
- **Schema**: Use **Sequelize** to create a database schema that models the data from the provided JSON file.
- The schema should capture key characteristics of each wallet, including but not limited to:
  - Wallet ID
  - Total profit
  - Number of tokens traded
  - Number of active days
  - Minimum and maximum profit/loss
- The database should allow storing and retrieving analyzed data for the wallet service.
  
#### **2. Wallet Service**
- Create a service that provides analysis and information about wallets, with the following endpoints:
  - **GET /wallets**: Fetch a list of wallets with sorting options such as:
    - Total profit
    - Number of tokens traded
    - Number of active days
    - Maximum profit
    - Minimum profit
    - Number of transactions
  - **GET /wallets/:id**: Fetch detailed information about a specific wallet.
- Use **Sequelize** to handle database interactions.
- Implement **basic logging** to monitor incoming requests and errors.
- **Environment Configuration**: Use environment variables for configuration (e.g., database credentials, RabbitMQ settings).
- Implement **simple error handling** to handle database and service-related issues.

#### **3. Caller Service**
- This service randomly calls the wallet service endpoints, simulating external usage.
- Use **RabbitMQ** for inter-service communication:
  - Set up RabbitMQ to queue requests from the caller service to the wallet service.
  - Use asynchronous messaging to improve system reliability and allow the wallet service to handle multiple requests efficiently.
- **Implementation Details**:
  - Configure **message acknowledgments** to ensure messages are processed successfully.
  - Use **persistent messages** in RabbitMQ to maintain message integrity.
  - Implement basic **retry mechanisms** to handle failed messages and ensure reliability.

#### **4. Scalability**
- Implement solutions to ensure the services are scalable. Document the scalability solutions used in the `README.md`, explaining how the system can handle high loads. Considerations may include:
  - Using RabbitMQ for asynchronous processing to avoid overloading the wallet service with synchronous requests.
  - Using **environment-based configurations** to easily adjust settings for different environments (development, production).
  - **Database indexing** to improve query performance for sorting and filtering.
- (Optional) Introduce a **caching mechanism** (e.g., using Redis) to store frequently accessed wallet data, reducing database load and improving response times.

#### **5. Environment Configuration**
- All settings (RabbitMQ host, port, credentials, database credentials) must be configured using environment variables.
- Provide a `.env.example` file for reference, listing all the environment variables required for the project.

#### **6. Logging and Error Handling**
- Use simple logging (e.g., **console logging**, **Winston**, or **Pino**) to log API requests, responses, and errors.
- Implement centralized error handling in both services to catch and log errors gracefully.

### **Optional Bonus: Dockerization**
- **Docker**: Containerize both the **Caller Service** and **Wallet Service** using Docker.
- Use a `docker-compose` file to set up a multi-container application, including:
  - **Wallet Service**
  - **Caller Service**
  - **RabbitMQ**
  - Any additional services (e.g., **Database**, **Redis**)

### **Submission Instructions**
- Upload your solution to a GitHub repository.
- Include a `README.md` file with:
  - Clear instructions for setting up and running the project.
  - An explanation of the database schema and any endpoints created.
  - A detailed description of the scalability solutions implemented.
  - Instructions for setting up environment configurations using the `.env.example` file.
  - (Optional) Details of the Docker setup, if implemented. 

### **Additional Notes**
- Use **RabbitMQ** for inter-service communication, focusing on asynchronous processing to enhance scalability.
- Use **Sequelize** as the ORM for database interactions.
- Implement basic logging and error handling throughout the project.
- The project should be structured and coded to a production-level quality, using best practices in service communication, error handling, and performance optimization.
