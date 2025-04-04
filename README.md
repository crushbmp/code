# code

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Confucius Wisdom</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;700&display=swap');
        
        body {
            font-family: 'Noto Serif SC', serif;
            background-color: #f8f4e9;
            transition: background-color 0.5s ease;
        }
        
        .container {
            max-width: 600px;
        }
        
        .quote-container {
            min-height: 300px;
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.8s ease, transform 0.8s ease;
        }
        
        .quote-container.show {
            opacity: 1;
            transform: translateY(0);
        }
        
        .chinese-character {
            font-size: 3rem;
            line-height: 1;
            color: #8b5a2b;
        }
        
        .fade-in {
            animation: fadeIn 1.5s ease-in-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        .bg-paper {
            background-image: url('data:image/svg+xml;utf8,<svg width="100" height="100" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><path d="M0 0h100v100H0z" fill="none"/><path d="M20 20h60v60H20z" fill="none" stroke="%238b5a2b" stroke-width="0.5" stroke-dasharray="2,2"/></svg>');
            background-size: 40px 40px;
        }
    </style>
</head>
<body class="min-h-screen flex items-center justify-center p-4">
    <div class="container mx-auto text-center">
        <div class="bg-white rounded-lg shadow-lg overflow-hidden bg-paper p-8">
            <div class="mb-6">
                <div class="chinese-character mb-2">孔</div>
                <h1 class="text-2xl font-bold text-gray-800">Confucius Wisdom</h1>
                <p class="text-gray-600">Daily teachings from the great philosopher</p>
            </div>
            
            <div class="quote-container flex flex-col items-center justify-center p-6 border border-gray-200 rounded-lg mb-6 bg-white/80">
                <i class="fas fa-quote-left text-gray-400 mb-4 text-xl"></i>
                <p id="quote-text" class="text-lg text-gray-800 mb-4 italic leading-relaxed"></p>
                <p id="quote-date" class="text-sm text-gray-500 mt-4"></p>
                <div id="chinese-translation" class="mt-4 text-gray-700 text-sm"></div>
            </div>
            
            <div class="flex justify-center space-x-4">
                <button id="new-quote" class="px-4 py-2 bg-amber-800 text-white rounded hover:bg-amber-700 transition flex items-center">
                    <i class="fas fa-sync-alt mr-2"></i> New Quote
                </button>
                <button id="share-btn" class="px-4 py-2 bg-gray-200 text-gray-700 rounded hover:bg-gray-300 transition flex items-center">
                    <i class="fas fa-share-alt mr-2"></i> Share
                </button>
            </div>
        </div>
        
        <div class="mt-6 text-xs text-gray-500">
            <p>Wisdom grows from reflection</p>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const quotes = [
                {
                    text: "It does not matter how slowly you go as long as you do not stop.",
                    chinese: "不患无位，患所以立。不患莫己知，求为可知也。",
                    date: ""
                },
                {
                    text: "Our greatest glory is not in never falling, but in rising every time we fall.",
                    chinese: "不患人之不己知，患不知人也。",
                    date: ""
                },
                {
                    text: "Everything has beauty, but not everyone sees it.",
                    chinese: "君子成人之美，不成人之恶。小人反是。",
                    date: ""
                },
                {
                    text: "The man who moves a mountain begins by carrying away small stones.",
                    chinese: "不积跬步，无以至千里；不积小流，无以成江海。",
                    date: ""
                },
                {
                    text: "When anger rises, think of the consequences.",
                    chinese: "小不忍，则乱大谋。",
                    date: ""
                },
                {
                    text: "The will to win, the desire to succeed, the urge to reach your full potential... these are the keys that will unlock the door to personal excellence.",
                    chinese: "志于道，据于德，依于仁，游于艺。",
                    date: ""
                },
                {
                    text: "Respect yourself and others will respect you.",
                    chinese: "己所不欲，勿施于人。",
                    date: ""
                },
                {
                    text: "The superior man is modest in his speech but exceeds in his actions.",
                    chinese: "君子欲讷于言而敏于行。",
                    date: ""
                },
                {
                    text: "To see what is right and not do it is the worst cowardice.",
                    chinese: "见义不为，无勇也。",
                    date: ""
                },
                {
                    text: "Learning without thought is labor lost; thought without learning is perilous.",
                    chinese: "学而不思则罔，思而不学则殆。",
                    date: ""
                }
            ];
            
            // Get today's date for consistent daily quote
            const today = new Date();
            const dayOfYear = Math.floor((today - new Date(today.getFullYear(), 0, 0)) / (1000 * 60 * 60 * 24));
            const quoteIndex = dayOfYear % quotes.length;
            let currentQuote = quotes[quoteIndex];
            
            // Format date
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            currentQuote.date = today.toLocaleDateString('en-US', options);
            
            // Display quote
            const quoteText = document.getElementById('quote-text');
            const quoteDate = document.getElementById('quote-date');
            const chineseTranslation = document.getElementById('chinese-translation');
            const quoteContainer = document.querySelector('.quote-container');
            
            function displayQuote() {
                quoteText.textContent = currentQuote.text;
                quoteDate.textContent = currentQuote.date;
                chineseTranslation.textContent = currentQuote.chinese;
                
                // Show with animation
                setTimeout(() => {
                    quoteContainer.classList.add('show');
                }, 100);
            }
            
            // New quote button
            document.getElementById('new-quote').addEventListener('click', function() {
                quoteContainer.classList.remove('show');
                
                setTimeout(() => {
                    const randomIndex = Math.floor(Math.random() * quotes.length);
                    currentQuote = quotes[randomIndex];
                    currentQuote.date = "Today's Reflection";
                    displayQuote();
                }, 300);
            });
            
            // Share button
            document.getElementById('share-btn').addEventListener('click', function() {
                if (navigator.share) {
                    navigator.share({
                        title: 'Confucius Wisdom',
                        text: `"${currentQuote.text}" - Confucius`,
                        url: window.location.href
                    }).catch(err => {
                        console.log('Error sharing:', err);
                    });
                } else {
                    // Fallback for browsers that don't support Web Share API
                    const shareText = `"${currentQuote.text}" - Confucius\n\n${window.location.href}`;
                    prompt('Copy this quote to share:', shareText);
                }
            });
            
            // Initial display
            displayQuote();
            
            // Change background color based on time of day
            const hour = today.getHours();
            if (hour >= 6 && hour < 12) {
                document.body.style.backgroundColor = '#f8f4e9'; // Morning - light beige
            } else if (hour >= 12 && hour < 18) {
                document.body.style.backgroundColor = '#f0e6d2'; // Afternoon - warm beige
            } else {
                document.body.style.backgroundColor = '#e8d8b5'; // Evening - darker beige
            }
        });
    </script>
</body>
</html>
