# User Details Display Application

A simple, responsive web application that fetches and displays user information from the JSONPlaceholder API in a clean, Bootstrap-styled table format.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Component Documentation](#component-documentation)
- [JavaScript Functions](#javascript-functions)
- [Usage Instructions](#usage-instructions)
- [Code Examples](#code-examples)
- [Browser Support](#browser-support)
- [Contributing](#contributing)

## 🔍 Overview

This application demonstrates a simple implementation of fetching data from an external REST API and displaying it in a user-friendly format. It uses vanilla JavaScript with XMLHttpRequest to retrieve user data from JSONPlaceholder and renders it in a responsive Bootstrap table.

## ✨ Features

- **Real-time Data Fetching**: Retrieves live user data from JSONPlaceholder API
- **Responsive Design**: Mobile-friendly layout using Bootstrap 5.3.3
- **Clean UI**: Modern styling with custom Google Fonts (Anek Devanagari)
- **Error Handling**: Basic error handling for API requests
- **No Dependencies**: Uses vanilla JavaScript (no frameworks required)

## 🛠 Technology Stack

- **HTML5**: Semantic markup structure
- **CSS3**: Custom styling with Google Fonts integration
- **JavaScript (ES6)**: Vanilla JavaScript for API interactions
- **Bootstrap 5.3.3**: Responsive framework via CDN
- **Google Fonts**: Anek Devanagari font family

## 📁 Project Structure

```
/
├── index.html          # Main application file
└── README.md          # This documentation file
```

## 🔌 API Documentation

### External API Endpoint

The application integrates with the JSONPlaceholder API, a free fake REST API for testing and prototyping.

#### Endpoint Details

- **URL**: `https://jsonplaceholder.typicode.com/users`
- **Method**: `GET`
- **Response Format**: `JSON`
- **Authentication**: None required

#### Response Schema

```json
[
  {
    "id": number,
    "name": string,
    "email": string,
    "address": {
      "city": string,
      "street": string,
      "suite": string,
      "zipcode": string,
      "geo": {
        "lat": string,
        "lng": string
      }
    },
    "company": {
      "name": string,
      "catchPhrase": string,
      "bs": string
    },
    "phone": string,
    "website": string,
    "username": string
  }
]
```

#### Used Fields

The application extracts and displays the following fields:
- `id`: User identifier
- `name`: Full name
- `email`: Email address  
- `address.city`: City from address object
- `company.name`: Company name from company object

## 🧩 Component Documentation

### HTML Structure

#### Main Container
- **Element**: `<div class="row w-100">`
- **Purpose**: Bootstrap responsive container
- **Layout**: 12-column grid system (2-8-2 layout)

#### Data Table
- **Element**: `<table class="table table-bordered mt-5 shadow">`
- **Classes**: 
  - `table`: Bootstrap table styling
  - `table-bordered`: Adds borders to table cells
  - `mt-5`: Top margin of 3rem
  - `shadow`: Adds subtle shadow effect

#### Table Header
- **Element**: `<thead>`
- **Styling**: Blue headers with white text (`bg-info text-light`)
- **Columns**: id, name, email, city, company_name

#### Table Body
- **Element**: `<tbody id="result">`
- **Purpose**: Container for dynamically generated user rows
- **ID**: `result` (used by JavaScript for DOM manipulation)

### CSS Styling

#### Global Styles
```css
* {
    font-family: "Anek Devanagari", sans-serif;
}
```
- **Selector**: Universal selector
- **Property**: Applies Anek Devanagari font to all elements
- **Fallback**: Sans-serif font family

## 📜 JavaScript Functions

### Main Execution Flow

#### XMLHttpRequest Setup
```javascript
const http = new XMLHttpRequest() 
http.open('get','https://jsonplaceholder.typicode.com/users')
http.send()
```

**Description**: Initializes and sends HTTP GET request to fetch user data.

**Parameters**: None

**Returns**: void

#### State Change Handler
```javascript
http.onreadystatechange = () => {
    // Handle response based on readyState and status
}
```

**Description**: Event handler that processes API response based on request state.

**Parameters**: None (uses closure to access `http` object)

**Returns**: void

### Response Processing Logic

#### Success Path (Status 200-299)
```javascript
if(http.status>=200 && http.status<300){
    let users=JSON.parse(http.responseText)
    users.forEach(item => {
        result.innerHTML += `<tr>...</tr>`
    });
}
```

**Description**: Parses JSON response and generates table rows for each user.

**Data Processing**:
- Parses JSON string to JavaScript object
- Iterates through user array
- Generates HTML table rows with template literals

#### Error Handling
```javascript
else{
    console.log('no proper response');
}
```

**Description**: Basic error logging for unsuccessful HTTP responses.

### DOM Manipulation

#### Dynamic Content Generation
```javascript
result.innerHTML += `
<tr>
    <td>${item.id}</td>
    <td>${item.name}</td>
    <td>${item.email}</td>
    <td>${item.address.city}</td>
    <td>${item.company.name}</td>
</tr>`
```

**Description**: Creates table rows using template literals and object property access.

**Template Variables**:
- `${item.id}`: User ID
- `${item.name}`: User full name
- `${item.email}`: User email address
- `${item.address.city}`: User's city from nested address object
- `${item.company.name}`: Company name from nested company object

## 📖 Usage Instructions

### Running the Application

1. **Local Development**:
   ```bash
   # Option 1: Open directly in browser
   open index.html
   
   # Option 2: Use a local server
   python -m http.server 8000
   # Then visit http://localhost:8000
   ```

2. **Production Deployment**:
   - Upload `index.html` to any web server
   - No build process required
   - Ensure internet access for CDN resources

### Browser Requirements

- **Internet Connection**: Required for CDN resources and API calls
- **JavaScript**: Must be enabled
- **Modern Browser**: Supports ES6 features

### Expected Behavior

1. **Page Load**: Application automatically fetches user data
2. **Loading State**: Console logs show XMLHttpRequest state changes
3. **Success State**: Table populates with 10 user records
4. **Error State**: Error messages logged to console

## 💡 Code Examples

### Customizing the API Endpoint

```javascript
// Replace the API URL to use different data source
const http = new XMLHttpRequest() 
http.open('get','https://your-api.com/users')  // Custom endpoint
http.send()
```

### Adding New Table Columns

```javascript
// 1. Add header column in HTML
<th class="bg-info text-light">phone</th>

// 2. Add data column in JavaScript template
<td>${item.phone}</td>
```

### Implementing Loading State

```javascript
// Add before sending request
document.getElementById('result').innerHTML = '<tr><td colspan="5">Loading...</td></tr>';

// Clear loading state on response
if(http.readyState==4){
    document.getElementById('result').innerHTML = '';
    // ... rest of response handling
}
```

### Adding Error Display

```javascript
else{
    document.getElementById('result').innerHTML = 
        '<tr><td colspan="5" class="text-danger">Failed to load user data</td></tr>';
}
```

### Converting to Fetch API

```javascript
// Modern alternative using Fetch API
fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => response.json())
    .then(users => {
        const result = document.getElementById('result');
        result.innerHTML = '';
        users.forEach(item => {
            result.innerHTML += `
            <tr>
                <td>${item.id}</td>
                <td>${item.name}</td>
                <td>${item.email}</td>
                <td>${item.address.city}</td>
                <td>${item.company.name}</td>
            </tr>`;
        });
    })
    .catch(error => {
        console.error('Error fetching users:', error);
        document.getElementById('result').innerHTML = 
            '<tr><td colspan="5" class="text-danger">Error loading data</td></tr>';
    });
```

## 🌐 Browser Support

- **Chrome**: 60+
- **Firefox**: 55+
- **Safari**: 12+
- **Edge**: 79+
- **Opera**: 47+

## 🐛 Troubleshooting

### Common Issues

1. **Empty Table**:
   - Check console for network errors
   - Verify internet connection
   - Check browser developer tools Network tab

2. **Styling Issues**:
   - Verify Bootstrap CDN is loading
   - Check console for 404 errors on CSS resources

3. **Font Not Loading**:
   - Verify Google Fonts CDN access
   - Check for adblockers blocking font requests

### Debug Mode

Enable verbose logging by adding:
```javascript
console.log('Request state:', http.readyState);
console.log('Response status:', http.status);
console.log('Response data:', http.responseText);
```

## 🔧 Contributing

### Code Style Guidelines

- Use consistent indentation (4 spaces)
- Follow semantic HTML structure
- Use descriptive variable names
- Add comments for complex logic
- Maintain responsive design principles

### Enhancement Ideas

- Add search/filter functionality
- Implement data sorting
- Add pagination for large datasets
- Include user detail modal views
- Add data refresh button
- Implement offline caching

---

**Last Updated**: December 2024  
**Version**: 1.0.0  
**License**: MIT