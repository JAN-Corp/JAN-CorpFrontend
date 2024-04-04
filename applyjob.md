---

permalink: /applyjob/
---

<html>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <style>
            /* CSS styles */
            .centertext {
                text-align: center;
            }
    
            table {
                margin: 0 auto; /* Center the table */
            }
    
            th, td {
                padding: 10px;
                text-align: left;
            }
    
            .textarea {
                width: 100%; /* Set the width to 100% of the parent container */
                resize: vertical; /* Allow vertical resizing */
            }
    
            /* Additional styling if needed */
        </style>
    </head>
    <body>
        <h2 class="centertext">Apply For This Job!</h2>
        <h3 class="centertext">Hey! Enter more information about yourself to let the job postee know of your application.</h3>
       
            <table>
                <tr>
                    <td><label for="address">Address</label></td>
                    <td><input type="text" id="address" name="address" required></td>
                </tr>
                <tr>
                    <td><label for="email">Email</label></td>
                    <td><textarea id="email" name="email" required></textarea></td>
                </tr>
                <tr>
                    <td><label for="separationFactor">What separates you from others?</label></td>
                    <td><textarea id="separationFactor" name="separationFactor" required></textarea></td>
                </tr>
                
               
                    
                <tr>
                    <td>
                    <label for="resume">Resume</label>
                    </td>
                    <td>
                        <input type="file" id="resume" name="resume" accept="image/png, image/jpeg" />
                    </td>
                </tr>    

                </tr>
                <tr>
                    <td colspan="2" class="centertext">
                        <button onclick="applyJob()">Apply For This Job</button>
                    </td>
                </tr>
            </table>
            </body>
            <script>

                const checkStatusUrl = "http://127.0.0.1:8181/api/users/apply";
                const checkStatusHeaders = {
                  method: 'GET', // *GET, POST, PUT, DELETE, etc.
                  mode: 'cors', // no-cors, *cors, same-origin
                  cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
                  credentials: 'include', // include, *same-origin, omit
                  headers: {
                    'Content-Type': 'application/json'
                    // 'Content-Type': 'application/x-www-form-urlencoded',
                  },
                };
                // fetch the API
                fetch(checkStatusUrl, checkStatusHeaders)
                  .then(response => {
                    console.log(response)
                    if (response.status !== 200) {
                      const errorMsg = 'Database response error: ' + response.status;
                      console.log(errorMsg);
                      window.location.href = '{{site.baseurl}}/userapply';
                      return;
                    }
                    response.json().then(data => {
                        if (data !== null) {
                            console.log(data)
            
                        
                            return;
                        }
                    });
                  })
                  .catch(err => {
                    console.error(err);
            
                  });
                // ...
                </script>
<script>
    let urlParams = new URLSearchParams(window.location.search);
    let jobid = urlParams.get('id');   
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
  
  function applyJob() {
      // Extract data from inputs
      //const name = document.getElementById("name").value;
      //const password = document.getElementById("password").value;
      //const dob = document.getElementById("dob").value;
      const address = document.getElementById('address').value;
    const email = document.getElementById('email').value;
    console.log(email)
    const separationFactor = document.getElementById('separationFactor').value;
      userid = getCookie("userid");
      // Prepare data for POST request
      const applicationDetails = {
          address: address,
          email: email,
          separationFactor: separationFactor
      };
      
      // Prepare request options
      const applicationDetailsHeader = {
          method: 'POST',
          headers: new Headers({'content-type': 'application/json'}),
          credentials: 'include',
          body: JSON.stringify(applicationDetails), // Convert data object to JSON string
          mode: 'cors',
      };
  
      // URL for Create API
      // const url = 'http://127.0.0.1:8181/api/jwt_auth/register';
     const applicationDetailsUrl = 'http://127.0.0.1:8181/api/job/submitapplication?jobid=' + jobid + '&userid=' + userid;
     
      // Async fetch API call to the database to create a new user
      fetch(applicationDetailsUrl, applicationDetailsHeader)
          .then(response => {
              // Handle server response
              if (response.status !== 200) {
                  const errorMsg = 'Database response error: ' + response.status;
                  console.log(errorMsg);
                  return;
              }
  
              // Response contains valid result
              response.json().then(data => {
                  if (data !== null) {
                      console.log(data)
                      document.cookie = "auth="+data;
                      window.location.href = '{{site.baseurl}}/application';
                  }
              });
          })
          .catch(error => {
              // Handle fetch errors
              console.error('Fetch error:', error);
          });
  }
  
</script>
</html>
