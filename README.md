<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Para Mi Niña — Felices 9 Meses</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Tipografías románticas y elegantes -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@600;700&family=Lora:ital,wght@0,400;0,500;1,400&family=Playfair+Display:ital,wght@0,600;1,600&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --env-back: #9f1239;       /* Rosa vino profundo */
            --env-front: #be123c;      /* Carmesí cálido */
            --env-flap: #e11d48;       /* Solapa superior */
            --env-side: #be123c;       /* Pliegues laterales */
            --env-bottom: #881337;     /* Pliegue inferior */
            --paper-bg: #fffdf9;       /* Marfil papel artesanal */
            --ink-color: #291d1e;      /* Tinta oscura cálida */
            --accent-gold: #d97706;    /* Dorado sutil */
        }

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            margin: 0;
            padding: 0;
            min-height: 100vh;
            min-height: 100dvh;
            background: radial-gradient(circle at 50% 35%, #fff1f2 0%, #ffe4e6 45%, #fecdd3 100%);
            font-family: 'Lora', serif;
            color: var(--ink-color);
            overflow: hidden;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            user-select: none;
        }

        /* Textura sutil de grano en el fondo */
        body::before {
            content: "";
            position: fixed;
            inset: 0;
            background-image: radial-gradient(rgba(159, 18, 57, 0.04) 1px, transparent 0);
            background-size: 24px 24px;
            pointer-events: none;
            z-index: 0;
        }

        /* Canvas de partículas y confeti */
        #fx-canvas {
            position: fixed;
            inset: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 50;
        }

        /* Contenedor principal de la escena 3D */
        .stage {
            position: relative;
            width: 100%;
            height: 100dvh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            perspective: 1400px;
            z-index: 10;
            padding: 1rem;
        }

        /* Título superior de instrucción */
        .hint-banner {
            position: absolute;
            top: clamp(1.5rem, 6vh, 3.5rem);
            text-align: center;
            transition: opacity 0.6s ease, transform 0.6s ease;
            z-index: 20;
            pointer-events: none;
        }

        .hint-banner.hidden-hint {
            opacity: 0;
            transform: translateY(-15px);
        }

        .font-script {
            font-family: 'Dancing Script', cursive;
        }

        .font-display {
            font-family: 'Playfair Display', serif;
        }

        /* Contenedor del sobre */
        .envelope-wrapper {
            position: relative;
            width: min(88vw, 390px);
            height: min(58vw, 255px);
            transform-style: preserve-3d;
            transition: transform 0.9s cubic-bezier(0.22, 1, 0.36, 1), opacity 0.8s ease;
            cursor: pointer;
        }

        /* Animación flotante suave mientras está cerrado */
        .envelope-wrapper:not(.opened):not(.opening) {
            animation: floatEnvelope 3.8s ease-in-out infinite;
        }

        @keyframes floatEnvelope {
            0%, 100% { transform: translateY(0px) rotateX(4deg) rotateY(-2deg); }
            50% { transform: translateY(-10px) rotateX(7deg) rotateY(2deg); }
        }

        /* Cuando se abre, el sobre desciende ligeramente para dar protagonismo a la carta */
        .envelope-wrapper.opened {
            transform: translateY(34vh) scale(0.88);
            cursor: default;
        }

        /* Base trasera del sobre */
        .envelope-back {
            position: absolute;
            inset: 0;
            background: var(--env-back);
            border-radius: 12px;
            box-shadow: 0 25px 50px -12px rgba(136, 19, 55, 0.38),
                        0 8px 20px -6px rgba(136, 19, 55, 0.25);
            z-index: 1;
        }

        /* Interior oscuro con patrón sutil */
        .envelope-lining {
            position: absolute;
            inset: 4px;
            background: repeating-linear-gradient(
                45deg,
                #881337,
                #881337 10px,
                #9f1239 10px,
                #9f1239 20px
            );
            border-radius: 9px;
            z-index: 2;
        }

        /* Pliegues frontales del bolsillo del sobre */
        .envelope-pocket {
            position: absolute;
            inset: 0;
            z-index: 15;
            pointer-events: none;
            border-radius: 12px;
            overflow: hidden;
        }

        /* Triángulo izquierdo */
        .pocket-left {
            position: absolute;
            inset: 0;
            background: linear-gradient(135deg, #be123c 0%, #9f1239 100%);
            clip-path: polygon(0 0, 53% 52%, 0 100%);
            filter: drop-shadow(2px 0 4px rgba(0,0,0,0.18));
        }

        /* Triángulo derecho */
        .pocket-right {
            position: absolute;
            inset: 0;
            background: linear-gradient(225deg, #be123c 0%, #9f1239 100%);
            clip-path: polygon(100% 0, 47% 52%, 100% 100%);
            filter: drop-shadow(-2px 0 4px rgba(0,0,0,0.18));
        }

        /* Triángulo inferior */
        .pocket-bottom {
            position: absolute;
            inset: 0;
            background: linear-gradient(0deg, #881337 0%, #be123c 85%);
            clip-path: polygon(0 100%, 50% 46%, 100% 100%);
            filter: drop-shadow(0 -3px 6px rgba(0,0,0,0.22));
        }

        /* Solapa superior móvil (3D) */
        .envelope-flap {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 58%;
            background: linear-gradient(180deg, #e11d48 0%, #be123c 100%);
            clip-path: polygon(0 0, 100% 0, 50% 100%);
            transform-origin: top center;
            transition: transform 0.75s cubic-bezier(0.4, 0, 0.2, 1), z-index 0s linear 0.35s;
            z-index: 20;
            border-top-left-radius: 12px;
            border-top-right-radius: 12px;
            filter: drop-shadow(0 6px 8px rgba(0, 0, 0, 0.25));
        }

        .envelope-wrapper.opening .envelope-flap,
        .envelope-wrapper.opened .envelope-flap {
            transform: rotateX(180deg);
            z-index: 3;
        }

        /* Sello de lacre dorado/carmesí con corazón */
        .wax-seal {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -42%);
            width: 66px;
            height: 66px;
            border-radius: 50%;
            background: radial-gradient(circle at 32% 30%, #fbbf24 0%, #d97706 48%, #92400e 100%);
            box-shadow: 
                0 6px 14px rgba(0, 0, 0, 0.35),
                inset 0 2px 4px rgba(255, 255, 255, 0.6),
                inset 0 -3px 6px rgba(120, 53, 15, 0.7),
                0 0 0 4px rgba(217, 119, 6, 0.35);
            z-index: 25;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: transform 0.45s cubic-bezier(0.34, 1.56, 0.64, 1), opacity 0.35s ease;
            cursor: pointer;
        }

        .wax-seal::after {
            content: "";
            position: absolute;
            inset: 6px;
            border-radius: 50%;
            border: 1.5px solid rgba(254, 243, 199, 0.55);
        }

        .envelope-wrapper:not(.opened):not(.opening):hover .wax-seal {
            transform: translate(-50%, -42%) scale(1.1);
        }

        .wax-seal svg {
            width: 30px;
            height: 30px;
            fill: #fef3c7;
            filter: drop-shadow(0 1px 2px rgba(120, 53, 15, 0.6));
            animation: heartbeat 1.8s ease-in-out infinite;
        }

        @keyframes heartbeat {
            0%, 100% { transform: scale(1); }
            14% { transform: scale(1.16); }
            28% { transform: scale(1); }
            42% { transform: scale(1.12); }
            70% { transform: scale(1); }
        }

        .envelope-wrapper.opening .wax-seal,
        .envelope-wrapper.opened .wax-seal {
            transform: translate(-50%, -42%) scale(1.4);
            opacity: 0;
            pointer-events: none;
        }

        /* =========================================================
           HOJA DE PAPEL (Sale disparada y se expande en pantalla)
           ========================================================= */
        .letter-sheet {
            position: fixed;
            top: 50%;
            left: 50%;
            /* Estado inicial: guardado dentro del sobre */
            width: min(82vw, 355px);
            height: min(50vw, 220px);
            transform: translate(-50%, -50%) scale(0.85);
            background-color: var(--paper-bg);
            /* Textura realista de papel artesanal con líneas sutiles */
            background-image: 
                radial-gradient(rgba(217, 119, 6, 0.045) 1px, transparent 0),
                linear-gradient(180deg, rgba(254, 243, 199, 0.25) 0%, rgba(255, 253, 249, 0.9) 15%, rgba(255, 253, 249, 1) 85%, rgba(253, 230, 138, 0.22) 100%);
            background-size: 18px 18px, 100% 100%;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
            opacity: 0;
            pointer-events: none;
            z-index: 8;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            border: 1px solid rgba(217, 119, 6, 0.22);
        }

        /* Fase 1: El papel sale disparado hacia arriba desde dentro del sobre */
        .letter-sheet.shooting {
            opacity: 1;
            z-index: 30;
            animation: shootOutPaper 0.72s cubic-bezier(0.16, 1.28, 0.38, 1) forwards;
        }

        @keyframes shootOutPaper {
            0% {
                opacity: 1;
                transform: translate(-50%, -45%) scale(0.88) rotate(0deg);
            }
            65% {
                opacity: 1;
                transform: translate(-50%, -132%) scale(0.96) rotate(-2.5deg);
            }
            100% {
                opacity: 1;
                transform: translate(-50%, -118%) scale(0.98) rotate(0deg);
            }
        }

        /* Fase 2: La carta se despliega y expande para lectura cómoda */
        .letter-sheet.expanded {
            opacity: 1;
            pointer-events: auto;
            z-index: 40;
            width: min(92vw, 650px);
            height: min(82dvh, 760px);
            transform: translate(-50%, -52%) scale(1) rotate(0deg);
            border-radius: 16px;
            box-shadow: 
                0 25px 60px -12px rgba(136, 19, 55, 0.28),
                0 0 0 1px rgba(217, 119, 6, 0.25),
                inset 0 0 40px rgba(254, 243, 199, 0.45);
            transition: 
                width 0.65s cubic-bezier(0.22, 1, 0.36, 1),
                height 0.65s cubic-bezier(0.22, 1, 0.36, 1),
                transform 0.65s cubic-bezier(0.22, 1, 0.36, 1),
                border-radius 0.5s ease,
                box-shadow 0.5s ease;
        }

        /* Contenido interno de la carta */
        .letter-inner {
            opacity: 0;
            transform: translateY(12px);
            transition: opacity 0.55s ease 0.28s, transform 0.55s ease 0.28s;
            height: 100%;
            display: flex;
            flex-direction: column;
            user-select: text;
        }

        .letter-sheet.expanded .letter-inner {
            opacity: 1;
            transform: translateY(0);
        }

        /* Marco ornamental interior */
        .letter-border-frame {
            margin: 12px;
            padding: clamp(1.25rem, 4vw, 2.5rem);
            border: 1px solid rgba(225, 29, 72, 0.18);
            border-radius: 12px;
            height: calc(100% - 24px);
            overflow-y: auto;
            scroll-behavior: smooth;
            position: relative;
        }

        /* Estilización elegante del scrollbar de la carta */
        .letter-border-frame::-webkit-scrollbar {
            width: 6px;
        }
        .letter-border-frame::-webkit-scrollbar-track {
            background: rgba(254, 226, 226, 0.35);
            border-radius: 999px;
        }
        .letter-border-frame::-webkit-scrollbar-thumb {
            background: rgba(225, 29, 72, 0.35);
            border-radius: 999px;
        }
        .letter-border-frame::-webkit-scrollbar-thumb:hover {
            background: rgba(225, 29, 72, 0.55);
        }

        /* Párrafos de la carta */
        .letter-body p {
            font-size: clamp(0.98rem, 2.4vw, 1.12rem);
            line-height: 1.82;
            color: #332224;
            margin-bottom: 1.15rem;
            text-align: left;
        }

        .letter-body p:first-of-type::first-letter {
            font-family: 'Playfair Display', serif;
            font-size: 2.5rem;
            float: left;
            line-height: 0.9;
            padding-right: 0.5rem;
            padding-top: 0.15rem;
            color: #be123c;
            font-weight: 600;
        }

        /* Botón flotante para volver a guardar la carta */
        .reset-btn {
            position: fixed;
            bottom: clamp(1rem, 3.5vh, 2rem);
            left: 50%;
            transform: translateX(-50%) translateY(30px);
            opacity: 0;
            pointer-events: none;
            z-index: 45;
            transition: opacity 0.5s ease 0.45s, transform 0.5s ease 0.45s, background-color 0.2s ease;
        }

        .reset-btn.visible {
            opacity: 1;
            pointer-events: auto;
            transform: translateX(-50%) translateY(0);
        }
    </style>
</head>
<body>

    <!-- Lienzo para corazones flotantes y explosión de confeti al dispararse la carta -->
    <canvas id="fx-canvas"></canvas>

    <main class="stage">
        <!-- Encabezado de bienvenida -->
        <header id="hintBanner" class="hint-banner px-4">
            <span class="inline-block px-3 py-1 rounded-full bg-rose-100/90 border border-rose-200 text-rose-700 text-xs font-medium tracking-widest uppercase mb-2 shadow-sm">
                9 Meses Juntos
            </span>
            <h1 class="font-script text-3xl sm:text-4xl md:text-5xl text-rose-900 drop-shadow-sm">
                Tengo algo especial para ti...
            </h1>
            <p class="text-rose-700/80 text-sm sm:text-base mt-1.5 italic">
                Toca el sello de lacre para abrir tu carta
            </p>
        </header>

        <!-- Sobre 3D Interactivo -->
        <div id="envelopeWrapper" class="envelope-wrapper" role="button" tabindex="0" aria-label="Abrir carta romántica de 9 meses">
            <!-- Parte trasera -->
            <div class="envelope-back"></div>
            <div class="envelope-lining"></div>

            <!-- Etiqueta decorativa en el frente del sobre -->
            <div class="absolute bottom-4 left-0 right-0 text-center z-[18] pointer-events-none">
                <span class="font-script text-rose-100/90 text-xl sm:text-2xl tracking-wide drop-shadow">
                    Para mi niña ❤️
                </span>
            </div>

            <!-- Pliegues del bolsillo -->
            <div class="envelope-pocket">
                <div class="pocket-left"></div>
                <div class="pocket-right"></div>
                <div class="pocket-bottom"></div>
            </div>

            <!-- Solapa superior que gira en 3D -->
            <div class="envelope-flap"></div>

            <!-- Sello de lacre dorado con corazón -->
            <div id="waxSeal" class="wax-seal" title="Haz clic para abrir">
                <svg viewBox="0 0 24 24" aria-hidden="true">
                    <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/>
                </svg>
            </div>
        </div>

        <!-- Papel de la carta que sale disparado y se despliega -->
        <article id="letterSheet" class="letter-sheet" aria-hidden="true">
            <div class="letter-inner">
                <div id="letterScrollArea" class="letter-border-frame">
                    <!-- Cabecera ornamental de la carta -->
                    <div class="text-center mb-5 pb-3 border-b border-rose-200/70">
                        <div class="inline-flex items-center justify-center gap-2 text-rose-600 mb-1">
                            <span class="h-px w-8 bg-rose-300"></span>
                            <svg class="w-4 h-4 fill-current" viewBox="0 0 24 24">
                                <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/>
                            </svg>
                            <span class="h-px w-8 bg-rose-300"></span>
                        </div>
                        <h2 class="font-script text-3xl sm:text-4xl text-rose-800 font-bold">
                            Felices 9 Meses, Mi Amor
                        </h2>
                    </div>

                    <!-- Cuerpo exacto de la nota -->
                    <div class="letter-body">
                        <p>
                            Mi amor... No sé ni por dónde empezar para explicarte lo feliz que me haces, pero quiero que sepas que estos 9 meses a tu lado han sido los mejores 9 meses de mi vida. Desde que llegaste, muchas cosas cambiaron para bien. Me has regalado momentos, sonrisas, abrazos y recuerdos que voy a guardar siempre en mi corazón. No me arrepentiré nunca de haberlo intentado una vez más, porque aquí estamos siendo felices juntos.
                        </p>
                        <p>
                            Quiero agradecerte por cada instante, por tu cariño, por tu paciencia, por hacerme sonreír incluso en los días difíciles y, sobre todo, por permitirme compartir mi vida contigo. No sabes lo bonito que es tener a alguien como tú a mi lado, alguien que hace que los días sean más especiales con tan solo existir.
                        </p>
                        <p>
                            Sé que no somos perfectos y que quizás en el camino tengamos momentos difíciles, pero quiero que tengas la seguridad de que mi intención siempre será cuidarte, respetarte, apoyarte y seguir eligiéndote cada día. Porque no quiero solamente cumplir meses contigo, quiero construir una historia hermosa a tu lado.
                        </p>
                        <p>
                            Si estos 9 meses han sido los mejores de mi vida, no me imagino lo maravillosos que serán los próximos meses y todos los años que todavía nos esperan. Quiero seguir creando recuerdos contigo, cumplir sueños juntos, reírnos hasta que nos duela la barriga y demostrarte cada día lo mucho que significas para mí.
                        </p>
                        <p>
                            Gracias por llegar a mi vida, por ser tú y por hacerme sentir un amor tan bonito. Nunca dudes de lo especial que eres para mí.
                        </p>
                    </div>

                    <!-- Cierre / Firma -->
                    <div class="mt-6 pt-4 border-t border-rose-200/70 text-center">
                        <p class="font-display italic text-rose-900 text-base sm:text-lg leading-relaxed">
                            “Te amo muchísimo mi niña. Felices 9 meses, Y si pudiera volver a empezar mi historia, te volvería a elegir a ti, una y mil veces.”
                        </p>
                        <div class="mt-3 flex justify-center items-center gap-1.5 text-rose-500">
                            <span>♥</span>
                            <span class="text-xs uppercase tracking-[0.25em] text-rose-400 font-sans">Por siempre tuyo</span>
                            <span>♥</span>
                        </div>
                    </div>
                </div>
            </div>
        </article>

        <!-- Botón para volver a guardar la carta en el sobre -->
        <button id="resetBtn" type="button" class="reset-btn px-5 py-2.5 rounded-full bg-rose-800 hover:bg-rose-900 active:scale-95 text-rose-50 text-sm font-medium shadow-lg shadow-rose-900/25 flex items-center gap-2 border border-rose-600/40 cursor-pointer">
            <svg class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/>
            </svg>
            <span>Guardar carta en el sobre</span>
        </button>
    </main>

    <script>
        /**
         * Lógica de interacción del sobre 3D, animación de disparo del papel,
         * síntesis de sonido suave (Web Audio API) y partículas en Canvas.
         */
        (function () {
            const envelopeWrapper = document.getElementById('envelopeWrapper');
            const letterSheet = document.getElementById('letterSheet');
            const letterScrollArea = document.getElementById('letterScrollArea');
            const hintBanner = document.getElementById('hintBanner');
            const resetBtn = document.getElementById('resetBtn');
            const canvas = document.getElementById('fx-canvas');
            const ctx = canvas.getContext('2d');

            let isOpen = false;
            let isAnimating = false;
            let particles = [];
            let ambientHearts = [];

            // Ajustar tamaño del lienzo de partículas
            function resizeCanvas() {
                canvas.width = window.innerWidth;
                canvas.height = window.innerHeight;
            }
            window.addEventListener('resize', resizeCanvas);
            resizeCanvas();

            // Generar corazones ambientales flotantes sutiles
            function initAmbientHearts() {
                ambientHearts = [];
                const count = Math.min(18, Math.floor(window.innerWidth / 65));
                for (let i = 0; i < count; i++) {
                    ambientHearts.push({
                        x: Math.random() * canvas.width,
                        y: Math.random() * canvas.height,
                        size: 7 + Math.random() * 10,
                        speedY: 0.25 + Math.random() * 0.45,
                        drift: Math.random() * Math.PI * 2,
                        opacity: 0.12 + Math.random() * 0.18,
                        color: ['#fb7185', '#f43f5e', '#fda4af'][Math.floor(Math.random() * 3)]
                    });
                }
            }
            initAmbientHearts();

            // Dibujar forma de corazón en canvas
            function drawHeart(x, y, size, color, alpha, rotation = 0) {
                ctx.save();
                ctx.translate(x, y);
                ctx.rotate(rotation);
                ctx.globalAlpha = Math.max(0, Math.min(1, alpha));
                ctx.fillStyle = color;
                ctx.beginPath();
                const topCurveHeight = size * 0.3;
                ctx.moveTo(0, topCurveHeight);
                ctx.bezierCurveTo(0, 0, -size / 2, 0, -size / 2, topCurveHeight);
                ctx.bezierCurveTo(-size / 2, (size + topCurveHeight) / 2, 0, (size + topCurveHeight) / 1.4, 0, size);
                ctx.bezierCurveTo(0, (size + topCurveHeight) / 1.4, size / 2, (size + topCurveHeight) / 2, size / 2, topCurveHeight);
                ctx.bezierCurveTo(size / 2, 0, 0, 0, 0, topCurveHeight);
                ctx.closePath();
                ctx.fill();
                ctx.restore();
            }

            // Explosión de corazones y destellos dorados al dispararse la carta
            function triggerHeartBurst(originX, originY) {
                const colors = ['#e11d48', '#fb7185', '#f43f5e', '#f59e0b', '#fbbf24', '#fda4af'];
                for (let i = 0; i < 48; i++) {
                    const angle = (Math.PI * 2 * i) / 48 + (Math.random() - 0.5) * 0.35;
                    const speed = 3.5 + Math.random() * 7.5;
                    particles.push({
                        x: originX,
                        y: originY,
                        vx: Math.cos(angle) * speed,
                        vy: Math.sin(angle) * speed - 3.2, // Impulso ascendente extra
                        size: 9 + Math.random() * 11,
                        color: colors[Math.floor(Math.random() * colors.length)],
                        alpha: 1,
                        decay: 0.011 + Math.random() * 0.008,
                        rotation: (Math.random() - 0.5) * 0.8,
                        vRot: (Math.random() - 0.5) * 0.08,
                        isSparkle: i % 4 === 0
                    });
                }
            }

            // Bucle de animación del canvas
            function animateParticles() {
                ctx.clearRect(0, 0, canvas.width, canvas.height);

                // 1. Corazones ambientales
                for (const h of ambientHearts) {
                    h.y -= h.speedY;
                    h.drift += 0.015;
                    const xOffset = Math.sin(h.drift) * 18;
                    if (h.y < -30) {
                        h.y = canvas.height + 20;
                        h.x = Math.random() * canvas.width;
                    }
                    drawHeart(h.x + xOffset, h.y, h.size, h.color, h.opacity);
                }

                // 2. Partículas de explosión
                for (let i = particles.length - 1; i >= 0; i--) {
                    const p = particles[i];
                    p.x += p.vx;
                    p.y += p.vy;
                    p.vy += 0.14; // Gravedad suave
                    p.vx *= 0.985;
                    p.rotation += p.vRot;
                    p.alpha -= p.decay;

                    if (p.alpha <= 0) {
                        particles.splice(i, 1);
                        continue;
                    }

                    if (p.isSparkle) {
                        ctx.save();
                        ctx.globalAlpha = p.alpha;
                        ctx.fillStyle = p.color;
                        ctx.beginPath();
                        ctx.arc(p.x, p.y, p.size * 0.28, 0, Math.PI * 2);
                        ctx.fill();
                        ctx.restore();
                    } else {
                        drawHeart(p.x, p.y, p.size, p.color, p.alpha, p.rotation);
                    }
                }

                requestAnimationFrame(animateParticles);
            }

            // Sonido sutil y mágico sintetizado con Web Audio API (sin archivos externos)
            function playMagicalChime() {
                try {
                    const AudioCtx = window.AudioContext || window.webkitAudioContext;
                    if (!AudioCtx) return;
                    const audioCtx = new AudioCtx();
                    const notes = [523.25, 659.25, 783.99, 1046.50]; // Acorde de Do Mayor cálido

                    notes.forEach((freq, idx) => {
                        const osc = audioCtx.createOscillator();
                        const gain = audioCtx.createGain();
                        osc.type = 'sine';
                        osc.frequency.setValueAtTime(freq, audioCtx.currentTime + idx * 0.09);

                        gain.gain.setValueAtTime(0.001, audioCtx.currentTime + idx * 0.09);
                        gain.gain.exponentialRampToValueAtTime(0.06, audioCtx.currentTime + idx * 0.09 + 0.06);
                        gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + idx * 0.09 + 1.1);

                        osc.connect(gain);
                        gain.connect(audioCtx.destination);

                        osc.start(audioCtx.currentTime + idx * 0.09);
                        osc.stop(audioCtx.currentTime + idx * 0.09 + 1.15);
                    });
                } catch (e) {
                    // Ignorar si el navegador bloquea el audio
                }
            }

            // Secuencia de apertura del sobre y disparo de la carta
            function openLetter() {
                if (isOpen || isAnimating) return;
                isAnimating = true;

                playMagicalChime();
                hintBanner.classList.add('hidden-hint');
                envelopeWrapper.classList.add('opening');

                // Paso 1: Se abre la solapa 3D y tras 420ms sale disparado el papel
                setTimeout(() => {
                    const rect = envelopeWrapper.getBoundingClientRect();
                    triggerHeartBurst(rect.left + rect.width / 2, rect.top + rect.height * 0.35);

                    letterSheet.classList.add('shooting');
                    letterSheet.setAttribute('aria-hidden', 'false');
                }, 420);

                // Paso 2: Una vez que el papel sale disparado hacia arriba, se expande al centro para leer
                setTimeout(() => {
                    envelopeWrapper.classList.remove('opening');
                    envelopeWrapper.classList.add('opened');

                    letterSheet.classList.remove('shooting');
                    letterSheet.classList.add('expanded');
                    letterScrollArea.scrollTop = 0;

                    resetBtn.classList.add('visible');
                    isOpen = true;
                    isAnimating = false;
                }, 1120);
            }

            // Secuencia para volver a doblar/guardar la carta dentro del sobre
            function closeLetter() {
                if (!isOpen || isAnimating) return;
                isAnimating = true;

                resetBtn.classList.remove('visible');
                letterSheet.classList.remove('expanded');
                letterSheet.setAttribute('aria-hidden', 'true');

                setTimeout(() => {
                    envelopeWrapper.classList.remove('opened');
                    hintBanner.classList.remove('hidden-hint');
                    isOpen = false;
                    isAnimating = false;
                }, 650);
            }

            // Eventos táctiles, de clic y teclado
            envelopeWrapper.addEventListener('click', openLetter);
            envelopeWrapper.addEventListener('keydown', (e) => {
                if (e.key === 'Enter' || e.key === ' ') {
                    e.preventDefault();
                    openLetter();
                }
            });

            resetBtn.addEventListener('click', closeLetter);

            // Iniciar animación en window.onload
            window.onload = function () {
                animateParticles();
            };
        })();
    </script>
</body>
</html>
