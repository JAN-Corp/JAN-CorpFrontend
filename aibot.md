---
permalink: /aibot
title: AI Bot
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Real Estate Bot</title>
    <style>
        .chatbot-container {
            width: 80vw; /* Set width to 80% of the viewport width */
            margin: 50px auto;
            background-color: #fff;
            border: 1px solid #e1e1e1;
            border-radius: 10px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
        }
        #header {
            background-color: dodgerblue;
            color: #ffffff;
            padding: 15px;
            border-radius: 10px 10px 0 0;
            text-align: center;
            font-size: 1.5em;
        }
        #conversation {
            height: 400px;
            overflow-y: auto;
            padding: 20px;
            border-bottom: 1px solid #e1e1e1;
        }
        .chatbot-message {
            margin-bottom: 20px;
            animation: fadeIn 0.3s linear;
        }
        .chatbot-text {
            background-color: #f1f1f1;
            color: #222;
            padding: 10px 15px;
            border-radius: 20px;
            display: inline-block;
            max-width: 80%;
        }
        #input-form {
            display: flex;
            align-items: center;
            padding: 10px;
        }
        #input-field {
            flex: 1;
            padding: 10px;
            border-radius: 20px;
            border: 1px solid #e1e1e1;
            outline: none;
        }
        #submit-button {
            background-color: dodgerblue;
            color: #ffffff;
            border: none;
            padding: 10px 20px;
            margin-left: 10px;
            border-radius: 20px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        #submit-button:hover {
            background-color: #45a049;
        }
        @keyframes fadeIn {
            from {opacity: 0;}
            to {opacity: 1;}
        }
        .user-message {
            text-align: right; /* Align user's message to the right */
        }
        .user-text {
            background-color: dodgerblue; /* Set background color to dodgerblue */
            color: #fff; /* Set text color to white */
            border-radius: 20px;
            padding: 10px 15px;
            display: inline-block;
            max-width: 80%;
        }
    </style>
</head>
<body>
    <div class="chatbot-container">
        <div id="header">AI Real Estate Bot</div>
        <div id="conversation">
            <!-- Chat messages will appear here -->
        </div>
        <form id="input-form">
            <input id="input-field" type="text" placeholder="Type your message here">
            <button id="submit-button" type="submit">Send</button>
        </form>
    </div>
<script type="module">
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    // Function to get the JWT token from cookies
    function getJwtToken() {
        return document.cookie.split(';').find(cookie => cookie.trim().startsWith('jwt='));
    }
    // Function to redirect to the login page if the JWT token does not exist
    function redirectToLogin() {
        window.location.href = "{{site.baseurl}}/login"; // Adjust the login page URL as needed
    }
    // Check for the existence of the JWT token when the page loads
    window.addEventListener('load', function() {
        const jwtToken = getJwtToken();
        // If the JWT token does not exist, redirect to the login page
        if (!jwtToken) {
            redirectToLogin();
        }
    });
    // Your existing JavaScript code for fetching and rendering houses data
    document.addEventListener('DOMContentLoaded', () => {
        // Your existing JavaScript code for fetching and rendering houses data
    });
    document.addEventListener("DOMContentLoaded", function () {
        const conversation = document.getElementById("conversation");
        const inputField = document.getElementById("input-field");
        const submitButton = document.getElementById("submit-button");
        submitButton.addEventListener("click", function (e) {
            e.preventDefault();
            const userQuestion = inputField.value.trim();
            if (!userQuestion) return; // Don't send empty questions
            const accessCode = prompt("Please enter your access code:");
            if (!accessCode) return; // Don't proceed without access code
            // Display the user's prompt in a different style and position
            const userMessage = document.createElement("div");
            userMessage.classList.add("user-message"); // New class for user messages
            const userText = document.createElement("div");
            userText.classList.add("user-text"); // New class for user text
            userText.textContent = userQuestion;
            userMessage.appendChild(userText);
            conversation.appendChild(userMessage);
            // Send the user's question to the API
            const url = uri + '/api/house/openai';
            fetch(url + `?question=${encodeURIComponent(userQuestion)}&code=${accessCode}`, {method: 'GET', mode: 'cors'}).then((response) => response.json()).then((data) => {
                    // Display the chatbot's response
                    const chatbotMessage = document.createElement("div");
                    chatbotMessage.classList.add("chatbot-message");
                    const chatbotText = document.createElement("div");
                    chatbotText.classList.add("chatbot-text");
                    chatbotText.textContent = data;
                    chatbotMessage.appendChild(chatbotText);
                    conversation.appendChild(chatbotMessage);
                    conversation.scrollTop = conversation.scrollHeight;
                    inputField.value = "";
                    inputField.focus();
                })
                .catch((error) => {
                    console.error("Error fetching data from the API:", error);
                });
        });
    });
</script>
</body>
</html>
