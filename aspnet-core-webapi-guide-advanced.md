# Comprehensive Guide for ASP.NET Core Web API, EF Core, and Related Skills (Part 2)

---

## 11. How to Retrieve Data from the Database Using EF Core?
### **Explanation:**
You can use LINQ queries to fetch data from the database using EF Core.

### **Code Example:**
```csharp
[HttpGet]
public async Task<ActionResult<List<Hotel>>> GetHotels()
{
    var hotels = await _dbContext.Hotels.ToListAsync();
    return Ok(hotels);
}
```

### **Key Points:**
- Use `ToListAsync()` for async queries.
- Use `FirstOrDefaultAsync()` to fetch a single record.

---

## 12. How to Update Data Using EF Core?
### **Explanation:**
To update data, first retrieve the record, modify its properties, and call `SaveChangesAsync()`.

### **Code Example:**
```csharp
[HttpPut("{id}")]
public async Task<IActionResult> UpdateHotel(int id, Hotel hotel)
{
    var existingHotel = await _dbContext.Hotels.FindAsync(id);
    if (existingHotel == null)
    {
        return NotFound();
    }

    existingHotel.Name = hotel.Name;
    existingHotel.Location = hotel.Location;

    await _dbContext.SaveChangesAsync();
    return NoContent();
}
```

---

## 13. How to Delete Data Using EF Core?
### **Explanation:**
Fetch the record, call `Remove()`, and save changes.

### **Code Example:**
```csharp
[HttpDelete("{id}")]
public async Task<IActionResult> DeleteHotel(int id)
{
    var hotel = await _dbContext.Hotels.FindAsync(id);
    if (hotel == null)
    {
        return NotFound();
    }

    _dbContext.Hotels.Remove(hotel);
    await _dbContext.SaveChangesAsync();
    return NoContent();
}
```

---

## 14. What is JWT Authentication?
### **Explanation:**
JWT (JSON Web Token) is a secure way to transmit information between parties. It is commonly used for authentication.

### **Structure of JWT:**
1. Header: Contains the algorithm and token type.
2. Payload: Contains claims (e.g., user ID, roles).
3. Signature: Verifies the token’s authenticity.

---

## 15. How to Configure JWT Authentication in ASP.NET Core?
### **Steps:**
1. Add the `Microsoft.AspNetCore.Authentication.JwtBearer` package.
2. Configure JWT in `Program.cs`:
   ```csharp
   builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
       .AddJwtBearer(options =>
       {
           options.TokenValidationParameters = new TokenValidationParameters
           {
               ValidateIssuerSigningKey = true,
               IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("YourSecretKey")),
               ValidateIssuer = false,
               ValidateAudience = false
           };
       });
   ```

---

## 16. How to Generate a JWT Token?
### **Code Example:**
```csharp
public string GenerateToken(User user)
{
    var claims = new List<Claim>
    {
        new Claim(ClaimTypes.NameIdentifier, user.Id.ToString()),
        new Claim(ClaimTypes.Role, user.Role)
    };

    var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("YourSecretKey"));
    var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

    var token = new JwtSecurityToken(
        claims: claims,
        expires: DateTime.Now.AddDays(1),
        signingCredentials: creds
    );

    return new JwtSecurityTokenHandler().WriteToken(token);
}
```

---

## 17. How to Protect API Endpoints Using JWT?
### **Explanation:**
Add the `[Authorize]` attribute to restrict access to authenticated users.

### **Code Example:**
```csharp
[Authorize]
[HttpGet]
public IActionResult GetProtectedData()
{
    return Ok("This is protected data.");
}
```

---

## 18. How to Add Role-Based Authorization?
### **Explanation:**
Use the `[Authorize(Roles = "RoleName")]` attribute to restrict access based on user roles.

### **Code Example:**
```csharp
[Authorize(Roles = "Admin")]
[HttpPost]
public IActionResult AdminOnlyAction()
{
    return Ok("This action is only for Admins.");
}
```

---

## 19. What is CORS, and Why is It Important?
### **Explanation:**
CORS (Cross-Origin Resource Sharing) allows a web app from one domain to access APIs hosted on another domain. It’s essential for security when integrating APIs with frontend apps.

### **How to Enable CORS?**
Add CORS support in `Program.cs`:
```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowReactApp", builder =>
    {
        builder.WithOrigins("http://localhost:3000")
               .AllowAnyHeader()
               .AllowAnyMethod();
    });
});

app.UseCors("AllowReactApp");
```

---

## 20. What is Swagger, and How Do You Use It?
### **Explanation:**
Swagger provides interactive API documentation, making it easy to test and understand APIs.

### **Steps to Enable Swagger:**
1. Add the `Swashbuckle.AspNetCore` package.
2. Configure Swagger in `Program.cs`:
   ```csharp
   builder.Services.AddSwaggerGen();

   if (app.Environment.IsDevelopment())
   {
       app.UseSwagger();
       app.UseSwaggerUI();
   }
   ```
3. Access Swagger UI at `https://localhost:<port>/swagger`.

---

### **Next Steps:**
This set covers CRUD operations, JWT authentication, CORS, and Swagger. Let me know if you'd like me to proceed with advanced EF Core concepts, debugging, or specific use cases!