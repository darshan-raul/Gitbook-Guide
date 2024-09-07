# MongoDB

{% embed url="https://www.mongodb.com" %}



Basic concepts of MongoDB:

#### 1. **Document**

* **Definition**: The basic unit of data in MongoDB, similar to a row in a relational database.
* **Format**: BSON (Binary JSON), which allows for embedded documents and arrays.

{% content-ref url="bson.md" %}
[bson.md](bson.md)
{% endcontent-ref %}

*   **Example**:

    ```json
    {
      "_id": ObjectId("507f191e810c19729de860ea"),
      "name": "John Doe",
      "age": 29,
      "address": {
        "street": "123 Main St",
        "city": "New York",
        "zip": "10001"
      },
      "interests": ["reading", "traveling", "coding"]
    }
    ```

#### 2. **Collection**

* **Definition**: A group of documents, similar to a table in a relational database.
* **Characteristics**: Collections do not enforce a schema, allowing for flexibility in the structure of documents.
* **Example**: A `users` collection could contain documents representing different users.

#### 3. **Database**

* **Definition**: A container for collections, similar to a database in a relational system.
* **Characteristics**: Each database has its own set of collections and is identified by a unique name.
* **Example**: A `shopDB` database could contain collections like `products`, `orders`, and `customers`.

#### 4. **Index**

* **Definition**: A special data structure that improves the speed of query operations.
* **Types**: Single field, compound (multiple fields), multikey (array fields), text, geospatial, and more.
*   **Example**: Creating an index on the `email` field of a `users` collection:

    ```javascript
    db.users.createIndex({ email: 1 });
    ```

#### 5. **Replica Set**

* **Definition**: A group of MongoDB servers that maintain the same data set, providing redundancy and high availability.
* **Components**: Primary (accepts writes), Secondary (replicates data), and optionally an Arbiter (votes in elections).
* **Example**: A replica set with one primary and two secondary members.

#### 6. **Sharding**

* **Definition**: The process of distributing data across multiple servers or clusters to support large data sets and high-throughput operations.
* **Components**: Shard (data subset), Config Server (metadata and config), and Query Router (routes queries to appropriate shards).
* **Example**: A sharded cluster with three shards, each holding a portion of the `orders` collection.

#### 7. **Aggregation**

* **Definition**: A framework for processing data and transforming it into aggregated results.
* **Stages**: `$match`, `$group`, `$sort`, `$project`, `$lookup`, and more.
*   **Example**: Aggregating sales data to calculate total sales per product:

    ```javascript
    db.sales.aggregate([
      { $match: { status: "completed" } },
      { $group: { _id: "$productId", totalSales: { $sum: "$amount" } } },
      { $sort: { totalSales: -1 } }
    ]);
    ```

#### 8. **CRUD Operations**

*   **Create**: Insert new documents into a collection.

    ```javascript
    db.users.insertOne({ name: "Alice", age: 25 });
    db.users.insertMany([{ name: "Bob", age: 30 }, { name: "Carol", age: 28 }]);
    ```
*   **Read**: Query documents from a collection.

    ```javascript
    db.users.find({ age: { $gt: 20 } });
    db.users.findOne({ name: "Alice" });
    ```
*   **Update**: Modify existing documents in a collection.

    ```javascript
    db.users.updateOne({ name: "Alice" }, { $set: { age: 26 } });
    db.users.updateMany({ age: { $gt: 25 } }, { $inc: { age: 1 } });
    ```
*   **Delete**: Remove documents from a collection.

    ```javascript
    db.users.deleteOne({ name: "Alice" });
    db.users.deleteMany({ age: { $lt: 25 } });
    ```

#### 9. **Schema Design**

* **Flexibility**: MongoDB allows for a flexible schema design, meaning documents in the same collection can have different fields.
* **Best Practices**: Embed data when you have one-to-one or one-to-many relationships and reference data when you have many-to-many relationships.

#### 10. **Transactions**

* **Definition**: MongoDB supports multi-document transactions to maintain ACID (Atomicity, Consistency, Isolation, Durability) properties.
* **Use**: Useful for operations that need to be atomic across multiple documents or collections.
*   **Example**:

    ```javascript
    const session = client.startSession();
    session.startTransaction();
    try {
      db.users.updateOne({ name: "Alice" }, { $set: { age: 27 } }, { session });
      db.orders.insertOne({ userId: aliceId, product: "Book", amount: 30 }, { session });
      session.commitTransaction();
    } catch (error) {
      session.abortTransaction();
    } finally {
      session.endSession();
    }
    ```

#### 11. **MongoDB Atlas**

* **Definition**: MongoDB's fully-managed database-as-a-service offering.
* **Features**: Automated backups, monitoring, scaling, and support for global clusters.

#### Conclusion

MongoDB is a flexible, scalable, and powerful NoSQL database that supports a wide range of use cases through its document-oriented storage, rich query language, and robust data management features. Understanding these basic concepts will help you effectively design and interact with MongoDB databases.
