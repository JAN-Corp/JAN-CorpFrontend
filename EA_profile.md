---

permalink: /profile/
title: Profile
---

<style>
.card {
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 20px;
    margin-bottom: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.card-title {
    font-size: 1.25rem;
    margin-bottom: 10px;
  
    align-items: center;
}

.back-arrow {
    cursor: pointer;
    margin-right: 10px;
}

.back-arrow svg {
    fill: #007bff;
    width: 20px;
    height: 20px;
    transition: transform 0.3s ease;
}

.back-arrow svg:hover {
    transform: translateX(-3px);
}

.card-text {
    margin-bottom: 5px;
}

.btn {
    display: inline-block;
    font-weight: 400;
    color: #212529;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    border: 1px solid transparent;
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    line-height: 1.5;
    border-radius: 0.25rem;
    background-color: #007bff;
    border-color: #007bff;
    color: #fff;
    text-decoration: none;
}

.btn-primary {
    background-color: #007bff;
    border-color: #007bff;
}

.btn-primary:hover {
    background-color: #0056b3;
    border-color: #0056b3;
}

.btn-primary:focus {
    box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.5);
    outline: none;
}
.dynamiccount {
    float: right;
    text-align: right;
}

</style>
</head>
<body>
    <h1 id="userName"></h1>
    <h2 id="roledescription"></h2>
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
                <th id="editColumn"></th>
            </tr>
        </thead>
        <tbody id="results">
        </tbody>
    </table>

<script>
    const editColumn = document.getElementById('editColumn');
    const resultContainer = document.getElementById("results");
    function getCookie(cname) {
        var name = cname + "=";
        var decodedCookie = decodeURIComponent(document.cookie);
        var ca = decodedCookie.split(';');
        for(var i = 0; i <ca.length; i++) {
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
      userid = getCookie("userid");

    const getUserStatusHeader = {
        method: 'GET',
        headers: new Headers({'content-type': 'application/json'}),
        mode: 'cors',
    };

    // URL for Create API
    // const url = 'http://127.0.0.1:8181/api/jwt_auth/register';
   const getUserStatusUrl = 'http://127.0.0.1:8181/api/jobuser/userstatus?userid=' + userid;
    // Async fetch API call to the database to create a new user
    fetch(getUserStatusUrl, getUserStatusHeader)
    .then(response => {
      // Error handling
      if (response.status !== 200) {
        const errorMsg = 'Database response error: ' + response.status;
        console.log(errorMsg);

      }
      response.json().then(data => {
        console.log(data)
        if (data.status == "Employer") {
            employerRoute()
            document.getElementById('userName').innerHTML = `Hello ${data.name}`
            document.getElementById('roledescription').innerHTML = `Since you're an ${data.status}, you are able to view the jobs you posted here. Edit the details of your job by clicking on the "Edit Job" button.`
        } else if (data.status == "Freelancer") {
            freelancerRoute()
            document.getElementById('userName').innerHTML = `Hello ${data.name}`
            document.getElementById('roledescription').innerHTML = `Since you're a ${data.status}, you are able to view the jobs you applied to here.`
        }
      });
    })
    .catch(err => {
      // Error handling
      console.error(err);
    });
    
    

    function employerRoute()  {
        const getJobsPostedHeader = {
            method: 'GET',
            headers: new Headers({'content-type': 'application/json'}),
            mode: 'cors',
        };
    
        // URL for Create API
        // const url = 'http://127.0.0.1:8181/api/jwt_auth/register';
       const getJobsPostedUrl = 'http://127.0.0.1:8181/api/jobuser/profile?userid=' + userid;
        // Async fetch API call to the database to create a new user
        fetch(getJobsPostedUrl, getJobsPostedHeader)
        .then(response => {
          // Error handling
          if (response.status !== 200) {
            const errorMsg = 'Database response error: ' + response.status;
            console.log(errorMsg);
    
          }
          response.json().then(data => {
            editColumn.innerHTML = 'Edit Job Details';
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
          
                    <td><a href="{{site.baseurl}}/applicants?jobid=${row.id}">View Applicants</a></td>
                    <td><a href={{site.baseurl}}/editjob?id=${row.id}">Edit Job</a></td>
                `;
                resultContainer.appendChild(tr);
            });
          });
        })
        .catch(err => {
          // Error handling
          console.error(err);
        });
        
    }

    function freelancerRoute()  {
        const getJobsAppliedToHeader = {
            method: 'GET',
            headers: new Headers({'content-type': 'application/json'}),
            mode: 'cors',
        };
    
        // URL for Create API
        // const url = 'http://127.0.0.1:8181/api/jwt_auth/register';
       const getJobsAppliedToUrl = 'http://127.0.0.1:8181/api/jobuser/profile?userid=' + userid;
        // Async fetch API call to the database to create a new user
        fetch(getJobsAppliedToUrl, getJobsAppliedToHeader)
        .then(response => {
          // Error handling
          if (response.status !== 200) {
            const errorMsg = 'Database response error: ' + response.status;
            console.log(errorMsg);
    
          }
          response.json().then(data => {
            editColumn.innerHTML = 'Edit Application Details';
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
                    <td><a href="{{site.baseurl}}/editapplication?id=${row.id}">Edit Application</a></td>
                `;
                resultContainer.appendChild(tr);
            });
          });
        })
        .catch(err => {
          // Error handling
          console.error(err);
        });
        
    }
    

</script>


</body>





     

