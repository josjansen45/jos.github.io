# jos.github.io
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Financial Education. Simplified.</title>
    <!-- Load Tailwind CSS --><script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom font import for a clean, modern look */
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f5f5f7; /* Light neutral background for the overall page */
            scroll-behavior: smooth;
        }
        /* Custom scroll indicator styling */
        #scroll-indicator {
            height: 3px;
            background: linear-gradient(90deg, #059669, #34d399); /* Green gradient for finance/growth theme */
            width: 0;
            position: fixed;
            top: 0;
            left: 0;
            z-index: 50;
        }
        /* Content Styling: Ensure content is above the dark hero background */
        .hero-content {
            position: relative;
            z-index: 10;
        }
        
        /* Custom CSS for the Open Book Visual */
        #financial-book {
            position: relative;
            transform-style: preserve-3d; /* Allows child elements to be positioned in 3D */
            /* Initial transform: tilt the whole book back. This will be animated by JS. */
            transform: perspective(1500px) rotateX(15deg) rotateY(0deg);
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
            /* Ensure rounded corners on the overall book structure */
            border-radius: 1.5rem; 
            max-width: 800px; /* Constrain width for a more book-like aspect */
        }

        .book-page {
            position: absolute;
            top: 0;
            width: 50%;
            height: 100%;
            background-color: #ffffff; /* White pages */
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            align-items: flex-start;
            padding: 2.5rem; /* Tailwind p-10 */
            box-sizing: border-box;
            backface-visibility: hidden; /* Hide back of element when rotated */
            transform-origin: 0% 50%; /* Pivot point for rotation on the spine */
            box-shadow: inset -5px 0 10px rgba(0,0,0,0.05); /* Inner shadow for page depth */
        }

        .book-page-left {
            left: 0;
            border-radius: 0.5rem 0 0 0.5rem; /* Rounded left edge, sharp inner */
            /* Initial transform: Rotate left page slightly back. This will be animated by JS. */
            transform: rotateY( -20deg ); 
            z-index: 2;
        }

        .book-page-right {
            left: 50%;
            border-radius: 0 0.5rem 0.5rem 0; /* Rounded right edge, sharp inner */
            /* Initial transform: Rotate right page slightly back. This will be animated by JS. */
            transform: rotateY( 20deg ); 
            z-index: 2;
        }

        .book-spine {
            position: absolute;
            left: 50%;
            top: 0;
            width: 20px; /* Width of the spine */
            height: 100%;
            background-color: #16a34a; /* Green spine */
            transform: translateX(-50%);
            z-index: 3; /* Ensure spine is on top */
            border-radius: 0.25rem; /* Slight rounding for spine */
            box-shadow: 0 0 15px rgba(0,0,0,0.3);
        }
        
    </style>
</head>
<body>

    <!-- Scroll Indicator (Hidden until scroll) --><div id="scroll-indicator"></div>

    <!-- 1. Sticky Navigation Bar --><header id="navbar" class="sticky top-0 z-40 bg-white/80 backdrop-blur-md transition-all duration-300 shadow-sm">
        <nav class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-12 flex items-center justify-between text-sm">
            <a href="#" class="font-bold text-gray-800 text-lg">💰</a>
            <div class="hidden md:flex space-x-8">
                <a href="#hero" class="text-gray-600 hover:text-gray-900">Platform</a>
                <a href="#specs" class="text-gray-600 hover:text-gray-900">Learning Path</a>
                <a href="#grid" class="text-gray-600 hover:text-gray-900">Ecosystem</a>
                <a href="#deep-dive" class="text-gray-600 hover:text-gray-900">AI Mentor</a>
                <a href="#support" class="text-gray-600 hover:text-gray-900">B2B/Pricing</a>
            </div>
            <div class="flex items-center space-x-4">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-600 hover:text-gray-900 cursor-pointer" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110 0z" clip-rule="evenodd" />
                </svg>
                <button class="bg-green-600 text-white px-3 py-1.5 rounded-full text-xs font-semibold hover:bg-green-700 transition duration-150">Start Free Trial</button>
            </div>
        </nav>
    </header>

    <!-- 2. Hero Section (High-Impact, Dark Theme) --><section id="hero" class="min-h-[90vh] flex flex-col justify-center items-center text-center bg-gray-900 text-white p-4 relative overflow-hidden">
        
        <div class="max-w-4xl mx-auto mt-12 mb-16 hero-content">
            <h1 class="text-5xl sm:text-7xl lg:text-8xl font-extrabold tracking-tight mb-4">
                Financial Education. <span class="block text-green-400">Finally Accessible.</span>
            </h1>
            <p class="text-xl sm:text-2xl text-gray-300 mb-8 max-w-2xl mx-auto">
                Introducing the AI Mentor. A full-stack path from financial zero to confident wealth builder.
            </p>
            <div class="space-x-4">
                <a href="#specs" class="text-green-500 hover:text-green-400 font-semibold text-lg transition duration-150">
                    Explore the Platform &gt;
                </a>
                <a href="#" class="text-green-500 hover:text-green-400 font-semibold text-lg transition duration-150">
                    See Learning Paths &gt;
                </a>
            </div>
        </div>

        <!-- New Visual: The Physical Open Book (Animated by JS) --><div id="financial-book" class="w-full max-w-5xl h-64 sm:h-96 lg:h-[400px] bg-gray-700/50 flex items-center justify-center mb-8 hero-content">
            <!-- Book Spine --><div class="book-spine"></div>
            
            <!-- Left Page --><div class="book-page book-page-left">
                <p class="text-xl sm:text-2xl font-bold mb-4 text-green-700">Foundational Literacy</p>
                <ul class="list-disc ml-4 space-y-2 text-base text-left">
                    <li>Budgeting & Cash Flow</li>
                    <li>Debt Reduction Strategies</li>
                    <li>Credit Score Optimization</li>
                    <li>Building Emergency Funds</li>
                </ul>
                <span class="mt-auto pt-4 text-sm font-semibold text-gray-400">Chapter 1: Control Your Money</span>
            </div>
            
            <!-- Right Page --><div class="book-page book-page-right">
                <p class="text-xl sm:text-2xl font-bold mb-4 text-green-700">Wealth Acceleration</p>
                <ul class="list-disc ml-4 space-y-2 text-base text-left">
                    <li>Index Funds vs. Stocks</li>
                    <li>Tax-Advantaged Accounts</li>
                    <li>Real Estate Principles</li>
                    <li>Risk & Diversification</li>
                </ul>
                <span class="mt-auto pt-4 text-sm font-semibold text-gray-400">Chapter 2: Grow Your Wealth</span>
            </div>
        </div>
    </section>

    <!-- 3. Feature Section 1 (Video Integration for Global Impact) --><section id="specs" class="py-20 sm:py-32 text-center bg-white p-4">
        <div class="max-w-7xl mx-auto">
            <h2 class="text-4xl sm:text-6xl font-extrabold tracking-tight mb-4 text-gray-900">
                The Learning Path. Structured Clarity.
            </h2>
            <p class="text-lg sm:text-xl text-gray-600 mb-12 max-w-3xl mx-auto">
                Financial decisions happen in real-time, in every corner of the globe. See the worldly impact of true financial independence.
            </p>

            <!-- Video Block: Shibuya Crossing --><div class="w-full max-w-6xl mx-auto h-96 sm:h-[600px] rounded-[2.5rem] shadow-2xl overflow-hidden relative">
                
                <video class="absolute top-0 left-0 w-full h-full object-cover" 
                       autoplay loop muted playsinline 
                       poster="https://placehold.co/1280x720/000000/FFFFFF?text=Shibuya+Crossing+Video+Placeholder">
                    <!-- 
                        IMPORTANT: Replace 'https://example.com/videos/shibuya_crossing.mp4' with the actual
                        full HD video URL of the Shibuya crossing.
                        Due to environment restrictions, we cannot host the video file directly.
                    -->
                    <source src="(https://www.youtube.com/watch?v=ajOIjwATsrU)" type="video/mp4">
                    Your browser does not support the video tag.
                </video>
                
                <!-- Overlay text to maintain context and readability -->
                <div class="absolute inset-0 bg-black/30 flex items-center justify-center">
                    <p class="text-white text-4xl sm:text-5xl font-extrabold tracking-tight p-4">
                    </p>
                </div>
            </div>
        </div>
    </section>

    <!-- 4. Multi-Product Grid (Showcasing core pillars) --><section id="grid" class="py-20 sm:py-32 bg-gray-50 p-4">
        <div class="max-w-7xl mx-auto">
            <h2 class="text-4xl sm:text-6xl font-extrabold tracking-tight text-center mb-16 text-gray-900">
                Your Full-Stack Financial Ecosystem.
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">

                <!-- Card 1: AI Mentor --><div class="bg-white p-6 sm:p-10 rounded-3xl shadow-lg hover:shadow-xl transition duration-300">
                    <div class="h-48 mb-6 bg-green-100 rounded-xl flex items-center justify-center">
                        <svg class="h-12 w-12 text-green-600" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M7 3a1 1 0 000 2h6a1 1 0 100-2H7zM3 8h14a1 1 0 010 2H3a1 1 0 010-2zM4 15h12a1 1 0 100-2H4a1 1 0 100 2z"></path></svg>
                    </div>
                    <h3 class="text-2xl font-bold mb-2 text-gray-900">The AI Teacher & Coach</h3>
                    <p class="text-gray-600">The Mentor adapts to your goals, explaining complex concepts simply and guiding you step-by-step to mastery. It's truly personalized learning.</p>
                </div>

                <!-- Card 2: Ecosystem --><div class="bg-white p-6 sm:p-10 rounded-3xl shadow-lg hover:shadow-xl transition duration-300">
                    <div class="h-48 mb-6 bg-blue-100 rounded-xl flex items-center justify-center">
                        <svg class="h-12 w-12 text-blue-600" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M3 10a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z" clip-rule="evenodd"></path><path fill-rule="evenodd" d="M10 3a1 1 0 011 1v12a1 1 0 11-2 0V4a1 1 0 011-1z" clip-rule="evenodd"></path></svg>
                    </div>
                    <h3 class="text-2xl font-bold mb-2 text-gray-900">Real-World Mastery</h3>
                    <p class="text-gray-600">Premium courses, real market data integration, quizzes, and projects that simulate the financial decisions you'll face in life.</p>
                </div>

                <!-- Card 3: Confidence --><div class="bg-white p-6 sm:p-10 rounded-3xl shadow-lg hover:shadow-xl transition duration-300">
                    <div class="h-48 mb-6 bg-yellow-100 rounded-xl flex items-center justify-center">
                        <svg class="h-12 w-12 text-yellow-600" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M11 6a3 3 0 11-6 0 3 3 0 016 0zM7 14a6 6 0 00-2 4h10a6 6 0 00-2-4H7z"></path><path fill-rule="evenodd" d="M19 10a9 9 0 11-18 0 9 9 0 0118 0zm-9 3a7 7 0 100-14 7 7 0 000 14z" clip-rule="evenodd"></path></svg>
                    </div>
                    <h3 class="text-2xl font-bold mb-2 text-gray-900">Wealth Confidence</h3>
                    <p class="text-gray-600">Move from financial anxiety to absolute control. We empower a generation to understand money and make confident, long-term wealth decisions.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- 5. Intelligent Deep Dive (Gemini Feature: AI Mentor) --><section id="deep-dive" class="py-20 sm:py-32 text-center bg-gray-900 text-white p-4">
        <div class="max-w-4xl mx-auto">
            <h2 class="text-4xl sm:text-5xl font-extrabold tracking-tight mb-4 text-white">
                Ask the AI Mentor. 🧠
            </h2>
            <p class="text-lg text-gray-400 mb-12">
                Get instant, concise explanations on any financial concept. Your personal co-pilot is ready to analyze and guide.
            </p>

            <div class="space-y-4 text-left">
                <input type="text" id="deep-dive-query" placeholder="e.g., What is compounding interest and why is it important?"
                       class="w-full p-4 rounded-xl bg-gray-700 text-white placeholder-gray-400 border border-gray-600 focus:outline-none focus:ring-2 focus:ring-green-500">
                
                <button id="deep-dive-button"
                        class="w-full sm:w-auto bg-green-600 text-white font-semibold py-3 px-6 rounded-full hover:bg-green-700 transition duration-150 flex items-center justify-center space-x-2">
                    <span>Ask Your Financial Question 💬</span>
                </button>
                
                <div id="deep-dive-loading" class="hidden text-green-400 font-medium mt-4">
                    Searching and generating response...
                </div>
                
                <div id="deep-dive-result" class="mt-6 p-6 bg-gray-800 rounded-xl shadow-lg border border-gray-700 text-left">
                    <p id="deep-dive-text" class="text-gray-200 min-h-[3rem]">
                        Your personalized answer will appear here. Try asking about "Index Funds" or "Roth vs. Traditional IRA".
                    </p>
                    <div id="deep-dive-sources" class="mt-4 text-xs text-gray-500">
                        
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 6. Footer (Minimal and Functional) --><footer id="support" class="bg-gray-100 p-8 text-sm text-gray-500">
        <div class="max-w-7xl mx-auto border-t border-gray-200 pt-6">
            <p class="mb-4">
                *The AI Mentor provides educational insights and is not financial advice. Consult a professional advisor for personal planning.
            </p>
            <div class="flex flex-col sm:flex-row justify-between items-center">
                <p>&copy; 2025 Financial Mastery Platform. All rights reserved. | B2C & B2B Solutions Available.</p>
                <div class="flex space-x-4 mt-4 sm:mt-0">
                    <a href="#" class="hover:text-gray-700">Privacy Policy</a>
                    <a href="#" class="hover:text-gray-700">Terms of Use</a>
                    <a href="#" class="hover:text-gray-700">B2B/Enterprise</a>
                </div>
            </div>
        </div>
    </footer>

    <script>
        // JavaScript for Scroll Indicator and Navigation Bar Effects

        const navbar = document.getElementById('navbar');
        const scrollIndicator = document.getElementById('scroll-indicator');
        
        // References for the new scroll animation
        const financialBook = document.getElementById('financial-book');
        const leftPage = document.querySelector('.book-page-left');
        const rightPage = document.querySelector('.book-page-right');
        
        /**
         * Animates the 3D book visual based on the user's scroll position.
         */
        function animateBookOnScroll() {
            const scrollY = window.scrollY;
            
            // Define the range over which the animation occurs (e.g., the first 500px of scroll)
            const animationStart = 0;
            const animationEnd = 500; 

            // Calculate scroll progress (clamped between 0 and 1)
            let progress = Math.max(0, Math.min(1, (scrollY - animationStart) / (animationEnd - animationStart)));

            /* --- 1. Animate Page Rotation (Opening) --- */
            
            // Animate the left page rotation: from -20deg (initial) to -90deg (fully open)
            const startLeftRotation = -20;
            const endLeftRotation = -90;
            const currentLeftRotation = startLeftRotation + (progress * (endLeftRotation - startLeftRotation));
            leftPage.style.transform = `rotateY(${currentLeftRotation}deg)`;

            // Animate the right page rotation: from 20deg (initial) to 90deg (fully open)
            const startRightRotation = 20;
            const endRightRotation = 90;
            const currentRightRotation = startRightRotation + (progress * (endRightRotation - startRightRotation));
            rightPage.style.transform = `rotateY(${currentRightRotation}deg)`;
            
            /* --- 2. Animate Book Tilt (Flattening) --- */

            // Animate the entire book's tilt (rotateX) to make it lie flat as it opens
            const startBookRotationX = 15;
            const endBookRotationX = 0; // Flat
            const currentBookRotationX = startBookRotationX + (progress * (endBookRotationX - startBookRotationX));
            
            // Note: The perspective transform must be maintained
            financialBook.style.transform = `perspective(1500px) rotateX(${currentBookRotationX}deg) rotateY(0deg)`;
        }

        // Function to update the scroll indicator and sticky header appearance
        function updateScrollEffects() {
            const docHeight = document.documentElement.scrollHeight;
            const clientHeight = document.documentElement.clientHeight;
            const scrollableHeight = docHeight - clientHeight;
            const scrollTop = window.scrollY;

            // 1. Update Scroll Progress Indicator
            if (scrollableHeight > 0) {
                const scrollPercent = (scrollTop / scrollableHeight) * 100;
                scrollIndicator.style.width = scrollPercent + '%';
            }

            // 2. Change Nav Bar Appearance on scroll
            if (scrollTop > 50) {
                // If scrolled past 50px, add a subtle border/shadow and reduce opacity slightly
                navbar.classList.add('shadow-xl', 'bg-white/95');
                navbar.classList.remove('shadow-sm', 'bg-white/80');
            } else {
                // At the top, keep it minimal
                navbar.classList.remove('shadow-xl', 'bg-white/95');
                navbar.classList.add('shadow-sm', 'bg-white/80');
            }
            
            // 3. Animate the Book
            animateBookOnScroll();
        }

        // Initialize and listen for scroll events
        window.addEventListener('scroll', updateScrollEffects);
        
        // Ensure scroll indicator state is correct on page load
        window.addEventListener('load', () => {
            updateScrollEffects();
        });

        // --- Gemini API Integration: Intelligent Deep Dive Expert ---

        const apiKey = ""; // Canvas environment will provide the key
        const apiUrl = "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent";
        const MAX_RETRIES = 5;

        /**
         * Utility function to introduce delay.
         * @param {number} ms - Milliseconds to wait.
         */
        function sleep(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        }

        /**
         * Fetches data with exponential backoff for resilience against rate limiting.
         * @param {string} url - The API URL.
         * @param {object} options - Fetch options (method, headers, body).
         * @returns {Promise<Response>} The response object.
         */
        async function fetchWithExponentialBackoff(url, options) {
            for (let i = 0; i < MAX_RETRIES; i++) {
                try {
                    const response = await fetch(url, options);
                    if (response.status !== 429) { // 429 is Too Many Requests
                        return response;
                    }
                    // If 429, retry logic starts
                } catch (error) {
                    // Treat network/connection errors as needing a retry
                }

                if (i < MAX_RETRIES - 1) {
                    // Calculate delay: base delay (1000ms) * 2^i
                    const delay = 1000 * Math.pow(2, i);
                    await sleep(delay);
                }
            }
            throw new Error('API call failed after multiple retries.');
        }

        // Select DOM elements for the Deep Dive feature
        const queryInput = document.getElementById('deep-dive-query');
        const generateButton = document.getElementById('deep-dive-button');
        const loadingIndicator = document.getElementById('deep-dive-loading');
        const resultText = document.getElementById('deep-dive-text');
        const resultSources = document.getElementById('deep-dive-sources');

        /**
         * Calls the Gemini API to get a grounded, concise explanation.
         * @param {string} userQuery - The question about the product feature.
         */
        async function generateDeepDiveResponse(userQuery) {
            if (!userQuery.trim()) {
                resultText.textContent = "Please enter a question to get an expert insight.";
                return;
            }

            // Show loading state
            generateButton.disabled = true;
            loadingIndicator.classList.remove('hidden');
            resultText.textContent = '';
            resultSources.innerHTML = '';
            
            // System instruction updated for the AI Mentor persona
            const systemPrompt = "You are the 'AI Mentor' for a premium financial education platform. Provide a single, concise, and empowering paragraph (max 4 sentences) that clearly explains the financial concept or answers the user's query. Your tone should be friendly, highly trustworthy, and focused on simplicity for beginners.";

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                // Use Google Search grounding to find the answer
                tools: [{ "google_search": {} }],
                systemInstruction: {
                    parts: [{ text: systemPrompt }]
                },
                config: {
                    // Ensure the response is concise and high-quality
                    maxOutputTokens: 2048, 
                    temperature: 0.2
                }
            };
            
            const fetchUrl = `${apiUrl}?key=${apiKey}`;

            try {
                const response = await fetchWithExponentialBackoff(fetchUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }

                const result = await response.json();
                const candidate = result.candidates?.[0];

                if (candidate && candidate.content?.parts?.[0]?.text) {
                    const text = candidate.content.parts[0].text;
                    resultText.textContent = text;

                    // Extract and display grounding sources
                    let sources = [];
                    const groundingMetadata = candidate.groundingMetadata;
                    if (groundingMetadata && groundingMetadata.groundingAttributions) {
                        sources = groundingMetadata.groundingAttributions
                            .map(attribution => ({
                                uri: attribution.web?.uri,
                                title: attribution.web?.title,
                            }))
                            .filter(source => source.uri && source.title);
                    }

                    if (sources.length > 0) {
                        const sourceHtml = sources.map((s, index) => 
                            `<a href="${s.uri}" target="_blank" class="text-green-400 hover:text-green-300 underline">${s.title}</a>`
                        ).join(' | ');
                        resultSources.innerHTML = `<p class="font-medium text-gray-400 mt-2">Sources:</p>${sourceHtml}`;
                    } else {
                         resultSources.innerHTML = `<p class="font-medium text-gray-400 mt-2">Source: AI Knowledge Base.</p>`;
                    }

                } else {
                    resultText.textContent = "Error: Could not retrieve a meaningful response. Please try a different query.";
                }

            } catch (error) {
                console.error("Gemini API Request Failed:", error);
                resultText.textContent = `Sorry, an error occurred during the expert consultation. (${error.message})`;
            } finally {
                // Hide loading state and re-enable button
                generateButton.disabled = false;
                loadingIndicator.classList.add('hidden');
            }
        }

        // Event listener for the button click
        generateButton.addEventListener('click', () => {
            generateDeepDiveResponse(queryInput.value);
        });

        // Allow hitting Enter in the input field
        queryInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                e.preventDefault(); // Prevent default form submission behavior
                generateDeepDiveResponse(queryInput.value);
            }
        });

    </script>
</body>
</html>
