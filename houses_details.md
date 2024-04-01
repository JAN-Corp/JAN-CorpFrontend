---
permalink: /houses/house_details
title: House Details
---

<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>House Info</title>
    <style>
        /* Your existing styles */ a
        body {
            font-family: Arial, sans-serif;
            background-color: #f8f8f8;
            margin: 0;
            padding: 0;
        }
        .house-info {
            background-color: #fff;
            margin: 20px auto; /* Center aligning the container */
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            text-align: center; /* Center-aligning text content */
        }
        h1 {
            color: #333;
            font-size: 24px;
            margin-bottom: 10px;
        }
        p {
            color: #666;
            margin-bottom: 5px;
        }
        img {
            width: 60%; /* Decreased image width to 60% of its parent container */
            height: auto;
            margin-bottom: 15px;
            border-radius: 8px;
        }
        a {
            color: #3498db;
            text-decoration: none;
        }
        a:hover {
            text-decoration: underline;
        }
        /* Additional Styles */
        .house-details {
            margin-top: 20px;
        }
        .house-details p {
            font-size: 16px;
            margin-bottom: 8px;
        }
        .house-details strong {
            color: #333;
        }
    </style>
</head>

<body>
    <div class="house-info" id="house-info">
        <!-- House information will be inserted here dynamically using JavaScript -->
    </div>
    <script type="module">
        import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
        // Function to get the JWT token from cookies
        function getJwtToken() {
            const cookies = document.cookie.split(';');
            for (let cookie of cookies) {
                const [name, value] = cookie.split('=');
                if (name.trim() === 'jwt') {
                    return value;
                }
            }
            return null;
        }
        // Function to redirect to the login page if the JWT token does not exist
        function redirectToLogin() {
            window.location.href = "{{site.baseurl}}/login"; // Adjust the login page URL as needed
        }
        // Function to add the house to favorites
        async function addToFavorites(houseId) {
            try {
                const jwtToken = getJwtToken();
                if (!jwtToken) {
                    redirectToLogin();
                    return;
                }
                const decodedToken = JSON.parse(atob(jwtToken.split('.')[1])); // Decode token and parse JSON
                const userId = decodedToken.id;
                const url = uri + '/api/house/addtofavorites?id=' + userId + '&house_id=' + houseId;
                console.log(url);
                const response = await fetch(url, {
                    method: 'POST',
                    headers: {
                        'Authorization': `Bearer ${jwtToken}`
                    }
                });
                if (response.ok) {
                    window.location.href = "/favorites";
                } else {
                    alert('Failed to add house to favorites. Please try again later.');
                }
            } catch (error) {
                console.error('Error adding house to favorites:', error);
                alert('Failed to add house to favorites. Please try again later.');
            }
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
            const urlParams = new URLSearchParams(window.location.search);
            const houseId = urlParams.get('id');
            const houseInfoContainer = document.getElementById('house-info');
            async function fetchHouseInfo() {
                try {
                    const url = uri + '/api/house/housedetails?id=' + houseId;
                    const response = await fetch(url);
                    const house = await response.json();
                    houseInfoContainer.innerHTML = `
                        <img src="${house.imgSRC || 'https://www.avantistones.com/images/noImage.png'}" alt="${house.address}" />
                        <h1>${house.address}</h1>
                        <div class="house-details">
                            ${house.city ? `<p><strong>City:</strong> ${house.city}</p>` : ''}
                            ${house.state ? `<p><strong>State:</strong> ${house.state}</p>` : ''}
                            ${house.zip ? `<p><strong>Zip Code:</strong> ${house.zip}</p>` : ''}
                        </div>
                        <div class="house-details">
                            ${house.price ? `<p><strong>Price:</strong> $${house.price}</p>` : ''}
                            ${house.bedrooms ? `<p><strong>Bedrooms:</strong> ${house.bedrooms}</p>` : ''}
                            ${house.bathrooms ? `<p><strong>Bathrooms:</strong> ${house.bathrooms}</p>` : ''}
                            ${house.livingarea ? `<p><strong>Living Area:</strong> ${house.livingarea} sqft</p>` : ''}
                            ${house.homeType ? `<p><strong>Home Type:</strong> ${house.homeType}</p>` : ''}
                            ${house.priceEstimate ? `<p><strong>Price Estimate:</strong> $${house.priceEstimate}</p>` : ''}
                            ${house.rentEstimate ? `<p><strong>Rent Estimate:</strong> $${house.rentEstimate}</p>` : ''}
                            <button class="btn btn-primary btn-lg font-weight-bold" id="addToFavorites">Add to Favorites</button>
                        </div>
                    `;
                    // Add event listener to the "Add to Favorites" button
                    const addToFavoritesButton = document.getElementById('addToFavorites');
                    addToFavoritesButton.addEventListener('click', () => {
                        addToFavorites(houseId); // Call the addToFavorites function with the house ID
                    });
                } catch (error) {
                    console.error('Error fetching data:', error);
                    houseInfoContainer.innerHTML = '<p>Error loading house information.</p>';
                }
            }
            fetchHouseInfo();
        });
</script>

</body>

</html>
