# 🍔 Food Delivery Application - Backend

A full-featured **Food Delivery REST API** built with **Spring Boot**, **Spring Security (JWT)**, **Hibernate/JPA**, and **MySQL**. This backend supports customer registration, restaurant/menu management, order placement, cart operations, payments, and delivery tracking.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Java 17 | Core language |
| Spring Boot 3.x | Application framework |
| Spring Security + JWT | Authentication & Authorization |
| Spring Data JPA / Hibernate | ORM & database operations |
| MySQL | Relational database |
| Maven | Build tool |
| Lombok | Boilerplate reduction |
| ModelMapper | DTO ↔ Entity mapping |
| BCryptPasswordEncoder | Password hashing |

---

## 📁 Project Structure

```
food_delivery_application/
│
├── src/main/java/com/manish/
│   ├── FoodDeliveryApplication.java          # Main entry point
│   │
│   ├── Config/
│   │   ├── SecurityConfig.java               # Spring Security config + JWT filter
│   │   └── AppConfig.java                    # ModelMapper, BCrypt beans
│   │
│   ├── controller/
│   │   ├── AuthController.java               # Login, Register
│   │   ├── UserController.java               # User profile
│   │   ├── RestaurantController.java         # Restaurant CRUD
│   │   ├── MenuController.java               # Menu items
│   │   ├── CartController.java               # Cart operations
│   │   ├── OrderController.java              # Order management
│   │   ├── PaymentController.java            # Payment processing
│   │   └── DeliveryController.java           # Delivery tracking
│   │
│   ├── DTO/
│   │   ├── Request/
│   │   │   ├── LoginRequest.java
│   │   │   ├── RegisterRequest.java
│   │   │   ├── RestaurantRequest.java
│   │   │   ├── MenuItemRequest.java
│   │   │   ├── CartRequest.java
│   │   │   ├── OrderRequest.java
│   │   │   └── PaymentRequest.java
│   │   └── Response/
│   │       ├── AuthResponse.java
│   │       ├── UserResponse.java
│   │       ├── RestaurantResponse.java
│   │       ├── MenuItemResponse.java
│   │       ├── CartResponse.java
│   │       ├── OrderResponse.java
│   │       └── ApiResponse.java
│   │
│   ├── entity/
│   │   ├── User.java
│   │   ├── Restaurant.java
│   │   ├── MenuItem.java
│   │   ├── Cart.java
│   │   ├── CartItem.java
│   │   ├── Order.java
│   │   ├── OrderItem.java
│   │   ├── Payment.java
│   │   └── Delivery.java
│   │
│   ├── exception/
│   │   ├── GlobalExceptionHandler.java
│   │   ├── ResourceNotFoundException.java
│   │   ├── UnauthorizedException.java
│   │   └── BadRequestException.java
│   │
│   ├── repository/
│   │   ├── UserRepository.java
│   │   ├── RestaurantRepository.java
│   │   ├── MenuItemRepository.java
│   │   ├── CartRepository.java
│   │   ├── OrderRepository.java
│   │   ├── PaymentRepository.java
│   │   └── DeliveryRepository.java
│   │
│   ├── security/
│   │   ├── JwtTokenProvider.java
│   │   ├── JwtAuthenticationFilter.java
│   │   └── CustomUserDetailsService.java
│   │
│   ├── service/
│   │   ├── AuthService.java
│   │   ├── UserService.java
│   │   ├── RestaurantService.java
│   │   ├── MenuService.java
│   │   ├── CartService.java
│   │   ├── OrderService.java
│   │   ├── PaymentService.java
│   │   └── DeliveryService.java
│   │
│   └── serviceImpl/
│       ├── AuthServiceImpl.java
│       ├── UserServiceImpl.java
│       ├── RestaurantServiceImpl.java
│       ├── MenuServiceImpl.java
│       ├── CartServiceImpl.java
│       ├── OrderServiceImpl.java
│       ├── PaymentServiceImpl.java
│       └── DeliveryServiceImpl.java
│
├── src/main/resources/
│   ├── application.properties
│   └── application-dev.properties
│
├── pom.xml
└── README.md
```

---

## 🗄️ Database Tables

### `users`
| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| name | VARCHAR(100) | NOT NULL |
| email | VARCHAR(100) | UNIQUE, NOT NULL |
| password | VARCHAR(255) | NOT NULL |
| phone | VARCHAR(15) | |
| role | ENUM('CUSTOMER','ADMIN','RESTAURANT_OWNER','DELIVERY_AGENT') | NOT NULL |
| address | TEXT | |
| created_at | TIMESTAMP | DEFAULT NOW() |

### `restaurants`
| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| name | VARCHAR(150) | NOT NULL |
| address | TEXT | NOT NULL |
| cuisine_type | VARCHAR(100) | |
| contact_number | VARCHAR(15) | |
| owner_id | BIGINT | FK → users(id) |
| is_active | BOOLEAN | DEFAULT TRUE |
| rating | DOUBLE | DEFAULT 0.0 |
| created_at | TIMESTAMP | |

### `menu_items`
| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| restaurant_id | BIGINT | FK → restaurants(id) |
| name | VARCHAR(150) | NOT NULL |
| description | TEXT | |
| price | DECIMAL(10,2) | NOT NULL |
| category | VARCHAR(100) | |
| is_available | BOOLEAN | DEFAULT TRUE |
| image_url | VARCHAR(255) | |

### `carts`
| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| user_id | BIGINT | FK → users(id) |
| restaurant_id | BIGINT | FK → restaurants(id) |
| created_at | TIMESTAMP | |

### `cart_items`
| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| cart_id | BIGINT | FK → carts(id) |
| menu_item_id | BIGINT | FK → menu_items(id) |
| quantity | INT | NOT NULL |
| price | DECIMAL(10,2) | NOT NULL |

### `orders`
| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| user_id | BIGINT | FK → users(id) |
| restaurant_id | BIGINT | FK → restaurants(id) |
| total_amount | DECIMAL(10,2) | NOT NULL |
| status | ENUM('PENDING','CONFIRMED','PREPARING','OUT_FOR_DELIVERY','DELIVERED','CANCELLED') | |
| delivery_address | TEXT | NOT NULL |
| created_at | TIMESTAMP | |
| updated_at | TIMESTAMP | |

### `order_items`
| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| order_id | BIGINT | FK → orders(id) |
| menu_item_id | BIGINT | FK → menu_items(id) |
| quantity | INT | |
| price | DECIMAL(10,2) | |

### `payments`
| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| order_id | BIGINT | FK → orders(id) |
| payment_method | ENUM('CASH','UPI','CARD','WALLET') | |
| payment_status | ENUM('PENDING','SUCCESS','FAILED','REFUNDED') | |
| transaction_id | VARCHAR(100) | UNIQUE |
| amount | DECIMAL(10,2) | |
| paid_at | TIMESTAMP | |

### `deliveries`
| Column | Type | Constraints |
|---|---|---|
| id | BIGINT | PK, AUTO_INCREMENT |
| order_id | BIGINT | FK → orders(id) |
| agent_id | BIGINT | FK → users(id) |
| status | ENUM('ASSIGNED','PICKED_UP','ON_THE_WAY','DELIVERED') | |
| estimated_time | INT | (minutes) |
| delivered_at | TIMESTAMP | |

---

## 🔐 Security

- JWT-based stateless authentication
- Roles: `CUSTOMER`, `ADMIN`, `RESTAURANT_OWNER`, `DELIVERY_AGENT`
- Passwords encrypted using **BCrypt**
- All endpoints (except `/api/auth/**`) require a valid Bearer token

**Authorization Header:**
```
Authorization: Bearer <JWT_TOKEN>
```

---

## 🌐 API Endpoints

### 🔑 Auth (`/api/auth`)

| Method | Endpoint | Description | Access |
|---|---|---|---|
| POST | `/api/auth/register` | Register new user | Public |
| POST | `/api/auth/login` | Login and get JWT | Public |

### 👤 User (`/api/users`)

| Method | Endpoint | Description | Access |
|---|---|---|---|
| GET | `/api/users/profile` | Get own profile | CUSTOMER, ADMIN |
| PUT | `/api/users/profile` | Update profile | CUSTOMER |
| GET | `/api/users` | Get all users | ADMIN |
| DELETE | `/api/users/{id}` | Delete user | ADMIN |

### 🏪 Restaurant (`/api/restaurants`)

| Method | Endpoint | Description | Access |
|---|---|---|---|
| GET | `/api/restaurants` | Get all restaurants | Public |
| GET | `/api/restaurants/{id}` | Get restaurant by ID | Public |
| POST | `/api/restaurants` | Create restaurant | RESTAURANT_OWNER, ADMIN |
| PUT | `/api/restaurants/{id}` | Update restaurant | RESTAURANT_OWNER |
| DELETE | `/api/restaurants/{id}` | Delete restaurant | ADMIN |

### 🍽️ Menu (`/api/restaurants/{restaurantId}/menu`)

| Method | Endpoint | Description | Access |
|---|---|---|---|
| GET | `/api/restaurants/{id}/menu` | Get menu items | Public |
| POST | `/api/restaurants/{id}/menu` | Add menu item | RESTAURANT_OWNER |
| PUT | `/api/restaurants/{id}/menu/{itemId}` | Update item | RESTAURANT_OWNER |
| DELETE | `/api/restaurants/{id}/menu/{itemId}` | Delete item | RESTAURANT_OWNER |

### 🛒 Cart (`/api/cart`)

| Method | Endpoint | Description | Access |
|---|---|---|---|
| GET | `/api/cart` | View cart | CUSTOMER |
| POST | `/api/cart/add` | Add item to cart | CUSTOMER |
| PUT | `/api/cart/update/{itemId}` | Update item quantity | CUSTOMER |
| DELETE | `/api/cart/remove/{itemId}` | Remove item | CUSTOMER |
| DELETE | `/api/cart/clear` | Clear cart | CUSTOMER |

### 📦 Orders (`/api/orders`)

| Method | Endpoint | Description | Access |
|---|---|---|---|
| GET | `/api/orders` | Get all orders | ADMIN |
| GET | `/api/orders/my` | Get own orders | CUSTOMER |
| GET | `/api/orders/{id}` | Get order by ID | CUSTOMER, ADMIN |
| POST | `/api/orders/place` | Place order | CUSTOMER |
| PUT | `/api/orders/{id}/status` | Update order status | RESTAURANT_OWNER, ADMIN |
| DELETE | `/api/orders/{id}/cancel` | Cancel order | CUSTOMER |

### 💳 Payment (`/api/payments`)

| Method | Endpoint | Description | Access |
|---|---|---|---|
| POST | `/api/payments/initiate` | Initiate payment | CUSTOMER |
| GET | `/api/payments/{orderId}` | Get payment details | CUSTOMER, ADMIN |
| PUT | `/api/payments/{id}/status` | Update payment status | ADMIN |

### 🚴 Delivery (`/api/delivery`)

| Method | Endpoint | Description | Access |
|---|---|---|---|
| GET | `/api/delivery/{orderId}` | Track delivery | CUSTOMER |
| POST | `/api/delivery/assign` | Assign agent | ADMIN |
| PUT | `/api/delivery/{id}/status` | Update delivery status | DELIVERY_AGENT |

---

## 📨 Sample JSON Payloads

### Register
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "phone": "9876543210",
  "role": "CUSTOMER",
  "address": "123 Main Street, Hyderabad"
}
```

### Login
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

### Login Response
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "tokenType": "Bearer",
  "userId": 1,
  "email": "john@example.com",
  "role": "CUSTOMER"
}
```

### Create Restaurant
```json
{
  "name": "Spice Garden",
  "address": "456 Food Street, Hyderabad",
  "cuisineType": "Indian",
  "contactNumber": "9000012345"
}
```

### Add Menu Item
```json
{
  "name": "Paneer Butter Masala",
  "description": "Rich and creamy paneer curry",
  "price": 220.00,
  "category": "Main Course",
  "isAvailable": true,
  "imageUrl": "https://example.com/images/paneer.jpg"
}
```

### Add to Cart
```json
{
  "menuItemId": 3,
  "restaurantId": 1,
  "quantity": 2
}
```

### Place Order
```json
{
  "restaurantId": 1,
  "deliveryAddress": "789 Delivery Lane, Hyderabad",
  "items": [
    { "menuItemId": 3, "quantity": 2 },
    { "menuItemId": 5, "quantity": 1 }
  ]
}
```

### Order Response
```json
{
  "orderId": 101,
  "status": "CONFIRMED",
  "totalAmount": 660.00,
  "deliveryAddress": "789 Delivery Lane, Hyderabad",
  "restaurantName": "Spice Garden",
  "items": [
    { "itemName": "Paneer Butter Masala", "quantity": 2, "price": 220.00 },
    { "itemName": "Garlic Naan", "quantity": 1, "price": 50.00 }
  ],
  "createdAt": "2024-06-11T10:30:00"
}
```

### Initiate Payment
```json
{
  "orderId": 101,
  "paymentMethod": "UPI",
  "amount": 660.00
}
```

---

## ⚙️ Configuration (`application.properties`)

```properties
# Server
server.port=8080

# Database
spring.datasource.url=jdbc:mysql://localhost:3306/food_delivery_db
spring.datasource.username=root
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect

# JWT
jwt.secret=your_jwt_secret_key_here
jwt.expiration=86400000
```

---

## 🚀 Getting Started

### Prerequisites
- Java 17+
- MySQL 8+
- Maven 3.8+

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/your-username/food-delivery-application.git
cd food-delivery-application

# 2. Create MySQL database
CREATE DATABASE food_delivery_db;

# 3. Update application.properties with your DB credentials

# 4. Build and run
mvn clean install
mvn spring-boot:run
```

API will be available at: `http://localhost:8080`

---

## 👨‍💻 Author

**Manish**  
GitHub: [@your-username](https://github.com/your-username)

---

## 📄 License

This project is licensed under the MIT License.
