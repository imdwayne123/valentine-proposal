# valentine-proposal
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Will you be my Valentine?</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom font for a cute look */
        @import url('https://fonts.googleapis.com/css2?family=Fredoka:wght@400;600&display=swap');

        body {
            font-family: 'Fredoka', sans-serif;
            background-color: #ffe4e6; /* Rose 100 */
            overflow: hidden; /* Prevent scrolling when button moves */
        }

        .btn-transition {
            transition: all 0.3s ease;
        }

        /* Float animation for the cute images */
        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
            100% { transform: translateY(0px); }
        }

        .floating {
            animation: float 3s ease-in-out infinite;
        }

        /* Heartbeat animation for the Yes button hover */
        @keyframes heartbeat {
            0% { transform: scale(1); }
            25% { transform: scale(1.1); }
            50% { transform: scale(1); }
            75% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        .heartbeat:hover {
            animation: heartbeat 1s infinite;
        }
    </style>
</head>
<body class="h-screen w-screen flex flex-col justify-center items-center relative">

    <!-- Proposal Container -->
    <div id="proposal-container" class="text-center z-10 flex flex-col items-center gap-6 p-4">
        
        <!-- Cute Initial Image (Peach & Goma Cat or similar cute bear) -->
        <img src="https://media1.tenor.com/m/1k5sT6A6y48AAAAC/cute-cat.gif" 
             alt="Cute Cat Asking" 
             class="w-48 h-48 object-cover rounded-xl floating mb-4 drop-shadow-xl"
             onerror="this.src='https://placekitten.com/200/200'" />

        <h1 class="text-4xl md:text-5xl font-bold text-rose-600 drop-shadow-sm px-4">
            POKLOO Will you be my Valentines?
        </h1>

        <div class="flex gap-8 mt-8 relative w-full justify-center">
            <!-- Yes Button -->
            <button onclick="acceptProposal()" 
                    class="bg-green-500 hover:bg-green-600 text-white font-bold py-3 px-8 rounded-full text-xl shadow-lg transform transition-transform duration-200 hover:scale-125 hover:rotate-3">
                Yes! ❤️
            </button>

            <!-- No Button -->
            <!-- We use absolute positioning on hover logic via JS to make it jump -->
            <button id="no-btn" 
                    onmouseover="moveButton()" 
                    ontouchstart="moveButton()"
                    class="bg-rose-400 text-white font-bold py-3 px-8 rounded-full text-xl shadow-lg transition-all duration-100 ease-out absolute"
                    style="position: relative;">
                No
            </button>
        </div>
    </div>

    <!-- Celebration Container (Hidden by default) -->
    <div id="celebration-container" class="hidden text-center z-20 flex flex-col items-center gap-6 p-4 animate-fade-in">
        
        <!-- Celebration GIF -->
        <img src="https://media1.tenor.com/m/gUiu1zyxfzYAAAAC/bear-kiss-bear-kisses.gif" 
             alt="Celebration Kiss" 
             class="w-64 h-64 object-cover rounded-xl drop-shadow-2xl floating mb-4" />

        <h1 class="text-5xl md:text-6xl font-bold text-rose-600 mb-2">
            YAYYYY! 🎉
        </h1>
        <p class="text-2xl text-rose-800 font-semibold">
            Can't wait for our date, Pokloo! 💖
        </p>
        
        <button onclick="location.reload()" class="mt-8 text-rose-400 underline hover:text-rose-600 text-sm">
            Replay
        </button>
    </div>

    <!-- Floating Hearts Background (Purely Decorative) -->
    <div id="hearts-bg" class="absolute inset-0 pointer-events-none overflow-hidden z-0 opacity-50">
        <!-- JS will populate this with SVGs -->
    </div>

    <script>
        const noBtn = document.getElementById('no-btn');
        const proposalContainer = document.getElementById('proposal-container');
        const celebrationContainer = document.getElementById('celebration-container');
        const heartsBg = document.getElementById('hearts-bg');

        // Initial setup to ensure No button behaves relatively at first, then absolute when moving
        let hasMoved = false;

        function moveButton() {
            // Get viewport dimensions
            const maxX = window.innerWidth - noBtn.offsetWidth - 50; // buffer
            const maxY = window.innerHeight - noBtn.offsetHeight - 50;
            
            // Calculate random positions
            const randomX = Math.random() * maxX;
            const randomY = Math.random() * maxY;

            // Apply new position
            // We switch to fixed positioning to allow it to roam the whole screen freely
            noBtn.style.position = 'fixed';
            noBtn.style.left = randomX + 'px';
            noBtn.style.top = randomY + 'px';
            
            // Add a little rotation for chaos
            const randomRot = (Math.random() - 0.5) * 40;
            noBtn.style.transform = `rotate(${randomRot}deg)`;

            hasMoved = true;
        }

        function acceptProposal() {
            // Hide proposal, show celebration
            proposalContainer.style.display = 'none';
            celebrationContainer.classList.remove('hidden');
            celebrationContainer.style.display = 'flex'; // Ensure flex layout is applied
            
            // Trigger confetti effect
            createConfetti();
        }

        // Simple Background Hearts Generator
        function createBackgroundHearts() {
            const colors = ['#f43f5e', '#fb7185', '#fda4af']; // Tailwind Rose colors
            
            for (let i = 0; i < 20; i++) {
                const heart = document.createElement('div');
                heart.innerHTML = `
                    <svg fill="${colors[Math.floor(Math.random() * colors.length)]}" viewBox="0 0 24 24" class="w-full h-full">
                        <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/>
                    </svg>
                `;
                heart.className = 'absolute opacity-20 animate-pulse';
                
                // Random Size
                const size = Math.random() * 50 + 20;
                heart.style.width = `${size}px`;
                heart.style.height = `${size}px`;
                
                // Random Position
                heart.style.left = `${Math.random() * 100}vw`;
                heart.style.top = `${Math.random() * 100}vh`;
                
                // Random Animation Delay
                heart.style.animationDelay = `${Math.random() * 2}s`;
                heart.style.animationDuration = `${Math.random() * 3 + 2}s`;

                heartsBg.appendChild(heart);
            }
        }

        // Confetti effect for "Yes" click
        function createConfetti() {
            for (let i = 0; i < 50; i++) {
                const confetti = document.createElement('div');
                confetti.style.position = 'fixed';
                confetti.style.width = '10px';
                confetti.style.height = '10px';
                confetti.style.backgroundColor = ['#ff0', '#f00', '#0f0', '#00f', '#f0f'][Math.floor(Math.random() * 5)];
                confetti.style.left = '50%';
                confetti.style.top = '50%';
                confetti.style.borderRadius = '50%';
                confetti.style.zIndex = '100';
                document.body.appendChild(confetti);

                // Animate outward
                const angle = Math.random() * Math.PI * 2;
                const velocity = Math.random() * 300 + 100;
                const tx = Math.cos(angle) * velocity;
                const ty = Math.sin(angle) * velocity;

                confetti.animate([
                    { transform: 'translate(0, 0) scale(1)', opacity: 1 },
                    { transform: `translate(${tx}px, ${ty}px) scale(0)`, opacity: 0 }
                ], {
                    duration: 1000 + Math.random() * 1000,
                    easing: 'cubic-bezier(0, .9, .57, 1)',
                    fill: 'forwards'
                });
            }
        }

        // Init background
        createBackgroundHearts();

    </script>
</body>
</html>
