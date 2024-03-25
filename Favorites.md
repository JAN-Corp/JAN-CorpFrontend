---
title: Favorites
permalink: /favorites
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Favorites</title>
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
    <h1 class="house-h1">Favorites</h1>
    <input type="text" id="search-bar" class="form-control house-search-bar" placeholder="Search by address">
    <!-- Add the following div with the id "house-cards" -->
    <div class="row" id="house-cards"></div>
</div>


<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL" crossorigin="anonymous"></script>

<script type="module">
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    // Your JavaScript code
    // Function to get the JWT token from cookies
    function getJwtToken() {
        return document.cookie.split(';').find(cookie => cookie.trim().startsWith('jwt='));
    }

    // Define getHouseDetailsLink function
    function getHouseDetailsLink(houseId) {
        return `/houses/house_details?id=${houseId}`;
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

    // Function to fetch favorites for the current user
    async function fetchFavorites() {
        try {
            const jwtToken = getJwtToken();
            const userId = JSON.parse(atob(jwtToken.split('.')[1])).id;
            const url = uri + '/api/house/getfavorites?id=' + userId;
            const response = await fetch(url);
            const favorites = await response.json();
            renderFavorites(favorites);
        } catch (error) {
            console.error('Error fetching favorites:', error);
        }
    }

    // Function to render favorites
    function renderFavorites(favorites) {
    const favoritesContainer = document.getElementById('house-cards');
    favoritesContainer.innerHTML = ''; // Clear previous content

    favorites.forEach(favorite => {
        const favoriteItem = document.createElement('div');
        favoriteItem.classList.add('col-lg-4', 'col-md-6', 'mb-4');
        favoriteItem.innerHTML = `
                    <div class="card">
                        <img src="${favorite.imgSRC || placeholderImageUrl}" class="card-img-top" alt="${favorite.address}">
                        <div class="card-body">
                            <h5 class="card-title">${favorite.address}</h5>
                            <h5 class="card-title">Price: $${favorite.price}</h5>
                            ${favorite.livingarea ? `<p class="card-text">${favorite.livingarea} sqft</p>` : ''}
                            ${favorite.bedrooms ? `<p class="card-text">Bedrooms: ${favorite.bedrooms}</p>` : ''}
                            ${favorite.bathrooms ? `<p class="card-text">Bathrooms: ${favorite.bathrooms}</p>` : ''}
                            ${favorite.id ? `<a href="${getHouseDetailsLink(favorite.id)}" class="btn btn-primary view-details-btn">View Details</a>` : ''}
                        </div>
                    </div>
                `;
        favoritesContainer.appendChild(favoriteItem);
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

    // Call the fetchFavorites function when the DOM content is loaded
    document.addEventListener('DOMContentLoaded', fetchFavorites);
</script>
</body>
</html>
