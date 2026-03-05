<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GET307 AI - University CBT Portal</title>
<style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');

    body {
        font-family: 'Inter', Arial, sans-serif;
        background-color: #f1f5f9;
        color: #1e293b;
        margin: 0;
        display: flex;
        flex-direction: column;
        height: 100vh;
        overflow: hidden;
    }

    header {
        background: linear-gradient(135deg, #2563eb, #7c3aed, #db2777);
        padding: 15px 30px;
        color: white;
        display: flex;
        justify-content: space-between;
        align-items: center;
        box-shadow: 0 4px 10px rgba(0,0,0,0.15);
        z-index: 10;
    }

    .exam-info h1 {
        margin: 0;
        font-size: 22px;
        font-weight: 700;
    }

    .exam-info p {
        margin: 5px 0 0 0;
        font-size: 14px;
        opacity: 0.9;
    }

    .timer-container {
        background: rgba(255, 255, 255, 0.2);
        padding: 10px 20px;
        border-radius: 8px;
        font-size: 24px;
        font-weight: bold;
        backdrop-filter: blur(5px);
        border: 1px solid rgba(255, 255, 255, 0.3);
    }

    .timer-warning {
        color: #fca5a5;
        animation: pulse 1s infinite;
    }

    @keyframes pulse {
        0% { opacity: 1; }
        50% { opacity: 0.5; }
        100% { opacity: 1; }
    }

    .container {
        display: flex;
        flex: 1;
        overflow: hidden;
    }

    .main {
        flex: 1;
        padding: 40px;
        overflow-y: auto;
        display: flex;
        flex-direction: column;
    }

    .question-card {
        background: white;
        padding: 30px;
        border-radius: 12px;
        box-shadow: 0 10px 25px rgba(0,0,0,0.05);
        margin-bottom: 20px;
        border-top: 5px solid #3b82f6;
    }

    .question-card h3 {
        margin-top: 0;
        color: #3b82f6;
        border-bottom: 2px solid #f1f5f9;
        padding-bottom: 10px;
    }

    .question-text {
        font-size: 18px;
        line-height: 1.6;
        margin-bottom: 25px;
    }

    .options label {
        display: block;
        background: #f8fafc;
        margin: 10px 0;
        padding: 15px;
        border-radius: 8px;
        cursor: pointer;
        border: 1px solid #e2e8f0;
        transition: all 0.2s;
        font-size: 16px;
    }

    .options label:hover {
        background: #eff6ff;
        border-color: #bfdbfe;
    }

    .options input {
        margin-right: 15px;
        transform: scale(1.2);
    }

    .hint-box {
        display: none;
        background: #fef9c3;
        border-left: 4px solid #eab308;
        padding: 15px;
        border-radius: 0 8px 8px 0;
        margin-bottom: 20px;
        color: #854d0e;
        font-weight: 600;
        animation: fadeIn 0.3s ease-in-out;
    }

    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(-5px); }
        to { opacity: 1; transform: translateY(0); }
    }

    .controls {
        display: flex;
        gap: 15px;
        margin-top: auto;
        flex-wrap: wrap;
    }

    button {
        padding: 12px 24px;
        border: none;
        border-radius: 8px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
        transition: transform 0.1s, filter 0.2s;
    }

    button:active { transform: scale(0.98); }
    button:hover { filter: brightness(1.1); }

    .prev { background: #64748b; color: white; }
    .next { background: #3b82f6; color: white; }
    .hintbtn { background: #06b6d4; color: white; margin-left: auto; }
    .flagbtn { background: #f59e0b; color: white; }
    .submit { background: #10b981; color: white; }

    .sidebar {
        width: 300px;
        background: white;
        padding: 20px;
        border-left: 1px solid #e2e8f0;
        display: flex;
        flex-direction: column;
        box-shadow: -4px 0 15px rgba(0,0,0,0.02);
    }

    .legend {
        display: flex;
        flex-wrap: wrap;
        gap: 10px;
        margin-bottom: 20px;
        font-size: 12px;
        padding-bottom: 15px;
        border-bottom: 1px solid #e2e8f0;
    }

    .legend-item { display: flex; align-items: center; gap: 5px; }
    .legend-box { width: 15px; height: 15px; border-radius: 3px; }

    .palette {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        gap: 10px;
        overflow-y: auto;
        padding-right: 5px;
    }

    .palette button {
        width: 100%;
        aspect-ratio: 1;
        padding: 0;
        font-size: 14px;
        border-radius: 6px;
        background: #e2e8f0;
        color: #334155;
    }

    .palette button.answered { background: #10b981; color: white; }
    .palette button.flagged { background: #f59e0b; color: white; }
    .palette button.active { border: 3px solid #3b82f6; transform: scale(1.05); }

    .result-screen {
        text-align: center;
        padding: 50px;
    }

    .result-screen h2 { font-size: 32px; color: #1e293b; }
    .score-circle {
        width: 150px;
        height: 150px;
        background: linear-gradient(135deg, #10b981, #059669);
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
        font-size: 48px;
        font-weight: bold;
        margin: 30px auto;
        box-shadow: 0 10px 20px rgba(16, 185, 129, 0.3);
    }
</style>
</head>

<body>

<header>
    <div class="exam-info">
        <h1>GET307: Artificial Intelligence</h1>
        <p>Candidate ID: 2026/XYZ-001 | Practice Mode CBT</p>
    </div>
    <div class="timer-container" id="timer">30:00</div>
</header>

<div class="container">
    <div class="main" id="mainArea">
        <div id="questionBox" class="question-card"></div>
        <div id="hintBox" class="hint-box"></div>
        
        <div class="controls" id="controlPanel">
            <button class="prev" onclick="prevQ()">Previous</button>
            <button class="next" onclick="nextQ()">Next</button>
            <button class="hintbtn" onclick="showHint()">💡 Show Hint</button>
            <button class="flagbtn" onclick="toggleFlag()" id="flagBtn">Flag for Review</button>
            <button class="submit" onclick="confirmSubmit()">Submit Exam</button>
        </div>
    </div>

    <div class="sidebar">
        <div class="legend">
            <div class="legend-item"><div class="legend-box" style="background:#e2e8f0;"></div> Unanswered</div>
            <div class="legend-item"><div class="legend-box" style="background:#10b981;"></div> Answered</div>
            <div class="legend-item"><div class="legend-box" style="background:#f59e0b;"></div> Flagged</div>
        </div>
        <h3>Question Navigator</h3>
        <div class="palette" id="palette"></div>
    </div>
</div>

<script>
const questions = [
    {
        q: "Which statement best distinguishes Artificial Intelligence from traditional software?",
        options: [
            "AI systems can learn patterns from data rather than relying only on explicit instructions",
            "AI programs use faster processors",
            "AI replaces mathematics with intuition",
            "AI systems eliminate algorithms"
        ],
        a: 0,
        hint: "Think about learning vs. fixed programming."
    },
    {
        q: "In computational intelligence, fuzzy logic is preferred over classical Boolean logic when:",
        options: [
            "system variables exist on a spectrum rather than binary states",
            "computers operate faster",
            "memory is limited",
            "sensors are unavailable"
        ],
        a: 0,
        hint: "Think about 'partial truth' instead of just absolute true/false."
    },
    {
        q: "Scenario: A robot arm adjusts its grip strength depending on the softness of an object detected by pressure sensors. Which AI concept is being applied?",
        options: [
            "Adaptive control",
            "Static programming",
            "Manual calibration",
            "Deterministic automation"
        ],
        a: 0,
        hint: "The robot dynamically changes behavior based on sensory feedback."
    },
    {
        q: "Which ethical concern arises when an AI system trained on biased hiring data recommends candidates?",
        options: [
            "Algorithmic bias",
            "Hardware failure",
            "Sensor noise",
            "Memory overflow"
        ],
        a: 0,
        hint: "This is a fairness issue related to the data it was trained on."
    },
    {
        q: "Scenario: A drone navigating terrain uses LiDAR to construct a 3D map of obstacles before planning its path. Which capability is primarily demonstrated?",
        options: [
            "Environmental perception",
            "Human emotional modeling",
            "Hardware redundancy",
            "Data encryption"
        ],
        a: 0,
        hint: "Think about spatial awareness and understanding surroundings."
    }
];

// Generate remaining questions
for(let i=6; i<=40; i++){
    questions.push({
        q: `Question ${i}: Which capability allows modern AI systems to identify complex relationships within large datasets that are difficult for humans to detect?`,
        options: [
            "Machine learning pattern discovery",
            "Manual coding",
            "Mechanical automation",
            "Random search"
        ],
        a: 0,
        hint: "It involves extracting hidden knowledge and learning directly from data."
    });
}

let current = 0;
let answers = new Array(questions.length).fill(null);
let flags = new Array(questions.length).fill(false);
let examSubmitted = false;

const palette = document.getElementById("palette");

// Initialize Palette
for(let i=0; i<questions.length; i++){
    let b = document.createElement("button");
    b.innerText = i + 1;
    b.onclick = () => goQ(i);
    palette.appendChild(b);
}

function updatePalette() {
    for(let i=0; i<questions.length; i++){
        let btn = palette.children[i];
        btn.className = ""; // reset classes
        if (i === current) btn.classList.add("active");
        if (flags[i]) btn.classList.add("flagged");
        else if (answers[i] !== null) btn.classList.add("answered");
    }
}

function loadQ(){
    let q = questions[current];
    let html = `<h3>Question ${current + 1} of ${questions.length}</h3>
                <div class="question-text">${q.q}</div>
                <div class="options">`;

    q.options.forEach((opt, i) => {
        let checked = answers[current] === i ? "checked" : "";
        html += `<label>
                    <input type="radio" name="opt" value="${i}" ${checked} onclick="saveAns(${i})"> 
                    ${opt}
                 </label>`;
    });

    html += `</div>`;
    document.getElementById("questionBox").innerHTML = html;
    
    // Hide hint box when loading a new question
    document.getElementById("hintBox").style.display = "none";

    // Update Flag button text
    document.getElementById("flagBtn").innerText = flags[current] ? "Unflag Question" : "Flag for Review";
    updatePalette();
}

function saveAns(v){
    answers[current] = v;
    updatePalette();
}

function showHint() {
    let hintBox = document.getElementById("hintBox");
    hintBox.innerText = questions[current].hint;
    hintBox.style.display = "block";
}

function nextQ(){ if(current < questions.length - 1){ current++; loadQ(); } }
function prevQ(){ if(current > 0){ current--; loadQ(); } }
function goQ(i){ current = i; loadQ(); }

function toggleFlag() {
    flags[current] = !flags[current];
    loadQ(); 
}

function confirmSubmit() {
    let unattempted = answers.filter(a => a === null).length;
    let msg = unattempted > 0 
        ? `You have ${unattempted} unattempted questions. Are you sure you want to submit?`
        : `Are you sure you want to submit your exam?`;
        
    if(confirm(msg)) {
        submitExam();
    }
}

function submitExam(){
    examSubmitted = true;
    let score = 0;

    answers.forEach((ans, i) => {
        if(ans === questions[i].a) score++;
    });

    let percentage = Math.round((score / questions.length) * 100);
    let grade = percentage >= 70 ? 'A' : percentage >= 60 ? 'B' : percentage >= 50 ? 'C' : percentage >= 45 ? 'D' : 'F';

    let resultHtml = `
        <div class="result-screen">
            <h2>Practice Exam Submitted</h2>
            <p>Great job completing the GET307 AI CBT.</p>
            <div class="score-circle">${percentage}%</div>
            <h3>Raw Score: ${score} / ${questions.length}</h3>
            <h3 style="color: #3b82f6;">Final Grade: ${grade}</h3>
        </div>
    `;

    document.getElementById("mainArea").innerHTML = resultHtml;
    document.getElementById("timer").innerHTML = "EXAM ENDED";
    document.getElementById("timer").classList.remove("timer-warning");
}

loadQ();

// Timer Logic
let time = 1800; // 30 minutes
let timerInterval = setInterval(() => {
    if(examSubmitted) {
        clearInterval(timerInterval);
        return;
    }

    let m = Math.floor(time / 60);
    let s = time % 60;
    let timerEl = document.getElementById("timer");

    timerEl.innerText = m + ":" + (s < 10 ? "0" + s : s);

    if (time <= 300) {
        timerEl.classList.add("timer-warning");
    }

    time--;

    if(time <= 0) {
        clearInterval(timerInterval);
        alert("Time is up! Your exam will now be submitted automatically.");
        submitExam();
    }
}, 1000);

</script>
</body>
</html>
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
