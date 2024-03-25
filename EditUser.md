---
title: Edit User
permalink: /editUser
---
<style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 0;
        }
        .container {
            width: 500px;
            margin: 100px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .container h2 {
            text-align: center;
            margin-bottom: 20px;
        }
        .container p {
            margin-bottom: 10px;
        }
        .container label {
            display: block;
            font-weight: bold;
        }
        .userInput {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 3px;
            box-sizing: border-box;
        }
        .container button {
            width: 100%;
            padding: 10px;
            background-color: #007bff;
            color: #fff;
            border: none;
            border-radius: 3px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        .container button:hover {
            background-color: #0056b3;
        }
        h2 {
            text-align: center;
            color: black;
        }
        p {
            color: black;
        }
    </style>
<body>
    <div class="container">
        <h2>Edit User Information</h2>
        <form id="username" action="javascript:edit_user()">
            <p><label for="name">User Name:</label></p>
            <p><input class="userInput" type="text" name="name" id="name" required></p>
            <p><label for="dob">Date of Birth:</label></p>
            <p><input class="userInput" type="text" id="dob" required></p>
            <p><button onclick="edit_user()">Submit</button></p>
        </form>
    </div>
</body>



<!-- 
Below JavaScript code is designed to handle user authentication in a web application. It's written to work with a backend server that uses JWT (JSON Web Tokens) for authentication.

The script defines a function when the page loads. This function is triggered when the Login button in the HTML form above is pressed. 
 -->
<script type="module">

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

    // Your existing JavaScript code for fetching and displaying house details
    document.addEventListener('DOMContentLoaded', () => {
        // Your existing JavaScript code for fetching and displaying house details
    });

    // uri variable and options object are obtained from config.js
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    
    function edit_user(){
        // Set Authenticate endpoint
        const url = uri + '/api/users/';

        // Set the body of the request to include login data from the DOM
        const body = {
            name: document.getElementById("name").value,
            dob: document.getElementById("dob").value,
        };

        // Change options according to Authentication requirements
        const authOptions = {
            ...options, // This will copy all properties from options
            method: 'PUT', // Override the method property
            cache: 'no-cache', // Set the cache property
            headers: {
                'Content-Type': 'application/json', // Example header
                'uid': localStorage.getItem('uid') // Set the uid as a header
            },
            body: JSON.stringify(body),
        };

        console.log(url);
        console.log(authOptions);
        console.log(body);
        // Fetch JWT
        fetch(url, authOptions)
        .then(response => {
            // handle error response from Web API
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);
                return;
            }
            window.location.href = "{{site.baseurl}}/database";
        })
        // catch fetch errors (ie ACCESS to server blocked)
        .catch(err => {
            console.error(err);
        });
    }

    // Attach login_user to the window object, allowing access to form action
    window.edit_user = edit_user;
</script>