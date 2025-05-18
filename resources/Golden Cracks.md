
```
<canvas id="crackCanvas"></canvas>

<script>
const canvas = document.getElementById("crackCanvas");
const ctx = canvas.getContext("2d");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

ctx.lineCap = "round";
ctx.shadowColor = "gold";
ctx.shadowBlur = 6;

class Crack {
  constructor(x, y, angle, depth = 0, maxDepth = 35) {
    this.x = x;
    this.y = y;
    this.angle = angle;
    this.depth = depth;
    this.maxDepth = maxDepth;
    this.done = false;
  }

  update() {
    if (this.depth >= this.maxDepth) {
      this.done = true;
      return;
    }

    const length = 10 + Math.random() * 4;
    const newAngle = this.angle + (Math.random() - 0.5) * 0.1;
    const newX = this.x + Math.cos(newAngle) * length;
    const newY = this.y + Math.sin(newAngle) * length;

    ctx.beginPath();
    ctx.moveTo(this.x, this.y);
    ctx.lineTo(newX, newY);
    ctx.strokeStyle = `hsl(45, 100%, ${70 - this.depth * 1.5}%)`;
    ctx.lineWidth = Math.max(0.5, 2.5 - this.depth * 0.05);
    ctx.stroke();

    this.x = newX;
    this.y = newY;
    this.angle = newAngle;
    this.depth++;

    if (Math.random() < 0.07 && this.depth > 5 && this.depth < this.maxDepth - 5) {
      cracks.push(new Crack(this.x, this.y, this.angle + (Math.random() - 0.5) * 1.2, this.depth, this.maxDepth - 5));
    }
  }
}

// Generate a realistic burst of cracks
function createImpact(x, y, spokes = 12) {
  // Small central burst (glass star shape)
  for (let i = 0; i < spokes; i++) {
    const angle = (Math.PI * 2 / spokes) * i + (Math.random() - 0.5) * 0.1;
    const len = 6 + Math.random() * 10;
    const endX = x + Math.cos(angle) * len;
    const endY = y + Math.sin(angle) * len;

    ctx.beginPath();
    ctx.moveTo(x, y);
    ctx.lineTo(endX, endY);
    ctx.strokeStyle = "gold";
    ctx.lineWidth = 1.5;
    ctx.shadowBlur = 2;
    ctx.stroke();

    // Start longer cracks from end points
    cracks.push(new Crack(endX, endY, angle));
  }
}

// Store cracks
const cracks = [];

// Make 1-2 realistic impact points
const originCount = 1;
for (let i = 0; i < originCount; i++) {
  const x = Math.random() * canvas.width;
  const y = Math.random() * canvas.height;
  createImpact(x, y, 10 + Math.floor(Math.random() * 4));
}

// Animate cracks
function animate() {
  requestAnimationFrame(animate);
  cracks.forEach(c => {
    if (!c.done) c.update();
  });
}

// Resize support
window.addEventListener("resize", () => {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
});

// Start animation
animate();
</script>
```

start on click

```
<canvas id="crackCanvas"></canvas>

<script>
const canvas = document.getElementById("crackCanvas");
const ctx = canvas.getContext("2d");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

ctx.lineCap = "round";
ctx.shadowColor = "gold";
ctx.shadowBlur = 6;

const cracks = [];

class Crack {
  constructor(x, y, angle, depth = 0, maxDepth = 35) {
    this.x = x;
    this.y = y;
    this.angle = angle;
    this.depth = depth;
    this.maxDepth = maxDepth;
    this.done = false;
  }

  update() {
    if (this.depth >= this.maxDepth) {
      this.done = true;
      return;
    }

    const length = 10 + Math.random() * 4;
    const newAngle = this.angle + (Math.random() - 0.5) * 0.1;
    const newX = this.x + Math.cos(newAngle) * length;
    const newY = this.y + Math.sin(newAngle) * length;

    ctx.beginPath();
    ctx.moveTo(this.x, this.y);
    ctx.lineTo(newX, newY);
    ctx.strokeStyle = `hsl(45, 100%, ${70 - this.depth * 1.5}%)`;
    ctx.lineWidth = Math.max(0.5, 2.5 - this.depth * 0.05);
    ctx.stroke();

    this.x = newX;
    this.y = newY;
    this.angle = newAngle;
    this.depth++;

    if (Math.random() < 0.07 && this.depth > 5 && this.depth < this.maxDepth - 5) {
      cracks.push(new Crack(this.x, this.y, this.angle + (Math.random() - 0.5) * 1.2, this.depth, this.maxDepth - 5));
    }
  }
}

// Create a starburst + launch cracks from its ends
function createImpact(x, y, spokes = 12) {
  for (let i = 0; i < spokes; i++) {
    const angle = (Math.PI * 2 / spokes) * i + (Math.random() - 0.5) * 0.1;
    const len = 8 + Math.random() * 10;
    const endX = x + Math.cos(angle) * len;
    const endY = y + Math.sin(angle) * len;

    ctx.beginPath();
    ctx.moveTo(x, y);
    ctx.lineTo(endX, endY);
    ctx.strokeStyle = "gold";
    ctx.lineWidth = 1.5;
    ctx.shadowBlur = 3;
    ctx.stroke();

    cracks.push(new Crack(endX, endY, angle));
  }
}

// Handle click to trigger cracks
canvas.addEventListener("click", (e) => {
  const rect = canvas.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  createImpact(x, y, 10 + Math.floor(Math.random() * 4));
});

// Animate
function animate() {
  requestAnimationFrame(animate);
  cracks.forEach(c => {
    if (!c.done) c.update();
  });
}

// Resize support
window.addEventListener("resize", () => {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
});

// Initial animation start
animate();
</script>3
```
