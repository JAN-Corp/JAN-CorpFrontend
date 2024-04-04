---

permalink: /jobdetails/
---
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Job Details</title>
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
   
<div id="results" class="card-container"></div>

<script>
    let urlParams = new URLSearchParams(window.location.search);
    let jobdetails = urlParams.get('id');   

    // prepare HTML result container for new output
    const resultContainer = document.getElementById("results");
  
    // prepare fetch options
    //const displayJobDetailsUrl = 'https://jobly.stu.nighthawkcodingsociety.com/api/job/?id=' + jobdetails;
    const displayJobDetailsUrl = "http://127.0.0.1:8181/api/job/?id=" + jobdetails;
    const displayJobDetailsHeaders = {
      method: 'GET', // *GET, POST, PUT, DELETE, etc.
      mode: 'cors', // no-cors, *cors, same-origin
      cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
     
      headers: {
        'Content-Type': 'application/json'
        // 'Content-Type': 'application/x-www-form-urlencoded',
      },
    };
    // fetch the API
    fetch(displayJobDetailsUrl, displayJobDetailsHeaders)
      .then(response => {
        // Error handling
        if (response.status !== 200) {
          const errorMsg = 'Database response error: ' + response.status;
          console.log(errorMsg);
          const errorCard = document.createElement("div");
          errorCard.classList.add("card");
          errorCard.innerHTML = `<div class="card-body">${errorMsg}</div>`;
          resultContainer.appendChild(errorCard);
          return;
        }
        response.json().then(data => {
          // Render job details
          renderJobDetails(data);
          apply_Count()
        });
      })
      .catch(err => {
        // Error handling
        console.error(err);
        const errorCard = document.createElement("div");
        errorCard.classList.add("card");
        errorCard.innerHTML = `<div class="card-body">${err}</div>`;
        resultContainer.appendChild(errorCard);
      });

    function renderJobDetails(data) {
        // Create a card
        const card = document.createElement("div");
        card.classList.add("card");

        // Populate card with job details
        card.innerHTML = `
            <div class="card-body">
                
                <h5 class="card-title">
                    <span class="back-arrow" onclick="goBack()">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M0 0h24v24H0z" fill="none"/><path d="M20 11H7.41l5.3-5.29a1 1 0 1 0-1.42-1.42l-7 7a1 1 0 0 0 0 1.42l7 7a1 1 0 0 0 1.42-1.42L7.41 13H20a1 1 0 0 0 0-2z"/></svg>
                    </span>
                    ${data.title}
                    <div class="dynamiccount">
                    <p id="applycount"></p>
                    </div>
                </h5>
          
                
            </div>
                <p class="card-text"><strong>Description:</strong> ${data.description}</p>
                <p class="card-text"><strong>Field:</strong> ${data.field}</p>
                <p class="card-text"><strong>Location:</strong> ${data.location}</p>
                <p class="card-text"><strong>Qualification:</strong> ${data.qualification}</p>
                <p class="card-text"><strong>Hourly Pay:</strong> ${data.pay}</p>
                <a href="{{site.baseurl}}/applyjob?id=${data.id}" class="btn btn-primary">Apply For This Job</a>
            </div>
        `;

        // Append card to container
        resultContainer.appendChild(card);
    }

    function goBack() {
        window.history.back();
    }
</script>
<script>
    function apply_Count() {
    const displayApplyCountUrl = "http://127.0.0.1:8181/api/jobuser/applycount?id=" + jobdetails;
    // const displayApplyCountUrl = "https://jobly.stu.nighthawkcodingsociety.com/api/jobuser/applycount?id=" + jobdetails;
    const displayApplyCountHeaders = {
      method: 'GET', // *GET, POST, PUT, DELETE, etc.
      mode: 'cors', // no-cors, *cors, same-origin
      cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
 
      headers: {
        'Content-Type': 'application/json'
        // 'Content-Type': 'application/x-www-form-urlencoded',
      },
    };
    // fetch the API
    fetch(displayApplyCountUrl, displayApplyCountHeaders)
      .then(response => {
        // Error handling
        if (response.status !== 200) {
          const errorMsg = 'Database response error: ' + response.status;
          console.log(errorMsg);
        
          
        }
        response.json().then(data => {
            console.log('apply count')
            console.log(data)
        document.getElementById('applycount').innerHTML = `${data} users have applied to this job.`
          });
        
      })
      .catch(err => {
        // Error handling
        console.error(err);
        
      });
    }
</script>
</body>
</html>

