# <!DOCTYPE html>
<html>
<head>
<title>GET307 AI Quiz</title>

<style>

body{
font-family:Arial;
background:#0f172a;
color:white;
padding:30px;
}

h1{
text-align:center;
}

.question{
background:#1e293b;
padding:20px;
margin:20px 0;
border-radius:10px;
}

button{
padding:8px 12px;
margin-top:10px;
border:none;
border-radius:5px;
cursor:pointer;
}

.hint{
display:none;
color:#facc15;
margin-top:10px;
}

.result{
margin-top:40px;
background:#020617;
padding:20px;
border-radius:10px;
}

.correct{
color:#22c55e;
}

.wrong{
color:#ef4444;
}

</style>

</head>

<body>

<h1>GET307 AI & Machine Learning Quiz</h1>

<form id="quizForm"></form>

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
