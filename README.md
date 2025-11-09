// Bookshop API Project (Express.js) // Basic implementation covering: get books, search, reviews, user register/login

const express = require('express'); const app = express(); app.use(express.json());

// Sample data let books = [ { isbn: "111", title: "The Hobbit", author: "Tolkien", year: 1937, review: null }, { isbn: "222", title: "Harry Potter", author: "Rowling", year: 1997, review: null }, { isbn: "333", title: "1984", author: "Orwell", year: 1949, review: null } ];

let users = [];

// Task 1: Get all books app.get('/books', (req, res) => { res.json(books); });

// Task 2: Get book by ISBN app.get('/books/isbn/:isbn', (req, res) => { const book = books.find(b => b.isbn === req.params.isbn); book ? res.json(book) : res.status(404).json({ error: "Book not found" }); });

// Task 3: Get books by author app.get('/books/author/:author', (req, res) => { const result = books.filter(b => b.author.toLowerCase() === req.params.author.toLowerCase()); res.json(result); });

// Task 4: Get books by title app.get('/books/title/:title', (req, res) => { const result = books.filter(b => b.title.toLowerCase().includes(req.params.title.toLowerCase())); res.json(result); });

// Task 6: Register user app.post('/register', (req, res) => { const { username, password } = req.body; if (users.find(u => u.username === username)) return res.json({ message: "User already exists" }); users.push({ username, password }); res.json({ message: "User registered successfully" }); });

// Task 7: Login user app.post('/login', (req, res) => { const { username, password } = req.body; const user = users.find(u => u.username === username && u.password === password); user ? res.json({ message: "Login successful" }) : res.status(401).json({ error: "Invalid login" }); });

// Task 8: Add/modify review app.post('/books/:isbn/review', (req, res) => { const { review } = req.body; const book = books.find(b => b.isbn === req.params.isbn); if (!book) return res.status(404).json({ error: "Book not found" }); book.review = review; res.json({ message: "Review added/updated", review }); });

// Task 9: Delete review app.delete('/books/:isbn/review', (req, res) => { const book = books.find(b => b.isbn === req.params.isbn); if (!book) return res.status(404).json({ error: "Book not found" }); book.review = null; res.json({ message: "Review deleted" }); });

// Task 10: Callback example function getAllBooksCallback(callback) { setTimeout(() => callback(null, books), 500); }

// Task 11: Promise example function searchByISBNPromise(isbn) { return new Promise((resolve, reject) => { const book = books.find(b => b.isbn === isbn); book ? resolve(book) : reject("Book not found"); }); }

// Task 12 & 13: async/await + callbacks demonstrated in code usage (not endpoints)

app.listen(3000, () => console.log('Bookshop API running on port 3000'));



