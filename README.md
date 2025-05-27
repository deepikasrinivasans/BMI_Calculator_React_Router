# Ex06 BMI Calculator
## Date: 27-05-2025
## Name : Deepika S
## Register Number: 212222230028

## AIM
To develop a responsive and interactive Body Mass Index (BMI) Calculator using React that allows users to input their height and weight, and calculates their BMI to categorize their health status (e.g., Underweight, Normal, Overweight, Obese).

## DESIGN STEPS

### STEP 1: Initialize React Project

<li>Create a new React app using create-react-app.</li>
<li>Install React Router using:</li>
npm install react-router-dom

### STEP 2: Set Up Routing

Create routing structure with react-router-dom:

<li>Home route (/) – Intro or Navigation</li>

<li>BMI Calculator route (/bmi)</li>

<li>Result route (/result)</li>

### STEP 3: Design the BMI Form Page

<li>Create a form to accept Height (in cm or m) and Weight (in kg).</li>

<li>On form submit, navigate to the result page with entered values via URL query params or context/state.</li>

## STEP 4: Handle Input Validation

<li>Check if height and weight are valid numbers.</li>

<li>Optionally, show error messages for invalid inputs.</li>

### STEP 5: Perform BMI Calculation

<li>In the result component:

<li>Extract height and weight from the route (URL or passed state).</li>

<li>Apply the BMI formula:</li>

![image](https://github.com/user-attachments/assets/ec785506-c96b-489e-8783-fb1a5d36101a)
​
 
<li>Convert height from cm to m if needed.</li></li>

### STEP 6: Display Result

<li>Show calculated BMI.</li>

<li>Show category based on BMI range:

<li>Underweight, Normal, Overweight, Obese, etc.</li></li>

### STEP 7: Navigation Options

<li>Provide a button to go back to the BMI form to calculate again.</li>

### STEP 8: Enhancements

<li>Add styling using CSS or Tailwind.</li>

## PROGRAM
### APP.js
```
import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Home from "./pages/Home";
import BMIForm from "./pages/BMIForm";
import Result from "./pages/Result";
import "./App.css"; // Make sure this line is here for footer styling

function App() {
  return (
    <Router>
      <div className="app-container">
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/bmi" element={<BMIForm />} />
          <Route path="/result" element={<Result />} />
        </Routes>
        <footer className="footer">
          <p>Created by Deepika S | Register Number: 212222230028</p>
        </footer>
      </div>
    </Router>
  );
}

export default App;

```
### Home.js
```
import React from 'react';
import { Link } from 'react-router-dom';

function Home() {
  return (
    <div style={{ textAlign: 'center', marginTop: '100px' }}>
      <h1>CHECK YOUR BMI</h1>
      <Link to="/bmi">
        <button>Go to BMI Calculator</button>
      </Link>
    </div>
  );
}

export default Home;

```
### BMIForm.js
```
// src/pages/BMIForm.js
import React, { useState } from 'react';
import { useNavigate } from 'react-router-dom';
import '../App.css';

function BMIForm() {
  const [height, setHeight] = useState('');
  const [weight, setWeight] = useState('');
  const navigate = useNavigate();

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!height || !weight || isNaN(height) || isNaN(weight)) {
      alert('Please enter valid height and weight.');
      return;
    }
    navigate('/result', { state: { height, weight } });
  };

  return (
    <div className="container">
      <div className="sensor"></div>
      <h1>BMI Calculator</h1>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Enter height (cm)"
          value={height}
          onChange={(e) => setHeight(e.target.value)}
        />
        <input
          type="text"
          placeholder="Enter weight (kg)"
          value={weight}
          onChange={(e) => setWeight(e.target.value)}
        />
        <button type="submit">Calculate BMI</button>
      </form>
    </div>
  );
}

export default BMIForm;

```
### Result.js
```
// src/pages/Result.js
import React from 'react';
import { useLocation, useNavigate } from 'react-router-dom';
import '../App.css';

function Result() {
  const location = useLocation();
  const navigate = useNavigate();
  const { height, weight } = location.state || {};

  if (!height || !weight) {
    return <p>Invalid input. Please go back and enter values.</p>;
  }

  const h = parseFloat(height) / 100;
  const w = parseFloat(weight);
  const bmi = (w / (h * h)).toFixed(2);

  let category = '';
  if (bmi < 18.5) category = 'Underweight';
  else if (bmi < 24.9) category = 'Normal';
  else if (bmi < 29.9) category = 'Overweight';
  else category = 'Obese';

  return (
    <div className="container">
      <div className="sensor"></div>
      <h1>Your BMI Result</h1>
      <div className="screen">
        <h2>BMI: {bmi}</h2>
        <p>Status: {category}</p>
      </div>
      <button className="back-button" onClick={() => navigate('/bmi')}>
        Calculate Again
      </button>
    </div>
  );
}

export default Result;

```

### APP.css
```
body {
  font-family: 'Segoe UI', sans-serif;
  background: linear-gradient(135deg, #c8e6c9, #f0f4c3);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  animation: bgMove 15s infinite alternate;
}

@keyframes bgMove {
  0% {
    background-position: left top;
  }
  100% {
    background-position: right bottom;
  }
}

.container {
  background: #f1f8e9;
  border: 8px solid #4caf50;
  border-radius: 20px;
  box-shadow: 0 0 30px rgba(76, 175, 80, 0.6);
  width: 90%;
  max-width: 420px;
  padding: 40px 30px;
  text-align: center;
  position: relative;
  animation: floatMachine 2s ease-in-out forwards;
}

@keyframes floatMachine {
  0% {
    transform: translateY(100px);
    opacity: 0;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}

h1 {
  color: #2e7d32;
  font-size: 28px;
  margin-bottom: 25px;
  animation: glowText 1.2s infinite alternate;
}

@keyframes glowText {
  from {
    text-shadow: 0 0 5px #81c784;
  }
  to {
    text-shadow: 0 0 15px #66bb6a;
  }
}

input {
  width: 100%;
  padding: 12px 14px;
  margin: 12px 0;
  font-size: 16px;
  border: 2px solid #a5d6a7;
  border-radius: 10px;
  transition: 0.3s;
  background-color: #e8f5e9;
}

input:focus {
  outline: none;
  border-color: #66bb6a;
  box-shadow: 0 0 8px #a5d6a7;
}

button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 12px 25px;
  margin-top: 18px;
  font-size: 16px;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 5px #357a38;
}

button:hover {
  background-color: #388e3c;
  transform: scale(1.05);
}

.screen {
  background: #212121;
  color: #76ff03;
  font-family: 'Courier New', Courier, monospace;
  font-size: 22px;
  padding: 20px;
  margin-top: 30px;
  border-radius: 12px;
  box-shadow: 0 0 10px #76ff03, inset 0 0 15px #76ff03;
  animation: flicker 2s infinite;
}

@keyframes flicker {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.9;
  }
}
.sensor {
  position: absolute;
  top: -20px;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 20px;
  background: #66bb6a;
  border-radius: 25px;
  box-shadow: 0 0 10px #00e676, 0 0 20px #69f0ae;
  animation: pulse 1.2s infinite;
}

@keyframes pulse {
  0%, 100% {
    transform: translateX(-50%) scale(1);
  }
  50% {
    transform: translateX(-50%) scale(1.1);
  }
}

.back-button {
  background-color: #81c784;
  margin-top: 20px;
}

```
## OUTPUT
![bmi1](https://github.com/user-attachments/assets/244ec36c-b976-4ebb-8e2a-767acb0ddb8d)
![bmi2](https://github.com/user-attachments/assets/ce34ef25-439c-417e-b14a-c0ad21b36a3e)
![bmi3](https://github.com/user-attachments/assets/8c935c0d-1bad-4ad7-bf70-beea6f9491bc)
![bmi4](https://github.com/user-attachments/assets/f8d99c1a-01cb-44f9-a666-6ff090b4983a)


## RESULT
The BMI Calculator successfully takes user input for height and weight, performs the BMI calculation in real-time using React state and event handling, and displays the BMI value along with the corresponding health category.
