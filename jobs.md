---

permalink: /jobs/
title: Jobs
---


<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Job Details</title>
<style>
/* Style for rounded corners */
.searchbar {
    padding: 10px;
    width: 400px;
    border-radius: 20px; /* Adjust border-radius to round the corners */
    border: 1px solid #ccc;
    font-size: 16px;
    margin-bottom: 10px;
    height: 40px;
}
/* Style for centering */
.center {
    text-align: center;
}
/* Style for rounded corners on the table */
table {
    border-collapse: collapse;
    width: 100%;
    border-radius: 5px; /* Adjust border-radius to round the corners */
    overflow: hidden;
    border: 1px solid #ccc;
}
table th,
table td {
    padding: 8px;
    text-align: left;
    border-bottom: 1px solid #ddd;
}
</style>
</head>
<body>
<div class="center">
    <!-- Add a search bar -->
    <input class="searchbar" type="text" id="searchInput" placeholder="Search for job titles">
    <p></p>
    <!-- Add dropdowns for filtering -->
    <label for="fieldSelect">Filter by Field:</label>
    <select id="fieldSelect">
        <option value="">All Fields</option>
        <option value="Finance">Finance</option>
        <option value="IT">IT</option>
        <option value="Software">Software</option>
        <option value="Web">Web</option>
        <option value="Engineering">Engineering</option>
    </select>
    <label for="locationSelect">Filter by Location:</label>
    <select id="locationSelect">
        <option value="">All Locations</option>
        <option value="On-site">On-site</option>
        <option value="Remote">Remote</option>
    </select>
    <label for="qualificationSelect">Filter by Qualification:</label>
    <select id="qualificationSelect">
        <option value="">All Qualifications</option>
        <option value="Bachelors">Bachelors</option>
        <option value="Masters">Masters</option>
    </select>
</div>
<br>
<table>
    <thead>
        <tr>
            <th>Job ID</th>
            <th>Title</th>
            <th>Description</th>
            <th>Field</th>
            <th>Location</th>
            <th>Qualification</th>
            <th>Hourly Pay</th>
            <th>View</th>
        </tr>
    </thead>
    <tbody id="results">
    </tbody>
</table>
<script>
   
   const resultContainer = document.getElementById("results");
    const fieldSelect = document.getElementById("fieldSelect");
    const locationSelect = document.getElementById("locationSelect");
    const qualificationSelect = document.getElementById("qualificationSelect");

    // Function to filter jobs based on search query, field, location, and qualification
    function filterJobs() {
        const searchQuery = document.getElementById("searchInput").value.toLowerCase();
        const selectedField = fieldSelect.value.toLowerCase();
        const selectedLocation = locationSelect.value.toLowerCase();
        const selectedQualification = qualificationSelect.value.toLowerCase();
        
        const rows = resultContainer.querySelectorAll("tr");
        rows.forEach(row => {
            const title = row.querySelector("td:nth-child(2)").textContent.toLowerCase();
            const field = row.querySelector("td:nth-child(4)").textContent.toLowerCase();
            const location = row.querySelector("td:nth-child(5)").textContent.toLowerCase();
            const qualification = row.querySelector("td:nth-child(6)").textContent.toLowerCase();
            if ((title.includes(searchQuery) || searchQuery === "") &&
                (selectedField === "" || field === selectedField) &&
                (selectedLocation === "" || location === selectedLocation) &&
                (selectedQualification === "" || qualification === selectedQualification)) {
                row.style.display = "";
            } else {
                row.style.display = "none";
            }
        });
    }

    // Add event listeners to dropdowns and search input
    fieldSelect.addEventListener("change", filterJobs);
    locationSelect.addEventListener("change", filterJobs);
    qualificationSelect.addEventListener("change", filterJobs);
    document.getElementById("searchInput").addEventListener("input", filterJobs);

    // Fetch data and populate the table as before
    const displayJobsUrl = "http://127.0.0.1:8181/api/job/";
   // const displayJobsUrl = "https://jobly.stu.nighthawkcodingsociety.com/api/job/";
    const displayJobsHeaders = {
        method: 'GET',
        mode: 'cors',
        cache: 'default',
        credentials: 'include',
        headers: {
            'Content-Type': 'application/json',
            // Add the Access-Control-Allow-Origin header here
        },
    };

    fetch(displayJobsUrl, displayJobsHeaders)
        .then(response => {
            if (response.status !== 200) {
                const tr = document.createElement("tr");
                const td = document.createElement("td");
                td.innerHTML = errorMsg;
                tr.appendChild(td);
                resultContainer.appendChild(tr);
                return;
            }
            response.json().then(data => {
                data.forEach(row => {
                    const tr = document.createElement("tr");
                    tr.innerHTML = `
                        <td>${row.id}</td>
                        <td>${row.title}</td>
                        <td>${row.description}</td>
                        <td>${row.field}</td>
                        <td>${row.location}</td>
                        <td>${row.qualification}</td>
                        <td>${row.pay}</td>
                        <td><a href="{{site.baseurl}}/jobdetails?id=${row.id}">View Job</a></td>
                    `;
                    resultContainer.appendChild(tr);
                });
                // After populating the table, apply initial filtering
                filterJobs();
            });
        })
        .catch(err => {
            console.error(err);
            const tr = document.createElement("tr");
            const td = document.createElement("td");
            td.innerHTML = err;
            tr.appendChild(td);
            resultContainer.appendChild(tr);
        });
</script>
</body>
</html>
