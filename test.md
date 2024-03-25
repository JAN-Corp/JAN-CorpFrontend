---
permalink: /test
title: Test
---

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>String Sender</title>
</head>
<body>
  <form id="stringForm">
    <label for="string1">String 1 (Path Variable):</label>
    <input type="text" id="string1" name="string1" required>
    <br>
    <label for="string2">String 2 (Query Parameter):</label>
    <input type="text" id="string2" name="string2" required>
    <br>
    <button type="submit">Send Strings</button>
  </form>

  <h1 id="result">Result will be displayed here</h1>

  <script>
    document.getElementById('stringForm').addEventListener('submit', function (e) {
      e.preventDefault();

      const string1 = document.getElementById('string1').value;
      const string2 = document.getElementById('string2').value;

      const url = 'https://collegerankings.stu.nighthawkcodingsociety.com/api/helloworld/say/'+string1+'?lastname='+ string2;

      console.log("URL: " + url);
      fetch(url, {
        method: 'GET',
        headers: {
          'Accept': 'application/json'
        }
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('Network response was not ok', response.statusText);
        }
        return response.json();
      })
      .then(data => {
        console.log(data);
        document.getElementById('result').textContent = JSON.stringify(data); // Display result in the h1 element
      })
      .catch(error => {
        console.error('Error:', error);
        document.getElementById('result').textContent = 'Error: ' + error; // Display error in the h1 element
      });
    });
  </script>
</body>
</html>
