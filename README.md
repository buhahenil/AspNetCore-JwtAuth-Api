# AspNetCore-JwtAuth-Api

**Production-ready JWT Authentication backend using ASP.NET Core Web API and EF Core.**  
Provides secure user registration, login, password hashing, and JWT-based authorization with clean architecture and scalable design.

---

## 📌 Overview

This project implements **JWT authentication** in an **ASP.NET Core Web API** using:

- Entity Framework Core (SQL Server)
- Password hashing (`IPasswordHasher`)
- JWT Bearer Authentication
- Clean Architecture (Services, Interfaces, DTOs, Controllers)
- Login / Register with JWT token generation
- Protected API endpoints using `[Authorize]`

---

## 📁 Project Folder Structure

User_Auth/
│
├── Controllers/
│   └── AuthController.cs
│
├── Data/
│   └── ApplicationDbContext.cs
│
├── DTOs/
│   └── Auth/
│       ├── RegisterRequest.cs
│       ├── LoginRequest.cs
│       └── AuthResponse.cs
│
├── Entities/
│   └── User.cs
│
├── Helpers/
│   └── JwtSettings.cs
│
├── Interfaces/
│   └── IAuthService.cs
│
├── Services/
│   └── AuthService.cs
│
├── appsettings.json
└── Program.cs

---

## ⚙️ Technologies Used

- ASP.NET Core Web API (.NET 8)
- Entity Framework Core
- SQL Server
- JWT Authentication
- Swagger UI

---

## 🛠️ Setup Instructions

## 1️⃣ Install Dependencies

Install required NuGet packages:

Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Tools
Install-Package Microsoft.AspNetCore.Authentication.JwtBearer
Install-Package Swashbuckle.AspNetCore

### 2️⃣ Configure Database & JWT in `appsettings.json`

***json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=UserAuthDb;Trusted_Connection=True;"
  },
  "Jwt": {
    "Key": "your-very-long-secret-key-change-this",
    "Issuer": "MyApi",
    "Audience": "MyApiUsers",
    "ExpiresMinutes": 60
  }
}


3️⃣ Apply EF Core Migrations
Add-Migration InitialCreate
Update-Database


📌 Code Explanation (Summary)
🔹 User Entity
Stores user data, including hashed password, not plaintext.
🔹 AuthService
Handles:
User registration
Password hashing & verification
Generating the JWT token
Returning AuthResponse

🔹 AuthController
API endpoints:
POST /api/auth/register
POST /api/auth/login

🔹 Program.cs
Configures:
EF Core DbContext
JWT Authentication Middleware
Dependency Injection

🔐 JWT Token Flow
✔️ Register
User sends username/email/password
Password hashed
User saved to DB
Token returned

✔️ Login
Validate user credentials
Generate token
Return token to client

✔️ Protected Routes
Client sends:
Authorization: Bearer <token>
JWT middleware validates the token before executing the controller.

🔥 JWT Token Structure
Token includes:
User ID (NameIdentifier)
Username (Name)
Email
Expiry time (exp)
Signed using HMAC-SHA256

🧪 API Endpoints
POST /api/auth/register
Body:
{
  "userName": "john",
  "email": "john@gmail.com",
  "password": "Password123"
}

POST /api/auth/login
Body:
{
  "userNameOrEmail": "john",
  "password": "Password123"
}

Response:
{
  "token": "<JWT_TOKEN>",
  "expires": "2025-01-01T10:20:30Z",
  "userName": "john"
}


🔐 Protected Example Endpoint
[Authorize]
[HttpGet("profile")]
public IActionResult Profile()
{
    return Ok(new {
        UserId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value,
        UserName = User.Identity?.Name
    });
}

Testing Header:
Authorization: Bearer <your_token_here>


🚀 Run the Project
dotnet run

Open Swagger:
👉 https://localhost:5001/swagger
Click Authorize → enter:
Bearer <token>


🧰 Common Issues
❌ 401 Unauthorized

Missing Authorization header
Token expired
Wrong issuer/audience
Wrong JWT secret
Calling HTTP while RequireHttpsMetadata = true

❌ Login fails

PasswordHasher mismatch
Incorrect username or email

📌 Future Enhancements

Refresh tokens
Role-based authorization
Email verification
Password reset
API versioning

🏁 Conclusion
This project delivers a clean and scalable JWT authentication system using ASP.NET Core Web API and EF Core.
Easily extend it with additional modules such as products, categories, orders, admin roles, etc.
