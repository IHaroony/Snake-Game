// Select the canvas element and set up the context
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

// Loading and setting up background images
const images = [
    'image1.jpg', 'image2.jpg', 'image3.jpg', 'image4.jpg', 'image5.jpg', 
    'image23.jpg', 'image24.jpg', 'image25.jpg', 'image26.jpg', 'image27.jpg',
    'image29.jpg', 'image29.webp', 'image30.jpg', 'image32.jpg', 'image33.jpg', 
    'image34.jpg', 'image35.jpg', 'image36.jpg', 'image37.jpg', 'image38.jpg',
    'image6.jpg', 'image7.jpg', 'image8.jpg', 'image9.jpg', 'image10.jpg',
    'image17.jpg', 'image19.jpg', 'image20.jpg', 'image21.jpg', 'image22.jpg',
    'i9.jpg', 'i10.jpg', 'i11.jpg', 'i12.jpg', 'i13.jpg', 'i14.jpg', 
    'i15.jpg', 'i16.jpg', 'i17.jpg', 'i18.jpg',
    'image17.jpg', 'image19.jpg', 'image20.jpg', 'image21.jpg', 'image22.jpg',
    'i9.jpg', 'i10.jpg', 'i11.jpg', 'i12.jpg', 'i13.jpg', 'i14.jpg', 
    'i15.jpg', 'i16.jpg', 'i17.jpg', 'i18.jpg'
];

// Creating and positioning images
const movingImages = [];

images.forEach(src => {
    const img = new Image();
    img.src = src;
    img.classList.add('moving-image');
    
    // Set random initial position
    img.style.position = 'absolute';
    img.style.left = `${Math.random() * (window.innerWidth - 100)}px`;
    img.style.top = `${Math.random() * (window.innerHeight - 100)}px`;
    
    document.body.appendChild(img);
    movingImages.push(img);
});

// Animation properties
const imageSpeeds = movingImages.map(() => ({
    x: Math.random() * 3 - 2,
    y: Math.random() * 3 - 2,
}));

function animateImages() {
    movingImages.forEach((img, index) => {
        const speed = imageSpeeds[index];
        let rect = img.getBoundingClientRect();
        img.style.left = `${rect.left + speed.x}px`;
        img.style.top = `${rect.top + speed.y}px`;

        // Boundary checks to keep images within the viewport
        if (rect.left + speed.x < 0 || rect.right + speed.x > window.innerWidth) {
            speed.x *= -1;
        }
        if (rect.top + speed.y < 0 || rect.bottom + speed.y > window.innerHeight) {
            speed.y *= -1;
        }
    });

    requestAnimationFrame(animateImages);
}

// Call this function once when the DOM is fully loaded
document.addEventListener('DOMContentLoaded', animateImages);

// Adding sound to the game
const eatSound = document.getElementById('eatSound');
const gameOverSound = document.getElementById('gameOverSound');
const backgroundMusic = document.getElementById('backgroundMusic');

// Adjust the speed of the audio
eatSound.playbackRate = 2;
gameOverSound.playbackRate = 1;

// Play background music and set it to loop
backgroundMusic.volume = 0.5;  
backgroundMusic.loop = true;  
backgroundMusic.play();

// Load the background image
const backgroundImage = new Image();
backgroundImage.src = 'background-image.jpg';  

// Define the size of the grid and the initial game state
const gridSize = 20;
let snake = [{ x: 100, y: 100 }]; // Starting position of the snake
let direction = { x: 0, y: 0 };
let food = { x: Math.floor(Math.random() * canvas.width / gridSize) * gridSize, y: Math.floor(Math.random() * canvas.height / gridSize) * gridSize };
let score = 0;

// Draw the snake and food on the canvas
function draw() {
    // Clear the canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Draw the background image first
    ctx.drawImage(backgroundImage, 0, 0, canvas.width, canvas.height);

    // Draw the snake with a border
    ctx.fillStyle = "white";
    snake.forEach((segment, index) => {
        ctx.fillRect(segment.x, segment.y, gridSize, gridSize);
        if (index === 0) {
            // Draw the head with a border
            ctx.strokeStyle = "green"; // Border color
            ctx.lineWidth = 2;
            ctx.strokeRect(segment.x, segment.y, gridSize, gridSize);
        }
    });

    // Draw the food with a shadow and a circle shape
    ctx.fillStyle = "#90EE90";
    ctx.shadowColor = "rgba(10,3,10,50.3)";
    ctx.shadowBlur = 10;
    ctx.beginPath();
    ctx.arc(food.x + gridSize / 2, food.y + gridSize / 2, gridSize / 2, 0, Math.PI * 2);
    ctx.fill();
    ctx.shadowBlur = 0; // Reset shadow for other drawings

    // Move the snake
    let newHead = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

    // Check if the snake eats the food
    if (newHead.x === food.x && newHead.y === food.y) {
        score++;
        eatSound.currentTime = 0;
        eatSound.play(); 
        food = { x: Math.floor(Math.random() * canvas.width / gridSize) * gridSize, y: Math.floor(Math.random() * canvas.height / gridSize) * gridSize };
    } else {
        snake.pop();
    }

    // Check if the snake collides with itself or the walls
    if (newHead.x < 0 || newHead.x >= canvas.width || newHead.y < 0 || newHead.y >= canvas.height || snake.some(segment => segment.x === newHead.x && segment.y === newHead.y)) {
        gameOverSound.play();
        alert("Try Again! Your current score: " + score);
        snake = [{ x: 100, y: 100 }];
        direction = { x: 0, y: 0 };
        score = 0;
        return;
    }

    snake.unshift(newHead);

    // Draw the score on the canvas
    ctx.fillStyle = "white";
    ctx.font = "20px fantasy";
    ctx.fillText("Score: " + score, 10, 20);
}

// Handle keyboard input to change the snake's direction
document.addEventListener("keydown", event => {
    switch (event.key) {
        case "ArrowUp":
            if (direction.y === 0) direction = { x: 0, y: -gridSize };
            break;
        case "ArrowDown":
            if (direction.y === 0) direction = { x: 0, y: gridSize };
            break;
        case "ArrowLeft":
            if (direction.x === 0) direction = { x: -gridSize, y: 0 };
            break;
        case "ArrowRight":
            if (direction.x === 0) direction = { x: gridSize, y: 0 };
            break;
    }
});

// Ensure all assets are loaded before starting the game
backgroundImage.onload = () => {
    backgroundMusic.play();  // Play background music when the game starts
    setInterval(draw, 70);   // Start the game loop
};
