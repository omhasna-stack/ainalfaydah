<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🚀 Prepare today, shine tomorrow! 🚀 استعد اليوم، وتألّق غداً!</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Load Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /* Custom scrollbar for better look */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f0f4f8; }
        ::-webkit-scrollbar-thumb { background: #3b82f6; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #2563eb; }

        /* Arabic specific adjustments for text alignment */
        .rtl-text { direction: rtl; text-align: right; }

        /* Ensure smooth transitions for interactive elements */
        .transition-all { transition: all 0.3s ease-in-out; }

        .loader {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #3b82f6;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        'primary': '#3b82f6', // blue-500
                        'secondary': '#10b981', // emerald-500
                        'background': '#f8fafc', // slate-50
                    },
                    fontFamily: {
                        sans: ['Inter', 'Arial', 'sans-serif'],
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-4 font-sans">

    <!-- Main Quiz Container -->
    <div id="quiz-container" class="bg-white shadow-2xl rounded-xl w-full max-w-lg p-6 md:p-8 space-y-6 transition-all border-t-8 border-primary">
        <h1 class="text-2xl font-extrabold text-gray-800 text-center mb-6">
            <span class="text-primary">🚀Grade 2... Prepare today, shine tomorrow! 🚀</span>
            <br>
            <span class="text-lg text-secondary rtl-text">استعد اليوم، وتألّق غداً!</span>
        </h1>

        <!-- 1. Welcome and Name Input Screen -->
        <div id="welcome-screen" class="space-y-6">
            <p class="text-gray-600 text-base text-center">
                Hello! Please enter your name to start the Quiz: / مرحباً! من فضلك أدخل اسمك لبدء الاختبار:
            </p>
            <input type="text" id="user-name" placeholder="Your Name / اسمك" class="w-full p-3 border-2 border-gray-300 rounded-lg focus:border-primary focus:ring-primary transition-all">

            <p class="text-gray-600 text-sm font-bold text-center pt-2">
                Choose your topic: / اختر موضوعك:
            </p>
            <div class="flex flex-col space-y-3">
                <button onclick="startQuiz('CCDI')" class="topic-btn bg-secondary hover:bg-emerald-600 text-white font-bold py-3 px-4 rounded-lg shadow-md transition-all">
                    CCDI: Design Process 🎨
                </button>
                <button onclick="startQuiz('AI')" class="topic-btn bg-blue-500 hover:bg-blue-600 text-white font-bold py-3 px-4 rounded-lg shadow-md transition-all">
                    AI: Artificial Intelligence 🤖
                </button>
            </div>
        </div>

        <!-- 2. Quiz Questions Screen -->
        <div id="quiz-screen" class="hidden space-y-6">
            <p id="progress" class="text-sm font-medium text-gray-500 text-center"></p>

            <div class="bg-primary/10 p-4 rounded-xl border border-primary/20">
                <!-- Image Placeholder Container (for image questions) -->
                <div id="question-image-container" class="mb-4 hidden">
                    <img id="question-image" src="" alt="Question Image" class="w-full h-auto rounded-lg shadow-inner mx-auto max-h-40 object-contain" onerror="this.classList.add('hidden')">
                </div>
                
                <p id="question-text" class="text-lg font-semibold text-gray-800 mb-2"></p>
                <p id="question-text-ar" class="text-lg font-semibold text-gray-800 rtl-text"></p>
            </div>

            <div id="options-container" class="space-y-3">
                <!-- Options will be injected here -->
            </div>
            
            <!-- Gemini AI Help Button -->
            <button id="gemini-help-btn" onclick="askGeminiForHelp()" class="w-full bg-yellow-500 hover:bg-yellow-600 text-white font-bold py-3 rounded-lg shadow-lg transition-all flex items-center justify-center space-x-2">
                <span id="help-text">✨ Ask Gemini for a Hint / اسأل چيميني عن تلميح ✨</span>
                <div id="help-loader" class="loader hidden"></div>
            </button>
            <div id="gemini-hint-area" class="mt-2 p-3 bg-yellow-100 rounded-lg text-sm text-yellow-800 hidden"></div>
            
            <button id="submit-btn" onclick="submitAnswer()" class="w-full bg-primary hover:bg-blue-600 text-white font-bold py-3 rounded-lg shadow-lg transition-all opacity-50 cursor-not-allowed" disabled>
                Submit Answer / إرسال الإجابة
            </button>
            <button id="next-btn" onclick="nextQuestion()" class="w-full bg-gray-500 hover:bg-gray-600 text-white font-bold py-3 rounded-lg shadow-lg transition-all hidden">
                Next Question / السؤال التالي
            </button>

            <!-- Feedback Area -->
            <div id="feedback-area" class="min-h-[50px]"></div>
        </div>

        <!-- 3. Results Screen -->
        <div id="results-screen" class="hidden text-center space-y-6">
            <h2 class="text-3xl font-extrabold text-primary">Quiz Complete! 🎉</h2>
            <h2 class="text-3xl font-extrabold text-secondary rtl-text">انتهى الاختبار! 🎉</h2>

            <div class="bg-primary/10 p-4 rounded-xl border border-primary/20 space-y-2">
                <p class="text-xl font-bold text-gray-700">Your Score: <span id="final-score" class="text-primary text-2xl">0/5</span></p>
                <p class="text-xl font-bold text-gray-700 rtl-text">درجتك النهائية: <span id="final-score-ar" class="text-primary text-2xl">0/5</span></p>
            </div>

            <!-- Gemini Generated Encouragement -->
            <p id="encouragement-loader" class="text-lg font-semibold text-gray-500 flex items-center justify-center space-x-2">
                <span>Generating encouragement... / جاري توليد رسالة التشجيع...</span>
                <div class="loader"></div>
            </p>
            <p id="encouragement" class="text-lg font-semibold text-gray-700 transition-all hidden"></p>

            <div id="mistakes-review" class="text-left space-y-4 max-h-60 overflow-y-auto bg-gray-50 p-4 rounded-lg">
                <!-- Mistakes will be shown here -->
            </div>
            
            <p class="text-gray-600 pt-4">
                Do you want to continue practicing? / هل تريد مواصلة التدريب؟
            </p>
            <div class="flex space-x-4 justify-center rtl:space-x-reverse">
                <button onclick="continueQuiz(true)" id="continue-btn" class="flex-1 bg-secondary hover:bg-emerald-600 text-white font-bold py-3 rounded-lg shadow-md transition-all">
                    Yes 👍 / نعم
                </button>
                <button onclick="continueQuiz(false)" class="flex-1 bg-red-500 hover:bg-red-600 text-white font-bold py-3 rounded-lg shadow-md transition-all">
                    No 🛑 / لا
                </button>
            </div>
        </div>

        <!-- 4. Goodbye Screen -->
        <div id="goodbye-screen" class="hidden text-center space-y-6">
            <h2 class="text-4xl font-extrabold text-primary">
                Goodbye! See you next time! 👋
            </h2>
            <h2 class="text-4xl font-extrabold text-secondary rtl-text">
                إلى اللقاء! نراك لاحقاً! 👋
            </h2>
        </div>
    </div>

    <script>
        // Global variables for Gemini API
        const API_KEY = ""; // Kept empty, handled by environment
        const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${API_KEY}`;
        const MAX_RETRIES = 5;

        // --- Question Data ---
        // NOTE: Arabic translations for options (ar property) have been removed as requested.
        const CCDI_QUESTIONS = [
            // Round 1 Questions (Index 0-4)
            { q: "What is the main goal of the Design Process?", q_ar: "ما هو الهدف الرئيسي لعملية التصميم؟", options: [{ text: "To memorize facts", correct: false }, { text: "To make something or solve a problem", correct: true }, { text: "To play video games", correct: false }], exp: "The Design Process is used to make something new or solve a problem.", exp_ar: "تُستخدم عملية التصميم لصنع شيء جديد أو حل مشكلة." },
            { q: "Which step comes right after 'Ask'?", q_ar: "ما هي الخطوة التي تأتي مباشرة بعد 'اسأل'؟", options: [{ text: "Test", correct: false }, { text: "Imagine (تخيل)", correct: true }, { text: "Create", correct: false }], exp: "The order is Ask, Imagine, Plan, Create, Test, Improve. 'Imagine' is where you think of ideas.", exp_ar: "الترتيب هو اسأل، تخيل، خطط، أنشئ، اختبر، حسّن. 'تخيل' هي حيث تفكر في الأفكار." },
            { q: "In the 'Ask' step, what do you primarily do?", q_ar: "في خطوة 'اسأل'، ما هو الشيء الرئيسي الذي تفعله؟", options: [{ text: "Build the project", correct: false }, { text: "Understand the problem you want to solve", correct: true }, { text: "Choose the best idea", correct: false }], exp: "The 'Ask' step is where we ask questions to understand the problem.", exp_ar: "خطوة 'اسأل' هي حيث نسأل الأسئلة لفهم المشكلة." },
            { q: "Why do we 'Plan'?", q_ar: "لماذا 'نخطط'؟", options: [{ text: "To test the final product", correct: false }, { text: "To choose the best idea and make a clear plan", correct: true }, { text: "To think of many new ideas", correct: false }], exp: "In 'Plan', we choose the best idea and make a clear plan to follow.", exp_ar: "في خطوة 'خطط'، نختار الفكرة الأفضل ونضع خطة واضحة لاتباعها." },
            { q: "What is the main purpose of 'Improve'?", q_ar: "ما هو الهدف الرئيسي من 'حسّن'؟", options: [{ text: "To finish the project and stop", correct: false }, { text: "To find ways to make the project better and fix problems", correct: true }, { text: "To draw the plans", correct: false }], exp: "In 'Improve', we find ways to make the project better and fix any problems.", exp_ar: "في خطوة 'حسّن'، نجد طرقاً لتحسين المشروع وإصلاح أية مشاكل." },
            
            // Round 2 Questions (Index 5-9)
            { q: "What is the action associated with the 'Imagine' step?", q_ar: "ما هو الفعل المرتبط بخطوة 'تخيل'؟", options: [{ text: "Building the solution", correct: false }, { text: "Thinking of many ideas for solutions", correct: true }, { text: "Checking if it works", correct: false }], exp: "In 'Imagine', we think of many ideas that could be good solutions.", exp_ar: "في خطوة 'تخيل'، نفكر في العديد من الأفكار التي يمكن أن تكون حلولاً جيدة." },
            { q: "Which step is about testing what you built?", q_ar: "ما هي الخطوة التي تدور حول اختبار ما بنيته؟", options: [{ text: "Plan", correct: false }, { text: "Test (اختبر)", correct: true }, { text: "Create", correct: false }], exp: "In 'Test', we test what we made to see if it works well.", exp_ar: "في خطوة 'اختبر'، نختبر ما صنعناه لمعرفة ما إذا كان يعمل جيداً." },
            // Image Question (using a text description)
            { q: "You are looking at a drawing on paper. Which step are you most likely in?", q_ar: "أنت تنظر إلى رسم على الورق. في أي خطوة من المحتمل أن تكون؟", options: [{ text: "Ask", correct: false }, { text: "Plan (خطط)", correct: true }, { text: "Test", correct: false }], exp: "Drawing a design is part of the 'Plan' step, where you make a clear plan.", exp_ar: "رسم تصميم هو جزء من خطوة 'خطط'، حيث تضع خطة واضحة." },
            { q: "The step 'Test' comes right after which step?", q_ar: "خطوة 'اختبر' تأتي مباشرة بعد أي خطوة؟", options: [{ text: "Imagine", correct: false }, { text: "Create (أنشئ)", correct: true }, { text: "Ask", correct: false }], exp: "The order is Ask, Imagine, Plan, Create, Test, Improve. 'Test' follows 'Create'.", exp_ar: "الترتيب هو اسأل، تخيل، خطط، أنشئ، اختبر، حسّن. 'اختبر' تلي 'أنشئ'." },
            { q: "What is the main goal of the 'Create' step?", q_ar: "ما هو الهدف الرئيسي من خطوة 'أنشئ'؟", options: [{ text: "To build or make the project using our plan", correct: true }, { text: "To ask about the problem", correct: false }, { text: "To choose the best idea", correct: false }], exp: "In 'Create', we build or make the project using our plan.", exp_ar: "في خطوة 'أنشئ'، نبني أو نصنع المشروع باستخدام خطتنا." },
        ];

        const AI_QUESTIONS = [
            // Round 1 Questions (Index 0-4)
            { q: "What does AI stand for?", q_ar: "ماذا يعني الاختصار AI؟", options: [{ text: "Automated Ideas", correct: false }, { text: "Artificial Intelligence", correct: true }, { text: "Amazing Internet", correct: false }], exp: "AI stands for Artificial Intelligence.", exp_ar: "يشير AI إلى الذكاء الاصطناعي." },
            { q: "How does a smart AI machine mainly learn?", q_ar: "كيف تتعلم آلة الذكاء الاصطناعي الذكية بشكل رئيسي؟", options: [{ text: "By sleeping", correct: false }, { text: "By learning from data", correct: true }, { text: "By making moral choices", correct: false }], exp: "AI is a smart machine that learns from data.", exp_ar: "الذكاء الاصطناعي هو آلة ذكية تتعلم من البيانات." },
            { q: "Which ability is ONLY for humans, not AI?", q_ar: "أي قدرة هي للبشر فقط، وليست للذكاء الاصطناعي؟", options: [{ text: "Playing music", correct: false }, { text: "Processing data", correct: false }, { text: "Feeling happy or sad", correct: true }], exp: "Only humans can feel happy, sad, imagine, forgive, and make moral choices.", exp_ar: "البشر فقط هم من يمكنهم الشعور بالسعادة والحزن والتخيل والمسامحة واتخاذ الخيارات الأخلاقية." },
            { q: "What can AI do?", q_ar: "ماذا يمكن للذكاء الاصطناعي أن يفعل؟", options: [{ text: "Forgive a friend", correct: false }, { text: "Answer questions", correct: true }, { text: "Make moral choices", correct: false }], exp: "AI can do many tasks like answering questions or playing music.", exp_ar: "يمكن للذكاء الاصطناعي القيام بالعديد من المهام مثل الإجابة على الأسئلة أو تشغيل الموسيقى." },
            { q: "What does AI use to 'see and hear' the world?", q_ar: "ماذا يستخدم الذكاء الاصطناعي 'للرؤية والسمع' في العالم؟", options: [{ text: "Smell sensors", correct: false }, { text: "Cameras and microphones", correct: true }, { text: "Telescopes", correct: false }], exp: "AI uses cameras and microphones to see and hear.", exp_ar: "يستخدم الذكاء الاصطناعي الكاميرات والميكروفونات للرؤية والسمع." },

            // Round 2 Questions (Index 5-9)
            { q: "Do robots have feelings like happiness or sadness?", q_ar: "هل تمتلك الروبوتات مشاعر كالسعادة أو الحزن؟", options: [{ text: "Yes, always", correct: false }, { text: "No, robots do not have feelings", correct: true }, { text: "Only when programmed to", correct: false }], exp: "Robots do not have feelings.", exp_ar: "الروبوتات ليس لديها مشاعر." },
            { q: "What is an example of a task AI can perform?", q_ar: "ما هو مثال لمهمة يمكن للذكاء الاصطناعي أداؤها؟", options: [{ text: "Imagining a new color", correct: false }, { text: "Playing music", correct: true }, { text: "Feeling sadness", correct: false }], exp: "AI can do many tasks like answering questions or playing music.", exp_ar: "يمكن للذكاء الاصطناعي القيام بالعديد من المهام مثل الإجابة على الأسئلة أو تشغيل الموسيقى." },
            // Image Question 
            { 
                q: "Look at the image below. Which unique human ability does it represent?", 
                q_ar: "انظر إلى الصورة أدناه. ما هي القدرة البشرية الفريدة التي تمثلها؟", 
                image: "https://placehold.co/400x150/f9f654/1e3a8a?text=Idea+Lightbulb+%F0%9F%92%A1", // Lightbulb placeholder
                options: [
                    { text: "Processing data", correct: false }, 
                    { text: "Forgiving others", correct: false }, 
                    { text: "Imagining new ideas (تخيل)", correct: true }
                ], 
                exp: "The lightbulb represents imagining new ideas, a unique human ability that AI cannot do.", 
                exp_ar: "يمثل المصباح تخيل الأفكار الجديدة، وهي قدرة بشرية فريدة لا يستطيع الذكاء الاصطناعي القيام بها." 
            },
            { q: "Which one is NOT a human-only ability?", q_ar: "أي من هذه ليست قدرة مقتصرة على البشر؟", options: [{ text: "Feeling happy", correct: false }, { text: "Playing music", correct: true }, { text: "Making moral choices", correct: false }], exp: "AI can play music; the others are human-only feelings/choices.", exp_ar: "يمكن للذكاء الاصطناعي تشغيل الموسيقى؛ أما البقية فهي مشاعر/خيارات مقتصرة على البشر." },
            { q: "How is AI described in simple terms?", q_ar: "كيف يوصف الذكاء الاصطناعي بعبارات بسيطة؟", options: [{ text: "A fast calculator", correct: false }, { text: "A smart machine that learns from data", correct: true }, { text: "A human replacement", correct: false }], exp: "AI is a smart machine that learns from data.", exp_ar: "الذكاء الاصطناعي هو آلة ذكية تتعلم من البيانات." },
        ];
        
        // --- Quiz State Variables ---
        let userName = '';
        let currentTopic = '';
        let currentQuestions = [];
        let currentQIndex = 0;
        let score = 0;
        let mistakes = [];
        let roundCount = 0; // Tracks which round we are in (max 2)

        // --- Gemini AI Helper Functions ---

        /**
         * Generic function to call the Gemini API with exponential backoff.
         * @param {object} payload - The API request payload.
         * @param {number} retries - Current retry attempt count.
         * @returns {Promise<string>} The generated text response.
         */
        async function callGeminiApi(payload, retries = 0) {
            try {
                const response = await fetch(API_URL, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (response.status === 429 && retries < MAX_RETRIES) {
                    const delay = Math.pow(2, retries) * 1000;
                    console.log(`Rate limit exceeded. Retrying in ${delay / 1000}s...`);
                    await new Promise(resolve => setTimeout(resolve, delay));
                    return callGeminiApi(payload, retries + 1);
                }

                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }

                const result = await response.json();
                const text = result.candidates?.[0]?.content?.parts?.[0]?.text;
                if (!text) {
                    throw new Error("Gemini response structure is missing text.");
                }
                return text;

            } catch (error) {
                console.error("Gemini API call failed:", error);
                return "Sorry, the AI helper is currently unavailable. / آسف، المساعد الذكي غير متاح حالياً.";
            }
        }

        async function askGeminiForHelp() {
            const currentQ = currentQuestions[currentQIndex];
            if (!currentQ) return;
            
            const helpBtn = document.getElementById('gemini-help-btn');
            const helpTextSpan = document.getElementById('help-text');
            const helpLoader = document.getElementById('help-loader');
            const hintArea = document.getElementById('gemini-hint-area');
            
            helpBtn.disabled = true;
            helpTextSpan.classList.add('hidden');
            helpLoader.classList.remove('hidden');
            hintArea.classList.add('hidden');
            hintArea.innerHTML = '';

            const userQuery = `I am a grade 2 student doing a quiz. The current question is: "${currentQ.q}". The question is about ${currentTopic}. Give me a short, simple, single-line hint in both English and Arabic that doesn't give away the direct answer but helps me think.`;
            
            const systemPrompt = "You are a friendly, encouraging, and very simple educational assistant for a 7-year-old child (Grade 2). You must provide hints that are extremely easy to understand and always appear in both English and Arabic in one line, separated by a slash (/).";

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
            };

            const hint = await callGeminiApi(payload);

            hintArea.innerHTML = `<i class="fas fa-lightbulb mr-2"></i> ${hint}`;
            hintArea.classList.remove('hidden');
            
            helpBtn.disabled = false;
            helpTextSpan.classList.remove('hidden');
            helpLoader.classList.add('hidden');
        }

        async function generateEncouragement() {
            const totalQ = roundCount * 5;
            const percentage = Math.round((score / totalQ) * 100);
            
            const loader = document.getElementById('encouragement-loader');
            const finalMsgElement = document.getElementById('encouragement');
            
            loader.classList.remove('hidden');
            finalMsgElement.classList.add('hidden');
            finalMsgElement.textContent = '';

            const userQuery = `The student's name is ${userName}. The quiz score is ${score} out of ${totalQ}, which is ${percentage}%. Generate a very short, positive, and personalized one-line encouragement message for a child in Grade 2. The message must be friendly and appear in both English and Arabic, separated by a slash (/). Focus on their effort and future practice.`;

            const systemPrompt = "You are an AI motivator and friend for young students. Your response must be a single, encouraging sentence in both English and Arabic, designed for a 7-year-old. Use an emoji.";

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
            };

            const msg = await callGeminiApi(payload);
            
            finalMsgElement.textContent = msg;
            finalMsgElement.classList.remove('hidden');
            loader.classList.add('hidden');
        }

        // --- Standard Quiz Logic ---

        // Function to shuffle questions and select 5 unique ones for the current round
        function getRoundQuestions(sourceArray) {
            // Shuffle the entire array once to ensure randomness across rounds
            for (let i = sourceArray.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [sourceArray[i], sourceArray[j]] = [sourceArray[j], sourceArray[i]];
            }

            // For round 1, take the first 5 questions. For round 2, take the next 5.
            const start = (roundCount - 1) * 5; 
            const end = start + 5;

            // Only return the 5 questions for the current round (0-4 or 5-9)
            return sourceArray.slice(start, end);
        }

        // --- View Handlers ---
        function showScreen(screenId) {
            document.getElementById('welcome-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('results-screen').classList.add('hidden');
            document.getElementById('goodbye-screen').classList.add('hidden');
            document.getElementById(screenId).classList.remove('hidden');
        }

        // --- Quiz Logic ---
        function startQuiz(topic) {
            userName = document.getElementById('user-name').value.trim() || 'Student';

            if (!['CCDI', 'AI'].includes(topic)) return;

            currentTopic = topic;
            if (roundCount === 0) { // Only reset mistakes and score completely at the start of the first round
                mistakes = [];
                score = 0;
            }
            roundCount++; // Increment round to 1 or 2
            
            // Reset per round state
            currentQIndex = 0;
            
            const sourceArray = topic === 'CCDI' ? CCDI_QUESTIONS : AI_QUESTIONS;
            currentQuestions = getRoundQuestions(sourceArray);

            showScreen('quiz-screen');
            displayQuestion();
        }

        function displayQuestion() {
            if (currentQIndex >= currentQuestions.length) {
                endQuiz();
                return;
            }

            const qData = currentQuestions[currentQIndex];
            // Calculate current question number across all rounds
            const qNum = (roundCount - 1) * 5 + currentQIndex + 1; 

            document.getElementById('progress').innerHTML = `Question ${qNum} | السؤال ${qNum}`;
            document.getElementById('question-text').textContent = qData.q;
            document.getElementById('question-text-ar').textContent = qData.q_ar;
            document.getElementById('feedback-area').innerHTML = '';
            document.getElementById('gemini-hint-area').classList.add('hidden'); // Clear hint area

            // --- Image Handling ---
            const imageContainer = document.getElementById('question-image-container');
            const imageElement = document.getElementById('question-image');

            if (qData.image) {
                imageElement.src = qData.image;
                imageContainer.classList.remove('hidden');
            } else {
                imageElement.src = '';
                imageContainer.classList.add('hidden');
            }
            // -------------------------

            const optionsContainer = document.getElementById('options-container');
            optionsContainer.innerHTML = '';
            
            // Reset button states
            document.getElementById('submit-btn').classList.remove('hidden');
            document.getElementById('submit-btn').classList.add('opacity-50', 'cursor-not-allowed');
            document.getElementById('submit-btn').disabled = true;
            document.getElementById('next-btn').classList.add('hidden');
            document.getElementById('gemini-help-btn').disabled = false; // Re-enable help button

            qData.options.forEach((option, index) => {
                const optionBtn = document.createElement('button');
                optionBtn.setAttribute('data-index', index);
                optionBtn.onclick = () => selectOption(optionBtn);
                // Use data-correct attribute for easier lookup after submission
                optionBtn.setAttribute('data-correct', option.correct); 
                optionBtn.className = 'w-full text-left p-3 border-2 border-gray-300 rounded-lg hover:bg-primary/10 transition-all font-medium text-gray-700 option-btn cursor-pointer';
                
                // Content for the button: ONLY English text is included as requested
                optionBtn.innerHTML = `
                    <span class="block">${String.fromCharCode(65 + index)}. ${option.text}</span>
                `;
                optionsContainer.appendChild(optionBtn);
            });
        }

        function selectOption(selectedBtn) {
            document.querySelectorAll('.option-btn').forEach(btn => {
                btn.classList.remove('bg-primary/20', 'border-primary');
                btn.classList.add('border-gray-300');
            });
            selectedBtn.classList.add('bg-primary/20', 'border-primary');
            selectedBtn.classList.remove('border-gray-300');

            // Enable submit button
            document.getElementById('submit-btn').classList.remove('opacity-50', 'cursor-not-allowed');
            document.getElementById('submit-btn').disabled = false;
        }

        function submitAnswer() {
            const selectedBtn = document.querySelector('.option-btn.bg-primary\\/20');
            if (!selectedBtn) {
                return;
            }

            // Disable all options and submit/help buttons
            document.querySelectorAll('.option-btn').forEach(btn => {
                btn.disabled = true;
                btn.classList.remove('cursor-pointer', 'hover:bg-primary/10');
            });
            document.getElementById('submit-btn').classList.add('hidden');
            document.getElementById('next-btn').classList.remove('hidden');
            document.getElementById('gemini-help-btn').disabled = true;

            const isCorrect = selectedBtn.getAttribute('data-correct') === 'true';
            const qData = currentQuestions[currentQIndex];

            let feedbackHTML = '';

            if (isCorrect) {
                score++;
                selectedBtn.classList.add('bg-secondary/50', 'border-secondary', 'shadow-md');
                feedbackHTML = `
                    <p class="text-secondary font-bold text-lg flex items-center">
                        <i class="fas fa-check-circle mr-2"></i> Correct! Excellent job! / إجابة صحيحة! عمل ممتاز!
                    </p>
                `;
            } else {
                // Find correct answer and highlight it
                const correctOption = qData.options.find(opt => opt.correct);
                const correctBtn = document.querySelector(`.option-btn[data-correct="true"]`);
                
                selectedBtn.classList.add('bg-red-500/50', 'border-red-500', 'shadow-md');
                if (correctBtn) {
                    correctBtn.classList.add('bg-green-500/50', 'border-green-500', 'shadow-md');
                }

                mistakes.push({
                    q: qData.q,
                    q_ar: qData.q_ar,
                    correct_answer: correctOption.text, // ONLY English answer stored
                    explanation: qData.exp,
                    explanation_ar: qData.exp_ar,
                });

                feedbackHTML = `
                    <p class="text-red-500 font-bold text-lg flex items-center mb-2">
                        <i class="fas fa-times-circle mr-2"></i> Incorrect. Review the answer below to learn! / إجابة خاطئة. راجع الإجابة أدناه لتتعلم!
                    </p>
                    <div class="mt-3 text-base text-gray-800">
                        <p class="font-bold text-green-700">✅ Correct Answer: ${correctOption.text}</p>
                        <p class="mt-2 pt-2 border-t border-gray-300"><strong>Explanation:</strong> ${qData.exp}</p>
                        <p class="rtl-text"><strong>التفسير:</strong> ${qData.exp_ar}</p>
                    </div>
                `;
            }

            // Enhanced feedback area display
            document.getElementById('feedback-area').innerHTML = `
                <div class="mt-4 p-4 rounded-xl border-l-4 shadow-lg ${isCorrect ? 'bg-secondary/10 border-secondary' : 'bg-red-500/10 border-red-500'}">
                    ${feedbackHTML}
                </div>
            `;
        }

        function nextQuestion() {
            currentQIndex++;
            displayQuestion();
        }

        function endQuiz() {
            // Update score
            document.getElementById('final-score').textContent = `${score}/${(roundCount * 5)}`;
            document.getElementById('final-score-ar').textContent = `${score}/${(roundCount * 5)}`;

            // Generate Encouragement using Gemini
            generateEncouragement();

            // Mistakes Review
            const reviewContainer = document.getElementById('mistakes-review');
            reviewContainer.innerHTML = '';
            if (mistakes.length > 0) {
                reviewContainer.innerHTML = `<h3 class="text-lg font-bold text-red-700 mb-2">Mistakes Review:</h3><h3 class="text-lg font-bold text-red-700 mb-2 rtl-text">مراجعة الأخطاء:</h3>`;
                mistakes.forEach((mistake, index) => {
                    reviewContainer.innerHTML += `
                        <div class="p-3 bg-red-100 rounded-lg border-l-4 border-red-500">
                            <p class="font-semibold text-red-800">Q: ${mistake.q}</p>
                            <p class="font-semibold text-red-800 rtl-text">س: ${mistake.q_ar}</p>
                            <p class="mt-1 text-sm">✅ Correct Answer: <span class="text-green-700 font-bold">${mistake.correct_answer}</span></p>
                            <p class="text-xs mt-1 italic">(${mistake.explanation})</p>
                            <p class="text-xs mt-1 italic rtl-text">(${mistake.explanation_ar})</p>
                        </div>
                    `;
                });
            } else {
                 reviewContainer.innerHTML = `
                    <p class="text-lg font-bold text-secondary">No mistakes! Excellent work! 🌟</p>
                    <p class="text-lg font-bold text-secondary rtl-text">لا توجد أخطاء! عمل ممتاز! 🌟</p>
                `;
            }

            // Disable 'Yes' button if max rounds reached
            const continueBtn = document.getElementById('continue-btn');
            if (roundCount >= 2) {
                continueBtn.disabled = true;
                continueBtn.classList.add('opacity-50', 'cursor-not-allowed');
                continueBtn.innerHTML = 'Finished Practice / انتهى التدريب';
            } else {
                continueBtn.disabled = false;
                continueBtn.classList.remove('opacity-50', 'cursor-not-allowed');
                continueBtn.innerHTML = 'Yes 👍 / نعم';
            }

            showScreen('results-screen');
        }

        function continueQuiz(shouldContinue) {
            if (shouldContinue && roundCount < 2) {
                // If the user wants to continue and hasn't done the second round yet
                startQuiz(currentTopic); // startQuiz handles roundCount increment and question selection
            } else {
                // If user says no, or if they finished both rounds (max 2)
                showScreen('goodbye-screen');
                // Optional: set a timeout to return to the welcome screen after a few seconds
                setTimeout(() => {
                    roundCount = 0; // Reset round count for a completely new session
                    showScreen('welcome-screen');
                }, 5000); 
            }
        }

        // Initialize the welcome screen on load
        document.addEventListener('DOMContentLoaded', () => {
            showScreen('welcome-screen');
        });
    </script>
</body>
</html>
