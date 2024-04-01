---
permalink: /salarycheck
title: Salary Quiz
---
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Heart Disease Prediction</title>
</head>
<body>
    <h1>Heart Disease Prediction</h1>
    <form id="heartDiseaseForm">
        <label for="age">Age:</label>
        <input type="number" id="age" name="age" required><br><br>
        <label for="sex">Sex:</label>
        <select id="sex" name="sex" required>
            <option value="1">Male</option>
            <option value="0">Female</option>
        </select><br><br>
        <label for="chestPainType">Chest Pain Type:</label>
        <select id="chestPainType" name="chestPainType" required>
            <option value="1">Typical Angina</option>
            <option value="2">Atypical Angina</option>
            <option value="3">Non-anginal Pain</option>
            <option value="4">Asymptomatic</option>
        </select><br><br>
        <label for="restingBPS">Resting Blood Pressure (mm Hg):</label>
        <input type="number" id="restingBPS" name="restingBPS" required><br><br>
        <label for="cholesterol">Cholesterol (mg/dl):</label>
        <input type="number" id="cholesterol" name="cholesterol" required><br><br>
        <label for="maxHeartRate">Max Heart Rate:</label>
        <input type="number" id="maxHeartRate" name="maxHeartRate" required><br><br>
        <label for="exerciseAngina">Exercise Induced Angina:</label>
        <select id="exerciseAngina" name="exerciseAngina" required>
            <option value="0">No</option>
            <option value="1">Yes</option>
        </select><br><br>
        <label for="oldpeak">ST Depression Induced by Exercise Relative to Rest:</label>
        <input type="number" id="oldpeak" name="oldpeak" step="0.01" required><br><br>
        <button type="button" onclick="predictHeartDisease()">Predict Heart Disease</button>
    </form>
    <div id="predictionResult"></div>
    <script>
        function predictHeartDisease() {
            var form = document.getElementById('heartDiseaseForm');
            var predictionResult = document.getElementById('predictionResult');
            var formData = new FormData(form);
            fetch('http://localhost:8084/api/heart_disease/predict', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Accept': 'application/json'
                },
                body: JSON.stringify(Object.fromEntries(formData))
            })
            .then(response => response.json())
            .then(data => {
                var predictionMessage;
                if (data['Prediction'] === 1) {
                    predictionMessage = 'Likelihood of Heart Disease: High';
                } else {
                    predictionMessage = 'Likelihood of Heart Disease: Low';
                }
                predictionResult.innerText = predictionMessage;
            })
            .catch(error => {
                console.error('Error:', error);
            });
        }
    </script>
</body>
</html>


<button onclick="location.href='{{site.baseurl}}/houses'" type="button">
         Find Your House</button>

