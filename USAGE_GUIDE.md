# Usage Guide

## Table of Contents
- [Quick Start](#quick-start)
- [Development Setup](#development-setup)
- [Deployment Options](#deployment-options)
- [Customization Examples](#customization-examples)
- [Integration Patterns](#integration-patterns)
- [Troubleshooting](#troubleshooting)
- [Performance Optimization](#performance-optimization)

## Quick Start

### Prerequisites
- Modern web browser (Chrome 60+, Firefox 55+, Safari 12+, Edge 79+)
- Internet connection (for CDN resources and API calls)
- Text editor (for customization)

### Running the Application

#### Method 1: Direct Browser Open
```bash
# Navigate to project directory
cd /path/to/user-details-app

# Open in default browser (macOS)
open index.html

# Open in default browser (Linux)
xdg-open index.html

# Open in default browser (Windows)
start index.html
```

#### Method 2: Local Web Server
```bash
# Python 3.x
python -m http.server 8000

# Python 2.x
python -m SimpleHTTPServer 8000

# Node.js (if npx is available)
npx serve .

# PHP
php -S localhost:8000
```

Then visit: `http://localhost:8000`

#### Method 3: VS Code Live Server
1. Install "Live Server" extension
2. Right-click `index.html`
3. Select "Open with Live Server"

### Expected Behavior
1. **Page Load**: Displays "USERS DETAILS" heading
2. **API Request**: Automatically fetches data from JSONPlaceholder
3. **Data Display**: Populates table with 10 user records
4. **Responsive**: Adapts to different screen sizes

## Development Setup

### Project Structure
```
user-details-app/
├── index.html              # Main application file
├── README.md              # Project documentation
├── API_REFERENCE.md       # API documentation
├── COMPONENTS.md          # Component documentation
└── USAGE_GUIDE.md         # This usage guide
```

### Development Tools

#### Recommended IDE Extensions
- **HTML/CSS**: Auto formatting, syntax highlighting
- **JavaScript**: ESLint, Prettier for code quality
- **Bootstrap**: IntelliSense for class suggestions
- **Live Server**: Real-time browser refresh

#### Browser Developer Tools
```javascript
// Console commands for debugging
console.log('XMLHttpRequest state:', http.readyState);
console.log('Response status:', http.status);
console.log('Response data:', http.responseText);

// Inspect the result element
console.log('Table body:', document.getElementById('result'));
```

## Deployment Options

### Static Hosting Platforms

#### GitHub Pages
```bash
# 1. Create GitHub repository
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/username/user-details-app.git
git push -u origin main

# 2. Enable GitHub Pages in repository settings
# 3. Access via: https://username.github.io/user-details-app
```

#### Netlify
```bash
# Option 1: Drag and drop deployment
# Visit netlify.com and drag project folder

# Option 2: Git integration
# Connect GitHub repository to Netlify
# Automatic deployments on push
```

#### Vercel
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy from project directory
vercel

# Follow prompts for configuration
```

#### Traditional Web Hosting
```bash
# Upload files via FTP/SFTP
# Ensure index.html is in document root
# No server-side configuration needed
```

### CDN Considerations

#### Production Deployment
- Verify CDN availability in target regions
- Consider hosting Bootstrap/fonts locally for reliability
- Implement fallback strategies for CDN failures

#### Content Security Policy (CSP)
```html
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; 
               style-src 'self' 'unsafe-inline' https://cdn.jsdelivr.net https://fonts.googleapis.com; 
               font-src 'self' https://fonts.gstatic.com; 
               script-src 'self' https://cdn.jsdelivr.net; 
               connect-src 'self' https://jsonplaceholder.typicode.com;">
```

## Customization Examples

### Changing API Endpoint

#### Custom User API
```javascript
// Replace in index.html script section
const http = new XMLHttpRequest()
http.open('get', 'https://your-api.com/users')  // Custom endpoint
http.send()

// Ensure response format matches expected structure
```

#### Adding Request Headers
```javascript
const http = new XMLHttpRequest()
http.open('get', 'https://api.example.com/users')
http.setRequestHeader('Authorization', 'Bearer your-token')
http.setRequestHeader('Content-Type', 'application/json')
http.send()
```

### Modifying Table Structure

#### Adding New Columns
```html
<!-- 1. Add header in HTML -->
<thead>
    <tr>
        <th class="bg-info text-light">id</th>
        <th class="bg-info text-light">name</th>
        <th class="bg-info text-light">email</th>
        <th class="bg-info text-light">city</th>
        <th class="bg-info text-light">company_name</th>
        <th class="bg-info text-light">phone</th>  <!-- New column -->
    </tr>
</thead>
```

```javascript
// 2. Update JavaScript template
result.innerHTML += `
<tr>
    <td>${item.id}</td>
    <td>${item.name}</td>
    <td>${item.email}</td>
    <td>${item.address.city}</td>
    <td>${item.company.name}</td>
    <td>${item.phone}</td>  <!-- New data -->
</tr>`
```

#### Custom Data Formatting
```javascript
// Format phone numbers
<td>${item.phone.replace(/(\d{1})-(\d{3})-(\d{3})-(\d{4})/, '($2) $3-$4')}</td>

// Format email as link
<td><a href="mailto:${item.email}">${item.email}</a></td>

// Capitalize city names
<td>${item.address.city.toUpperCase()}</td>
```

### Styling Customizations

#### Custom Color Scheme
```html
<style>
    /* Custom color variables */
    :root {
        --primary-color: #007bff;
        --secondary-color: #6c757d;
        --success-color: #28a745;
        --info-color: #17a2b8;
        --warning-color: #ffc107;
        --danger-color: #dc3545;
    }
    
    /* Override Bootstrap colors */
    .bg-info {
        background-color: var(--primary-color) !important;
    }
    
    .text-info {
        color: var(--primary-color) !important;
    }
</style>
```

#### Alternative Table Styling
```html
<table class="table table-hover table-striped table-sm">
    <!-- table-hover: Hover effects -->
    <!-- table-striped: Alternating row colors -->
    <!-- table-sm: Smaller padding -->
</table>
```

#### Custom Font Integration
```html
<!-- Replace Google Fonts link -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&display=swap" 
      rel="stylesheet">

<style>
    * {
        font-family: "Inter", system-ui, sans-serif;
    }
</style>
```

### Adding Interactive Features

#### Search Functionality
```html
<!-- Add search input above table -->
<div class="mb-3">
    <input type="text" 
           id="searchInput" 
           class="form-control" 
           placeholder="Search users...">
</div>
```

```javascript
// Add search logic
function filterUsers(users, searchTerm) {
    if (!searchTerm) return users;
    
    return users.filter(user => 
        user.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
        user.email.toLowerCase().includes(searchTerm.toLowerCase()) ||
        user.address.city.toLowerCase().includes(searchTerm.toLowerCase()) ||
        user.company.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
}

// Implement search event listener
document.getElementById('searchInput').addEventListener('input', (e) => {
    const searchTerm = e.target.value;
    const filteredUsers = filterUsers(originalUsers, searchTerm);
    renderUsers(filteredUsers);
});
```

#### Sorting Functionality
```html
<!-- Add sorting buttons to headers -->
<th class="bg-info text-light">
    <button class="btn btn-link text-light p-0" onclick="sortBy('name')">
        name <i class="fas fa-sort"></i>
    </button>
</th>
```

```javascript
let sortDirection = 'asc';
let sortField = '';

function sortBy(field) {
    sortDirection = (sortField === field && sortDirection === 'asc') ? 'desc' : 'asc';
    sortField = field;
    
    users.sort((a, b) => {
        let aVal = getNestedValue(a, field);
        let bVal = getNestedValue(b, field);
        
        if (sortDirection === 'asc') {
            return aVal > bVal ? 1 : -1;
        } else {
            return aVal < bVal ? 1 : -1;
        }
    });
    
    renderUsers(users);
}

function getNestedValue(obj, path) {
    return path.split('.').reduce((o, p) => o && o[p], obj);
}
```

## Integration Patterns

### Modern JavaScript (ES6+)

#### Fetch API Implementation
```javascript
// Replace XMLHttpRequest with modern Fetch
async function fetchUsers() {
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const users = await response.json();
        renderUsers(users);
        
    } catch (error) {
        console.error('Failed to fetch users:', error);
        showErrorMessage('Failed to load user data');
    }
}

function renderUsers(users) {
    const result = document.getElementById('result');
    result.innerHTML = '';
    
    users.forEach(user => {
        result.innerHTML += `
        <tr>
            <td>${user.id}</td>
            <td>${user.name}</td>
            <td>${user.email}</td>
            <td>${user.address.city}</td>
            <td>${user.company.name}</td>
        </tr>`;
    });
}

function showErrorMessage(message) {
    const result = document.getElementById('result');
    result.innerHTML = `
    <tr>
        <td colspan="5" class="text-center text-danger">
            <i class="fas fa-exclamation-triangle"></i> ${message}
        </td>
    </tr>`;
}

// Call function on page load
document.addEventListener('DOMContentLoaded', fetchUsers);
```

### Framework Integration

#### React Component Example
```jsx
import React, { useState, useEffect } from 'react';
import 'bootstrap/dist/css/bootstrap.min.css';

function UserTable() {
    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
        fetch('https://jsonplaceholder.typicode.com/users')
            .then(response => response.json())
            .then(data => {
                setUsers(data);
                setLoading(false);
            })
            .catch(err => {
                setError(err.message);
                setLoading(false);
            });
    }, []);
    
    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error}</div>;
    
    return (
        <div className="container">
            <h1 className="text-center mt-5 text-info">USERS DETAILS</h1>
            <table className="table table-bordered mt-5 shadow">
                <thead>
                    <tr>
                        <th className="bg-info text-light">ID</th>
                        <th className="bg-info text-light">Name</th>
                        <th className="bg-info text-light">Email</th>
                        <th className="bg-info text-light">City</th>
                        <th className="bg-info text-light">Company</th>
                    </tr>
                </thead>
                <tbody>
                    {users.map(user => (
                        <tr key={user.id}>
                            <td>{user.id}</td>
                            <td>{user.name}</td>
                            <td>{user.email}</td>
                            <td>{user.address.city}</td>
                            <td>{user.company.name}</td>
                        </tr>
                    ))}
                </tbody>
            </table>
        </div>
    );
}

export default UserTable;
```

#### Vue.js Component Example
```vue
<template>
  <div class="container">
    <h1 class="text-center mt-5 text-info">USERS DETAILS</h1>
    <table class="table table-bordered mt-5 shadow">
      <thead>
        <tr>
          <th class="bg-info text-light">ID</th>
          <th class="bg-info text-light">Name</th>
          <th class="bg-info text-light">Email</th>
          <th class="bg-info text-light">City</th>
          <th class="bg-info text-light">Company</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <td>{{ user.id }}</td>
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.address.city }}</td>
          <td>{{ user.company.name }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'UserTable',
  data() {
    return {
      users: []
    }
  },
  async mounted() {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/users');
      this.users = await response.json();
    } catch (error) {
      console.error('Failed to fetch users:', error);
    }
  }
}
</script>
```

## Troubleshooting

### Common Issues and Solutions

#### Issue: Table Not Loading
**Symptoms**: Empty table, no data displayed

**Possible Causes**:
1. Network connectivity issues
2. API endpoint unavailable
3. CORS policy blocking request
4. JavaScript disabled

**Solutions**:
```javascript
// Debug network requests
console.log('XMLHttpRequest state:', http.readyState);
console.log('Response status:', http.status);

// Check for CORS issues
if (http.status === 0) {
    console.log('Possible CORS issue or network error');
}

// Verify API endpoint
fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => console.log('API status:', response.status))
    .catch(error => console.log('API error:', error));
```

#### Issue: Styling Not Applied
**Symptoms**: Plain HTML appearance, no Bootstrap styling

**Possible Causes**:
1. Bootstrap CDN not loaded
2. Network issues blocking CSS
3. CSP blocking external stylesheets

**Solutions**:
```html
<!-- Check if Bootstrap loaded -->
<script>
    if (typeof bootstrap === 'undefined') {
        console.error('Bootstrap not loaded');
    }
</script>

<!-- Fallback to local Bootstrap -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" 
      rel="stylesheet"
      onerror="this.onerror=null;this.href='./css/bootstrap.min.css';">
```

#### Issue: Mobile Layout Problems
**Symptoms**: Poor mobile display, horizontal scrolling

**Solutions**:
```html
<!-- Add responsive table wrapper -->
<div class="table-responsive">
    <table class="table table-bordered mt-5 shadow">
        <!-- table content -->
    </table>
</div>

<!-- Ensure viewport meta tag -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### Debugging Tools

#### Console Debugging
```javascript
// Add comprehensive logging
const http = new XMLHttpRequest()
http.open('get','https://jsonplaceholder.typicode.com/users')

// Track all state changes
http.onreadystatechange = () => {
    console.log(`State: ${http.readyState}, Status: ${http.status}`);
    
    if(http.readyState === 4) {
        console.log('Response received');
        console.log('Status code:', http.status);
        console.log('Response text length:', http.responseText.length);
        
        if(http.status >= 200 && http.status < 300) {
            try {
                const users = JSON.parse(http.responseText);
                console.log('Parsed users:', users.length);
                console.log('First user:', users[0]);
            } catch (error) {
                console.error('JSON parse error:', error);
            }
        }
    }
}

http.send()
```

#### Network Debugging
```bash
# Test API endpoint directly
curl -I https://jsonplaceholder.typicode.com/users

# Check response headers
curl -v https://jsonplaceholder.typicode.com/users
```

## Performance Optimization

### Loading Performance

#### Critical Resource Optimization
```html
<!-- Preload critical resources -->
<link rel="preload" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" as="style">
<link rel="preload" href="https://fonts.googleapis.com/css2?family=Anek+Devanagari:wght@100..800&display=swap" as="style">

<!-- DNS prefetch for external domains -->
<link rel="dns-prefetch" href="//cdn.jsdelivr.net">
<link rel="dns-prefetch" href="//fonts.googleapis.com">
<link rel="dns-prefetch" href="//jsonplaceholder.typicode.com">
```

#### Lazy Loading Implementation
```html
<!-- Defer non-critical JavaScript -->
<script defer src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>

<!-- Async load non-critical fonts -->
<link rel="preload" href="https://fonts.googleapis.com/css2?family=Anek+Devanagari:wght@100..800&display=swap" 
      as="style" onload="this.onload=null;this.rel='stylesheet'">
```

### Runtime Performance

#### Efficient DOM Updates
```javascript
// Instead of repeated innerHTML +=
function renderUsersEfficiently(users) {
    const fragment = document.createDocumentFragment();
    
    users.forEach(user => {
        const row = document.createElement('tr');
        row.innerHTML = `
            <td>${user.id}</td>
            <td>${user.name}</td>
            <td>${user.email}</td>
            <td>${user.address.city}</td>
            <td>${user.company.name}</td>
        `;
        fragment.appendChild(row);
    });
    
    const result = document.getElementById('result');
    result.innerHTML = '';
    result.appendChild(fragment);
}
```

#### Caching Strategy
```javascript
// Implement simple caching
const cache = new Map();
const CACHE_DURATION = 5 * 60 * 1000; // 5 minutes

async function fetchUsersWithCache() {
    const cacheKey = 'users';
    const cached = cache.get(cacheKey);
    
    if (cached && Date.now() - cached.timestamp < CACHE_DURATION) {
        renderUsers(cached.data);
        return;
    }
    
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        const users = await response.json();
        
        cache.set(cacheKey, {
            data: users,
            timestamp: Date.now()
        });
        
        renderUsers(users);
    } catch (error) {
        console.error('Fetch failed:', error);
        // Try to use stale cache data
        if (cached) {
            renderUsers(cached.data);
        }
    }
}
```

### Bundle Size Optimization

#### Selective Bootstrap Imports
```scss
// Create custom bootstrap.scss
@import "~bootstrap/scss/functions";
@import "~bootstrap/scss/variables";
@import "~bootstrap/scss/mixins";

// Only import needed components
@import "~bootstrap/scss/reboot";
@import "~bootstrap/scss/grid";
@import "~bootstrap/scss/tables";
@import "~bootstrap/scss/utilities";
```

#### Modern JavaScript Modules
```html
<!-- Use ES modules for modern browsers -->
<script type="module">
    import { fetchUsers, renderUsers } from './modules/userService.js';
    
    document.addEventListener('DOMContentLoaded', async () => {
        const users = await fetchUsers();
        renderUsers(users);
    });
</script>

<!-- Fallback for older browsers -->
<script nomodule src="./js/legacy-bundle.js"></script>
```

This completes the comprehensive documentation covering all aspects of the User Details Display Application, including usage instructions, customization examples, and performance optimization strategies.