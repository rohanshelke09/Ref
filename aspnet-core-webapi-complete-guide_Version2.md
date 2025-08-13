# Comprehensive Guide for ASP.NET Core Web API, EF Core, JWT, and Related Skills (Part 3)

---

## 31. How to Configure Logging in ASP.NET Core?
### **Explanation:**
Logging helps track application behavior and troubleshoot issues. ASP.NET Core provides a built-in logging framework.

### **Steps:**
1. Configure logging settings in `appsettings.json`:
   ```json
   {
     "Logging": {
       "LogLevel": {
         "Default": "Information",
         "Microsoft.AspNetCore": "Warning"
       }
     }
   }
   ```
2. Inject `ILogger` into your controller:
   ```csharp
   private readonly ILogger<HotelsController> _logger;

   public HotelsController(ILogger<HotelsController> logger)
   {
       _logger = logger;
   }

   [HttpGet]
   public IActionResult GetHotels()
   {
       _logger.LogInformation("Fetching all hotels.");
       return Ok(new List<string> { "Hotel A", "Hotel B" });
   }
   ```

---

## 32. How to Use Query Parameters in Web API?
### **Explanation:**
Query parameters are appended to the URL to filter or modify the request.

### **Code Example:**
```csharp
[HttpGet]
public IActionResult GetHotels([FromQuery] string location)
{
    var hotels = _dbContext.Hotels
        .Where(h => h.Location == location)
        .ToList();
    return Ok(hotels);
}
```
**Request Example:**  
`GET /api/hotels?location=NewYork`

---

## 33. How to Use Route Parameters in Web API?
### **Explanation:**
Route parameters are part of the URL path and are used to identify resources.

### **Code Example:**
```csharp
[HttpGet("{id}")]
public IActionResult GetHotelById(int id)
{
    var hotel = _dbContext.Hotels.Find(id);
    if (hotel == null)
    {
        return NotFound();
    }
    return Ok(hotel);
}
```
**Request Example:**  
`GET /api/hotels/1`

---

## 34. How to Handle Pagination in Web API?
### **Explanation:**
Pagination breaks large datasets into smaller chunks for efficient data retrieval.

### **Code Example:**
```csharp
[HttpGet]
public IActionResult GetHotels(int page = 1, int pageSize = 10)
{
    var hotels = _dbContext.Hotels
        .Skip((page - 1) * pageSize)
        .Take(pageSize)
        .ToList();
    return Ok(hotels);
}
```
**Request Example:**  
`GET /api/hotels?page=2&pageSize=5`

---

## 35. How to Use Model Binding in ASP.NET Core?
### **Explanation:**
Model binding maps HTTP request data to action method parameters.

### **Code Example:**
```csharp
[HttpPost]
public IActionResult AddHotel([FromBody] Hotel hotel)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }
    _dbContext.Hotels.Add(hotel);
    _dbContext.SaveChanges();
    return CreatedAtAction(nameof(GetHotelById), new { id = hotel.Id }, hotel);
}
```

---

## 36. How to Use Filters in ASP.NET Core?
### **Explanation:**
Filters allow you to execute logic before or after an action method.

### **Example:**
- **Authorization Filter:** Validates user access.
- **Action Filter:** Runs logic before or after an action.
- **Exception Filter:** Handles unhandled exceptions.

### **Code Example:**
```csharp
public class LogActionFilter : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context)
    {
        Console.WriteLine("Action is executing");
    }

    public void OnActionExecuted(ActionExecutedContext context)
    {
        Console.WriteLine("Action executed");
    }
}

// Register filter globally
builder.Services.AddControllers(options =>
{
    options.Filters.Add<LogActionFilter>();
});
```

---

## 37. How to Use Asynchronous Programming in Web API?
### **Explanation:**
Async programming improves performance by freeing up threads for other tasks.

### **Code Example:**
```csharp
[HttpGet]
public async Task<IActionResult> GetHotelsAsync()
{
    var hotels = await _dbContext.Hotels.ToListAsync();
    return Ok(hotels);
}
```

---

## 38. What are Data Transfer Objects (DTOs)?
### **Explanation:**
DTOs are objects that transfer data between layers, often used to prevent over-posting or under-posting.

### **Code Example:**
```csharp
public class HotelDTO
{
    public string Name { get; set; }
    public string Location { get; set; }
}

[HttpPost]
public IActionResult AddHotel(HotelDTO hotelDto)
{
    var hotel = new Hotel
    {
        Name = hotelDto.Name,
        Location = hotelDto.Location
    };
    _dbContext.Hotels.Add(hotel);
    _dbContext.SaveChanges();
    return Ok(hotel);
}
```

---

## 39. How to Use Versioning in ASP.NET Core Web API?
### **Explanation:**
API versioning allows you to manage changes to your API without breaking existing clients.

### **Steps:**
1. Install the versioning package:
   ```bash
   dotnet add package Microsoft.AspNetCore.Mvc.Versioning
   ```
2. Configure versioning in `Program.cs`:
   ```csharp
   builder.Services.AddApiVersioning(options =>
   {
       options.ReportApiVersions = true;
       options.AssumeDefaultVersionWhenUnspecified = true;
       options.DefaultApiVersion = new ApiVersion(1, 0);
   });
   ```
3. Use version attributes in controllers:
   ```csharp
   [ApiVersion("1.0")]
   [ApiController]
   [Route("api/v{version:apiVersion}/[controller]")]
   public class HotelsController : ControllerBase { ... }
   ```

---

## 40. How to Deploy an ASP.NET Core Web API to Azure?
### **Steps:**
1. **Create a Web App in Azure Portal:**
   - Go to Azure Portal, create a new "Web App."
2. **Publish API from Visual Studio:**
   - Right-click the project -> Publish -> Azure -> Select Web App.
3. **Use Azure CLI for Deployment:**
   ```bash
   az webapp up --name HotelManagementAPI --resource-group MyResourceGroup
   ```
4. **Set Environment Variables:**
   - Configure connection strings or secrets in the Azure portal under "Configuration."

---

### **Next Steps:**
This section covers advanced configuration, DTOs, filters, pagination, and deployment. Let me know if you'd like me to continue with testing, edge cases, or other advanced topics!