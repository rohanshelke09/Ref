# Ref

```
Day 8
Entity Framework Core 8.0, Dapper

Overview of EF Core 8 and .NET 8 Integration
Setting up EF Core in a .NET 8 Project
Create a Model with Database Table in .NET 8 using EF Core
Day 9
Entity Framework Core 8.0, Dapper

Creating a Simple Database Model
Performing Basic CRUD Operations
LINQ Queries in EF Core 8
CRUD Operations in Entity Framework Core
Car Repository - Insert
Car Repository - Eager Loading
Car Repository - Lazy Loading
Day 10
Entity Framework Core 8.0, Dapper

EF Core Migrations and Database Updates
Handling Relationships and Data Loading
Performance Optimizations and Best Practices
EF Core Performance Optimizations
Day 11
Entity Framework Core 8.0, Dapper

Getting Started with Dapper
Database Providers with Dapper
Querying Data with Dapper
Executing Non-Query Commands with Dapper
Day 12 - Forenoon
Entity Framework Core 8.0, Dapper

Using Parameters with Dapper
Using Relationships with Dapper
Bulk Operations with Dapper
Day 15 - Afternoon
ASP.NET Core 8

Introduction to ASP.NET Core 8
Creating a New ASP.NET Core Project
Understanding MVC Architecture
Building your first ASP.NET Core application
Interrogation Panel - Data Transferring
Interrogation Panel - Component
Interrogation Panel - Razor Page
Day 17
ASP.NET Core 8

Working with Models
Building Controllers
Designing Views in ASP.NET Core
Get started with ASP.NET Core MVC
Add a controller to an ASP.NET Core MVC app
Add a view to an ASP.NET Core MVC app
Add a model to an ASP.NET Core MVC app
Day 18
ASP.NET Core 8

Routing in ASP.NET Core
Working with Forms and User Input
State Management
Integrating EF Core 8 with ASP.NET Core
Routing in ASP.NET Core
ASP.NET Core Blazor forms overview
Session and state management in ASP.NET Core
Tag Helpers [MVC]
Entity Framework Core [MVC and Web API]
Interrogation Panel - DB Initialize
Day 19 - Forenoon
ASP.NET Core 8

Dependency Injection (DI) in ASP.NET Core
Publishing and Deployment
Dependency injection in ASP.NET Core
Host and deploy ASP.NET Core
Day 19 - Afternoon, 20
ASP.NET Core 8 Web API

Introduction to Web APIs and ASP.NET Core
Building RESTful APIs with ASP.NET Core
Implement Entity Framework A Code First Approach in .Net 8 API
Online Bookstore API - Insert
Day 21
ASP.NET Core 8 Web API

Advanced API Features
Securing our ASP.NET Core API - Authentication and Authorization - JWT Tokens
Routing in ASP.NET Core Web API
Filters in ASP.NET Core Web API
Online Bookstore API – Retrieve
Online Bookstore API - Modify
Online Bookstore API - Remove
Day 22
ASP.NET Core 8 Web API

Consuming and Creating SOAP Services
SOAP Web Service in .NET Core
Day 23
ASP.NET Core 8 Web API

API Security and Exception Handling
Advanced Functionality in ASP.NET Core Web APIs
Day 24, 25 - Forenoon
ASP.NET Core 8 Web API

Securing Your API
API Versioning
Documenting Your API with Swagger/OpenAPI
Securing our ASP.NET Core API - Authentication and Authorization - JWT Tokens
Versioning in ASP.NET Core Web API
Swagger / Open API






Description


Super Nova Fashion Clothings

Objective :

To work with "Routing" along with components in React JS.

Scenario :


One of the most prestigious apparel stores, SuperNova fashion clothings, has chosen to build its purchasing page on React JS. Create a navigation bar for this application that allows users to switch between View Cart and Display Products.

1. App Component

File: App.js

Refer the screenshot 1 and design the App component with a list of two links: Display Products and View Cart.

Structure:

Nova_diagram

Styling:


1. The <div> tag should have the below styles:

(i) Minimum height must be 950px and it's background image must have linear-gradient(to bottom, #0052b0, #520f41) as its value.

2.  Link tag(style: link2) for View Cart must should have the below style:

(i) Size of the font must be 1.3em

3. Link tag(style: link1) for Display Products must should have the below styles:

(i) Size of the font must be 1.3em

(ii) Left side Margin must be 1.25%

Routing configuration

Link

Router path

Route To

Display Products

/displayProducts

DisplayProducts Component

View Cart

/viewCart

ViewCart Component


On clicking the Display Products link, it should route to the DisplayProducts Component

On clicking the View Cart link, it should route to ViewCart Component.

Screenshot 1





2. DisplayProducts Component

Design the DisplayProducts component with the below requirement.

File: DisplayProducts.js

Design constraints:

All the available products must appear on the web page, as shown in screenshot 2

Perform the iterations for the products.

[List of products are fetched from products.json file and it is imported which you can make use of it directly]

Implementation:

Created component named "DisplayProducts".

a) Iterate the productList using map within div tag

b) Within map, function with product and index as it's arguments should return another div tag as follows:

Tag Name

Key attribute's Value

Style attribute's value

div

Product's id

styles.dt


c) Structure to be followed inside div tag is placed below





Tag Name

Label Name

Value

Style attribute's value

dl





fontWeight:"500"

dt

Product id:

{product.id}

styles.dt

dd

Title:

{product.title}

styles.label

dd

Price:

{product.price}

styles.label

dd

Available Size:

{product.availableSizes}

styles.label

dd>button

Add To Cart

-

styles.button


d) Available size must use map and should return 'td' tag within which size of the product must be displayed.

e) Tag 'button' must include onclick event whose value should be {this.cartHandler.bind(this,index)}

f) cartHandler method must invoke addToCart function which is present inside Service.js


Styling:

Consider "styles" as constant which holds the following styles:

1. The label tag should have the below styles:

(i) Weight of the font must be 600

(ii) Text color must be #D0F0C0

2. The dt tag that iterates the cart list must use the following styles:

(i) Color must be #000000

(ii) Weight of the font must be 700

(iii) Background color must be #D1C1F8

3. The paragraph tag should have the below styles:

(i) Weight of the font must be 700

(ii) Color must be #FFA07A

4. The button tag should have the below styles:

(i) Left side of the margin must be 77.50%


Screenshot 2




3. ViewCart Component

Design the ViewCart component with the below requirement.

File: ViewCart .js


Design constraints:

Products which are added to cart must be displayed on the web page, as shown in Screenshot 4.


Styling:

Consider "styles" as constant which holds the following styles:

1. The h2 tag should have the below styles:

(i) Weight of the font must be 650 and it's text should be aligned center.

(ii) Text color must be #FFA07A.

2. The <dt> tag should have the below styles:

(i) Weight of the font must be 700.

(ii) Color must be #ffffff.

3. The <dl> tag should have the below styles:

(i) Weight of the font must be 500.

Implementation:

a) Method viewCart must be invoked and it's returned value must be stored in the variable 'products'.

b) ViewCart component must return the div tag.

c) If there is no product added to the cart, then "There are no items in the Cart list that can be shown." must be displayed as <h2> tag. [Refer screenshot 3]

d) If not, then iterate the products using map and return the div which in turn consists of dl, dt, and dd tags.[Refer screnshot 4]

Tag Name

Label Name

Value

Style attribute's value

dl





styles.dl

dt

Product id:

{product.id}

styles.dt

dd

Title:

{product.title}



dd

Price:

{product.price}



<dl> tag must have key attribute whose value must be the id of the product.




Screenshot 3



Screenshot 4:



4. Service

File: Service.js

Implementation: This Service.js file has two functions namely, addToCart and viewCart [Provided as template]

(i) addToCart(index): When the "Add To Cart" button is clicked, this function must push the product into the cartList(already given) and display the alert message "Product has been added to your cart."

(ii) viewCart(): This function must return the cartList(declared and provided as a part of template)


SuperNova-name project 


.gitignore
package.json
public
index.html
manifest.json
robots.txt
README.md
src
App.css
App.js
App.test.js
Components
DisplayProducts.js
ViewCart.js
index.css
index.js
products.json
reportWebVitals.js
Service
Service.js
setupTests.js
.gitignore
package.json
index.html
manifest.json
robots.txt
README.md
App.css
App.js
App.test.js
DisplayProducts.js
ViewCart.js
index.css
index.js
products.json
reportWebVitals.js
Service.js
setupTests.js


