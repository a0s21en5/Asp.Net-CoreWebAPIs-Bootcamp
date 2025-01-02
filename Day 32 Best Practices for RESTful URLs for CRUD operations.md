# Day 32 Best Practices for RESTful URLs for CRUD operations

In ASP.NET Core 5.0, building a RESTful API that adheres to standard conventions is crucial for maintainability and scalability. One of the key elements of a RESTful API is how URLs (also referred to as endpoints) are designed for performing CRUD operations (Create, Read, Update, Delete). Below are best practices for designing RESTful URLs for CRUD operations:

## 1. **Use Nouns for Resources**

- URLs should represent entities or resources and be **plural nouns**. For example, use `products` to refer to a collection of products, and `product` to refer to a single product.
- **Bad Example**: `/createProduct`, `/getProductById`
- **Good Example**: `/products`, `/products/{id}`

## 2. **Use HTTP Methods to Define CRUD Operations**

RESTful APIs rely on standard HTTP methods (verbs) to define actions on resources:

- **GET**: Retrieve data.
- **POST**: Create a new resource.
- **PUT** or **PATCH**: Update an existing resource.
- **DELETE**: Delete a resource.

**Example CRUD Operations:**

- **Create**: `POST /products` — Adds a new product.
- **Read** (all): `GET /products` — Retrieves a list of products.
- **Read** (single): `GET /products/{id}` — Retrieves a product by ID.
- **Update**: `PUT /products/{id}` — Updates an existing product by ID (replaces entire resource).
- **Delete**: `DELETE /products/{id}` — Deletes a product by ID.

**Note**: Use `PUT` for full updates (replacing the resource) and `PATCH` for partial updates (updating only specific fields).

## 3. **Use HTTP Status Codes Properly**

HTTP status codes should indicate the result of an operation:

- **200 OK**: Successful GET, PUT, or PATCH request.
- **201 Created**: Successful POST request.
- **204 No Content**: Successful DELETE request.
- **400 Bad Request**: Invalid data sent (e.g., validation errors).
- **404 Not Found**: Resource not found (e.g., trying to get a non-existent resource).
- **500 Internal Server Error**: Server encountered an error.

## 4. **Be Consistent with Plural and Singular Naming**

Consistency in naming is important:

- Use **plural** for collection endpoints: `/products`, `/orders`, `/customers`
- Use **singular** for actions on a specific resource: `/products/{id}`, `/orders/{id}`

## 5. **Use Path Parameters for Resource Identifiers**

When accessing a specific resource, include its identifier in the path:

- **Good Example**: `/products/123` — Get the product with ID 123.
- **Bad Example**: `/products?id=123` — Avoid using query parameters for resource identifiers.

## 6. **Avoid Using Verbs in URL Paths**

REST is resource-oriented, so avoid including verbs in the URL path. The HTTP method itself already indicates the operation.

- **Bad Example**: `/getProduct/{id}`, `/deleteProduct/{id}`
- **Good Example**: `/products/{id}`, `/products/{id}` (use DELETE HTTP method to indicate delete).

## 7. **Use Query Parameters for Filtering, Sorting, and Pagination**

- **Filtering**: Use query parameters to filter resources based on specific attributes (e.g., `/products?category=electronics`).
- **Sorting**: To sort resources, use query parameters such as `/products?sortBy=name&sortOrder=asc`.
- **Pagination**: For large collections, provide pagination options using `limit` and `offset` (e.g., `/products?limit=10&offset=0` or `/products?page=1&size=10`).

## 8. **Version Your API (Optional but Recommended)**

It’s good practice to version your API to ensure backward compatibility when you make breaking changes. There are several strategies for versioning:

- **In the URL path**: `/v1/products`, `/v2/products`
- **In the request header**: `Accept: application/vnd.myapi.v1+json`

**Example**:

- **Version 1**: `/v1/products`
- **Version 2**: `/v2/products`

Versioning can be useful when maintaining and upgrading APIs, ensuring clients still work even if the API changes.

## 9. **Use HATEOAS (Hypermedia as the Engine of Application State) (Optional)**

HATEOAS is a REST constraint that makes APIs more discoverable by providing related links within resource responses. This is an optional practice but highly recommended for complex applications where clients should know what other actions they can perform next.

**Example Response:**

```json
{
  "id": 123,
  "name": "Laptop",
  "price": 1000,
  "_links": {
    "self": { "href": "/products/123" },
    "update": { "href": "/products/123" },
    "delete": { "href": "/products/123" }
  }
}
```

### 10. **Error Handling and Descriptive Responses**

Provide meaningful error messages in your responses to help clients understand what went wrong.

Example:

```json
{
  "error": {
    "message": "Product with ID 123 not found",
    "code": "PRODUCT_NOT_FOUND"
  }
}
```

### Example of Complete API Endpoints for a Product Resource

| **HTTP Method** | **Endpoint**                          | **Description**                            |
| --------------- | ------------------------------------- | ------------------------------------------ |
| GET             | `/products`                           | Get all products                           |
| GET             | `/products/{id}`                      | Get a specific product by ID               |
| POST            | `/products`                           | Create a new product                       |
| PUT             | `/products/{id}`                      | Update an existing product by ID           |
| PATCH           | `/products/{id}`                      | Partially update an existing product by ID |
| DELETE          | `/products/{id}`                      | Delete a product by ID                     |
| GET             | `/products?category=electronics`      | Get all products filtered by category      |
| GET             | `/products?sortBy=name&sortOrder=asc` | Get all products sorted by name            |
| GET             | `/products?page=1&size=10`            | Get products with pagination               |

### ASP.NET Core 5.0 Example Implementation

1. **Controller Example:**

```csharp
[Route("api/[controller]")]
[ApiController]
public class ProductsController : ControllerBase
{
    private readonly IProductService _productService;

    public ProductsController(IProductService productService)
    {
        _productService = productService;
    }

    [HttpGet]
    public async Task<IActionResult> GetAll()
    {
        var products = await _productService.GetAllAsync();
        return Ok(products);
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetById(int id)
    {
        var product = await _productService.GetByIdAsync(id);
        if (product == null) return NotFound();
        return Ok(product);
    }

    [HttpPost]
    public async Task<IActionResult> Create([FromBody] ProductDto productDto)
    {
        var product = await _productService.CreateAsync(productDto);
        return CreatedAtAction(nameof(GetById), new { id = product.Id }, product);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> Update(int id, [FromBody] ProductDto productDto)
    {
        var updatedProduct = await _productService.UpdateAsync(id, productDto);
        if (updatedProduct == null) return NotFound();
        return Ok(updatedProduct);
    }

    [HttpDelete("{id}")]
    public async Task<IActionResult> Delete(int id)
    {
        var result = await _productService.DeleteAsync(id);
        if (!result) return NotFound();
        return NoContent();
    }
}
```
