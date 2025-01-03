# Format response with status code

## Return 200 Status Code | 200 OK

This status code indicates that the request was successful and the server has returned the requested data.

### Example:

```csharp
public IActionResult Get()
{
    return Ok(new { Message = "Request successful" });
}
```

## 400 Status Code | Bad Request

The 400 status code is used when the server cannot process the request due to invalid syntax or parameters.

### Example:

```csharp
public IActionResult Post([FromBody] MyModel model)
{
    if (model == null)
    {
        return BadRequest("Invalid input data");
    }
    return Ok();
}
```

## Return 201 Status Code | 201 Created

The 201 status code indicates that a new resource has been created successfully.

### Example:

```csharp
public IActionResult Create([FromBody] MyModel model)
{
    if (model == null)
    {
        return BadRequest("Invalid data");
    }

    // Assume the model is saved to the database
    return CreatedAtAction(nameof(Get), new { id = model.Id }, model);
}
```

## Return 301, 302 Status Code | 301 302 Local Redirect

The 301 and 302 status codes are used for redirection. 301 is a permanent redirect, while 302 is temporary.

### Example (301 Redirect):

```csharp
public IActionResult RedirectToNewLocation()
{
    return RedirectPermanent("https://new-location.com");
}
```

### Example (302 Redirect):

```csharp
public IActionResult RedirectTemporarily()
{
    return Redirect("https://temporary-location.com");
}
```

## Return 404 Status Code | 404 Not Found

This status code indicates that the requested resource could not be found.

### Example:

```csharp
public IActionResult Get(int id)
{
    var item = _repository.GetItemById(id);
    if (item == null)
    {
        return NotFound();
    }
    return Ok(item);
}
```

## Remaining Built-in Methods | Remaining Methods

ASP.NET Core provides other status codes and helper methods to handle various HTTP responses, including:

- **201 Created**
- **400 Bad Request**
- **404 Not Found**
- **500 Internal Server Error**
- **401 Unauthorized**
- **403 Forbidden**
- **204 No Content**

Refer to the official documentation for a complete list of status codes and usage examples.
