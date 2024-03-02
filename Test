<!DOCTYPE html>
<html>
<body>

<div id="demographics" style="text-align: center;">
  <h2>Demographic Information</h2>
  <form>
    <label for="age">Age:</label><br>
    <input type="number" id="age" name="age"><br>
    <label for="sex">Sex:</label><br>
    <input type="text" id="sex" name="sex"><br>
    <label for="year">Year in college:</label><br>
    <input type="number" id="year" name="year"><br>
    <label for="caffeine">Caffeine today? If so, how much?</label><br>
    <input type="text" id="caffeine" name="caffeine"><br>
    <label for="confidence">On a scale of 1-10, how confident are you in your task completion skills?</label><br>
    <input type="number" id="confidence" name="confidence" min="1" max="10"><br>
    <label for="puzzles">Do you like Puzzles or Riddles?</label><br>
    <input type="text" id="puzzles" name="puzzles"><br>
    <label for="games">Do you play Video Games?</label><br>
    <input type="text" id="games" name="games"><br>
    <input type="button" value="Submit" onclick="submitDemographics()">
  </form>
</div>

<div id="stroopTest" style="text-align: center; display: none;">
  <h2>Stroop Test</h2>
  <div id="quiz" style="font-size: 50px; text-align: center; margin-top: 20%; min-height: 50px;"></div>
  <div id="buttons">
    <button id="start" onclick="startTest()">Start</button>
    <button id="red" onclick="submitAnswer('Red')" style="color: red;">Red</button>
    <button id="green" onclick="submitAnswer('Green')" style="color: green;">Green</button>
    <button id="yellow" onclick="submitAnswer('Yellow')" style="color: yellow;">Yellow</button>
    <button id="blue" onclick="submitAnswer('Blue')" style="color: blue;">Blue</button>
  </div>
  <div id="results" style="font-size: 50px; text-align: center; margin-top: 20%;"></div>
</div>



<script>
var questions = [
  { word: "Red", correctAnswer: "Blue" },
  { word: "Blue", correctAnswer: "Red" },
  { word: "Green", correctAnswer: "Yellow" },
  { word: "Yellow", correctAnswer: "Green" },
  { word: "Blue", correctAnswer: "Green" },
  { word: "Red", correctAnswer: "Yellow" },
  { word: "Green", correctAnswer: "Red" },
  { word: "Yellow", correctAnswer: "Blue" },
  { word: "Blue", correctAnswer: "Red" },
  { word: "Red", correctAnswer: "Blue" },
  { word: "Red", correctAnswer: "Green" },
  { word: "Green", correctAnswer: "Yellow" },
  { word: "Yellow", correctAnswer: "Red" },
  { word: "Blue", correctAnswer: "Green" },
  { word: "Green", correctAnswer: "Blue" },
  { word: "Blue", correctAnswer: "Yellow" },
  { word: "Yellow", correctAnswer: "Green" },
  { word: "Green", correctAnswer: "Red" },
  { word: "Red", correctAnswer: "Yellow" },
  { word: "Yellow", correctAnswer: "Blue" }
];

var demographicAnswers = [];
var allResults = [];
var currentQuestion = 0;
var startTime;
var testCount = 1;

function disableButtons() {
  document.getElementById('red').disabled = true;
  document.getElementById('green').disabled = true;
  document.getElementById('yellow').disabled = true;
  document.getElementById('blue').disabled = true;
}

function enableButtons() {
  document.getElementById('red').disabled = false;
  document.getElementById('green').disabled = false;
  document.getElementById('yellow').disabled = false;
  document.getElementById('blue').disabled = false;
}

function showQuestion() {
  if (currentQuestion >= questions.length) {
    // Calculate the score
    var correctAnswers = allResults.filter(function(result) {
      return result.correct == 1;
    }).length;
    var score = correctAnswers / questions.length * 100;

  

    // Only call the excel function after all tests
    if (testCount == 4) {
      excel(allResults);
    }
    currentQuestion = 0; // Reset question counter for the next test
    testCount++; // Increment the test count

    // Show the start button after 3 seconds
    setTimeout(function() {
      if (testCount <= 3 && currentQuestion >= questions.length) {
        document.getElementById('start').style.display = 'inline';
        document.getElementById('quiz').innerHTML = '';  // Clear the 'Loading...' message
      }
    }, 3000);

    return;
  }

  if (currentQuestion == 0) {
    document.getElementById('start').style.display = 'inline';  // Display the 'Start' button before each test
  }

  var question = questions[currentQuestion];
  document.getElementById('quiz').innerHTML = 
    `<div class="question" style="color:${question.correctAnswer}; font-size: 30px;"> ${question.word} </div>`;
  
  startTime = new Date();

  setTimeout(function() {
    document.getElementById('quiz').innerHTML = '';
  }, 350);
}

function submitAnswer(answer) {
  var endTime = new Date();
  var timeTaken = endTime - startTime;
  var question = questions[currentQuestion];
  var correct = 0;

  if (answer == question.correctAnswer) {
    correct = 1;
  }

  allResults.push({
    test: testCount,
    question: (testCount - 1) * questions.length + currentQuestion + 1, // This will number the questions from 1 to 80
    answer: answer,
    timeTaken: timeTaken,
    correct: correct
  });

  currentQuestion++;
  if (currentQuestion < questions.length) {
    showQuestion();
  } else {
    // Calculate the percentage of correct answers after each test
    var correctAnswers = allResults.filter(function(result) {
      return result.correct == 1 && result.test == testCount;
    }).length;
    var percentageCorrect = correctAnswers / questions.length * 100;

    // Display the percentage of correct answers
    document.getElementById('results').innerHTML = "Correct answers: " + percentageCorrect.toFixed(2) + "%";

    document.getElementById('quiz').innerHTML = "Loading...";
    setTimeout(showQuestion, 3000);
  }
}


function startTest() {
  document.getElementById('start').style.display = 'none';
  document.getElementById('red').style.display = 'inline';
  document.getElementById('green').style.display = 'inline';
  document.getElementById('yellow').style.display = 'inline';
  document.getElementById('blue').style.display = 'inline';
  enableButtons();

  var countdown = 3;
  var countdownInterval = setInterval(function() {
    document.getElementById('quiz').innerHTML = "The test will start in " + countdown + " seconds...";
    countdown--;
    if (countdown < 0) {
      clearInterval(countdownInterval);
      showQuestion();
    }
  }, 1000);
}

document.getElementById('red').style.display = 'none';
document.getElementById('green').style.display = 'none';
document.getElementById('yellow').style.display = 'none';
document.getElementById('blue').style.display = 'none';
disableButtons();

function excel(data) {
  var csv = 'Test,Question,Answer,Time Taken,Correct\n';
  for (var i = 0; i < data.length; i++) {
    if (data[i].test === 'Demographics') {
      csv += 'Demographics,' + data[i].age + ',' + data[i].sex + ',' + data[i].year + ',' + data[i].caffeine + ',' + data[i].confidence + ',' + data[i].puzzles + ',' + data[i].games + '\n';
    } else if (data[i].test) {
      csv += data[i].test + ',' + data[i].question + ',' + data[i].answer + ',' + data[i].timeTaken + ',' + data[i].correct + '\n';
    }
  }

// Create a Blob with the CSV data
  var blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });

  // Create a link element
  var url = URL.createObjectURL(blob);
  var link = document.createElement("a");
  link.setAttribute("href", url);

  link.style.visibility = 'hidden';
  // Append the link to the body and simulate a click to download
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
}

function submitDemographics() {
  // Get the form data
  var age = document.getElementById('age').value;
  var sex = document.getElementById('sex').value;
  var year = document.getElementById('year').value;
  var caffeine = document.getElementById('caffeine').value;
  var confidence = document.getElementById('confidence').value;
  var puzzles = document.getElementById('puzzles').value;
  var games = document.getElementById('games').value;

  // Reset allResults for the new set of tests
  allResults = [];

  // Store the demographic data in allResults
  allResults.push({
    test: 'Demographics',
    age: age,
    sex: sex,
    year: year,
    caffeine: caffeine,
    confidence: confidence,
    puzzles: puzzles,
    games: games
  });

  // Hide the demographic form and show the Stroop Test
  document.getElementById('demographics').style.display = 'none';
  document.getElementById('stroopTest').style.display = 'block';

  // Start the Stroop Test
  startTest();
}

// Rest of your JavaScript code goes here

</script>

</body>
</html>
