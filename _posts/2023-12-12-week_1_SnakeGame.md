---
toc: false
comments: false
layout: post
title: Eel Game
description: A Game that I created that takes place in an underwater world, an eel eating fish.
type: tangibles
courses: { compsci: {week: 1} }
---


<style>


    body{
    }
    .wrap{
        margin-left: auto;
        margin-right: auto;
    }


    canvas{
        display: none;
    }
   
    canvas:focus{
        outline: none;
    }


    /* All screens style */
    #gameover p, #setting p, #menu p{
        font-size: 25px;
    }


    #gameover a, #setting a, #menu a{
        font-size: 30px;
        display: block;
    }


    #gameover a:hover, #setting a:hover, #menu a:hover{
        cursor: pointer;
    }


    #gameover a:hover::before, #setting a:hover::before, #menu a:hover::before{
        margin-right: 10px;
    }


    #menu{
        display: block;
    }


    #gameover{
        display: none;
    }


    #setting{
        display: none;
    }


    #setting input{
        display:none;
    }


    #setting label{
        cursor: pointer;
    }


    #setting input:checked + label{
        background: #00008B;
        -webkit-text-fill-color: transparent;
        -webkit-background-clip: text;
    }


     #score_value {
        font-size: 40px;
        text-align: center;
    }


    .fs-4 {
        font-size: 40px;
        font-weight: bold;
        text-align: center;
    }
    .theme-dark {
        background-color: #010203;
        color: #fff;
    }
     .theme-dark h1 {
        color: #fff;
    }
    .theme-light {
        background-color: #E8DACC;
    }
<style>

    body{
    }
    .wrap{
        margin-left: auto;
        margin-right: auto;
    }

    canvas{
        border-radius: 4px; box-shadow: 0px 0px 30px #00FF00;
        display: none;
        border-style: solid;
        border-width: 10px;
        border-color: #00FF00;
    }
    canvas:focus{
        outline: none;
    }

    /* All screens style */
    #gameover p, #setting p, #menu p{
        font-size: 20px;
    }

    #gameover a, #setting a, #menu a{
        font-size: 30px;
        display: block;
    }

    #gameover a:hover, #setting a:hover, #menu a:hover{
        cursor: pointer;
    }

    #gameover a:hover::before, #setting a:hover::before, #menu a:hover::before{
        margin-right: 10px;
    }

    #menu{
        display: block;
    }

    #gameover{
        display: none;
    }

    #setting{
        display: none;
    }

    #setting input{
        display:none;
    }

    #setting label{
        cursor: pointer;
    }

    #setting input:checked + label{
        background: linear-gradient(to right, #ADD8E6, #0000FF); 
        -webkit-text-fill-color: transparent; 
        -webkit-background-clip: text;
    }

     #score_value {
        font-size: 40px;
        text-align: center;
    }

    .fs-4 {
        font-size: 40px;
        font-weight: bold;
        text-align: center;
    } 
    .theme-dark {
        background-color: #010203;
        color: #fff;
    }
    .theme-dark h1 {
        color: #fff;
    }
    .theme-light {
        background-color: #E8DACC;
    }
</style>


<div class="container">
    <header class="pb-3 mb-4 border-bottom border-primary text-dark">
        <p class="fs-4">🐟: <span id="score_value">0</span></p>
    </header>
    <div class="container bg-secondary" style="text-align:center;">
        <!-- Main Menu -->
        <div id="menu" class="py-4 text-light">
            <p>Move Using Arrows or WASD to Begin!</p>
            <a id="new_game" class="link-alert" style="font-size: 20px;">New Game</a>
            <a id="setting_menu" class="link-alert" style="font-size: 20px; ">Settings</a>
        </div>
        <!-- Game Over -->
        <div id="gameover" class="py-4 text-light" style="color: #D2042D; font-weight: bold;">
            <p style="color:red">Game over. Better Luck Next Time!</p>
            <a id="new_game1" class="link-alert" style="font-size: 20px; ">New Game</a>
            <a id="setting_menu1" class="link-alert" style="font-size: 20px; ">Settings</a>
            <br>
        </div>
        <!-- Play Screen -->
        <canvas id="snake" class="wrap" width="480" height="480" tabindex="1"></canvas>
        <!-- Settings Screen -->
        <div id="setting" class="py-4 text-light">
            <p>Settings Screen, press space to go back to playing</p>
            <a id="new_game2" class="link-alert">New Game</a>
            <br>
            <p>Speed:
                <input id="speed1" type="radio" name="speed" value="95"/>
                <label for="speed1">Slow</label>
                <input id="speed2" type="radio" name="speed" value="65" checked /> <!-- Added checked to end of the speed you want to be default -->
                <label for="speed2">Normal</label>
                <input id="speed3" type="radio" name="speed" value="35"/>
                <label for="speed3">Fast</label>
            </p>
            <p>Theme:
                <input type="radio" id="theme-default" name="theme" value="default" checked>
                <label for="theme-default">Default</label>
                <input type="radio" id="theme-dark" name="theme" value="dark">
                <label for="theme-dark">Dark</label>
                <input type="radio" id="theme-light" name="theme" value="light">
                <label for="theme-light">Light</label>
            </p>
            <p>Wall:
                <input id="wallon" type="radio" name="wall" value="1" checked/>
                <label for="wallon">On</label>
                <input id="walloff" type="radio" name="wall" value="0"/>
                <label for="walloff">Off</label>
            </p>
        </div>
    </div>
</div>

<!-- Audio -->
<audio id="pointSound" src="{{site.baseurl}}/audio/points2.wav" preload="auto"></audio>
<audio id="lostSound" src="{{site.baseurl}}/audio/game-over.wav" preload="auto"></audio>
<audio id="winnerSound" src="{{site.baseurl}}/audio/winner.wav" preload="auto"></audio>

<script>
    //disable arrow key scrolling
    window.addEventListener("keydown", function(e) { if(["Space","ArrowUp","ArrowDown","ArrowLeft","ArrowRight"].indexOf(e.code) > -1) { e.preventDefault(); } }, false);


    // Add a function to handle theme switching
    function switchTheme(theme) {
        const body = document.body;

        // Reset all theme-related classes
        body.classList.remove('theme-default', 'theme-dark', 'theme-light');

        // Apply the selected theme class
        body.classList.add(`theme-${theme}`);
    }

    // Add event listeners to the theme radio buttons
    const themeRadios = document.getElementsByName('theme');
    themeRadios.forEach(radio => {
        radio.addEventListener('change', function() {
            const selectedTheme = document.querySelector('input[name="theme"]:checked').value;
            switchTheme(selectedTheme);
        });
    });

    // Initialize with the default theme
    switchTheme('default');

    //Sound when food is picked up 
    function playPointSound() {
    const pointSound = document.getElementById("pointSound");
    pointSound.play();
    }

    //Sound when game ends
    function playLostSound() {
    const lostSound = document.getElementById("lostSound");
    lostSound.play();
    }

    //Sound for score 20
    function playWinnerSound() {
    const winnerSound = document.getElementById("winnerSound");
    winnerSound.play();
    }

    (function(){
        /* Attributes of Game */
        /////////////////////////////////////////////////////////////
        // Canvas & Context
        const canvas = document.getElementById("snake");
        const ctx = canvas.getContext("2d");
        // HTML Game IDs
        const SCREEN_SNAKE = 0;
        const screen_snake = document.getElementById("snake");
        const ele_score = document.getElementById("score_value");
        const speed_setting = document.getElementsByName("speed");
        const wall_setting = document.getElementsByName("wall");
        // HTML Screen IDs (div)
        const SCREEN_MENU = -1, SCREEN_GAME_OVER=1, SCREEN_SETTING=2;
        const screen_menu = document.getElementById("menu");
        const screen_game_over = document.getElementById("gameover");
        const screen_setting = document.getElementById("setting");
        // HTML Event IDs (a tags)
        const button_new_game = document.getElementById("new_game");
        const button_new_game1 = document.getElementById("new_game1");
        const button_new_game2 = document.getElementById("new_game2");
        const button_setting_menu = document.getElementById("setting_menu");
        const button_setting_menu1 = document.getElementById("setting_menu1");
        // Game Control
        const BLOCK = 10;   // size of block rendering
        let SCREEN = SCREEN_MENU;
        let snake;
        let snake_dir;
        let snake_next_dir;
        let snake_speed;
        let food = {x: 0, y: 0};
        let score;
        let wall;
        /* Controls */
        /////////////////////////////////////////////////////////////
        // 0 for the game
        // 1 for the main menu
        // 2 for the Settings screen
        // 3 for the game over screen
        let showScreen = function(screen_opt){
            SCREEN = screen_opt;
            switch(screen_opt){
                case SCREEN_SNAKE:
                    screen_snake.style.display = "block";
                    screen_menu.style.display = "none";
                    screen_setting.style.display = "none";
                    screen_game_over.style.display = "none";
                    break;
                case SCREEN_GAME_OVER:
                    screen_snake.style.display = "block";
                    screen_menu.style.display = "none";
                    screen_setting.style.display = "none";
                    screen_game_over.style.display = "block";
                    break;
                case SCREEN_SETTING:
                    screen_snake.style.display = "none";
                    screen_menu.style.display = "none";
                    screen_setting.style.display = "block";
                    screen_game_over.style.display = "none";
                    break;
            }
        }
        /* Button Functions  */
        /////////////////////////////////////////////////////////////
        window.onload = function(){
            // HTML Events to Functions
            button_new_game.onclick = function(){newGame();};
            button_new_game1.onclick = function(){newGame();};
            button_new_game2.onclick = function(){newGame();};
            button_setting_menu.onclick = function(){showScreen(SCREEN_SETTING);};
            button_setting_menu1.onclick = function(){showScreen(SCREEN_SETTING);};
            // speed (starting speed)
            setSnakeSpeed(55);
            for(let i = 0; i < speed_setting.length; i++){
                speed_setting[i].addEventListener("click", function(){
                    for(let i = 0; i < speed_setting.length; i++){
                        if(speed_setting[i].checked){
                            setSnakeSpeed(speed_setting[i].value);
                        }
                    }
                });
            }
            // wall setting
            setWall(1);
            for(let i = 0; i < wall_setting.length; i++){
                wall_setting[i].addEventListener("click", function(){
                    for(let i = 0; i < wall_setting.length; i++){
                        if(wall_setting[i].checked){
                            setWall(wall_setting[i].value);
                        }
                    }
                });
            }
            // activate window events
            window.addEventListener("keydown", function(evt) {
                // spacebar detected
                if(evt.code === "Space" && SCREEN !== SCREEN_SNAKE)
                    newGame();
            }, true);
        }
        /* Snake Movement  */
        /////////////////////////////////////////////////////////////
        let mainLoop = function(){
            let _x = snake[0].x;
            let _y = snake[0].y;
            snake_dir = snake_next_dir;   // read async event key
            // Direction 0 - Up, 1 - Right, 2 - Down, 3 - Left
            switch(snake_dir){
                case 0: _y--; break;
                case 1: _x++; break;
                case 2: _y++; break;
                case 3: _x--; break;
            }
            snake.pop(); // tail is removed
            snake.unshift({x: _x, y: _y}); // head is new in new position/orientation
            // Wall Checker
            if(wall === 1){
                // Wall on, Game over test
                if (snake[0].x < 0 || snake[0].x === canvas.width / BLOCK || snake[0].y < 0 || snake[0].y === canvas.height / BLOCK){
                    showScreen(SCREEN_GAME_OVER);
                    playLostSound();
                    return;
                }
            }else{
                // Wall Off, Circle around
                for(let i = 0, x = snake.length; i < x; i++){
                    if(snake[i].x < 0){
                        snake[i].x = snake[i].x + (canvas.width / BLOCK);
                    }
                    if(snake[i].x === canvas.width / BLOCK){
                        snake[i].x = snake[i].x - (canvas.width / BLOCK);
                    }
                    if(snake[i].y < 0){
                        snake[i].y = snake[i].y + (canvas.height / BLOCK);
                    }
                    if(snake[i].y === canvas.height / BLOCK){
                        snake[i].y = snake[i].y - (canvas.height / BLOCK);
                    }
                }
            }
            // Snake vs Snake checker
            for(let i = 1; i < snake.length; i++){
                // Game over test
                if (snake[0].x === snake[i].x && snake[0].y === snake[i].y){
                    showScreen(SCREEN_GAME_OVER);
                    playLostSound();
                    return;
                }
            }
            // Snake eats food checker
            if(checkBlock(snake[0].x, snake[0].y, food.x, food.y)){
                snake[snake.length] = {x: snake[0].x, y: snake[0].y};
                altScore(++score);
                addFood();
                activeDot(food.x, food.y);

                playPointSound();

            //If the score is 20 or above, do the following
              if(score >= 20) {
                    canvas.style.borderColor = "#00FF00";
                    // Check if canvas size has been shrunk
                    if(canvas.width !== 500) {
                        canvas.width = 500;
                        canvas.height = 500;
                    }
                    if(score === 20) {
                        playWinnerSound();
                    }
                }
                else {

                    if(canvas.width !== 480) {
                        canvas.width = 500;
                        canvas.height = 500;
                    }
                }
            }

            // Repaint canvas
            const my_gradient = ctx.createLinearGradient(0, 0, 170, 0);
            my_gradient.addColorStop(0, "#04064b")
            my_gradient.addColorStop(1, "#07052a")
            ctx.beginPath();
            ctx.fillStyle = my_gradient;
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            // Paint snake
            for(let i = 0; i < snake.length; i++){
                activeDot(snake[i].x, snake[i].y);
            }
            // Paint food
            activeDot2(food.x, food.y);
            // Debug
            //document.getElementById("debug").innerHTML = snake_dir + " " + snake_next_dir + " " + snake[0].x + " " + snake[0].y;
            // Recursive call after speed delay, déjà vu
            setTimeout(mainLoop, snake_speed);
        }

        /* New Game setup */
        /////////////////////////////////////////////////////////////
        let newGame = function(){
            // snake game screen
            showScreen(SCREEN_SNAKE);
            screen_snake.focus();
            // game score to zero
            score = 0;
            // Reset Canvas Size
            canvas.width = 480;
            canvas.height = 480;
            //Reset Border Color
            const selectedTheme = document.querySelector('input[name="theme"]:checked').value; // Checks what the current theme is
            if (selectedTheme === 'dark') {
                canvas.style.borderColor = "#00FF00";
            } else {
                canvas.style.borderColor = "#00FF00";
            }
            altScore(score);
            // initial snake
            snake = [];
            snake.push({x: 3, y: 15}); // Head (3, 15)
            snake.push({x: 2, y: 15}); // Second Segment (2, 15)
            snake.push({x: 1, y: 15}); // Third segment (1, 15)
            snake.push({x: 0, y: 15}); // Fourth segment (0, 15)
            snake_next_dir = 1;
            // food on canvas
            addFood();
            // activate canvas event
            canvas.onkeydown = function(evt) {
                changeDir(evt.keyCode);
            }
            mainLoop();
        }
        /* Key Inputs and Actions */
        /////////////////////////////////////////////////////////////
        let changeDir = function(key){
            // test key and switch direction
            switch(key) {
                case 65:    // A (Left)
                case 37:
                    if (snake_dir !== 1)    // not right
                        snake_next_dir = 3; // then switch left
                    break;
                case 87:    // W (Up)
                case 38:
                    if (snake_dir !== 2)    // not down
                        snake_next_dir = 0; // then switch up
                    break;
                case 68:    // D (Right)
                case 39:
                    if (snake_dir !== 3)    // not left
                        snake_next_dir = 1; // then switch right
                    break;
                case 83:    // S (Down)
                case 40:
                    if (snake_dir !== 0)    // not up
                        snake_next_dir = 2; // then switch down
                    break;
            }
        }
        /* Dot for Food or Snake part */
        /////////////////////////////////////////////////////////////

        //Color for Snake
        let activeDot = function(x, y){
            if (score >= 20) {
                ctx.fillStyle = "#00FF85";
            }
            else {
                ctx.fillStyle = "#7CFC00";
            }
            ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
        }

        // Fish
        let activeDot2 = function(x, y){
            ctx.fillStyle = "#DC143C";
            ctx.fillText("🐟", x * BLOCK, y * BLOCK + BLOCK);
        }
        /* Random food placement */
        /////////////////////////////////////////////////////////////
        let addFood = function(){
            food.x = Math.floor(Math.random() * ((canvas.width / BLOCK) - 1));
            food.y = Math.floor(Math.random() * ((canvas.height / BLOCK) - 1));
            for(let i = 0; i < snake.length; i++){
                if(checkBlock(food.x, food.y, snake[i].x, snake[i].y)){
                    addFood();
                }
            }
        }
        /* Collision Detection */
        /////////////////////////////////////////////////////////////
        let checkBlock = function(x, y, _x, _y){
            return (x === _x && y === _y);
        }
        /* Update Score */
        /////////////////////////////////////////////////////////////
        let altScore = function(score_val){
            ele_score.innerHTML = String(score_val);
        }
        /////////////////////////////////////////////////////////////
        // Change the snake speed...
        // 100 = slow
        // 80 = normal
        // 40 = fast
        let setSnakeSpeed = function(speed_value){
            snake_speed = speed_value;
        }
        /////////////////////////////////////////////////////////////
        let setWall = function(wall_value){
            wall = wall_value;
            if(wall === 0){screen_snake.style.borderColor = "#606060";}
            if(wall === 1){screen_snake.style.borderColor = "#B2BEB5";}
        }
    })();
</script>