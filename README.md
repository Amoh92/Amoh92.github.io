<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bouncing Text Fun - Auto Music</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-color: #121212;
            font-family: 'Arial Black', sans-serif;
            cursor: pointer; /* Suggest interaction */
        }
        .bouncing-text {
            position: absolute;
            white-space: nowrap;
            font-size: 24px;
            font-weight: bold;
            user-select: none;
        }
    </style>
</head>
<body onclick="startAudio()">

<audio id="bg-audio" loop preload="auto">
    <source src="background.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
</audio>

<script>
    const textContent = "FUCK YA·LL 😁😁";
    const count = 40; // More elements for more fun
    const elements = [];
    const audio = document.getElementById('bg-audio');
    let hasStarted = false;

    // Function to start audio and animation
    function startAudio() {
        if (!hasStarted) {
            audio.play().catch(e => {
                console.log("Autoplay blocked. Waiting for user interaction.");
            });
            hasStarted = true;
        }
    }

    // Attempt autoplay on load
    window.addEventListener('load', () => {
        startAudio();
        // Also try to play on any mouse move or touch as a fallback
        window.addEventListener('mousemove', startAudio, { once: true });
        window.addEventListener('touchstart', startAudio, { once: true });
    });

    function getRandomColor() {
        const letters = '0123456789ABCDEF';
        let color = '#';
        for (let i = 0; i < 6; i++) {
            color += letters[Math.floor(Math.random() * 16)];
        }
        return color;
    }

    // Create elements
    for (let i = 0; i < count; i++) {
        const el = document.createElement('div');
        el.className = 'bouncing-text';
        el.textContent = textContent;
        el.style.color = getRandomColor();
        document.body.appendChild(el);

        const rect = el.getBoundingClientRect();
        
        elements.push({
            el: el,
            x: Math.random() * (window.innerWidth - 200),
            y: Math.random() * (window.innerHeight - 50),
            dx: (Math.random() - 0.5) * 12,
            dy: (Math.random() - 0.5) * 12,
            width: rect.width || 180, // Fallback width
            height: rect.height || 30 // Fallback height
        });
    }

    function animate() {
        elements.forEach(obj => {
            obj.x += obj.dx;
            obj.y += obj.dy;

            // Bounce off walls
            if (obj.x <= 0 || obj.x + obj.width >= window.innerWidth) {
                obj.dx *= -1;
                obj.el.style.color = getRandomColor();
            }
            if (obj.y <= 0 || obj.y + obj.height >= window.innerHeight) {
                obj.dy *= -1;
                obj.el.style.color = getRandomColor();
            }

            obj.el.style.transform = `translate(${obj.x}px, ${obj.y}px)`;
        });

        requestAnimationFrame(animate);
    }

    // Initialize dimensions after a short delay to ensure rendering
    setTimeout(() => {
        elements.forEach(obj => {
            const rect = obj.el.getBoundingClientRect();
            obj.width = rect.width;
            obj.height = rect.height;
        });
        animate();
    }, 100);

    // Handle window resize
    window.addEventListener('resize', () => {
        elements.forEach(obj => {
            const rect = obj.el.getBoundingClientRect();
            obj.width = rect.width;
            obj.height = rect.height;
        });
    });
</script>

</body>
</html>
