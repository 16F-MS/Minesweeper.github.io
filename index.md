---
title: Welcome to my blog!
---
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>マインスイーパー (無限モード)</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap');

        body {
            font-family: 'Inter', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background-color: #e0e0e0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
            overflow: hidden; /* Prevent body scroll for modal screens */
        }

        /* Common styles for all screens */
        .screen {
            background-color: #c0c0c0;
            border: 6px solid;
            border-color: #ffffff #808080 #808080 #ffffff;
            padding: 20px 32px; /* Reduced from 25px 40px */
            border-radius: 8px;
            box-shadow: 5px 5% 15px rgba(0, 0, 0, 0.3);
            display: none; /* Hidden by default */
            flex-direction: column;
            align-items: center;
            gap: 16px; /* Reduced from 20px */
            font-size: 1.2em; /* Reduced from 1.5em */
            font-weight: bold;
            text-align: center;
            max-width: 90%; /* Default responsive max-width */
            position: absolute; /* Position screens centrally */
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 1000; /* Ensure screens are on top */
        }

        .screen.active {
            display: flex;
        }

        /* Title Screen Specific */
        #title-screen {
            max-width: 400px; /* Increased max-width for the title screen box to accommodate wider buttons */
        }

        #title-screen h1 {
            font-size: 2em; /* Reduced from 2.5em */
            color: #333;
            margin-bottom: 24px; /* Reduced from 30px */
            text-shadow: 2px 2px 5px rgba(0,0,0,0.2);
        }

        .title-button {
            background-color: #c0c0c0;
            border: 4px solid;
            border-color: #ffffff #808080 #808080 #ffffff;
            padding: 12px 24px; /* Reduced from 15px 30px */
            font-size: 1.2em; /* Reduced from 1.5em */
            cursor: pointer;
            border-radius: 8px;
            transition: background-color 0.2s;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 280px; /* Increased max-width for buttons to prevent text wrapping */
            text-align: center;
        }

        .title-button:active {
            border-color: #808080 #ffffff #ffffff #808080;
            background-color: #a0a0a0;
            box-shadow: inset 1px 1px 3px rgba(0, 0, 0, 0.2);
        }

        /* Difficulty Selection Screen Specific */
        #difficulty-selection-screen h2 {
            font-size: 1.6em; /* Reduced from 2em */
            color: #333;
            margin-bottom: 16px; /* Reduced from 20px */
        }

        .difficulty-button {
            background-color: #c0c0c0;
            border: 4px solid;
            border-color: #ffffff #808080 #808080 #ffffff;
            padding: 10px 20px; /* Reduced from 12px 25px */
            font-size: 1em; /* Reduced from 1.2em */
            cursor: pointer;
            border-radius: 6px;
            transition: background-color 0.2s;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 200px; /* Reduced from 250px */
        }

        .difficulty-button:active {
            border-color: #808080 #ffffff #ffffff #808080;
            background-color: #a0a0a0;
            box-shadow: inset 1px 1px 3px rgba(0, 0, 0, 0.2);
        }

        /* Ranking Screen Specific (formerly result-screen) */
        #ranking-screen h2 {
            margin-top: 0;
            margin-bottom: 12px; /* Reduced from 15px */
            font-size: 1.4em; /* Reduced from 1.8em */
            color: #333;
        }

        #ranking-screen p {
            margin: 4px 0; /* Reduced from 5px */
            font-size: 1em; /* Reduced from 1.2em */
        }

        #ranking-screen h3 {
            margin-top: 16px; /* Reduced from 20px */
            margin-bottom: 8px; /* Reduced from 10px */
            font-size: 1.2em; /* Reduced from 1.5em */
            color: #333;
            border-bottom: 2px solid #808080;
            padding-bottom: 4px; /* Reduced from 5px */
            width: 100%;
        }

        .ranking-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.8em; /* Reduced from 1em */
            margin-bottom: 16px; /* Reduced from 20px */
        }

        .ranking-table th, .ranking-table td {
            border: 1px solid #a0a0a0;
            padding: 6px; /* Reduced from 8px */
            text-align: center;
        }

        .ranking-table th {
            background-color: #b0b0b0;
            color: #333;
        }

        .ranking-table tr:nth-child(even) {
            background-color: #e0e0e0;
        }

        .ranking-table tr.highlight-score {
            background-color: #ffeb3b; /* Highlight color */
            font-weight: bold;
            color: #000;
        }

        .ranking-difficulty-controls {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
            flex-wrap: wrap; /* Allow wrapping on smaller screens */
            justify-content: center;
        }

        .ranking-difficulty-button {
            background-color: #c0c0c0;
            border: 4px solid;
            border-color: #ffffff #808080 #808080 #ffffff;
            padding: 8px 15px;
            font-size: 0.9em; /* Adjusted for consistency */
            cursor: pointer;
            border-radius: 6px;
            transition: background-color 0.2s;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
        }

        .ranking-difficulty-button:active {
            border-color: #808080 #ffffff #ffffff #808080;
            background-color: #a0a0a0;
            box-shadow: inset 1px 1px 3px rgba(0, 0, 0, 0.2);
        }

        .ranking-difficulty-button.active {
            background-color: #a0a0a0;
            border-color: #808080 #ffffff #ffffff #808080;
        }

        /* Game Container (main game area) */
        .game-container {
            background-color: #c0c0c0;
            border: 6px solid;
            border-color: #ffffff #808080 #808080 #ffffff;
            padding: 4px; /* Reduced from 8px */
            border-radius: 8px;
            box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.3);
            flex-direction: column;
            align-items: center;
            gap: 6px; /* Reduced from 12px */
            z-index: 1; /* Below modal screens */
        }

        .game-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 100%;
            background-color: #c0c0c0;
            border: 3px solid;
            border-color: #808080 #ffffff #ffffff #808080;
            padding: 4px 8px; /* Reduced from 5px 10px */
            box-sizing: border-box;
            border-radius: 4px;
        }

        .counter-group {
            display: flex;
            flex-direction: column; /* Stack counter and label vertically */
            align-items: center;
            gap: 4px; /* Reduced from 5px */
        }

        .counter-label {
            font-size: 0.7em; /* Reduced from 0.8em */
            color: #333;
            text-align: center;
        }

        .counter, .timer, .life-display, .mine-damage-display {
            background-color: #000;
            color: #ff0000;
            font-family: 'Digital-7 Mono', monospace;
            font-size: 1.6em; /* Reduced from 2em */
            padding: 2px 6px; /* Reduced from 2px 8px */
            border-radius: 4px;
            border: 2px inset #ffffff;
            min-width: 48px; /* Reduced from 60px */
            text-align: center;
        }

        @font-face {
            font-family: 'Digital-7 Mono';
            src: url('https://raw.githubusercontent.com/seven-segment/digital-7/master/digital-7%20(mono).ttf') format('truetype');
        }

        .reset-button {
            background-color: #c0c0c0;
            border: 4px solid;
            border-color: #ffffff #808080 #808080 #ffffff;
            font-size: 2em; /* Reduced from 2.5em */
            cursor: pointer;
            width: 40px; /* Reduced from 50px */
            height: 40px; /* Reduced from 50px */
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            transition: background-color 0.2s;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
        }

        .reset-button:active {
            border-color: #808080 #ffffff #ffffff #808080;
            background-color: #a0a0a0;
            box-shadow: inset 1px 1px 3px rgba(0, 0, 0, 0.2);
        }

        .game-board {
            display: grid;
            border: 3px solid;
            border-color: #808080 #ffffff #ffffff #808080;
            background-color: #c0c0c0;
            border-radius: 4px;
            overflow: hidden;
            transition: transform 0.5s ease-out;
        }

        .game-board.shifting-down {
            transform: translateY(24px); /* Reduced from 30px */
        }

        @keyframes shake {
            0% { transform: translateX(0); }
            25% { transform: translateX(-4px); } /* Reduced from 5px */
            50% { transform: translateX(4px); } /* Reduced from 5px */
            75% { transform: translateX(-4px); } /* Reduced from 5px */
            100% { transform: translateX(0); }
        }

        .game-board.shake {
            animation: shake 0.5s ease-out;
        }

        .cell {
            width: 24px; /* Reduced from 30px */
            height: 24px; /* Reduced from 30px */
            background-color: #c0c0c0;
            border: 3px solid;
            border-color: #ffffff #808080 #808080 #ffffff;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1em; /* Reduced from 1.2em */
            cursor: pointer;
            user-select: none;
            transition: background-color 0.1s;
        }

        .cell.revealed {
            background-color: #d0d0d0;
            border-color: #a0a0a0;
            border-style: inset;
            cursor: default;
        }

        .cell.revealed.mine {
            background-color: #ff4d4d;
            color: #000;
        }

        .cell.flagged {
            color: #008000;
            font-size: 1.2em; /* Reduced from 1.5em */
        }

        .cell.revealed[data-mines="1"] { color: #0000ff; }
        .cell.revealed[data-mines="2"] { color: #008000; }
        .cell.revealed[data-mines="3"] { color: #ff0000; }
        .cell.revealed[data-mines="4"] { color: #000080; }
        .cell.revealed[data-mines="5"] { color: #800000; }
        .cell.revealed[data-mines="6"] { color: #008080; }
        .cell.revealed[data-mines="7"] { color: #000000; }
        .cell.revealed[data-mines="8"] { color: #808080; }

        .cell.darkened {
            background-color: #909090;
            border-color: #a0a0a0 #606060 #606060 #a0a0a0;
            cursor: default;
            pointer-events: none;
            opacity: 0.7;
        }

        .cell.darkened.revealed {
            background-color: #a0a0a0;
            border-color: #707070;
        }

        .cell.misflagged-red-bg {
            background-color: #ff8080;
        }
        .cell.misflagged-red-bg.revealed {
            background-color: #ff8080;
        }

        /* Bonus Cell Styles */
        .cell.heart {
            background-color: #d0d0d0; /* Revealed background for heart/treasure */
            border-color: #a0a0a0;
            border-style: inset;
        }

        .cell.heart-effect {
            animation: pulse-heart 0.5s infinite alternate; /* Pulsing effect for heart */
            color: #ff0000; /* Red heart color */
            font-size: 1.6em;
        }

        @keyframes pulse-heart {
            from { transform: scale(1); opacity: 1; }
            to { transform: scale(1.2); opacity: 0.8; }
        }

        .cell.treasure-closed {
            background-color: #d0d0d0;
            border-color: #a0a0a0;
            border-style: inset;
        }

        .cell.treasure-open {
            background-color: #d0d0d0;
            border-color: #a0a0a0;
            border-style: inset;
            color: #daa520; /* Gold color for treasure */
            font-size: 1.6em;
            text-shadow: 0 0 8px #ffcc00, 0 0 15px #ffff00; /* Glow effect */
        }

        /* New animation class for treasure */
        .cell.treasure-bounce-animation {
            animation: bounce-in 0.3s ease-out;
        }

        @keyframes bounce-in {
            0% { transform: scale(0.5); opacity: 0; }
            50% { transform: scale(1.2); opacity: 1; }
            100% { transform: scale(1); }
        }

        /* Message Box */
        #message-box {
            padding: 20px 32px; /* Reduced from 25px 40px */
            font-size: 1.2em; /* Reduced from 1.5em */
        }

        #message-box button {
            padding: 8px 16px; /* Reduced from 10px 20px */
            font-size: 1em; /* Reduced from 1.2em */
        }

        /* Manual Screen Specific */
        #manual-screen {
            max-width: 600px;
            padding: 25px;
            gap: 20px;
        }

        #manual-screen h2 {
            font-size: 1.8em;
            color: #333;
            margin-bottom: 20px;
        }

        .manual-nav-buttons {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 8px;
            margin-bottom: 15px;
        }

        .manual-nav-button {
            background-color: #c0c0c0;
            border: 4px solid;
            border-color: #ffffff #808080 #808080 #ffffff;
            padding: 8px 15px;
            font-size: 0.9em;
            cursor: pointer;
            border-radius: 6px;
            transition: background-color 0.2s;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
        }

        .manual-nav-button:active {
            border-color: #808080 #ffffff #ffffff #808080;
            background-color: #a0a0a0;
            box-shadow: inset 1px 1px 3px rgba(0, 0, 0, 0.2);
        }

        .manual-nav-button.active {
            background-color: #a0a0a0;
            border-color: #808080 #ffffff #ffffff #808080;
        }

        .manual-content {
            background-color: #e0e0e0;
            border: 3px inset #808080;
            padding: 15px;
            text-align: left;
            font-size: 0.9em;
            line-height: 1.6;
            max-height: 300px; /* Limit height for scrollability */
            overflow-y: auto; /* Enable scrolling */
            width: 100%;
            box-sizing: border-box;
            color: #333;
        }

        .manual-content h3 {
            font-size: 1.1em;
            margin-top: 0;
            margin-bottom: 10px;
            color: #000;
        }

        .manual-content p {
            margin-bottom: 10px;
        }

        .manual-content ul {
            padding-left: 20px;
            margin-bottom: 10px;
        }

        .manual-content li {
            margin-bottom: 5px;
        }

        /* Pause Menu Specific */
        #pause-menu {
            max-width: 350px;
            padding: 30px;
            gap: 20px;
        }

        #pause-menu h2 {
            font-size: 1.8em;
            color: #333;
            margin-bottom: 20px;
        }

        /* Options Screen Specific */
        #options-screen {
            max-width: 400px;
            padding: 25px;
            gap: 20px;
        }

        #options-screen h2 {
            font-size: 1.8em;
            color: #333;
            margin-bottom: 20px;
        }

        .option-group {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            width: 100%;
            margin-bottom: 15px;
        }

        .option-group label {
            font-size: 1.1em;
            color: #333;
            margin-bottom: 5px;
        }

        .option-group input[type="range"] {
            width: 80%;
            -webkit-appearance: none;
            height: 8px;
            background: #d3d3d3;
            outline: none;
            opacity: 0.7;
            transition: opacity .2s;
            border-radius: 4px;
        }

        .option-group input[type="range"]:hover {
            opacity: 1;
        }

        .option-group input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            background: #4CAF50;
            cursor: pointer;
            border-radius: 50%;
            border: 2px solid #fff;
            box-shadow: 0 0 5px rgba(0,0,0,0.2);
        }

        .option-group input[type="range"]::-moz-range-thumb {
            width: 20px;
            height: 20px;
            background: #4CAF50;
            cursor: pointer;
            border-radius: 50%;
            border: 2px solid #fff;
            box-shadow: 0 0 5px rgba(0,0,0,0.2);
        }

        .option-group .option-button {
            background-color: #c0c0c0;
            border: 4px solid;
            border-color: #ffffff #808080 #808080 #ffffff;
            padding: 8px 15px;
            font-size: 1em;
            cursor: pointer;
            border-radius: 6px;
            transition: background-color 0.2s;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
            margin: 0 5px;
        }

        .option-group .option-button:active {
            border-color: #808080 #ffffff #ffffff #808080;
            background-color: #a0a0a0;
            box-shadow: inset 1px 1px 3px rgba(0, 0, 0, 0.2);
        }

        .option-group .option-button.active {
            background-color: #a0a0a0;
            border-color: #808080 #ffffff #ffffff #808080;
        }

        /* Responsive adjustments */
        @media (max-width: 600px) {
            .game-container {
                padding: 3px; /* Reduced from 6px */
                gap: 4px; /* Reduced from 8px */
            }
            .game-header {
                padding: 2px 6px; /* Reduced from 3px 8px */
            }
            .counter-group {
                gap: 4px; /* Reduced from 5px */
            }
            .counter, .timer, .life-display, .mine-damage-display {
                font-size: 1.2em; /* Reduced from 1.5em */
                padding: 1px 4px; /* Reduced from 1px 5px */
                min-width: 36px; /* Reduced from 45px */
            }
            .reset-button {
                font-size: 1.6em; /* Reduced from 2em */
                width: 32px; /* Reduced from 40px */
                height: 32px; /* Reduced from 40px */
            }
            .cell {
                width: 20px; /* Reduced from 25px */
                height: 20px; /* Reduced from 25px */
                font-size: 0.8em; /* Reduced from 1em */
            }
            .title-button, .difficulty-button {
                padding: 8px 16px; /* Reduced from 10px 20px */
                font-size: 1em; /* Reduced from 1.2em */
            }
            .screen {
                padding: 16px 24px; /* Reduced from 20px 30px */
                font-size: 1em; /* Reduced from 1.2em */
            }
            .ranking-table {
                font-size: 0.7em; /* Reduced from 0.9em */
            }
            .ranking-table th, .ranking-table td {
                padding: 5px; /* Reduced from 6px */
            }
            #manual-screen {
                padding: 15px;
                gap: 10px;
            }
            .manual-nav-button {
                padding: 6px 12px;
                font-size: 0.8em;
            }
            .manual-content {
                font-size: 0.8em;
                padding: 10px;
            }
            .manual-content h3 {
                font-size: 1em;
            }
            #options-screen {
                padding: 15px;
                gap: 10px;
            }
            .option-group label {
                font-size: 1em;
            }
            .option-group .option-button {
                font-size: 0.9em;
                padding: 6px 12px;
            }
        }
    </style>
</head>
<body>
    <div id="title-screen" class="screen">
        <h1 data-key="title_main"></h1>
        <button id="start-game-button" class="title-button" data-key="button_startGame"></button>
        <button id="show-ranking-button" class="title-button" data-key="button_rankingBoard"></button>
        <button id="show-manual-button" class="title-button" data-key="button_manual"></button>
        <button id="show-options-button" class="title-button" data-key="button_options"></button> </div>

    <div id="resume-options-screen" class="screen">
        <h2 data-key="resume_title"></h2>
        <button id="resume-data-button" class="title-button" data-key="resume_data"></button>
        <button id="start-new-game-button" class="title-button" data-key="start_new_game"></button>
    </div>

    <div id="difficulty-selection-screen" class="screen">
        <h2 data-key="difficulty_title"></h2>
        <button id="easy-button" class="difficulty-button" data-difficulty="easy" data-key="difficulty_easy"></button>
        <button id="medium-button" class="difficulty-button" data-difficulty="medium" data-key="difficulty_medium"></button>
        <button id="hard-button" class="difficulty-button" data-difficulty="hard" data-key="difficulty_hard"></button>
        <button id="super-hard-button" class="difficulty-button" data-difficulty="superHard" data-key="difficulty_superHard"></button>
        <button id="hardcore-button" class="difficulty-button" data-difficulty="hardcore" data-key="difficulty_hardcore"></button>
        <button id="back-to-title-from-difficulty" class="difficulty-button" data-key="button_back"></button>
    </div>

    <div id="game-container" class="game-container screen">
        <div class="game-header">
            <div class="counter-group">
                <div id="life-display" class="life-display">50</div>
                <div class="counter-label" data-key="game_life"></div>
            </div>
            <div class="counter-group">
                <div id="mine-damage-display" class="mine-damage-display">20</div>
                <div class="counter-label" data-key="game_mineDamage"></div>
            </div>
            <button id="reset-button" class="reset-button">😊</button>
            <div class="counter-group">
                <div id="flags-placed-counter" class="counter">000</div>
                <div class="counter-label" data-key="game_flagsPlaced"></div>
            </div>
            <div class="counter-group">
                <div id="timer" class="timer">000</div>
                <div class="counter-label" data-key="game_timeElapsed"></div>
            </div>
        </div>
        <div id="game-board" class="game-board">
            </div>
    </div>

    <div id="ranking-screen" class="screen">
        <h2 id="ranking-screen-title" data-key="ranking_title"></h2>
        <p id="result-flags-display" style="display:none;"><span data-key="ranking_flags"></span> <span id="result-flags">0</span></p>
        <p id="result-time-display" style="display:none;"><span data-key="ranking_time"></span> <span id="result-time">0</span>秒</p>

        <div class="ranking-difficulty-controls">
            <button id="ranking-easy-button" class="ranking-difficulty-button" data-difficulty="easy" data-key="difficulty_easy"></button>
            <button id="ranking-medium-button" class="ranking-difficulty-button" data-difficulty="medium" data-key="difficulty_medium"></button>
            <button id="ranking-hard-button" class="ranking-difficulty-button" data-difficulty="hard" data-key="difficulty_hard"></button>
            <button id="ranking-super-hard-button" class="ranking-difficulty-button" data-difficulty="superHard" data-key="difficulty_superHard"></button>
            <button id="ranking-hardcore-button" class="ranking-difficulty-button" data-difficulty="hardcore" data-key="difficulty_hardcore"></button>
        </div>
        <p id="current-ranking-difficulty-display" style="font-size: 0.9em; margin-top: -10px; margin-bottom: 15px;"></p>

        <table class="ranking-table">
            <thead>
                <tr>
                    <th data-key="ranking_position"></th>
                    <th data-key="ranking_flagsCount"></th>
                    <th data-key="ranking_timeSec"></th>
                    <th data-key="ranking_date"></th>
                </tr>
            </thead>
            <tbody id="ranking-list">
                </tbody>
        </table>
        <button id="back-to-title-button" class="title-button" data-key="button_back"></button>
    </div>

    <div id="manual-screen" class="screen">
        <h2 data-key="manual_title"></h2>
        <div class="manual-nav-buttons">
            <button class="manual-nav-button active" data-chapter="rules" data-key="manual_rules"></button>
            <button class="manual-nav-button" data-chapter="scroll" data-key="manual_scroll"></button>
            <button class="manual-nav-button" data-chapter="controls" data-key="manual_controls"></button>
            <button class="manual-nav-button" data-chapter="life" data-key="manual_life"></button>
            <button class="manual-nav-button" data-chapter="items" data-key="manual_items"></button>
            <button class="manual-nav-button" data-chapter="difficulty" data-key="manual_difficulty"></button>
        </div>
        <div id="manual-content" class="manual-content">
            </div>
        <button id="close-manual-button" class="title-button" data-key="button_close"></button>
    </div>

    <div id="pause-menu" class="screen">
        <h2 data-key="pause_title"></h2>
        <button id="resume-game-button" class="title-button" data-key="pause_resume"></button>
        <button id="save-game-button" class="title-button" data-key="pause_save"></button>
    </div>

    <div id="options-screen" class="screen"> <h2 data-key="options_title"></h2>
        <div class="option-group">
            <label for="volume-slider" data-key="options_volume"></label>
            <input type="range" id="volume-slider" min="0" max="100" step="10" value="100">
            <span id="volume-percentage">100%</span>
        </div>
        <div class="option-group">
            <label data-key="options_language"></label>
            <button id="lang-japanese" class="option-button" data-lang="ja" data-key="options_lang_ja"></button>
            <button id="lang-english" class="option-button" data-lang="en" data-key="options_lang_en"></button>
        </div>
        <button id="back-to-title-from-options" class="title-button" data-key="button_back"></button>
    </div>

    <div id="message-box" class="screen">
        <p id="message-text"></p>
        <button id="message-ok-button" data-key="message_ok"></button>
    </div>

    <script>
        // Screen Elements
        const titleScreen = document.getElementById('title-screen');
        const startGameButton = document.getElementById('start-game-button');
        const showRankingButton = document.getElementById('show-ranking-button');
        const showManualButton = document.getElementById('show-manual-button');
        const showOptionsButton = document.getElementById('show-options-button'); // New

        const resumeOptionsScreen = document.getElementById('resume-options-screen');
        const resumeDataButton = document.getElementById('resume-data-button');
        const startNewGameButton = document.getElementById('start-new-game-button');

        const difficultySelectionScreen = document.getElementById('difficulty-selection-screen');
        const easyButton = document.getElementById('easy-button');
        const mediumButton = document.getElementById('medium-button');
        const hardButton = document.getElementById('hard-button');
        const superHardButton = document.getElementById('super-hard-button');
        const hardcoreButton = document.getElementById('hardcore-button');
        const backToTitleFromDifficultyButton = document.getElementById('back-to-title-from-difficulty');

        const gameContainer = document.getElementById('game-container');
        const gameBoard = document.getElementById('game-board');
        const flagsPlacedDisplay = document.getElementById('flags-placed-counter');
        const timerDisplay = document.getElementById('timer');
        const lifeDisplay = document.getElementById('life-display');
        const mineDamageDisplay = document.getElementById('mine-damage-display');
        const resetButton = document.getElementById('reset-button');

        const rankingScreen = document.getElementById('ranking-screen');
        const rankingScreenTitle = document.getElementById('ranking-screen-title');
        const resultFlagsDisplayContainer = document.getElementById('result-flags-display');
        const resultTimeDisplayContainer = document.getElementById('result-time-display');
        const resultFlags = document.getElementById('result-flags');
        const resultTime = document.getElementById('result-time');
        const rankingList = document.getElementById('ranking-list');
        const backToTitleButton = document.getElementById('back-to-title-button');
        const rankingEasyButton = document.getElementById('ranking-easy-button');
        const rankingMediumButton = document.getElementById('ranking-medium-button');
        const rankingHardButton = document.getElementById('ranking-hard-button');
        const rankingSuperHardButton = document = document.getElementById('ranking-super-hard-button');
        const rankingHardcoreButton = document.getElementById('ranking-hardcore-button');
        const currentRankingDifficultyDisplay = document.getElementById('current-ranking-difficulty-display');

        const manualScreen = document.getElementById('manual-screen');
        const manualContent = document.getElementById('manual-content');
        const closeManualButton = document.getElementById('close-manual-button');

        const pauseMenu = document.getElementById('pause-menu');
        const resumeGameButton = document.getElementById('resume-game-button');
        const saveGameButton = document.getElementById('save-game-button');

        const optionsScreen = document.getElementById('options-screen'); // New
        const volumeSlider = document.getElementById('volume-slider'); // New
        const volumePercentage = document.getElementById('volume-percentage'); // New
        const langJapaneseButton = document.getElementById('lang-japanese'); // New
        const langEnglishButton = document.getElementById('lang-english'); // New
        const backToTitleFromOptionsButton = document.getElementById('back-to-title-from-options'); // New

        const messageBox = document.getElementById('message-box');
        const messageText = document.getElementById('message-text');
        const messageOkButton = document.getElementById('message-ok-button');

        let board = [];
        let rows = 9;
        let cols = 9;
        let mines = 10;
        let revealedCellsCount = 0;
        let gameStarted = false;
        let gameOver = false;
        let timerInterval;
        let timeElapsed = 0;
        let totalFlagsPlacedInCurrentGame = 0;
        let isShifting = false;
        let isShaking = false;

        let currentLife = 50;
        let mineDamage = 20;
        let scrollCount = 0;

        let currentPlayedScore = null;
        let currentDifficulty = 'easy';
        let currentRankingDifficulty = 'easy';

        let currentVolume = parseInt(localStorage.getItem('minesweeperVolume')) || 100; // Load from localStorage or default to 100
        let currentLanguage = localStorage.getItem('minesweeperLanguage') || 'ja'; // Load from localStorage or default to Japanese

        // Difficulty settings
        const difficulties = {
            easy: { rows: 9, cols: 9, mines: 10, name_ja: '初級', name_en: 'Easy', initialLife: 50, damageIncreaseInterval: 15, heartProbability: 0.005, treasureProbability: 0 },
            medium: { rows: 16, cols: 16, mines: 40, name_ja: '中級', name_en: 'Medium', initialLife: 50, damageIncreaseInterval: 10, heartProbability: 0.005, treasureProbability: 0.001 },
            hard: { rows: 16, cols: 30, mines: 99, name_ja: '上級', name_en: 'Hard', initialLife: 50, damageIncreaseInterval: 5, heartProbability: 0.005, treasureProbability: 0.001 },
            superHard: { rows: 20, cols: 30, mines: 130, name_ja: '超上級', name_en: 'Super Hard', initialLife: 50, damageIncreaseInterval: 3, heartProbability: 0.005, treasureProbability: 0.002 },
            hardcore: { rows: 16, cols: 30, mines: 99, name_ja: 'ハードコア', name_en: 'Hardcore', initialLife: 1, damageIncreaseInterval: 10, heartProbability: 0, treasureProbability: 0 }
        };

        const translations = {
            ja: {
                title_main: '無限スクロール<br>マインスイーパー',
                button_startGame: 'ゲームスタート',
                button_rankingBoard: 'ランキングボード',
                button_manual: 'マニュアル',
                button_options: 'オプション',
                resume_title: 'ゲームの再開',
                resume_data: '中断データを再開',
                start_new_game: '最初からスタート',
                difficulty_title: '難易度を選択',
                difficulty_easy: '初級',
                difficulty_medium: '中級',
                difficulty_hard: '上級',
                difficulty_superHard: '超上級',
                difficulty_hardcore: 'ハードコア',
                button_back: 'タイトルに戻る',
                game_life: 'ライフ',
                game_mineDamage: '地雷のダメージ',
                game_flagsPlaced: '立てた旗の数',
                game_timeElapsed: '経過時間',
                ranking_title: 'ベストスコア',
                ranking_gameOver: 'ゲームオーバー！',
                ranking_flags: '立てた旗の数:',
                ranking_time: 'プレイ時間:',
                ranking_currentDifficulty: '現在の難易度:',
                ranking_position: '順位',
                ranking_flagsCount: '旗の数',
                ranking_timeSec: '時間 (秒)',
                ranking_date: '日付',
                ranking_noScore: 'まだスコアがありません。',
                manual_title: 'マニュアル',
                manual_rules: '基本ルール',
                manual_scroll: 'スクロール',
                manual_controls: '操作方法',
                manual_life: 'ライフ',
                manual_items: 'アイテム',
                manual_difficulty: '難易度',
                button_close: '閉じる',
                pause_title: 'ポーズ',
                pause_resume: 'ゲーム再開',
                pause_save: '中断する',
                message_gameSaved: 'ゲームを中断しました。',
                message_noSaveData: '保存されたデータがありません。',
                message_ok: 'OK',
                options_title: 'オプション',
                options_volume: '効果音量:',
                options_language: '言語:',
                options_lang_ja: '日本語',
                options_lang_en: 'English',
                manual_rules_content: `
                    <h3>地雷を探してフラグを立てよう！</h3>
                    <p>マインスイーパーは、隠された地雷のマスを推測し、それ以外の安全なマスを全て開くゲームです。</p>
                    <ul>
                        <li>マスを左クリックして開くと、そのマスが安全であれば数字が表示されます。</li>
                        <li>数字は、そのマスに隣接する（周囲8マス）地雷の数を示しています。</li>
                        <li>地雷のないマス（数字が0のマス）を開くと、周囲の地雷がないマスが自動的に開かれます。</li>
                        <li><strong>重要:</strong> 地雷のマスを開けてしまうか、地雷のないマスに旗を立ててしまうと、ライフが減少します。</li>
                    </ul>
                `,
                manual_scroll_content: `
                    <h3>無限に続く盤面！</h3>
                    <p>このゲームは、通常の地雷ゲームとは異なり、盤面が自動的にスクロールし、無限にプレイを続けることができます。</p>
                    <ul>
                        <li><strong>スクロール条件:</strong> 盤面の一番下の3行（最下行、下から2番目の行、下から3番目の行）のすべてのマスが、開かれるか、地雷として旗が立てられると、盤面がスクロールします。</li>
                        <li><strong>スクロール動作:</strong>
                            <ul>
                                <li>盤面の一番下の行が消滅し、一番上に新しい行が追加されます。</li>
                                <li>一定回数のスクロールごとに、地雷によるダメージが徐々に増加します。</li>
                            </ul>
                        </li>
                    </ul>
                    <p>無限に続く盤面で、どこまでハイスコアを伸ばせるか挑戦しましょう！</p>
                `,
                manual_controls_content: `
                    <h3>クリックと右クリック</h3>
                    <ul>
                        <li><strong>左クリック:</strong> マスを開きます。地雷でなければ数字が表示され、安全なマスが連鎖して開くことがあります。</li>
                        <li><strong>右クリック:</strong> マスに旗（🚩）を立てます。地雷があると思われるマスに旗を立てて、誤って開くのを防ぎます。一度立てた旗は右クリックしても解除されません。</li>
                        <li><strong>数字マスを左クリック（コード操作）:</strong> 開いている数字マスを左クリックし、その周囲に立てた旗の数が数字と一致する場合、旗が立っていない周囲のマスが自動的に開きます。</li>
                        <li><strong>数字マスを右クリック（コード操作）:</strong> 開いている数字マスを右クリックすると、その周囲の「まだ開かれていない」「旗が立っていない」マス全てに旗を立てます。</li>
                    </ul>
                `,
                manual_life_content: `
                    <h3>ライフゲージと地雷のダメージ</h3>
                    <p>このゲームにはライフの概念があります。初期ライフは50です。</p>
                    <ul>
                        <li><strong>地雷を踏む:</strong> ライフが20減少します。</li>
                        <li><strong>旗を誤った場所に立てる:</strong> ライフが20減少します。（ボーナスセルに旗を立てると、そのセルが開きライフが減少します。）</li>
                        <li><strong>ライフ回復:</strong> ハートのアイテム（❤️）を取得するとライフが5回復します。</li>
                        <li><strong>ライフ回復:</strong> 宝箱（💰）を取得すると、地雷ダメージと同じライフが回復します。</li>
                        <li><strong>ライフ回復:</strong> 盤面がスクロールするたびにライフが1回復します。</li>
                        <li><strong>ゲームオーバー:</strong> ライフが0になるとゲームオーバーです。</li>
                    </ul>
                    <p>ゲームが進行し、盤面がスクロールするたびに、地雷のダメージが徐々に増加します。</p>
                `,
                manual_items_content: `
                    <h3>ボーナスセル</h3>
                    <p>稀に特別なアイテムが隠されています。</p>
                    <ul>
                        <li><strong>ハート（❤️）:</strong> ライフを回復します。獲得するとライフが5回復します。</li>
                        <li><strong>宝箱（💰）:</strong> ライフを回復します。獲得すると地雷ダメージと同じライフが回復します。宝箱マスは必ず周囲に地雷があるマスに発生します。</li>
                    </ul>
                    <p>これらのボーナスセルは地雷ではありません。</p>
                `,
                manual_difficulty_content: `
                    <h3>5つの難易度モード</h3>
                    <p>ゲームには5つの難易度があります。</p>
                    <ul>
                        <li><strong>初級:</strong> 9x9マス、地雷10個。ダメージ増加は15行ごと。宝箱なし。初心者向け。</li>
                        <li><strong>中級:</strong> 16x16マス、地雷40個。ダメージ増加は10行ごと。標準的なマインスイーパーの難易度。</li>
                        <li><strong>上級:</strong> 16x30マス、地雷99個。ダメージ増加は5行ごと。上級者向け。</li>
                        <li><strong>超上級:</strong> 20x30マス、地雷130個。ダメージ増加は3行ごと。宝箱の出現率が上級の2倍。究極の挑戦。</li>
                        <li><strong>ハードコア:</strong> 16x30マス、地雷99個。初期ライフ1。ハートと宝箱は出現しません。ダメージ増加は10行ごと。真の挑戦者向け。</li>
                    </ul>
                    <p>どの難易度でも、無限にスクロールするため、長期的な戦略とライフ管理が重要になります。</p>
                `
            },
            en: {
                title_main: 'Infinite Scroll<br>Minesweeper',
                button_startGame: 'Start Game',
                button_rankingBoard: 'Ranking Board',
                button_manual: 'Manual',
                button_options: 'Options',
                resume_title: 'Resume Game',
                resume_data: 'Resume Saved Data',
                start_new_game: 'Start New Game',
                difficulty_title: 'Select Difficulty',
                difficulty_easy: 'Easy',
                difficulty_medium: 'Medium',
                difficulty_hard: 'Hard',
                difficulty_superHard: 'Super Hard',
                difficulty_hardcore: 'Hardcore',
                button_back: 'Back to Title',
                game_life: 'Life',
                game_mineDamage: 'Mine Damage',
                game_flagsPlaced: 'Flags Placed',
                game_timeElapsed: 'Time Elapsed',
                ranking_title: 'Best Scores',
                ranking_gameOver: 'Game Over!',
                ranking_flags: 'Flags Placed:',
                ranking_time: 'Play Time:',
                ranking_currentDifficulty: 'Current Difficulty:',
                ranking_position: 'Rank',
                ranking_flagsCount: 'Flags',
                ranking_timeSec: 'Time (s)',
                ranking_date: 'Date',
                ranking_noScore: 'No scores yet.',
                manual_title: 'Manual',
                manual_rules: 'Basic Rules',
                manual_scroll: 'Scrolling',
                manual_controls: 'Controls',
                manual_life: 'Life',
                manual_items: 'Items',
                manual_difficulty: 'Difficulty',
                button_close: 'Close',
                pause_title: 'Pause',
                pause_resume: 'Resume Game',
                pause_save: 'Save & Exit',
                message_gameSaved: 'Game saved.',
                message_noSaveData: 'No saved data found.',
                message_ok: 'OK',
                options_title: 'Options',
                options_volume: 'Sound Volume:',
                options_language: 'Language:',
                options_lang_ja: 'Japanese',
                options_lang_en: 'English',
                manual_rules_content: `
                    <h3>Find Mines and Place Flags!</h3>
                    <p>Minesweeper is a game where you deduce the location of hidden mines and open all other safe cells.</p>
                    <ul>
                        <li>Click a cell to open it. If it's safe, a number will appear.</li>
                        <li>The number indicates how many mines are adjacent to that cell (in the surrounding 8 cells).</li>
                        <li>If you open a cell with no mines (a 0-cell), surrounding safe cells will automatically open.</li>
                        <li><strong>Important:</strong> If you open a mine cell, or place a flag on a non-mine cell, you will lose life.</li>
                    </ul>
                `,
                manual_scroll_content: `
                    <h3>Infinitely Scrolling Board!</h3>
                    <p>Unlike traditional Minesweeper, this game features an automatically scrolling board, allowing for endless play.</p>
                    <ul>
                        <li><strong>Scrolling Condition:</strong> The board scrolls when all cells in the bottom three rows (bottom, second from bottom, and third from bottom) are either revealed or flagged as mines.</li>
                        <li><strong>Scrolling Action:</strong>
                            <ul>
                                <li>The bottommost row of the board disappears, and a new row is added to the top.</li>
                                <li>Mine damage gradually increases after a certain number of scrolls.</li>
                            </ul>
                        </li>
                    </ul>
                    <p>Challenge yourself to achieve a high score on this infinitely scrolling board!</p>
                `,
                manual_controls_content: `
                    <h3>Left Click and Right Click</h3>
                    <ul>
                        <li><strong>Left Click:</strong> Opens a cell. If it's not a mine, a number will appear, and safe cells may open in a chain reaction.</li>
                        <li><strong>Right Click:</strong> Places a flag (🚩) on a cell. Use this to mark cells you suspect contain mines, preventing accidental opening. Once a flag is placed, right-clicking again will not remove it.</li>
                        <li><strong>Left Click on Numbered Cell (Chord):</strong> If you left-click an opened numbered cell, and the number of flags placed around it matches the cell's number, all unflagged, unrevealed neighboring cells will automatically open.</li>
                        <li><strong>Right Click on Numbered Cell (Auto-Flag):</strong> If you right-click an opened numbered cell, all unrevealed, unflagged neighboring cells will automatically be flagged.</li>
                    </ul>
                `,
                manual_life_content: `
                    <h3>Life Gauge and Mine Damage</h3>
                    <p>This game includes a life system. Your initial life is 50.</p>
                    <ul>
                        <li><strong>Stepping on a Mine:</strong> Lose 20 life.</li>
                        <li><strong>Misflagging:</strong> Lose 20 life. (If you flag a bonus cell, it opens and you lose life.)</li>
                        <li><strong>Life Recovery:</strong> Gain 5 life by acquiring a Heart item (❤️).</li>
                        <li><strong>Life Recovery:</strong> Gain life equal to current mine damage by acquiring a Treasure Chest (💰).</li>
                        <li><strong>Life Recovery:</strong> Gain 1 life each time the board scrolls.</li>
                        <li><strong>Game Over:</strong> The game ends when your life reaches 0.</li>
                    </ul>
                    <p>As the game progresses and the board scrolls, mine damage will gradually increase.</p>
                `,
                manual_items_content: `
                    <h3>Bonus Cells</h3>
                    <p>Special items are occasionally hidden.</p>
                    <ul>
                        <li><strong>Heart (❤️):</strong> Recovers life. Gain 5 life when acquired.</li>
                        <li><strong>Treasure Chest (💰):</strong> Recovers life. Gain life equal to current mine damage when acquired. Treasure chest cells always appear next to cells with mines.</li>
                    </ul>
                    <p>These bonus cells are not mines.</p>
                `,
                manual_difficulty_content: `
                    <h3>5 Difficulty Modes</h3>
                    <p>The game has 5 difficulty levels:</p>
                    <ul>
                        <li><strong>Easy:</strong> 9x9 grid, 10 mines. Damage increases every 15 rows. No treasure chests. Recommended for beginners.</li>
                        <li><strong>Medium:</strong> 16x16 grid, 40 mines. Damage increases every 10 rows. Standard Minesweeper difficulty.</li>
                        <li><strong>Hard:</strong> 16x30 grid, 99 mines. Damage increases every 5 rows. For advanced players.</li>
                        <li><strong>Super Hard:</strong> 20x30 grid, 130 mines. Damage increases every 3 rows. Treasure chest appearance rate is double that of Hard. The ultimate challenge.</li>
                        <li><strong>Hardcore:</strong> 16x30 grid, 99 mines. Starting life of 1. No hearts or treasure chests appear. Damage increases every 10 rows. For true challengers.</li>
                    </ul>
                    <p>In all difficulties, the infinite scroll requires long-term strategy and life management.</p>
                `
            }
        };

        // --- Sound Effects ---
        // Synth for menu buttons
        const synth = new Tone.Synth().toDestination();

        // Click sound for cell interactions
        const clickSynth = new Tone.PluckSynth().toDestination();

        // Piano phrase for bonus discovery
        const pianoSynth = new Tone.PolySynth(Tone.Synth, {
            oscillator: { type: "triangle" },
            envelope: {
                attack: 0.02,
                decay: 0.1,
                sustain: 0.3,
                release: 0.5
            }
        }).toDestination();

        // Coin sound for treasure open
        const coinSynth = new Tone.MetalSynth({
            frequency: 200,
            envelope: {
                attack: 0.001,
                decay: 0.4,
                release: 0.1
            },
            harmonicity: 3.1,
            modulationIndex: 23,
            resonance: 4000,
            octaves: 1.5
        }).toDestination();

        // Explosion sound for mine hit
        const explosionSynth = new Tone.NoiseSynth({
            noise: {
                type: "white"
            },
            envelope: {
                attack: 0.005,
                decay: 0.2,
                sustain: 0.01,
                release: 0.1
            }
        }).toDestination();
        explosionSynth.chain(new Tone.Filter(800, "lowpass"), Tone.Destination);

        // Buzzer sound for misflag
        const buzzerSynth = new Tone.Synth({
            oscillator: { type: "square" },
            envelope: {
                attack: 0.01,
                decay: 0.1,
                sustain: 0.0,
                release: 0.2
            }
        }).toDestination();

        // New scroll sound - Volume increased (e.g., +6dB for roughly double perceived loudness)
        const scrollSynth = new Tone.MembraneSynth({
            pitchDecay: 0.05,
            octaves: 2,
            envelope: {
                attack: 0.001,
                decay: 0.4,
                sustain: 0.01,
                release: 0.2
            }
        }).toDestination();
        scrollSynth.volume.value = 6; // Set volume to 6dB (approx. double perceived loudness)

        // Keep track of the last time a click sound was scheduled
        let lastClickSoundTime = 0;
        // Minimum interval between click sounds to prevent "strictly greater" error
        // A 32n duration at 120 BPM is approx 0.015625 seconds. Use a slightly larger interval.
        const MIN_TIME_BETWEEN_CLICKS = 0.02; // seconds

        // New variable for menu sound scheduling
        let lastMenuSoundTime = 0;
        const MIN_TIME_BETWEEN_MENU_CLICKS = 0.1; // seconds, slightly longer for menu clicks

        /**
         * Plays a synth sound for menu button clicks.
         */
        function playMenuSound() {
            if (Tone.context.state !== 'running') Tone.start();
            const now = Tone.now();
            const scheduledTime = Math.max(now, lastMenuSoundTime + MIN_TIME_BETWEEN_MENU_CLICKS);
            synth.triggerAttackRelease("C4", "8n", scheduledTime);
            lastMenuSoundTime = scheduledTime;
        }

        /**
         * Plays a click sound for cell interactions.
         */
        function playClickSound() {
            if (Tone.context.state !== 'running') Tone.start();
            const now = Tone.now();
            // Ensure the new sound is scheduled strictly after the previous one's start time plus a minimum interval
            const scheduledTime = Math.max(now, lastClickSoundTime + MIN_TIME_BETWEEN_CLICKS);
            clickSynth.triggerAttackRelease("G5", "32n", scheduledTime);
            lastClickSoundTime = scheduledTime; // Update last scheduled time
        }

        /**
         * Plays a bright piano phrase for bonus discovery as an arpeggio.
         */
        function playPianoPhrase() {
            if (Tone.context.state !== 'running') Tone.start();
            const now = Tone.now();
            pianoSynth.triggerAttackRelease("C5", "8n", now);
            pianoSynth.triggerAttackRelease("E5", "8n", now + 0.1); // 0.1秒遅延 (以前の0.2秒から変更)
            pianoSynth.triggerAttackRelease("G5", "8n", now + 0.2); // 0.2秒遅延 (以前の0.4秒から変更)
        }

        /**
         * Plays a coin sound for treasure opening.
         */
        function playCoinSound() {
            if (Tone.context.state !== 'running') Tone.start();
            coinSynth.triggerAttackRelease("8n", Tone.now());
        }

        /**
         * Plays an explosion sound for mine hits.
         */
        function playExplosionSound() {
            if (Tone.context.state !== 'running') Tone.start();
            explosionSynth.triggerAttackRelease("0.5s", Tone.now());
        }

        /**
         * Plays a buzzer sound for misflag.
         */
        function playBuzzerSound() {
            if (Tone.context.state !== 'running') Tone.start();
            buzzerSynth.triggerAttackRelease("C3", "0.2s", Tone.now());
        }

        /**
         * Plays a sound for board scrolling.
         */
        function playScrollSound() {
            if (Tone.context.state !== 'running') Tone.start();
            scrollSynth.triggerAttackRelease("C2", "8n", Tone.now());
        }

        /**
         * Hides all main screens and shows the specified one.
         * @param {HTMLElement} screenToShow - The screen element to display.
         */
        function showScreen(screenToShow) {
            [titleScreen, difficultySelectionScreen, gameContainer, rankingScreen, manualScreen, pauseMenu, messageBox, resumeOptionsScreen, optionsScreen].forEach(screen => {
                screen.classList.remove('active');
            });
            if (screenToShow) {
                screenToShow.classList.add('active');
            }
        }

        /**
         * Loads manual chapter content and updates active button.
         * @param {string} chapterKey - The key of the chapter to load.
         */
        function loadManualChapter(chapterKey) {
            if (manualContent) manualContent.innerHTML = translations[currentLanguage][`manual_${chapterKey}_content`];
            document.querySelectorAll('.manual-nav-button').forEach(button => {
                if (button.dataset.chapter === chapterKey) {
                    button.classList.add('active');
                } else {
                    button.classList.remove('active');
                }
            });
        }

        /**
         * Updates all text content on the page based on the current language setting.
         */
        function updateLanguageDisplay() {
            // Update main title
            document.querySelector('[data-key="title_main"]').innerHTML = translations[currentLanguage].title_main;

            // Update buttons and labels with data-key attributes
            document.querySelectorAll('[data-key]').forEach(element => {
                const key = element.dataset.key;
                if (translations[currentLanguage][key] && key !== 'title_main') { // title_main is innerHTML, others are textContent
                    element.textContent = translations[currentLanguage][key];
                }
            });

            // Update difficulty names in ranking display
            if (currentRankingDifficultyDisplay) {
                currentRankingDifficultyDisplay.textContent = `${translations[currentLanguage].ranking_currentDifficulty} ${difficulties[currentRankingDifficulty][`name_${currentLanguage}`]}`;
            }

            // Update manual content for the currently active chapter
            const activeManualButton = document.querySelector('#manual-screen .manual-nav-button.active');
            if (activeManualButton) {
                loadManualChapter(activeManualButton.dataset.chapter);
            } else {
                // If no chapter is active, default to rules
                loadManualChapter('rules');
            }

            // Update volume percentage display
            if (volumePercentage) {
                volumePercentage.textContent = `${currentVolume}%`;
            }

            // Update language buttons active state
            if (langJapaneseButton) {
                if (currentLanguage === 'ja') {
                    langJapaneseButton.classList.add('active');
                    langEnglishButton.classList.remove('active');
                } else {
                    langJapaneseButton.classList.remove('active');
                    langEnglishButton.classList.add('active');
                }
            }
        }

        /**
         * Initializes the game board based on current difficulty settings.
         * @param {Object} [saveData=null] - Optional save data to load the game from.
         */
        function initializeGame(saveData = null) {
            // Stop and reset timer
            clearInterval(timerInterval);
            timeElapsed = 0;
            // Defensive checks for elements
            if (timerDisplay) timerDisplay.textContent = '000';
            gameStarted = false;
            gameOver = false;
            if (resetButton) resetButton.textContent = '😊';
            isShifting = false;
            isShaking = false;

            revealedCellsCount = 0;
            totalFlagsPlacedInCurrentGame = 0;
            if (flagsPlacedDisplay) flagsPlacedDisplay.textContent = '000';
            currentPlayedScore = null;

            if (saveData) {
                // Load game from save data
                board = saveData.board;
                rows = saveData.rows;
                cols = saveData.cols;
                mines = saveData.mines;
                currentLife = saveData.currentLife;
                mineDamage = saveData.mineDamage;
                scrollCount = saveData.scrollCount;
                timeElapsed = saveData.timeElapsed;
                totalFlagsPlacedInCurrentGame = saveData.totalFlagsPlacedInCurrentGame;
                currentDifficulty = saveData.currentDifficulty;
                gameStarted = true; // Saved game implies it was already started
                startTimer(); // Resume timer immediately
            } else {
                // Start a new game
                currentLife = difficulties[currentDifficulty].initialLife; // Use difficulty-specific initial life
                mineDamage = 20;
                scrollCount = 0;

                gameBoard.innerHTML = '';
                gameBoard.style.gridTemplateColumns = `repeat(${cols}, 1fr)`;

                board = [];
                for (let r = 0; r < rows; r++) {
                    const newRow = [];
                    for (let c = 0; c < cols; c++) {
                        newRow.push({
                            isMine: false,
                            isRevealed: false,
                            isFlagged: false,
                            isMisflagged: false,
                            minesAround: 0,
                            isHeartBonus: false,
                            isTreasureBonus: false,
                            isTreasureClosed: false
                        });
                    }
                    board.push(newRow);
                }

                // Phase 1: Place initial mines (excluding top and bottom rows)
                let minesPlaced = 0;
                const mineableRowsForInitialPlacement = rows - 2; // Rows from index 1 to rows-2
                while (minesPlaced < mines) {
                    const r = Math.floor(Math.random() * mineableRowsForInitialPlacement) + 1; // +1 to start from row 1
                    const c = Math.floor(Math.random() * cols);
                    if (!board[r][c].isMine) {
                        board[r][c].isMine = true;
                        minesPlaced++;
                    }
                }

                // Phase 2: Set up the initial bottom row (always safe and revealed)
                setupInitialBottomRow(); // This also calls calculateMinesAround internally after clearing bottom row

                // Phase 3: Place heart bonuses on cells that are not mines and have 0 mines around
                // This must happen *after* mines are placed and minesAround is calculated.
                // No treasure bonuses on initial board.
                for (let r = 1; r < rows - 1; r++) { // Iterate over rows that can have hearts
                    for (let c = 0; c < cols; c++) {
                        const cell = board[r][c];
                        // Use difficulty-specific heart probability
                        if (!cell.isMine && cell.minesAround === 0 && Math.random() < difficulties[currentDifficulty].heartProbability) {
                            cell.isHeartBonus = true;
                        }
                    }
                }
            }

            if (lifeDisplay) lifeDisplay.textContent = String(currentLife).padStart(2, '0');
            if (mineDamageDisplay) mineDamageDisplay.textContent = String(mineDamage).padStart(2, '0');
            if (timerDisplay) timerDisplay.textContent = String(timeElapsed).padStart(3, '0');
            if (flagsPlacedDisplay) flagsPlacedDisplay.textContent = String(totalFlagsPlacedInCurrentGame).padStart(3, '0');

            renderBoard();
            updateCounters();
            showScreen(gameContainer);
            autoRevealZeroCells(); // Call auto-reveal after initial setup or load
        }

        /**
         * Creates a new top row with only bonus cells (heart and treasure) based on probabilities.
         * No mines are placed in this row initially.
         * @returns {Array<Object>} The new row data.
         */
        function createNewRowWithBonusesOnly() {
            const newRowCells = [];
            for (let c = 0; c < cols; c++) {
                newRowCells.push({
                    isMine: false, // No mines initially in the very top row
                    isRevealed: false,
                    isFlagged: false,
                    isMisflagged: false,
                    minesAround: 0,
                    isHeartBonus: false,
                    isTreasureBonus: false,
                    isTreasureClosed: false
                });
            }

            // Place Treasure Bonus based on current difficulty's probability
            if (Math.random() < difficulties[currentDifficulty].treasureProbability * cols) { // Scale probability by number of columns
                 const c = Math.floor(Math.random() * cols);
                 const cell = newRowCells[c];
                 cell.isTreasureBonus = true;
                 cell.isTreasureClosed = true;
            }

            // Place Heart Bonus
            for (let c = 0; c < cols; c++) {
                const cell = newRowCells[c];
                // Only place heart on cells that are not already a treasure bonus
                if (!cell.isTreasureBonus && Math.random() < difficulties[currentDifficulty].heartProbability) { // Use difficulty-specific heart probability
                    cell.isHeartBonus = true;
                }
            }
            return newRowCells;
        }

        function setupInitialBottomRow() {
            const bottomRowIndex = rows - 1;
            for (let c = 0; c < cols; c++) {
                const cellData = board[bottomRowIndex][c];
                // Bottom row cannot be special bonus cells or mines.
                cellData.isMine = false;
                cellData.isHeartBonus = false;
                cellData.isTreasureBonus = false;
                cellData.isTreasureClosed = false;
                
                // Then proceed with standard bottom row setup
                cellData.isRevealed = true;
                revealedCellsCount++;
            }
            // Recalculate mines around after fixing the bottom row
            calculateMinesAround();
        }

        function renderBoard() {
            gameBoard.innerHTML = '';
            for (let r = 0; r < rows; r++) {
                for (let c = 0; c < cols; c++) {
                    const cell = document.createElement('div');
                    cell.classList.add('cell');
                    cell.dataset.row = r;
                    cell.dataset.col = c;

                    const cellData = board[r][c];

                    if (cellData.isRevealed) {
                        cell.classList.add('revealed');
                        if (cellData.isMine) {
                            cell.classList.add('mine');
                            cell.textContent = '💣';
                        } else if (cellData.isHeartBonus) {
                            cell.classList.add('heart');
                            cell.textContent = '❤️'; // Display heart when revealed
                        } else if (cellData.isTreasureBonus) {
                            if (cellData.isTreasureClosed) {
                                cell.classList.add('treasure-closed');
                                cell.textContent = '🎁';
                            } else {
                                // Already opened, render open treasure icon without re-animating
                                cell.classList.add('treasure-open');
                                cell.textContent = '💰';
                            }
                        }
                        else if (cellData.minesAround > 0) {
                            cell.textContent = cellData.minesAround;
                            cell.dataset.mines = cellData.minesAround;
                        }
                        if (cellData.isMisflagged) {
                            cell.classList.add('misflagged-red-bg');
                        }
                    } else if (cellData.isFlagged) {
                        cell.classList.add('flagged');
                        cell.textContent = '🚩';
                    } else {
                        // This is an unrevealed, unflagged cell. It should look like a standard unrevealed cell.
                    }

                    if (r === 0 || r === rows - 1) {
                        cell.classList.add('darkened');
                    } else {
                        cell.addEventListener('click', handleCellClick);
                        cell.addEventListener('contextmenu', handleCellRightClick);
                    }
                    gameBoard.appendChild(cell);
                }
            }
        }

        function calculateMinesAround() {
            for (let r = 0; r < rows; r++) {
                for (let c = 0; c < cols; c++) {
                    const cellData = board[r][c];
                    // Calculate minesAround for all non-mine cells.
                    // Heart bonus cells still don't show numbers, and are always safe (minesAround 0).
                    // Treasure bonus cells DO show numbers if they have mines around them.
                    if (!cellData.isMine && !cellData.isHeartBonus) {
                        let count = 0;
                        for (let dr = -1; dr <= 1; dr++) {
                            for (let dc = -1; dc <= 1; dc++) {
                                if (dr === 0 && dc === 0) continue;

                                const nr = r + dr;
                                const nc = c + dc;

                                if (nr >= 0 && nr < rows && nc >= 0 && nc < cols) {
                                    // Make sure to check if board[nr] and board[nr][nc] exist before accessing isMine
                                    if (board[nr] && board[nr][nc] && board[nr][nc].isMine) {
                                        count++;
                                    }
                                }
                            }
                        }
                        cellData.minesAround = count;
                    } else if (cellData.isMine || cellData.isHeartBonus) {
                        cellData.minesAround = 0; // Mines and Heart bonus cells don't show numbers
                    }
                }
            }
        }

        function getNeighbors(r, c) {
            const neighbors = [];
            for (let dr = -1; dr <= 1; dr++) {
                for (let dc = -1; dc <= 1; dc++) {
                    if (dr === 0 && dc === 0) continue;
                    const nr = r + dr;
                    const nc = c + dc;
                    if (nr >= 0 && nr < rows && nc >= 0 && nc < cols) {
                        neighbors.push({ r: nr, c: nc, data: board[nr][nc], element: gameBoard.children[nr * cols + nc] });
                    }
                }
            }
            return neighbors;
        }

        /**
         * Attempts to start the game timer if it hasn't started yet.
         */
        function tryStartTimer() {
            if (!gameStarted) {
                gameStarted = true;
                startTimer();
            }
        }

        function handleCellClick(event) {
            if (gameOver || isShifting || isShaking) return;

            tryStartTimer(); // Attempt to start timer on first click

            const cellElement = event.target;
            const r = parseInt(cellElement.dataset.row);
            const c = parseInt(cellElement.dataset.col);
            const cellData = board[r][c];

            if (cellElement.classList.contains('darkened') || cellData.isFlagged) {
                return;
            }

            if (cellData.isRevealed) {
                // Handle chord (revealing neighbors)
                if (cellData.minesAround > 0 && !cellData.isHeartBonus && !cellData.isTreasureBonus) {
                    const neighbors = getNeighbors(r, c);
                    // Count flagged cells AND revealed mine cells as "known" mines
                    const knownMinesCount = neighbors.filter(n => n.data.isFlagged || (n.data.isMine && n.data.isRevealed)).length;

                    if (knownMinesCount === cellData.minesAround) {
                        let anyNewCellRevealed = false;
                        let hitMineDuringChord = false;
                        for (const neighbor of neighbors) {
                            if (!neighbor.data.isRevealed && !neighbor.data.isFlagged && !neighbor.element.classList.contains('darkened')) {
                                if (revealCell(neighbor.r, neighbor.c, neighbor.element)) {
                                    // Mine hit during chord reveal
                                    currentLife -= mineDamage;
                                    if (lifeDisplay) lifeDisplay.textContent = String(currentLife).padStart(2, '0');
                                    isShaking = true;
                                    gameBoard.classList.add('shake');
                                    playExplosionSound();
                                    setTimeout(() => {
                                        gameBoard.classList.remove('shake');
                                        isShaking = false;
                                        if (currentLife <= 0) {
                                            revealAllMines();
                                            endGame(false);
                                        }
                                    }, 500);
                                    hitMineDuringChord = true;
                                    break; // Stop revealing if a mine is hit
                                }
                                anyNewCellRevealed = true;
                            }
                        }
                        if (!hitMineDuringChord && anyNewCellRevealed) {
                            playClickSound(); // Play sound once for successful chord reveal
                        }
                        if (hitMineDuringChord) return; // If mine hit, stop further processing
                        checkShiftCondition();
                    }
                }
                return; // Already revealed, no further action for single click
            }

            playClickSound(); // Play sound for the initial click

            if (cellData.isMine) {
                currentLife -= mineDamage;
                if (lifeDisplay) lifeDisplay.textContent = String(currentLife).padStart(2, '0');

                revealCell(r, c, cellElement); // This will mark it as revealed and display bomb

                isShaking = true;
                gameBoard.classList.add('shake');
                playExplosionSound();

                setTimeout(() => {
                    gameBoard.classList.remove('shake');
                    isShaking = false;
                    if (currentLife <= 0) {
                        revealAllMines();
                        endGame(false);
                    }
                }, 500);
            } else {
                revealCell(r, c, cellElement); // This will handle recursive reveals for 0-cells
                checkShiftCondition();
            }
        }

        function handleCellRightClick(event) {
            event.preventDefault();
            if (gameOver || isShifting || isShaking) return;

            tryStartTimer(); // Attempt to start timer on first flag

            const cellElement = event.target;
            const r = parseInt(cellElement.dataset.row);
            const c = parseInt(cellElement.dataset.col);
            const cellData = board[r][c];

            if (cellElement.classList.contains('darkened')) {
                return;
            }

            if (cellData.isRevealed) {
                // Chord-flagging (right-click on revealed number cell to flag neighbors)
                if (cellData.minesAround > 0) {
                    const neighbors = getNeighbors(r, c);
                    // Count flagged cells AND revealed mine cells as "known" mines
                    const knownMinesCount = neighbors.filter(n => n.data.isFlagged || (n.data.isMine && n.data.isRevealed)).length;
                    const unrevealedUnflagged = neighbors.filter(n => !n.data.isRevealed && !n.data.isFlagged);
                    const unrevealedUnflaggedCount = unrevealedUnflagged.length;

                    // Condition: The number on the cell equals (known mines + unrevealed, unflagged neighbors)
                    // This implies that all unrevealed, unflagged neighbors must be mines.
                    if (cellData.minesAround === knownMinesCount + unrevealedUnflaggedCount) {
                        let flagsPlacedThisAction = 0;
                        for (const neighbor of unrevealedUnflagged) {
                            // Ensure we don't flag darkened cells (top/bottom rows)
                            if (!neighbor.element.classList.contains('darkened')) {
                                neighbor.data.isFlagged = true;
                                neighbor.element.classList.add('flagged');
                                neighbor.element.textContent = '🚩';
                                totalFlagsPlacedInCurrentGame++;
                                flagsPlacedThisAction++;
                            }
                        }
                        if (flagsPlacedThisAction > 0) {
                            playClickSound(); // Play sound once for this mass flagging operation
                            updateCounters();
                        }
                    }
                }
                return;
            }

            // Normal flagging/misflagging of an unrevealed cell
            if (cellData.isFlagged) {
                return; // 旗が立っているマスを右クリックしても旗が解除されず、そのままの状態を維持
            } else {
                // This is a new flag attempt or misflag
                playClickSound(); // Play sound once for this flagging/misflagging action

                if (cellData.isHeartBonus || cellData.isTreasureBonus) {
                    currentLife -= mineDamage;
                    if (lifeDisplay) lifeDisplay.textContent = String(currentLife).padStart(2, '0');

                    isShaking = true;
                    gameBoard.classList.add('shake');
                    playBuzzerSound(); // Play buzzer sound for misflag

                    cellData.isRevealed = true;
                    cellData.isMisflagged = true;
                    cellData.isFlagged = false;

                    if (cellData.isHeartBonus) {
                        cellElement.textContent = '❤️';
                    } else if (cellData.isTreasureBonus) {
                        cellElement.textContent = '🎁';
                    } else {
                        cellElement.textContent = '';
                    }

                    cellElement.classList.add('revealed', 'misflagged-red-bg');
                    cellElement.classList.remove('flagged');

                    setTimeout(() => {
                        gameBoard.classList.remove('shake');
                        isShaking = false;
                        if (currentLife <= 0) {
                            revealAllMines();
                            endGame(false);
                        }
                    }, 500);
                } else if (cellData.isMine) {
                    cellData.isFlagged = true;
                    cellElement.classList.add('flagged');
                    cellElement.textContent = '🚩';
                    totalFlagsPlacedInCurrentGame++;
                } else {
                    currentLife -= mineDamage;
                    if (lifeDisplay) lifeDisplay.textContent = String(currentLife).padStart(2, '0');

                    isShaking = true;
                    gameBoard.classList.add('shake');
                    playBuzzerSound();

                    cellData.isRevealed = true;
                    cellData.isMisflagged = true;
                    cellData.isFlagged = false;

                    if (cellData.minesAround > 0) {
                        cellElement.textContent = cellData.minesAround;
                        cellElement.dataset.mines = cellData.minesAround;
                    } else {
                        cellElement.textContent = '';
                    }
                    cellElement.classList.add('revealed', 'misflagged-red-bg');
                    cellElement.classList.remove('flagged');

                    setTimeout(() => {
                        gameBoard.classList.remove('shake');
                        isShaking = false;
                        if (currentLife <= 0) {
                            revealAllMines();
                            endGame(false);
                        }
                    }, 500);
                }
            }
            updateCounters();
        }


        function revealCell(r, c, cellElement = null) {
            if (r < 0 || r >= rows || c < 0 || c >= cols || board[r][c].isRevealed || board[r][c].isFlagged) {
                return false;
            }

            const cellData = board[r][c];
            if (!cellElement) {
                cellElement = gameBoard.children[r * cols + c];
            }

            if (cellData.isMine) {
                cellData.isRevealed = true;
                if (cellElement) {
                    cellElement.classList.add('revealed', 'mine');
                    cellElement.textContent = '💣';
                }
                return true;
            }

            cellData.isRevealed = true;
            cellElement.classList.add('revealed');
            revealedCellsCount++;

            // Handle bonus effects first
            if (cellData.isHeartBonus) {
                cellElement.classList.add('heart');
                cellElement.textContent = '❤️';
                currentLife += 5; // Life recovery reduced to 5 (changed from 10)
                if (lifeDisplay) lifeDisplay.textContent = String(currentLife).padStart(2, '0');
                cellElement.classList.add('heart-effect'); // Add the pulsing effect
                playPianoPhrase(); // Play piano phrase for heart discovery
                setTimeout(() => {
                    cellElement.classList.remove('heart-effect');
                    cellElement.classList.remove('heart'); // Remove the heart class
                    cellData.isHeartBonus = false; // Reset the flag after animation
                    // Revert to displaying minesAround number or empty
                    if (cellData.minesAround > 0) { // This minesAround is for the non-bonus state
                        cellElement.textContent = cellData.minesAround;
                        cellElement.dataset.mines = cellData.minesAround;
                    } else {
                        cellElement.textContent = '';
                    }
                }, 3000); // Heart effect for 3 seconds
            } else if (cellData.isTreasureBonus) {
                if (cellData.isTreasureClosed) { // This means it's being opened for the first time
                    cellElement.classList.add('treasure-closed');
                    cellData.isTreasureClosed = false; // Mark as opened
                    cellElement.textContent = '🎁';
                    playPianoPhrase(); // Play piano phrase for treasure discovery
                    setTimeout(() => {
                        cellElement.textContent = '💰';
                        cellElement.classList.remove('treasure-closed');
                        cellElement.classList.add('treasure-open');
                        cellElement.classList.add('treasure-bounce-animation'); // Add animation class here
                        currentLife += mineDamage;
                        if (lifeDisplay) lifeDisplay.textContent = String(currentLife).padStart(2, '0');
                        playCoinSound(); // Play coin sound when treasure opens
                    }, 1000);
                } else { // Already opened, just render the open treasure icon without re-animating
                    cellElement.textContent = '💰';
                    cellElement.classList.add('treasure-open');
                    // Do NOT add treasure-bounce-animation here
                }
            }

            // Now handle normal cell display and recursive reveal if it's a 0-mine cell
            // This part should execute for all non-mine cells, including bonus cells after their effects.
            // Heart cells have minesAround === 0, so they will trigger this recursive reveal.
            if (cellData.minesAround > 0 && !cellData.isHeartBonus && !cellData.isTreasureBonus) { // Ensure it's a regular number cell
                cellElement.textContent = cellData.minesAround;
                cellElement.dataset.mines = cellData.minesAround;
            } else if (cellData.minesAround === 0 && !cellData.isMine) { // This covers empty cells and heart cells (which have minesAround 0)
                // Only clear text content if it's not a bonus cell that has its own icon
                // For heart/treasure, their icon is set in the bonus handling block
                if (!cellData.isHeartBonus && !cellData.isTreasureBonus) {
                    cellElement.textContent = '';
                }
                // Recursively reveal neighbors for 0-mine cells (including heart cells after their effect)
                for (let dr = -1; dr <= 1; dr++) {
                    for (let dc = -1; dc <= 1; dc++) {
                        if (dr === 0 && dc === 0) continue;
                        // Pass the result of recursive call to check for mine hit
                        if (revealCell(r + dr, c + dc)) {
                            return true; // Propagate mine hit up the chain
                        }
                    }
                }
            }
            return false;
        }

        function revealAllMines() {
            for (let r = 0; r < rows; r++) {
                for (let c = 0; c < cols; c++) {
                    const cellData = board[r][c];
                    const cellElement = gameBoard.children[r * cols + c];
                    if (cellData.isMine && !cellData.isRevealed) {
                        cellElement.classList.add('revealed');
                        cellElement.textContent = '💣';
                    }
                    if (cellData.isFlagged && !cellData.isMine) {
                        cellElement.textContent = '❌';
                        cellElement.style.color = '#ff0000';
                    }
                }
            }
        }

        /**
         * Checks if a given row is ready for a board shift.
         * A row is ready if all its cells are revealed or are flagged mines.
         * @param {number} rowIndex - The index of the row to check.
         * @returns {boolean} True if the row is ready, false otherwise.
         */
        function isRowReadyForShift(rowIndex) {
            if (rowIndex < 0 || rowIndex >= rows) return false;
            console.log(`Checking if Row ${rowIndex} is ready for shift...`);
            for (let c = 0; c < cols; c++) {
                const cell = board[rowIndex][c];
                console.log(`  Cell (${rowIndex}, ${c}): isMine=${cell.isMine}, isRevealed=${cell.isRevealed}, isFlagged=${cell.isFlagged}, isMisflagged=${cell.isMisflagged}, isHeartBonus=${cell.isHeartBonus}, isTreasureBonus=${cell.isTreasureBonus}`);

                // A cell is considered "ready" if it's revealed, OR if it's a flagged mine.
                // Misflagged cells are already set to isRevealed = true, so they will pass this check.
                if (!cell.isRevealed && !(cell.isMine && cell.isFlagged)) {
                    console.log(`    Cell (${rowIndex}, ${c}) is NOT ready: It's not revealed AND not a flagged mine.`);
                    return false; // This cell blocks the row from being ready.
                }
            }
            console.log(`Row ${rowIndex} isReady: true`);
            return true; // All cells in the row are ready.
        }

        /**
         * Automatically reveals zero-mine cells and their neighbors.
         * This simulates a "chording" action for all suitable 0-cells.
         */
        function autoRevealZeroCells() {
            let cellsToProcess = [];
            // Collect all revealed, non-mine, zero-mine cells that might trigger a chain
            for (let r = 0; r < rows; r++) {
                for (let c = 0; c < cols; c++) {
                    const cellData = board[r][c];
                    // Include treasure cells here as they can also trigger auto-reveal if they have 0 mines around them
                    if (cellData.isRevealed && !cellData.isMine && cellData.minesAround === 0) {
                        // Check if it has any unrevealed, unflagged neighbors
                        const neighbors = getNeighbors(r, c);
                        const hasUnrevealedUnflagged = neighbors.some(n => !n.data.isRevealed && !n.data.isFlagged);
                        if (hasUnrevealedUnflagged) {
                            cellsToProcess.push({r, c});
                        }
                    }
                }
            }

            // Process the collected cells to trigger revelations
            // Use a queue to ensure proper BFS-like expansion
            let queue = [...cellsToProcess];
            let visited = new Set(); // To prevent infinite loops for already processed cells
            let mineHitDuringAutoReveal = false;

            while (queue.length > 0) {
                const {r, c} = queue.shift();
                const cellKey = `${r},${c}`;

                if (visited.has(cellKey)) continue;
                visited.add(cellKey);

                const neighbors = getNeighbors(r, c);
                for (const neighbor of neighbors) {
                    if (!neighbor.data.isRevealed && !neighbor.data.isFlagged) {
                        // Reveal the neighbor. If it's also a 0-cell, it will be added to the queue by revealCell's recursion.
                        if (revealCell(neighbor.r, neighbor.c, neighbor.element)) {
                            mineHitDuringAutoReveal = true;
                            break; // Stop processing current neighbors if a mine is hit
                        }
                        // If revealCell itself triggers a recursive call for a 0-cell, it will handle it.
                        // We just need to ensure this top-level autoRevealZeroCells continues finding new starting points.
                        // This might be redundant if revealCell's recursion is robust, but ensures all chains are followed.
                        if (board[neighbor.r][neighbor.c].isRevealed && !board[neighbor.r][neighbor.c].isMine && board[neighbor.r][neighbor.c].minesAround === 0) {
                             const newNeighbors = getNeighbors(neighbor.r, neighbor.c);
                             const hasNewUnrevealedUnflagged = newNeighbors.some(n => !n.data.isRevealed && !n.data.isFlagged);
                             if (hasNewUnrevealedUnflagged && !visited.has(`${neighbor.r},${neighbor.c}`)) {
                                 queue.push({r: neighbor.r, c: neighbor.c});
                             }
                        }
                    }
                }
                if (mineHitDuringAutoReveal) break; // Break outer loop if a mine was hit
            }

            if (mineHitDuringAutoReveal) {
                currentLife -= mineDamage;
                if (lifeDisplay) lifeDisplay.textContent = String(currentLife).padStart(2, '0');
                isShaking = true;
                gameBoard.classList.add('shake');
                playExplosionSound(); // Play explosion sound on mine hit during auto-reveal
                setTimeout(() => {
                    gameBoard.classList.remove('shake');
                    isShaking = false;
                    if (currentLife <= 0) {
                        revealAllMines();
                        endGame(false);
                    }
                }, 500);
            }

            // After all auto-reveals, re-render to ensure visual consistency
            renderBoard();
        }


        function updateCounters() {
            if (flagsPlacedDisplay) flagsPlacedDisplay.textContent = String(totalFlagsPlacedInCurrentGame).padStart(3, '0');
        }

        function checkShiftCondition() {
            console.log("Checking shift condition... currentLife:", currentLife); // Debug log
            if (currentLife <= 0) { // If life is 0 or less, do not shift. Game is over.
                console.log("Life is 0 or less, preventing shift.");
                return;
            }

            const rowMinus1Ready = isRowReadyForShift(rows - 1);
            const rowMinus2Ready = isRowReadyForShift(rows - 2);
            const rowMinus3Ready = isRowReadyForShift(rows - 3);

            if (rowMinus1Ready && rowMinus2Ready && rowMinus3Ready) {
                console.log("All bottom three rows are ready for shift. Initiating shift."); // Debug log
                shiftBoardDown();
            } else {
                let debugMessage = "スクロール条件が満たされていません。\n"; // Scroll condition not met.
                if (!rowMinus1Ready) debugMessage += `最下行 (R${rows-1}) がクリアされていません。\n`; // Bottom row (R8) not cleared.
                if (!rowMinus2Ready) debugMessage += `下から2番目の行 (R${rows-2}) がクリアされていません。\n`; // 2nd from bottom row (R7) not cleared.
                if (!rowMinus3Ready) debugMessage += `下から3番目の行 (R${rows-3}) がクリアされていません。\n`; // 3rd from bottom row (R6) not cleared.
                console.log(debugMessage); // Debug log
                // showMessageBox(debugMessage); // Uncomment this line to show a message box to the user for debugging
            }
        }

        function showMessageBox(messageKey) {
            if (messageText) messageText.textContent = translations[currentLanguage][messageKey];
            messageBox.classList.add('active');
        }

        function shiftBoardDown() {
            isShifting = true;
            gameBoard.classList.add('shifting-down');

            currentLife += 1;
            if (lifeDisplay) lifeDisplay.textContent = String(currentLife).padStart(2, '0');

            // Adjust mine damage increase frequency based on difficulty
            if (scrollCount % difficulties[currentDifficulty].damageIncreaseInterval === 0) {
                mineDamage = Math.floor(mineDamage * 1.05);
                if (mineDamage === 0) mineDamage = 1;
                if (mineDamageDisplay) mineDamageDisplay.textContent = String(mineDamage).padStart(2, '0');
            }
            scrollCount++;
            playScrollSound(); // Play scroll sound for board scroll


            setTimeout(() => {
                // 1. Remove the bottom row (n)
                board.pop();
                console.log("Board after pop (old bottom row removed).");

                // 2. Create the new top row (n+2) with only bonus cells
                const newTopRowData = createNewRowWithBonusesOnly();
                console.log("Created newTopRowData (n+2, bonuses only):", newTopRowData);

                // 3. Prepare the row that will become n+1 (currently board[0]) for new mine placement
                const rowToPlaceMinesIn = board[0]; // This is the row that will be board[1] after unshift
                console.log("Preparing rowToPlaceMinesIn (n+1, old board[0]):", rowToPlaceMinesIn);

                // Reset relevant flags for cells in this row before placing new mines
                for (let c = 0; c < cols; c++) {
                    const cell = rowToPlaceMinesIn[c];
                    cell.isMine = false; // Clear any old mine state
                    cell.isRevealed = false; // Crucial: Reset revealed state to prevent "revealed mine" bug
                    cell.isFlagged = false; // Reset flagged state
                    cell.isMisflagged = false; // Reset misflagged state
                    // IMPORTANT: Do NOT reset isHeartBonus or isTreasureBonus here, as per previous user request.
                    // These flags should only be reset when the cell is *revealed and its effect applied*.
                    cell.minesAround = 0; // Will be recalculated
                }
                console.log("rowToPlaceMinesIn after partial reset (bonus flags preserved):", rowToPlaceMinesIn);

                let minesPlacedInThisRow = 0;
                let cellsForRandomMines = []; // Collect cells that are not forced mine/non-mine

                // Helper function to get cell data from the "virtual" 3-row block for neighbor checks
                // rOffset: -1 for row above (newTopRowData), 0 for current row (rowToPlaceMinesIn), 1 for row below (board[1])
                const getVirtualCellForMinePlacement = (rOffset, cIndex) => {
                    if (cIndex < 0 || cIndex >= cols) return undefined;

                    if (rOffset === -1) return newTopRowData[cIndex]; // The new row that will be board[0] (n+2)
                    if (rOffset === 0) return rowToPlaceMinesIn[cIndex]; // The row currently being processed (n+1)
                    if (rOffset === 1) return board[1] ? board[1][cIndex] : undefined; // The row below (n, old board[1])
                    return undefined;
                };

                // First pass: Apply constraints for forced mine/non-mine cells in rowToPlaceMinesIn
                for (let c = 0; c < cols; c++) {
                    const cell = rowToPlaceMinesIn[c];

                    // If the cell itself is a bonus cell, it cannot be a mine
                    if (cell.isHeartBonus || cell.isTreasureBonus) {
                        cell.isMine = false;
                        console.log(`  Cell (current board[0], ${c}) forced NOT mine (is bonus cell)`);
                        continue; // Skip to next cell
                    }

                    let mustNotBeMine = false;
                    let mustBeMine = false;

                    // Check neighbors for heart/treasure bonuses
                    for (let dr = -1; dr <= 1; dr++) {
                        for (let dc = -1; dc <= 1; dc++) {
                            if (dr === 0 && dc === 0) continue;
                            const neighbor = getVirtualCellForMinePlacement(dr, c + dc);
                            if (neighbor) {
                                if (neighbor.isHeartBonus) {
                                    mustNotBeMine = true;
                                }
                                if (neighbor.isTreasureBonus) {
                                    mustBeMine = true;
                                }
                            }
                        }
                    }

                    // Apply constraints based on priority: mustNotBeMine > mustBeMine
                    if (mustNotBeMine) {
                        cell.isMine = false;
                        console.log(`  Cell (current board[0], ${c}) forced NOT mine (neighbor heart)`);
                    } else if (mustBeMine) {
                        cell.isMine = true;
                        minesPlacedInThisRow++;
                        console.log(`  Cell (current board[0], ${c}) forced MINE (neighbor treasure)`);
                    } else {
                        // If no strong constraint, add to candidates for random placement
                        cellsForRandomMines.push(c);
                        console.log(`  Cell (current board[0], ${c}) added to random candidates`);
                    }
                }

                // Second pass: Place remaining mines randomly among candidates
                const minesForTargetRow = Math.max(1, Math.round(mines / rows)); // Number of mines for this row
                const minesToPlaceRandomly = Math.max(0, minesForTargetRow - minesPlacedInThisRow);
                console.log(`Need to place ${minesToPlaceRandomly} random mines in rowToPlaceMinesIn.`);
                for (let i = 0; i < minesToPlaceRandomly && cellsForRandomMines.length > 0; i++) {
                    const randomIndex = Math.floor(Math.random() * cellsForRandomMines.length);
                    const c = cellsForRandomMines[randomIndex];
                    
                    const cell = rowToPlaceMinesIn[c];
                    if (!cell.isMine) { // Ensure it wasn't already set as mine by treasure constraint
                        cell.isMine = true;
                        minesPlacedInThisRow++;
                        console.log(`  Cell (current board[0], ${c}) placed random MINE`);
                    }
                    cellsForRandomMines.splice(randomIndex, 1); // Remove from potential list
                }
                console.log("rowToPlaceMinesIn after mine placement:", rowToPlaceMinesIn);


                // 4. Add the new top row to the board (performs the "slide")
                board.unshift(newTopRowData);
                console.log("Board after unshift (new top row added).");


                // 5. Recalculate mines around for the entire board (crucial after mine placement)
                calculateMinesAround();
                console.log("Board after calculateMinesAround (all cells updated).");


                // 6. Re-render the board
                renderBoard();

                revealedCellsCount = board.flat().filter(cell => cell.isRevealed).length;
                updateCounters();

                gameBoard.classList.remove('shifting-down');
                isShifting = false;
                checkShiftCondition();
                autoRevealZeroCells(); // Call auto-reveal after shift
            }, 500);
        }

        function endGame(won) {
            gameOver = true;
            clearInterval(timerInterval);
            if (resetButton) resetButton.textContent = '😵';

            currentPlayedScore = { flags: totalFlagsPlacedInCurrentGame, time: timeElapsed };
            currentRankingDifficulty = currentDifficulty; // Set ranking display to current game's difficulty

            displayRankingScreen(true); // true to indicate it's from game over
        }

        /**
         * Displays the ranking screen.
         * @param {boolean} fromGameOver - True if called from game over, false if from title screen.
         */
        function displayRankingScreen(fromGameOver) {
            rankingScreenTitle.textContent = fromGameOver ? translations[currentLanguage].ranking_gameOver : translations[currentLanguage].ranking_title;
            resultFlagsDisplayContainer.style.display = fromGameOver ? 'block' : 'none';
            resultTimeDisplayContainer.style.display = fromGameOver ? 'block' : 'none';
            if (messageText) messageText.style.display = 'none'; // Ensure no generic message

            if (fromGameOver) {
                if (resultFlags) resultFlags.textContent = totalFlagsPlacedInCurrentGame;
                if (resultTime) resultTime.textContent = timeElapsed;
                saveScore(totalFlagsPlacedInCurrentGame, timeElapsed, currentDifficulty);
            }

            loadAndDisplayRanking(currentRankingDifficulty); // Load ranking for the selected difficulty
            updateRankingDifficultyButtons(); // Update active state of ranking buttons
            showScreen(rankingScreen);
        }

        /**
         * Saves the current score to localStorage and updates the best scores for the given difficulty.
         * @param {number} flags - Number of flags placed.
         * @param {number} time - Time taken.
         * @param {string} difficultyKey - The difficulty key ('easy', 'medium', 'hard').
         */
        function saveScore(flags, time, difficultyKey) {
            const localStorageKey = `minesweeperBestScores_${difficultyKey}`;
            let bestScores = JSON.parse(localStorage.getItem(localStorageKey)) || [];
            const newScore = { flags: flags, time: time, date: new Date().toLocaleString(currentLanguage === 'ja' ? 'ja-JP' : 'en-US') };
            bestScores.push(newScore);

            bestScores.sort((a, b) => {
                if (b.flags !== a.flags) {
                    return b.flags - a.flags;
                }
                return a.time - b.time;
            });

            bestScores = bestScores.slice(0, 10);
            localStorage.setItem(localStorageKey, JSON.stringify(bestScores));
        }

        /**
         * Loads best scores from localStorage for the given difficulty and displays them in the ranking table.
         * @param {string} difficultyKey - The difficulty key ('easy', 'medium', 'hard').
         */
        function loadAndDisplayRanking(difficultyKey) {
            const localStorageKey = `minesweeperBestScores_${difficultyKey}`;
            const bestScores = JSON.parse(localStorage.getItem(localStorageKey)) || [];
            if (rankingList) rankingList.innerHTML = ''; // Clear previous ranking

            if (currentRankingDifficultyDisplay) currentRankingDifficultyDisplay.textContent = `${translations[currentLanguage].ranking_currentDifficulty} ${difficulties[difficultyKey][`name_${currentLanguage}`]}`;

            if (bestScores.length === 0) {
                const row = rankingList.insertRow();
                const cell = row.insertCell();
                cell.colSpan = 4;
                cell.textContent = translations[currentLanguage].ranking_noScore;
                return;
            }

            bestScores.forEach((score, index) => {
                const row = rankingList.insertRow();
                // Highlight if this is the current game's score and it matches the difficulty
                if (currentPlayedScore && score.flags === currentPlayedScore.flags && score.time === currentPlayedScore.time && !score.highlighted && difficultyKey === currentDifficulty) {
                    row.classList.add('highlight-score');
                    // Mark as highlighted to prevent re-highlighting if multiple identical scores exist
                    score.highlighted = true;
                    // Note: currentPlayedScore is cleared after the first highlight to prevent multiple highlights across different difficulties.
                    // If you want to highlight the same score in multiple difficulty rankings (e.g., if a score is valid for multiple difficulties),
                    // you would need a more complex highlighting logic.
                }
                row.insertCell().textContent = index + 1;
                row.insertCell().textContent = score.flags;
                row.insertCell().textContent = score.time;
                row.insertCell().textContent = score.date;
            });
            // Persist the highlighted status if needed (optional, for refresh persistence)
            // localStorage.setItem(localStorageKey, JSON.stringify(bestScores));
        }

        /**
         * Updates the active state of the ranking difficulty buttons.
         */
        function updateRankingDifficultyButtons() {
            document.querySelectorAll('.ranking-difficulty-button').forEach(button => {
                if (button.dataset.difficulty === currentRankingDifficulty) {
                    button.classList.add('active');
                } else {
                    button.classList.remove('active');
                }
            });
        }

        function startTimer() {
            timerInterval = setInterval(() => {
                timeElapsed++;
                if (timerDisplay) timerDisplay.textContent = String(timeElapsed).padStart(3, '0');
            }, 1000);
        }

        /**
         * Saves the current game state to localStorage.
         */
        function saveCurrentGame() {
            playMenuSound();
            clearInterval(timerInterval); // Stop timer when saving
            const saveData = {
                board: board,
                rows: rows,
                cols: cols,
                mines: mines,
                currentLife: currentLife,
                mineDamage: mineDamage,
                scrollCount: scrollCount,
                timeElapsed: timeElapsed,
                totalFlagsPlacedInCurrentGame: totalFlagsPlacedInCurrentGame,
                currentDifficulty: currentDifficulty
            };
            localStorage.setItem('minesweeperSaveData', JSON.stringify(saveData));
            showMessageBox('message_gameSaved');
            gameOver = true; // Mark game as over for proper reset when returning to title
            showScreen(titleScreen);
        }

        /**
         * Loads the game state from localStorage and resumes the game.
         */
        function loadSavedGame() {
            playMenuSound();
            const savedData = localStorage.getItem('minesweeperSaveData');
            if (savedData) {
                const data = JSON.parse(savedData);
                // Set global game parameters based on saved data
                rows = data.rows;
                cols = data.cols;
                mines = data.mines;
                currentDifficulty = data.currentDifficulty; // Set difficulty before initializing
                initializeGame(data); // Pass saved data to initializeGame
                localStorage.removeItem('minesweeperSaveData'); // Clear save data after loading
            } else {
                showMessageBox('message_noSaveData');
                showScreen(titleScreen); // No saved data, return to title
            }
        }

        /**
         * Shows the pause menu and pauses the game timer.
         */
        function showPauseMenu() {
            playMenuSound();
            clearInterval(timerInterval); // Pause the timer
            showScreen(pauseMenu);
        }

        /**
         * Resumes the game from the pause menu.
         */
        function resumeGame() {
            playMenuSound();
            showScreen(gameContainer);
            if (gameStarted && !gameOver) { // Only resume timer if game was actually started and not over
                startTimer();
            }
        }

        /**
         * Sets the master volume for Tone.js and updates local storage.
         * @param {number} volume - Volume percentage (0-100).
         */
        function setVolume(volume) {
            currentVolume = volume;
            if (volumeSlider) volumeSlider.value = volume;
            if (volumePercentage) volumePercentage.textContent = `${volume}%`;
            // Tone.js volume is in dB. 0% is -Infinity, 100% is 0dB.
            // Use Tone.gainToDb to convert linear gain (0-1) to dB.
            Tone.Destination.volume.value = Tone.gainToDb(volume / 100);
            localStorage.setItem('minesweeperVolume', currentVolume);
        }

        /**
         * Sets the current language and updates the display.
         * @param {string} lang - Language code ('ja' or 'en').
         */
        function setLanguage(lang) {
            currentLanguage = lang;
            localStorage.setItem('minesweeperLanguage', currentLanguage);
            updateLanguageDisplay();
        }

        // Event Listeners for new flow
        startGameButton.addEventListener('click', () => {
            playMenuSound();
            const savedData = localStorage.getItem('minesweeperSaveData');
            if (savedData) {
                showScreen(resumeOptionsScreen);
            } else {
                showScreen(difficultySelectionScreen);
            }
        });

        // Resume options screen buttons
        resumeDataButton.addEventListener('click', loadSavedGame);

        startNewGameButton.addEventListener('click', () => {
            playMenuSound();
            // Do NOT clear save data here. It should be cleared only when a new game difficulty is selected.
            showScreen(difficultySelectionScreen);
        });

        showRankingButton.addEventListener('click', () => {
            playMenuSound();
            currentRankingDifficulty = currentDifficulty; // Default to the last played difficulty or 'easy'
            displayRankingScreen(false); // false to indicate it's not from game over
        });

        showManualButton.addEventListener('click', () => {
            playMenuSound();
            showScreen(manualScreen);
            loadManualChapter('rules'); // Load basic rules by default
        });

        closeManualButton.addEventListener('click', () => {
            playMenuSound();
            showScreen(titleScreen);
        });

        backToTitleButton.addEventListener('click', () => {
            playMenuSound();
            showScreen(titleScreen);
        });

        backToTitleFromDifficultyButton.addEventListener('click', () => {
            playMenuSound();
            showScreen(titleScreen); // Just go back to title screen, keep save data
        });

        // Clicking anywhere on the ranking screen (except the button) returns to title
        rankingScreen.addEventListener('click', (event) => {
            // Check if the click target is the ranking screen itself, not its children buttons/elements
            if (event.target === rankingScreen || event.target.tagName === 'TABLE' || event.target.tagName === 'THEAD' || event.target.tagName === 'TBODY' || event.target.tagName === 'TR' || event.target.tagName === 'TH' || event.target.tagName === 'TD' || event.target.tagName === 'P' || event.target.tagName === 'SPAN' || event.target.tagName === 'H2' || event.target.tagName === 'H3' || event.target.classList.contains('ranking-difficulty-controls') || event.target.classList.contains('ranking-difficulty-button')) {
                // Do nothing if a button or table element is clicked, to allow button functionality
                return;
            }
            playMenuSound();
            showScreen(titleScreen);
        });


        easyButton.addEventListener('click', () => {
            playMenuSound();
            localStorage.removeItem('minesweeperSaveData'); // Clear save data when starting a new game
            currentDifficulty = 'easy';
            const settings = difficulties.easy;
            rows = settings.rows;
            cols = settings.cols;
            mines = settings.mines;
            initializeGame();
        });

        mediumButton.addEventListener('click', () => {
            playMenuSound();
            localStorage.removeItem('minesweeperSaveData'); // Clear save data when starting a new game
            currentDifficulty = 'medium';
            const settings = difficulties.medium;
            rows = settings.rows;
            cols = settings.cols;
            mines = settings.mines;
            initializeGame();
        });

        hardButton.addEventListener('click', () => {
            playMenuSound();
            localStorage.removeItem('minesweeperSaveData'); // Clear save data when starting a new game
            currentDifficulty = 'hard';
            const settings = difficulties.hard;
            rows = settings.rows;
            cols = settings.cols;
            mines = settings.mines;
            initializeGame();
        });

        superHardButton.addEventListener('click', () => {
            playMenuSound();
            localStorage.removeItem('minesweeperSaveData'); // Clear save data when starting a new game
            currentDifficulty = 'superHard';
            const settings = difficulties.superHard;
            rows = settings.rows;
            cols = settings.cols;
            mines = settings.mines;
            initializeGame();
        });
        
        hardcoreButton.addEventListener('click', () => {
            playMenuSound();
            localStorage.removeItem('minesweeperSaveData'); // Clear save data when starting a new game
            currentDifficulty = 'hardcore';
            const settings = difficulties.hardcore;
            rows = settings.rows;
            cols = settings.cols;
            mines = settings.mines;
            initializeGame();
        });

        // Ranking difficulty selection buttons
        rankingEasyButton.addEventListener('click', () => {
            playMenuSound();
            currentRankingDifficulty = 'easy';
            loadAndDisplayRanking('easy');
            updateRankingDifficultyButtons();
        });
        rankingMediumButton.addEventListener('click', () => {
            playMenuSound();
            currentRankingDifficulty = 'medium';
            loadAndDisplayRanking('medium');
            updateRankingDifficultyButtons();
        });
        rankingHardButton.addEventListener('click', () => {
            playMenuSound();
            currentRankingDifficulty = 'hard';
            loadAndDisplayRanking('hard');
            updateRankingDifficultyButtons();
        });
        rankingSuperHardButton.addEventListener('click', () => {
            playMenuSound();
            currentRankingDifficulty = 'superHard';
            loadAndDisplayRanking('superHard');
            updateRankingDifficultyButtons();
        });
        rankingHardcoreButton.addEventListener('click', () => {
            playMenuSound();
            currentRankingDifficulty = 'hardcore';
            loadAndDisplayRanking('hardcore');
            updateRankingDifficultyButtons();
        });

        // resetButton event listener now opens pause menu
        resetButton.addEventListener('click', showPauseMenu);

        // Pause menu button event listeners
        resumeGameButton.addEventListener('click', resumeGame);
        saveGameButton.addEventListener('click', saveCurrentGame);

        messageOkButton.addEventListener('click', () => {
            playMenuSound();
            messageBox.classList.remove('active');
        });

        // New Options screen event listeners
        showOptionsButton.addEventListener('click', () => {
            playMenuSound();
            showScreen(optionsScreen);
            // Initialize slider and language buttons when options screen is shown
            if (volumeSlider) volumeSlider.value = currentVolume;
            if (volumePercentage) volumePercentage.textContent = `${currentVolume}%`;
            updateLanguageDisplay(); // Update language buttons active state
        });

        volumeSlider.addEventListener('input', (event) => {
            setVolume(parseInt(event.target.value));
        });

        langJapaneseButton.addEventListener('click', () => {
            playMenuSound();
            setLanguage('ja');
        });

        langEnglishButton.addEventListener('click', () => {
            playMenuSound();
            setLanguage('en');
        });

        backToTitleFromOptionsButton.addEventListener('click', () => {
            playMenuSound();
            showScreen(titleScreen);
        });

        // Initial screen setup
        window.onload = () => {
            // Set initial volume for Tone.js
            setVolume(currentVolume);
            // Set initial language and update all display elements
            updateLanguageDisplay();
            showScreen(titleScreen);

            // manualNavButtonsの宣言とイベントリスナーの割り当てをここに移す
            const manualNavButtons = document.querySelectorAll('.manual-nav-button');
            manualNavButtons.forEach(button => {
                button.addEventListener('click', () => {
                    loadManualChapter(button.dataset.chapter);
                });
            });
        };
    </script>
</body>
</html>
