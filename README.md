<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UNO KUM CALCULATION</title>
    <style>
        body {
            font-family: 'Press Start 2P', cursive;
            text-align: center;
            margin: 20px;
            background-color: #1f1f1f; /* Dark Background */
            color: #ffffff; /* White Text */
        }
        h1 {
            color: #ffffff; /* White Heading */
            margin-bottom: 20px;
        }
        input, button {
            margin: 10px;
            padding: 10px;
            border: 1px solid #3c3c3c; /* Darker Gray Border */
            border-radius: 4px;
            font-size: 16px;
            color: #1f1f1f; /* Dark Background Text */
            background-color: #ffffff; /* White Background */
        }
        button {
            color: #1f1f1f; /* Dark Background Text */
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #3c3c3c; /* Slightly Darker Gray on Hover */
        }
        #playerProfiles {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 20px;
        }
        #playerProfiles > div {
            width: 250px;
            padding: 20px;
            border: 1px solid #3c3c3c; /* Darker Gray Border */
            border-radius: 6px;
            background-color: #ffffff; /* White Background */
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }
        table {
            margin-top: 30px;
            border-collapse: collapse;
            width: 100%;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        th, td {
            border: 1px solid #3c3c3c; /* Darker Gray Border */
            padding: 15px;
            text-align: left;
            color: #ffffff; /* White Text */
        }
        th {
            background-color: #1f1f1f; /* Dark Background */
        }
        .highScoreCaption {
            color: #e44d26; /* Red Color for High Score Caption */
        }
        .lowScoreCaption {
            color: #3498db; /* Dark Blue Text */
        }
        .crown {
            margin-left: 5px;
        }
        .poo {
            margin-left: 5px;
        }
    </style>
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap">
</head>
<body>

    <h1>UNO KUM CALCULATION</h1>

    <label for="numPlayers" style="color: #ffffff;">Enter the number of players: </label>
    <input type="number" id="numPlayers" min="1" required>
    <button onclick="generateProfiles()">Generate Profiles</button>

    <div id="playerProfiles"></div>

    <table id="scoreTable"></table>

    <script>
        var playerScores = [];

        function generateProfiles() {
            // Clear previous profiles and scores
            document.getElementById('playerProfiles').innerHTML = '';
            playerScores = [];

            // Get the number of players from the input
            var numPlayers = document.getElementById('numPlayers').value;

            // Generate player profiles
            for (var i = 1; i <= numPlayers; i++) {
                // Display input fields for the name, number of cards, action cards, and wild cards of each player
                var profileDiv = document.createElement('div');
                profileDiv.innerHTML = '<h3 style="color: #ffffff;">Player ' + i + '</h3>' +
                                       '<label for="playerName' + i + '" style="color: #1f1f1f;">Name:</label>' +
                                       '<input type="text" id="playerName' + i + '" required>' +
                                       '<br>' +
                                       '<label for="numberCards' + i + '" style="color: #1f1f1f;">Number Cards:</label>' +
                                       '<input type="number" id="numberCards' + i + '" min="0" value="0" required>' +
                                       '<br>' +
                                       '<label for="actionCards' + i + '" style="color: #1f1f1f;">Action Cards (x20):</label>' +
                                       '<input type="number" id="actionCards' + i + '" min="0" value="0" required>' +
                                       '<br>' +
                                       '<label for="wildCards' + i + '" style="color: #1f1f1f;">Wild Cards (x50):</label>' +
                                       '<input type="number" id="wildCards' + i + '" min="0" value="0" required>' +
                                       '<br>' +
                                       '<button onclick="calculateScores(' + i + ')" style="color: #1f1f1f; background-color: #ffffff;">Calculate Scores</button>' +
                                       '<p id="scoreDisplay' + i + '"></p>' +
                                       '<br>';
                document.getElementById('playerProfiles').appendChild(profileDiv);
            }
        }

        function calculateScores(playerNumber) {
            // Get the entered values
            var playerName = document.getElementById('playerName' + playerNumber).value;
            var numberCards = parseFloat(document.getElementById('numberCards' + playerNumber).value);
            var actionCards = parseFloat(document.getElementById('actionCards' + playerNumber).value);
            var wildCards = parseFloat(document.getElementById('wildCards' + playerNumber).value);

            // Multiply by their respective multipliers
            var numberCardsScore = numberCards * 1;
            var actionCardsScore = actionCards * 20;
            var wildCardsScore = wildCards * 50;

            // Calculate total score
            var totalScore = numberCardsScore + actionCardsScore + wildCardsScore;

            // Store the total score for the player
            playerScores.push({ playerName: playerName, playerNumber: playerNumber, totalScore: totalScore });

            // Display the total score
            var displayElement = document.getElementById('scoreDisplay' + playerNumber);
            displayElement.innerHTML = 'Total Score: ' + totalScore;

            // Update the score table
            updateScoreTable();
        }

        function updateScoreTable() {
            // Sort playerScores array by totalScore in descending order
            playerScores.sort((a, b) => b.totalScore - a.totalScore);

            // Create and update the score table
            var table = document.getElementById('scoreTable');
            table.innerHTML = '';

            // Create table caption
            var caption = table.createCaption();
            if (playerScores.length > 0) {
                caption.innerHTML = 'Player Scores';
            }

            // Create table header
            var header = table.createTHead();
            var row = header.insertRow(0);
            var th1 = row.insertCell(0);
            var th2 = row.insertCell(1);
            th1.innerHTML = '<b>Player Name</b>';
            th2.innerHTML = '<b>Total Score</b>';

            // Create table body
            var tbody = table.createTBody();
            for (var i = 0; i < playerScores.length; i++) {
                var tr = tbody.insertRow(i);
                var td1 = tr.insertCell(0);
                var td2 = tr.insertCell(1);
                td1.innerHTML = playerScores[i].playerName;
                td2.innerHTML = playerScores[i].totalScore;

                // Add emojis for the highest and lowest scores
                if (i === 0) {
                    td1.innerHTML += ' <span class="crown">👑</span><br><em class="highScoreCaption" style="color: #e44d26;">Don\'t be too happy, you are just lucky</em>';
                } else if (i === playerScores.length - 1) {
                    td1.innerHTML += ' <span class="poo">💩</span><br><em class="lowScoreCaption">What a loser!</em>';
                }
            }
        }
    </script>

</body>
</html>
