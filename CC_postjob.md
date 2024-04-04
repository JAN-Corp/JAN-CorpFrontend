---
permalink: /postjob/
title: Post a Job
---

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
/* CSS styles */
.centertext {
    text-align: center;
}

.form-table {
    margin: 0 auto; /* Center the table */
}

.form-table th, .form-table td {
    padding: 10px;
    text-align: left;
}

.textarea {
    width: 100%; /* Set the width to 100% of the parent container */
    resize: vertical; /* Allow vertical resizing */
}

/* Additional styling if needed */
.post-job-button {
    display: block;
    margin: 0 auto;
    padding: 10px 20px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
}

.post-job-button:hover {
    background-color: #0056b3;
}
</style>
</head>
<body>
<h2 class="centertext">Add Job</h2>
<h3 class="centertext">Hey Employer! Start receiving job applicants by completing this form!</h3>
<form>
    <table class="form-table">
        <tr>
            <td><label for="title">Job Title:</label></td>
            <td><input type="text" id="title" name="title" required></td>
        </tr>
        <tr>
            <td><label for="company">Company:</label></td>
            <td><input type="text" id="company" name="company" required></td>
        </tr>
        <tr>
            <td><label for="description">Job Description:</label></td>
            <td><textarea id="description" name="description" rows="4" required></textarea></td>
        </tr>
        <tr>
            <td><label for="qualification">Job Qualifications:</label></td>
            <td><textarea id="qualification" name="qualification" rows="4" required></textarea></td>
        </tr>
        <tr>
            <td><label for="pay">Hourly Pay:</label></td>
            <td><input type="number" id="pay" name="pay" required></td>
        </tr>
        <tr>
            <td><label for="field">Field:</label></td>
            <td>
                <select id="field" name="field" required>
                    <option value="">Select a Field</option>
                    <option value="IT">IT</option>
                    <option value="Engineering">Engineering</option>
                    <option value="Finance">Finance</option>
                    <option value="Marketing">Marketing</option>
                    <option value="Web">Web</option>
                    <!-- Add more options as needed -->
                </select>
            </td>
        </tr>
        <tr>
            <td><label for="location">Location:</label></td>
            <td>
                <select id="location" name="location" required>
                    <option value="">Select a Location</option>
                    <option value="Onsite">Onsite</option>
                    <option value="Remote">Remote</option>
                    <!-- Add more options as needed -->
                </select>
            </td>
        </tr>
        <tr>
            <td colspan="2" class="centertext">
                <button class="post-job-button" type="button" onclick="postJob()">Post Job</button>
            </td>
        </tr>
    </table>
</form>

<script>
// JavaScript code
function postJob() {
    const title = document.getElementById("title").value;
    const company = document.getElementById("company").value;
    const description = document.getElementById("description").value;
    const qualification = document.getElementById("qualification").value;
    const pay = parseInt(document.getElementById("pay").value);
    const field = document.getElementById("field").value;
    const location = document.getElementById("location").value; 
    
    const jobpostee = parseInt(getCookie("userid"));

    const userData = {
        title: title,
        company: company,
        description: description,
        qualification: qualification,
        pay: pay,
        field: field,
        location: location,
        jobpostee: jobpostee
    };

    const requestOptions = {
        method: 'POST', // *GET, POST, PUT, DELETE, etc.
        mode: 'cors', // no-cors, *cors, same-origin
        cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
        body: JSON.stringify(userData),
        credentials: 'include', // include, *same-origin, omit
        headers: {
            'Content-Type': 'application/json'
        },
    };

    const url = 'http://127.0.0.1:8181/api/job/';
    
    fetch(url, requestOptions)
    .then(response => {
        // Handle server response
        if (response.status !== 200) {
            const errorMsg = 'Database response error: ' + response.status;
            console.log(errorMsg);
            window.location.href = '{{site.baseurl}}/jobauth';
        }

        // Response contains valid result
        response.json().then(data => {
            if (data !== null) {
                console.log(data)
                window.location.href = '{{site.baseurl}}/jobs';
            }
        });
    })
    .catch(error => {
        // Handle fetch errors
        console.error('Fetch error:', error);
    });
}

function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i < ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}
</script>

</body>
</html>
