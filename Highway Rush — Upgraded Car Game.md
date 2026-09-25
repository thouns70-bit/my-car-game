```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport"
      content="width=device-width, initial-scale=1.0,
      maximum-scale=1.0, user-scalable=no">

<title>Neon Highway Rush</title>

<style>
/* =========================================================
   GLOBAL
========================================================= */

* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

html,
body {
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #05070d;
    font-family:
        Inter,
        Arial,
        Helvetica,
        sans-serif;
}

body {
    display: flex;
    align-items: center;
    justify-content: center;
}

#game {
    position: relative;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
    background: #05070d;
}

canvas {
    display: block;
    width: 100%;
    height: 100%;
}


/* =========================================================
   TOP HUD
========================================================= */

#hud {
    position: absolute;
    top: 18px;
    left: 18px;
    right: 18px;

    display: flex;
    justify-content: space-between;
    align-items: flex-start;

    pointer-events: none;
    z-index: 20;
}

.hud-left,
.hud-right {
    display: flex;
    gap: 10px;
}

.hud-card {
    min-width: 125px;

    padding: 10px 15px;

    border: 1px solid rgba(255,255,255,.12);
    border-radius: 15px;

    background:
        linear-gradient(
            135deg,
            rgba(15,20,35,.88),
            rgba(8,11,20,.72)
        );

    box-shadow:
        0 10px 30px rgba(0,0,0,.25),
        inset 0 1px rgba(255,255,255,.05);

    backdrop-filter: blur(12px);

    color: white;
}

.hud-label {
    display: block;

    color: #7f8ba5;

    font-size: 9px;
    font-weight: 800;

    letter-spacing: 1.5px;
    text-transform: uppercase;
}

.hud-number {
    display: block;

    margin-top: 2px;

    color: white;

    font-size: 21px;
    font-weight: 900;
}

#speedNumber {
    color: #35d9ff;
}


/* =========================================================
   SCREEN OVERLAYS
========================================================= */

.overlay {
    position: absolute;
    inset: 0;

    display: flex;
    align-items: center;
    justify-content: center;

    padding: 25px;

    background:
        radial-gradient(
            circle at center,
            rgba(20,30,70,.35),
            rgba(2,4,10,.88)
        );

    z-index: 50;
}

.overlay.hidden {
    display: none;
}

.panel {
    width: min(520px, 94vw);

    padding: 42px 32px;

    text-align: center;

    border: 1px solid rgba(255,255,255,.12);
    border-radius: 28px;

    background:
        linear-gradient(
            145deg,
            rgba(16,22,40,.95),
            rgba(6,9,17,.96)
        );

    box-shadow:
        0 30px 100px rgba(0,0,0,.65),
        inset 0 1px rgba(255,255,255,.06);
}

.logo {
    margin-bottom: 8px;

    color: white;

    font-size: clamp(42px, 9vw, 78px);
    font-weight: 1000;

    line-height: .95;
    letter-spacing: -3px;

    text-shadow:
        0 0 12px rgba(53,217,255,.45),
        0 0 45px rgba(53,217,255,.2);
}

.logo span {
    color: #35d9ff;
}

.tagline {
    margin-bottom: 28px;

    color: #8490a8;

    font-size: 14px;
    letter-spacing: 2px;
    text-transform: uppercase;
}

.description {
    margin: 0 auto 28px;

    max-width: 400px;

    color: #adb6c9;

    font-size: 15px;
    line-height: 1.7;
}

.main-button {
    width: 100%;
    max-width: 280px;

    padding: 16px 25px;

    border: 0;
    border-radius: 14px;

    color: #041018;

    background:
        linear-gradient(
            135deg,
            #5ce5ff,
            #35a8ff
        );

    box-shadow:
        0 10px 35px rgba(53,217,255,.25);

    font-size: 15px;
    font-weight: 900;

    cursor: pointer;

    transition:
        transform .15s,
        box-shadow .15s;
}

.main-button:hover {
    transform: translateY(-2px);

    box-shadow:
        0 15px 45px rgba(53,217,255,.38);
}

.main-button:active {
    transform: scale(.97);
}

.controls {
    display: flex;
    justify-content: center;
    gap: 25px;

    margin-top: 25px;

    color: #758198;

    font-size: 12px;
}

.control-key {
    display: inline-block;

    margin-bottom: 5px;

    padding: 5px 9px;

    border-radius: 6px;

    color: #cbd5e8;

    background: rgba(255,255,255,.07);

    font-weight: 800;
}


/* =========================================================
   COUNTDOWN
========================================================= */

#countdown {
    position: absolute;
    inset: 0;

    display: flex;
    align-items: center;
    justify-content: center;

    z-index: 30;

    pointer-events: none;
}

#countdown.hidden {
    display: none;
}

#countdownNumber {
    color: white;

    font-size: clamp(80px, 18vw, 170px);
    font-weight: 1000;

    text-shadow:
        0 0 25px #35d9ff,
        0 0 80px rgba(53,217,255,.5);

    animation: countdownPop .65s ease;
}

@keyframes countdownPop {
    0% {
        transform: scale(1.5);
        opacity: 0;
    }

    35% {
        opacity: 1;
    }

    100% {
        transform: scale(1);
        opacity: 0;
    }
}


/* =========================================================
   PAUSE
========================================================= */

#pauseButton {
    position: absolute;

    top: 18px;
    left: 50%;

    transform: translateX(-50%);

    z-index: 25;

    width: 42px;
    height: 42px;

    border: 1px solid rgba(255,255,255,.12);
    border-radius: 12px;

    color: white;
    background: rgba(5,8,16,.72);

    cursor: pointer;

    backdrop-filter: blur(10px);
}

#pauseOverlay {
    z-index: 45;
}


/* =========================================================
   MOBILE CONTROLS
========================================================= */

#mobileControls {
    position: absolute;

    left: 0;
    right: 0;
    bottom: 20px;

    display: none;

    justify-content: space-between;

    padding: 0 20px;

    z-index: 25;

    pointer-events: none;
}

.mobile-side {
    display: flex;
    gap: 10px;

    pointer-events: auto;
}

.mobile-button {
    width: 65px;
    height: 65px;

    border: 1px solid rgba(255,255,255,.16);
    border-radius: 20px;

    color: white;

    background:
        rgba(8,12,23,.7);

    box-shadow:
        0 10px 30px rgba(0,0,0,.35);

    backdrop-filter: blur(10px);

    font-size: 23px;
    font-weight: bold;

    touch-action: none;
    user-select: none;
}

.mobile-button:active {
    background: rgba(53,217,255,.25);
    border-color: #35d9ff;
}


/* =========================================================
   GAME OVER STATS
========================================================= */

.stats {
    display: grid;
    grid-template-columns: 1fr 1fr;

    gap: 10px;

    margin: 25px 0;
}

.stat {
    padding: 14px;

    border-radius: 13px;

    background: rgba(255,255,255,.045);
}

.stat small {
    display: block;

    color: #718099;

    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 1px;
}

.stat strong {
    display: block;

    margin-top: 4px;

    color: white;

    font-size: 24px;
}


/* =========================================================
   RESPONSIVE
========================================================= */

@media (max-width: 700px) {

    #hud {
        top: 12px;
        left: 10px;
        right: 10px;
    }

    .hud-left,
    .hud-right {
        gap: 6px;
    }

    .hud-card {
        min-width: 82px;
        padding: 8px 9px;
    }

    .hud-number {
        font-size: 16px;
    }

    .hud-label {
        font-size: 8px;
    }

    #pauseButton {
        display: none;
    }

    #mobileControls {
        display: flex;
    }

    .panel {
        padding: 32px 22px;
    }

    .controls {
        display: none;
    }
}
</style>
</head>


<body>

<div id="game">

    <canvas id="canvas"></canvas>


    <!-- HUD -->

    <div id="hud">

        <div class="hud-left">

            <div class="hud-card">
                <span class="hud-label">Score</span>
                <span
                    id="score"
                    class="hud-number">
                    0
                </span>
            </div>

            <div class="hud-card">
                <span class="hud-label">Distance</span>
                <span
                    id="distance"
                    class="hud-number">
                    0 m
                </span>
            </div>

        </div>


        <div class="hud-right">

            <div class="hud-card">
                <span class="hud-label">Speed</span>
                <span
                    id="speed"
                    class="hud-number">
                    0
                </span>
            </div>

        </div>

    </div>


    <!-- Pause button -->

    <button id="pauseButton">
        II
    </button>


    <!-- Start -->

    <div
        id="startScreen"
        class="overlay">

        <div class="panel">

            <div class="logo">
                NEON<br>
                <span>RUSH</span>
            </div>

            <div class="tagline">
                Highway Racing
            </div>

            <div class="description">
                Dodge traffic, push your speed to the limit,
                and survive as long as possible.
            </div>

            <button
                id="startButton"
                class="main-button">
                START RACE
            </button>

            <div class="controls">

                <div>
                    <div class="control-key">
                        A / D
                    </div>
                    <br>
                    STEER
                </div>

                <div>
                    <div class="control-key">
                        W
                    </div>
                    <br>
                    BOOST
                </div>

                <div>
                    <div class="control-key">
                        S
                    </div>
                    <br>
                    BRAKE
                </div>

            </div>

        </div>

    </div>


    <!-- Countdown -->

    <div
        id="countdown"
        class="hidden">

        <div id="countdownNumber">
            3
        </div>

    </div>


    <!-- Pause -->

    <div
        id="pauseOverlay"
        class="overlay hidden">

        <div class="panel">

            <div class="logo"
                 style="font-size:55px;">
                PAUSED
            </div>

            <div class="description">
                Take a break. Your race is waiting.
            </div>

            <button
                id="resumeButton"
                class="main-button">
                RESUME
            </button>

        </div>

    </div>


    <!-- Game over -->

    <div
        id="gameOverScreen"
        class="overlay hidden">

        <div class="panel">

            <div class="logo"
                 style="font-size:52px;">
                CRASHED
            </div>

            <div class="tagline">
                Race finished
            </div>

            <div class="stats">

                <div class="stat">
                    <small>Score</small>
                    <strong id="finalScore">
                        0
                    </strong>
                </div>

                <div class="stat">
                    <small>Distance</small>
                    <strong id="finalDistance">
                        0 m
                    </strong>
                </div>

                <div class="stat">
                    <small>Top Speed</small>
                    <strong id="finalSpeed">
                        0
                    </strong>
                </div>

                <div class="stat">
                    <small>Best</small>
                    <strong id="bestScore">
                        0
                    </strong>
                </div>

            </div>

            <button
                id="restartButton"
                class="main-button">
                RACE AGAIN
            </button>

        </div>

    </div>


    <!-- Mobile controls -->

    <div id="mobileControls">

        <div class="mobile-side">

            <button
                id="left"
                class="mobile-button">
                ←
            </button>

            <button
                id="right"
                class="mobile-button">
                →
            </button>

        </div>

        <div class="mobile-side">

            <button
                id="brake"
                class="mobile-button">
                ↓
            </button>

            <button
                id="boost"
                class="mobile-button">
                ↑
            </button>

        </div>

    </div>

</div>


<script>
"use strict";

/* =========================================================
   CONFIG
========================================================= */

const CONFIG = {

    ROAD_RATIO: 0.60,

    LANES: 4,

    PLAYER_WIDTH: 46,
    PLAYER_HEIGHT: 82,

    PLAYER_START_SPEED: 320,
    PLAYER_MAX_SPEED: 900,

    ACCELERATION: 460,
    BRAKING: 720,
    FRICTION: 150,

    STEERING: 430,

    TRAFFIC_WIDTH: 44,
    TRAFFIC_HEIGHT: 80,

    TRAFFIC_MIN_SPEED: 220,
    TRAFFIC_MAX_SPEED: 440,

    START_SPAWN: 1.05,

    MIN_SPAWN: .38,

    MAX_TRAFFIC: 9,

    DIFFICULTY_TIME: 9,

    ROAD_DASH_LENGTH: 70,
    ROAD_DASH_GAP: 45,

    MAX_DT: .033

};


/* =========================================================
   CANVAS
========================================================= */

const canvas =
    document.getElementById("canvas");

const ctx =
    canvas.getContext("2d");

let width = 0;
let height = 0;

let dpr = 1;

let roadLeft = 0;
let roadRight = 0;
let roadWidth = 0;


/* =========================================================
   DOM
========================================================= */

const startScreen =
    document.getElementById("startScreen");

const gameOverScreen =
    document.getElementById("gameOverScreen");

const pauseOverlay =
    document.getElementById("pauseOverlay");

const countdown =
    document.getElementById("countdown");

const countdownNumber =
    document.getElementById("countdownNumber");

const scoreElement =
    document.getElementById("score");

const distanceElement =
    document.getElementById("distance");

const speedElement =
    document.getElementById("speed");

const finalScore =
    document.getElementById("finalScore");

const finalDistance =
    document.getElementById("finalDistance");

const finalSpeed =
    document.getElementById("finalSpeed");

const bestScore =
    document.getElementById("bestScore");


/* =========================================================
   GAME STATE
========================================================= */

const STATE = {
    MENU: "menu",
    COUNTDOWN: "countdown",
    PLAYING: "playing",
    PAUSED: "paused",
    GAME_OVER: "gameover"
};

let state = STATE.MENU;


/* =========================================================
   INPUT
========================================================= */

const input = {

    left: false,
    right: false,
    boost: false,
    brake: false

};


/* =========================================================
   PLAYER
========================================================= */

const player = {

    x: 0,
    y: 0,

    width: CONFIG.PLAYER_WIDTH,
    height: CONFIG.PLAYER_HEIGHT,

    speed: CONFIG.PLAYER_START_SPEED,

    maxSpeedReached:
        CONFIG.PLAYER_START_SPEED,

    tilt: 0

};


/* =========================================================
   GAME DATA
========================================================= */

let traffic = [];

let particles = [];

let elapsed = 0;

let distance = 0;

let score = 0;

let difficulty = 1;

let spawnTimer = 0;

let roadOffset = 0;

let shake = 0;

let lastTime = 0;

let countdownTimer = 0;


/* =========================================================
   TRAFFIC COLORS
========================================================= */

const CAR_COLORS = [
    "#ff4757",
    "#ffa502",
    "#2ed573",
    "#1e90ff",
    "#a55eea",
    "#ff6b81",
    "#70a1ff",
    "#ffffff"
];


/* =========================================================
   RESIZE
========================================================= */

function resize() {

    dpr =
        Math.min(
            window.devicePixelRatio || 1,
            2
        );

    width =
        window.innerWidth;

    height =
        window.innerHeight;

    canvas.width =
        width * dpr;

    canvas.height =
        height * dpr;

    canvas.style.width =
        width + "px";

    canvas.style.height =
        height + "px";

    ctx.setTransform(
        dpr,
        0,
        0,
        dpr,
        0,
        0
    );


    roadWidth =
        Math.max(
            280,
            Math.min(
                width * CONFIG.ROAD_RATIO,
                width - 30
            )
        );

    roadLeft =
        (width - roadWidth) / 2;

    roadRight =
        roadLeft + roadWidth;


    player.y =
        height -
        player.height -
        65;


    keepPlayerOnRoad();

}


/* =========================================================
   INPUT
========================================================= */

function setupKeyboard() {

    window.addEventListener(
        "keydown",
        event => {

            const key =
                event.key.toLowerCase();

            if (
                [
                    "arrowleft",
                    "arrowright",
                    "arrowup",
                    "arrowdown",
                    "a",
                    "d",
                    "w",
                    "s",
                    " "
                ].includes(key)
            ) {
                event.preventDefault();
            }


            if (
                key === "a" ||
                key === "arrowleft"
            ) {
                input.left = true;
            }

            if (
                key === "d" ||
                key === "arrowright"
            ) {
                input.right = true;
            }

            if (
                key === "w" ||
                key === "arrowup"
            ) {
                input.boost = true;
            }

            if (
                key === "s" ||
                key === "arrowdown"
            ) {
                input.brake = true;
            }

            if (
                key === " " &&
                state === STATE.PLAYING
            ) {
                pauseGame();
            }

        }
    );


    window.addEventListener(
        "keyup",
        event => {

            const key =
                event.key.toLowerCase();

            if (
                key === "a" ||
                key === "arrowleft"
            ) {
                input.left = false;
            }

            if (
                key === "d" ||
                key === "arrowright"
            ) {
                input.right = false;
            }

            if (
                key === "w" ||
                key === "arrowup"
            ) {
                input.boost = false;
            }

            if (
                key === "s" ||
                key === "arrowdown"
            ) {
                input.brake = false;
            }

        }
    );


    window.addEventListener(
        "blur",
        () => {

            clearInput();

            if (
                state === STATE.PLAYING
            ) {
                pauseGame();
            }

        }
    );

}


function clearInput() {

    input.left = false;
    input.right = false;
    input.boost = false;
    input.brake = false;

}


/* =========================================================
   MOBILE INPUT
========================================================= */

function bindTouch(id, property) {

    const element =
        document.getElementById(id);

    if (!element) {
        return;
    }


    const press = event => {

        event.preventDefault();

        input[property] = true;

    };


    const release = event => {

        event.preventDefault();

        input[property] = false;

    };


    element.addEventListener(
        "pointerdown",
        press
    );

    element.addEventListener(
        "pointerup",
        release
    );

    element.addEventListener(
        "pointercancel",
        release
    );

    element.addEventListener(
        "pointerleave",
        release
    );

}


function setupTouch() {

    bindTouch("left", "left");
    bindTouch("right", "right");
    bindTouch("boost", "boost");
    bindTouch("brake", "brake");

}


/* =========================================================
   START GAME
========================================================= */

function startGame() {

    elapsed = 0;
    distance = 0;
    score = 0;

    difficulty = 1;

    spawnTimer = 0;

    roadOffset = 0;

    shake = 0;

    traffic = [];
    particles = [];

    player.speed =
        CONFIG.PLAYER_START_SPEED;

    player.maxSpeedReached =
        CONFIG.PLAYER_START_SPEED;

    player.x =
        width / 2;

    player.y =
        height -
        player.height -
        65;


    state =
        STATE.COUNTDOWN;

    startScreen.classList.add("hidden");
    gameOverScreen.classList.add("hidden");
    pauseOverlay.classList.add("hidden");

    runCountdown();

}


/* =========================================================
   COUNTDOWN
========================================================= */

function runCountdown() {

    let number = 3;

    countdown.classList.remove("hidden");

    countdownNumber.textContent =
        number;

    countdownNumber.style.animation =
        "none";

    void countdownNumber.offsetWidth;

    countdownNumber.style.animation =
        "countdownPop .65s ease";


    const timer =
        setInterval(() => {

            number--;

            if (number <= 0) {

                clearInterval(timer);

                countdownNumber.textContent =
                    "GO!";

                countdownNumber.style.animation =
                    "none";

                void countdownNumber.offsetWidth;

                countdownNumber.style.animation =
                    "countdownPop .65s ease";


                setTimeout(() => {

                    countdown.classList.add("hidden");

                    state =
                        STATE.PLAYING;

                }, 500);

                return;

            }


            countdownNumber.textContent =
                number;

            countdownNumber.style.animation =
                "none";

            void countdownNumber.offsetWidth;

            countdownNumber.style.animation =
                "countdownPop .65s ease";

        }, 700);

}


/* =========================================================
   PLAYER PHYSICS
========================================================= */

function updatePlayer(dt) {

    const maxSpeed =
        CONFIG.PLAYER_MAX_SPEED +
        difficulty * 20;


    if (input.boost) {

        player.speed +=
            CONFIG.ACCELERATION *
            1.35 *
            dt;

    } else {

        player.speed -=
            CONFIG.FRICTION *
            dt;

    }


    if (input.brake) {

        player.speed -=
            CONFIG.BRAKING *
            dt;

    }


    player.speed =
        Math.max(
            100,
            Math.min(
                maxSpeed,
                player.speed
            )
        );


    let direction = 0;

    if (input.left) {
        direction--;
    }

    if (input.right) {
        direction++;
    }


    const steering =
        CONFIG.STEERING *
        (
            .75 +
            player.speed /
            CONFIG.PLAYER_MAX_SPEED
        );


    player.x +=
        direction *
        steering *
        dt;


    player.tilt +=
        (
            direction * .22 -
            player.tilt
        ) *
        10 *
        dt;


    keepPlayerOnRoad();


    player.maxSpeedReached =
        Math.max(
            player.maxSpeedReached,
            player.speed
        );

}


/* =========================================================
   ROAD BOUNDARY
========================================================= */

function keepPlayerOnRoad() {

    const margin = 15;

    const min =
        roadLeft +
        margin +
        player.width / 2;

    const max =
        roadRight -
        margin -
        player.width / 2;


    if (player.x < min) {

        player.x = min;

        player.speed *= .98;

    }


    if (player.x > max) {

        player.x = max;

        player.speed *= .98;

    }

}


/* =========================================================
   DIFFICULTY
========================================================= */

function updateDifficulty() {

    difficulty =
        1 +
        Math.floor(
            elapsed /
            CONFIG.DIFFICULTY_TIME
        );

}


/* =========================================================
   TRAFFIC
========================================================= */

function laneX(lane) {

    const laneWidth =
        roadWidth /
        CONFIG.LANES;

    return (
        roadLeft +
        laneWidth * lane +
        laneWidth / 2
    );

}


function spawnTraffic() {

    if (
        traffic.length >=
        CONFIG.MAX_TRAFFIC
    ) {
        return;
    }


    let lane = -1;


    for (
        let attempt = 0;
        attempt < 10;
        attempt++
    ) {

        const candidate =
            Math.floor(
                Math.random() *
                CONFIG.LANES
            );


        const blocked =
            traffic.some(car =>

                car.lane === candidate &&
                car.y < 190

            );


        if (!blocked) {

            lane =
                candidate;

            break;

        }

    }


    if (lane < 0) {
        return;
    }


    const speed =
        CONFIG.TRAFFIC_MIN_SPEED +
        Math.random() *
        (
            CONFIG.TRAFFIC_MAX_SPEED -
            CONFIG.TRAFFIC_MIN_SPEED
        ) +
        difficulty * 12;


    traffic.push({

        x: laneX(lane),

        y: -100,

        width: CONFIG.TRAFFIC_WIDTH,

        height: CONFIG.TRAFFIC_HEIGHT,

        lane,

        speed,

        color:
            CAR_COLORS[
                Math.floor(
                    Math.random() *
                    CAR_COLORS.length
                )
            ],

        wobble:
            Math.random() * Math.PI * 2

    });

}


function updateTraffic(dt) {

    const interval =
        Math.max(
            CONFIG.MIN_SPAWN,
            CONFIG.START_SPAWN -
            difficulty * .055
        );


    spawnTimer += dt;


    if (spawnTimer >= interval) {

        spawnTimer = 0;

        spawnTraffic();

    }


    for (
        let i = traffic.length - 1;
        i >= 0;
        i--
    ) {

        const car =
            traffic[i];


        car.y +=
            (
                player.speed -
                car.speed
            ) *
            dt;


        car.wobble +=
            dt * 2;


        if (
            car.y >
            height + 150
        ) {

            traffic.splice(i, 1);

            score += 25;

        }

    }

}


/* =========================================================
   COLLISION
========================================================= */

function collision(a, b) {

    const padding = 8;

    return (

        a.x - a.width / 2 + padding
        <
        b.x + b.width / 2 - padding

        &&

        a.x + a.width / 2 - padding
        >
        b.x - b.width / 2 + padding

        &&

        a.y - a.height / 2 + padding
        <
        b.y + b.height / 2 - padding

        &&

        a.y + a.height / 2 - padding
        >
        b.y - b.height / 2 + padding

    );

}


function checkCollision() {

    for (const car of traffic) {

        if (
            collision(
                player,
                car
            )
        ) {

            crash();

            return;

        }

    }

}


/* =========================================================
   CRASH
========================================================= */

function crash() {

    if (
        state === STATE.GAME_OVER
    ) {
        return;
    }


    state =
        STATE.GAME_OVER;


    shake = 18;


    createExplosion(
        player.x,
        player.y
    );


    const oldBest =
        Number(
            localStorage.getItem(
                "neonRushBest"
            ) || 0
        );


    const newBest =
        Math.max(
            oldBest,
            Math.floor(score)
        );


    localStorage.setItem(
        "neonRushBest",
        newBest
    );


    finalScore.textContent =
        Math.floor(score);


    finalDistance.textContent =
        Math.floor(distance) +
        " m";


    finalSpeed.textContent =
        Math.floor(
            player.maxSpeedReached * .28
        ) +
        " km/h";


    bestScore.textContent =
        newBest;


    gameOverScreen.classList.remove(
        "hidden"
    );

}


/* =========================================================
   SCORE
========================================================= */

function updateScore(dt) {

    distance +=
        player.speed *
        dt *
        .06;


    score +=
        player.speed *
        dt *
        .09;

}


/* =========================================================
   PARTICLES
========================================================= */

function createParticle(
    x,
    y,
    color,
    size,
    speed
) {

    particles.push({

        x,
        y,

        vx:
            (Math.random() - .5) *
            speed,

        vy:
            Math.random() *
            speed,

        life: 1,

        size,

        color

    });

}


function createExhaust() {

    if (
        state !== STATE.PLAYING
    ) {
        return;
    }


    if (
        Math.random() > .5
    ) {
        return;
    }


    createParticle(
        player.x - 12,
        player.y + 35,
        "#35d9ff",
        2 + Math.random() * 3,
        25
    );


    createParticle(
        player.x + 12,
        player.y + 35,
        "#35d9ff",
        2 + Math.random() * 3,
        25
    );

}


function createExplosion(
    x,
    y
) {

    for (
        let i = 0;
        i < 45;
        i++
    ) {

        const colors = [
            "#ff4757",
            "#ffb142",
            "#fff",
            "#35d9ff"
        ];


        createParticle(
            x,
            y,
            colors[
                Math.floor(
                    Math.random() *
                    colors.length
                )
            ],
            2 + Math.random() * 6,
            250
        );

    }

}


function updateParticles(dt) {

    for (
        let i = particles.length - 1;
        i >= 0;
        i--
    ) {

        const p =
            particles[i];


        p.x +=
            p.vx * dt;

        p.y +=
            p.vy * dt;

        p.life -=
            dt * 1.8;

        p.vx *=
            .98;

        p.vy *=
            .98;


        if (p.life <= 0) {

            particles.splice(i, 1);

        }

    }

}


/* =========================================================
   ROAD
========================================================= */

function updateRoad(dt) {

    roadOffset +=
        player.speed *
        dt;


    const cycle =
        CONFIG.ROAD_DASH_LENGTH +
        CONFIG.ROAD_DASH_GAP;


    roadOffset %=
        cycle;

}


/* =========================================================
   UPDATE
========================================================= */

function update(dt) {

    updateParticles(dt);


    if (
        state !== STATE.PLAYING
    ) {
        return;
    }


    elapsed += dt;


    updateDifficulty();

    updatePlayer(dt);

    updateTraffic(dt);

    updateRoad(dt);

    updateScore(dt);

    checkCollision();

    createExhaust();

    shake *=
        Math.pow(.02, dt);

    updateHUD();

}


/* =========================================================
   BACKGROUND
========================================================= */

function drawBackground() {

    const gradient =
        ctx.createLinearGradient(
            0,
            0,
            0,
            height
        );


    gradient.addColorStop(
        0,
        "#05091b"
    );

    gradient.addColorStop(
        .5,
        "#081127"
    );

    gradient.addColorStop(
        1,
        "#04110b"
    );


    ctx.fillStyle =
        gradient;

    ctx.fillRect(
        0,
        0,
        width,
        height
    );


    /* City lights */

    for (
        let i = 0;
        i < 35;
        i++
    ) {

        const side =
            i % 2 === 0
                ? 0
                : 1;


        const x =
            side === 0
                ? Math.random() * roadLeft
                : roadRight +
                  Math.random() *
                  (width - roadRight);


        const y =
            (
                i * 83 +
                roadOffset * .12
            ) %
            height;


        ctx.fillStyle =
            i % 3 === 0
                ? "rgba(53,217,255,.18)"
                : "rgba(255,255,255,.10)";


        ctx.fillRect(
            x,
            y,
            2,
            2
        );

    }

}


/* =========================================================
   ROAD RENDERING
========================================================= */

function drawRoad() {

    /* Grass */

    ctx.fillStyle =
        "#071a14";

    ctx.fillRect(
        0,
        0,
        width,
        height
    );


    /* Road */

    const roadGradient =
        ctx.createLinearGradient(
            roadLeft,
            0,
            roadRight,
            0
        );


    roadGradient.addColorStop(
        0,
        "#161b28"
    );

    roadGradient.addColorStop(
        .5,
        "#222938"
    );

    roadGradient.addColorStop(
        1,
        "#161b28"
    );


    ctx.fillStyle =
        roadGradient;

    ctx.fillRect(
        roadLeft,
        0,
        roadWidth,
        height
    );


    /* Neon edges */

    ctx.shadowBlur = 14;
    ctx.shadowColor = "#35d9ff";

    ctx.fillStyle =
        "#35d9ff";

    ctx.fillRect(
        roadLeft,
        0,
        3,
        height
    );

    ctx.fillRect(
        roadRight - 3,
        0,
        3,
        height
    );

    ctx.shadowBlur = 0;


    /* Lane lines */

    const laneWidth =
        roadWidth /
        CONFIG.LANES;


    ctx.fillStyle =
        "rgba(255,255,255,.55)";


    for (
        let lane = 1;
        lane < CONFIG.LANES;
        lane++
    ) {

        const x =
            roadLeft +
            laneWidth * lane;


        for (
            let y =
                -CONFIG.ROAD_DASH_LENGTH +
                roadOffset %
                (
                    CONFIG.ROAD_DASH_LENGTH +
                    CONFIG.ROAD_DASH_GAP
                );

            y < height;

            y +=
                CONFIG.ROAD_DASH_LENGTH +
                CONFIG.ROAD_DASH_GAP
        ) {

            ctx.fillRect(
                x - 2,
                y,
                4,
                CONFIG.ROAD_DASH_LENGTH
            );

        }

    }


    /* Road reflections */

    const reflection =
        ctx.createLinearGradient(
            roadLeft,
            0,
            roadRight,
            0
        );

    reflection.addColorStop(
        0,
        "rgba(53,217,255,.04)"
    );

    reflection.addColorStop(
        .5,
        "rgba(255,255,255,.025)"
    );

    reflection.addColorStop(
        1,
        "rgba(53,217,255,.04)"
    );


    ctx.fillStyle =
        reflection;

    ctx.fillRect(
        roadLeft,
        0,
        roadWidth,
        height
    );

}


/* =========================================================
   CAR DRAWING
========================================================= */

function drawCar(
    car,
    playerCar = false
) {

    ctx.save();


    ctx.translate(
        car.x,
        car.y
    );


    if (
        playerCar
    ) {

        ctx.rotate(
            player.tilt
        );

    }


    const w =
        car.width;

    const h =
        car.height;


    /* Shadow */

    ctx.shadowBlur = 18;
    ctx.shadowColor =
        playerCar
            ? "rgba(53,217,255,.35)"
            : "rgba(0,0,0,.5)";


    ctx.fillStyle =
        "rgba(0,0,0,.5)";


    ctx.beginPath();

    ctx.roundRect(
        -w / 2 + 4,
        -h / 2 + 7,
        w,
        h,
        10
    );

    ctx.fill();

    ctx.shadowBlur = 0;


    /* Body gradient */

    const body =
        ctx.createLinearGradient(
            -w / 2,
            0,
            w / 2,
            0
        );


    body.addColorStop(
        0,
        "#111827"
    );

    body.addColorStop(
        .22,
        car.color
    );

    body.addColorStop(
        .75,
        car.color
    );

    body.addColorStop(
        1,
        "#111827"
    );


    ctx.fillStyle =
        body;


    ctx.beginPath();

    ctx.roundRect(
        -w / 2,
        -h / 2,
        w,
        h,
        10
    );

    ctx.fill();


    /* Roof */

    ctx.fillStyle =
        "rgba(5,10,20,.78)";


    ctx.beginPath();

    ctx.roundRect(
        -w * .32,
        -h * .22,
        w * .64,
        h * .42,
        7
    );

    ctx.fill();


    /* Window */

    ctx.fillStyle =
        "rgba(90,210,255,.45)";


    ctx.beginPath();

    ctx.roundRect(
        -w * .25,
        -h * .17,
        w * .5,
        h * .20,
        4
    );

    ctx.fill();


    /* Window divider */

    ctx.fillStyle =
        "rgba(255,255,255,.12)";

    ctx.fillRect(
        -1,
        -h * .17,
        2,
        h * .20
    );


    /* Wheels */

    ctx.fillStyle =
        "#050608";


    const wheelW = 7;
    const wheelH = 19;


    ctx.fillRect(
        -w / 2 - 2,
        -h * .28,
        wheelW,
        wheelH
    );

    ctx.fillRect(
        w / 2 - 5,
        -h * .28,
        wheelW,
        wheelH
    );

    ctx.fillRect(
        -w / 2 - 2,
        h * .08,
        wheelW,
        wheelH
    );

    ctx.fillRect(
        w / 2 - 5,
        h * .08,
        wheelW,
        wheelH
    );


    /* Lights */

    if (
        playerCar
    ) {

        ctx.shadowBlur = 12;
        ctx.shadowColor = "#fff";


        ctx.fillStyle =
            "#fff";


        ctx.fillRect(
            -w * .32,
            -h / 2 + 4,
            10,
            5
        );

        ctx.fillRect(
            w * .32 - 10,
            -h / 2 + 4,
            10,
            5
        );


        ctx.shadowBlur = 0;


        /* Tail lights */

        ctx.fillStyle =
            "#ff334f";

        ctx.fillRect(
            -w * .34,
            h / 2 - 8,
            9,
            5
        );

        ctx.fillRect(
            w * .34 - 9,
            h / 2 - 8,
            9,
            5
        );

    } else {

        ctx.fillStyle =
            "#ff334f";

        ctx.fillRect(
            -w * .34,
            h / 2 - 8,
            9,
            5
        );

        ctx.fillRect(
            w * .34 - 9,
            h / 2 - 8,
            9,
            5
        );

    }


    ctx.restore();

}


/* =========================================================
   PARTICLES
========================================================= */

function drawParticles() {

    for (const p of particles) {

        ctx.globalAlpha =
            Math.max(
                0,
                p.life
            );

        ctx.fillStyle =
            p.color;


        ctx.beginPath();

        ctx.arc(
            p.x,
            p.y,
            p.size,
            0,
            Math.PI * 2
        );

        ctx.fill();

    }


    ctx.globalAlpha = 1;

}


/* =========================================================
   SPEED LINES
========================================================= */

function drawSpeedLines() {

    if (
        player.speed < 650
    ) {
        return;
    }


    const intensity =
        (
            player.speed - 650
        ) / 300;


    ctx.strokeStyle =
        `rgba(53,217,255,${intensity * .16})`;

    ctx.lineWidth = 2;


    for (
        let i = 0;
        i < 14;
        i++
    ) {

        const x =
            Math.random() *
            width;


        const y =
            Math.random() *
            height;


        ctx.beginPath();

        ctx.moveTo(
            x,
            y
        );

        ctx.lineTo(
            x,
            y + 20 + intensity * 50
        );

        ctx.stroke();

    }

}


/* =========================================================
   VIGNETTE
========================================================= */

function drawVignette() {

    const gradient =
        ctx.createRadialGradient(
            width / 2,
            height / 2,
            height * .2,
            width / 2,
            height / 2,
            height * .75
        );


    gradient.addColorStop(
        0,
        "rgba(0,0,0,0)"
    );

    gradient.addColorStop(
        1,
        "rgba(0,0,0,.55)"
    );


    ctx.fillStyle =
        gradient;

    ctx.fillRect(
        0,
        0,
        width,
        height
    );

}


/* =========================================================
   RENDER
========================================================= */

function render() {

    ctx.save();


    if (
        shake > .5
    ) {

        ctx.translate(
            (Math.random() - .5) *
            shake,

            (Math.random() - .5) *
            shake
        );

    }


    drawBackground();

    drawRoad();


    for (const car of traffic) {

        drawCar(
            car,
            false
        );

    }


    drawParticles();

    drawCar(
        player,
        true
    );


    drawSpeedLines();

    drawVignette();


    ctx.restore();

}


/* =========================================================
   HUD
========================================================= */

function updateHUD() {

    scoreElement.textContent =
        Math.floor(score).toLocaleString();


    distanceElement.textContent =
        Math.floor(distance) +
        " m";


    speedElement.textContent =
        Math.floor(
            player.speed * .28
        ) +
        " km/h";

}


/* =========================================================
   PAUSE
========================================================= */

function pauseGame() {

    if (
        state !== STATE.PLAYING
    ) {
        return;
    }


    state =
        STATE.PAUSED;

    clearInput();

    pauseOverlay.classList.remove(
        "hidden"
    );

}


function resumeGame() {

    if (
        state !== STATE.PAUSED
    ) {
        return;
    }


    state =
        STATE.PLAYING;

    pauseOverlay.classList.add(
        "hidden"
    );

}


/* =========================================================
   GAME LOOP
========================================================= */

function loop(timestamp) {

    if (!lastTime) {
        lastTime = timestamp;
    }


    let dt =
        (timestamp - lastTime) /
        1000;


    lastTime =
        timestamp;


    dt =
        Math.min(
            dt,
            CONFIG.MAX_DT
        );


    update(dt);

    render();


    requestAnimationFrame(
        loop
    );

}


/* =========================================================
   BUTTONS
========================================================= */

document
    .getElementById("startButton")
    .addEventListener(
        "click",
        startGame
    );


document
    .getElementById("restartButton")
    .addEventListener(
        "click",
        startGame
    );


document
    .getElementById("pauseButton")
    .addEventListener(
        "click",
        pauseGame
    );


document
    .getElementById("resumeButton")
    .addEventListener(
        "click",
        resumeGame
    );


/* =========================================================
   INITIALIZATION
========================================================= */

function initialize() {

    resize();

    setupKeyboard();

    setupTouch();

    player.x =
        width / 2;

    player.y =
        height -
        player.height -
        65;


    const savedBest =
        Number(
            localStorage.getItem(
                "neonRushBest"
            ) || 0
        );

    bestScore.textContent =
        savedBest;


    updateHUD();


    window.addEventListener(
        "resize",
        resize
    );


    requestAnimationFrame(
        loop
    );

}


initialize();


/* =========================================================
   SIMPLE TESTS
========================================================= */

function runTests() {

    const results = [];


    function test(
        name,
        condition
    ) {

        results.push({
            test: name,
            passed: Boolean(condition)
        });

    }


    test(
        "Collision detects overlap",

        collision(
            {
                x: 100,
                y: 100,
                width: 50,
                height: 80
            },

            {
                x: 105,
                y: 110,
                width: 50,
                height: 80
            }
        )
    );


    test(
        "Collision rejects separated objects",

        !collision(
            {
                x: 100,
                y: 100,
                width: 50,
                height: 80
            },

            {
                x: 400,
                y: 400,
                width: 50,
                height: 80
            }
        )
    );


    test(
        "Road has four lanes",

        CONFIG.LANES === 4
    );


    test(
        "Player starts inside road",

        player.x >= roadLeft &&
        player.x <= roadRight
    );


    test(
        "Difficulty increases",

        1 +
        Math.floor(
            20 /
            CONFIG.DIFFICULTY_TIME
        ) > 1
    );


    console.table(results);


    const passed =
        results.filter(
            item => item.passed
        ).length;


    console.log(
        `${passed}/${results.length} tests passed.`
    );


    return results;

}
```