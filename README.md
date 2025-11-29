# ☕ CoffeeShop - Smart Order Management System

[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.8-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://www.oracle.com/java/)
[![React](https://img.shields.io/badge/React-18-blue.svg)](https://reactjs.org/)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](https://www.docker.com/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> Modern coffee shop management system demonstrating **7 Design Patterns** with a beautiful React frontend. Features smart customization rules, real-time pricing, and production-ready Docker deployment.

**🌐 Live Demo:** [https://coffeeshop-website.onrender.com](https://coffeeshop-website.onrender.com/)

**📚 Based on:** [Original CoffeeShop Console Project](https://github.com/commedeschamps/CoffeeShop) - Extended with REST API and modern web interface.

---

## 🛠️ Technology Stack

### Backend
- **Spring Boot 3.5.8** - Enterprise Java framework
- **Java 17** - Modern Java with enhanced features
- **Maven** - Dependency management and build tool
- **Jackson** - JSON serialization/deserialization

### Frontend
- **React 18** - Modern UI library
- **Vite** - Lightning-fast build tool
- **CSS3** - Custom styling with gradients and animations
- **Fetch API** - REST API communication

### DevOps
- **Docker** - Containerization with multi-stage builds
- **Docker Compose** - Multi-container orchestration
- **Render** - Cloud deployment platform
- **Git/GitHub** - Version control and CI/CD

## 🎯 Key Features

### 🎨 Smart Customization Engine
- **Topping Restrictions** - Each beverage has specific allowed customizations
- **Dynamic Pricing** - Real-time calculation with decorators
- **Visual UI** - Product cards with images from Unsplash
- **Mobile Responsive** - Works perfectly on all devices

### 🏗️ Enterprise Architecture
- **7 Design Patterns** - Facade, Builder, Decorator, Factory, Strategy, Observer, Adapter
- **RESTful API** - Clean endpoints with DTO validation
- **Multi-stage Docker** - Optimized images (~250MB)
- **Health Checks** - Production-ready monitoring

### 📦 Rich Product Catalog
- **10 Beverages** - Espresso, Latte, Cappuccino, Teas, Hot Chocolate
- **4 Desserts** - Cheesecake, Brownie, Muffin, Croissant
- **3 Meals** - Sandwich, Salad, Lunchbox
- **Seasonal Items** - Pumpkin Macchiato, Ginger Tea, Cinnamon Roll

---

## 🚀 Quick Start

### Prerequisites
- **Java 17** or higher
- **Maven 3.6+**
- **Node.js 16+** (for frontend)
- **Docker** (optional)

### 🔥 Run with Docker (Fastest)

```bash
git clone https://github.com/commedeschamps/coffeeshop-backend.git
cd coffeeshop-backend
docker-compose up --build
```

**Backend:** http://localhost:8080  
**API Docs:** See [API Reference](#-api-reference)

### 🛠️ Run Locally

```bash
# Backend
./mvnw spring-boot:run

# Frontend (in new terminal)
cd coffeeshop-ui
npm install && npm run dev
```

**Frontend:** http://localhost:5173

---

## 🏛️ Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                    REST Controller                       │
│         (CoffeeShopController + CORS)                   │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│                  Service Layer                          │
│            (CoffeeShopService + DTOs)                   │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│                 FACADE PATTERN                          │
│              (CoffeeShopFacade)                         │
│     Simplifies complex subsystem interactions           │
└──┬──────────┬──────────┬──────────┬──────────┬─────────┘
   │          │          │          │          │
   ▼          ▼          ▼          ▼          ▼
Builder   Factory   Decorator  Strategy  Observer
Pattern   Pattern    Pattern   Pattern   Pattern
   │          │          │          │          │
   ▼          ▼          ▼          ▼          ▼
Order     Menu      Toppings   Pricing   Events
Build    Creation   (Milk,    (Discounts) (Status)
                    Syrup)                Updates
```

---

## 🎨 Design Patterns Implemented

### 1️⃣ **Facade Pattern** 🎭
**Location:** `patterns/facade/CoffeeShopFacade.java`

**Purpose:** Simplifies complex operations for clients

```java
CoffeeShopFacade facade = new CoffeeShopFacade(factory, strategy, payment);
facade.startNewOrder();
facade.addSimpleItem("LAT", 2);
facade.addCustomizedDrink(drinkRequest);
Order order = facade.checkoutAndPay();
```

**Benefits:**
- ✅ Hides complexity of 6+ subsystems
- ✅ Single entry point for ordering
- ✅ Easy to test and mock

---

### 2️⃣ **Builder Pattern** 🏗️
**Location:** `patterns/builder/OrderBuilder.java`

**Purpose:** Constructs complex Order objects step-by-step

```java
Order order = new OrderBuilder()
    .addItem(espresso, 2)
    .addItem(cheesecake, 1)
    .build();
```

**Benefits:**
- ✅ Fluent API
- ✅ Validates before building
- ✅ Immutable orders

---

### 3️⃣ **Decorator Pattern** 🎨
**Location:** `patterns/decorator/decorators/`

**Purpose:** Dynamically adds toppings to beverages

```java
Beverage latte = new Latte();
latte = new MilkDecorator(latte, MilkType.OAT);
latte = new SyrupDecorator(latte, SyrupType.CARAMEL);
latte = new ExtraShotDecorator(latte);

System.out.println(latte.getDescription()); 
// "Latte, with oat milk, with caramel syrup, extra shot"
System.out.println(latte.getBaseCost()); 
// 1450₸ (900 + 100 + 150 + 200)
```

**Benefits:**
- ✅ Open/Closed Principle
- ✅ Unlimited combinations
- ✅ Runtime composition

**Decorators Available:**
- 🥛 `MilkDecorator` - Whole, Oat, Almond, Coconut (+100₸)
- 🍯 `SyrupDecorator` - Caramel, Vanilla, Hazelnut, Chocolate (+150₸)
- ⚡ `ExtraShotDecorator` - Double the caffeine (+200₸)
- 🍦 `WhippedCreamDecorator` - Creamy topping (+100₸)
- 🌿 `CinnamonDecorator` - Aromatic spice (+50₸)

---

### 4️⃣ **Factory Pattern** 🏭
**Location:** `patterns/factory/MenuFactory.java`

**Purpose:** Creates menu items without exposing creation logic

```java
MenuFactory factory = new StandardMenuFactory();
Beverage espresso = factory.createBeverage("ESP");
Dessert cake = factory.createDessert("CHEESE");
Meal sandwich = factory.createMeal("SANDWICH");
```

**Implementations:**
- `StandardMenuFactory` - Regular menu items
- `AutumnMenuFactory` - Seasonal items (Pumpkin, Ginger)

**Benefits:**
- ✅ Encapsulates object creation
- ✅ Easy to add new products
- ✅ Supports seasonal variations

---

### 5️⃣ **Strategy Pattern** 💰
**Location:** `patterns/strategy/`

**Purpose:** Different pricing algorithms at runtime

```java
// Regular pricing
PricingStrategy regular = new NoDiscountStrategy();
double total = regular.calculateTotal(order); // 4500₸

// Student discount (10%)
PricingStrategy student = new StudentDiscountStrategy();
total = student.calculateTotal(order); // 4050₸

// Happy hour (20% on drinks)
PricingStrategy happyHour = new HappyHourStrategy();
total = happyHour.calculateTotal(order); // 3900₸
```

**Strategies:**
- `NoDiscountStrategy` - Full price
- `StudentDiscountStrategy` - 10% off total
- `HappyHourStrategy` - 20% off beverages

**Benefits:**
- ✅ Swap algorithms at runtime
- ✅ Easy to add promotions
- ✅ Single Responsibility Principle

---

### 6️⃣ **Observer Pattern** 👀
**Location:** `patterns/observer/`

**Purpose:** Notifies listeners about order status changes

```java
Order order = new Order(items);
order.getEvents().subscribe("STATUS_CHANGED", new BaristaConsoleObserver());
order.getEvents().subscribe("STATUS_CHANGED", new OrderLogObserver());

order.setStatus(OrderStatus.PREPARING); 
// Both observers notified automatically
```

**Observers:**
- `BaristaConsoleObserver` - Prints to console for barista
- `OrderLogObserver` - Logs to file/database

**Benefits:**
- ✅ Loose coupling
- ✅ Multiple subscribers
- ✅ Real-time updates

---

### 7️⃣ **Adapter Pattern** 🔌
**Location:** `patterns/adapter/`

**Purpose:** Integrates external payment systems

```java
PaymentProcessor kaspi = new KaspiPaymentAdapter();
kaspi.processPayment(4500.0);

PaymentProcessor halyk = new HalykPaymentAdapter();
halyk.processPayment(4500.0);
```

**Adapters:**
- `KaspiPaymentAdapter` - Kaspi QR payment
- `HalykPaymentAdapter` - Halyk Bank API
- `CashPayment` - Cash transactions

**Benefits:**
- ✅ Uniform interface for different APIs
- ✅ Easy to add payment methods
- ✅ Testable with mocks

---

## 📡 API Reference

### Get Full Catalog

```http
GET /catalog
```

**Response:**
```json
[
  {
    "code": "LAT",
    "name": "Latte",
    "description": "Smooth espresso with steamed milk",
    "category": "beverage",
    "price": 900.0,
    "imageUrl": "https://images.unsplash.com/...",
    "customizable": true,
    "toppingInfo": {
      "milkAllowed": true,
      "syrupAllowed": true,
      "extraShotAllowed": true,
      "whippedCreamAllowed": true,
      "cinnamonAllowed": true
    }
  }
]
```

### Create Simple Order

```http
POST /order
Content-Type: application/json

{
  "items": [
    { "code": "ESP", "quantity": 2 },
    { "code": "CHEESE", "quantity": 1 }
  ]
}
```

### Create Custom Order

```http
POST /order/custom
Content-Type: application/json

{
  "items": [
    { "code": "CHEESE", "quantity": 1 }
  ],
  "drinks": [
    {
      "code": "LAT",
      "quantity": 2,
      "withMilk": true,
      "milkType": "OAT",
      "withSyrup": true,
      "syrupType": "CARAMEL",
      "withExtraShot": true,
      "withWhippedCream": false,
      "withCinnamon": true
    }
  ]
}
```

**Response:**
```json
{
  "lines": [
    "Latte, with oat milk, with caramel syrup, extra shot, cinnamon x2 = 2900.00 ₸",
    "Cheesecake x1 = 1500.00 ₸"
  ],
  "subtotal": 4400.0,
  "discount": 0.0,
  "total": 4400.0
}
```

---

## 📦 Product Catalog

### ☕ Beverages (Customizable)

| Code | Name | Price | 🥛 | 🍯 | ⚡ | 🍦 | 🌿 |
|------|------|-------|----|----|----|----|-----|
| **ESP** | Espresso | 500₸ | ✅ | ✅ | ✅ | ❌ | ✅ |
| **LAT** | Latte | 900₸ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **CAP** | Cappuccino | 850₸ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **AME** | Americano | 900₸ | ✅ | ✅ | ✅ | ❌ | ✅ |
| **ICED_LAT** | Iced Latte | 950₸ | ✅ | ✅ | ❌ | ✅ | ❌ |
| **B_TEA** | Black Tea | 600₸ | ✅ | ✅ | ❌ | ❌ | ✅ |
| **G_TEA** | Green Tea | 650₸ | ❌ | ✅ | ❌ | ❌ | ❌ |
| **H_TEA** | Herbal Tea | 700₸ | ❌ | ✅ | ❌ | ❌ | ❌ |
| **HOT_CHOC** | Hot Chocolate | 1000₸ | ✅ | ❌ | ❌ | ✅ | ✅ |
| **LEM** | Lemonade | 750₸ | - | - | - | - | - |

**Legend:** 🥛 Milk · 🍯 Syrup · ⚡ Extra Shot · 🍦 Whipped Cream · 🌿 Cinnamon

### 🍰 Desserts & 🥗 Meals

| Code | Name | Category | Price |
|------|------|----------|-------|
| **CHEESE** | Cheesecake | Dessert | 1500₸ |
| **BROWNIE** | Brownie | Dessert | 800₸ |
| **MUFFIN** | Muffin | Dessert | 1200₸ |
| **CROISSANT** | Croissant | Dessert | 850₸ |
| **SANDWICH** | Sandwich | Meal | 1290₸ |
| **SALAD** | Fresh Salad | Meal | 790₸ |
| **LUNCHBOX** | Lunchbox | Meal | 1800₸ |

---

## 🐳 Docker Deployment

### Quick Deploy

```bash
# Build and run
docker-compose up -d

# Check logs
docker-compose logs -f

# Stop
docker-compose down
```

### Manual Docker

```bash
# Build image
docker build -t coffeeshop-backend:latest .

# Run container
docker run -d \
  --name coffeeshop \
  -p 8080:8080 \
  coffeeshop-backend:latest
```

**Image Size:** ~250MB (multi-stage build)  
**Health Check:** Built-in at `/actuator/health`

See [DOCKER.md](DOCKER.md) for detailed guide.

---

## 📁 Project Structure

```
src/main/java/com/coffeeshop/shop/
├── api/                          # DTOs and request/response models
│   ├── MenuItemDTO.java
│   ├── CustomDrinkRequest.java
│   ├── ToppingInfo.java
│   └── OrderResponse.java
├── core/
│   ├── model/                    # Domain models
│   │   ├── beverage/            # All drinks (10 types)
│   │   ├── dessert/             # Desserts + seasonal
│   │   ├── meal/                # Meals + seasonal
│   │   └── order/               # Order, OrderItem, Status
│   └── patterns/                 # Design patterns
│       ├── facade/              # 1️⃣ Facade
│       ├── builder/             # 2️⃣ Builder
│       ├── decorator/           # 3️⃣ Decorator (5 types)
│       ├── factory/             # 4️⃣ Factory (2 implementations)
│       ├── strategy/            # 5️⃣ Strategy (3 strategies)
│       ├── observer/            # 6️⃣ Observer (2 listeners)
│       └── adapter/             # 7️⃣ Adapter (3 payment methods)
├── service/                      # Business logic
│   └── CoffeeShopService.java
└── CoffeeShopController.java     # REST endpoints
```

---

## 🧪 Testing

```bash
# Run all tests
./mvnw test

# Run with coverage
./mvnw clean verify

# Integration tests
./mvnw integration-test
```

---

## 📚 Documentation

- 🐳 **[Docker Guide](DOCKER.md)** - Complete deployment and troubleshooting guide
- 📜 **[License](LICENSE)** - MIT License details

### Quick Tips

**Change Product Images:**
Edit `CoffeeShopService.java` and replace image URLs:
```java
"https://images.unsplash.com/photo-YOUR-ID?w=400"
// or use local images:
"/images/espresso.jpg"
```

**Enable CORS:**
Already configured in `CoffeeShopController.java`:
```java
@CrossOrigin(origins = "http://localhost:5173")
```

**Change Server Port:**
Edit `src/main/resources/application.properties`:
```properties
server.port=8080
```

---

## 📖 Project Evolution

This project is an **enhanced web version** of the [original console-based CoffeeShop](https://github.com/commedeschamps/CoffeeShop) application.

### What's New:
- ✅ **RESTful API** - Added Spring Boot REST controllers
- ✅ **React Frontend** - Modern UI with product cards and images
- ✅ **Docker Support** - Production-ready containerization
- ✅ **Smart Restrictions** - Backend validates topping compatibility
- ✅ **Visual Catalog** - `/catalog` endpoint with full product info
- ✅ **Live Deployment** - Hosted on Render cloud platform

### Original Console Features (Preserved):
- ✅ All 7 Design Patterns implementation
- ✅ Menu management (Beverages, Desserts, Meals)
- ✅ Order building and customization
- ✅ Pricing strategies (discounts)
- ✅ Payment processing adapters
- ✅ Event notifications

---

## 🎯 Key Features

### Customer Orders Latte with Customizations
```
1. Customer selects "Latte" from catalog
2. Chooses: Oat Milk + Caramel Syrup + Extra Shot
3. System uses Decorator Pattern to wrap base Latte
4. Calculates: 900₸ + 100₸ + 150₸ + 200₸ = 1350₸
5. Observer notifies barista console
6. Payment processed via Adapter
```

### Barista Applies Happy Hour Discount
```
1. Facade switches Strategy to HappyHourStrategy
2. Order total: 4500₸
3. Strategy calculates: 20% off drinks = 3900₸
4. Customer pays via Kaspi QR (Adapter)
5. Observer logs transaction
```

---

## 🤝 Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

---

## 📜 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file.

---

## 🙏 Acknowledgments

- **Spring Boot Team** - Excellent framework
- **Unsplash** - Free high-quality images
- **Design Patterns Community** - Gang of Four
- **React Team** - Powerful UI library

---

## 📧 Contact

**Project Link:** [https://github.com/commedeschamps/coffeeshop-backend](https://github.com/commedeschamps/coffeeshop-backend)

---

<div align="center">

**⭐ Star this repo if you found it helpful!**

Built with ❤️ and ☕ by [commedeschamps](https://github.com/commedeschamps)

</div>

