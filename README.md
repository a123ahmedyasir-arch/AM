// A starter prototype for a Minecraft‑like game using JavaScript + HTML Canvas.
// This is NOT a full Minecraft clone, but a foundation you can expand.

// === index.html (place in same project) ===
// <canvas id="game" width="800" height="600"></canvas>
// <script src="game.js"></script>

// === game.js ===

const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

// --- Basic World Settings ---
const TILE_SIZE = 32;
const WORLD_WIDTH = 100;
const WORLD_HEIGHT = 50;

// Generate simple terrain
const world = [];
for (let y = 0; y < WORLD_HEIGHT; y++) {
  const row = [];
  for (let x = 0; x < WORLD_WIDTH; x++) {
    if (y > WORLD_HEIGHT / 2) row.push("dirt");
    else row.push("air");
  }
  world.push(row);
}

// --- Player ---
const player = {
  x: 10,
  y: 10,
  mode: "survival", // survival | creative
};

// --- Input ---
const keys = {};
window.addEventListener("keydown", (e) => (keys[e.key] = true));
window.addEventListener("keyup", (e) => (keys[e.key] = false));

// --- Camera ---
function getCamera() {
  return {
    x: player.x * TILE_SIZE - canvas.width / 2,
    y: player.y * TILE_SIZE - canvas.height / 2,
  };
}

// --- Main Loop ---
function update() {
  if (keys["w"]) player.y -= 0.1;
  if (keys["s"]) player.y += 0.1;
  if (keys["a"]) player.x -= 0.1;
  if (keys["d"]) player.x += 0.1;
}

function draw() {
  ctx.fillStyle = "#87CEEB"; // sky
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  const cam = getCamera();

  for (let y = 0; y < WORLD_HEIGHT; y++) {
    for (let x = 0; x < WORLD_WIDTH; x++) {
      const tile = world[y][x];
      if (tile === "air") continue;

      ctx.fillStyle = tile === "dirt" ? "#8B4513" : "gray";
      ctx.fillRect(
        x * TILE_SIZE - cam.x,
        y * TILE_SIZE - cam.y,
        TILE_SIZE,
        TILE_SIZE
      );
    }
  }

  // Player
  ctx.fillStyle = "red";
  ctx.fillRect(
    player.x * TILE_SIZE - cam.x,
    player.y * TILE_SIZE - cam.y,
    TILE_SIZE,
    TILE_SIZE
  );
}

function gameLoop() {
  update();
  draw();
  requestAnimationFrame(gameLoop);
}

gameLoop();
