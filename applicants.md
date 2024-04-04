---

permalink: /applicants/
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
       
        <table>
            <thead>
              <tr>
                <th>User ID</th>
                <th>Name</th>
                <th>Status</th>
                <th>View Application</th>
        
              </tr>
            </thead>
            <tbody id="results">
             
            </tbody>
          </table>
    
    <script>
        let urlParams = new URLSearchParams(window.location.search);
        let jobid = urlParams.get('jobid');   
    
        // prepare HTML result container for new output
        const resultContainer = document.getElementById("results");
      
    
        const displayApplicantsUrl = 'http://127.0.0.1:8181/api/jobuser/whoapplied?id=' + jobid;
        const displayApplicantsHeaders = {
          method: 'GET', // *GET, POST, PUT, DELETE, etc.
          mode: 'cors', // no-cors, *cors, same-origin
          cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
         
          headers: {
            'Content-Type': 'application/json'
            // 'Content-Type': 'application/x-www-form-urlencoded',
          },
        };
        // fetch the API
        fetch(displayApplicantsUrl, displayApplicantsHeaders)
        
          .then(response => {
            console.log(response)
            // Error handling
            if (response.status !== 200) {
              const errorMsg = 'Database response error: ' + response.status;
              console.log(errorMsg);
      
            }
            response.json().then(data => {
          
              data.forEach(row => {
                  const tr = document.createElement("tr");
                  tr.innerHTML = `
                      <td>${row.id}</td>
                      <td>${row.name}</td>
                      <td>${row.status}</td>
                      <td><a href="/joblyFrontend/viewapplication?userid=${row.id}&jobid=${jobid}">View Application</a></td>
                     
                  `;
                  resultContainer.appendChild(tr);
              });
            });
          })
          .catch(err => {
            // Error handling
            console.error(err);
         
          });
    
   
    </script>