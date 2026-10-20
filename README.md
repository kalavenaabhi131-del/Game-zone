btn.className = "option";
    btn.textContent = String.fromCharCode(65 + i) + ". " + answer;
    btn.onclick = () => answerQuestion(i);
    area.appendChild(btn);
  });
  document.getElementById("quizScore").textContent = "Current score: " + score;
}
function answerQuestion(choice) {
  if (locked) return;
  locked = true;
  const item = questions[qIndex];
  if (choice === item.correct) {
    score++;
    document.getElementById("feedback").textContent = "✅ Correct! " + item.why;
  } else {
    document.getElementById("feedback").textContent =
      "❌ The answer is " + item.a[item.correct] + ". " + item.why;
  }
  document.getElementById("quizScore").textContent =
    "Current score: " + score + " / " + questions.length;
  document.getElementById("nextBtn").disabled = false;
}
function nextQuestion() {
  if (!locked) return;
  if (qIndex < questions.length - 1) {
    qIndex++;
    loadQuestion();
  } else {
    best = Math.max(best, score);
    save("pythonpathBest", best);
    updateProgress();
    document.getElementById("quizNumber").textContent = "Quiz complete!";
    document.getElementById("quizQuestion").textContent =
      "You scored " + score + " out of " + questions.length + ".";
    document.getElementById("quizOptions").innerHTML = "";
    document.getElementById("feedback").textContent =
