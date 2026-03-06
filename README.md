<!DOCTYPE html>
<html>
<head>
<title>GET307 AI CBT Exam - Advanced</title>

<style>

body{
font-family:Arial;
margin:0;
background:linear-gradient(120deg,#0f172a,#1e293b);
color:white;
}

header{
background:#020617;
padding:15px;
text-align:center;
font-size:22px;
}

.timer{
float:right;
color:#38bdf8;
}

.container{
display:flex;
}

.palette{
width:220px;
background:#020617;
padding:10px;
height:100vh;
overflow:auto;
transition: all 0.3s ease;
}

.palette button{
width:40px;
height:40px;
margin:4px;
border:none;
border-radius:6px;
background:#334155;
color:white;
cursor:pointer;
}

.palette button.answered{
background:#22c55e;
}

.main{
flex:1;
padding:30px;
overflow-y:auto;
height: calc(100vh - 60px); /* Account for header */
box-sizing: border-box;
}

.question{
background:#1e293b;
padding:20px;
border-radius:10px;
}

.options label{
display:block;
margin:10px 0;
cursor:pointer;
}

.controls{
margin-top:20px;
}

button{
padding:8px 14px;
border:none;
border-radius:6px;
cursor:pointer;
}

.next{background:#3b82f6;color:white;}
.prev{background:#64748b;color:white;}
.hintbtn{background:#eab308;color:black;}
.submit{background:#22c55e;color:white;}

.hint{
display:none;
color:#facc15;
margin-top:10px;
}

/* Review Screen Styles */
.review-card {
background:#0f172a; 
padding:15px; 
margin-bottom:15px; 
border-left:4px solid #ef4444; 
border-radius:6px;
}
.review-correct {
color:#22c55e; 
margin:5px 0;
}
.review-wrong {
color:#ef4444; 
margin:5px 0;
}
.review-explanation {
color:#facc15; 
margin-top:10px; 
font-size:14px;
}

</style>
</head>

<body>

<header>
GET307 Artificial Intelligence CBT (Advanced Level)
<span class="timer" id="timer">30:00</span>
</header>

<div class="container">

<div class="palette" id="palette"></div>

<div class="main" id="mainArea">

<div id="questionBox"></div>

<div class="controls">
<button class="prev" onclick="prevQ()">Previous</button>
<button class="next" onclick="nextQ()">Next</button>
<button class="hintbtn" onclick="showHint()">Hint</button>
<button class="submit" onclick="confirmSubmit()">Submit</button>
</div>

<div id="hint" class="hint"></div>

</div>
</div>

<script>

const questions=[
{q:`For the A* search algorithm to guarantee an optimal solution, the heuristic function h(n) must be admissible. What does this mean?`,options:[`h(n) must accurately predict the exact cost to the goal.`,`h(n) must never overestimate the actual cost to reach the goal.`,`h(n) must always be greater than the step cost g(n).`,`h(n) must equal the total path cost f(n).`],a:1,hint:`Think about avoiding overly pessimistic estimates.`},
{q:`In the worst-case scenario, what is the time complexity of the Minimax algorithm with Alpha-Beta pruning applied to a game tree with branching factor 'b' and depth 'd'?`,options:[`O(b^(d/2))`,`O(b * d)`,`O(b^d)`,`O(d^b)`],a:2,hint:`Alpha-Beta pruning improves the best/average case, but what happens if the nodes are ordered in the worst possible way?`},
{q:`Which mathematical operation allows Support Vector Machines (SVMs) to classify non-linearly separable data without explicitly mapping features to a higher-dimensional space?`,options:[`The Sigmoid Activation`,`The Kernel Trick`,`Gradient Descent`,`Principal Component Analysis`],a:1,hint:`It computes the dot product in the higher-dimensional space directly.`},
{q:`In an artificial neural network, what is the primary consequence of using a linear activation function in all hidden layers?`,options:[`The network will suffer from the exploding gradient problem.`,`The network collapses into an equivalent single-layer perceptron.`,`The model will overfit the training data almost immediately.`,`The forward pass will output binary values only.`],a:1,hint:`Linear combinations of linear combinations are still linear.`},
{q:`When applying L1 Regularization (Lasso) to a Machine Learning model, what happens to the weight vectors during optimization?`,options:[`All weights are scaled down uniformly but never reach zero.`,`The weights tend to become sparse, with many weights driven exactly to zero.`,`The weights increase infinitely if the learning rate is too high.`,`The weights map directly to the eigenvectors of the dataset.`],a:1,hint:`This regularization acts as a built-in feature selection method.`},
{q:`In Natural Language Processing, the self-attention mechanism in a Transformer model computes a weighted sum of values. How are these weights determined?`,options:[`By multiplying the Query (Q) and Key (K) matrices and applying a softmax function.`,`By using recurrent hidden states from the previous timestep.`,`By applying Convolutional pooling over the word embeddings.`,`By using the backpropagation error from the decoder.`],a:0,hint:`Look for the Q and K interaction.`},
{q:`What does the Bellman Optimality Equation compute in a Markov Decision Process (MDP)?`,options:[`The probability of transitioning from state s to state s'.`,`The maximum expected utility of a state assuming optimal future actions.`,`The loss function of a Deep Q-Network.`,`The probability of selecting a random action in epsilon-greedy exploration.`],a:1,hint:`It looks for the 'utility' or 'value' of making the best choice.`},
{q:`The 'Vanishing Gradient' problem in Deep Learning is most frequently caused by:`,options:[`Using ReLU activation functions in deep networks.`,`Using large batch sizes during stochastic gradient descent.`,`Repeated multiplication of derivatives less than 1 when using Sigmoid/Tanh activations.`,`A learning rate that is set too high.`],a:2,hint:`Think about the chain rule and multiplying decimals.`},
{q:`In First-Order Logic (FOL), 'Skolemization' is a process used to:`,options:[`Convert Modus Ponens into Modus Tollens.`,`Remove existential quantifiers by replacing their variables with Skolem constants or functions.`,`Prove that a set of clauses is logically valid without inference.`,`Convert universal quantifiers into propositional logic symbols.`],a:1,hint:`It helps standardize formulas into Conjunctive Normal Form (CNF).`},
{q:`Which of the following describes the 'Curse of Dimensionality' in algorithms like K-Nearest Neighbors (KNN)?`,options:[`As dimensions increase, the dataset requires exponentially more memory.`,`As dimensions increase, the distance between any two points converges, making 'nearest' meaningless.`,`As dimensions increase, the model forces the learning rate to drop to zero.`,`As dimensions increase, K must be reduced to 1.`],a:1,hint:`Think about how distance metrics behave in a 10,000-dimension space.`},
{q:`Which statistical technique is primarily used to project high-dimensional data onto a lower-dimensional subspace while maximizing variance?`,options:[`Linear Discriminant Analysis (LDA)`,`Principal Component Analysis (PCA)`,`Singular Value Decomposition (SVD)`,`K-Means Clustering`],a:1,hint:`It relies on the eigenvectors of the data's covariance matrix.`},
{q:`In a Hidden Markov Model (HMM), which algorithm is used to find the most likely sequence of hidden states given a sequence of observations?`,options:[`The Forward Algorithm`,`The Viterbi Algorithm`,`The Baum-Welch Algorithm`,`The Metropolis-Hastings Algorithm`],a:1,hint:`It's a dynamic programming approach for decoding.`},
{q:`What is the mathematical objective of a Generative Adversarial Network (GAN)?`,options:[`Minimizing the Mean Squared Error of the generator.`,`Maximizing the log-likelihood of the discriminator.`,`Finding the Nash Equilibrium in a two-player minimax game.`,`Minimizing the KL divergence between two identical distributions.`],a:2,hint:`Think of game theory between the Generator and Discriminator.`},
{q:`In Decision Trees, the 'Information Gain' used to select the best splitting attribute is calculated based on which concept from information theory?`,options:[`Cross-Entropy`,`Gini Impurity`,`Shannon Entropy`,`Kullback-Leibler Divergence`],a:2,hint:`It measures the expected reduction in uncertainty.`},
{q:`Which component of a Convolutional Neural Network (CNN) is responsible for providing spatial invariance to small translations in the input image?`,options:[`The Fully Connected Layer`,`The Activation Function`,`The Pooling (Subsampling) Layer`,`The Flattening Layer`],a:2,hint:`This layer reduces the resolution of the feature maps.`},
{q:`What does 'Experience Replay' solve in Deep Q-Networks (DQN)?`,options:[`It speeds up the computation time of the Bellman equation.`,`It breaks the temporal correlation between consecutive training samples, stabilizing the neural network.`,`It prevents the epsilon-greedy algorithm from exploring too much.`,`It replaces the target network with a frozen actor-critic architecture.`],a:1,hint:`Sequential frames in a game are highly correlated; training on them directly causes instability.`},
{q:`In the context of the Bias-Variance Tradeoff, an overly complex model (like an unpruned decision tree) will typically exhibit:`,options:[`High Bias, High Variance`,`Low Bias, High Variance`,`High Bias, Low Variance`,`Low Bias, Low Variance`],a:1,hint:`It learns the training data perfectly but fails on unseen data.`},
{q:`Which ensemble learning method trains multiple weak learners sequentially, where each new learner focuses on the errors made by the previous ones?`,options:[`Bagging (Bootstrap Aggregating)`,`Random Forests`,`Gradient Boosting`,`K-Fold Cross Validation`],a:2,hint:`It 'boosts' performance by correcting past mistakes.`},
{q:`In Propositional Logic, the Resolution inference rule states that if we have (A OR B) and (NOT B OR C), we can infer:`,options:[`A OR C`,`A AND C`,`NOT A OR NOT C`,`B OR C`],a:0,hint:`The conflicting literal (B) is resolved/cancelled out.`},
{q:`Which heuristic function is strictly dominant (always expands equal or fewer nodes) for the 8-puzzle problem?`,options:[`Number of misplaced tiles.`,`Sum of Manhattan distances of tiles from their goal positions.`,`Euclidean distance of the empty space.`,`Breadth-First Search cost.`],a:1,hint:`Which one gives a higher, yet still admissible, estimate?`},
{q:`What is the primary role of the 'Forget Gate' in a Long Short-Term Memory (LSTM) network?`,options:[`To decide what new information should be added to the cell state.`,`To scale the output activation before passing it to the next layer.`,`To decide what proportion of the previous cell state should be discarded.`,`To prevent the vanishing gradient by skipping backward passes.`],a:2,hint:`It uses a sigmoid layer to output a number between 0 and 1 for the old cell state.`},
{q:`In constraint satisfaction problems (CSPs), the AC-3 algorithm is used to enforce:`,options:[`Node Consistency`,`Arc Consistency`,`Path Consistency`,`K-Consistency`],a:1,hint:`It ensures every value in a variable's domain satisfies the binary constraints.`},
{q:`What is 'Dropout' in the context of training deep neural networks?`,options:[`Removing outliers from the training dataset.`,`Randomly setting a fraction of input units to 0 at each update during training to prevent overfitting.`,`Stopping the training process early when validation loss increases.`,`Discarding gradients that exceed a certain threshold.`],a:1,hint:`It forces the network to learn robust features without relying on specific neurons.`},
{q:`In unsupervised learning, what does the K-Means clustering algorithm actually guarantee?`,options:[`Finding the global optimal clustering.`,`Convergence to a local minimum of the within-cluster sum of squares.`,`Automatic selection of the optimal number of clusters (K).`,`Equal distribution of data points across all clusters.`],a:1,hint:`It depends heavily on the initial random placement of centroids.`},
{q:`Which evaluation metric is most appropriate for an imbalanced classification problem where false negatives are highly critical (e.g., medical diagnosis)?`,options:[`Accuracy`,`Precision`,`Recall (Sensitivity)`,`F1-Score`],a:2,hint:`You want to capture as many true positive cases as possible, even if it means some false alarms.`},
{q:`In logic, a knowledge base KB entails a sentence alpha (KB ⊨ α) if and only if:`,options:[`Alpha is true in at least one model where KB is true.`,`Alpha is true in all models where KB is true.`,`KB can be derived from Alpha using inference rules.`,`Alpha and KB contain the same variables.`],a:1,hint:`Think of entailment as a strict guarantee across all possible worlds.`},
{q:`What is the primary difference between Q-Learning and SARSA in Reinforcement Learning?`,options:[`Q-Learning requires a known transition model, SARSA is model-free.`,`Q-Learning is an off-policy algorithm, SARSA is an on-policy algorithm.`,`SARSA updates values using the Bellman Optimality Equation, Q-Learning does not.`,`Q-Learning is used for continuous action spaces, SARSA for discrete.`],a:1,hint:`One assumes the greedy action for the update, while the other uses the action actually taken by the policy.`},
{q:`Which property of Fuzzy Logic operations ensures that the intersection of two fuzzy sets is determined by the minimum of their membership values?`,options:[`S-Norm (T-conorm)`,`T-Norm`,`Defuzzification`,`Linguistic Hedges`],a:1,hint:`It generalizes the logical AND operator.`},
{q:`In a Bayesian Network, two variables are considered 'conditionally independent' given a third variable if:`,options:[`They are connected by a direct directed edge.`,`The path between them is d-separated by the third variable.`,`They share the same prior probability.`,`They are both root nodes in the graph.`],a:1,hint:`Look up the concept of 'd-separation'.`},
{q:`What does the 'Word2Vec Skip-Gram' model attempt to predict during training?`,options:[`The target word given its surrounding context words.`,`The surrounding context words given a single target word.`,`The next character in a sequence.`,`The syntactic parse tree of a sentence.`],a:1,hint:`It 'skips' outward from the center.`},
{q:`In the context of optimization algorithms, what is the purpose of adding 'Momentum' to Gradient Descent?`,options:[`To ensure the learning rate decreases over time.`,`To compute the second derivative (Hessian matrix) efficiently.`,`To accelerate gradients in the right direction and dampen oscillations.`,`To randomly reset weights when stuck in a local minimum.`],a:2,hint:`Think of a ball rolling down a hill, gaining speed.`},
{q:`The concept of 'Gradient Clipping' is most commonly used to solve which problem in Recurrent Neural Networks?`,options:[`Overfitting`,`The Vanishing Gradient Problem`,`The Exploding Gradient Problem`,`Catastrophic Forgetting`],a:2,hint:`It caps the derivatives to a maximum threshold.`},
{q:`Which of the following is true about the Naive Bayes classifier?`,options:[`It assumes that all features are highly correlated.`,`It assumes that features are conditionally independent given the class label.`,`It calculates the exact joint probability distribution of the dataset.`,`It requires a differentiable loss function to train.`],a:1,hint:`This 'naive' assumption makes the math much simpler to compute.`},
{q:`In AI planning, a STRIPS operator is defined by three components:`,options:[`States, Actions, Rewards`,`Preconditions, Add List, Delete List`,`Heuristics, Costs, Goals`,`Nodes, Edges, Weights`],a:1,hint:`What must be true before the action, what becomes true, and what becomes false?`},
{q:`Batch Normalization in Deep Learning provides which primary benefit?`,options:[`It replaces the need for activation functions.`,`It reduces internal covariate shift, allowing for higher learning rates.`,`It converts all weights into binary values to save memory.`,`It eliminates the need for a loss function.`],a:1,hint:`It standardizes the inputs to a layer for each mini-batch.`},
{q:`In an Autoencoder neural network, the 'bottleneck' hidden layer is crucial because:`,options:[`It prevents the network from simply learning the identity function by copying input to output.`,`It increases the dimensionality of the data for better classification.`,`It stores the labels for supervised fine-tuning.`,`It computes the gradient descent update explicitly.`],a:0,hint:`It forces the network to learn a compressed representation.`},
{q:`In genetic algorithms, what is the primary purpose of the 'Mutation' operator?`,options:[`To combine the best traits of two parents.`,`To select the fittest individuals for the next generation.`,`To maintain genetic diversity and prevent premature convergence to a local optimum.`,`To calculate the fitness score of the population.`],a:2,hint:`It introduces random, unpredictable changes.`},
{q:`Which formulation is used to update the posterior probability of a hypothesis as new evidence becomes available?`,options:[`Markov Property`,`Bellman Equation`,`Bayes' Theorem`,`Laplace Smoothing`],a:2,hint:`P(A|B) = [P(B|A) * P(A)] / P(B)`},
{q:`In Natural Language Processing, the BLEU score is primarily used to evaluate:`,options:[`The sentiment of a given text.`,`The quality of text generated by machine translation systems against human reference translations.`,`The grammatical correctness of a syntax tree.`,`The cosine similarity between two word embeddings.`],a:1,hint:`It relies on n-gram precision.`},
{q:`What distinguishes a 'Zero-Shot Learning' model from traditional supervised learning models?`,options:[`It is trained entirely on unlabelled data using clustering.`,`It can classify data instances from classes it has never explicitly seen during training.`,`It uses zero hidden layers in its architecture.`,`It requires zero computational power during inference.`],a:1,hint:`It uses auxiliary information like semantic attributes to recognize new things.`}
];

let current=0;
let answers=new Array(questions.length).fill(null);

const palette=document.getElementById("palette");

for(let i=0;i<questions.length;i++){
let b=document.createElement("button");
b.innerText=i+1;
b.onclick=()=>goQ(i);
palette.appendChild(b);
}

function loadQ(){
let q=questions[current];
let html=`<div class="question"><h3>Question ${current+1}</h3><p>${q.q}</p><div class="options">`;

q.options.forEach((opt,i)=>{
let checked=answers[current]==i?"checked":"";
html+=`<label><input type="radio" name="opt" value="${i}" ${checked} onclick="saveAns(${i})"> ${opt}</label>`;
});

html+=`</div></div>`;
document.getElementById("questionBox").innerHTML=html;
document.getElementById("hint").style.display="none";
}

function saveAns(v){
answers[current]=v;
palette.children[current].classList.add("answered");
}

function nextQ(){if(current<questions.length-1){current++;loadQ();}}
function prevQ(){if(current>0){current--;loadQ();}}
function goQ(i){current=i;loadQ();}

function showHint(){
document.getElementById("hint").innerText=questions[current].hint;
document.getElementById("hint").style.display="block";
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

// Global variable for the timer so we can stop it later
let timerInterval;

function submitExam(){
    // Stop the timer
    clearInterval(timerInterval);
    document.getElementById("timer").innerText = "EXAM OVER";
    
    // Hide the sidebar palette to give the review screen full width
    document.getElementById("palette").style.display = "none";
    
    let score = 0;
    answers.forEach((ans,i)=>{if(ans===questions[i].a)score++;});
    
    let percentage = ((score / questions.length) * 100).toFixed(1);
    
    // Build the results UI
    let resultHTML = `<div class="question">`;
    resultHTML += `<h2>Exam Completed!</h2>`;
    resultHTML += `<h3>Your Score: ${score} / ${questions.length} (${percentage}%)</h3>`;
    resultHTML += `<hr style="border-color:#334155; margin:20px 0;">`;
    
    let wrongCount = 0;
    
    // Loop through to find incorrect answers
    questions.forEach((q, i) => {
        if (answers[i] !== q.a) {
            wrongCount++;
            let userAnswerText = answers[i] !== null ? q.options[answers[i]] : "Not Answered";
            let correctAnswerText = q.options[q.a];
            
            resultHTML += `
            <div class="review-card">
                <p><strong>Q${i+1}: ${q.q}</strong></p>
                <p class="review-wrong">❌ Your Answer: ${userAnswerText}</p>
                <p class="review-correct">✅ Correct Answer: ${correctAnswerText}</p>
                <p class="review-explanation">💡 <em>Explanation: ${q.hint}</em></p>
            </div>`;
        }
    });
    
    if (wrongCount === 0) {
        resultHTML += `<p class="review-correct">Outstanding! You scored a perfect 100%. No incorrect answers to review.</p>`;
    } else {
        resultHTML = `<h3>Review of Incorrect Answers (${wrongCount}):</h3>` + resultHTML;
    }
    
    resultHTML += `</div>`;
    
    // Inject the results into the main container
    document.getElementById("mainArea").innerHTML = resultHTML;
}

loadQ();

let time=1800; // 30 minutes in seconds
timerInterval = setInterval(()=>{
let m=Math.floor(time/60);
let s=time%60;
document.getElementById("timer").innerText=m+":"+(s<10?"0"+s:s);
time--;
if(time<=0) submitExam(); // Auto submit if time runs out
},1000);

</script>

</body>
</html>
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
