---
permalink: /houses
title: houses
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
    </style>
</head>
<body class="house-body">

<div class="container house-container">
    <h1 class="house-h1">Explore Properties</h1>
    <input type="text" id="search-bar" class="form-control house-search-bar" placeholder="Search by address">
    <div class="row" id="house-cards"></div>
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL" crossorigin="anonymous"></script>

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

    // Your existing JavaScript code for fetching and rendering houses data
    document.addEventListener('DOMContentLoaded', () => {
        // Your existing JavaScript code for fetching and rendering houses data
    });

    document.addEventListener('DOMContentLoaded', () => {
        const houseCardsContainer = document.getElementById('house-cards');
        const searchBar = document.getElementById('search-bar');
        const placeholderImageUrl = 'https://www.avantistones.com/images/noImage.png';
        let housesData = [];

        // Define getHouseDetailsLink function
        function getHouseDetailsLink(houseId) {
            return `/houses/house_details?id=${houseId}`;
        }

        async function fetchData() {
            try {
                const url = uri + '/api/house/houses';
                const response = await fetch(url);
                const data = await response.json();
                housesData = data;
                renderHouses(housesData);
            } catch (error) {
                console.error('Error fetching data:', error);
            }
        }

        function renderHouses(houses) {
            houseCardsContainer.innerHTML = '';
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
                houseCardsContainer.appendChild(houseCard);
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

        function filterHouses() {
            const searchTerm = searchBar.value.toLowerCase();
            const filteredHouses = housesData.filter(house => {
                const houseAddress = house.address.toLowerCase();
                return houseAddress.includes(searchTerm);
            });
            renderHouses(filteredHouses);
        }

        fetchData();
        searchBar.addEventListener('input', filterHouses);
    });
</script>

</body>
</html>
