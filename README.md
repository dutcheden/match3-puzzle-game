<!DOCTYPE html>
<html>
<head>
    <title>Match-3 Puzzle Game</title>
    <style>
        canvas {
            border: 2px solid black;
            display: block;
            margin: auto;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");
        const GRID_SIZE = 6;
        const TILE_SIZE = canvas.width / GRID_SIZE;
        const COLORS = ["red", "blue", "green", "yellow", "purple"];
        let grid = [];
        let selectedTile = null;
 function initGrid() {
            for (let x = 0; x < GRID_SIZE; x++) {
                grid[x] = [];
                for (let y = 0; y < GRID_SIZE; y++) {
                    grid[x][y] = Math.floor(Math.random() * COLORS.length);
                }
            }
        }
 function drawGrid() {
            for (let x = 0; x < GRID_SIZE; x++) {
                for (let y = 0; y < GRID_SIZE; y++) {
                    ctx.fillStyle = COLORS[grid[x][y]];
                    ctx.fillRect(x * TILE_SIZE, y * TILE_SIZE, TILE_SIZE - 2, TILE_SIZE - 2);
                }
            }
        }
 function getTileAtPosition(x, y) {
            return { x: Math.floor(x / TILE_SIZE), y: Math.floor(y / TILE_SIZE) };
        }
 function swapTiles(pos1, pos2) {
            let temp = grid[pos1.x][pos1.y];
            grid[pos1.x][pos1.y] = grid[pos2.x][pos2.y];
            grid[pos2.x][pos2.y] = temp;
        }
  function checkMatches() {
            for (let x = 0; x < GRID_SIZE; x++) {
                for (let y = 0; y < GRID_SIZE - 2; y++) {
                    if (grid[x][y] === grid[x][y + 1] && grid[x][y] === grid[x][y + 2]) {
                        grid[x][y] = grid[x][y + 1] = grid[x][y + 2] = null;
                    }
                }
            }
      for (let x = 0; x < GRID_SIZE - 2; x++) {
                for (let y = 0; y < GRID_SIZE; y++) {
                    if (grid[x][y] === grid[x + 1][y] && grid[x][y] === grid[x + 2][y]) {
                        grid[x][y] = grid[x + 1][y] = grid[x + 2][y] = null;
                    }
                }
            }
        }
 function applyGravity() {
            for (let x = 0; x < GRID_SIZE; x++) {
                for (let y = GRID_SIZE - 1; y >= 0; y--) {
                    if (grid[x][y] === null) {
                        for (let k = y; k > 0; k--) {
                            grid[x][k] = grid[x][k - 1];
                        }
                        grid[x][0] = Math.floor(Math.random() * COLORS.length);
                    }
                }
            }
        }
   canvas.addEventListener("click", (event) => {
            let mousePos = getTileAtPosition(event.offsetX, event.offsetY);
            if (!selectedTile) {
                selectedTile = mousePos;
            } else {
                swapTiles(selectedTile, mousePos);
                checkMatches();
                applyGravity();
                selectedTile = null;
            }
            drawGrid();
        });
  initGrid();
        drawGrid();
    </script>
</body>
</html>
