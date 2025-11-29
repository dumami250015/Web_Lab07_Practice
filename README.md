# LAB 07: Product Management System

## Student Information
- **Name:** Võ Trí Khôi
- **Student ID:** ITCSIU24045
- **Class:** Group 2

## Technologies Used
- Spring Boot 3.3.x
- Spring Data JPA
- MySQL 8.0
- Thymeleaf
- Maven

## Setup Instructions
1. Import project into VS Code
2. Create database: `product_management`
3. Update `application.properties` with your MySQL credentials
4. Run: `mvn spring-boot:run`
5. Open browser: http://localhost:8080/products

## Completed Features
- [x] CRUD operations
- [x] Search functionality
- [x] Advanced search with filters
- [x] Validation
- [x] Sorting
- [ ] Pagination
- [ ] REST API (Bonus)

## Project Structure
```
product-management.zip
├── src/
│   ├── main/
│   │   ├── java/com/example/productmanagement/
│   │   │   ├── ProductManagementApplication.java
│   │   │   ├── entity/Product.java
│   │   │   ├── repository/ProductRepository.java
│   │   │   ├── service/
│   │   │   │   ├── ProductService.java
│   │   │   │   └── ProductServiceImpl.java
│   │   │   └── controller/ProductController.java
│   │   └── resources/
│   │       ├── application.properties
│   │       └── templates/
│   │           ├── product-list.html
│   │           └── product-form.html
├── pom.xml
└── README.md
```

# Code Flow
## List Products
   1. **User Request**: The user accesses the URL `/products/list`.
   2. **Controller Layer** (`ProductController.java`):
      - The `listProducts` method mapped to `@GetMapping("/list")` is invoked.
      - It calls `productService.findAll()`.
   3. **Service Layer** (`ProductServiceImpl.java`):
      - The `findAll()` method calls `productRepository.findAllByOrderByNameAsc()`.
   4. **Repository Layer** (`ProductRepository.java`):
      - The `findAllByOrderByNameAsc()` method executes a JPQL/SQL query to fetch all products sorted by name.
   5. **View Layer** (`product-list.html`):
      - The list of products is added to the `Model`.
      - The controller returns `"products/list-products"`.
      - Thymeleaf iterates over the list (`th:each`) and renders the table rows.

## Create Product
**Step A: Show Form**
   1. **User Request**: User clicks "Add Button", accessing `/products/showFormForAdd`.
   2. **Controller**: Creates a new `Product` object and adds it to the model.
   3. **View**: Renders `product-form.html` with an empty form bound to the new object.

**Step B: Save Product**
   1. **User Submission**: User fills the form and submits POST to `/products/save`.
   2. Controller:
      - Accepts the `@ModelAttribute("product")`.
      - Calls `productService.save(theProduct)`.
   3. **Service -> Repository**:
      - The service delegates to `productRepository.save(product)`.
      - Hibernate performs an `INSERT` statement.
   4. **Redirect**: The controller redirects the user back to the list (`redirect:/products/list`).

## Update Product
**Step A: Show Form with Data**
   1. **User Request**: User clicks "Update" on a product row (`/products/showFormForUpdate?productId=X`).
   2. **Controller**:
      - Extracts `productId` from the request param.
      - Calls `productService.findById(theId)`.
   3. **Service -> Repository**: Fetches the existing product from the DB.
   4. **View**: Renders `product-form.html`, but this time the fields are pre-filled with the existing product's data.

**Step B: Save Changes**
   1. **User Submission**: User modifies data and submits.
   2. **Controller**: The `saveProduct` method is called (same as Create).
   3. **Service**: Because the `Product` object has an `id` (hidden field in the form), `productRepository.save(product)` performs an `UPDATE` instead of an `INSERT`.

## Delete Product
   1. **User Request**: User clicks "Delete" on a product row (`/products/delete?productId=X`).
   2. **Controller**:
      - Extracts `productId`.
      - Calls `productService.deleteById(theId)`.
   3. **Service -> Repository**:
      - Calls `productRepository.deleteById(theId)`.
      - Hibernate executes a `DELETE` statement.
   4. **Redirect**: The user is redirected back to the product list.
