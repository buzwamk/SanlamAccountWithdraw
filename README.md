I chose to use Spring Boot with an MVC-style layered structure to modularize the withdrawal functionality and separate responsibilities into clear components.
This makes the code easier to read, maintain, and extend.

The solution is built around these main layers:

1. Controller layer to exposes the REST API endpoint for the withdrawal request and Its responsibility is to receive the client request and we keep client request separate from business logic.
2. Service layer contains the core business logic for the withdrawal funcatinality ,Its responsibility is to validate the request, call data layer to determine the result of the transaction and publish a withdrawal event when the transaction succeeds
3. Repository layer handles database interaction using JDBC / JdbcTemplate and focused only on data access, Its responsibility is to execute the SQL statement and keeps database logic isolated from the rest of the application.
4. Event publishing component is to show how the system can notify other systems or services after a successful withdrawal.

Below is an architecture  diagram that explain the solution above :
Client
  |
  | HTTP POST /bank/withdraw
  v
+---------------------------+
|   BankAccountController   |
|  Handles API request      |
+-------------+-------------+
              |
              v
+---------------------------+
|    BankAccountService     |
|  Business Logic Layer     |
|  - Validate request       |
|  - Execute withdrawal     |
|  - Publish event          |
+------+--------------------+
		|		          |
					      v		
		|			+---------------------------+
        |					| WithdrawalEventPublisher  |
					|   Event Notification      |
		|			|   (e.g., AWS SNS)         |
		|			+---------------------------+
        v
+---------------------------+
|  BankAccountRepository    |
|  JDBC / SQL operations    |
+-------------+-------------+
              |
              v
+---------------------------+
|        Database           |
|      Accounts Table       |
+---------------------------+

