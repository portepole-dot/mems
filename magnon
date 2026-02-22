<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Magnonic Holographic Processor HUD</title>
    <!-- p5.js Library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.0/p5.js"></script>
    <style>
        :root {
            --bg-color: #000000;
            --magenta: #ff00ff;
            --cyan: #00ffff;
            --violet: #8a2be2;
            --white: #ffffff;
            --green: #00ff00;
            --grid-color: rgba(0, 255, 255, 0.1);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg-color);
            color: var(--cyan);
            font-family: 'Courier New', Courier, monospace;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            height: 100vh;
            background-image: 
                linear-gradient(var(--grid-color) 1px, transparent 1px),
                linear-gradient(90deg, var(--grid-color) 1px, transparent 1px);
            background-size: 20px 20px;
        }

        /* Glitch & Shake Effects */
        .shake {
            animation: kinetic-shake 0.1s infinite;
        }

        .glitch {
            animation: rad-glitch 0.2s infinite;
        }

        @keyframes kinetic-shake {
            0% { transform: translate(0, 0) rotate(0deg); }
            25% { transform: translate(-8px, 6px) rotate(-1deg); }
            50% { transform: translate(6px, -8px) rotate(1deg); }
            75% { transform: translate(-6px, -6px) rotate(0deg); }
            100% { transform: translate(8px, 8px) rotate(-1deg); }
        }

        @keyframes rad-glitch {
            0% { filter: invert(0) contrast(100%); transform: translate(0); }
            20% { filter: invert(1) contrast(300%) hue-rotate(90deg); transform: translate(-5px, 2px); }
            40% { filter: invert(0) contrast(100%); transform: translate(5px, -2px); }
            60% { filter: invert(1) contrast(200%) hue-rotate(180deg); transform: translate(-2px, 5px); }
            80% { filter: invert(0) contrast(100%); transform: translate(2px, -5px); }
            100% { filter: invert(0) contrast(100%); transform: translate(0); }
        }

        #flash-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background-color: white;
            z-index: 9999;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.1s ease-out;
        }

        /* Layout */
        header {
            padding: 15px 20px;
            border-bottom: 2px solid var(--magenta);
            font-size: 1.2rem;
            font-weight: bold;
            text-shadow: 0 0 10px var(--magenta);
            display: flex;
            justify-content: space-between;
            background: rgba(255, 0, 255, 0.05);
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .header-sub {
            color: var(--white);
            font-size: 0.9rem;
            text-shadow: 0 0 5px var(--white);
        }

        .main-container {
            display: flex;
            flex: 1;
            padding: 20px;
            gap: 20px;
            height: calc(100vh - 250px);
        }

        /* Panels */
        .panel {
            border: 1px solid var(--cyan);
            background: rgba(0, 20, 20, 0.6);
            position: relative;
            box-shadow: inset 0 0 20px rgba(0, 255, 255, 0.1);
        }

        .panel::before {
            content: '';
            position: absolute;
            top: 0; left: 0; width: 10px; height: 10px;
            border-top: 2px solid var(--white);
            border-left: 2px solid var(--white);
        }
        .panel::after {
            content: '';
            position: absolute;
            bottom: 0; right: 0; width: 10px; height: 10px;
            border-bottom: 2px solid var(--white);
            border-right: 2px solid var(--white);
        }

        #panel-1 {
            flex: 7;
            display: flex;
            flex-direction: column;
        }

        #panel-2 {
            flex: 3;
            display: flex;
            flex-direction: column;
        }

        .panel-header {
            padding: 10px;
            background: rgba(0, 255, 255, 0.1);
            border-bottom: 1px solid var(--cyan);
            font-size: 0.85rem;
            letter-spacing: 1px;
            display: flex;
            justify-content: space-between;
        }

        .canvas-container {
            flex: 1;
            position: relative;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        canvas {
            width: 100%;
            height: 100%;
            object-fit: fill;
        }

        .overlay-text {
            position: absolute;
            bottom: 15px;
            left: 15px;
            font-size: 1.2rem;
            font-weight: bold;
            color: var(--white);
            text-shadow: 0 0 10px var(--white);
            background: rgba(0, 0, 0, 0.7);
            padding: 5px 10px;
            border-left: 3px solid var(--magenta);
            pointer-events: none;
        }

        /* Panel 3: Controls */
        #panel-3 {
            height: 200px;
            margin: 0 20px 20px 20px;
            display: flex;
            gap: 20px;
            border: 1px solid var(--violet);
            background: rgba(138, 43, 226, 0.05);
            padding: 15px;
            box-shadow: inset 0 0 20px rgba(138, 43, 226, 0.1);
        }

        .controls-wrapper {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .slider-container {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .slider-label {
            font-size: 0.9rem;
            color: var(--violet);
            text-shadow: 0 0 5px var(--violet);
            display: flex;
            justify-content: space-between;
        }

        input[type=range] {
            -webkit-appearance: none;
            width: 100%;
            background: rgba(138, 43, 226, 0.2);
            height: 10px;
            border: 1px solid var(--violet);
            outline: none;
            box-shadow: 0 0 10px rgba(138, 43, 226, 0.5);
        }

        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 25px;
            background: var(--white);
            cursor: pointer;
            box-shadow: 0 0 10px var(--white);
        }

        .btn-group {
            display: flex;
            gap: 15px;
            flex: 1;
        }

        button {
            flex: 1;
            background: rgba(0, 0, 0, 0.8);
            border: 1px solid var(--cyan);
            color: var(--cyan);
            font-family: 'Courier New', Courier, monospace;
            font-size: 0.9rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            text-transform: uppercase;
            box-shadow: 0 0 10px rgba(0, 255, 255, 0.2);
        }

        button:hover {
            background: var(--cyan);
            color: var(--bg-color);
            box-shadow: 0 0 20px var(--cyan);
        }

        button.btn-red {
            border-color: #ff0044;
            color: #ff0044;
            box-shadow: 0 0 10px rgba(255, 0, 68, 0.2);
        }

        button.btn-red:hover {
            background: #ff0044;
            color: var(--bg-color);
            box-shadow: 0 0 20px #ff0044;
        }

        /* Tactical Log */
        .tactical-log {
            flex: 1.5;
            background: rgba(0, 10, 0, 0.8);
            border: 1px solid var(--green);
            padding: 10px;
            font-size: 0.85rem;
            color: var(--green);
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 5px;
            box-shadow: inset 0 0 15px rgba(0, 255, 0, 0.1);
        }

        .log-entry {
            border-left: 2px solid;
            padding-left: 8px;
            animation: typeIn 0.3s ease-out forwards;
            opacity: 0;
        }

        @keyframes typeIn {
            to { opacity: 1; }
        }

        .log-time {
            color: var(--white);
            margin-right: 10px;
        }

        .status-lock {
            color: #ff0044;
            font-weight: bold;
            text-shadow: 0 0 5px #ff0044;
        }
        .status-lock.locked {
            color: var(--green);
            text-shadow: 0 0 5px var(--green);
        }

    </style>
</head>
<body>

    <div id="flash-overlay"></div>

    <header>
        <div>SYSTEM: MAGNONIC HOLOGRAPHIC PROCESSOR [TOP SECRET]</div>
        <div class="header-sub">OPERATIONAL STATUS: ACTIVE</div>
    </header>

    <div class="main-container">
        <!-- PANEL 1: YIG FILM HOLOGRAPHY -->
        <div id="panel-1" class="panel">
            <div class="panel-header">
                <span>PANEL 1: YIG FILM HOLOGRAPHY (2D WAVE ARRAY)</span>
                <span>DATA TYPE: CONTINUOUS FIELD</span>
            </div>
            <div class="canvas-container" id="canvas-container-1">
                <div class="overlay-text">MASS (m) = 0.<br>NO MOVING PARTS.</div>
            </div>
        </div>

        <!-- PANEL 2: HOLOGRAPHIC READOUT -->
        <div id="panel-2" class="panel">
            <div class="panel-header">
                <span>PANEL 2: READOUT OSCILLOSCOPE</span>
                <span id="lock-status" class="status-lock">ACQUIRING...</span>
            </div>
            <div class="canvas-container" id="canvas-container-2">
                <div class="overlay-text" style="font-size: 0.85rem; border-color: var(--cyan); bottom: auto; top: 15px;">
                    COMPUTE SPEED: 2.4 THz<br>
                    TARGET: <span style="color:var(--white)">ACOUSTIC SIG (WHITE)</span><br>
                    OUTPUT: <span style="color:var(--cyan)">SUPERPOSITION (CYAN)</span>
                </div>
            </div>
        </div>
    </div>

    <!-- PANEL 3: CONTROLS & EW -->
    <div id="panel-3">
        <div class="controls-wrapper">
            <div class="slider-container">
                <div class="slider-label">
                    <span>Magnetic Phase Poling (SOT Lens Tuning)</span>
                    <span id="tuning-val">0.00 THz</span>
                </div>
                <input type="range" id="tuning-slider" min="0" max="1" step="0.001" value="0">
            </div>
            <div class="btn-group">
                <button class="btn-red" id="btn-emp">TRIGGER NUCLEAR EMP / TID RADIATION</button>
                <button id="btn-kinetic">KINETIC STICTION ATTACK</button>
            </div>
        </div>
        <div class="tactical-log" id="tactical-log">
            <div class="log-entry" style="border-color: var(--cyan);">
                <span class="log-time">[SYS]</span> INITIALIZING YIG THIN FILM...
            </div>
            <div class="log-entry" style="border-color: var(--cyan);">
                <span class="log-time">[SYS]</span> MAGNETIC NANO-ISLANDS ACTIVE. SPIN WAVES NOMINAL.
            </div>
        </div>
    </div>

    <script>
        // --- GLOBAL STATE & SETTINGS ---
        const State = {
            currentTuning: 0,
            targetTuning: 0.742, // The "Sweet Spot" to lock onto
            isGlitching: false,
            isShaking: false,
            isLocked: false
        };

        const config = {
            simWidth: 200,   // Internal low-res width for fast pixel math
            simHeight: 120,
            waveFreq: 0.35,
            sources: [
                {x: 5, y: 30},
                {x: 5, y: 60},
                {x: 5, y: 90}
            ],
            lenses: [
                {x: 60,  y: 40,  r: 18, strength: 3.5},
                {x: 90,  y: 80,  r: 24, strength: -4.2},
                {x: 120, y: 30,  r: 20, strength: 5.0},
                {x: 140, y: 90,  r: 15, strength: -3.0},
                {x: 100, y: 55,  r: 12, strength: 2.5},
                {x: 160, y: 60,  r: 22, strength: 4.0}
            ],
            historyLen: 300
        };

        // Shared data between panels
        const Signals = {
            targetWaveHistory: new Array(config.historyLen).fill(0),
            currentWaveHistory: new Array(config.historyLen).fill(0)
        };

        // --- DOM Elements ---
        const slider = document.getElementById('tuning-slider');
        const tuningValText = document.getElementById('tuning-val');
        const lockStatus = document.getElementById('lock-status');
        const logContainer = document.getElementById('tactical-log');
        const flashOverlay = document.getElementById('flash-overlay');

        // --- Event Listeners ---
        slider.addEventListener('input', (e) => {
            State.currentTuning = parseFloat(e.target.value);
            tuningValText.innerText = (State.currentTuning * 10).toFixed(2) + " THz";
            checkLock();
        });

        document.getElementById('btn-emp').addEventListener('click', () => {
            // Flash
            flashOverlay.style.opacity = '1';
            setTimeout(() => { flashOverlay.style.opacity = '0'; }, 100);
            
            // Glitch body
            document.body.classList.add('glitch');
            State.isGlitching = true;
            
            logMessage("MASSLESS SPIN WAVES DETECTED. NO CMOS GATES FOUND. MAGNETIC SPINS UNAFFECTED BY IONIZING RADIATION. 100% IMMUNITY.", "var(--green)");
            
            setTimeout(() => {
                document.body.classList.remove('glitch');
                State.isGlitching = false;
            }, 1000);
        });

        document.getElementById('btn-kinetic').addEventListener('click', () => {
            document.body.classList.add('shake');
            State.isShaking = true;
            
            logMessage("KINETIC SHOCKWAVE DETECTED. MOVING MASS = 0. STICTION = IMPOSSIBLE. COMPUTATION UNINTERRUPTED.", "var(--green)");
            
            setTimeout(() => {
                document.body.classList.remove('shake');
                State.isShaking = false;
            }, 500);
        });

        function logMessage(text, color) {
            const entry = document.createElement('div');
            entry.className = 'log-entry';
            entry.style.borderColor = color;
            
            const now = new Date();
            const timeStr = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}:${now.getSeconds().toString().padStart(2, '0')}.${now.getMilliseconds().toString().padStart(3, '0')}`;
            
            entry.innerHTML = `<span class="log-time">[${timeStr}]</span> <span style="color:${color}">${text}</span>`;
            logContainer.appendChild(entry);
            logContainer.scrollTop = logContainer.scrollHeight;
        }

        function checkLock() {
            const diff = Math.abs(State.currentTuning - State.targetTuning);
            if (diff < 0.02) {
                if (!State.isLocked) {
                    State.isLocked = true;
                    lockStatus.textContent = "PHASE LOCK: ACQUIRED";
                    lockStatus.className = "status-lock locked";
                    logMessage("INTERFERENCE PATTERN ALIGNED. OUTPUT HOLOGRAPHY MATCHES TARGET SIGNATURE.", "var(--cyan)");
                }
            } else {
                if (State.isLocked) {
                    State.isLocked = false;
                    lockStatus.textContent = "ACQUIRING...";
                    lockStatus.className = "status-lock";
                }
            }
        }

        // Setup Initial View
        slider.value = 0.15;
        State.currentTuning = 0.15;
        tuningValText.innerText = "1.50 THz";

        // --- P5.JS SKETCH FOR PANEL 1 (YIG FILM) ---
        new p5((p) => {
            let sim;
            
            p.setup = () => {
                let container = document.getElementById('canvas-container-1');
                let canvas = p.createCanvas(container.clientWidth, container.clientHeight);
                canvas.parent(container);
                
                // Low res graphics buffer for speed
                sim = p.createGraphics(config.simWidth, config.simHeight);
                sim.pixelDensity(1);
            };

            p.windowResized = () => {
                let container = document.getElementById('canvas-container-1');
                p.resizeCanvas(container.clientWidth, container.clientHeight);
            };

            p.draw = () => {
                let time = p.frameCount * 0.15;
                
                sim.loadPixels();
                let index = 0;
                
                let W = config.simWidth;
                let H = config.simHeight;
                let cTuning = State.currentTuning;
                
                // We calculate output sum simultaneously for performance
                let rightEdgeCurrentSum = 0;

                for (let y = 0; y < H; y++) {
                    for (let x = 0; x < W; x++) {
                        
                        // Calculate phase distortion from magnetic islands (lenses)
                        let phaseShift = 0;
                        for (let i = 0; i < config.lenses.length; i++) {
                            let l = config.lenses[i];
                            // Using standard distance is slightly slow, approximation or direct calculation:
                            let dx = x - l.x;
                            let dy = y - l.y;
                            let distSq = dx*dx + dy*dy;
                            let rSq = l.r * l.r;
                            
                            if (distSq < rSq) {
                                let dist = Math.sqrt(distSq);
                                phaseShift += (1 - dist/l.r) * l.strength * cTuning;
                            }
                        }

                        // Calculate wave superposition
                        let totalAmp = 0;
                        for (let i = 0; i < config.sources.length; i++) {
                            let s = config.sources[i];
                            let dx = x - s.x;
                            let dy = y - s.y;
                            let dist = Math.sqrt(dx*dx + dy*dy);
                            
                            // Wave function: sin(k*x - w*t + phase) * attenuation
                            totalAmp += Math.sin(dist * config.waveFreq - time + phaseShift) * (20 / (dist + 5));
                        }

                        // Map amplitude to colors (Magenta -> Black -> Cyan -> White)
                        let R = 0, G = 0, B = 0;
                        if (totalAmp > 0) {
                            R = totalAmp * 120; // Starts getting white at high amp
                            G = totalAmp * 255;
                            B = totalAmp * 255;
                        } else {
                            let negAmp = -totalAmp;
                            R = negAmp * 255;
                            G = 0;
                            B = negAmp * 255;
                        }

                        sim.pixels[index++] = p.constrain(R, 0, 255);
                        sim.pixels[index++] = p.constrain(G, 0, 255);
                        sim.pixels[index++] = p.constrain(B, 0, 255);
                        sim.pixels[index++] = 255;

                        // Accumulate right edge
                        if (x === W - 1) {
                            rightEdgeCurrentSum += totalAmp;
                        }
                    }
                }
                sim.updatePixels();

                // Calculate Target Right Edge directly (to ensure perfect match at targetTuning)
                let rightEdgeTargetSum = 0;
                let tTuning = State.targetTuning;
                for (let y = 0; y < H; y++) {
                    let phaseShiftT = 0;
                    let x = W - 1;
                    for (let i = 0; i < config.lenses.length; i++) {
                        let l = config.lenses[i];
                        let dx = x - l.x;
                        let dy = y - l.y;
                        let dist = Math.sqrt(dx*dx + dy*dy);
                        if (dist < l.r) {
                            phaseShiftT += (1 - dist/l.r) * l.strength * tTuning;
                        }
                    }
                    let totalAmpT = 0;
                    for (let i = 0; i < config.sources.length; i++) {
                        let s = config.sources[i];
                        let dx = x - s.x;
                        let dy = y - s.y;
                        let dist = Math.sqrt(dx*dx + dy*dy);
                        totalAmpT += Math.sin(dist * config.waveFreq - time + phaseShiftT) * (20 / (dist + 5));
                    }
                    rightEdgeTargetSum += totalAmpT;
                }

                // Update signal buffers
                Signals.currentWaveHistory.push(rightEdgeCurrentSum);
                Signals.currentWaveHistory.shift();
                
                Signals.targetWaveHistory.push(rightEdgeTargetSum);
                Signals.targetWaveHistory.shift();

                // Draw main simulation
                p.clear();
                if (State.isGlitching && p.frameCount % 4 < 2) {
                    // Random noise burst visually on the canvas during glitch
                    p.image(sim, p.random(-10,10), p.random(-10,10), p.width + 20, p.height + 20);
                } else {
                    p.image(sim, 0, 0, p.width, p.height);
                }

                // Draw Nano-Islands
                p.noFill();
                for (let l of config.lenses) {
                    let sx = (l.x / config.simWidth) * p.width;
                    let sy = (l.y / config.simHeight) * p.height;
                    let sr = ((l.r * 2) / config.simWidth) * p.width;
                    
                    // Outer glow
                    p.strokeWeight(2);
                    p.stroke(138, 43, 226, 150);
                    p.ellipse(sx, sy, sr + Math.sin(time*2)*5);
                    
                    // Core
                    p.strokeWeight(1);
                    p.stroke(200, 100, 255, 200);
                    p.ellipse(sx, sy, sr);
                    
                    // SOT modification visualization (inner rings)
                    let innerR = sr * State.currentTuning;
                    p.stroke(0, 255, 255, 150);
                    p.ellipse(sx, sy, innerR);
                }
                
                // Scanline effect
                p.stroke(0, 50);
                p.strokeWeight(2);
                for(let i=0; i<p.height; i+=4) {
                    p.line(0, i, p.width, i);
                }
            };
        }, 'canvas-container-1');


        // --- P5.JS SKETCH FOR PANEL 2 (OSCILLOSCOPE) ---
        new p5((p) => {
            p.setup = () => {
                let container = document.getElementById('canvas-container-2');
                let canvas = p.createCanvas(container.clientWidth, container.clientHeight);
                canvas.parent(container);
            };

            p.windowResized = () => {
                let container = document.getElementById('canvas-container-2');
                p.resizeCanvas(container.clientWidth, container.clientHeight);
            };

            p.draw = () => {
                p.background(0, 10, 10);
                
                // Draw Grid
                p.stroke(0, 255, 255, 30);
                p.strokeWeight(1);
                for (let x = 0; x < p.width; x += 20) p.line(x, 0, x, p.height);
                for (let y = 0; y < p.height; y += 20) p.line(0, y, p.width, y);
                p.line(0, p.height/2, p.width, p.height/2);

                if (State.isGlitching && p.frameCount % 3 === 0) return; // Drop frames intentionally

                let len = config.historyLen;
                let stepX = p.width / (len - 1);
                
                let scaleY = 15; // Vertical scaling for wave

                // Draw Target Wave (White)
                p.noFill();
                p.strokeWeight(3);
                p.stroke(255, 255, 255, 180);
                p.beginShape();
                for (let i = 0; i < len; i++) {
                    let y = p.height/2 - Signals.targetWaveHistory[i] * scaleY;
                    p.vertex(i * stepX, y);
                }
                p.endShape();

                // Draw Current Wave (Cyan)
                p.strokeWeight(2);
                p.stroke(0, 255, 255, 255);
                
                // If locked, add intense glow
                if (State.isLocked) {
                    p.drawingContext.shadowBlur = 15;
                    p.drawingContext.shadowColor = '#00ffff';
                } else {
                    p.drawingContext.shadowBlur = 0;
                }

                p.beginShape();
                for (let i = 0; i < len; i++) {
                    let y = p.height/2 - Signals.currentWaveHistory[i] * scaleY;
                    p.vertex(i * stepX, y);
                }
                p.endShape();
                
                p.drawingContext.shadowBlur = 0; // Reset

                // Scope scanline overlay
                p.stroke(0, 100);
                p.strokeWeight(2);
                for(let i=0; i<p.height; i+=4) {
                    p.line(0, i, p.width, i);
                }
            };
        }, 'canvas-container-2');

    </script>
</body>
</html>
