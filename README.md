<!DOCTYPE html>
<html>
<head>
    <title>贪吃蛇</title>
    <style>
        canvas {
            border: 2px solid #333;
            background: #f0f0f0;
        }
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            background: #2c3e50;
            color: white;
        }
    </style>
</head>
<body>
    <h1>贪吃蛇</h1>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <p>使用方向键控制移动</p >

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const gridSize = 20;
        const tileCount = canvas.width / gridSize;
        
        let snake = [
            { x: 10, y: 10 }
        ];
        let food = { x: 15, y: 15 };
        let dx = 0;
        let dy = 0;

        document.addEventListener('keydown', changeDirection);

        function changeDirection(event) {
            const LEFT_KEY = 37;
            const RIGHT_KEY = 39;
            const UP_KEY = 38;
            const DOWN_KEY = 40;

            const keyPressed = event.keyCode;
            const goingUp = dy === -1;
            const goingDown = dy === 1;
            const goingRight = dx === 1;
            const goingLeft = dx === -1;

            if (keyPressed === LEFT_KEY && !goingRight) {
                dx = -1;
                dy = 0;
            }
            if (keyPressed === UP_KEY && !goingDown) {
                dx = 0;
                dy = -1;
            }
            if (keyPressed === RIGHT_KEY && !goingLeft) {
                dx = 1;
                dy = 0;
            }
            if (keyPressed === DOWN_KEY && !goingUp) {
                dx = 0;
                dy = 1;
            }
        }

        function drawGame() {
            // 移动蛇
            const head = { x: snake[0].x + dx, y: snake[0].y + dy };
            snake.unshift(head);

            // 检测吃食物
            if (head.x === food.x && head.y === food.y) {
                food.x = Math.floor(Math.random() * tileCount);
                food.y = Math.floor(Math.random() * tileCount);
            } else {
                snake.pop();
            }

            // 清空画布
            ctx.fillStyle = '#2c3e50';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            // 绘制蛇
            ctx.fillStyle = '#2ecc71';
            snake.forEach((segment, index) => {
                ctx.fillRect(segment.x * gridSize, segment.y * gridSize, gridSize-2, gridSize-2);
            });

            // 绘制食物
            ctx.fillStyle = '#e74c3c';
            ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize-2, gridSize-2);
        }

        setInterval(drawGame, 100);
    </script>
</body>
</html>
