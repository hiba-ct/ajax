# API Reference Documentation

## Table of Contents
- [External API Integration](#external-api-integration)
- [JavaScript Functions](#javascript-functions)
- [DOM Manipulation](#dom-manipulation)
- [Event Handlers](#event-handlers)
- [Error Handling](#error-handling)

## External API Integration

### JSONPlaceholder Users API

#### Endpoint
```
GET https://jsonplaceholder.typicode.com/users
```

#### Request Details
- **Method**: GET
- **Headers**: None required
- **Authentication**: Not required
- **Rate Limiting**: No explicit limits documented
- **CORS**: Enabled for all origins

#### Response Format

**Success Response (200 OK)**
```json
[
  {
    "id": 1,
    "name": "Leanne Graham",
    "username": "Bret",
    "email": "Sincere@april.biz",
    "address": {
      "street": "Kulas Light",
      "suite": "Apt. 556",
      "city": "Gwenborough",
      "zipcode": "92998-3874",
      "geo": {
        "lat": "-37.3159",
        "lng": "81.1496"
      }
    },
    "phone": "1-770-736-8031 x56442",
    "website": "hildegard.org",
    "company": {
      "name": "Romaguera-Crona",
      "catchPhrase": "Multi-layered client-server neural-network",
      "bs": "harness real-time e-markets"
    }
  }
  // ... 9 more user objects
]
```

**Error Response (4xx/5xx)**
```json
{
  "error": "Error message description"
}
```

#### Field Descriptions

| Field | Type | Description | Used in App |
|-------|------|-------------|-------------|
| `id` | number | Unique user identifier | ✅ |
| `name` | string | User's full name | ✅ |
| `username` | string | User's username | ❌ |
| `email` | string | User's email address | ✅ |
| `address.street` | string | Street address | ❌ |
| `address.suite` | string | Suite/apartment number | ❌ |
| `address.city` | string | City name | ✅ |
| `address.zipcode` | string | Postal code | ❌ |
| `address.geo.lat` | string | Latitude coordinate | ❌ |
| `address.geo.lng` | string | Longitude coordinate | ❌ |
| `phone` | string | Phone number | ❌ |
| `website` | string | Personal website | ❌ |
| `company.name` | string | Company name | ✅ |
| `company.catchPhrase` | string | Company slogan | ❌ |
| `company.bs` | string | Company business | ❌ |

## JavaScript Functions

### Global Variables

#### `http`
```javascript
const http = new XMLHttpRequest()
```
- **Type**: `XMLHttpRequest`
- **Scope**: Global
- **Purpose**: HTTP client for API communication
- **Lifecycle**: Created on script load, persists throughout application

### Core Functions

#### Request Initialization
```javascript
http.open('get','https://jsonplaceholder.typicode.com/users')
```
- **Purpose**: Configures the HTTP request
- **Parameters**:
  - `method`: `'get'` - HTTP method
  - `url`: `'https://jsonplaceholder.typicode.com/users'` - API endpoint
- **Returns**: `void`
- **Side Effects**: Prepares XMLHttpRequest object for sending

#### Request Execution
```javascript
http.send()
```
- **Purpose**: Sends the configured HTTP request
- **Parameters**: None
- **Returns**: `void`
- **Side Effects**: Initiates network request, triggers state changes

### Event Handlers

#### State Change Handler
```javascript
http.onreadystatechange = () => {
    console.log(http.readyState);
    if(http.readyState==4){
        // Response processing logic
    }
}
```

**Function Signature**:
- **Type**: Arrow function
- **Parameters**: None (accesses `http` via closure)
- **Returns**: `void`
- **Triggers**: On every `readyState` change

**ReadyState Values**:
| Value | Constant | Description |
|-------|----------|-------------|
| 0 | UNSENT | Client created, `open()` not called |
| 1 | OPENED | `open()` has been called |
| 2 | HEADERS_RECEIVED | `send()` called, headers received |
| 3 | LOADING | Downloading, `responseText` has partial data |
| 4 | DONE | Operation complete |

#### Response Processing Logic

```javascript
if(http.status>=200 && http.status<300){
    let users=JSON.parse(http.responseText)
    console.log(users);
    
    users.forEach(item => {
        result.innerHTML +=`
        <tr>
            <td>${item.id}</td>
            <td>${item.name}</td>
            <td>${item.email}</td>
            <td> ${item.address.city}</td>
            <td>${item.company.name}</td>
        </tr>` 
    });
}
```

**Success Path (200-299)**:
- **Condition**: `http.status>=200 && http.status<300`
- **Process**:
  1. Parse JSON response: `JSON.parse(http.responseText)`
  2. Log users array to console
  3. Iterate through users with `forEach()`
  4. Generate HTML table rows
  5. Append to DOM via `innerHTML +=`

**Data Extraction**:
- `item.id` → User ID
- `item.name` → Full name
- `item.email` → Email address
- `item.address.city` → City (nested property)
- `item.company.name` → Company name (nested property)

## DOM Manipulation

### Element References

#### Result Container
```javascript
result.innerHTML += "content"
```
- **Element**: `<tbody id="result">`
- **Access Method**: Direct global reference (not `document.getElementById`)
- **Purpose**: Container for dynamically generated table rows

### Content Generation

#### Template Literal Structure
```javascript
`<tr>
    <td>${item.id}</td>
    <td>${item.name}</td>
    <td>${item.email}</td>
    <td> ${item.address.city}</td>
    <td>${item.company.name}</td>
</tr>`
```

**Template Features**:
- **Syntax**: ES6 template literals with `${}` interpolation
- **Structure**: HTML table row with 5 cells
- **Data Binding**: Direct property access with dot notation
- **Whitespace**: Extra space before city field (` ${item.address.city}`)

#### DOM Update Strategy
```javascript
result.innerHTML += templateString
```
- **Method**: Concatenative assignment (`+=`)
- **Performance**: O(n²) complexity for large datasets
- **Alternative**: More efficient approach would use `DocumentFragment`

## Error Handling

### Network Errors

#### HTTP Status Errors
```javascript
else{
    console.log('no proper response');
}
```
- **Trigger**: HTTP status outside 200-299 range
- **Action**: Console logging only
- **User Feedback**: None (silent failure)
- **Recovery**: None implemented

#### Request State Errors
```javascript
else{
    console.log('no response');
}
```
- **Trigger**: `readyState !== 4`
- **Action**: Console logging only
- **Context**: This branch may be misleading as it triggers during normal loading states

### Potential Runtime Errors

#### JSON Parsing Errors
```javascript
let users=JSON.parse(http.responseText)
```
- **Risk**: Malformed JSON response
- **Current Handling**: None (will throw exception)
- **Recommended**: Wrap in try-catch block

#### Property Access Errors
```javascript
${item.address.city}
${item.company.name}
```
- **Risk**: Undefined nested properties
- **Current Handling**: None (will display "undefined")
- **Recommended**: Add null checks or optional chaining

#### DOM Access Errors
```javascript
result.innerHTML += content
```
- **Risk**: Element with id "result" not found
- **Current Handling**: None (will throw exception)
- **Recommended**: Verify element exists before manipulation

## Performance Considerations

### Current Implementation Issues

1. **String Concatenation**: Using `innerHTML +=` causes DOM re-parsing
2. **No Debouncing**: No protection against rapid successive calls
3. **Memory Leaks**: XMLHttpRequest object persists indefinitely
4. **No Caching**: Every page load triggers new API request

### Optimization Recommendations

#### Efficient DOM Updates
```javascript
// Instead of: result.innerHTML += row
// Use:
const fragment = document.createDocumentFragment();
users.forEach(item => {
    const row = document.createElement('tr');
    row.innerHTML = `<td>${item.id}</td>...`;
    fragment.appendChild(row);
});
result.appendChild(fragment);
```

#### Error Boundary Implementation
```javascript
try {
    const users = JSON.parse(http.responseText);
    // ... process users
} catch (error) {
    console.error('Failed to parse JSON:', error);
    result.innerHTML = '<tr><td colspan="5">Data format error</td></tr>';
}
```

#### Null Safety
```javascript
<td>${item.address?.city || 'N/A'}</td>
<td>${item.company?.name || 'N/A'}</td>
```

## Browser Compatibility

### Required Features
- **XMLHttpRequest**: IE7+, All modern browsers
- **Template Literals**: ES6 (IE: Not supported, Chrome 41+, Firefox 34+)
- **Arrow Functions**: ES6 (IE: Not supported, Chrome 45+, Firefox 22+)
- **forEach**: ES5 (IE9+, All modern browsers)

### Polyfill Requirements
For IE support, consider:
- Babel for ES6 transpilation
- Template literal polyfill
- Arrow function transpilation