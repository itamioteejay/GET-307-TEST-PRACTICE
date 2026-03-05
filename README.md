<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GET307 AI & Machine Learning Quiz</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">

<style>
    body {
        font-family: 'Poppins', sans-serif;
        background: linear-gradient(135deg, #0f172a, #1e1b4b, #172554);
        background-attachment: fixed;
        color: #f8fafc;
        margin: 0;
        padding: 40px 20px;
    }

    .container {
        max-width: 800px;
        margin: 0 auto;
    }

    h1 {
        text-align: center;
        font-size: 2.5rem;
        background: -webkit-linear-gradient(45deg, #38bdf8, #a855f7);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        margin-bottom: 40px;
    }

    .question-card {
        background: rgba(30, 41, 59, 0.8);
        padding: 25px;
        margin: 25px 0;
        border-radius: 16px;
        box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
        border: 1px solid rgba(255, 255, 255, 0.05);
        backdrop-filter: blur(10px);
        transition: transform 0.3s ease;
    }

    .question-card:hover {
        transform: translateY(-2px);
    }

    .question-text {
        font-size: 1.2rem;
        font-weight: 600;
        margin-top: 0;
        margin-bottom: 20px;
        color: #e2e8f0;
    }

    .option-label {
        display: flex;
        align-items: center;
        padding: 15px 20px;
        margin-bottom: 12px;
        background: #334155;
        border-radius: 10px;
        cursor: pointer;
        transition: all 0.2s ease;
        border: 2px solid transparent;
    }

    .option-label:hover {
        background: #475569;
    }

    .option-label:has(input[type="radio"]:checked) {
        background: rgba(168, 85, 247, 0.2);
        border-color: #a855f7;
    }

    input[type="radio"] {
        margin-right: 15px;
        transform: scale(1.3);
        accent-color: #a855f7;
        cursor: pointer;
    }

    .btn-hint {
        background: transparent;
        color: #94a3b8;
        border: 1px solid #64748b;
        padding: 6px 15px;
        border-radius: 20px;
        font-size: 0.85rem;
        cursor: pointer;
        transition: 0.3s;
        margin-top: 10px;
    }

    .btn-hint:hover {
        background: #64748b;
        color: white;
    }

    .hint-box {
        display: none;
        background: rgba(250, 204, 21, 0.1);
        color: #facc15;
        padding: 12px 15px;
        border-left: 4px solid #facc15;
        margin-top: 15px;
        border-radius: 4px;
        font-size: 0.95rem;
    }

    /* New Feedback Box Styles */
    .feedback-box {
        display: none;
        margin-top: 15px;
        padding: 15px;
        border-radius: 8px;
        font-weight: 400;
        font-size: 1rem;
        animation: fadeIn 0.4s ease-in;
    }

    .feedback-correct {
        background: rgba(34, 197, 94, 0.15);
        color: #4ade80;
        border: 1px solid rgba(34, 197, 94, 0.4);
    }

    .feedback-wrong {
        background: rgba(239, 68, 68, 0.15);
        color: #f87171;
        border: 1px solid rgba(239, 68, 68, 0.4);
    }

    .feedback-warning {
        background: rgba(245, 158, 11, 0.15);
        color: #fbbf24;
        border: 1px solid rgba(245, 158, 11, 0.4);
    }

    .btn-submit {
        display: block;
        width: 100%;
        padding: 18px;
        font-size: 1.2rem;
        font-weight: 600;
        color: white;
        background: linear-gradient(90deg, #3b82f6, #8b5cf6);
        border: none;
        border-radius: 12px;
        cursor: pointer;
        box-shadow: 0 4px 15px rgba(139, 92, 246, 0.4);
        transition: all 0.3s ease;
        margin-top: 40px;
    }

    .btn-submit:hover {
        transform: scale(1.02);
        box-shadow: 0 6px 20px rgba(139, 92, 246, 0.6);
    }

    .result-container {
        display: none;
        text-align: center;
        margin-top: 40px;
        background: rgba(2, 6, 23, 0.8);
        padding: 40px;
        border-radius: 16px;
        border: 2px solid #38bdf8;
        animation: fadeIn 0.5s ease-in;
    }

    .result-container h2 {
        margin: 0;
        font-size: 2.5rem;
        color: #38bdf8;
    }

    .correct-ans {
        background: rgba(34, 197, 94, 0.2) !important;
        border-color: #22c55e !important;
        color: #4ade80;
    }
    
    .wrong-ans {
        background: rgba(239, 68, 68, 0.2) !important;
        border-color: #ef4444 !important;
        color: #f87171;
    }

    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(10px); }
        to { opacity: 1; transform: translateY(0); }
    }
</style>
</head>

<body>

<div class="container">
    <h1>GET307 AI & Machine Learning Quiz</h1>
    <form id="quizForm"></form>
    <button class="btn-submit" id="submitBtn" onclick="submitQuiz()">Submit Quiz</button>
    <div id="result" class="result-container"></div>
</div>

<script>
const quiz = [
    { q:"1. What primarily characterizes human intelligence?", options:["Massive data processing","Biological neural networks","Binary logic","Sensor arrays"], answer:1, hint:"Think about the biological system in the brain." },
    { q:"2. Artificial Intelligence mainly relies on:", options:["Emotion and intuition","Mathematical models","Biological neurons","Genetic coding"], answer:1, hint:"AI systems run using algorithms and math." },
    { q:"3. Artificial Narrow Intelligence (ANI) refers to:", options:["AI that can do everything","AI specialized for one task","Human intelligence","Robot consciousness"], answer:1, hint:"Today's AI systems are built for specific tasks." },
    { q:"4. Artificial General Intelligence (AGI) would be:", options:["Limited to one task","Human-level intelligence across domains","Simple automation","Spreadsheet algorithms"], answer:1, hint:"AGI is theoretical and would match human thinking." },
    { q:"5. Computational intelligence is often described as:", options:["Hard computing","Soft computing","Mechanical computing","Manual computing"], answer:1, hint:"It deals with uncertainty and real-world messiness." },
    { q:"6. Fuzzy logic helps systems understand:", options:["Binary values only","Continuous states","Only numbers","Computer memory"], answer:1, hint:"Example: slightly warm, somewhat fast." },
    { q:"7. In robotics, cameras replace human:", options:["Hands","Eyes","Memory","Voice"], answer:1, hint:"They allow machines to see." },
    { q:"8. LiDAR sensors are used for:", options:["Navigation and spatial mapping","Cooking automation","Audio recording","Weather prediction"], answer:0, hint:"Used heavily in self-driving vehicles." },
    { q:"9. Predictive maintenance helps industries:", options:["Repair machines after failure","Predict failures early","Ignore machine data","Reduce sensors"], answer:1, hint:"The goal is to detect problems before breakdown." },
    { q:"10. Smart manufacturing uses AI mainly to:", options:["Slow production","Optimize processes","Remove sensors","Increase waste"], answer:1, hint:"Efficiency is the main goal." },
    { q:"11. One ethical concern of AI systems is:", options:["Bias in data","Too much electricity","Slow computers","Screen brightness"], answer:0, hint:"Training data can cause unfair decisions." },
    { q:"12. Data privacy in AI refers to:", options:["Protecting collected data","Deleting sensors","Removing networks","Blocking algorithms"], answer:0, hint:"Large datasets must be secured." },
    { q:"13. Propositional logic deals with:", options:["Continuous values","True/False statements","Random numbers","Machine learning"], answer:1, hint:"Boolean logic." },
    { q:"14. Boolean logic includes:", options:["AND, OR, NOT","Multiply, divide","Add, subtract","Predict, classify"], answer:0, hint:"Basic digital logic gates." },
    { q:"15. Predicate logic allows AI to:", options:["Understand relationships","Only calculate numbers","Run hardware","Increase voltage"], answer:0, hint:"Uses variables and quantifiers." },
    { q:"16. In logic systems 'AND' means:", options:["Both conditions must be true","One must be true","None must be true","Random output"], answer:0, hint:"Both statements required." },
    { q:"17. Autonomous navigation systems process:", options:["Spatial data","Cooking data","Weather reports","Music files"], answer:0, hint:"Used in drones and robots." },
    { q:"18. Microcontrollers in AI systems perform:", options:["Decision-making tasks","Cooking operations","Printing operations","Internet browsing"], answer:0, hint:"They control embedded systems." },
    { q:"19. Sensors in AI systems replace human:", options:["Perception","Memory","Emotions","Sleep"], answer:0, hint:"Sensors detect environmental data." },
    { q:"20. Machine learning mainly focuses on:", options:["Learning patterns from data","Hardware manufacturing","Battery design","Manual programming"], answer:0, hint:"Algorithms learn from datasets." },
    { q:"21. A dataset is:", options:["Collection of data","Single value","Computer chip","Robot arm"], answer:0, hint:"Used to train AI." },
    { q:"22. Pattern recognition helps AI:", options:["Detect hidden relationships","Reduce electricity","Build sensors","Generate heat"], answer:0, hint:"Common in computer vision." },
    { q:"23. AI in manufacturing helps:", options:["Quality control","Increase defects","Slow systems","Remove machines"], answer:0, hint:"Cameras inspect products." },
    { q:"24. Computer vision allows machines to:", options:["See and interpret images","Smell objects","Cook food","Repair motors"], answer:0, hint:"Image processing." },
    { q:"25. Industrial AI systems analyze:", options:["Vibration data","Cooking recipes","Movie files","Paper notes"], answer:0, hint:"Used in predictive maintenance." },
    { q:"26. A major advantage of AI is:", options:["Processing massive datasets","Human emotions","Slow thinking","Limited memory"], answer:0, hint:"Computers process huge data quickly." },
    { q:"27. AI decision systems depend heavily on:", options:["Algorithms","Human mood","Paper reports","Mechanical gears"], answer:0, hint:"Step-by-step computational rules." },
    { q:"28. Ethical AI requires:", options:["Fair training data","Random outputs","Removing sensors","Slow computation"], answer:0, hint:"Bias must be avoided." },
    { q:"29. Robotics combines:", options:["AI and mechanical systems","Cooking and music","Medicine and law","Weather and farming"], answer:0, hint:"Hardware plus intelligence." },
    { q:"30. Industrial automation focuses on:", options:["Efficiency","Manual work","Reducing machines","Paper operations"], answer:0, hint:"Automation improves productivity." },
    { q:"31. Machine learning models improve through:", options:["Training","Sleeping","Charging","Cooling"], answer:0, hint:"Models learn from examples." },
    { q:"32. Data used for training AI must be:", options:["Relevant and clean","Random","Broken","Encrypted always"], answer:0, hint:"Quality data improves accuracy." },
    { q:"33. AI safety ensures:", options:["Systems behave predictably","Systems crash","Systems ignore humans","Systems delete data"], answer:0, hint:"Important in industrial robotics." },
    { q:"34. Autonomous robots rely on:", options:["Sensors and algorithms","Cooking instructions","Manual control","Human memory"], answer:0, hint:"Both perception and decision." },
    { q:"35. Convergent technologies combine:", options:["Multiple advanced technologies","Old mechanical tools","Manual labor","Paper systems"], answer:0, hint:"Integration of fields." },
    { q:"36. Neural networks are inspired by:", options:["Human brain","Electric motors","Mechanical gears","Solar panels"], answer:0, hint:"Biological neurons." },
    { q:"37. AI used for detecting defects relies on:", options:["Computer vision","Sound only","Temperature only","Manual inspection"], answer:0, hint:"Cameras inspect materials." },
    { q:"38. Real-time data processing is important for:", options:["Autonomous systems","Paper filing","Manual reports","Books"], answer:0, hint:"Robots must react instantly." },
    { q:"39. Soft computing helps systems handle:", options:["Uncertainty","Exact binary values","Only integers","Hardware faults"], answer:0, hint:"Real-world complexity." },
    { q:"40. The ultimate goal of AI engineering is to:", options:["Build intelligent systems","Replace electricity","Destroy computers","Stop automation"], answer:0, hint:"Creating smart systems." }
];

const form = document.getElementById("quizForm");

// Generate Quiz UI
quiz.forEach((q, index) => {
    let div = document.createElement("div");
    div.className = "question-card";

    let html = `<p class="question-text">${q.q}</p>`;

    q.options.forEach((opt, i) => {
        html += `
        <label class="option-label" id="label-q${index}-opt${i}">
            <input type="radio" name="q${index}" value="${i}">
            ${opt}
        </label>`;
    });

    // Added a feedback box directly under the hint
    html += `
        <button type="button" class="btn-hint" onclick="toggleHint(${index})">💡 Show Hint</button>
        <div id="hint${index}" class="hint-box">${q.hint}</div>
        <div id="feedback${index}" class="feedback-box"></div>
    `;

    div.innerHTML = html;
    form.appendChild(div);
});

function toggleHint(i) {
    let h = document.getElementById("hint" + i);
    let btn = event.target;
    if (h.style.display === "block") {
        h.style.display = "none";
        btn.innerText = "💡 Show Hint";
    } else {
        h.style.display = "block";
        btn.innerText = "Hide Hint";
    }
}

function submitQuiz() {
    let score = 0;
    
    quiz.forEach((q, i) => {
        let ans = document.querySelector(`input[name=q${i}]:checked`);
        let user = ans ? Number(ans.value) : -1;
        
        let feedbackBox = document.getElementById(`feedback${i}`);

        // Loop through all options to apply colors and disable inputs
        q.options.forEach((opt, optIndex) => {
            let label = document.getElementById(`label-q${i}-opt${optIndex}`);
            let input = label.querySelector('input');
            
            input.disabled = true;

            // Optional: You can keep or remove the card highlights now that you have text feedback
            if (optIndex === q.answer) {
                label.classList.add('correct-ans');
            } else if (optIndex === user && user !== q.answer) {
                label.classList.add('wrong-ans');
            }
        });

        // Set up the feedback text under the question
        feedbackBox.style.display = "block";

        if (user === q.answer) {
            score++;
            feedbackBox.innerHTML = `<strong>✅ Correct!</strong>`;
            feedbackBox.className = "feedback-box feedback-correct";
        } else if (user === -1) {
            feedbackBox.innerHTML = `<strong>⚠️ Skipped.</strong> The correct answer is: <em>${q.options[q.answer]}</em>`;
            feedbackBox.className = "feedback-box feedback-warning";
        } else {
            feedbackBox.innerHTML = `<strong>❌ Incorrect.</strong> The correct answer is: <em>${q.options[q.answer]}</em>`;
            feedbackBox.className = "feedback-box feedback-wrong";
        }
    });

    // Display Final Score Summary
    const resultBox = document.getElementById("result");
    resultBox.style.display = "block";
    
    let message = "";
    let percentage = (score / quiz.length) * 100;
    
    if(percentage >= 80) message = "Outstanding! You're an AI Expert! 🤖";
    else if(percentage >= 50) message = "Great job! Keep learning! 🧠";
    else message = "Good effort! Review your answers above. 📚";

    resultBox.innerHTML = `
        <h2>Total Score: ${score} / ${quiz.length}</h2>
        <p style="font-size: 1.2rem; margin-top: 15px;">${message}</p>
        <p style="color: #94a3b8; font-size: 0.9rem;">Scroll up to review your individual answers.</p>
    `;

    document.getElementById("submitBtn").disabled = true;
    document.getElementById("submitBtn").style.opacity = "0.5";
    document.getElementById("submitBtn").innerText = "Quiz Submitted";

    // Scroll back to the top so the user can review from question 1
    window.scrollTo({ top: 0, behavior: 'smooth' });
}
</script>

</body>
</html>
<button onclick="submitQuiz()">Submit Quiz</button>

<div id="result" class="result"></div>

<script>

const quiz = [
{
q:"1. What primarily characterizes human intelligence?",
options:["Massive data processing","Biological neural networks","Binary logic","Sensor arrays"],
answer:1,
hint:"Think about the biological system in the brain."
},

{
q:"2. Artificial Intelligence mainly relies on:",
options:["Emotion and intuition","Mathematical models","Biological neurons","Genetic coding"],
answer:1,
hint:"AI systems run using algorithms and math."
},

{
q:"3. Artificial Narrow Intelligence (ANI) refers to:",
options:["AI that can do everything","AI specialized for one task","Human intelligence","Robot consciousness"],
answer:1,
hint:"Today's AI systems are built for specific tasks."
},

{
q:"4. Artificial General Intelligence (AGI) would be:",
options:["Limited to one task","Human-level intelligence across domains","Simple automation","Spreadsheet algorithms"],
answer:1,
hint:"AGI is theoretical and would match human thinking."
},

{
q:"5. Computational intelligence is often described as:",
options:["Hard computing","Soft computing","Mechanical computing","Manual computing"],
answer:1,
hint:"It deals with uncertainty and real-world messiness."
},

{
q:"6. Fuzzy logic helps systems understand:",
options:["Binary values only","Continuous states","Only numbers","Computer memory"],
answer:1,
hint:"Example: slightly warm, somewhat fast."
},

{
q:"7. In robotics, cameras replace human:",
options:["Hands","Eyes","Memory","Voice"],
answer:1,
hint:"They allow machines to see."
},

{
q:"8. LiDAR sensors are used for:",
options:["Navigation and spatial mapping","Cooking automation","Audio recording","Weather prediction"],
answer:0,
hint:"Used heavily in self-driving vehicles."
},

{
q:"9. Predictive maintenance helps industries:",
options:["Repair machines after failure","Predict failures early","Ignore machine data","Reduce sensors"],
answer:1,
hint:"The goal is to detect problems before breakdown."
},

{
q:"10. Smart manufacturing uses AI mainly to:",
options:["Slow production","Optimize processes","Remove sensors","Increase waste"],
answer:1,
hint:"Efficiency is the main goal."
},

{
q:"11. One ethical concern of AI systems is:",
options:["Bias in data","Too much electricity","Slow computers","Screen brightness"],
answer:0,
hint:"Training data can cause unfair decisions."
},

{
q:"12. Data privacy in AI refers to:",
options:["Protecting collected data","Deleting sensors","Removing networks","Blocking algorithms"],
answer:0,
hint:"Large datasets must be secured."
},

{
q:"13. Propositional logic deals with:",
options:["Continuous values","True/False statements","Random numbers","Machine learning"],
answer:1,
hint:"Boolean logic."
},

{
q:"14. Boolean logic includes:",
options:["AND, OR, NOT","Multiply, divide","Add, subtract","Predict, classify"],
answer:0,
hint:"Basic digital logic gates."
},

{
q:"15. Predicate logic allows AI to:",
options:["Understand relationships","Only calculate numbers","Run hardware","Increase voltage"],
answer:0,
hint:"Uses variables and quantifiers."
},

{
q:"16. In logic systems 'AND' means:",
options:["Both conditions must be true","One must be true","None must be true","Random output"],
answer:0,
hint:"Both statements required."
},

{
q:"17. Autonomous navigation systems process:",
options:["Spatial data","Cooking data","Weather reports","Music files"],
answer:0,
hint:"Used in drones and robots."
},

{
q:"18. Microcontrollers in AI systems perform:",
options:["Decision-making tasks","Cooking operations","Printing operations","Internet browsing"],
answer:0,
hint:"They control embedded systems."
},

{
q:"19. Sensors in AI systems replace human:",
options:["Perception","Memory","Emotions","Sleep"],
answer:0,
hint:"Sensors detect environmental data."
},

{
q:"20. Machine learning mainly focuses on:",
options:["Learning patterns from data","Hardware manufacturing","Battery design","Manual programming"],
answer:0,
hint:"Algorithms learn from datasets."
},

{
q:"21. A dataset is:",
options:["Collection of data","Single value","Computer chip","Robot arm"],
answer:0,
hint:"Used to train AI."
},

{
q:"22. Pattern recognition helps AI:",
options:["Detect hidden relationships","Reduce electricity","Build sensors","Generate heat"],
answer:0,
hint:"Common in computer vision."
},

{
q:"23. AI in manufacturing helps:",
options:["Quality control","Increase defects","Slow systems","Remove machines"],
answer:0,
hint:"Cameras inspect products."
},

{
q:"24. Computer vision allows machines to:",
options:["See and interpret images","Smell objects","Cook food","Repair motors"],
answer:0,
hint:"Image processing."
},

{
q:"25. Industrial AI systems analyze:",
options:["Vibration data","Cooking recipes","Movie files","Paper notes"],
answer:0,
hint:"Used in predictive maintenance."
},

{
q:"26. A major advantage of AI is:",
options:["Processing massive datasets","Human emotions","Slow thinking","Limited memory"],
answer:0,
hint:"Computers process huge data quickly."
},

{
q:"27. AI decision systems depend heavily on:",
options:["Algorithms","Human mood","Paper reports","Mechanical gears"],
answer:0,
hint:"Step-by-step computational rules."
},

{
q:"28. Ethical AI requires:",
options:["Fair training data","Random outputs","Removing sensors","Slow computation"],
answer:0,
hint:"Bias must be avoided."
},

{
q:"29. Robotics combines:",
options:["AI and mechanical systems","Cooking and music","Medicine and law","Weather and farming"],
answer:0,
hint:"Hardware plus intelligence."
},

{
q:"30. Industrial automation focuses on:",
options:["Efficiency","Manual work","Reducing machines","Paper operations"],
answer:0,
hint:"Automation improves productivity."
},

{
q:"31. Machine learning models improve through:",
options:["Training","Sleeping","Charging","Cooling"],
answer:0,
hint:"Models learn from examples."
},

{
q:"32. Data used for training AI must be:",
options:["Relevant and clean","Random","Broken","Encrypted always"],
answer:0,
hint:"Quality data improves accuracy."
},

{
q:"33. AI safety ensures:",
options:["Systems behave predictably","Systems crash","Systems ignore humans","Systems delete data"],
answer:0,
hint:"Important in industrial robotics."
},

{
q:"34. Autonomous robots rely on:",
options:["Sensors and algorithms","Cooking instructions","Manual control","Human memory"],
answer:0,
hint:"Both perception and decision."
},

{
q:"35. Convergent technologies combine:",
options:["Multiple advanced technologies","Old mechanical tools","Manual labor","Paper systems"],
answer:0,
hint:"Integration of fields."
},

{
q:"36. Neural networks are inspired by:",
options:["Human brain","Electric motors","Mechanical gears","Solar panels"],
answer:0,
hint:"Biological neurons."
},

{
q:"37. AI used for detecting defects relies on:",
options:["Computer vision","Sound only","Temperature only","Manual inspection"],
answer:0,
hint:"Cameras inspect materials."
},

{
q:"38. Real-time data processing is important for:",
options:["Autonomous systems","Paper filing","Manual reports","Books"],
answer:0,
hint:"Robots must react instantly."
},

{
q:"39. Soft computing helps systems handle:",
options:["Uncertainty","Exact binary values","Only integers","Hardware faults"],
answer:0,
hint:"Real-world complexity."
},

{
q:"40. The ultimate goal of AI engineering is to:",
options:["Build intelligent systems","Replace electricity","Destroy computers","Stop automation"],
answer:0,
hint:"Creating smart systems."
}

];

const form = document.getElementById("quizForm");

quiz.forEach((q,index)=>{

let div=document.createElement("div");
div.className="question";

let html=`<p>${q.q}</p>`;

q.options.forEach((opt,i)=>{
html+=`<label>
<input type="radio" name="q${index}" value="${i}">
${opt}
</label><br>`;
});

html+=`<button type="button" onclick="toggleHint(${index})">Hint</button>
<div id="hint${index}" class="hint">${q.hint}</div>`;

div.innerHTML=html;
form.appendChild(div);

});

function toggleHint(i){
let h=document.getElementById("hint"+i);
h.style.display = h.style.display==="block"?"none":"block";
}

function submitQuiz(){

let score=0;
let output="";

quiz.forEach((q,i)=>{

let ans=document.querySelector(`input[name=q${i}]:checked`);
let user = ans ? Number(ans.value) : -1;

if(user===q.answer){
score++;
output+=`<p class="correct">Q${i+1}: Correct</p>`;
}else{
output+=`<p class="wrong">Q${i+1}: Wrong (Correct: ${q.options[q.answer]})</p>`;
}

});

document.getElementById("result").innerHTML =
`<h2>Your Score: ${score}/40</h2>` + output;

}

</script>

</body>
</html>
