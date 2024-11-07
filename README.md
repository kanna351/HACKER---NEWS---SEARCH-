# HACKER---NEWS---SEARCH-
https://codepen.io/Rampalli-Rahul/pen/GRVYxGM
# HACKER---NEWS---SEARCH-
https://codepen.io/Rampalli-Rahul/pen/GRVYxGM<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login & Dashboard</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <!-- Login Page (initially visible) -->
    <div id="loginPage" class="login-container">
        <h2>Login</h2>
        <form id="loginForm">
            <label for="userId">Username</label>
            <input type="text" id="userId" placeholder="Enter your username" required>
            <label for="password">Password</label>
            <input type="password" id="password" placeholder="Enter your password" required>
            <button type="button" onclick="signIn()">Login</button>
        </form>
        <p id="message"></p>
    </div>

    <!-- Dashboard Page (hidden initially) -->
    <div id="dashboardPage" class="dashboard-container" style="display: none;">
        <div class="header">
            <div id="usernameDisplay" class="username-display">Search Hacker News</div>
            <div class="search-container">
                <input type="text" id="searchInput" placeholder="Search Hacker News..." oninput="searchNews()">
            </div>
        </div>
        <div id="resultsContainer" class="results-container">
            <!-- Search results will appear here -->
        </div>
    </div>

    <script>
        // login.js

        // Hardcoded credentials for demo
        const validUserId = "user123";
        const validPassword = "password123";

        // Sign-in function
        function signIn() {
            const userId = document.getElementById("userId").value;
            const password = document.getElementById("password").value;
            const messageElement = document.getElementById("message");

            if (userId === validUserId && password === validPassword) {
                // Save the username to localStorage and redirect to dashboard
                window.localStorage.setItem("username", userId);
                showDashboard();
            } else {
                messageElement.style.color = "red";
                messageElement.textContent = "Invalid Username or Password";
            }
        }

        // Show dashboard after login
        function showDashboard() {
            const username = window.localStorage.getItem("username");
            const loginPage = document.getElementById("loginPage");
            const dashboardPage = document.getElementById("dashboardPage");

            // Hide login page and show dashboard page
            loginPage.style.display = "none";
            dashboardPage.style.display = "block";

            // Set username in dashboard header
            const usernameDisplay = document.getElementById("usernameDisplay");
            usernameDisplay.textContent = `Welcome, ${username}!`;
        }

        // dashboard.js

        // Retrieve the username from localStorage and show it in the header
        window.onload = function () {
            const username = window.localStorage.getItem("username");
            if (username) {
                showDashboard(); // Show dashboard if user is already logged in
            } else {
                document.getElementById("loginPage").style.display = "block"; // Show login if not logged in
            }
        }

        // Search functionality for Hacker News
        async function searchNews() {
            const query = document.getElementById("searchInput").value.trim();
            const resultsContainer = document.getElementById("resultsContainer");

            if (query === "") {
                resultsContainer.innerHTML = "";
                return;
            }

            try {
                const response = await fetch(`https://hn.algolia.com/api/v1/search?query=${query}`);
                const data = await response.json();
                displayResults(data.hits);
            } catch (error) {
                resultsContainer.innerHTML = "<p>Error fetching data. Please try again later.</p>";
            }
        }

        // Function to display search results
        function displayResults(results) {
            const resultsContainer = document.getElementById("resultsContainer");
            resultsContainer.innerHTML = "";  // Clear previous results

            if (results.length === 0) {
                resultsContainer.innerHTML = "<p>No results found.</p>";
                return;
            }

            results.forEach(result => {
                const resultCard = document.createElement("div");
                resultCard.classList.add("result-card");

                const title = document.createElement("h3");
                title.innerHTML = `<a href="${result.url}" target="_blank">${result.title}</a>`;

                const author = document.createElement("p");
                author.innerText = `By ${result.author} | ${new Date(result.created_at).toLocaleDateString()}`;

                const commentCount = document.createElement("p");
                commentCount.innerText = `${result.num_comments} comments`;

                resultCard.appendChild(title);
                resultCard.appendChild(author);
                resultCard.appendChild(commentCount);

                resultsContainer.appendChild(resultCard);
            });
        }
    </script>

    <script src="login.js"></script>
    <script src="dashboard.js"></script>
</body>
</html

  /* General Styles */
body {
    font-family: Arial, sans-serif;
    background-color: #f7f7f7;
    margin: 0;
    padding: 0;
}

/* Login Page */
.login-container, .dashboard-container {
    max-width: 600px;
    margin: 50px auto;
    padding: 20px;
    background-color: white;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    text-align: center;
}

/* Login Form */
.login-container h2 {
    margin-bottom: 20px;
}

.login-container input {
    width: 100%;
    padding: 10px;
    margin-bottom: 15px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 16px;
}

.login-container button {
    width: 100%;
    padding: 10px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 4px;
    font-size: 16px;
    cursor: pointer;
}

.login-container button:hover {
    background-color: #45a049;
}

/* Dashboard Styles */
.header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #ff6600;
    padding: 10px 20px;
    color: white;
}

.username-display {
    font-size: 20px;
    font-weight: bold;
}

.search-container {
    width: 100%;
    max-width: 500px;
    display: flex;
    justify-content: flex-end;
}

#searchInput {
    padding: 10px;
    width: 100%;
    max-width: 300px;
    border-radius: 4px;
    border: 1px solid #ddd;
    font-size: 16px;
}

.results-container {
    padding: 20px;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 20px;
    margin-top: 20px;
}

.result-card {
    background-color: white;
    padding: 15px;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s ease;
}

.result-card:hover {
    transform: scale(1.05);
}

.result-card h3 {
    margin: 0;
    font-size: 18px;
}

.result-card p {
    color: #555;
    font-size: 14px;
    margin: 10px 0;
}

.result-card a {
    color: #ff6600;
    text-decoration: none;
}

