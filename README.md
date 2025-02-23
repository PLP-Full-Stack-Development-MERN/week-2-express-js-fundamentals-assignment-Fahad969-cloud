**Week 2: Express.js Fundamentals Assignment**

**Objective:**

- Apply Express.js concepts learned throughout the week.
- Develop hands-on experience with creating routes, middleware, and API endpoints.
- Understand and implement RESTful APIs.

**Instructions:**

1. **Setup Express.js Project:**

   - Install Node.js using NVM.
   - Create a new project folder named `express-assignment`.
   - Initialize a Node.js project using:
     ```sh
     npm init -y
     ```
   - Install necessary dependencies:
     ```sh
     npm install express dotenv
     ```

2. **Project Structure:**

   - Organize your project files with a clear folder structure:
     ```
     express-assignment/
     │-- routes/
     │    ├── userRoutes.js
     │    ├── productRoutes.js
     │-- middleware/
     │    ├── logger.js
     │-- controllers/
     │    ├── userController.js
     │    ├── productController.js
     │-- index.js
     │-- package.json
     │-- README.md
     │-- .env
     ```

3. **Create Routes:**

   - Create `userRoutes.js` and `productRoutes.js` inside the `routes/` folder.
     USER ROUTES
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
    res.send('Get all users');
});

router.get('/:id', (req, res) => {
    res.send(`Get user with ID: ${req.params.id}`);
});

router.post('/', (req, res) => {
    res.send('Create a new user');
});

module.exports = router;




const express = require('express');
const router = express.Router();

// Example product routes
router.get('/', (req, res) => {
    res.send('Get all products');
});

router.get('/:id', (req, res) => {
    res.send(`Get product with ID: ${req.params.id}`);
});

router.post('/', (req, res) => {
    res.send('Create a new product');
});

module.exports = router;


- Implement RESTful routes for users and products (GET, POST, PUT, DELETE)
   - Ensure proper usage of route parameters and query strings.
     const express = require('express');
const router = express.Router();


router.get('/', (req, res) => {
    const { role, status } = req.query; // Extract query parameters
    if (role || status) {
        res.send(`Get users with role: ${role || 'any'}, status: ${status || 'any'}`);
    } else {
        res.send('Get all users');
    }
});


router.get('/:id', (req, res) => {
    res.send(`Get user with ID: ${req.params.id}`);
});


router.post('/', (req, res) => {
    res.send('Create a new user');
});


router.put('/:id', (req, res) => {
    res.send(`Update user with ID: ${req.params.id}`);
});


router.delete('/:id', (req, res) => {
    res.send(`Delete user with ID: ${req.params.id}`);
});

module.exports = router;





const express = require('express');
const router = express.Router();


router.get('/', (req, res) => {
    const { category, price } = req.query; // Extract query parameters
    if (category || price) {
        res.send(`Get products in category: ${category || 'all'}, with price under: ${price || 'any'}`);
    } else {
        res.send('Get all products');
    }
});


router.get('/:id', (req, res) => {
    res.send(`Get product with ID: ${req.params.id}`);
});


router.post('/', (req, res) => {
    res.send('Create a new product');
});


router.put('/:id', (req, res) => {
    res.send(`Update product with ID: ${req.params.id}`);
});


router.delete('/:id', (req, res) => {
    res.send(`Delete product with ID: ${req.params.id}`);
});

module.exports = router;



3. **Implement Middleware:**

   - Create a custom middleware function in `middleware/logger.js` to log request details (method, URL, timestamp).
   - Apply middleware globally to all routes.
const logger = (req, res, next) => {
    const timestamp = new Date().toISOString();
    console.log(`[${timestamp}] ${req.method} ${req.url}`);
    next(); 
};

module.exports = logger;




const express = require('express');
const app = express();


const logger = require('./middleware/logger');


app.use(logger);


app.use(express.json());


const userRoutes = require('./routes/userRoutes');
const productRoutes = require('./routes/productRoutes');


app.use('/users', userRoutes);
app.use('/products', productRoutes);


const PORT = 3000;
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});

4. **Develop Controllers:**

   - Create controller functions in `controllers/userController.js` and `controllers/productController.js`.
   - Implement business logic to handle requests and responses.
const express = require('express');
const app = express();
const cors = require('cors');
const morgan = require('morgan');



const logger = require('./middleware/logger');


app.use(logger); // Custom logger middleware
app.use(morgan('dev')); // Logs requests in a concise format
app.use(cors()); // Enables CORS for cross-origin requests
app.use(express.json()); // Parse JSON request body


const userRoutes = require('./routes/userRoutes');
const productRoutes = require('./routes/productRoutes');


app.use('/users', userRoutes);
app.use('/products', productRoutes);


app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ message: 'Internal Server Error' });
});


const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(` Server running on port ${PORT}`);
});

5. **Environment Variables:**

   - Use `dotenv` to manage environment variables.
   - Define variables such as `PORT` in the `.env` file and access them inside the application.
const express = require('express');
const dotenv = require('dotenv');
const cors = require('cors');
const morgan = require('morgan');

dotenv.config();

const app = express();

const logger = require('./middleware/logger');

app.use(logger);
app.use(morgan('dev'));
app.use(cors());
app.use(express.json());

const userRoutes = require('./routes/userRoutes');
const productRoutes = require('./routes/productRoutes');

app.use('/users', userRoutes);
app.use('/products', productRoutes);

app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ message: 'Internal Server Error' });
});

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
    console.log(` Server running on port ${PORT} in ${process.env.NODE_ENV} mode`);
});

6. **Error Handling:**

   - Implement a global error-handling middleware to catch and respond to errors gracefully.
const errorHandler = (err, req, res, next) => {
    console.error(`Error: ${err.message}`);

    const statusCode = err.statusCode || 500;
    res.status(statusCode).json({
        message: err.message || 'Internal Server Error',
        stack: process.env.NODE_ENV === 'development' ? err.stack : undefined
    });
};

module.exports = errorHandler;

7. **Testing:**

   - Run the server using:
     ```sh
     node index.js
     ```
   - Test API endpoints using Postman or cURL.
   - Verify routes, middleware functionality, and error handling.
{
    "message": "Something went wrong!",
    "stack": "Error: Something went wrong! at ..."
}


{
    "message": "Something went wrong!"
}

8. **Documentation:**

   - Add a `README.md` with instructions on setting up and running the project.
   - Document available API endpoints with descriptions and example requests.

9. **Submission:**

   - Push your code to your GitHub repository.

**Evaluation Criteria:**

- Correct implementation of Express routes and middleware.
- Proper error handling and logging.
- Clean project structure and code organization.
- Detailed documentation with clear instructions.
- Successful testing of all endpoints.

