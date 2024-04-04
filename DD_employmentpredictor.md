---

permalink: /employmentpredictor/
title: Employment Predictor
---

<style>
    body {
        color: #fff;
        font-family: Arial, sans-serif;
        padding: 20px;
    }
    form {
        max-width: 400px;
        margin: 0 auto;
    }
    label {
        display: block;
        margin-bottom: 5px;
    }
    input[type="text"],
    input[type="number"],
    select {
        width: 100%;
        padding: 8px;
        margin-bottom: 10px;
        background-color: #333;
        border: none;
        border-radius: 4px;
        color: #fff;
    }
    input[type="checkbox"] {
        margin-bottom: 10px;
    }
    button {
    padding: 10px 20px;
    background-color: #74C0FC;
    border: none;
    border-radius: 4px;
    color: #1A1A1A;
    cursor: pointer;
}
button:hover {
    background-color: #5AA6E5;
}
    #result {
        margin-top: 20px;
        padding: 20px;
        background-color: #333;
        border-radius: 12px;
    }
    #result h2 {
        color: #74C0FC;
        margin-bottom: 10px;
    }
    #result p {
        margin-bottom: 5px;
    }
</style>
<body>
  <h1>Will you get the job?</h1>
    <form id="jobForm">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required><br><br>
       <label for="EdLevel">Education Level:</label>
        <select id="EdLevel" name="EdLevel" required>
            <option value="Undergraduate">Undergraduate</option>
            <option value="Master">Master</option>
            <option value="PhD">PhD</option>
            <option value="Other">Other</option>
        </select><br><br>
        <label for="Employment">Employment:</label>
        <select id="Employment" name="Employment" required>
            <option value="Dev">Dev</option>
            <option value="NotDev">NotDev</option>
        </select><br><br>
        <label for="Gender">Gender:</label>
        <select id="Gender" name="Gender" required>
            <option value="Man">Man</option>
            <option value="Woman">Woman</option>
        </select><br><br>

        <label for="YearsCode">Years of Coding:</label>
        <input type="number" id="YearsCode" name="YearsCode" required><br><br>
        <label for="YearsCodePro">Years of Professional Coding:</label>
        <input type="number" id="YearsCodePro" name="YearsCodePro" required><br><br>

        </select><br><br>
        <button type="button" onclick="predictJob()">Predict Job</button>
    </form>
    <div id="result"></div>

    <script>
    function predictJob() {
        var form = document.getElementById('jobForm');
        var name = document.getElementById('name');
        var resultDiv = document.getElementById('result');
        var formData = new FormData(form);
        fetch('http://localhost:8181/api/datasalary/predict', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
            },
            body: JSON.stringify(Object.fromEntries(formData))
        })
        .then(response => response.json())
        .then(data => {
            resultDiv.innerHTML = '<h2>Prediction Result for ' + name.value + '</h2>';
            for (var key in data) {
                resultDiv.innerHTML += '<p>' + key + ': ' + data[key] + '</p>';
            }
            var unemploymentProbability = parseFloat(data['Unemployment probability']);
            var employmentProbability = parseFloat(data['Employment probability']);
            if (employmentProbability > unemploymentProbability) {
                resultDiv.innerHTML += '<h3>Solid Odds</h3>';
            } else {
                resultDiv.innerHTML += '<h3>Not Looking So Good</h3>';
            }
        })
        .catch(error => {
            console.error('Error:', error);
        });
    }
    </script>
</body>


