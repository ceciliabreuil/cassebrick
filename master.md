# cassebrick

<!DOCTYPE html>
<!-- https://developer.mozilla.org/fr/docs/Games/Workflows/2D_Breakout_game_pure_JavaScript/creer_element_canvas_et_afficher -->

<html>
    <head>
        <meta charset="utf-8" />
        <title>Casse-Brique</title>
        <style>
            * {padding: 0; margin: 0; }
            canvas { background: #eee; display: block; margin: 0 auto;}
        </style>
    </head>

    <body>

        <canvas id="myCanvas" width="480" height="320"></canvas>

        
        <script>           
            
var canvas = document.getElementById("myCanvas");       // dessin canvas 
var ctx = canvas.getContext("2d");      // contexte de rendu 2D

        // dessin balle
var ballRadius = 10;
var x = canvas.width/2;
var y = canvas.height-30;
var dx = 2;
var dy = -2;

        // dessin raquette
var paddleHeight = 10;
var paddleWidth = 75;
var paddleX = (canvas.width-paddleWidth)/2; // point de départ sur x
var rightPressed = false;
var leftPressed = false;

        // pour appel de la methode qui appelle draw() toutes ls 10millisecondes
var interval = setInterval(draw, 10);

        // déclancheur d'evenement (touches gauche-droite)
document.addEventListener("keydown", keyDownHandler, false);
document.addEventListener("keyup", keyUpHandler, false);

        // fonction pour lier les touches préssées aux déplacement de la raquette
function keyDownHandler(e) {                            // fonction touche pressée
    if(e.key == "Right" || e.key == "ArrowRight") {     // touche droite
        rightPressed = true;                            // var passe à true
    }
    else if(e.key == "Left" || e.key == "ArrowLeft") {      // etc
        leftPressed = true;
    }
}

function keyUpHandler(e) {                              // fonction touche relachée
    if(e.key == "Right" || e.key == "ArrowRight") {
        rightPressed = false;
    }
    else if(e.key == "Left" || e.key == "ArrowLeft") {
        leftPressed = false;
    }
}

function drawBall() {
    ctx.beginPath();
    ctx.arc(x, y, ballRadius, 0, Math.PI*2);
    ctx.fillStyle = "rgba(0, 70, 100, 1)";
    ctx.fill();
    ctx.closePath();
}
function drawPaddle() {
    ctx.beginPath();
    ctx.rect(paddleX, canvas.height-paddleHeight, paddleWidth, paddleHeight);
    ctx.fillStyle = "#0095DD";
    ctx.fill();
    ctx.closePath();
}

function draw() {
        // effacement du canvas à chaque tour de boucle > crée le mvt
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    drawBall();     // appel des fonctions qui dessinnent la balle et la raquette
    drawPaddle();   
    
        // detecteur de collition de la balle > mur gauche et droite
    if(x + dx > canvas.width-ballRadius || x + dx < ballRadius) {
        dx = -dx;
    }
        // detecteur de collision 
    if(y + dy  < ballRadius) {
        dy = -dy;
        // effacement mur du bas > si la balle touche, alerte + rechargement page
    } else if(y + dy > canvas.height-ballRadius) {
        alert("GAME OVER");
        document.location.reload();
        clearInternal(interval);    // necessaire pour Chrome pour finir le jeu
    }
    
        // verification > si les touches gauche et droite sont préssées ou non
            // + code pour que la raquette ne sorte pas du canvas
    if(rightPressed && paddleX < canvas.width-paddleWidth) {
        paddleX += 7;
    }
    else if(leftPressed && paddleX > 0) {
        paddleX -= 7;
    }
    
    x += dx;
    y += dy;
}



</script>
