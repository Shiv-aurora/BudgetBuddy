# Expense Tracker for College Students

![Project Banner](./screenshots/banner.png)

## Overview

**Expense Tracker** is a comprehensive web application designed specifically for college students to manage their finances efficiently. Built with a modern tech stack, the application offers real-time tracking and management of both income and expenses, fostering financial awareness and promoting effective budgeting habits.

### Key Highlights

- **Real-Time Tracking:** Instantly monitor your financial transactions as they occur.
- **User-Friendly Interface:** Intuitive design tailored for ease of use by college students.
- **Comprehensive Dashboard:** Gain insights into your spending patterns with detailed charts and summaries.
- **Secure and Reliable:** Robust backend ensuring data integrity and security.

## Features

### 1. **Real-Time Expense and Income Management**
- **Add Transactions:** Easily log income and expenses with detailed information such as title, amount, category, description, and date.
- **Edit and Delete:** Modify or remove transactions to keep your records up-to-date.
- **Categorization:** Organize transactions into predefined categories like Salary, Freelancing, Groceries, Health, etc., for better clarity.

### 2. **Comprehensive Dashboard**
- **Visual Analytics:** Utilize charts and graphs to visualize your financial data, helping you understand spending habits and income sources.
- **Financial Summary:** View total income, total expenses, and net balance at a glance.
- **Transaction History:** Access a consolidated view of recent transactions for quick reference.

### 3. **Responsive Design**
- **Multi-Device Access:** Seamlessly use the application on desktops, tablets, and mobile devices.
- **Adaptive Layouts:** Optimized for various screen sizes to ensure a consistent user experience.

### 4. **Secure Authentication**
- **User Accounts:** Create and manage personal accounts to keep financial data private and secure.
- **Protected Routes:** Ensure that only authenticated users can access sensitive information.

### 5. **Dynamic UI Components**
- **Animated Orbs:** Enhance visual appeal with interactive background elements.
- **Styled-Components:** Maintain a consistent and scalable styling approach across the application.

### 6. **Efficient Data Management**
- **MongoDB Integration:** Store and retrieve transaction data efficiently with a NoSQL database.
- **Express Routing:** Manage API endpoints for seamless frontend-backend communication.

### 7. **Error Handling and Validation**
- **Form Validation:** Ensure all necessary fields are filled out correctly before submission.
- **Server Error Management:** Gracefully handle and display server-side errors to the user.

## Technologies Used

### Frontend
- **React:** Building dynamic and responsive user interfaces.
- **Styled-Components:** Managing component-level styles with ease.
- **Axios:** Handling HTTP requests to the backend.
- **React Chart.js 2:** Integrating charts for data visualization.
- **React Datepicker:** Providing intuitive date selection for transactions.

### Backend
- **Node.js & Express.js:** Creating a robust and scalable server-side application.
- **MongoDB & Mongoose:** Managing data storage and schema definitions.
- **dotenv:** Handling environment variables securely.
- **CORS:** Enabling cross-origin resource sharing for frontend-backend communication.

### Other Tools
- **Git & GitHub:** Version control and repository management.
- **VS Code:** Development environment.
- **Font Awesome:** Utilizing icons for better UI/UX.

## Architecture

The application follows a **MERN (MongoDB, Express, React, Node.js)** architecture, ensuring a seamless flow of data between the frontend and backend.

### Frontend Structure
- **Components:** Reusable UI elements like Forms, Navigation, Dashboard, etc.
- **Context API:** Managing global state for income and expenses.
- **Styles:** Organized using Styled-Components for modular and maintainable CSS.

### Backend Structure
- **Models:** Defining data schemas for Income and Expenses using Mongoose.
- **Controllers:** Handling business logic for adding, retrieving, and deleting transactions.
- **Routes:** Managing API endpoints for frontend interactions.
- **Database Connection:** Establishing a connection to MongoDB using Mongoose.

## Screenshots

### 1. **Dashboard**

![Dashboard](./screenshots/dashboard.png)

*View your overall financial status with charts and summaries.*

### 2. **Add Income**

![Add Income](./screenshots/add_income.png)

*Easily log your income with necessary details.*

### 3. **Add Expense**

![Add Expense](./screenshots/add_expense.png)

*Track your expenses efficiently by categorizing them.*

## Code Highlights

### Backend: Express Server Setup
javascript:backend/app.js
const express = require('express')
const cors = require('cors');
const { db } = require('./db/db');
const { readdirSync } = require('fs')
const app = express()
require('dotenv').config()
const PORT = process.env.PORT
//middlewares
app.use(express.json())
app.use(cors())
//routes
readdirSync('./routes').map((route) => app.use('/api/v1', require('./routes/' + route)))
const server = () => {
db()
app.listen(PORT, () => {
console.log('listening to port:', PORT)
})
}
server()
javascript:frontend/src/context/globalContext.js
import React, { useContext, useState } from "react"
import axios from 'axios'
const BASE_URL = "http://localhost:5000/api/v1/";
const GlobalContext = React.createContext()
export const GlobalProvider = ({ children }) => {
const [incomes, setIncomes] = useState([])
const [expenses, setExpenses] = useState([])
const [error, setError] = useState(null)
// Income Functions
const addIncome = async (income) => {
const response = await axios.post(${BASE_URL}add-income, income)
.catch((err) => {
setError(err.response.data.message)
})
getIncomes()
}
const getIncomes = async () => {
const response = await axios.get(${BASE_URL}get-incomes)
setIncomes(response.data)
}
const deleteIncome = async (id) => {
await axios.delete(${BASE_URL}delete-income/${id})
getIncomes()
}
const totalIncome = () => incomes.reduce((total, income) => total + income.amount, 0)
// Expense Functions
const addExpense = async (expense) => {
const response = await axios.post(${BASE_URL}add-expense, expense)
.catch((err) => {
setError(err.response.data.message)
})
getExpenses()
}
const getExpenses = async () => {
const response = await axios.get(${BASE_URL}get-expenses)
setExpenses(response.data)
}
const deleteExpense = async (id) => {
await axios.delete(${BASE_URL}delete-expense/${id})
getExpenses()
}
const totalExpenses = () => expenses.reduce((total, expense) => total + expense.amount, 0)
const totalBalance = () => totalIncome() - totalExpenses()
const transactionHistory = () => {
const history = [...incomes, ...expenses]
history.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
return history.slice(0, 3)
}
return (
<GlobalContext.Provider value={{
addIncome,
getIncomes,
incomes,
deleteIncome,
expenses,
totalIncome,
addExpense,
getExpenses,
deleteExpense,
totalExpenses,
totalBalance,
transactionHistory,
error,
setError
}}>
{children}
</GlobalContext.Provider>
)
}
export const useGlobalContext = () => {
return useContext(GlobalContext)
}

## Future Enhancements

- **User Authentication:** Implement secure user registration and login functionalities.
- **Expense Categorization:** Allow users to create custom categories for better personalization.
- **Export Data:** Enable exporting of financial data in formats like CSV or PDF.
- **Notifications:** Add reminders for upcoming bills or budget limits.
- **Mobile Application:** Develop a mobile version for on-the-go expense tracking.

## Contact

For any inquiries or feedback, feel free to reach out:

- **Email:** your.email@example.com
- **LinkedIn:** [Your LinkedIn Profile](https://www.linkedin.com/in/yourprofile)
- **GitHub:** [yourusername](https://github.com/yourusername)

---

*Empower your financial journey with Expense Tracker — manage your money, achieve your goals.*
