# TODO: Switch to MongoDB - Project Refactor Steps

## Overview
This TODO tracks the steps to refactor the database layer from SQL Server (JDBC) to MongoDB, keeping all other code (models, controllers, views) unchanged. Only DAOs and DBContext will be modified. MongoDB collections will mirror SQL tables (e.g., 'users' collection for Users table). Relationships (e.g., foreign keys) will use embedded documents or ObjectIds where appropriate.

## Steps

### 1. Update Dependencies
- [x] Edit `nbproject/project.properties` to add MongoDB driver to javac.classpath: `${file.reference.mongodb-driver-sync.jar}`.
- [x] Remove or comment out SQL Server JDBC references (e.g., sqljdbc4.jar) from classpath to avoid conflicts.
- [x] Verify JAR is in `library/mongodb-driver-sync.jar`.

### 2. Refactor DBContext.java
- [x] Update `src/java/clothingstore/utils/DBContext.java` to use MongoDB Java driver.
  - Import MongoDB classes (MongoClient, MongoDatabase, MongoCollection).
  - Change `getConnection()` to `getDatabase()` returning MongoDatabase.
  - Connection string: mongodb://localhost:27017/ClothesShop (default local setup).
  - No username/password for local dev; add if needed later.

### 3. Refactor DAOs
Each DAO will replace SQL (PreparedStatement, ResultSet) with MongoDB operations (find, insertOne, updateOne, deleteOne). Use DTOs for data mapping. Handle queries with filters (e.g., eq for =, in for arrays).

- [x] `src/java/clothingstore/dao/UserDAO.java`
  - Adapt methods: checkLogin, getUser, insertUser, updateUser, getAllUsers, etc.
  - Collections: 'users'.

- [x] `src/java/clothingstore/dao/ProductDAO.java`
  - Adapt: getAllProducts, getProductsByCategory, insertProduct, updateProduct, deleteProduct, searchProducts.
  - Collections: 'products' (embed category/supplier refs as ObjectIds or strings).

- [x] `src/java/clothingstore/dao/CategoryDAO.java`
  - Adapt: getAllCategories, insertCategory, updateCategory, deleteCategory.
  - Collections: 'categories'.

- [x] `src/java/clothingstore/dao/SupplierDAO.java`
  - Adapt: getAllSuppliers, insertSupplier, updateSupplier, deleteSupplier.
  - Collections: 'suppliers'.

- [x] `src/java/clothingstore/dao/TypeDAO.java`
  - Adapt: getAllTypes, insertType, updateType, deleteType.
  - Collections: 'types'.

- [x] `src/java/clothingstore/dao/OrderDAO.java`
  - Adapt: getOrdersByUser, insertOrder, updateOrderStatus, getAllOrders.
  - Collections: 'orders'.

- [x] `src/java/clothingstore/dao/OrderItemDAO.java`
  - Adapt: insertOrderItem, getOrderItemsByOrder.
  - Collections: 'orderItems' (or embed in orders).

- [x] `src/java/clothingstore/dao/PaymentDAO.java`
  - Adapt: getAllPayments, insertPayment.
  - Collections: 'payments'.

### 4. Handle Relationships
- [ ] In DAOs, use ObjectId for foreign keys (e.g., product.categoryId as ObjectId ref to categories._id).
- [ ] Update DTOs if needed (minimal changes; add _id fields).

### 5. MongoDB Setup and Data Migration
- [ ] Instructions: Install MongoDB Community Edition, start mongod service.
- [ ] Create database 'ClothesShop' and collections.
- [ ] Provide a simple Java migration script or manual insert script to load sample data from data.sql.
- [ ] Test: Build project (`ant clean build`), deploy to Tomcat, verify app runs without SQL errors.

### 6. Testing
- [ ] After each DAO refactor, build and test specific features (e.g., login for UserDAO).
- [ ] Full test: Run app, perform CRUD on users/products/orders.

### 7. Cleanup
- [ ] Remove SQL scripts (createtable.sql, data.sql) or archive them.
- [ ] Update README.md with MongoDB instructions.

Progress: Starting with Step 1.
