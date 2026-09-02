<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BLUE JULES - Juego de Plataformas</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: #0a1a2a;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            padding: 10px;
            overflow: hidden;
        }

        /* --- PANTALLAS --- */

        .pantalla-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(135deg, #002244 0%, #0055a5 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 999;
            color: white;
            text-align: center;
            padding: 20px;
        }

        .pantalla-overlay h1 {
            font-size: 4rem;
            font-weight: 900;
            letter-spacing: 5px;
            margin-bottom: 20px;
            text-transform: uppercase;
            text-shadow: 0 5px 15px rgba(0,0,0,0.5);
        }

        .character-selector {
            display: flex;
            gap: 25px;
            background: rgba(10, 26, 42, 0.6);
            padding: 15px 30px;
            border-radius: 50px;
            border: 2px solid #4a8a9a;
            margin-bottom: 25px;
        }

        .character-option {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
            cursor: pointer;
            padding: 10px 30px;
            border-radius: 20px;
            transition: 0.3s;
            border: 3px solid transparent;
            user-select: none;
        }

        .character-option * {
            pointer-events: none;
        }

        .character-option:hover {
            background: rgba(26, 58, 74, 0.8);
        }

        .character-option.selected {
            border-color: #00cc88;
            background: rgba(26, 58, 74, 0.9);
            box-shadow: 0 0 20px rgba(0, 204, 136, 0.4);
        }

        .character-option .emoji {
            font-size: 4rem;
        }

        .character-option .label {
            color: #aaccdd;
            font-weight: bold;
            font-size: 1.1rem;
        }

        /* --- NIVELES --- */

        .levels-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 15px;
            max-width: 650px;
            width: 100%;
            margin-bottom: 30px;
        }

        .level-card {
            background: rgba(10, 26, 42, 0.7);
            border: 2px solid #4a8a9a;
            border-radius: 15px;
            padding: 15px 10px;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s;
            color: #aaccdd;
        }

        .level-card:hover {
            background: #00cc88;
            color: #0a1a2a;
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 204, 136, 0.4);
        }

        /* --- CONTENEDOR DEL JUEGO --- */

        .game-wrapper {
            background: #162a3a;
            padding: 20px;
            border-radius: 25px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.7);
            border: 2px solid #4a8a9a;
            display: flex;
            flex-direction: column;
            align-items: center;
            position: relative;
        }

        canvas {
            display: block;
            margin: 0 auto;
            border-radius: 16px;
            background: #b3d9e8;
            box-shadow: inset 0 0 30px rgba(0,0,0,0.3);
            width: 800px;
            height: 500px;
        }

        .ui-bar {
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
            padding: 10px 20px;
            background: #0a1a2a;
            border-radius: 50px;
            border: 1px solid #4a8a9a;
            color: white;
            flex-wrap: wrap;
            gap: 10px;
        }

        .ui-item {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 1.1rem;
            font-weight: 600;
        }

        .ui-item .icon {
            font-size: 1.4rem;
        }

        .ui-item .value {
            color: #7fc9d9;
            min-width: 25px;
            text-align: center;
        }

        .level-display {
            background: #0a1a2a;
            padding: 4px 16px;
            border-radius: 30px;
            border: 1px solid #4a8a9a;
            font-weight: bold;
            color: #7fc9d9;
        }

        .controls-info {
            color: #8aaabc;
            font-size: 0.8rem;
            display: flex;
            gap: 10px;
        }

        .controls-info span {
            background: #0a1a2a;
            padding: 3px 10px;
            border-radius: 20px;
            border: 1px solid #3a6a7a;
        }

        .bottom-nav {
            margin-top: 15px;
            width: 100%;
            display: flex;
            justify-content: center;
            gap: 15px;
        }

        .btn {
            border: none;
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: 0.3s;
            font-family: inherit;
            border-radius: 30px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
        }

        .btn:hover {
            transform: scale(1.05);
        }

        .btn-start {
            background: #00cc88;
            font-size: 1.3rem;
            padding: 14px 45px;
            box-shadow: 0 0 30px rgba(0, 204, 136, 0.4);
        }

        .btn-start:hover {
            background: #22eeaa;
            box-shadow: 0 0 50px rgba(0, 204, 136, 0.6);
        }

        .btn-home {
            background: #dc3545;
            font-size: 1rem;
            padding: 10px 30px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .btn-home:hover {
            background: #ff4d5d;
            box-shadow: 0 0 20px rgba(220, 53, 69, 0.5);
        }

        @media (max-width: 850px) {
            canvas {
                width: 100%;
                height: auto;
            }

            .pantalla-overlay h1 {
                font-size: 3rem;
            }

            .ui-bar {
                flex-direction: column;
                text-align: center;
            }

            .character-selector {
                flex-direction: column;
                gap: 10px;
            }

            .levels-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>

<body>

<!-- ==================== INICIO ==================== -->

<div id="pantalla-inicio" class="pantalla-overlay">

    <h1>BLUE JULES</h1>

    <div class="character-selector">

        <div class="character-option selected" id="charRabbit">
            <span class="emoji">🐰</span>
            <span class="label">Conejito</span>
        </div>

    </div>

    <button class="btn btn-start" id="startBtn" type="button">
        ▶ JUGAR
    </button>

</div>


<!-- ==================== SELECCIÓN DE NIVELES ==================== -->

<div id="pantalla-niveles" class="pantalla-overlay" style="display: none;">

    <h1>SELECCIONAR NIVEL</h1>

    <div class="levels-grid" id="levelsContainer"></div>

    <button class="btn btn-home" id="levelsHomeBtn" type="button">
        🏠 INICIO
    </button>

</div>


<!-- ==================== JUEGO ==================== -->

<div class="game-wrapper">

    <canvas id="gameCanvas" width="800" height="500"></canvas>

    <div class="ui-bar">

        <div class="ui-item">
            <span class="icon">❤️</span>
            <span>Vidas:</span>
            <span class="value" id="livesDisplay">3</span>
        </div>

        <div class="ui-item">
            <span class="icon">🍚</span>
            <span>Arroz:</span>
            <span class="value" id="medsDisplay">0</span>
        </div>

        <div class="ui-item">
            <span class="icon">🥕</span>
            <span>Zanahorias:</span>
            <span class="value" id="carrotsDisplay">0</span>
        </div>

        <div class="ui-item">
            <span class="icon">⭐</span>
            <span>Puntos:</span>
            <span class="value" id="scoreDisplay">0</span>
        </div>

        <div class="level-display" id="levelDisplay">
            🏦 Nivel 1
        </div>

        <div class="controls-info">
            <span>⬅ ➡ Moverse</span>
            <span>⬆ Saltar</span>
            <span>R Reiniciar</span>
            <span>I Inicio</span>
        </div>

    </div>

    <div class="bottom-nav">

        <button class="btn btn-home" id="homeBtn" type="button">
            🏠 INICIO
        </button>

    </div>

</div>


<script>

window.addEventListener('load', function() {

    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');

    const livesDisplay = document.getElementById('livesDisplay');
    const medsDisplay = document.getElementById('medsDisplay');
    const carrotsDisplay = document.getElementById('carrotsDisplay');
    const scoreDisplay = document.getElementById('scoreDisplay');
    const levelDisplay = document.getElementById('levelDisplay');

    const startBtn = document.getElementById('startBtn');
    const homeBtn = document.getElementById('homeBtn');
    const levelsHomeBtn = document.getElementById('levelsHomeBtn');

    const pantallaInicio = document.getElementById('pantalla-inicio');
    const pantallaNiveles = document.getElementById('pantalla-niveles');

    const levelsContainer = document.getElementById('levelsContainer');

    const charRabbit = document.getElementById('charRabbit');


    /* ==================== AUDIO ==================== */

    let audioCtx = null;

    function initAudio() {

        if (!audioCtx) {
            audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        }

    }


    function playSound(type) {

        if (!audioCtx) return;

        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();

        osc.connect(gain);
        gain.connect(audioCtx.destination);

        const now = audioCtx.currentTime;


        if (type === 'jump') {

            osc.type = 'sine';

            osc.frequency.setValueAtTime(150, now);
            osc.frequency.exponentialRampToValueAtTime(400, now + 0.15);

            gain.gain.setValueAtTime(0.3, now);
            gain.gain.linearRampToValueAtTime(0.01, now + 0.15);

            osc.start(now);
            osc.stop(now + 0.15);

        }


        else if (type === 'rice') {

            osc.type = 'triangle';

            osc.frequency.setValueAtTime(523.25, now);
            osc.frequency.setValueAtTime(659.25, now + 0.08);

            gain.gain.setValueAtTime(0.2, now);
            gain.gain.linearRampToValueAtTime(0.01, now + 0.2);

            osc.start(now);
            osc.stop(now + 0.2);

        }


        else if (type === 'carrot') {

            osc.type = 'square';

            osc.frequency.setValueAtTime(587.33, now);
            osc.frequency.setValueAtTime(880, now + 0.08);

            gain.gain.setValueAtTime(0.15, now);
            gain.gain.linearRampToValueAtTime(0.01, now + 0.25);

            osc.start(now);
            osc.stop(now + 0.25);

        }


        else if (type === 'damage') {

            osc.type = 'sawtooth';

            osc.frequency.setValueAtTime(220, now);
            osc.frequency.linearRampToValueAtTime(80, now + 0.25);

            gain.gain.setValueAtTime(0.3, now);
            gain.gain.linearRampToValueAtTime(0.01, now + 0.25);

            osc.start(now);
            osc.stop(now + 0.25);

        }


        else if (type === 'gameover') {

            osc.type = 'sawtooth';

            osc.frequency.setValueAtTime(300, now);
            osc.frequency.linearRampToValueAtTime(100, now + 0.6);

            gain.gain.setValueAtTime(0.4, now);
            gain.gain.linearRampToValueAtTime(0.01, now + 0.6);

            osc.start(now);
            osc.stop(now + 0.6);

        }


        else if (type === 'win') {

            osc.type = 'triangle';

            osc.frequency.setValueAtTime(440, now);
            osc.frequency.setValueAtTime(554.37, now + 0.1);
            osc.frequency.setValueAtTime(659.25, now + 0.2);
            osc.frequency.setValueAtTime(880, now + 0.3);

            gain.gain.setValueAtTime(0.3, now);
            gain.gain.linearRampToValueAtTime(0.01, now + 0.5);

            osc.start(now);
            osc.stop(now + 0.5);

        }

    }


    /* ==================== CONFIGURACIÓN ==================== */

    const W = 800;
    const H = 500;

    const GRAVITY = 0.6;
    const JUMP_FORCE = -11;
    const MOVE_SPEED = 4.5;
    const FRICTION = 0.85;

    const TOTAL_LEVELS = 10;


    /* ==================== ESTADO DEL JUEGO ==================== */

    let game = {

        player: {
            x: 80,
            y: 380,
            w: 32,
            h: 44,
            vx: 0,
            vy: 0,
            onGround: false
        },

        platforms: [],
        items: [],
        enemies: [],
        particles: [],

        lives: 3,
        meds: 0,
        carrots: 0,
        score: 0,

        level: 1,

        running: false,
        gameOver: false,
        gameComplete: false,

        character: 'rabbit',

        keys: {
            left: false,
            right: false,
            up: false
        }

    };


    /* ==================== NIVELES ==================== */

    const LEVELS = [

        {
            platforms: [
                {x:0,y:460,w:800,h:40},
                {x:120,y:370,w:100,h:18},
                {x:300,y:310,w:100,h:18},
                {x:500,y:250,w:100,h:18},
                {x:680,y:310,w:100,h:18},
                {x:350,y:400,w:120,h:18}
            ],

            items: [
                {x:160,y:340,type:'med'},
                {x:340,y:280,type:'med'},
                {x:540,y:220,type:'med'},
                {x:720,y:280,type:'med'},
                {x:390,y:370,type:'med'}
            ],

            enemies: [
                {x:250,y:432,vx:1.5,minX:180,maxX:380},
                {x:550,y:432,vx:1.8,minX:480,maxX:680}
            ]
        },


        {
            platforms: [
                {x:0,y:460,w:800,h:40},
                {x:50,y:380,w:80,h:18},
                {x:200,y:320,w:80,h:18},
                {x:350,y:260,w:80,h:18},
                {x:500,y:320,w:80,h:18},
                {x:650,y:380,w:80,h:18},
                {x:400,y:400,w:100,h:18}
            ],

            items: [
                {x:90,y:350,type:'med'},
                {x:240,y:290,type:'med'},
                {x:390,y:230,type:'med'},
                {x:540,y:290,type:'med'},
                {x:690,y:350,type:'med'},
                {x:440,y:370,type:'med'}
            ],

            enemies: [
                {x:150,y:432,vx:1.2,minX:80,maxX:280},
                {x:450,y:432,vx:1.6,minX:350,maxX:550},
                {x:600,y:432,vx:1.0,minX:550,maxX:700}
            ]
        },


        {
            platforms: [
                {x:0,y:460,w:800,h:40},
                {x:30,y:390,w:70,h:18},
                {x:160,y:330,w:70,h:18},
                {x:290,y:270,w:70,h:18},
                {x:420,y:210,w:70,h:18},
                {x:550,y:270,w:70,h:18},
                {x:680,y:330,w:70,h:18},
                {x:300,y:380,w:100,h:18}
            ],

            items: [
                {x:70,y:360,type:'med'},
                {x:200,y:300,type:'med'},
                {x:330,y:240,type:'med'},
                {x:460,y:180,type:'med'},
                {x:590,y:240,type:'med'},
                {x:720,y:300,type:'med'},
                {x:340,y:350,type:'med'}
            ],

            enemies: [
                {x:120,y:432,vx:1.3,minX:50,maxX:250},
                {x:350,y:432,vx:1.7,minX:250,maxX:450},
                {x:580,y:432,vx:1.5,minX:480,maxX:680}
            ]
        },


        {
            platforms: [
                {x:0,y:460,w:800,h:40},
                {x:80,y:380,w:90,h:18},
                {x:250,y:320,w:90,h:18},
                {x:420,y:260,w:90,h:18},
                {x:590,y:320,w:90,h:18},
                {x:200,y:200,w:80,h:18},
                {x:500,y:180,w:80,h:18},
                {x:350,y:400,w:120,h:18}
            ],

            items: [
                {x:120,y:350,type:'med'},
                {x:290,y:290,type:'med'},
                {x:460,y:230,type:'med'},
                {x:630,y:290,type:'med'},
                {x:240,y:170,type:'med'},
                {x:540,y:150,type:'med'},
                {x:390,y:370,type:'med'},
                {x:100,y:420,type:'star'}
            ],

            enemies: [
                {x:200,y:432,vx:1.4,minX:120,maxX:320},
                {x:500,y:432,vx:1.8,minX:400,maxX:600},
                {x:260,y:292,vx:1.2,minX:250,maxX:340}
            ]
        },


        {
            platforms: [
                {x:0,y:460,w:800,h:40},
                {x:50,y:390,w:60,h:18},
                {x:170,y:330,w:60,h:18},
                {x:290,y:270,w:60,h:18},
                {x:410,y:210,w:60,h:18},
                {x:530,y:270,w:60,h:18},
                {x:650,y:330,w:60,h:18},
                {x:770,y:390,w:60,h:18},
                {x:350,y:380,w:100,h:18}
            ],

            items: [
                {x:90,y:360,type:'med'},
                {x:210,y:300,type:'med'},
                {x:330,y:240,type:'med'},
                {x:450,y:180,type:'med'},
                {x:570,y:240,type:'med'},
                {x:690,y:300,type:'med'},
                {x:380,y:350,type:'med'}
            ],

            enemies: [
                {x:130,y:432,vx:1.5,minX:60,maxX:260},
                {x:330,y:432,vx:1.7,minX:230,maxX:430},
                {x:530,y:432,vx:1.9,minX:430,maxX:630}
            ]
        },


        {
            platforms: [
                {x:0,y:460,w:800,h:40},
                {x:100,y:370,w:80,h:18},
                {x:280,y:310,w:80,h:18},
                {x:460,y:250,w:80,h:18},
                {x:640,y:310,w:80,h:18},
                {x:190,y:200,w:80,h:18},
                {x:550,y:180,w:80,h:18},
                {x:370,y:400,w:100,h:18}
            ],

            items: [
                {x:140,y:340,type:'med'},
                {x:320,y:280,type:'med'},
                {x:500,y:220,type:'med'},
                {x:680,y:280,type:'med'},
                {x:230,y:170,type:'med'},
                {x:590,y:150,type:'med'},
                {x:410,y:370,type:'med'}
            ],

            enemies: [
                {x:180,y:432,vx:1.6,minX:110,maxX:310},
                {x:400,y:432,vx:1.8,minX:300,maxX:500},
                {x:620,y:432,vx:2.0,minX:520,maxX:720}
            ]
        },


        {
            platforms: [
                {x:0,y:460,w:800,h:40},
                {x:40,y:400,w:60,h:18},
                {x:160,y:340,w:60,h:18},
                {x:280,y:280,w:60,h:18},
                {x:400,y:220,w:60,h:18},
                {x:520,y:280,w:60,h:18},
                {x:640,y:340,w:60,h:18},
                {x:300,y:150,w:80,h:18}
            ],

            items: [
                {x:80,y:370,type:'med'},
                {x:200,y:310,type:'med'},
                {x:320,y:250,type:'med'},
                {x:440,y:190,type:'med'},
                {x:560,y:250,type:'med'},
                {x:680,y:310,type:'med'},
                {x:340,y:120,type:'med'},
                {x:200,y:420,type:'star'}
            ],

            enemies: [
                {x:120,y:432,vx:1.5,minX:50,maxX:250},
                {x:320,y:432,vx:1.7,minX:220,maxX:420},
                {x:520,y:432,vx:1.9,minX:420,maxX:620}
            ]
        },


        {
            platforms: [
                {x:0,y:460,w:800,h:40},
                {x:60,y:380,w:70,h:18},
                {x:200,y:320,w:70,h:18},
                {x:340,y:260,w:70,h:18},
                {x:480,y:200,w:70,h:18},
                {x:620,y:260,w:70,h:18},
                {x:150,y:170,w:70,h:18}
            ],

            items: [
                {x:100,y:350,type:'med'},
                {x:240,y:290,type:'med'},
                {x:380,y:230,type:'med'},
                {x:520,y:170,type:'med'},
                {x:660,y:230,type:'med'},
                {x:190,y:140,type:'med'}
            ],

            enemies: [
                {x:140,y:432,vx:1.6,minX:70,maxX:270},
                {x:340,y:432,vx:1.8,minX:240,maxX:440},
                {x:540,y:432,vx:2.0,minX:440,maxX:640}
            ]
        },


        {
            platforms: [
                {x:0,y:460,w:800,h:40},
                {x:30,y:400,w:50,h:18},
                {x:140,y:340,w:50,h:18},
                {x:250,y:280,w:50,h:18},
                {x:360,y:220,w:50,h:18},
                {x:470,y:160,w:50,h:18},
                {x:580,y:220,w:50,h:18},
                {x:690,y:280,w:50,h:18}
            ],

            items: [
                {x:70,y:370,type:'med'},
                {x:180,y:310,type:'med'},
                {x:290,y:250,type:'med'},
                {x:400,y:190,type:'med'},
                {x:510,y:130,type:'med'},
                {x:620,y:190,type:'med'},
                {x:730,y:250,type:'med'},
                {x:500,y:420,type:'star'}
            ],

            enemies: [
                {x:100,y:432,vx:1.5,minX:30,maxX:230},
                {x:300,y:432,vx:1.8,minX:200,maxX:400},
                {x:500,y:432,vx:2.0,minX:400,maxX:600}
            ]
        },


        {
            platforms: [
                {x:0,y:460,w:800,h:40},
                {x:60,y:380,w:60,h:18},
                {x:180,y:320,w:60,h:18},
                {x:300,y:260,w:60,h:18},
                {x:420,y:200,w:60,h:18},
                {x:540,y:260,w:60,h:18},
                {x:660,y:320,w:60,h:18},
                {x:340,y:120,w:60,h:18}
            ],

            items: [
                {x:100,y:350,type:'med'},
                {x:220,y:290,type:'med'},
                {x:340,y:230,type:'med'},
                {x:460,y:170,type:'med'},
                {x:580,y:230,type:'med'},
                {x:700,y:290,type:'med'},
                {x:380,y:90,type:'med'},
                {x:400,y:420,type:'star'}
            ],

            enemies: [
                {x:120,y:432,vx:1.6,minX:60,maxX:260},
                {x:320,y:432,vx:1.8,minX:220,maxX:420},
                {x:520,y:432,vx:2.0,minX:420,maxX:620},
                {x:220,y:292,vx:1.3,minX:180,maxX:300}
            ]
        }

    ];


    /* ==================== MENÚ DE NIVELES ==================== */

    for (let i = 1; i <= TOTAL_LEVELS; i++) {

        const btn = document.createElement('div');

        btn.className = 'level-card';

        btn.textContent = `🏦 Nivel ${i}`;

        btn.onclick = function() {

            initAudio();

            game.level = i;

            pantallaNiveles.style.display = 'none';

            loadLevel(i);

        };

        levelsContainer.appendChild(btn);

    }


    /* ==================== CARGAR NIVEL ==================== */

    function loadLevel(levelNum) {

        const data = LEVELS[levelNum - 1];

        if (!data) return;


        game.platforms = data.platforms.map(p => ({ ...p }));


        game.items = data.items.map((item, index) => ({

            ...item,

            w: item.type === 'star' ? 24 : 20,

            h: item.type === 'star' ? 24 : 20,

            collected: false,

            id: index

        }));


        game.enemies = data.enemies.map(e => ({

            ...e,

            w: 28,
            h: 28

        }));


        game.player.x = 80;
        game.player.y = 380;

        game.player.vx = 0;
        game.player.vy = 0;

        game.player.onGround = false;


        game.meds = 0;
        game.carrots = 0;

        game.running = true;

        game.gameOver = false;
        game.gameComplete = false;

        game.particles = [];


        levelDisplay.textContent = `🏦 Nivel ${levelNum}`;

        updateUI();

    }


    /* ==================== PARTÍCULAS ==================== */

    function createParticles(x, y, color, count) {

        for (let i = 0; i < count; i++) {

            game.particles.push({

                x: x,
                y: y,

                vx: (Math.random() - 0.5) * 8,
                vy: (Math.random() - 0.5) * 8,

                life: 30,

                color: color

            });

        }

    }


    /* ==================== SIGUIENTE NIVEL ==================== */

    function nextLevel() {

        if (game.level < TOTAL_LEVELS) {

            game.level++;

            loadLevel(game.level);

            createParticles(
                W / 2,
                H / 2,
                '#66ff88',
                30
            );

        }

        else {

            game.gameComplete = true;

            game.running = false;

            playSound('win');

            createParticles(
                W / 2,
                H / 2,
                '#ffdd44',
                60
            );

        }

    }


    /* ==================== REINICIAR ==================== */

    function resetGame() {

        game.level = 1;

        game.lives = 3;

        game.score = 0;

        game.meds = 0;

        game.carrots = 0;

        game.gameComplete = false;

        loadLevel(1);

    }


    /* ==================== INICIO ==================== */

    function volverAlInicio() {

        game.running = false;

        game.gameOver = false;

        pantallaNiveles.style.display = 'none';

        pantallaInicio.style.display = 'flex';

    }


    /* ==================== UI ==================== */

    function updateUI() {

        livesDisplay.textContent = game.lives;

        medsDisplay.textContent = game.meds;

        carrotsDisplay.textContent = game.carrots;

        scoreDisplay.textContent = game.score;

    }


    /* ==================== ACTUALIZAR JUEGO ==================== */

    function update() {

        if (!game.running) return;


        /* Movimiento */

        if (game.keys.left) {

            game.player.vx = -MOVE_SPEED;

        }

        else if (game.keys.right) {

            game.player.vx = MOVE_SPEED;

        }

        else {

            game.player.vx *= FRICTION;

        }


        /* Salto */

        if (game.keys.up && game.player.onGround) {

            game.player.vy = JUMP_FORCE;

            game.player.onGround = false;

            playSound('jump');

        }


        /* Gravedad */

        game.player.vy += GRAVITY;

        game.player.x += game.player.vx;
        game.player.y += game.player.vy;


        game.player.onGround = false;


        /* Plataformas */

        for (let p of game.platforms) {

            if (
                game.player.x + game.player.w > p.x &&
                game.player.x < p.x + p.w &&
                game.player.y + game.player.h >= p.y &&
                game.player.y + game.player.h <= p.y + p.h + game.player.vy
            ) {

                game.player.y = p.y - game.player.h;

                game.player.vy = 0;

                game.player.onGround = true;

            }

        }


        /* Límites */

        if (game.player.x < 0)
            game.player.x = 0;

        if (game.player.x + game.player.w > W)
            game.player.x = W - game.player.w;


        /* Caída */

        if (game.player.y > H) {

            takeDamage();

        }


        /* Enemigos */

        for (let e of game.enemies) {

            e.x += e.vx;


            if (
                e.x < e.minX ||
                e.x > e.maxX
            ) {

                e.vx *= -1;

            }


            if (
                game.player.x < e.x + e.w &&
                game.player.x + game.player.w > e.x &&
                game.player.y < e.y + e.h &&
                game.player.y + game.player.h > e.y
            ) {

                takeDamage();

            }

        }


        /* Recolección */

        let totalMeds = game.items.filter(
            i => i.type === 'med'
        ).length;

        let collectedMeds = 0;


        for (let item of game.items) {

            if (
                !item.collected &&
                game.player.x < item.x + item.w &&
                game.player.x + game.player.w > item.x &&
                game.player.y < item.y + item.h &&
                game.player.y + game.player.h > item.y
            ) {

                item.collected = true;


                if (item.type === 'med') {

                    game.meds++;

                    game.score += 50;

                    playSound('rice');

                    createParticles(
                        item.x,
                        item.y,
                        '#7fc9d9',
                        10
                    );

                }


                else if (item.type === 'star') {

                    game.carrots++;

                    game.score += 200;

                    playSound('carrot');

                    createParticles(
                        item.x,
                        item.y,
                        '#ffdd44',
                        15
                    );

                }


                updateUI();

            }


            if (
                item.type === 'med' &&
                item.collected
            ) {

                collectedMeds++;

            }

        }


        /* Completar nivel */

        if (
            collectedMeds === totalMeds &&
            totalMeds > 0
        ) {

            nextLevel();

        }


        /* Partículas */

        for (
            let i = game.particles.length - 1;
            i >= 0;
            i--
        ) {

            let p = game.particles[i];

            p.x += p.vx;
            p.y += p.vy;

            p.life--;


            if (p.life <= 0) {

                game.particles.splice(i, 1);

            }

        }

    }


    /* ==================== DAÑO ==================== */

    function takeDamage() {

        game.lives--;

        updateUI();


        createParticles(
            game.player.x,
            game.player.y,
            '#ff4444',
            20
        );


        if (game.lives <= 0) {

            game.gameOver = true;

            game.running = false;

            playSound('gameover');

        }

        else {

            playSound('damage');

            game.player.x = 80;
            game.player.y = 380;

            game.player.vx = 0;
            game.player.vy = 0;

        }

    }


    /* ==================== DIBUJAR ==================== */

    function draw() {

        ctx.clearRect(0, 0, W, H);


        /* Fondo */

        ctx.fillStyle = '#b3d9e8';

        ctx.fillRect(
            0,
            0,
            W,
            H
        );


        /* Plataformas */

        ctx.fillStyle = '#2c5a6b';


        for (let p of game.platforms) {

            ctx.fillRect(
                p.x,
                p.y,
                p.w,
                p.h
            );


            ctx.fillStyle = '#4a8a9a';


            ctx.fillRect(
                p.x,
                p.y,
                p.w,
                4
            );


            ctx.fillStyle = '#2c5a6b';

        }


        /* Objetos */

        for (let item of game.items) {

            if (!item.collected) {

                ctx.font = '20px Arial';


                if (item.type === 'med') {

                    ctx.fillText(
                        '🍚',
                        item.x,
                        item.y + 18
                    );

                }

                else {

                    ctx.fillText(
                        '🥕',
                        item.x,
                        item.y + 18
                    );

                }

            }

        }


        /* Enemigos */

        for (let e of game.enemies) {

            ctx.font = '24px Arial';

            ctx.fillText(
                '🛁',
                e.x,
                e.y + 22
            );

        }


        /* Conejo */

        if (
            game.running ||
            game.gameOver
        ) {

            ctx.font = '36px Arial';

            ctx.fillText(
                '🐰',
                game.player.x - 4,
                game.player.y + 36
            );

        }


        /* Partículas */

        for (let p of game.particles) {

            ctx.fillStyle = p.color;

            ctx.fillRect(
                p.x,
                p.y,
                4,
                4
            );

        }


        /* GAME OVER */

        if (game.gameOver) {

            ctx.fillStyle = 'rgba(0,0,0,0.7)';

            ctx.fillRect(
                0,
                0,
                W,
                H
            );


            ctx.fillStyle = '#ff4444';

            ctx.font = 'bold 40px Arial';

            ctx.textAlign = 'center';


            ctx.fillText(
                '¡GAME OVER!',
                W / 2,
                H / 2 - 30
            );


            ctx.fillStyle = '#ffffff';

            ctx.font = '20px Arial';


            ctx.fillText(
                'Presiona "R" para Reiniciar',
                W / 2,
                H / 2 + 15
            );


            ctx.fillStyle = '#7fc9d9';

            ctx.font = 'bold 20px Arial';


            ctx.fillText(
                'Presiona "I" para ir a Inicio',
                W / 2,
                H / 2 + 55
            );


            ctx.textAlign = 'left';

        }


        /* VICTORIA */

        else if (game.gameComplete) {

            ctx.fillStyle = 'rgba(0,0,0,0.7)';

            ctx.fillRect(
                0,
                0,
                W,
                H
            );


            ctx.fillStyle = '#00cc88';

            ctx.font = 'bold 34px Arial';

            ctx.textAlign = 'center';


            ctx.fillText(
                '¡FELICIDADES!',
                W / 2,
                H / 2 - 45
            );


            ctx.fillText(
                '¡BLUE JULES COMPLETADO!',
                W / 2,
                H / 2
            );


            ctx.fillStyle = '#ffffff';

            ctx.font = '20px Arial';


            ctx.fillText(
                `Puntos: ${game.score}`,
                W / 2,
                H / 2 + 40
            );


            ctx.fillStyle = '#7fc9d9';

            ctx.font = 'bold 20px Arial';


            ctx.fillText(
                'Presiona "I" para ir a Inicio',
                W / 2,
                H / 2 + 80
            );


            ctx.textAlign = 'left';

        }

    }


    /* ==================== LOOP ==================== */

    function loop() {

        update();

        draw();

        requestAnimationFrame(loop);

    }


    /* ==================== SELECCIÓN DEL CONEJO ==================== */

    charRabbit.addEventListener(
        'click',
        function() {

            charRabbit.classList.add('selected');

            game.character = 'rabbit';

        }
    );


    /* ==================== BOTÓN JUGAR ==================== */

    startBtn.addEventListener(
        'click',
        function() {

            initAudio();

            pantallaInicio.style.display = 'none';

            pantallaNiveles.style.display = 'flex';

        }
    );


    /* ==================== BOTONES INICIO ==================== */

    levelsHomeBtn.addEventListener(
        'click',
        function() {

            volverAlInicio();

        }
    );


    homeBtn.addEventListener(
        'click',
        function() {

            volverAlInicio();

        }
    );


    /* ==================== TECLADO ==================== */

    window.addEventListener(
        'keydown',
        e => {

            initAudio();


            if (
                e.key === 'ArrowLeft' ||
                e.key === 'a' ||
                e.key === 'A'
            ) {

                game.keys.left = true;

            }


            if (
                e.key === 'ArrowRight' ||
                e.key === 'd' ||
                e.key === 'D'
            ) {

                game.keys.right = true;

            }


            if (
                e.key === 'ArrowUp' ||
                e.key === 'w' ||
                e.key === 'W' ||
                e.key === ' '
            ) {

                game.keys.up = true;

            }


            /* Reiniciar */

            if (
                e.key === 'r' ||
                e.key === 'R'
            ) {

                if (
                    game.gameOver ||
                    game.running
                ) {

                    resetGame();

                }

            }


            /* Inicio */

            if (
                e.key === 'i' ||
                e.key === 'I'
            ) {

                volverAlInicio();

            }

        }
    );


    window.addEventListener(
        'keyup',
        e => {

            if (
                e.key === 'ArrowLeft' ||
                e.key === 'a' ||
                e.key === 'A'
            ) {

                game.keys.left = false;

            }


            if (
                e.key === 'ArrowRight' ||
                e.key === 'd' ||
                e.key === 'D'
            ) {

                game.keys.right = false;

            }


            if (
                e.key === 'ArrowUp' ||
                e.key === 'w' ||
                e.key === 'W' ||
                e.key === ' '
            ) {

                game.keys.up = false;

            }

        }
    );


    /* ==================== INICIAR LOOP ==================== */

    loop();

});

</script>

</body>
</html>