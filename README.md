# Conflict-Resolution-Styles
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Conflict Styles Assessment</title>
    <style>
        body { font-family: sans-serif; line-height: 1.6; max-width: 800px; margin: 20px auto; padding: 20px; background: #f4f4f9; }
        .card { background: white; padding: 20px; border-radius: 8px; shadow: 0 2px 5px rgba(0,0,0,0.1); margin-bottom: 20px; }
        .question { margin-bottom: 25px; padding-bottom: 15px; border-bottom: 1px solid #eee; }
        .option { display: block; margin: 10px 0; cursor: pointer; }
        button { background: #2c3e50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer; font-size: 16px; }
        #results { display: none; margin-top: 30px; }
        .style-score { margin: 10px 0; padding: 10px; border-left: 5px solid #2c3e50; background: #ececec; }
    </style>
</head>
<body>
    <h1>Conflict Styles Assessment</h1>
    <p>Select one statement from each pair that best describes your behavior.</p>
    <form id="quizForm"></form>
    <button type="button" onclick="calculateResults()">See My Results</button>

    <div id="results">
        <h2>Your Conflict Style Profile</h2>
        <div id="scoreDisplay"></div>
    </div>

<script>
const questions = [
    { q: 1, options: [{text: "Occasionally I hold back and let others figure out how to resolve the conflict.", style: "Avoider"}, {text: "I aim to focus on similarities rather than differences in views.", style: "Accommodator"}]},
    { q: 2, options: [{text: "I like to resolve problems through negotiating.", style: "Compromiser"}, {text: "I try to make sure everyone's concerns are addressed.", style: "Problem Solver"}]},
    { q: 3, options: [{text: "I know what I want and I go for it.", style: "Competer"}, {text: "I sometimes aim to make the other person feel better in order to end a conflict.", style: "Accommodator"}]},
    { q: 4, options: [{text: "I like to resolve problems through negotiating.", style: "Compromiser"}, {text: "I'm willing to give up my own views if it will help the other person feel better.", style: "Accommodator"}]},
    { q: 5, options: [{text: "I always try to work together to solve problems.", style: "Problem Solver"}, {text: "I aim to avert uncomfortable situations when possible.", style: "Avoider"}]},
    { q: 6, options: [{text: "I do what I can to avoid tension.", style: "Avoider"}, {text: "I aim to convince others that I am right.", style: "Competer"}]},
    { q: 7, options: [{text: "I stall in order to take some time to think about problems before approaching them.", style: "Avoider"}, {text: "I am willing to compromise when others do.", style: "Compromiser"}]},
    { q: 8, options: [{text: "I know what I want and I go for it.", style: "Competer"}, {text: "I aim to discuss problems openly so that they can be worked out right away.", style: "Problem Solver"}]},
    { q: 9, options: [{text: "Sometimes conflicts are better left not discussed.", style: "Avoider"}, {text: "I try to get what I want.", style: "Competer"}]},
    { q: 10, options: [{text: "I know what I want and I go for it.", style: "Competer"}, {text: "I like to resolve problems through negotiating.", style: "Compromiser"}]},
    { q: 11, options: [{text: "I aim to discuss problems openly so that they can be worked out right away.", style: "Problem Solver"}, {text: "I sometimes aim to make the other person feel better in order to end a conflict.", style: "Accommodator"}]},
    { q: 12, options: [{text: "At times I keep my views to myself in order to avoid conflict.", style: "Avoider"}, {text: "I prefer a 'give and take' solution to problems where both sides make adjustments.", style: "Compromiser"}]},
    { q: 13, options: [{text: "If the other person can agree to disagree, I can do the same.", style: "Compromiser"}, {text: "I make sure others know my views.", style: "Competer"}]},
    { q: 14, options: [{text: "I share my thoughts and ask others to share theirs.", style: "Problem Solver"}, {text: "I aim to convince others that I am right.", style: "Competer"}]},
    { q: 15, options: [{text: "I sometimes aim to make the other person feel better in order to end a conflict.", style: "Accommodator"}, {text: "I aim to avert uncomfortable situations when possible.", style: "Avoider"}]},
    { q: 16, options: [{text: "I try make sure the other person does not get upset.", style: "Accommodator"}, {text: "I try to make sure others understand my reasoning and why I am right.", style: "Competer"}]},
    { q: 17, options: [{text: "I know what I want and I go for it.", style: "Competer"}, {text: "I aim to avert uncomfortable situations when possible.", style: "Avoider"}]},
    { q: 18, options: [{text: "I allow others to voice their opinions without objecting if it makes them feel better.", style: "Accommodator"}, {text: "I prefer a 'give and take' solution to problems where both sides make adjustments.", style: "Compromiser"}]},
    { q: 19, options: [{text: "I try to work out problems with others right away.", style: "Problem Solver"}, {text: "I sometimes stall in order to take some time to think about problems before approaching them.", style: "Avoider"}]},
    { q: 20, options: [{text: "I try to work out problems with others right away.", style: "Problem Solver"}, {text: "I prefer to figure out what the fairest outcome would be from everyone's perspective.", style: "Compromiser"}]},
    { q: 21, options: [{text: "I try to pay attention to the other person's opinions when we are working out problems.", style: "Accommodator"}, {text: "I prefer to talk about problems directly.", style: "Problem Solver"}]},
    { q: 22, options: [{text: "I take a problem-solving approach where all sides figure out what we can agree on and what we are willing to give up.", style: "Problem Solver"}, {text: "I tell others what I want.", style: "Competer"}]},
    { q: 23, options: [{text: "I tend to worry about making everyone happy.", style: "Accommodator"}, {text: "Occasionally I hold back and let others figure out how to resolve the conflict.", style: "Avoider"}]},
    { q: 24, options: [{text: "I try to please others if it seems important to them.", style: "Accommodator"}, {text: "I aim to work together to settle our differences through a bargaining approach.", style: "Compromiser"}]},
    { q: 25, options: [{text: "I try to convince people to agree with me.", style: "Competer"}, {text: "I try to pay attention to the other person's opinions when we are working out problems.", style: "Accommodator"}]},
    { q: 26, options: [{text: "I try to find a way for different sides to meet half way in a conflict.", style: "Compromiser"}, {text: "I tend to worry about making everyone happy.", style: "Accommodator"}]},
    { q: 27, options: [{text: "At times I keep my views to myself in order to avoid conflict.", style: "Avoider"}, {text: "I allow others to voice their opinions without objecting if it makes them feel better.", style: "Accommodator"}]},
    { q: 28, options: [{text: "I know what I want and I go for it.", style: "Competer"}, {text: "I always try to work together to solve problems.", style: "Problem Solver"}]},
    { q: 29, options: [{text: "I try to find a way for different sides to meet half way in a conflict.", style: "Compromiser"}, {text: "Sometimes conflicts are better left not discussed.", style: "Avoider"}]},
    { q: 30, options: [{text: "I try make sure the other person does not get upset.", style: "Accommodator"}, {text: "I tell others when something is wrong so that we can work together to make it right.", style: "Problem Solver"}]}
];

const form = document.getElementById('quizForm');
questions.forEach((item, index) => {
    let qDiv = document.createElement('div');
    qDiv.className = 'question card';
    qDiv.innerHTML = `<strong>${item.q}. Choose one:</strong><br>`;
    item.options.forEach((opt, optIndex) => {
        qDiv.innerHTML += `
            <label class="option">
                <input type="radio" name="q${index}" value="${opt.style}" required> ${opt.text}
            </label>`;
    });
    form.appendChild(qDiv);
});

function calculateResults() {
    const scores = { "Competer": 0, "Problem Solver": 0, "Compromiser": 0, "Avoider": 0, "Accommodator": 0 };
    const formData = new FormData(form);
    let answered = 0;
    for (let value of formData.values()) {
        scores[value]++;
        answered++;
    }
    if (answered < 30) { alert("Please answer all 30 questions!"); return; }

    document.getElementById('results').style.display = 'block';
    let display = document.getElementById('scoreDisplay');
    display.innerHTML = "";
    
    for (let style in scores) {
        let level = scores[style] >= 9 ? "Frequent Use" : (scores[style] >= 5 ? "Occasional Use" : "Infrequent Use");
        display.innerHTML += `
            <div class="style-score">
                <strong>${style}: ${scores[style]} / 12</strong> - ${level}
            </div>`;
    }
    window.scrollTo(0, document.body.scrollHeight);
}
</script>
</body>
</html>
