<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="mobile-web-app-capable" content="yes">
    <title>Reproductor Pantalla Completa Automática</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        html, body {
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            background: #000;
        }
        video {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            object-fit: contain; /* Evita recortes en cualquier pantalla */
            background: #000;
        }
    </style>
</head>
<body>
    <video id="auto-player" autoplay playsinline webkit-playsinline>
        <source src="https://ia600809.us.archive.org/11/items/el-rey-leon-parte-1-1/El%20rey%20le%C3%B3n%20-%20Parte%201%20-%201.ia.mp4" type="video/mp4">
        <source src="https://cdn.plyr.io/static/demo/View_From_A_Blue_Moon_Trailer-720p.mp4" type="video/mp4">
        Tu dispositivo no soporta el vídeo.
    </video>

    <script>
        // Forzar pantalla completa al cargar y asegurar reproducción continua
        const player = document.getElementById('auto-player');
        
        // Forzar pantalla completa inmediata
        function goFullscreen() {
            if (!document.fullscreenElement) {
                document.documentElement.requestFullscreen().catch(err => {
                    // Si no se puede por política, ajustar para que ocupe toda la pantalla visualmente
                    player.style.width = "100vw";
                    player.style.height = "100vh";
                });
            }
        }

        // Reproducción automática y pantalla completa al iniciar
        window.onload = () => {
            player.play().then(goFullscreen).catch(() => {
                // Si el navegador bloquea la reproducción automática, activar con toque/clic
                player.addEventListener('click', () => {
                    player.play();
                    goFullscreen();
                }, { once: true });
            });
        };

        // Pasar a siguiente película cuando termine
        player.addEventListener('ended', () => {
            const sources = player.querySelectorAll('source');
            let current = Array.from(sources).findIndex(s => s.src === player.currentSrc);
            current = (current + 1) % sources.length;
            player.src = sources[current].src;
            player.load();
            player.play();
        });
    </script>
</body>
</html>
