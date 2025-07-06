<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Love Quiz</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: 'Arial', sans-serif;
      background-color: #f7f7f7;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .quiz-container {
      background: #fff;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
      max-width: 500px;
      width: 90%;
      text-align: center;
    }

    #question {
      font-size: 1.2rem;
      margin-bottom: 20px;
      font-weight: bold;
    }

    #answers-button {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-bottom: 20px;
    }

    .btn {
      padding: 10px 20px;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 5px;
      background: #f0f0f0;
      cursor: pointer;
      transition: background 0.3s;
    }

    .btn:hover {
      background: #e0e0e0;
    }

    .btn.correct {
      background-color: #c8e6c9;
      border-color: #2e7d32;
    }

    .btn.wrong {
      background-color: #ffcdd2;
      border-color: #c62828;
    }

    #next-button {
      padding: 10px 20px;
      background-color: #4caf50;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 1rem;
      display: none;
    }

    #next-button:hover {
      background-color: #45a049;
    }

    #progress-bar-container {
      background: #ddd;
      height: 15px;
      border-radius: 8px;
      margin-bottom: 20px;
      overflow: hidden;
    }

    #progress-bar {
      height: 100%;
      width: 0%;
      background: #4caf50;
      transition: width 0.3s;
    }
  </style>
</head>
<body>
  <div class="quiz-container">
    <div id="progress-bar-container">
      <div id="progress-bar"></div>
    </div>
    <div id="question">Question appears here</div>
    <div id="answers-button"></div>
    <button id="next-button">Next</button>
  </div>

  <script>
    const questions = [
      {
        question: "What is the most important ingredient in a healthy romantic relationship?",
        answers: [
          { Text: "Physical attraction", correct: false },
          { Text: "Communication", correct: true },
          { Text: "Money", correct: false },
          { Text: "Jealousy", correct: false }
        ]
      },
      {
        question: "How do most people express love according to the 5 Love Languages?",
        answers: [
          { Text: "By sending memes", correct: false },
          { Text: "Through silence", correct: false },
          { Text: "Words of affirmation", correct: true },
          { Text: "Giving advice", correct: false }
        ]
      },
      {
        question: "What does it mean to “love someone unconditionally”?",
        answers: [
          { Text: "Never getting upset with them", correct: false },
          { Text: "Loving without expectations or conditions", correct: true },
          { Text: "Loving only when they love you back", correct: false },
          { Text: "Ignoring red flags", correct: false }
        ]
      },
      {
        question: "What is the purpose of compromise in a relationship?",
        answers: [
          { Text: "To win arguments", correct: false },
          { Text: "To keep score", correct: false },
          { Text: "To create balance", correct: true },
          { Text: "To avoid talking", correct: false }
        ]
      },
      {
        question: "What does it mean to be emotionally available in a relationship?",
        answers: [
          { Text: "You cry a lot", correct: false },
          { Text: "You talk only about happy things", correct: false },
          { Text: "You get jealous", correct: false },
          { Text: "You’re open to sharing feelings and listening", correct: true }
        ]
      }
    ];

    const questionElement = document.getElementById("question");
    const answerButton = document.getElementById("answers-button");
    const nextButton = document.getElementById("next-button");

    let currentQuestionIndex = 0;
    let score = 0;

    function startQuiz() {
      currentQuestionIndex = 0;
      score = 0;
      nextButton.innerHTML = "Next";
      showQuestion();
    }

    function showQuestion() {
      resetState();
      updateProgressBar();
      const currentQuestion = questions[currentQuestionIndex];
      questionElement.innerHTML = `${currentQuestionIndex + 1}. ${currentQuestion.question}`;

      currentQuestion.answers.forEach(answer => {
        const button = document.createElement("button");
        button.innerHTML = answer.Text;
        button.classList.add("btn");
        if (answer.correct) {
          button.dataset.correct = answer.correct;
        }
        button.addEventListener("click", selectAnswer);
        answerButton.appendChild(button);
      });
    }

    function resetState() {
      nextButton.style.display = "none";
      answerButton.innerHTML = "";
    }

    function updateProgressBar() {
      const progress = ((currentQuestionIndex + 1) / questions.length) * 100;
      document.getElementById("progress-bar").style.width = `${progress}%`;
    }

    function selectAnswer(e) {
      const selectedBtn = e.target;
      const isCorrect = selectedBtn.dataset.correct === "true";
      if (isCorrect) {
        selectedBtn.classList.add("correct");
        score++;
      } else {
        selectedBtn.classList.add("wrong");
      }

      Array.from(answerButton.children).forEach(button => {
        if (button.dataset.correct === "true") {
          button.classList.add("correct");
        }
        button.disabled = true;
      });

      nextButton.style.display = "inline-block";
    }

    function showScore() {
      resetState();
      questionElement.innerHTML = `🎉 You scored ${score} out of ${questions.length}!`;
      nextButton.innerHTML = "Play Again";
      nextButton.style.display = "inline-block";
      document.getElementById("progress-bar").style.width = "100%";
    }

    function handleNextButton() {
      currentQuestionIndex++;
      if (currentQuestionIndex < questions.length) {
        showQuestion();
      } else {
        showScore();
      }
    }

    nextButton.addEventListener("click", () => {
      if (currentQuestionIndex < questions.length) {
        handleNextButton();
      } else {
        startQuiz();
      }
    });

    startQuiz();
  </script>
</body>
</html>