<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bouncing Text Fun</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            background-color: #121212;
            font-family: 'Arial Black', sans-serif;
        }
        .bouncing-text {
            position: absolute;
            white-space: nowrap;
            font-size: 24px;
            font-weight: bold;
            user-select: none;
            cursor: default;
        }
    </style>
</head>
<body>

<script>
    const textContent = "FUCK YA·LL 😁😁";
    const count = 30; // Number of bouncing elements
    const elements = [];

    function getRandomColor() {
        const letters = '0123456789ABCDEF';
        let color = '#';
        for (let i = 0; i < 6; i++) {
            color += letters[Math.floor(Math.random() * 16)];
        }
        return color;
    }

    for (let i = 0; i < count; i++) {
        const el = document.createElement('div');
        el.className = 'bouncing-text';
        el.textContent = textContent;
        el.style.color = getRandomColor();
        document.body.appendChild(el);

        const rect = el.getBoundingClientRect();
        
        elements.push({
            el: el,
            x: Math.random() * (window.innerWidth - rect.width),
            y: Math.random() * (window.innerHeight - rect.height),
            dx: (Math.random() - 0.5) * 10,
            dy: (Math.random() - 0.5) * 10,
            width: rect.width,
            height: rect.height
        });
    }

    function animate() {
        elements.forEach(obj => {
            obj.x += obj.dx;
            obj.y += obj.dy;

            // Bounce off left/right walls
            if (obj.x <= 0 || obj.x + obj.width >= window.innerWidth) {
                obj.dx *= -1;
                obj.el.style.color = getRandomColor();
            }

            // Bounce off top/bottom walls
            if (obj.y <= 0 || obj.y + obj.height >= window.innerHeight) {
                obj.dy *= -1;
                obj.el.style.color = getRandomColor();
            }

            obj.el.style.transform = `translate(${obj.x}px, ${obj.y}px)`;
        });

        requestAnimationFrame(animate);
    }

    // Handle window resize
    window.addEventListener('resize', () => {
        elements.forEach(obj => {
            const rect = obj.el.getBoundingClientRect();
            obj.width = rect.width;
            obj.height = rect.height;
        });
    });

    animate();
</script>

</body>
</html>
