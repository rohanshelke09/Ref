# Comprehensive Guide for ASP.NET Core Web API, EF Core, JWT, and Related Skills (Part 4)

---

## 41. How to Test ASP.NET Core Web API Using Postman?
### **Explanation:**
Postman is a tool to manually test APIs by sending HTTP requests and observing responses.

### **Steps:**
1. Install Postman.
2. Create a new request.
3. Enter the API URL (e.g., `https://localhost:5001/api/hotels`).
4. Select the HTTP method (GET, POST, etc.).
5. Add headers (e.g., Authorization: Bearer `<JWT Token>`).
6. For POST/PUT, add a JSON body:
   ```json
   {
       "name": "Hotel A",
       "location": "New York"
   }
   ```
7. Click "Send" and view the response.

---

## 42. How to Write Unit Tests for ASP.NET Core Web API?
### **Explanation:**
Unit testing ensures individual components work as expected. Use the `xUnit` library for testing.

### **Code Example:**
1. Install `xUnit`:
   ```bash
   dotnet add package xunit
   dotnet add package Microsoft.AspNetCore.Mvc.Testing
   ```

2. Create a test class:
   ```csharp
   public class HotelsControllerTests
   {
       private readonly HotelsController _controller;

       public HotelsControllerTests()
       {
           var mockService = new Mock<IHotelService>();
           mockService.Setup(service => service.GetAllHotels())
                      .Returns(new List<Hotel> { new Hotel { Name = "Test Hotel", Location = "Test City" } });

           _controller = new HotelsController(mockService.Object);
       }

       [Fact]
       public void GetHotels_ReturnsOkResult_WithListOfHotels()
       {
           var result = _controller.GetHotels();

           Assert.IsType<OkObjectResult>(result.Result);
           var hotels = (result.Result as OkObjectResult).Value as List<Hotel>;
           Assert.Single(hotels);
       }
   }
   ```

---

## 43. How to Mock Dependencies Using Moq?
### **Explanation:**
Moq is a mocking framework that allows you to create test doubles for dependencies.

### **Code Example:**
```csharp
var mockService = new Mock<IHotelService>();
mockService.Setup(service => service.GetHotelById(1))
           .Returns(new Hotel { Id = 1, Name = "Mock Hotel" });

var controller = new HotelsController(mockService.Object);
var result = controller.GetHotelById(1);
Assert.IsType<OkObjectResult>(result.Result);
```

---

## 44. How to Debug ASP.NET Core Web API Locally?
### **Steps:**
1. Use Visual Studio or Visual Studio Code.
2. Set breakpoints in your code.
3. Run the API in debug mode (F5 in Visual Studio).
4. Use tools like Postman to hit the API and inspect the code execution.

---

## 45. How to Handle Null Values in the API?
### **Explanation:**
Handle null values to avoid runtime exceptions.

### **Code Example:**
```csharp
[HttpGet("{id}")]
public IActionResult GetHotelById(int id)
{
    var hotel = _dbContext.Hotels.Find(id);
    if (hotel == null)
    {
        return NotFound("Hotel not found.");
    }
    return Ok(hotel);
}
```

---

## 46. How to Secure Sensitive Data Like Connection Strings?
### **Explanation:**
Store sensitive data in environment variables or secrets.

### **Steps:**
1. Use `dotnet user-secrets` for local development:
   ```bash
   dotnet user-secrets init
   dotnet user-secrets set "ConnectionStrings:DefaultConnection" "YourConnectionString"
   ```
2. Access in your app:
   ```csharp
   builder.Configuration.GetConnectionString("DefaultConnection");
   ```

---

## 47. How to Rate Limit API Requests?
### **Explanation:**
Rate limiting restricts the number of requests a user can make in a given time frame.

### **Code Example:**
Use middleware like `AspNetCoreRateLimit`:
1. Install the package:
   ```bash
   dotnet add package AspNetCoreRateLimit
   ```
2. Configure rate limiting:
   ```csharp
   builder.Services.AddMemoryCache();
   builder.Services.Configure<IpRateLimitOptions>(options =>
   {
       options.GeneralRules = new List<RateLimitRule>
       {
           new RateLimitRule
           {
               Endpoint = "*",
               Limit = 100,
               Period = "1m"
           }
       };
   });
   app.UseIpRateLimiting();
   ```

---

## 48. How to Handle Concurrency in EF Core?
### **Explanation:**
Concurrency occurs when multiple users try to update the same data simultaneously. Use concurrency tokens to handle it.

### **Steps:**
1. Add a concurrency token to your model:
   ```csharp
   public class Hotel
   {
       public int Id { get; set; }
       public string Name { get; set; }
       [ConcurrencyCheck]
       public string Location { get; set; }
   }
   ```
2. Handle concurrency exceptions:
   ```csharp
   try
   {
       _dbContext.SaveChanges();
   }
   catch (DbUpdateConcurrencyException)
   {
       return Conflict("The record was updated by another user.");
   }
   ```

---

## 49. How to Use Global Exception Handling Middleware?
### **Explanation:**
Global exception handling ensures consistent error responses.

### **Code Example:**
```csharp
app.UseMiddleware<ExceptionMiddleware>();

public class ExceptionMiddleware
{
    private readonly RequestDelegate _next;

    public ExceptionMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            context.Response.StatusCode = 500;
            await context.Response.WriteAsync("An error occurred: " + ex.Message);
        }
    }
}
```

---

## 50. How to Optimize ASP.NET Core API Performance?
### **Tips:**
1. **Caching:** Use `ResponseCache` or distributed caching for repeated requests.
   ```csharp
   [ResponseCache(Duration = 60)]
   [HttpGet]
   public IActionResult GetCachedData() => Ok("This data is cached.");
   ```
2. **Use Asynchronous Code:** Avoid blocking threads with synchronous code.
3. **Database Optimization:** Use indexes and avoid over-fetching data.
4. **Compression:** Enable gzip compression:
   ```csharp
   builder.Services.AddResponseCompression();
   app.UseResponseCompression();
   ```

---

### **Next Steps:**
This set covers testing, debugging, concurrency, and performance optimization. Let me know if you'd like to continue with additional advanced topics such as deployment automation, real-world scenarios, or API best practices!