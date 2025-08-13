# Comprehensive Guide for ASP.NET Core Web API, EF Core, JWT, and Related Skills

---

## 21. How to Validate User Input in ASP.NET Core?
### **Explanation:**
ASP.NET Core provides built-in validation attributes from the `System.ComponentModel.DataAnnotations` namespace to validate user input.

### **Steps:**
1. Add validation attributes to your model properties.
2. Use `ModelState.IsValid` to check for validation errors.

### **Code Example:**
```csharp
public class Hotel
{
    public int Id { get; set; }

    [Required(ErrorMessage = "Name is required.")]
    [MaxLength(100, ErrorMessage = "Name cannot exceed 100 characters.")]
    public string Name { get; set; }

    [Required]
    public string Location { get; set; }
}

[HttpPost]
public IActionResult AddHotel(Hotel hotel)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }
    // Add hotel logic here
    return Ok(hotel);
}
```
---

## 22. How to Handle Exceptions in ASP.NET Core?
### **Explanation:**
Use middleware to handle exceptions globally and ensure the app doesn’t crash.

### **Steps:**
1. Add `UseExceptionHandler` middleware in `Program.cs`.
2. Customize exception responses.

### **Code Example:**
```csharp
app.UseExceptionHandler(errorApp =>
{
    errorApp.Run(async context =>
    {
        context.Response.StatusCode = 500;
        await context.Response.WriteAsync("An unexpected error occurred.");
    });
});
```

---

## 23. How to Use Dependency Injection (DI) in ASP.NET Core?
### **Explanation:**
Dependency Injection is a design pattern used to achieve Inversion of Control. ASP.NET Core has built-in DI.

### **Steps:**
1. Register services in `Program.cs`.
2. Inject services into constructors.

### **Code Example:**
```csharp
// Register service
builder.Services.AddScoped<IHotelService, HotelService>();

// Inject service
public class HotelsController : ControllerBase
{
    private readonly IHotelService _hotelService;

    public HotelsController(IHotelService hotelService)
    {
        _hotelService = hotelService;
    }

    [HttpGet]
    public IActionResult GetHotels() => Ok(_hotelService.GetAllHotels());
}
```

---

## 24. How to Use LINQ for Querying Data?
### **Explanation:**
LINQ (Language Integrated Query) simplifies querying collections and databases.

### **Code Example:**
```csharp
var hotelsInCity = _dbContext.Hotels
    .Where(h => h.Location == "New York")
    .OrderBy(h => h.Name)
    .ToList();
```

---

## 25. What are Migrations in EF Core?
### **Explanation:**
Migrations track changes in your database schema and apply those changes to the database.

### **Steps:**
1. Add a migration:
   ```bash
   dotnet ef migrations add InitialCreate
   ```
2. Apply the migration:
   ```bash
   dotnet ef database update
   ```

---

## 26. How to Seed Data in EF Core?
### **Explanation:**
Seeding adds initial data to your database.

### **Code Example:**
```csharp
public class HotelDbContext : DbContext
{
    public DbSet<Hotel> Hotels { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Hotel>().HasData(
            new Hotel { Id = 1, Name = "Hotel A", Location = "New York" },
            new Hotel { Id = 2, Name = "Hotel B", Location = "London" }
        );
    }
}
```

---

## 27. How to Use Include in EF Core for Eager Loading?
### **Explanation:**
Use `Include` to load related data in a single query.

### **Code Example:**
```csharp
var hotelsWithRooms = _dbContext.Hotels
    .Include(h => h.Rooms)
    .ToList();
```

---

## 28. How to Set Up Role-Based Authorization with JWT?
### **Explanation:**
Add roles to JWT claims and configure role-based access in controllers.

### **Code Example:**
```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Name),
    new Claim(ClaimTypes.Role, user.Role)
};

[Authorize(Roles = "Admin")]
[HttpPost]
public IActionResult AdminOnlyEndpoint() => Ok("Welcome, Admin!");
```

---

## 29. How to Handle CORS Errors?
### **Explanation:**
CORS errors occur when your API is accessed from a different domain. Fix them by enabling CORS in your API.

### **Code Example:**
```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAllOrigins", policy =>
    {
        policy.AllowAnyOrigin().AllowAnyMethod().AllowAnyHeader();
    });
});

app.UseCors("AllowAllOrigins");
```

---

## 30. How to Create Custom Middleware?
### **Explanation:**
Middleware is a pipeline component that processes HTTP requests and responses.

### **Code Example:**
```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        // Custom logic here
        await context.Response.WriteAsync("Custom Middleware Executed.");
        await _next(context);
    }
}

// Register middleware
app.UseMiddleware<CustomMiddleware>();
```

---

### **Next Steps:**
This guide now includes getting started, intermediate concepts, and advanced usage like JWT, DI, LINQ, CORS, and middleware. Let me know if you'd like me to continue with edge cases, debugging, or deployment!