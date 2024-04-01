---
layout: default
title: Real Esate Analyzations
permalinl: /
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <title>Real Esate Analyzation</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        .hero-section {
            height: 60vh;
            background-image: url('https://cdn.hometogo.net/assets/media/pics/1920_600/6118fa9084ec1.jpg');
            background-size: cover;
            background-position: center;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            position: relative; /* Add this line to make the background overlay work */
        }
        /* Background overlay */
        .hero-section::before {
            content: "";
            background: rgba(0, 0, 0, 0.5); /* Adjust the transparency by changing the last value (0.5) */
            position: absolute;
            top: 0;
            right: 0;
            bottom: 0;
            left: 0;
            z-index: -1; /* Place it behind the text content */
        }
        .hero-section h1 {
            font-size: 80px;
            margin-bottom: 20px;
            background: rgba(0, 0, 0, 0.7);
        }
        .hero-section p {
            font-size: 40px;
            margin-bottom: 20px;
            background: rgba(0, 0, 0, 0.77);
        }
        .top-houses, .recommendations {
            padding: 50px 0;
        }
        .house-card {
            margin-bottom: 30px;
        }
    </style>
</head>
<body>

<!-- Hero Section -->
<section class="hero-section">
    <div>
        <h1>Welcome to SoCal Real Esate</h1>
        <p>Find the best properties in Southern California</p>
    </div>
</section>

<!-- Top House Section -->

<section class="top-houses container">
    <h2 class="text-center">Our Best Properties</h2>
    <div class="row" id="house-list"></div> <!-- This is where we'll display the house information -->
    <div class="text-center mt-4">
        <a href="#" class="btn btn-primary btn-lg font-weight-bold" id="viewMoreHouses">View More Properties</a>
    </div>
</section>

<script>

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

    // Function to get the correct link for "View More Houses" based on the hostname
    function getHousesLink() {
        if (location.hostname === "localhost" || location.hostname === "127.0.0.1") {
            return "/houses";
        } else {
            return "https://real-estate-analyzation.github.io/RealEstateFrontend/houses";
        }
    }

    // Update the "View More Houses" link dynamically
    const viewMoreHousesLink = document.getElementById('viewMoreHouses');
    viewMoreHousesLink.href = getHousesLink();
</script>

<!-- Personalized Recommendations Section 
<section class="recommendations container">
    <h2 class="text-center">Get Personalized Recommendations</h2>
    <p class="text-center">Answer a few questions and get houses recommendations tailored just for you</p>
    <div class="text-center mt-4">
        <a href="#" class="btn btn-primary btn-lg font-weight-bold" id="getStartedButton">Get Started</a>
    </div>
</section>

<script>
    // Function to get the correct link for "Get Started" based on the hostname
    function getGetStartedLink() {
        if (location.hostname === "localhost" || location.hostname === "127.0.0.1") {
            return "/houserecommendation";
        } else {
            return "https://real-estate-analyzation.github.io/RealEstateFrontend/houserecommendation";
        }
    }

    // Update the "Get Started" link dynamically
    const getStartedButton = document.getElementById('getStartedButton');
    getStartedButton.href = getGetStartedLink();
</script>-->


<script type="module">
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    // Function to fetch and display houses information
    async function fetchHouses() {
        try {
            const url = uri + '/api/house/houses';
            const response = await fetch(url); // Replace with your API URL
            const data = await response.json();

            // Select the div where you want to display the houses
            const houseList = document.getElementById('house-list');

            // Loop through the first 3 houses (assuming API returns an array of houses)
            data.slice(0, 3).forEach(house => {
                // Create a div to hold house card
                const houseCard = document.createElement('div');
                houseCard.classList.add('col-lg-4', 'col-md-6', 'mb-4'); // Adjust the column size here
                houseCard.innerHTML = `
                    <div class="card">
                        <img src="${house.imgSRC || placeholderImageUrl}" class="card-img-top" alt="${house.address}">
                        <div class="card-body">
                            <h5 class="card-title">${house.address}</h5>
                            ${house.price ? `<h5 class="card-title">Price: ${house.price}</h5>` : ''}
                            ${house.livingarea ? `<p class="card-text">${house.livingarea} sqft</p>` : ''}
                            ${house.bedrooms ? `<p class="card-text">Bedrooms: ${house.bedrooms}</p>` : ''}
                            ${house.bathrooms ? `<p class="card-text">Bathrooms: ${house.bathrooms}</p>` : ''}
                            <a href="${getHouseDetailsLink(house.id)}" class="btn btn-primary view-details-btn">View Details</a>
                        </div>
                    </div>
                `;
                houseList.appendChild(houseCard);
            });

            function getHouseDetailsLink(houseId) {
                let baseUrl;

                if (location.hostname === "localhost" || location.hostname === "127.0.0.1") {
                    baseUrl = "/houses/";
                } else {
                    baseUrl = "https://real-estate-analyzation.github.io/RealEstateFrontend/houses/";
                }

                return `${baseUrl}house_details?id=${houseId}`;
            }

        } catch (error) {
            console.error('Error fetching data:', error);
        }
    }

    // Call the fetchHouses function when the page loads
    window.addEventListener('load', fetchHouses);
</script>


<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</body>
</html>
