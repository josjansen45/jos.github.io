# jos.github.io
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Infinity Pro - The Future of Computing</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
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
            background: linear-gradient(90deg, #157aff, #3c49ff);
            width: 0;
            position: fixed;
            top: 0;
            left: 0;
            z-index: 50;
        }
        /* Ensure images scale nicely and have rounded corners */
        .product-image {
            width: 100%;
            height: auto;
            object-fit: contain;
            border-radius: 1.5rem; /* Large rounded corners */
        }
    </style>
</head>
<body>

    <!-- Scroll Indicator (Hidden until scroll) -->
    <div id="scroll-indicator"></div>

    <!-- 1. Sticky Navigation Bar -->
    <header id="navbar" class="sticky top-0 z-40 bg-white/80 backdrop-blur-md transition-all duration-300 shadow-sm">
        <nav class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-12 flex items-center justify-between text-sm">
            <a href="#" class="font-bold text-gray-800 text-lg"></a>
            <div class="hidden md:flex space-x-8">
                <a href="#hero" class="text-gray-600 hover:text-gray-900">Store</a>
                <a href="#specs" class="text-gray-600 hover:text-gray-900">Infinity Pro</a>
                <a href="#grid" class="text-gray-600 hover:text-gray-900">Vision</a>
                <a href="#deep-dive" class="text-gray-600 hover:text-gray-900">Expert Q&A</a>
                <a href="#support" class="text-gray-600 hover:text-gray-900">Support</a>
            </div>
            <div class="flex items-center space-x-4">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-600 hover:text-gray-900 cursor-pointer" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 1110 0z" clip-rule="evenodd" />
                </svg>
                <button class="bg-blue-600 text-white px-3 py-1.5 rounded-full text-xs font-semibold hover:bg-blue-700 transition duration-150">Buy</button>
            </div>
        </nav>
    </header>

    <!-- 2. Hero Section (High-Impact, Dark Theme) -->
    <section id="hero" class="min-h-[90vh] flex flex-col justify-center items-center text-center bg-black text-white p-4">
        <div class="max-w-4xl mx-auto mt-12 mb-16">
            <h1 class="text-5xl sm:text-7xl lg:text-8xl font-extrabold tracking-tight mb-4">
                Infinity Pro. <span class="block text-gray-400">Beyond Imagination.</span>
            </h1>
            <p class="text-xl sm:text-2xl text-gray-300 mb-8 max-w-2xl mx-auto">
                Introducing the revolutionary M3 Neural Chip. A leap into the future of personal computing power.
            </p>
            <div class="space-x-4">
                <a href="#specs" class="text-blue-500 hover:text-blue-400 font-semibold text-lg transition duration-150">
                    Learn More &gt;
                </a>
                <a href="#" class="text-blue-500 hover:text-blue-400 font-semibold text-lg transition duration-150">
                    See Pricing &gt;
                </a>
            </div>
        </div>

        <!-- Placeholder for the main product image (use a simple colored background for minimalism) -->
        <div class="w-full max-w-5xl h-64 sm:h-96 lg:h-[600px] bg-gray-900 rounded-3xl flex items-center justify-center shadow-2xl mb-8 overflow-hidden">
            <div class="text-gray-500 text-2xl font-light">
                [High-Res Product Shot Placeholder]
            </div>
        </div>
    </section>

    <!-- 3. Feature Section 1 (Minimal Text, Centered Visual) -->
    <section id="specs" class="py-20 sm:py-32 text-center bg-white p-4">
        <div class="max-w-7xl mx-auto">
            <h2 class="text-4xl sm:text-6xl font-extrabold tracking-tight mb-4 text-gray-900">
                The Display. A new dimension of color.
            </h2>
            <p class="text-lg sm:text-xl text-gray-600 mb-12 max-w-3xl mx-auto">
                Liquid Retina XDR. With ProMotion technology, everything is faster, smoother, and incredibly vibrant.
            </p>

            <!-- Placeholder for a detailed feature visual -->
            <div class="w-full max-w-6xl mx-auto h-96 sm:h-[600px] bg-gradient-to-tr from-cyan-400 to-blue-500 rounded-[2.5rem] flex items-center justify-center shadow-xl overflow-hidden">
                <div class="text-white text-3xl font-light">
                    [Display Feature Visual Placeholder]
                </div>
            </div>
        </div>
    </section>

    <!-- 4. Multi-Product Grid (Showcasing related products or features) -->
    <section id="grid" class="py-20 sm:py-32 bg-gray-50 p-4">
        <div class="max-w-7xl mx-auto">
            <h2 class="text-4xl sm:text-6xl font-extrabold tracking-tight text-center mb-16 text-gray-900">
                A connected ecosystem.
            </h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">

                <!-- Card 1: Power -->
                <div class="bg-white p-6 sm:p-10 rounded-3xl shadow-lg hover:shadow-xl transition duration-300">
                    <div class="h-48 mb-6 bg-red-100 rounded-xl flex items-center justify-center">
                        <svg class="h-12 w-12 text-red-600" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M11 3.055A9.001 9.001 0 1020 12h-8V3.055zM12 2A10 10 0 002 12h10V2z" clip-rule="evenodd"></path></svg>
                    </div>
                    <h3 class="text-2xl font-bold mb-2 text-gray-900">Unbelievable Speed</h3>
                    <p class="text-gray-600">The M3 chip redefines processing speed, handling massive workloads with ease and unmatched efficiency.</p>
                </div>

                <!-- Card 2: Design -->
                <div class="bg-white p-6 sm:p-10 rounded-3xl shadow-lg hover:shadow-xl transition duration-300">
                    <div class="h-48 mb-6 bg-green-100 rounded-xl flex items-center justify-center">
                        <svg class="h-12 w-12 text-green-600" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M17.414 2.586a2 2 0 00-2.828 0L7 10.172V13h2.828l7.586-7.586a2 2 0 000-2.828z"></path><path fill-rule="evenodd" d="M2 6a2 2 0 012-2h4a1 1 0 010 2H4v10h10v-4a1 1 0 112 0v4a2 2 0 01-2 2H4a2 2 0 01-2-2V6z" clip-rule="evenodd"></path></svg>
                    </div>
                    <h3 class="text-2xl font-bold mb-2 text-gray-900">Masterful Craftsmanship</h3>
                    <p class="text-gray-600">Forged from aerospace-grade aluminum, the chassis is both incredibly light and resilient, finished with precision.</p>
                </div>

                <!-- Card 3: Battery -->
                <div class="bg-white p-6 sm:p-10 rounded-3xl shadow-lg hover:shadow-xl transition duration-300">
                    <div class="h-48 mb-6 bg-purple-100 rounded-xl flex items-center justify-center">
                        <svg class="h-12 w-12 text-purple-600" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M3 5a2 2 0 012-2h10a2 2 0 012 2v10a2 2 0 01-2 2H5a2 2 0 01-2-2V5zm1 7a1 1 0 011-1h6a1 1 0 110 2H5a1 1 0 01-1-1z" clip-rule="evenodd"></path></svg>
                    </div>
                    <h3 class="text-2xl font-bold mb-2 text-gray-900">All-Day Energy</h3>
                    <p class="text-gray-600">Optimized power consumption gives you up to 18 hours of video playback or 15 hours of web browsing.</p>
                </div>
            </div>
        </div>
    </section>

    <!-- 5. Intelligent Deep Dive (Gemini Feature) -->
    <section id="deep-dive" class="py-20 sm:py-32 text-center bg-gray-900 text-white p-4">
        <div class="max-w-4xl mx-auto">
            <h2 class="text-4xl sm:text-5xl font-extrabold tracking-tight mb-4 text-white">
                Ask Infinity. ✨
            </h2>
            <p class="text-lg text-gray-400 mb-12">
                Want to know more? Our integrated AI expert can provide grounded, concise explanations on any feature.
            </p>

            <div class="space-y-4 text-left">
                <input type="text" id="deep-dive-query" placeholder="e.g., Explain the M3 Neural Chip in detail."
                       class="w-full p-4 rounded-xl bg-gray-700 text-white placeholder-gray-400 border border-gray-600 focus:outline-none focus:ring-2 focus:ring-blue-500">
                
                <button id="deep-dive-button"
                        class="w-full sm:w-auto bg-blue-600 text-white font-semibold py-3 px-6 rounded-full hover:bg-blue-700 transition duration-150 flex items-center justify-center space-x-2">
                    <span>Generate Expert Insight ✨</span>
                </button>
                
                <div id="deep-dive-loading" class="hidden text-blue-400 font-medium mt-4">
                    Searching and generating response...
                </div>
                
                <div id="deep-dive-result" class="mt-6 p-6 bg-gray-800 rounded-xl shadow-lg border border-gray-700 text-left">
                    <p id="deep-dive-text" class="text-gray-200 min-h-[3rem]">
                        Your personalized answer will appear here. Try asking about "ProMotion" or "Aerospace-grade aluminum".
                    </p>
                    <div id="deep-dive-sources" class="mt-4 text-xs text-gray-500">
                        
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- 6. Footer (Minimal and Functional) -->
    <footer id="support" class="bg-gray-100 p-8 text-sm text-gray-500">
        <div class="max-w-7xl mx-auto border-t border-gray-200 pt-6">
            <p class="mb-4">
                *The Infinity Pro is designed for professionals and is available in Silver and Space Black. All battery claims depend on device settings and usage.
            </p>
            <div class="flex flex-col sm:flex-row justify-between items-center">
                <p>&copy; 2025 Infinity Tech. All rights reserved.</p>
                <div class="flex space-x-4 mt-4 sm:mt-0">
                    <a href="#" class="hover:text-gray-700">Privacy Policy</a>
                    <a href="#" class="hover:text-gray-700">Terms of Use</a>
                    <a href="#" class="hover:text-gray-700">Sales and Refunds</a>
                </div>
            </div>
        </div>
    </footer>

    <script>
        // JavaScript for Scroll Indicator and Navigation Bar Effects

        const navbar = document.getElementById('navbar');
        const scrollIndicator = document.getElementById('scroll-indicator');

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
        }

        // Initialize and listen for scroll events
        window.addEventListener('scroll', updateScrollEffects);
        
        // Run once on load to set initial state
        window.onload = updateScrollEffects;


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
            
            // System instruction to guide the model's persona (Apple-style expert)
            const systemPrompt = "You are a world-class Product Expert for Infinity Tech. Provide a single, concise, and enthusiastic paragraph (max 4 sentences) that answers the user's query about the 'Infinity Pro' product using grounded, up-to-date information. Use sophisticated, yet clear, marketing language.";

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
                            `<a href="${s.uri}" target="_blank" class="text-blue-400 hover:text-blue-300 underline">${s.title}</a>`
                        ).join(' | ');
                        resultSources.innerHTML = `<p class="font-medium text-gray-400 mt-2">Sources:</p>${sourceHtml}`;
                    } else {
                         resultSources.innerHTML = `<p class="font-medium text-gray-400 mt-2">Source: Expert knowledge base.</p>`;
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
