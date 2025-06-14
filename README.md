// QUICKGROCER PROJECT OVERVIEW

// === Backend: server/server.js ===
const express = require("express");
const cors = require("cors");
const app = express();
const PORT = 5000;

app.use(cors());
app.use(express.json());

const products = [
  { id: 1, name: "Tomato", price: 20, category: "Vegetables" },
  { id: 2, name: "Banana", price: 40, category: "Fruits" },
  { id: 3, name: "Rice", price: 50, category: "Groceries" }
];

const admin = { username: "admin", password: "admin123" };

app.post("/api/login", (req, res) => {
  const { username, password } = req.body;
  if (username === admin.username && password === admin.password) {
    return res.json({ success: true });
  }
  return res.status(401).json({ success: false });
});

app.get("/api/products", (req, res) => {
  res.json(products);
});

app.listen(PORT, () => console.log(`Server running on http://localhost:${PORT}`));


// === Frontend: client/src/App.jsx ===
import React, { useState, useEffect } from "react";
import axios from "axios";

function App() {
  const [products, setProducts] = useState([]);
  const [loginData, setLoginData] = useState({ username: "", password: "" });
  const [isAdmin, setIsAdmin] = useState(false);

  useEffect(() => {
    axios.get("http://localhost:5000/api/products").then(res => {
      setProducts(res.data);
    });
  }, []);

  const handleLogin = () => {
    axios.post("http://localhost:5000/api/login", loginData).then(res => {
      if (res.data.success) setIsAdmin(true);
      else alert("Login failed");
    });
  };

  return (
    <div className="p-6 font-sans">
      <h1 className="text-3xl font-bold">QuickGrocer</h1>
      <p className="text-lg text-gray-600 mb-4">Grocery Delivery in Dhenkanal</p>

      {!isAdmin && (
        <div className="mb-4">
          <input
            placeholder="Username"
            onChange={(e) => setLoginData({ ...loginData, username: e.target.value })}
            className="border p-2 mr-2"
          />
          <input
            placeholder="Password"
            type="password"
            onChange={(e) => setLoginData({ ...loginData, password: e.target.value })}
            className="border p-2 mr-2"
          />
          <button onClick={handleLogin} className="bg-blue-500 text-white p-2 rounded">
            Admin Login
          </button>
        </div>
      )}

      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        {products.map((p) => (
          <div key={p.id} className="border p-4 rounded shadow">
            <h2 className="text-xl font-semibold">{p.name}</h2>
            <p>₹{p.price}</p>
            <p className="text-gray-500">{p.category}</p>
            <a href="tel:7600005959" className="text-green-600 font-bold">Order Now</a>
          </div>
        ))}
      </div>
    </div>
  );
}

export default App;


// === Frontend: client/src/index.js ===
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);


// === Frontend: client/index.html ===
<!-- Inside client/public/index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>QuickGrocer</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>


// === README.md ===
# QuickGrocer

**Steps to Run:**

1. Install dependencies:
```bash
cd server && npm install
cd ../client && npm install
```

2. Start backend:
```bash
cd server && node server.js
```

3. Start frontend:
```bash
cd client && npm start
```

4. Visit: `http://localhost:3000`

---
Admin Login:
- **Username**: admin
- **Password**: admin123

Call ordering number: **7600005959**

---

📦 This includes:
- Homepage
- Product listing
- Admin login
- Order via call
- Responsive layout
- Built with JavaScript only
