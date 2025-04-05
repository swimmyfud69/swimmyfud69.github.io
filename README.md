
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Floating RGB Logo with Music</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @keyframes float {
            0% {
                transform: translateY(0px) rotate(0deg);
            }
            50% {
                transform: translateY(-20px) rotate(5deg);
            }
            100% {
                transform: translateY(0px) rotate(0deg);
            }
        }
        
        @keyframes rgb {
            0% {
                color: #ff00ff;
                text-shadow: 0 0 10px #ff00ff;
            }
            25% {
                color: #ff69b4;
                text-shadow: 0 0 15px #ff69b4;
            }
            50% {
                color: #ffffff;
                text-shadow: 0 0 20px #ffffff;
            }
            75% {
                color: #ff1493;
                text-shadow: 0 0 15px #ff1493;
            }
            100% {
                color: #ff00ff;
                text-shadow: 0 0 10px #ff00ff;
            }
        }
        
        .floating-logo {
            animation: float 6s ease-in-out infinite;
        }
        
        .rgb-text {
            animation: rgb 4s linear infinite;
        }
        
        .discord-text {
            animation: rgb 5s linear infinite;
            animation-delay: 1s;
        }
        
        body {
            overflow: hidden;
            background-color: #000;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            width: 100vw;
            flex-direction: column;
        }
        
        .music-control {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 100;
            background: rgba(0,0,0,0.7);
            border-radius: 50%;
            padding: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .music-control:hover {
            transform: scale(1.1);
            background: rgba(255,255,255,0.2);
        }
        
        .particle {
            position: absolute;
            background: #fff;
            border-radius: 50%;
            pointer-events: none;
        }
        
        /* Hide the YouTube iframe visually but keep it functional */
        #youtube-player {
            position: absolute;
            width: 0;
            height: 0;
            opacity: 0;
            pointer-events: none;
        }
    </style>
</head>
<body>
    <div class="floating-logo">
        <h1 class="rgb-text text-center text-9xl font-bold tracking-wider">
            <span class="block">SWIM</span>
        </h1>
        <p class="discord-text text-center text-xl mt-4 font-mono">
            html dev dm swimisback. on discord!
        </p>
    </div>

    <!-- YouTube Player -->
    <div id="youtube-player"></div>
    
    <!-- Music control button -->
    <div class="music-control" id="music-toggle">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M9 18V5l12-2v13"></path>
            <circle cx="6" cy="18" r="3"></circle>
            <circle cx="18" cy="16" r="3"></circle>
        </svg>
    </div>

    <!-- Load YouTube API -->
    <script>
        // Load the YouTube API
        var tag = document.createElement('script');
        tag.src = "https://www.youtube.com/iframe_api";
        var firstScriptTag = document.getElementsByTagName('script')[0];
        firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

        var player;
        var isPlaying = false;

        function onYouTubeIframeAPIReady() {
            player = new YT.Player('youtube-player', {
                height: '0',
                width: '0',
                videoId: 'tnD4E4Epg3A',
                playerVars: {
                    'autoplay': 1,
                    'controls': 0,
                    'disablekb': 1,
                    'fs': 0,
                    'loop': 1,
                    'modestbranding': 1,
                    'playsinline': 1,
                    'rel': 0
                },
                events: {
                    'onReady': onPlayerReady,
                    'onStateChange': onPlayerStateChange
                }
            });
        }

        function onPlayerReady(event) {
            event.target.setVolume(50);
            isPlaying = true;
            updateMusicToggle();
        }

        function onPlayerStateChange(event) {
            // When video ends, it will automatically restart due to loop=1 parameter
            if (event.data === YT.PlayerState.ENDED) {
                player.playVideo();
            }
        }

        function togglePlayback() {
            if (isPlaying) {
                player.pauseVideo();
            } else {
                player.playVideo();
            }
            isPlaying = !isPlaying;
            updateMusicToggle();
        }

        function updateMusicToggle() {
            const musicToggle = document.getElementById('music-toggle');
            if (isPlaying) {
                musicToggle.innerHTML = `
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <path d="M9 18V5l12-2v13"></path>
                        <circle cx="6" cy="18" r="3"></circle>
                        <circle cx="18" cy="16" r="3"></circle>
                    </svg>
                `;
            } else {
                musicToggle.innerHTML = `
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <path d="M5 4h4v16H5zM15 4h4v16h-4z"></path>
                    </svg>
                `;
            }
        }

        document.addEventListener('DOMContentLoaded', () => {
            const logo = document.querySelector('.floating-logo');
            const musicToggle = document.getElementById('music-toggle');
            
            function addRandomFloat() {
                const randomX = (Math.random() - 0.5) * 40;
                const randomY = (Math.random() - 0.5) * 40;
                const randomRotate = (Math.random() - 0.5) * 15;
                
                logo.style.transform = `translate(${randomX}px, ${randomY}px) rotate(${randomRotate}deg)`;
                
                setTimeout(addRandomFloat, 3000 + Math.random() * 2000);
            }
            
            addRandomFloat();
            
            // Add mouse follow effect
            document.addEventListener('mousemove', (e) => {
                const x = e.clientX / window.innerWidth;
                const y = e.clientY / window.innerHeight;
                
                logo.style.transform = `translate(${(x - 0.5) * 40}px, ${(y - 0.5) * 40}px)`;
                
                // Create particles on mouse move
                createParticle(e.clientX, e.clientY);
            });
            
            // Create floating particles
            function createParticle(x, y) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                
                const size = Math.random() * 5 + 2;
                const duration = Math.random() * 3 + 2;
                
                particle.style.width = `${size}px`;
                particle.style.height = `${size}px`;
                particle.style.left = `${x}px`;
                particle.style.top = `${y}px`;
                particle.style.opacity = '0.7';
                particle.style.animation = `float ${duration}s ease-in-out infinite`;
                
                // Random RGB color
                const hue = Math.floor(Math.random() * 360);
                particle.style.backgroundColor = `hsl(${hue}, 100%, 70%)`;
                
                document.body.appendChild(particle);
                
                // Remove particle after animation completes
                setTimeout(() => {
                    particle.remove();
                }, duration * 1000);
            }
            
            // Music control
            musicToggle.addEventListener('click', togglePlayback);
            
            // Add keyboard shortcut for music control (Space)
            document.addEventListener('keydown', (e) => {
                if (e.code === 'Space') {
                    e.preventDefault();
                    togglePlayback();
                }
            });
        });
    </script>
</body>
</html>
