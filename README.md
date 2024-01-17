# PersonalityTest
What dog breed are you?
#quiz-container {
    width: 500px;
    margin: 50px auto;
    font-family: "Arial", sans-serif;
    padding: 30px;
    border: 2px solid #3498db; /* Dodger blue border */
    border-radius: 10px;
    box-shadow: 3px 3px 8px rgba(0, 0, 0, 0.2);
  }
  
  #question-area h2 {
    font-size: 1.5em;
    margin-bottom: 15px;
    color: #3498db; /* Title color */
  }
  
  .option {
    padding: 12px;
    margin-bottom: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
    cursor: pointer;
    background-color: #f5f5f5; /* Light background */
  }
  
  /* Highlight selected answer with a border */
  .option.selected {
    border: 2px solid #3498db;
  }
  
  .option:hover {
    background-color: #e8e8e8;
  }
  
  #next-button {
    background-color: #3498db;
    color: white;
    padding: 12px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    display: block; /* Full width on last question */
    margin: 20px auto 0 auto;
  }
  
  #result-area {
    text-align: center;
    margin-top: 30px;
  }
  
  #result-area h2 {
    font-size: 2em;
    color: #333; /* Title color */
    margin-bottom: 10px;
  }
  
  #result-area img {
    max-width: 250px;
    height: auto;
    border-radius: 8px;
    box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
  }
  
  #result-area p {
    font-size: 1.1em;
    margin-bottom: 15px;
  }
  
  #result-area ul {
    list-style: disc;
    margin-left: 40px; /* Indent list */
  }
<!DOCTYPE html>
<html>
<head>
  <title>What Dog Breed Are You?</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div id="quiz-container">
    <h1>What Dog Breed Are You?</h1>
    <div id="question-area">
      </div>
    <button id="next-button">Next</button>
    <div id="result-area">
      </div>
  </div>
  <script src="script.js"></script>
</body>
</html>
const questions = [
    {
      questionText: "What's your energy level?",
      options: ["High (Always ready to play!)", "Medium (Couch potato with bursts of energy)", "Low (Naps are life)"]
    },
    {
      questionText: "How do you feel about strangers?",
      options: ["Love them! Can't wait to meet new people", "Neutral - it depends", "Shy, I prefer my people"]
    },
    {
      questionText: "What's your ideal home environment?",
      options: ["Big yard to run around in", "Bustling city with lots of action", "Quiet and peaceful"]
    },
    {
      questionText: "How would you describe your personality?",
      options: ["Outgoing and playful", "Loyal and protective", "Independent and a bit stubborn"]
    }
  ];
  
  
  const dogBreeds = [
    {
      name: "Golden Retriever",
      image: "https://media.istockphoto.com/id/93215758/photo/golden-retriever.jpg?s=2048x2048&w=is&k=20&c=bwf4MSiLA6u23HG9e0XyQYcxMSGg0VjMZlF84JgSsD4=",
      description: "Friendly, playful, energetic, needs lots of exercise and attention.",
      reasons: ["You're always up for an adventure!", "You love spending time with people."],
      scoreModifiers: [2, 2, 1, 2] // High energy, loves people, adaptable, outgoing
    },
    {
      name: "Labrador Retriever",
      image: "https://media.istockphoto.com/id/1251033537/photo/portrait-of-a-blond-labrador-retriever-dog-looking-at-the-camera-with-a-big-happy-smile.jpg?s=2048x2048&w=is&k=20&c=xn_YHsbj3pIinOSLc3CZu06rQrPFEkWrRaPDxbUQzOE=",
      description: "Similar to Golden Retrievers - friendly, active, and outgoing.",
      reasons: ["You're a people-person!", "You have a lot of energy to burn."],
      scoreModifiers: [2, 2, 1, 2]
    },
    {
      name: "Border Collie",
      image: "https://media.istockphoto.com/id/1680600709/photo/head-shot-of-a-young-black-and-white-border-collie-panting-looking-at-the-camera-one-year-old.jpg?s=2048x2048&w=is&k=20&c=hz32Su5uw9nqqNyXWdHz-5w4lK45b8lqAIzDiAlIhEU=",
      description: "Intelligent, high-energy working breed, needs a lot of mental stimulation.",
      reasons: ["You're always up for a challenge!", "You need a dog to keep up with your active lifestyle."],
      scoreModifiers: [3, 1, 2, 2] // Very high energy, can be reserved, needs space, intelligent
    },
    {
      name: "German Shepherd",
      image: "https://media.istockphoto.com/id/155373560/photo/portrait-of-a-german-shephard.jpg?s=1024x1024&w=is&k=20&c=6Y-pktje9O8M5zFG140ymyds0rtJ6-nwY2tg2wlPlC0=",
      description: "Loyal, protective, intelligent, needs a firm owner.",
      reasons: ["You value loyalty above all else.", "You're looking for a dog who can be a companion and protector."],
      scoreModifiers: [2, 3, 2, 3] // High energy, protective, adaptable, loyal
    },
    {
      name: "Shiba Inu",
      image: "https://media.istockphoto.com/id/653001154/photo/red-shiba-inu-dog-on-white-background.jpg?s=2048x2048&w=is&k=20&c=9T4s8ub6qNJrb7GYLnZjRplniRTddrRoFzO2P4h3xHc=",
      description: "Alert, independent, sometimes stubborn, can be aloof.",
      reasons: ["You're a self-sufficient person.", "You admire a dog with a strong personality."],
      scoreModifiers: [1, 1, 3, 3] // Moderate energy, independent, adaptable, strong-willed
    }
  ];
  
  let currentQuestionIndex = 0;
  let userAnswers = new Array(questions.length).fill(-1); // Initialize with -1 to indicate unanswered
  
  const questionArea = document.getElementById("question-area");
  const nextButton = document.getElementById("next-button");
  const resultArea = document.getElementById("result-area");
  
  function loadQuestion() {
    // Clear previous question and options if they exist
    questionArea.innerHTML = "";
  
    const question = questions[currentQuestionIndex];
  
    const questionTitle = document.createElement("h2");
    questionTitle.textContent = question.questionText;
    questionArea.appendChild(questionTitle);
  
    question.options.forEach((option, index) => {
      const optionEl = document.createElement("div");
      optionEl.classList.add("option");
      optionEl.textContent = option;
  
      // Add click event listener to option elements
      optionEl.addEventListener("click", () => selectAnswer(index));
  
      questionArea.appendChild(optionEl);
    });
  }
  
  function selectAnswer(answerIndex) {
    userAnswers[currentQuestionIndex] = answerIndex; // Store answer at the corresponding index
    // Add visual feedback for selected answer
    document.querySelectorAll('.option').forEach((option, index) => {
      option.classList.toggle('selected', index === answerIndex);
    });
  }
  
  function showResult() {
    resultArea.innerHTML = "";
  
    const matchedBreed = calculateMatchingBreed();
    
    if (!matchedBreed) {
      resultArea.textContent = "No matching breed found.";
      return;
    }
  
    const resultTitle = document.createElement("h2");
    resultTitle.textContent = "You are a " + matchedBreed.name + "!";
    resultArea.appendChild(resultTitle);
  
    // Displaying image, description, and reasons
    const breedImage = document.createElement("img");
    breedImage.src = matchedBreed.image;
    resultArea.appendChild(breedImage);
  
    const description = document.createElement("p");
    description.textContent = matchedBreed.description;
    resultArea.appendChild(description);
  
    const reasonsList = document.createElement("ul");
    matchedBreed.reasons.forEach(reason => {
      const reasonItem = document.createElement("li");
      reasonItem.textContent = reason;
      reasonsList.appendChild(reasonItem);
    });
    resultArea.appendChild(reasonsList);
  }
  
  
  function calculateMatchingBreed() {
    let highestScore = -1;
    let matchedBreed = null;
  
    for (let i = 0; i < dogBreeds.length; i++) {
      let breedScore = 0;
      for (let j = 0; j < userAnswers.length; j++) {
        if (userAnswers[j] !== -1) {
          breedScore += dogBreeds[i].scoreModifiers[userAnswers[j]];
        }
      }
  
      if (breedScore > highestScore) {
        highestScore = breedScore;
        matchedBreed = dogBreeds[i];
      }
    }
  
    return matchedBreed;
  }
  
  // Event Listener for the "Next" Button
  nextButton.addEventListener("click", () => {
    if (userAnswers[currentQuestionIndex] === -1) {
      alert("Please select an answer before continuing.");
      return;
    }
  
    currentQuestionIndex++;
    if (currentQuestionIndex < questions.length) {
      loadQuestion();
    } else {
      showResult();
    }
    // handling case when quiz is finished
    if (currentQuestionIndex === questions.length) {
      nextButton.textContent = "Restart Quiz";
      nextButton.onclick = () => window.location.reload(); // Restart the quiz
    }
  });
  
  
  loadQuestion(); // Start the quiz with the first question


  

