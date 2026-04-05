# Universi
Galaxia de amor
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Mi Universo Móvil</title>
<style>
  body {
    margin: 0;
    padding: 0;
    overflow: hidden;
    background: #050508;
    font-family: sans-serif;
    height: 100vh;
    width: 100vw;
    touch-action: none; /* Evita el scroll accidental */
  }

  /* Fondo de estrellas */
  #universe {
    position: fixed;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at center, #1b2735 0%, #050508 100%);
  }

  .star {
    position: absolute;
    background: white;
    border-radius: 50%;
    opacity: 0.5;
  }

  /* Título Central - Adaptado para pantallas pequeñas */
  .center-text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 90%;
    font-size: 2.2em;
    font-weight: bold;
    text-align: center;
    color: #ffffff;
    text-shadow: 0 0 15px #ff00de, 0 0 30px #ff00de;
    z-index: 10;
    pointer-events: none;
  }

  /* Sistema planetario simplificado para rendimiento móvil */
  .solar-system {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 300px;
    height: 300px;
    transform: translate(-50%, -50%) rotateX(70deg);
    transform-style: preserve-3d;
  }

  .orbit {
    position: absolute;
    top: 50%;
    left: 50%;
    border: 1px solid rgba(255, 255, 255, 0.15);
    border-radius: 50%;
    transform: translate(-50%, -50%);
    animation: spin linear infinite;
  }

  .planet {
    position: absolute;
    top: 50%;
    left: 100%;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    transform: translate(-50%, -50%) rotateX(-70deg);
    box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
  }

  @keyframes spin {
    from { transform: translate(-50%, -50%) rotate(0deg); }
    to { transform: translate(-50%, -50%) rotate(360deg); }
  }

  /* Estallido de partículas al tocar */
  .particle {
    position: absolute;
    width: 4px;
    height: 4px;
    background: #ff00de;
    border-radius: 50%;
    pointer-events: none;
  }
</style>
</head>
<body>

<div id="universe"></div>
<div class="center-text" id="main-title">Mi universo entero</div>

<div class="solar-system">
  <div class="orbit" style="width:160px; height:160px; animation-duration: 6s;">
    <div class="planet" style="background: #4fc3f7;"></div>
  </div>
  <div class="orbit" style="width:260px; height:260px; animation-duration: 10s;">
    <div class="planet" style="background: #ff7043;"></div>
  </div>
</div>

<script>
  const universe = document.getElementById('universe');
  const title = document.getElementById('main-title');

  // Crear estrellas
  for (let i = 0; i < 100; i++) {
    const star = document.createElement('div');
    star.className = 'star';
    const size = Math.random() * 2 + 1;
    star.style.width = size + 'px';
    star.style.height = size + 'px';
    star.style.top = Math.random() * 100 + 'vh';
    star.style.left = Math.random() * 100 + 'vw';
    universe.appendChild(star);
  }

  // Interacción Táctil: Estallido de luz donde toques
  document.addEventListener('touchstart', (e) => {
    const touch = e.touches[0];
    createExplosion(touch.clientX, touch.clientY);
    
    // Mover el título ligeramente hacia el toque
    const x = (window.innerWidth / 2 - touch.clientX) / 15;
    const y = (window.innerHeight / 2 - touch.clientY) / 15;
    title.style.transform = `translate(calc(-50% + ${-x}px), calc(-50% + ${-y}px)) scale(1.05)`;
  });

  document.addEventListener('touchend', () => {
    title.style.transform = `translate(-50%, -50%) scale(1)`;
  });

  function createExplosion(x, y) {
    for (let i = 0; i < 8; i++) {
      const p = document.createElement('div');
      p.className = 'particle';
      p.style.left = x + 'px';
      p.style.top = y + 'px';
      document.body.appendChild(p);

      const angle = Math.random() * Math.PI * 2;
      const dist = Math.random() * 50 + 20;
      
      p.animate([
        { transform: 'translate(0, 0) scale(1)', opacity: 1 },
        { transform: `translate(${Math.cos(angle) * dist}px, ${Math.sin(angle) * dist}px) scale(0)`, opacity: 0 }
      ], { duration: 800, easing: 'ease-out' }).onfinish = () => p.remove();
    }
  }

  // Opcional: Reacción al movimiento del dispositivo (Giroscopio)
  if (typeof DeviceOrientationEvent !== 'undefined' && typeof DeviceOrientationEvent.requestPermission === 'function') {
    // Para iOS necesita permiso explícito
    document.addEventListener('click', () => {
      DeviceOrientationEvent.requestPermission();
    });
  }

  window.addEventListener('deviceorientation', (e) => {
    const tiltX = e.gamma / 2; // Inclinación izquierda/derecha
    const tiltY = e.beta / 2;  // Inclinación adelante/atrás
    universe.style.transform = `translate(${tiltX}px, ${tiltY}px)`;
  });

</script>
</body>
</html>
