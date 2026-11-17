HTTP Methods describe intent and  HTTP methods express the what of a request and intent and action
what you want to do on the particular of resource, what you want to tell your server that your intention whether you want to fetch some data and particular update or modify data or delete data that is called HTTP Methods 


# Routing 
Where of a request to sent you intent, express, action  perform it and which resource we want to perform it. 

-  Tell to server where we want to go.


#### ~={blue}**Routing** =~
- A route is just a rule that says: if someone requests this URL 
- The role of routing is it expresses the where of a request or do you want to send your intention
- We need to tell the server where do you want to do for.

#### ~={blue}**Static Route**=~
- A static route is a route with a fixed, unchanging URL pattern It's a constant string that always points to the same resource collection or endpoint. 
```jsx
// Static routes have exact matches
app.get('/home', homeHandler);        // ✅ Only matches "/home"
app.get('/about', aboutHandler);      // ✅ Only matches "/about"
app.get('/contact', contactHandler);  // ✅ Only matches "/contact"
```

####  ~={blue} ``Dynamic Routes & Path Parameters``=~
##### 1. Dynamic routes 
- A dynamic route is a route where part of the URL can change based on input.
```Node.js
app.get('/api/users/:id/posts/:slug', getUser);
```

##### 2. Path Parameter
- A path parameter is a variable inside the URL path. They are used to identify a **specific, unique resource**.
```ruby
Route:    /api/users/:id/posts/:slug
URL:      /api/users/42/posts/hello-world
Extracted: 
	{ 
		id: "42", 
		slug: "hello-world" 
	}
```


#### ~={red}**`Query Route`**=~
- Query parameters are key-value pairs appended to a URL after `?` used to pass additional data to the server. 
```ruby
https://example.com/api/users?name=john&age=25&active=true
           └───┬───┘ └─┬──┘ └─────────────┬─────────────┘
               |        |                  |
           Base URL    Path          Query Parameters
                                     (key=value pairs)
```

### Pattens
1. ~={red}Filtering=~
	-  We want to fetch users with a specific role:
```js
//                    GET /api/users?role=admin

app.get('/api/users', (req, res) => {  
const role = req.query.role;  
const users = getAllUsers().filter(user => user.role === role);  
res.json(users);  
});
```

2. ~={red}Sorting=~
	-  We want to get product category sorted by `price` 
```js
//                   URL: /api/products?sort=price

app.get('/api/products', async (req, res) => {
  let sortBy = { name: 1 };  // Default: sort by name ascending
  
  // Just check the sort parameter
  if (req.query.sort === 'price') {
    sortBy = { price: -1 };  // Price: high to low
  } else if (req.query.sort === 'newest') {
    sortBy = { createdAt: -1 };  // Newest first
  }
  // If no sort parameter or unknown value, use default
  
  const products = await Product.find().sort(sortBy);
  res.json(products);
});
```
## ~={red}`OR`=~

```js
//            URL: /api/products?sort=price&order=desc

app.get('/api/products', async (req, res) => {
  const { sort = 'name', order = 'desc' } = req.query;
  
  // Simple sorting
  let sortField = 'name';  // Default sort field
  let sortDirection = -1;  // Default: descending (-1)
  
  // Check if user provided a sort field
  if (sort === 'price') sortField = 'price';
  if (sort === 'date') sortField = 'createdAt';
  if (sort === 'popularity') sortField = 'views';
  
  // Check sort direction
  if (order === 'asc') sortDirection = 1;
  
  // Create sort object
  const sortBy = {};
  sortBy[sortField] = sortDirection;
  
  const products = await Product.find().sort(sortBy);
  res.json(products);
});
```

 3.  ~={red}Pagination=~
	 - Requesting paginated results:
```js
//             GET /api/products?page=2&limit=10

// URL: /api/products?page=1&limit=5
app.get('/api/products', async (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 10;
  const skip = (page - 1) * limit;
  
  // Get both products and total count
  const products = await Product.find()
    .skip(skip)
    .limit(limit);
    
  const totalProducts = await Product.countDocuments();
  
  res.json({
    data: products,
    total: totalProducts,
    page: page,
    limit: limit,
    totalPages: Math.ceil(totalProducts / limit)
  });
});
```

## ~={red}`OR`=~

```js
app.get('/api/products', (req, res) => {  
	const page = parseInt(req.query.page) || 1;  
	const limit = parseInt(req.query.limit) || 10;  
	const startIndex = (page - 1) * limit;  
	const endIndex = page * limit;
	
	const products = getAllProducts().slice(startIndex, endIndex);  
	res.json(products);  
});
```

4. ~={red}Search=~
	- Searching for something
```js
// URL: /api/users?role=admin
app.get('/api/users', async (req, res) => {
  const { role } = req.query;
  
  const filter = role ? { role } : {};
  const users = await User.find(filter);
  
  res.json(users);
});
```

## ~={red}`OR`=~

```js
//              GET /api/books?search=javascript

app.get('/api/books', (req, res) => {  
	const search = req.query.search?.toLowerCase();  
	const books = getAllBooks().filter(book =>  
		book.title.toLowerCase().includes(search)  
	);  
	
	res.json(books);  
});
```

5. Optional Feature Flags / Toggles
```js
//GET /api/data?includeMeta=true

app.get('/api/data', (req, res) => {  
	const includeMeta = req.query.includeMeta === 'true';  
	const data = getData();  
	
	if (includeMeta) {  
		res.json({ data, meta: { timestamp: Date.now() } });  
	} else {  
		res.json(data);  
	}  
});
```

#### ~={blue}**`Nested Routes`**=~.
- Nested routes are routes that represent hierarchical relationships between  parent resources and child's  or multi child's resources .

```rust
/users/:userId/orders             # All orders by a user
/users/:userId/orders/:orderId    # Specific order by a user
/users/:userId/orders/:orderId/items  # Items in an order
```


#### ~={blue}**`route versioning and deprecation`**=~.
- Evolve API versions allows for braking changes in new version ( V2 ) while maintaining older version (V1) for existing clients 
- This provides a migration window for front-end engineers to update their applications to the new API structure. Eventually, older versions can be deprecated and removed.

```ruby
GET /api/v1/users
GET /api/v2/users
```

#### ~={blue}**`Catch-All Routes`**=~.
- Catch-All Routes matches any request that doesn't match first routes.
	-  404 pages
	- API fallback responses
```js
app.get("*", (req, res) => {
  res.status(404).send("Page not found");
});
```

```js
app.use((req, res) => {
  res.status(404).json({
    error: "Route not found"
  });
});
```


