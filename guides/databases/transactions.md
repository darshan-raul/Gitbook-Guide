# Transactions

#### Database Transactions

**Definition:** A database transaction is a sequence of one or more SQL operations (queries or updates) executed as a single unit of work. Transactions ensure that a series of operations either all succeed or all fail, maintaining the integrity and consistency of the database.

#### Key Concepts

1. **ACID Properties:**
   * **Atomicity:** Ensures that all operations within a transaction are completed successfully. If any operation fails, the transaction is aborted, and all changes are rolled back.
   * **Consistency:** Ensures that the database transitions from one valid state to another valid state, maintaining database invariants.
   * **Isolation:** Ensures that transactions are executed in isolation from one another, preventing concurrent transactions from interfering with each other.
   * **Durability:** Ensures that once a transaction is committed, its changes are permanent, even in the event of a system failure.
2.  **Transaction Control Statements:**

    * **BEGIN:** Starts a new transaction.
    * **COMMIT:** Saves all changes made during the transaction.
    * **ROLLBACK:** Undoes all changes made during the transaction.

    **Example:**

    ```sql
    BEGIN;
    INSERT INTO accounts (account_id, balance) VALUES (1, 100);
    UPDATE accounts SET balance = balance - 50 WHERE account_id = 1;
    COMMIT;
    ```

#### Difference Between Transactions and Queries

* **Transactions:** A collection of one or more SQL startements executed as a single unit. Transactions ensure ACID properties, providing reliability and consistency.
* **Queries:** Individual SQL statements that retrieve or manipulate data. Queries do not provide the guarantees of atomicity, consistency, isolation, and durability on their own.

#### Using Transactions in a Payment Gateway Microservices Architecture

In a payment gateway microservices architecture, transactions play a crucial role in ensuring data consistency and integrity across multiple operations. Here's how transactions can be used in this context:

1.  **Initiating a Payment:**

    * Begin a transaction when a user initiates a payment.
    * Verify the user’s account balance and other necessary details.

    **Example:**

    ```sql
    BEGIN;
    SELECT balance FROM accounts WHERE account_id = 1 FOR UPDATE;
    ```
2.  **Debiting the User’s Account:**

    * Deduct the payment amount from the user's account.
    * Ensure that the balance update is part of the same transaction to maintain consistency.

    **Example:**

    ```sql
    UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
    ```
3.  **Crediting the Merchant’s Account:**

    * Add the payment amount to the merchant’s account.
    * This step is also part of the same transaction to ensure atomicity.

    **Example:**

    ```sql
    UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
    ```
4.  **Logging the Transaction:**

    * Insert a record of the transaction into a transactions table.
    * This ensures a consistent audit trail.

    **Example:**

    ```sql
    INSERT INTO transactions (transaction_id, from_account, to_account, amount) VALUES (1, 1, 2, 100);
    ```
5.  **Committing the Transaction:**

    * If all operations succeed, commit the transaction to save changes.
    * If any operation fails, roll back the transaction to undo all changes.

    **Example:**

    ```sql
    COMMIT;
    ```
6.  **Handling Failures:**

    * If an error occurs at any step, roll back the transaction to maintain data integrity.

    **Example:**

    ```sql
    ROLLBACK;
    ```

#### Example: Pseudo Code for Payment Service

Here’s a pseudo code example to illustrate how transactions might be used in a payment service within a microservices architecture:

```python
def process_payment(from_account_id, to_account_id, amount):
    try:
        # Start a new transaction
        connection.begin()
        
        # Check balance of the from_account
        from_balance = connection.execute(
            "SELECT balance FROM accounts WHERE account_id = %s FOR UPDATE", (from_account_id,)
        ).fetchone()
        
        if from_balance < amount:
            raise Exception("Insufficient funds")

        # Debit the from_account
        connection.execute(
            "UPDATE accounts SET balance = balance - %s WHERE account_id = %s", (amount, from_account_id)
        )

        # Credit the to_account
        connection.execute(
            "UPDATE accounts SET balance = balance + %s WHERE account_id = %s", (amount, to_account_id)
        )

        # Log the transaction
        connection.execute(
            "INSERT INTO transactions (from_account, to_account, amount) VALUES (%s, %s, %s)",
            (from_account_id, to_account_id, amount)
        )

        # Commit the transaction
        connection.commit()
        return "Payment processed successfully"

    except Exception as e:
        # Roll back the transaction in case of error
        connection.rollback()
        return f"Payment failed: {str(e)}"
```

#### Conclusion

Database transactions are essential for ensuring data integrity and consistency in multi-step operations. They are especially crucial in payment gateway microservices architectures, where financial operations must be accurate and reliable. By using transactions, you can ensure that complex sequences of operations either complete successfully or have no effect, maintaining the robustness of your application.
