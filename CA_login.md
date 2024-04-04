---
permalink: /aidanlogin
---


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login Page</title>
</head>
<body>

<div class="container">
    <table>
        <tr>
            <th colspan="2"><h2>Login</h2></th>
        </tr>
        <tr>
            <td><label for="uid">User ID</label></td>
            <td><input type="text" name="uid" id="uid" required></td>
        </tr>
        <tr>
            <td><label for="password">Password</label></td>
            <td><input type="password" name="password" id="password" required></td>
        </tr>
        <tr>
            <td colspan="2"><button onclick="login_User()">Login</button></td>
        </tr>
    </table>
</div>

<script>
    function login_User() {
        // Extract data from inputs
        const uid = document.getElementById("uid").value;
        const password = document.getElementById("password").value;
    
        // Prepare data for POST request
        const userData = {
            uid: uid,
            password: password
        };
    
        // Prepare request options
        const requestOptions = {
            method: 'POST',
            headers: new Headers({'content-type': 'application/json'}),
            body: JSON.stringify(userData), // Convert data object to JSON string
            mode: 'cors',
            credentials: 'include'
        };
    
        // URL for authentication API
        const url = 'http://127.0.0.1:8181/api/aidanuser/authenticate'; // Change to your actual URL
        
        // Async fetch API call to the authentication endpoint
        fetch(url, requestOptions)
            .then(response => {
                // Handle server response
                if (!response.ok) {
                    const errorMsg = 'Database response error: ' + response.status;
                    console.error(errorMsg);
                    return;
                }
                // Response contains valid result
                response.json().then(data => {
                    if (data !== null) {
                        console.log(data);
                        document.cookie = `userid=${data}; expires=Thu, 20 Jan 2025 00:00:00 UTC; path=/;`;
                        window.location.href = '{{site.baseurl}}/jobs';
                    }
                });
            })
            .catch(error => {
                // Handle fetch errors
                console.error('Fetch error:', error);
            });
    }
</script>

</body>
</html>
