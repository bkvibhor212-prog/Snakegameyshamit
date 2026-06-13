
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Retro Snake Arcade</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Plus+Jakarta+Sans:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #030712;
        }
        .retro-font {
            font-family: 'Orbitron', sans-serif;
        }
        .neon-border {
            box-shadow: 0 0 15px rgba(16, 185, 129, 0.2), inset 0 0 15px rgba(16, 185, 129, 0.1);
        }
        .neon-glow-red {
            box-shadow: 0 0 15px rgba(239, 68, 68, 0.4);
        }
        .neon-glow-emerald {
            box-shadow: 0 0 15px rgba(16, 185, 129, 0.4);
        }
        .neon-glow-amber {
            box-shadow: 0 0 15px rgba(245, 158, 11, 0.4);
        }
        /* Custom scrollbar for game rules log */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #111827;
        }
        ::-webkit-scrollbar-thumb {
            background: #374151;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #4b5563;
        }
    </style>
</head>
<body class="min-h-screen text-slate-100 flex flex-col justify-between overflow-x-hidden">
    <!-- HEADER -->
    <header class="bg-gray-950/80 border-b border-gray-800 backdrop-blur-md px-4 py-3 sticky top-0 z-40">
        <div class="max-w-5xl mx-auto flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="bg-emerald-500 text-slate-950 p-2 rounded-xl shadow-lg shadow-emerald-500/20 flex items-center justify-center animate-pulse">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M13 10V3L4 14h7v7l9-11h-7z" />
                    </svg>
                </div>
                <div>
                    <h1 class="retro-font text-lg md:text-xl font-extrabold tracking-wider bg-gradient-to-r from-emerald-400 via-teal-400 to-indigo-400 bg-clip-text text-transparent">
                        NEON VIPER
                    </h1>
                    <p class="text-[10px] text-gray-500 font-bold tracking-widest uppercase">Classic Arcade v2.5</p>
                </div>
            </div>
            <!-- AUDIO & CONTROLS CONFIG -->
            <div class="flex items-center gap-3">
                <button id="soundToggleBtn" class="bg-gray-900 hover:bg-gray-800 border border-gray-800 p-2 rounded-xl text-emerald-400 transition-all flex items-center gap-1.5" title="Toggle Sound FX">
                    <svg id="soundOnIcon" class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                    </svg>
                    <svg id="soundOffIcon" class="w-5 h-5 hidden text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2" />
                    </svg>
                    <span class="text-xs hidden sm:inline font-bold">Audio</span>
                </button>
            </div>
        </div>
    </header>
    <!-- MAIN ARCADE BODY -->
    <main class="flex-1 max-w-5xl w-full mx-auto p-4 flex flex-col lg:flex-row gap-6 items-stretch justify-center">  
        <!-- COLUMN 1: CONTROLS, SCORES & SELECTIONS -->
        <section class="w-full lg:w-80 flex flex-col gap-4">         
            <!-- SCORES & STATUS -->
            <div class="bg-gray-900/60 border border-gray-800 rounded-2xl p-4 flex flex-row lg:flex-col justify-between gap-4">
                <div class="flex-1">
                    <span class="text-xs font-semibold text-gray-500 uppercase tracking-widest block">Session Score</span>
                    <div id="currentScore" class="retro-font text-3xl md:text-4xl font-black text-emerald-400 mt-1">0000</div>
                </div>
                <div class="flex-1 border-l lg:border-l-0 lg:border-t border-gray-800 pl-4 lg:pl-0 lg:pt-4">
                    <span class="text-xs font-semibold text-gray-500 uppercase tracking-widest block">All-Time High</span>
                    <div id="highScore" class="retro-font text-3xl md:text-4xl font-black text-teal-400 mt-1">0000</div>
                </div>
            </div>
            <!-- GAME MODE SELECTOR -->
            <div class="bg-gray-900/60 border border-gray-800 rounded-2xl p-4 space-y-3">
                <span class="text-xs font-bold text-gray-400 uppercase tracking-wider block">Select Arena Mode</span>
                <div class="grid grid-cols-3 lg:grid-cols-1 gap-2">
                    <button id="modeClassic" class="mode-btn bg-emerald-500/10 border border-emerald-500/40 text-emerald-400 text-xs font-bold py-2.5 px-3 rounded-xl transition-all hover:bg-emerald-500/20">
                        🐍 Classic
                    </button>
                    <button id="modeObstacles" class="mode-btn bg-gray-950 border border-gray-800 text-gray-400 text-xs font-bold py-2.5 px-3 rounded-xl transition-all hover:border-gray-700 hover:text-gray-200">
                        🚧 Obstacles
                    </button>
                    <button id="modeSpeed" class="mode-btn bg-gray-950 border border-gray-800 text-gray-400 text-xs font-bold py-2.5 px-3 rounded-xl transition-all hover:border-gray-700 hover:text-gray-200">
                        ⚡ Speed Run
                    </button>
                </div>
            </div>

            <!-- POWER-UP LEGEND -->
            <div class="bg-gray-900/60 border border-gray-800 rounded-2xl p-4 space-y-3 flex-1 hidden lg:block">
                <span class="text-xs font-bold text-gray-400 uppercase tracking-wider block">Arcade Almanac</span>
                <div class="space-y-2.5 text-xs">
                    <div class="flex items-start gap-2.5">
                        <span class="text-base">🍎</span>
                        <div>
                            <p class="font-bold text-slate-200">Apple</p>
                            <p class="text-[10px] text-gray-500">Standard dietary fuel. Adds length & points.</p>
                        </div>
                    </div>
                    <div class="flex items-start gap-2.5">
                        <span class="text-base">⭐</span>
                        <div>
                            <p class="font-bold text-amber-400">Golden Apple</p>
                            <p class="text-[10px] text-gray-500">Double points & temporary snake speed boosts.</p>
                        </div>
                    </div>
                    <div class="flex items-start gap-2.5">
                        <span class="text-base">🛡️</span>
                        <div>
                            <p class="font-bold text-indigo-400">Shield Melon</p>
                            <p class="text-[10px] text-gray-500">Provides 8 seconds of crash/boundary immunity.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- COLUMN 2: MAIN ARCADE CANVAS & HUD -->
        <section class="flex-1 flex flex-col gap-4">
            
            <!-- GAME SCREEN HUD / PROGRESS BAR -->
            <div class="bg-gray-900/40 border border-gray-800/80 rounded-2xl px-4 py-3 flex items-center justify-between text-xs font-semibold text-gray-400">
                <div class="flex items-center gap-2">
                    <span class="w-2.5 h-2.5 rounded-full bg-emerald-500 animate-ping"></span>
                    <span id="activeModeLabel" class="uppercase text-emerald-400 font-bold tracking-widest text-[11px]">Classic Arena</span>
                </div>
                <div class="flex items-center gap-4">
                    <div>Level: <span id="currentLevel" class="text-emerald-400 font-bold">1</span></div>
                    <div id="shieldStatus" class="hidden text-indigo-400 font-bold animate-pulse">🛡️ Shield Active: <span id="shieldTimer">0</span>s</div>
                </div>
            </div>

            <!-- THE CANVAS WRAPPER -->
            <div class="relative flex-1 min-h-[320px] md:min-h-[440px] bg-gray-950 rounded-3xl overflow-hidden border-2 border-gray-800 shadow-2xl flex items-center justify-center p-2">
                
                <!-- Canvas Element -->
                <canvas id="gameCanvas" class="max-w-full max-h-full block rounded-2xl select-none outline-none"></canvas>

                <!-- GAME OVER OVERLAY -->
                <div id="gameOverScreen" class="absolute inset-0 bg-gray-950/90 backdrop-blur-sm hidden flex-col items-center justify-center p-6 text-center z-20">
                    <div class="animate-bounce mb-4 bg-red-500/10 text-red-500 p-4 rounded-full border border-red-500/20">
                        <svg class="w-12 h-12" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
                        </svg>
                    </div>
                    <h2 class="retro-font text-3xl font-black tracking-widest text-red-500 mb-1">CRASH DETECTED</h2>
                    <p class="text-xs text-gray-400 uppercase tracking-widest mb-6">Simulation Terminated</p>

                    <div class="bg-gray-900 border border-gray-800 rounded-2xl p-4 w-full max-w-xs mb-6 space-y-1">
                        <div class="flex justify-between text-xs">
                            <span class="text-gray-500">Final Score:</span>
                            <span id="finalScore" class="font-bold text-slate-100">0000</span>
                        </div>
                        <div class="flex justify-between text-xs">
                            <span class="text-gray-500">Apples Eaten:</span>
                            <span id="finalApples" class="font-bold text-emerald-400">0</span>
                        </div>
                    </div>

                    <button id="restartBtn" class="bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-black px-8 py-3 rounded-xl shadow-lg shadow-emerald-500/20 transition-all flex items-center gap-2 transform active:scale-95">
                        <svg class="w-5 h-5 fill-current" viewBox="0 0 20 20"><path d="M4 4a2 2 0 012-2h4.586A2 2 0 0112 2.586L15.414 6A2 2 0 0116 7.414V16a2 2 0 01-2 2H6a2 2 0 01-2-2V4z"></path></svg>
                        Play Again
                    </button>
                </div>

                <!-- START / PAUSE OVERLAY -->
                <div id="pauseScreen" class="absolute inset-0 bg-gray-950/80 backdrop-blur-sm flex flex-col items-center justify-center p-6 text-center z-20">
                    <div class="mb-4 bg-emerald-500/10 text-emerald-400 p-4 rounded-full border border-emerald-500/20">
                        <svg class="w-12 h-12" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.1" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" />
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.1" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                    </div>
                    <h2 class="retro-font text-2xl font-bold tracking-widest text-slate-100 mb-2">NEON VIPER ARCADE</h2>
                    <p class="text-xs text-gray-500 max-w-sm mb-6 leading-relaxed">
                        Navigate using keyboard arrow keys, WASD, or swipe the game screen. Avoid walls and your own tail. Eat special power-ups to unlock special abilities!
                    </p>
                    <button id="startBtn" class="bg-emerald-500 hover:bg-emerald-400 text-slate-950 font-black px-8 py-3 rounded-xl shadow-lg shadow-emerald-500/20 transition-all flex items-center gap-2 transform active:scale-95 text-sm uppercase tracking-wider">
                        Power Up Engine
                    </button>
                </div>
            </div>

            <!-- MOBILE DIRECTION CONTROLS -->
            <div class="block md:hidden bg-gray-900/60 border border-gray-800 p-4 rounded-3xl">
                <div class="grid grid-cols-3 gap-2 max-w-[200px] mx-auto">
                    <div></div>
                    <button id="ctrlUp" class="bg-gray-800 hover:bg-gray-700 text-slate-200 active:bg-emerald-500 active:text-slate-950 p-4 rounded-2xl flex items-center justify-center shadow transition-all transform active:scale-90">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M5 15l7-7 7 7" /></svg>
                    </button>
                    <div></div>
                    <button id="ctrlLeft" class="bg-gray-800 hover:bg-gray-700 text-slate-200 active:bg-emerald-500 active:text-slate-950 p-4 rounded-2xl flex items-center justify-center shadow transition-all transform active:scale-90">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M15 19l-7-7 7-7" /></svg>
                    </button>
                    <div class="bg-gray-950 rounded-2xl border border-gray-800/60 flex items-center justify-center">
                        <span class="w-2.5 h-2.5 rounded-full bg-emerald-500 animate-pulse"></span>
                    </div>
                    <button id="ctrlRight" class="bg-gray-800 hover:bg-gray-700 text-slate-200 active:bg-emerald-500 active:text-slate-950 p-4 rounded-2xl flex items-center justify-center shadow transition-all transform active:scale-90">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M9 5l7 7-7 7" /></svg>
                    </button>
                    <div></div>
                    <button id="ctrlDown" class="bg-gray-800 hover:bg-gray-700 text-slate-200 active:bg-emerald-500 active:text-slate-950 p-4 rounded-2xl flex items-center justify-center shadow transition-all transform active:scale-90">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M19 9l-7 7-7-7" /></svg>
                    </button>
                    <div></div>
                </div>
            </div>

        </section>
    </main>

    <!-- FOOTER -->
    <footer class="bg-gray-950 border-t border-gray-900 py-4 px-4 text-center text-xs text-gray-600">
        <div class="max-w-5xl mx-auto flex flex-col md:flex-row items-center justify-between gap-2">
            <p>© 2026 Neon Viper. Designed for dynamic desktop and mobile touchscreens.</p>
            <p>Built with Vanilla JavaScript & Canvas 2D.</p>
        </div>
    </footer>

    <!-- GAME ENGINE CODE -->
    <script>
        // Wait for DOM layout to load securely
        window.onload = function() {
            // --- CONSTANTS & CONFIGS ---
            const CANVAS_CONTAINER_RATIO = 20; // grid size
            const GRID_COUNT = 20; // 20x20 cells

            // Game State
            let snake = [];
            let direction = { x: 1, y: 0 };
            let nextDirection = { x: 1, y: 0 };
            let food = { x: 0, y: 0, type: 'apple' }; // types: apple, golden, shield
            let obstacles = [];
            let score = 0;
            let highScore = localStorage.getItem('snake_highscore') ? parseInt(localStorage.getItem('snake_highscore')) : 0;
            let level = 1;
            let applesEaten = 0;
            let isPlaying = false;
            let isPaused = true;
            let gameInterval = null;
            let activeMode = 'classic'; // classic, obstacles, speed
            let soundEnabled = true;

            // Power-up States
            let shieldActive = false;
            let shieldTimer = 0;
            let shieldInterval = null;

            // Particles Array
            let particles = [];

            // UI Elements
            const canvas = document.getElementById('gameCanvas');
            const ctx = canvas.getContext('2d');
            const currentScoreEl = document.getElementById('currentScore');
            const highScoreEl = document.getElementById('highScore');
            const currentLevelEl = document.getElementById('currentLevel');
            const shieldStatusEl = document.getElementById('shieldStatus');
            const shieldTimerEl = document.getElementById('shieldTimer');
            const activeModeLabelEl = document.getElementById('activeModeLabel');
            const gameOverScreen = document.getElementById('gameOverScreen');
            const pauseScreen = document.getElementById('pauseScreen');
            const startBtn = document.getElementById('startBtn');
            const restartBtn = document.getElementById('restartBtn');
            const finalScoreEl = document.getElementById('finalScore');
            const finalApplesEl = document.getElementById('finalApples');

            // Sound Toggle Elements
            const soundToggleBtn = document.getElementById('soundToggleBtn');
            const soundOnIcon = document.getElementById('soundOnIcon');
            const soundOffIcon = document.getElementById('soundOffIcon');

            // Keyboard/Direction Mapping
            const directions = {
                ArrowUp: { x: 0, y: -1 },
                ArrowDown: { x: 0, y: 1 },
                ArrowLeft: { x: -1, y: 0 },
                ArrowRight: { x: 1, y: 0 },
                w: { x: 0, y: -1 },
                s: { x: 0, y: 1 },
                a: { x: -1, y: 0 },
                d: { x: 1, y: 0 }
            };

            // Setup Scores in Interface
            highScoreEl.textContent = String(highScore).padStart(4, '0');

            // --- RESPONSIVE CANVAS ---
            function resizeCanvas() {
                // Adjust sizes fluidly based on its parent size
                const parent = canvas.parentElement;
                const size = Math.min(parent.clientWidth, parent.clientHeight, 600) - 16;
                canvas.width = size;
                canvas.height = size;
                draw(); // Redraw immediately on resize
            }
            window.addEventListener('resize', resizeCanvas);
            resizeCanvas();

            // --- SYNTH AUDIO ENGINE (WEB AUDIO API) ---
            function playChiptune(type) {
                if (!soundEnabled) return;
                try {
                    const AudioContext = window.AudioContext || window.webkitAudioContext;
                    if (!AudioContext) return;
                    const audioCtx = new AudioContext();
                    const osc = audioCtx.createOscillator();
                    const gain = audioCtx.createGain();

                    osc.connect(gain);
                    gain.connect(audioCtx.destination);

                    if (type === 'eat') {
                        // High pitch bubble sound
                        osc.type = 'triangle';
                        osc.frequency.setValueAtTime(523.25, audioCtx.currentTime); // C5
                        osc.frequency.exponentialRampToValueAtTime(1046.50, audioCtx.currentTime + 0.15); // C6
                        gain.gain.setValueAtTime(0.15, audioCtx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.15);
                        osc.start();
                        osc.stop(audioCtx.currentTime + 0.15);
                    } else if (type === 'powerup') {
                        // Level up arpeggio
                        osc.type = 'sine';
                        osc.frequency.setValueAtTime(587.33, audioCtx.currentTime); // D5
                        osc.frequency.setValueAtTime(659.25, audioCtx.currentTime + 0.08); // E5
                        osc.frequency.setValueAtTime(783.99, audioCtx.currentTime + 0.16); // G5
                        osc.frequency.setValueAtTime(1174.66, audioCtx.currentTime + 0.24); // D6
                        gain.gain.setValueAtTime(0.2, audioCtx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.4);
                        osc.start();
                        osc.stop(audioCtx.currentTime + 0.4);
                    } else if (type === 'crash') {
                        // Explosion/down-sweep noise
                        osc.type = 'sawtooth';
                        osc.frequency.setValueAtTime(220, audioCtx.currentTime);
                        osc.frequency.linearRampToValueAtTime(50, audioCtx.currentTime + 0.4);
                        gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.4);
                        osc.start();
                        osc.stop(audioCtx.currentTime + 0.4);
                    } else if (type === 'tick') {
                        // Short dry pulse
                        osc.type = 'sine';
                        osc.frequency.setValueAtTime(800, audioCtx.currentTime);
                        gain.gain.setValueAtTime(0.05, audioCtx.currentTime);
                        gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.05);
                        osc.start();
                        osc.stop(audioCtx.currentTime + 0.06);
                    }
                } catch (e) {
                    console.warn("Audio Context failed or was blocked by browser policies.");
                }
            }

            // --- PARTICLE EMITTER ENGINE ---
            function createExplosion(x, y, color) {
                const cellWidth = canvas.width / GRID_COUNT;
                const cellHeight = canvas.height / GRID_COUNT;
                const centerX = (x * cellWidth) + (cellWidth / 2);
                const centerY = (y * cellHeight) + (cellHeight / 2);

                for (let i = 0; i < 15; i++) {
                    particles.push({
                        x: centerX,
                        y: centerY,
                        vx: (Math.random() - 0.5) * 6,
                        vy: (Math.random() - 0.5) * 6,
                        radius: Math.random() * 3 + 1,
                        color: color,
                        alpha: 1,
                        decay: Math.random() * 0.05 + 0.02
                    });
                }
            }

            function updateParticles() {
                for (let i = particles.length - 1; i >= 0; i--) {
                    const p = particles[i];
                    p.x += p.vx;
                    p.y += p.vy;
                    p.alpha -= p.decay;
                    if (p.alpha <= 0) {
                        particles.splice(i, 1);
                    }
                }
            }

            function drawParticles() {
                particles.forEach(p => {
                    ctx.save();
                    ctx.globalAlpha = p.alpha;
                    ctx.fillStyle = p.color;
                    ctx.shadowBlur = 10;
                    ctx.shadowColor = p.color;
                    ctx.beginPath();
                    ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.restore();
                });
            }

            // --- INITIALIZE & RESET GAME STATE ---
            function initGame() {
                snake = [
                    { x: 10, y: 10 },
                    { x: 9, y: 10 },
                    { x: 8, y: 10 }
                ];
                direction = { x: 1, y: 0 };
                nextDirection = { x: 1, y: 0 };
                score = 0;
                level = 1;
                applesEaten = 0;
                shieldActive = false;
                shieldTimer = 0;
                clearInterval(shieldInterval);
                shieldStatusEl.classList.add('hidden');
                
                currentScoreEl.textContent = String(score).padStart(4, '0');
                currentLevelEl.textContent = level;

                // Build Obstacles if applicable
                obstacles = [];
                if (activeMode === 'obstacles') {
                    generateObstacles();
                }

                spawnFood();
                particles = [];
            }

            function generateObstacles() {
                // Create random static blockades on the field
                // Ensure they don't block the center starting vector
                const numObstacles = 12;
                for (let i = 0; i < numObstacles; i++) {
                    let obsX, obsY;
                    let isValid = false;
                    while (!isValid) {
                        obsX = Math.floor(Math.random() * GRID_COUNT);
                        obsY = Math.floor(Math.random() * GRID_COUNT);
                        
                        // Check distance from starting area
                        const distFromStart = Math.abs(obsX - 10) + Math.abs(obsY - 10);
                        if (distFromStart > 4) {
                            isValid = true;
                        }
                    }
                    obstacles.push({ x: obsX, y: obsY });
                }
            }

            function spawnFood() {
                let validSpawn = false;
                let rx, ry;

                while (!validSpawn) {
                    rx = Math.floor(Math.random() * GRID_COUNT);
                    ry = Math.floor(Math.random() * GRID_COUNT);

                    // Must not collide with snake
                    const collidesWithSnake = snake.some(part => part.x === rx && part.y === ry);
                    // Must not collide with active obstacles
                    const collidesWithObstacles = obstacles.some(obs => obs.x === rx && obs.y === ry);

                    if (!collidesWithSnake && !collidesWithObstacles) {
                        validSpawn = true;
                    }
                }

                // Decide Food Type
                const rand = Math.random();
                let type = 'apple';
                if (rand > 0.88) {
                    type = 'shield'; // Shield melon
                } else if (rand > 0.75) {
                    type = 'golden'; // Golden Apple
                }

                food = { x: rx, y: ry, type: type };
            }

            // --- GAME PLAY LOOP ---
            function gameStep() {
                direction = nextDirection;

                // Predict Head movement
                const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

                // Handle Wall Bounds Clipping
                if (head.x < 0) head.x = GRID_COUNT - 1;
                if (head.x >= GRID_COUNT) head.x = 0;
                if (head.y < 0) head.y = GRID_COUNT - 1;
                if (head.y >= GRID_COUNT) head.y = 0;

                // Collision Verifications
                // Self-crash
                const selfCrash = snake.some(part => part.x === head.x && part.y === head.y);
                // Obstacle collision
                const obstacleCrash = obstacles.some(obs => obs.x === head.x && obs.y === head.y);

                if ((selfCrash || obstacleCrash) && !shieldActive) {
                    // Trigger Death
                    endGame();
                    return;
                }

                // Add new head position
                snake.unshift(head);

                // Food consumption detection
                if (head.x === food.x && head.y === food.y) {
                    handleFoodConsumption();
                } else {
                    // Normal movement, pop tail to maintain length
                    snake.pop();
                }

                updateParticles();
                draw();
            }

            function handleFoodConsumption() {
                applesEaten++;
                let points = 10;
                let soundType = 'eat';

                if (food.type === 'golden') {
                    points = 30;
                    soundType = 'powerup';
                    createExplosion(food.x, food.y, '#F59E0B');
                } else if (food.type === 'shield') {
                    points = 15;
                    soundType = 'powerup';
                    activateShield(8);
                    createExplosion(food.x, food.y, '#818CF8');
                } else {
                    createExplosion(food.x, food.y, '#10B981');
                }

                score += points;
                if (score > highScore) {
                    highScore = score;
                    localStorage.setItem('snake_highscore', highScore);
                    highScoreEl.textContent = String(highScore).padStart(4, '0');
                }

                currentScoreEl.textContent = String(score).padStart(4, '0');
                playChiptune(soundType);

                // Level progression & Speed calibration
                if (applesEaten % 5 === 0) {
                    level++;
                    currentLevelEl.textContent = level;
                    playChiptune('powerup');
                    if (activeMode === 'speed') {
                        adjustGameSpeed();
                    }
                }

                spawnFood();
            }

            function activateShield(duration) {
                shieldActive = true;
                shieldTimer = duration;
                shieldStatusEl.classList.remove('hidden');
                shieldTimerEl.textContent = shieldTimer;

                clearInterval(shieldInterval);
                shieldInterval = setInterval(() => {
                    shieldTimer--;
                    shieldTimerEl.textContent = shieldTimer;
                    if (shieldTimer <= 0) {
                        shieldActive = false;
                        clearInterval(shieldInterval);
                        shieldStatusEl.classList.add('hidden');
                    }
                }, 1000);
            }

            function adjustGameSpeed() {
                // Speed run mode: gets continuously faster
                clearInterval(gameInterval);
                const currentSpeed = Math.max(120 - (level * 6), 40);
                gameInterval = setInterval(gameStep, currentSpeed);
            }

            function startGame() {
                initGame();
                isPlaying = true;
                isPaused = false;
                pauseScreen.classList.add('hidden');
                gameOverScreen.classList.add('hidden');

                let speed = 100; // default speed for Classic & Obstacles
                if (activeMode === 'speed') speed = 120;
                
                clearInterval(gameInterval);
                gameInterval = setInterval(gameStep, speed);
                playChiptune('powerup');
            }

            function endGame() {
                isPlaying = false;
                clearInterval(gameInterval);
                clearInterval(shieldInterval);
                playChiptune('crash');

                finalScoreEl.textContent = String(score).padStart(4, '0');
                finalApplesEl.textContent = applesEaten;
                gameOverScreen.classList.remove('hidden');
            }

            // --- GRAPHICS RENDERING (CANVAS) ---
            function draw() {
                ctx.fillStyle = '#030712';
                ctx.fillRect(0, 0, canvas.width, canvas.height);

                const cellWidth = canvas.width / GRID_COUNT;
                const cellHeight = canvas.height / GRID_COUNT;

                // Draw Grid Guide lines
                ctx.strokeStyle = 'rgba(31, 41, 55, 0.4)';
                ctx.lineWidth = 1;
                for (let i = 0; i <= GRID_COUNT; i++) {
                    ctx.beginPath();
                    ctx.moveTo(i * cellWidth, 0);
                    ctx.lineTo(i * cellWidth, canvas.height);
                    ctx.stroke();

                    ctx.beginPath();
                    ctx.moveTo(0, i * cellHeight);
                    ctx.lineTo(canvas.width, i * cellHeight);
                    ctx.stroke();
                }

                // Draw Obstacles (Obstacles Mode)
                if (activeMode === 'obstacles') {
                    ctx.shadowBlur = 8;
                    ctx.shadowColor = '#4B5563';
                    ctx.fillStyle = '#1F2937';
                    obstacles.forEach(obs => {
                        drawRoundedRect(
                            obs.x * cellWidth + 2, 
                            obs.y * cellHeight + 2, 
                            cellWidth - 4, 
                            cellHeight - 4, 
                            4, 
                            true, 
                            false
                        );
                    });
                }

                // Draw Food
                let foodColor = '#EF4444'; // default red apple
                let shadowColor = '#EF4444';
                let emoji = '🍎';

                if (food.type === 'golden') {
                    foodColor = '#F59E0B';
                    shadowColor = '#F59E0B';
                    emoji = '⭐';
                } else if (food.type === 'shield') {
                    foodColor = '#6366F1';
                    shadowColor = '#6366F1';
                    emoji = '🛡️';
                }

                // Draw pulsing neon glow for food
                ctx.save();
                ctx.shadowBlur = 12 + Math.sin(Date.now() / 100) * 4;
                ctx.shadowColor = shadowColor;
                ctx.fillStyle = foodColor;
                
                // Draw beautifully structured circular item
                const fx = food.x * cellWidth + (cellWidth / 2);
                const fy = food.y * cellHeight + (cellHeight / 2);
                ctx.beginPath();
                ctx.arc(fx, fy, (cellWidth / 2) - 3, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();

                // Draw Food Stem/Indicator decoration
                ctx.fillStyle = '#10B981';
                ctx.fillRect(fx - 1, fy - (cellHeight / 2) + 1, 3, 4);

                // Draw Snake
                snake.forEach((part, index) => {
                    const isHead = index === 0;
                    ctx.save();
                    
                    if (shieldActive) {
                        ctx.fillStyle = '#6366F1';
                        ctx.shadowColor = '#818CF8';
                    } else if (isHead) {
                        ctx.fillStyle = '#10B981';
                        ctx.shadowColor = '#34D399';
                    } else {
                        // Color degradation for nice glowing tail effect
                        ctx.fillStyle = `rgba(16, 185, 129, ${1 - (index / snake.length) * 0.65})`;
                        ctx.shadowColor = '#10B981';
                    }

                    ctx.shadowBlur = isHead ? 15 : 6;
                    
                    const pad = 2;
                    const px = part.x * cellWidth + pad;
                    const py = part.y * cellHeight + pad;
                    const pw = cellWidth - (pad * 2);
                    const ph = cellHeight - (pad * 2);

                    // Draw rounded viper scales
                    drawRoundedRect(px, py, pw, ph, 5, true, false);

                    // Eyes decorations on snake head
                    if (isHead) {
                        ctx.fillStyle = '#030712';
                        ctx.shadowBlur = 0;
                        const eyeRadius = 2.5;
                        
                        // Decide eye layouts based on directional vector
                        if (direction.x !== 0) { // Moving Horizontally
                            ctx.beginPath();
                            ctx.arc(px + (pw / 2), py + (ph / 4), eyeRadius, 0, Math.PI * 2);
                            ctx.arc(px + (pw / 2), py + (ph * 3 / 4), eyeRadius, 0, Math.PI * 2);
                            ctx.fill();
                        } else { // Moving Vertically
                            ctx.beginPath();
                            ctx.arc(px + (pw / 4), py + (ph / 2), eyeRadius, 0, Math.PI * 2);
                            ctx.arc(px + (pw * 3 / 4), py + (ph / 2), eyeRadius, 0, Math.PI * 2);
                            ctx.fill();
                        }
                    }
                    ctx.restore();
                });

                // Render Active Burst Particles
                drawParticles();
            }

            // Canvas Drawing Helpers
            function drawRoundedRect(x, y, width, height, radius, fill, stroke) {
                ctx.beginPath();
                ctx.moveTo(x + radius, y);
                ctx.lineTo(x + width - radius, y);
                ctx.quadraticCurveTo(x + width, y, x + width, y + radius);
                ctx.lineTo(x + width, y + height - radius);
                ctx.quadraticCurveTo(x + width, y + height, x + width - radius, y + height);
                ctx.lineTo(x + radius, y + height);
                ctx.quadraticCurveTo(x, y + height, x, y + height - radius);
                ctx.lineTo(x, y + radius);
                ctx.quadraticCurveTo(x, y, x + radius, y);
                ctx.closePath();
                if (fill) ctx.fill();
                if (stroke) ctx.stroke();
            }
            // --- CONTROLLER ACTIONS ---
            function changeDirection(dx, dy) {
                // Prevent running backwards into itself
                if (direction.x + dx === 0 && direction.y + dy === 0) return;
                nextDirection = { x: dx, y: dy };
                playChiptune('tick');
            }

            // Keyboard Events
            window.addEventListener('keydown', e => {
                const dir = directions[e.key];
                if (dir) {
                    e.preventDefault();
                    if (!isPlaying) return;
                    changeDirection(dir.x, dir.y);
                }
                
                // Toggle Space for Pause / Action
                if (e.key === ' ' || e.key === 'Spacebar') {
                    e.preventDefault();
                    if (isPlaying) {
                        isPaused = !isPaused;
                        if (isPaused) {
                            clearInterval(gameInterval);
                            pauseScreen.classList.remove('hidden');
                        } else {
                            let speed = 100;
                            if (activeMode === 'speed') speed = Math.max(120 - (level * 6), 40);
                            gameInterval = setInterval(gameStep, speed);
                            pauseScreen.classList.add('hidden');
                        }
                    } else {
                        startGame();
                    }
                }
            });

            // Mobile Navigation click listeners
            document.getElementById('ctrlUp').addEventListener('click', () => changeDirection(0, -1));
            document.getElementById('ctrlDown').addEventListener('click', () => changeDirection(0, 1));
            document.getElementById('ctrlLeft').addEventListener('click', () => changeDirection(-1, 0));
            document.getElementById('ctrlRight').addEventListener('click', () => changeDirection(1, 0));

            // --- MOBILE SWIPE INTERFACE DETECTORS ---
            let touchStartX = 0;
            let touchStartY = 0;

            canvas.addEventListener('touchstart', e => {
                touchStartX = e.touches[0].clientX;
                touchStartY = e.touches[0].clientY;
            }, { passive: true });

            canvas.addEventListener('touchend', e => {
                if (!isPlaying) return;
                const dx = e.changedTouches[0].clientX - touchStartX;
                const dy = e.changedTouches[0].clientY - touchStartY;
                
                const absX = Math.abs(dx);
                const absY = Math.abs(dy);

                if (Math.max(absX, absY) > 30) { // Threshold for swipe
                    if (absX > absY) {
                        // Swipe Horizontal
                        changeDirection(dx > 0 ? 1 : -1, 0);
                    } else {
                        // Swipe Vertical
                        changeDirection(0, dy > 0 ? 1 : -1);
                    }
                }
            }, { passive: true });

            // --- UI PANEL INTERACTIVITY ---
            
            // Mode Selectors
            const modeButtons = {
                classic: document.getElementById('modeClassic'),
                obstacles: document.getElementById('modeObstacles'),
                speed: document.getElementById('modeSpeed')
            };

            function setMode(mode) {
                activeMode = mode;
                
                // Update styling of buttons
                Object.keys(modeButtons).forEach(m => {
                    const btn = modeButtons[m];
                    if (m === mode) {
                        btn.className = "mode-btn bg-emerald-500/10 border border-emerald-500/40 text-emerald-400 text-xs font-bold py-2.5 px-3 rounded-xl transition-all";
                    } else {
                        btn.className = "mode-btn bg-gray-950 border border-gray-800 text-gray-400 text-xs font-bold py-2.5 px-3 rounded-xl transition-all hover:border-gray-700 hover:text-gray-200";
                    }
                });

                // Label updates
                activeModeLabelEl.textContent = `${mode} Arena`;
                
                initGame();
                draw();
            }

            modeButtons.classic.addEventListener('click', () => setMode('classic'));
            modeButtons.obstacles.addEventListener('click', () => setMode('obstacles'));
            modeButtons.speed.addEventListener('click', () => setMode('speed'));

            // Play/Restart Buttons
            startBtn.addEventListener('click', startGame);
            restartBtn.addEventListener('click', startGame);

            // Audio config toggle
            soundToggleBtn.addEventListener('click', () => {
                soundEnabled = !soundEnabled;
                if (soundEnabled) {
                    soundOnIcon.classList.remove('hidden');
                    soundOffIcon.classList.add('hidden');
                } else {
                    soundOnIcon.classList.add('hidden');
                    soundOffIcon.classList.remove('hidden');
                }
            });

            // Render Initial Frame on Screen
            initGame();
            draw();
        };
    </script>
</body>
</html>

```
