---
permalink: /SalaryHomeMatcher
title: SalaryHomeMatcher
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
    <h1 class="house-h1">Find Your Ideal Home Based on Salary</h1>
    <div class="search-container">
        <input type="text" id="salary-input" class="form-control house-search-bar salary-input" placeholder="Enter your salary">
        <br>
        <button id="find-home-btn" class="btn btn-primary house-btn-primary find-home-btn">Find Home</button>
    </div>
    <div class="row" id="house-details-container"></div>
</div>

<!-- ... Your existing <script> content ... -->

<script type="module">
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';

    // Function to get the JWT token from cookies
    function getJwtToken() {
        return document.cookie.split(';').find(cookie => cookie.trim().startsWith('jwt='));
    }

    // Function to redirect to the login page if the JWT token does not exist
    function redirectToLogin() {
        window.location.href = "{{site.baseurl}}/login"; // Adjust the login page URL as needed
    }

    // Check for the existence of the JWT token when the page loads
    window.addEventListener('load', function() {
        const jwtToken = getJwtToken();

        // If the JWT token does not exist, redirect to the login page
        if (!jwtToken) {
            redirectToLogin();
        }
    });

    document.addEventListener('DOMContentLoaded', () => {
    const houseDetailsContainer = document.getElementById('house-details-container');
    const salaryInput = document.getElementById('salary-input');
    const findHomeButton = document.getElementById('find-home-btn');
    const placeholderImageUrl = 'https://www.avantistones.com/images/noImage.png'; // Define the placeholder image URL


    // Function to create a URL for the house details page
    function getHouseDetailsLink(houseId) {
        return `/houses/house_details?id=${houseId}`;
    }

    findHomeButton.addEventListener('click', async () => {
        const salary = salaryInput.value;
        if (!salary) {
            alert("Please enter a salary.");
            return;
        }
        await fetchHousesData(salary);
    });

    async function fetchHousesData(salary) {
        try {
            const response = await fetch(`http://127.0.0.1:8181/api/house/mlsalarymodel?salary=${salary}`);
            const housesDetails = await response.json();
            renderHouses(housesDetails);
        } catch (error) {
            console.error('Error fetching data:', error);
        }
    }

    function renderHouses(houses) {
        houseDetailsContainer.innerHTML = ''; 
        houses.forEach(house => {
                const houseCard = document.createElement('div');
                houseCard.classList.add('col-lg-4', 'col-md-6', 'mb-4');
                houseCard.innerHTML = `
                    <div class="card">
                        <img src="${house.imgSRC || placeholderImageUrl}" class="card-img-top" alt="${house.address}">
                        <div class="card-body">
                            <h5 class="card-title">${house.address}</h5>
                            <h5 class="card-title">Price: $${house.price}</h5>
                            ${house.livingarea ? `<p class="card-text">${house.livingarea} sqft</p>` : ''}
                            ${house.bedrooms ? `<p class="card-text">Bedrooms: ${house.bedrooms}</p>` : ''}
                            ${house.bathrooms ? `<p class="card-text">Bathrooms: ${house.bathrooms}</p>` : ''}
                            ${house.id ? `<a href="${getHouseDetailsLink(house.id)}" class="btn btn-primary view-details-btn">View Details</a>` : ''}
                        </div>
                    </div>
                `;
                houseDetailsContainer.appendChild(houseCard);
            });

        const detailsButtons = document.querySelectorAll('.view-details-btn');
            detailsButtons.forEach(btn => {
                btn.addEventListener('click', (e) => {
                    const houseId = e.target.getAttribute('data-house-id');
                    let baseUrl;
                    if (location.hostname === "localhost" || location.hostname === "127.0.0.1") {
                        baseUrl = "/houses/";
                    } else {
                        baseUrl = "https://real-estate-analyzation.github.io/RealEstateFrontend/houses/";
                    }
                    console.log(`${baseUrl}house_details?id=${houseId}`)
                    location.href = `${baseUrl}house_details?id=${houseId}`;
            });
        });
    }
});
</script>



</html>
