# Learning-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PhysicsFlash | AI-Powered Physics Learning</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .card-flip {
            transition: transform 0.6s;
            transform-style: preserve-3d;
        }
        .card-flip.flipped {
            transform: rotateY(180deg);
        }
        .card-front, .card-back {
            backface-visibility: hidden;
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        .card-back {
            transform: rotateY(180deg);
        }
        .progress-bar {
            transition: width 0.5s ease-in-out;
        }
        .fade-in {
            animation: fadeIn 0.5s;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        .physics-gradient {
            background: linear-gradient(135deg, #3b82f6 0%, #8b5cf6 50%, #ec4899 100%);
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen font-sans">
    <!-- Navigation -->
    <nav class="physics-gradient text-white shadow-lg">
        <div class="container mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center space-x-2">
                <i class="fas fa-atom text-2xl"></i>
                <span class="text-xl font-bold">PhysicsFlash</span>
            </div>
            <div class="hidden md:flex space-x-6">
                <a href="#" class="hover:text-gray-200 transition">Home</a>
                <a href="#" class="hover:text-gray-200 transition">Courses</a>
                <a href="#" class="hover:text-gray-200 transition">Progress</a>
                <a href="#" class="hover:text-gray-200 transition">About</a>
            </div>
            <div class="md:hidden">
                <button id="mobile-menu-button" class="text-white focus:outline-none">
                    <i class="fas fa-bars text-2xl"></i>
                </button>
            </div>
        </div>
        <div id="mobile-menu" class="hidden md:hidden px-4 pb-3">
            <a href="#" class="block py-2 hover:text-gray-200 transition">Home</a>
            <a href="#" class="block py-2 hover:text-gray-200 transition">Courses</a>
            <a href="#" class="block py-2 hover:text-gray-2 00 transition">Progress</a>
            <a href="#" class="block py-2 hover:text-gray-200 transition">About</a>
        </div>
    </nav>

    <!-- Main Content -->
    <main class="container mx-auto px-4 py-8">
        <!-- Hero Section -->
        <section class="text-center mb-12 fade-in">
            <h1 class="text-4xl md:text-5xl font-bold text-gray-800 mb-4">Master Physics with AI-Powered Flashcards</h1>
            <p class="text-xl text-gray-600 max-w-3xl mx-auto mb-8">Our intelligent system assesses your knowledge and creates a personalized learning path from basic concepts to advanced theories.</p>
            <div class="flex flex-col sm:flex-row justify-center gap-4">
                <button id="start-assessment" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-3 px-6 rounded-lg transition transform hover:scale-105">
                    Start Knowledge Assessment
                </button>
                <button class="border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-50 font-bold py-3 px-6 rounded-lg transition transform hover:scale-105">
                    Browse Topics
                </button>
            </div>
        </section>

        <!-- Assessment Modal -->
        <div id="assessment-modal" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
            <div class="bg-white rounded-lg shadow-xl max-w-2xl w-full max-h-[90vh] overflow-y-auto">
                <div class="p-6">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="text-2xl font-bold text-gray-800">Knowledge Assessment</h2>
                        <button id="close-assessment" class="text-gray-500 hover:text-gray-700">
                            <i class="fas fa-times text-xl"></i>
                        </button>
                    </div>
                    <div class="mb-6">
                        <div class="flex justify-between mb-1">
                            <span class="text-sm font-medium text-gray-700">Progress</span>
                            <span id="progress-percent" class="text-sm font-medium text-gray-700">0%</span>
                        </div>
                        <div class="w-full bg-gray-200 rounded-full h-2.5">
                            <div id="progress-bar" class="bg-indigo-600 h-2.5 rounded-full progress-bar" style="width: 0%"></div>
                        </div>
                    </div>
                    <div id="assessment-content" class="space-y-6">
                        <div class="text-center py-8">
                            <i class="fas fa-brain text-5xl text-indigo-500 mb-4"></i>
                            <h3 class="text-xl font-semibold text-gray-800 mb-2">Let's assess your physics knowledge</h3>
                            <p class="text-gray-600 mb-6">We'll ask you a series of questions to determine your current understanding. Answer honestly - there's no penalty for "I don't know"!</p>
                            <button id="start-test" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-2 px-6 rounded-lg transition">
                                Begin Test
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Flashcard Section -->
        <section id="flashcard-section" class="hidden mb-12 fade-in">
            <div class="max-w-3xl mx-auto">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-2xl font-bold text-gray-800">Your Learning Path</h2>
                    <div class="flex items-center space-x-2">
                        <span id="current-card" class="font-medium text-gray-700">1/10</span>
                        <span class="text-gray-400">|</span>
                        <span id="difficulty-level" class="px-2 py-1 bg-indigo-100 text-indigo-800 text-xs font-semibold rounded">Beginner</span>
                    </div>
                </div>
                
                <div class="relative h-96 mb-8">
                    <!-- Flashcard -->
                    <div id="flashcard" class="card-flip w-full h-full bg-white rounded-xl shadow-lg cursor-pointer transition-all duration-300 hover:shadow-xl">
                        <!-- Front of Card -->
                        <div class="card-front flex flex-col p-8">
                            <div class="flex-1 flex flex-col justify-center items-center">
                                <div class="text-center">
                                    <span class="inline-block px-3 py-1 bg-indigo-100 text-indigo-800 rounded-full text-sm font-medium mb-4">Question</span>
                                    <h3 id="question-text" class="text-2xl font-bold text-gray-800 mb-4">What is Newton's First Law of Motion?</h3>
                                    <p class="text-gray-600">Click to reveal answer</p>
                                </div>
                            </div>
                            <div class="flex justify-center space-x-4">
                                <button class="flashcard-action bg-red-100 hover:bg-red-200 text-red-700 px-4 py-2 rounded-lg transition" data-action="dont-know">
                                    <i class="fas fa-question-circle mr-2"></i>Don't Know
                                </button>
                                <button class="flashcard-action bg-yellow-100 hover:bg-yellow-200 text-yellow-700 px-4 py-2 rounded-lg transition" data-action="learn-later">
                                    <i class="fas fa-clock mr-2"></i>Learn Later
                                </button>
                            </div>
                        </div>
                        
                        <!-- Back of Card -->
                        <div class="card-back flex flex-col p-8">
                            <div class="flex-1 flex flex-col justify-center items-center">
                                <div class="text-center">
                                    <span class="inline-block px-3 py-1 bg-green-100 text-green-800 rounded-full text-sm font-medium mb-4">Answer</span>
                                    <h3 id="answer-text" class="text-2xl font-bold text-gray-800 mb-4">An object in motion stays in motion unless acted upon by an external force</h3>
                                    <p id="explanation-text" class="text-gray-600">This is also known as the Law of Inertia. It describes how objects behave when no force is acting on them.</p>
                                </div>
                            </div>
                            <div class="flex justify-center space-x-4">
                                <button class="flashcard-action bg-red-100 hover:bg-red-200 text-red-700 px-4 py-2 rounded-lg transition" data-action="difficult">
                                    <i class="fas fa-times-circle mr-2"></i>Too Difficult
                                </button>
                                <button class="flashcard-action bg-green-100 hover:bg-green-200 text-green-700 px-4 py-2 rounded-lg transition" data-action="understood">
                                    <i class="fas fa-check-circle mr-2"></i>Understood
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="flex justify-between">
                    <button id="prev-card" class="bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold py-2 px-4 rounded-lg transition">
                        <i class="fas fa-arrow-left mr-2"></i>Previous
                    </button>
                    <button id="next-card" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-2 px-6 rounded-lg transition">
                        Next<i class="fas fa-arrow-right ml-2"></i>
                    </button>
                </div>
            </div>
        </section>

        <!-- Features Section -->
        <section class="mb-16 fade-in">
            <h2 class="text-3xl font-bold text-center text-gray-800 mb-12">Why Learn With PhysicsFlash?</h2>
            <div class="grid md:grid-cols-3 gap-8">
                <div class="bg-white p-6 rounded-xl shadow-md hover:shadow-lg transition">
                    <div class="text-indigo-600 mb-4">
                        <i class="fas fa-robot text-4xl"></i>
                    </div>
                    <h3 class="text-xl font-semibold mb-2 text-gray-800">AI-Powered Assessment</h3>
                    <p class="text-gray-600">Our system analyzes your responses to create a personalized learning path tailored to your current knowledge level.</p>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-md hover:shadow-lg transition">
                    <div class="text-indigo-600 mb-4">
                        <i class="fas fa-layer-group text-4xl"></i>
                    </div>
                    <h3 class="text-xl font-semibold mb-2 text-gray-800">Adaptive Learning</h3>
                    <p class="text-gray-600">The difficulty adjusts based on your performance, ensuring you're always challenged but not overwhelmed.</p>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-md hover:shadow-lg transition">
                    <div class="text-indigo-600 mb-4">
                        <i class="fas fa-chart-line text-4xl"></i>
                    </div>
                    <h3 class="text-xl font-semibold mb-2 text-gray-800">Progress Tracking</h3>
                    <p class="text-gray-600">Visualize your improvement over time with detailed analytics and progress reports.</p>
                </div>
            </div>
        </section>

        <!-- Topics Section -->
        <section class="mb-16 fade-in">
            <h2 class="text-3xl font-bold text-center text-gray-800 mb-8">Explore Physics Topics</h2>
            <div class="grid sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <div class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition transform hover:-translate-y-1">
                    <div class="h-32 bg-blue-500 flex items-center justify-center">
                        <i class="fas fa-ruler-combined text-white text-5xl"></i>
                    </div>
                    <div class="p-4">
                        <h3 class="font-bold text-lg mb-2 text-gray-800">Classical Mechanics</h3>
                        <p class="text-gray-600 text-sm">Motion, forces, energy, and momentum</p>
                    </div>
                </div>
                <div class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition transform hover:-translate-y-1">
                    <div class="h-32 bg-purple-500 flex items-center justify-center">
                        <i class="fas fa-bolt text-white text-5xl"></i>
                    </div>
                    <div class="p-4">
                        <h3 class="font-bold text-lg mb-2 text-gray-800">Electromagnetism</h3>
                        <p class="text-gray-600 text-sm">Electricity, magnetism, and circuits</p>
                    </div>
                </div>
                <div class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition transform hover:-translate-y-1">
                    <div class="h-32 bg-pink-500 flex items-center justify-center">
                        <i class="fas fa-atom text-white text-5xl"></i>
                    </div>
                    <div class="p-4">
                        <h3 class="font-bold text-lg mb-2 text-gray-800">Quantum Physics</h3>
                        <p class="text-gray-600 text-sm">Particles, waves, and quantum states</p>
                    </div>
                </div>
                <div class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition transform hover:-translate-y-1">
                    <div class="h-32 bg-indigo-500 flex items-center justify-center">
                        <i class="fas fa-star text-white text-5xl"></i>
                    </div>
                    <div class="p-4">
                        <h3 class="font-bold text-lg mb-2 text-gray-800">Astrophysics</h3>
                        <p class="text-gray-600 text-sm">Stars, galaxies, and cosmology</p>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white py-8">
        <div class="container mx-auto px-4">
            <div class="grid md:grid-cols-4 gap-8">
                <div>
                    <h3 class="text-xl font-bold mb-4">PhysicsFlash</h3>
                    <p class="text-gray-400">Making physics learning accessible and personalized through AI-powered flashcards.</p>
                </div>
                <div>
                    <h4 class="font-semibold mb-4">Quick Links</h4>
                    <ul class="space-y-2">
                        <li><a href="#" class="text-gray-400 hover:text-white transition">Home</a></li>
                        <li><a href="#" class="text-gray-400 hover:text-white transition">Courses</a></li>
                        <li><a href="#" class="text-gray-400 hover:text-white transition">About Us</a></li>
                        <li><a href="#" class="text-gray-400 hover:text-white transition">Contact</a></li>
                    </ul>
                </div>
                <div>
                    <h4 class="font-semibold mb-4">Resources</h4>
                    <ul class="space-y-2">
                        <li><a href="#" class="text-gray-400 hover:text-white transition">Blog</a></li>
                        <li><a href="#" class="text-gray-400 hover:text-white transition">FAQs</a></li>
                        <li><a href="#" class="text-gray-400 hover:text-white transition">Support</a></li>
                    </ul>
                </div>
                <div>
                    <h4 class="font-semibold mb-4">Connect</h4>
                    <div class="flex space-x-4">
                        <a href="#" class="text-gray-400 hover:text-white transition"><i class="fab fa-facebook-f"></i></a>
                        <a href="#" class="text-gray-400 hover:text-white transition"><i class="fab fa-twitter"></i></a>
                        <a href="#" class="text-gray-400 hover:text-white transition"><i class="fab fa-instagram"></i></a>
                        <a href="#" class="text-gray-400 hover:text-white transition"><i class="fab fa-linkedin-in"></i></a>
                    </div>
                </div>
            </div>
            <div class="border-t border-gray-700 mt-8 pt-6 text-center text-gray-400">
                <p>&copy; 2023 PhysicsFlash. All rights reserved.</p>
            </div>
        </div>
    </footer>

    <script>
        // Mobile Menu Toggle
        document.getElementById('mobile-menu-button').addEventListener('click', function() {
            const menu = document.getElementById('mobile-menu');
            menu.classList.toggle('hidden');
        });

        // Assessment Modal
        const assessmentModal = document.getElementById('assessment-modal');
        const startAssessmentBtn = document.getElementById('start-assessment');
        const closeAssessmentBtn = document.getElementById('close-assessment');
        const startTestBtn = document.getElementById('start-test');
        const assessmentContent = document.getElementById('assessment-content');
        const progressBar = document.getElementById('progress-bar');
        const progressPercent = document.getElementById('progress-percent');
        const flashcardSection = document.getElementById('flashcard-section');

        startAssessmentBtn.addEventListener('click', function() {
            assessmentModal.classList.remove('hidden');
            document.body.style.overflow = 'hidden';
        });

        closeAssessmentBtn.addEventListener('click', function() {
            assessmentModal.classList.add('hidden');
            document.body.style.overflow = 'auto';
        });

        // Sample assessment questions
        const assessmentQuestions = [
            {
                question: "What does F=ma represent?",
                options: [
                    "Newton's First Law",
                    "Newton's Second Law",
                    "The Law of Gravitation",
                    "The Work-Energy Theorem"
                ]
            },
            {
                question: "Which of these is a vector quantity?",
                options: [
                    "Temperature",
                    "Mass",
                    "Velocity",
                    "Energy"
                ]
            },
            {
                question: "What is the SI unit of electric current?",
                options: [
                    "Volt",
                    "Ohm",
                    "Ampere",
                    "Watt"
                ]
            },
            {
                question: "Which principle explains why ships float?",
                options: [
                    "Pascal's Principle",
                    "Bernoulli's Principle",
                    "Archimedes' Principle",
                    "Hooke's Law"
                ]
            },
            {
                question: "What does E=mc² describe?",
                options: [
                    "Energy of a photon",
                    "Mass-energy equivalence",
                    "Kinetic energy",
                    "Potential energy"
                ]
            }
        ];

        startTestBtn.addEventListener('click', function() {
            startAssessment();
        });

        function startAssessment() {
            let currentQuestion = 0;
            let score = 0;
            let userAnswers = [];
            
            function showQuestion(index) {
                if (index >= assessmentQuestions.length) {
                    // Assessment complete
                    showResults();
                    return;
                }
                
                const question = assessmentQuestions[index];
                const progress = ((index) / assessmentQuestions.length) * 100;
                
                progressBar.style.width = `${progress}%`;
                progressPercent.textContent = `${Math.round(progress)}%`;
                
                let html = `
                    <div class="question-container">
                        <h3 class="text-lg font-medium text-gray-800 mb-4">Question ${index + 1} of ${assessmentQuestions.length}</h3>
                        <p class="text-xl font-semibold mb-6">${question.question}</p>
                        <div class="space-y-3 mb-6">
                `;
                
                question.options.forEach((option, i) => {
                    html += `
                        <button class="option-btn w-full text-left bg-gray-100 hover:bg-gray-200 text-gray-800 py-3 px-4 rounded-lg transition" data-index="${i}">
                            ${option}
                        </button>
                    `;
                });
                
                html += `
                        </div>
                        <div class="flex justify-center">
                            <button class="dont-know-btn bg-gray-200 hover:bg-gray-300 text-gray-800 py-2 px-6 rounded-lg transition">
                                <i class="fas fa-question-circle mr-2"></i>I don't know
                            </button>
                        </div>
                    </div>
                `;
                
                assessmentContent.innerHTML = html;
                
                // Add event listeners to options
                document.querySelectorAll('.option-btn').forEach(btn => {
                    btn.addEventListener('click', function() {
                        const selectedIndex = parseInt(this.getAttribute('data-index'));
                        userAnswers.push({question: index, answer: selectedIndex});
                        currentQuestion++;
                        showQuestion(currentQuestion);
                    });
                });
                
                // Add event listener to "I don't know" button
                document.querySelector('.dont-know-btn').addEventListener('click', function() {
                    userAnswers.push({question: index, answer: -1}); // -1 represents "I don't know"
                    currentQuestion++;
                    showQuestion(currentQuestion);
                });
            }
            
            function showResults() {
                // Simple scoring - in a real app, this would be more sophisticated AI analysis
                const knownAnswers = userAnswers.filter(answer => answer.answer !== -1).length;
                const percentage = Math.round((knownAnswers / assessmentQuestions.length) * 100);
                
                let level, difficulty;
                if (percentage < 30) {
                    level = "Beginner";
                    difficulty = "basic";
                } else if (percentage < 70) {
                    level = "Intermediate";
                    difficulty = "intermediate";
                } else {
                    level = "Advanced";
                    difficulty = "advanced";
                }
                
                progressBar.style.width = `100%`;
                progressPercent.textContent = `100%`;
                
                assessmentContent.innerHTML = `
                    <div class="text-center py-8">
                        <div class="inline-block p-4 bg-green-100 rounded-full mb-4">
                            <i class="fas fa-check-circle text-4xl text-green-600"></i>
                        </div>
                        <h3 class="text-xl font-semibold text-gray-800 mb-2">Assessment Complete!</h3>
                        <p class="text-gray-600 mb-4">Based on your answers, we've determined your current level is:</p>
                        <div class="inline-block px-4 py-2 bg-indigo-600 text-white font-bold rounded-lg mb-6">${level}</div>
                        <p class="text-gray-600 mb-6">We'll create a personalized learning path with ${difficulty} concepts to help you progress.</p>
                        <button id="start-learning" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-2 px-6 rounded-lg transition">
                            Start Learning
                        </button>
                    </div>
                `;
                
                document.getElementById('start-learning').addEventListener('click', function() {
                    assessmentModal.classList.add('hidden');
                    document.body.style.overflow = 'auto';
                    flashcardSection.classList.remove('hidden');
                    document.getElementById('difficulty-level').textContent = level;
                    
                    // Scroll to flashcard section
                    flashcardSection.scrollIntoView({ behavior: 'smooth' });
                });
            }
            
            showQuestion(currentQuestion);
        }

        // Flashcard functionality
        const flashcard = document.getElementById('flashcard');
        const nextCardBtn = document.getElementById('next-card');
        const prevCardBtn = document.getElementById('prev-card');
        const currentCardDisplay = document.getElementById('current-card');
        
        // Sample flashcard data - in a real app, this would come from a database
        const flashcards = {
            beginner: [
                {
                    question: "What is Newton's First Law of Motion?",
                    answer: "An object in motion stays in motion unless acted upon by an external force",
                    explanation: "This is also known as the Law of Inertia. It describes how objects behave when no force is acting on them."
                },
                {
                    question: "What is the formula for velocity?",
                    answer: "Velocity = Displacement / Time",
                    explanation: "Velocity is a vector quantity that includes both speed and direction of motion."
                },
                {
                    question: "What is the SI unit of force?",
                    answer: "Newton (N)",
                    explanation: "One newton is the force needed to accelerate one kilogram of mass at one meter per second squared."
                }
            ],
            intermediate: [
                {
                    question: "What is the work-energy theorem?",
                    answer: "The work done on an object equals its change in kinetic energy",
                    explanation: "W = ΔKE. This connects the concepts of force, displacement, and energy."
                },
                {
                    question: "What does the term 'terminal velocity' mean?",
                    answer: "The maximum velocity an object reaches when the force of gravity is balanced by air resistance",
                    explanation: "At terminal velocity, the net force is zero so acceleration stops."
                },
                {
                    question: "What is the difference between elastic and inelastic collisions?",
                    answer: "Elastic collisions conserve both momentum and kinetic energy, while inelastic collisions conserve only momentum",
                    explanation: "In real-world collisions, some kinetic energy is usually converted to other forms (heat, sound, etc.)"
                }
            ],
            advanced: [
                {
                    question: "Explain the photoelectric effect",
                    answer: "Emission of electrons when light shines on a material, demonstrating light's particle nature",
                    explanation: "This phenomenon couldn't be explained by classical wave theory and helped establish quantum mechanics."
                },
                {
                    question: "What is the significance of the Lorentz factor in special relativity?",
                    answer: "It describes time dilation, length contraction, and relativistic mass increase at high velocities",
                    explanation: "γ = 1/√(1-v²/c²). As v approaches c, γ approaches infinity."
                },
                {
                    question: "What is the difference between bosons and fermions?",
                    answer: "Bosons have integer spin and can occupy the same quantum state, while fermions have half-integer spin and obey the Pauli exclusion principle",
                    explanation: "This fundamental distinction leads to dramatically different behavior at quantum scales."
                }
            ]
        };
        
        let currentDeck = [];
        let currentCardIndex = 0;
        let isFlipped = false;
        
        function loadDeck(difficulty) {
            currentDeck = [...flashcards[difficulty.toLowerCase()]];
            currentCardIndex = 0;
            updateCard();
        }
        
        function updateCard() {
            if (currentCardIndex >= currentDeck.length) {
                // Deck completed
                document.getElementById('question-text').textContent = "You've completed this deck!";
                document.getElementById('answer-text').textContent = "Great job!";
                document.getElementById('explanation-text').textContent = "Would you like to review again or move to more advanced topics?";
                return;
            }
            
            const card = currentDeck[currentCardIndex];
            document.getElementById('question-text').textContent = card.question;
            document.getElementById('answer-text').textContent = card.answer;
            document.getElementById('explanation-text').textContent = card.explanation;
            currentCardDisplay.textContent = `${currentCardIndex + 1}/${currentDeck.length}`;
            
            // Reset card to front if it was flipped
            if (isFlipped) {
                flashcard.classList.remove('flipped');
                isFlipped = false;
            }
        }
        
        flashcard.addEventListener('click', function() {
            this.classList.toggle('flipped');
            isFlipped = !isFlipped;
        });
        
        nextCardBtn.addEventListener('click', function() {
            if (currentCardIndex < currentDeck.length - 1) {
                currentCardIndex++;
                updateCard();
            }
        });
        
        prevCardBtn.addEventListener('click', function() {
            if (currentCardIndex > 0) {
                currentCardIndex--;
                updateCard();
            }
        });
        
        // Handle flashcard action buttons (Don't Know, Learn Later, etc.)
        document.addEventListener('click', function(e) {
            if (e.target.closest('.flashcard-action')) {
                const action = e.target.closest('.flashcard-action').getAttribute('data-action');
                handleFlashcardAction(action);
            }
        });
        
        function handleFlashcardAction(action) {
            // In a real app, this would track user performance and adjust the learning path
            switch(action) {
                case 'dont-know':
                    alert("We'll make sure to reinforce this concept in future sessions.");
                    break;
                case 'learn-later':
                    alert("This card has been added to your 'Review Later' list.");
                    break;
                case 'difficult':
                    alert("We'll adjust the difficulty and provide more foundational material on this topic.");
                    break;
                case 'understood':
                    alert("Great! We'll track your progress and introduce related advanced concepts.");
                    break;
            }
            
            // Move to next card
            if (currentCardIndex < currentDeck.length - 1) {
                currentCardIndex++;
                updateCard();
            }
        }
        
        // Initialize with beginner deck (in a real app, this would be based on assessment)
        loadDeck('beginner');
    </script>
</body>
</html>
