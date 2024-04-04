---
permalink: /salarypredictor
title: Salary Predictor
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Properties</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">
    <style>
        .house-body {
            background-color: #f4f7f6;
            font-family: 'Arial', sans-serif;
        }
        .house-container {
            margin-top: 50px;
        }
        .house-h1 {
            color: #333;
            text-align: center;
            margin-bottom: 40px;
        }
        .house-search-bar {
            margin-bottom: 30px;
            border-radius: 30px;
            padding: 20px;
            font-size: 18px;
        }
        .house-card {
            transition: transform 0.3s ease-in-out;
            border-radius: 20px;
            overflow: hidden;
            border: none;
            box-shadow: 0 6px 10px rgba(0, 0, 0, 0.1);
        }
        .house-card:hover {
            transform: scale(1.05);
        }
        .house-card-img-top {
            height: 200px;
            object-fit: cover;
        }
        .house-card-title, .houses-card-text {
            color: #333;
        }
        .house-btn-primary {
            background-color: #3498db;
            border: none;
            border-radius: 20px;
            padding: 10px 20px;
            font-size: 16px;
        }
        .house-btn-primary:hover {
            background-color: #48a2ca;
        }
        @media (max-width: 768px) {
            .house-h1 {
                font-size: 24px;
            }
        }
        .search-container {
            text-align: center; /* Center-align elements within this container */
            margin-bottom: 20px; /* Additional spacing */
        }
        .salary-input {
            width: 50%; /* Adjust width as needed */
            max-width: 300px; /* Prevents the input from being too wide on larger screens */
            display: inline-block; /* Allows width adjustment */
        }
        .find-home-btn {
            margin-top: 10px; /* Aligns button nicely below the input box */
        }
    </style>
</head>
<div class="container house-container">
    <form id="jobForm">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required><br><br>
       <label for="experience_level">Experience Level:</label>
        <select id="experience_level" name="experience_level" required>
            <option value="Entry-level">Entry-level</option>
            <option value="Mid-level">Mid-level</option>
            <option value="Senior">Senior</option>
            <option value="Executive">Executive</option>
        </select><br><br>
        <label for="employment_type">Employment Type:</label>
        <select id="employment_type" name="employment_type" required>
            <option value="Freelance">Freelance</option>
            <option value="Full-time">Full-time</option>
        </select><br><br>
        <label for="work_setting">Work Setting:</label>
        <select id="work_setting" name="work_setting" required>
            <option value="In-person">In-person</option>
            <option value="Remote">Remote</option>
        </select><br><br>

        <button type="button" onclick="predictSalary()">Predict Salary</button>
    </form>
</div>

<!-- ... Your existing <script> content ... -->

<script>
function predictSalary() {
    var form = document.getElementById('jobForm');
    var name = document.getElementById('name');
    var resultDiv = document.getElementById('result');
    var formData = new FormData(form);
    fetch('http://localhost:8181/api/aidanjobsalary/predict', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json'
        },
        body: JSON.stringify(Object.fromEntries(formData))
    })
    .then(response => response.json())
    .then(data => {
        resultDiv.innerHTML = 'successfull';
    })
    .catch(error => {
        console.error('Error:', error);
    });
}
</script>



</html>
