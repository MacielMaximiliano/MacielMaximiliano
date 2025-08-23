<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="color-scheme" content="dark light" />
  <title>Portfolio | Backend • Java • Spring</title>
  <meta name="description" content="Portfolio minimalista con animaciones para GitHub Pages. Backend, Java y Spring Boot." />
  <style>
    /* ====== Reset & base ====== */
    *, *::before, *::after { box-sizing: border-box; }
    :root {
      --bg: #0b0f13;
      --bg-soft: #0f141a;
      --text: #e8eef6;
      --muted: #a7b1c2;
      --card: #0f141a;
      --accent: #6ee7ff; /* cian */
      --accent-2: #7ee787; /* verde */
      --shadow: 0 10px 30px rgba(0,0,0,.35);
      --radius: 18px;
    }
    :root.light {
      --bg: #f6fafc;
      --bg-soft: #eef4f9;
      --text: #111318;
      --muted: #4b5563;
      --card: #ffffff;
      --accent: #007aff;
      --accent-2: #10b981;
      --shadow: 0 10px 20px rgba(2,6,23,.08);
    }
    html, body { height: 100%; }
    body {
      margin: 0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, Noto Sans, Helvetica Neue, Arial, "Apple Color Emoji", "Segoe UI Emoji";
      background: var(--bg);
      color: var(--text);
      line-height: 1.6;
      overflow-x: hidden;
    }
    a { color: inherit; text-decoration: none; }

    /* ====== Background Canvas ====== */
    #bg {
      position: fixed; inset: 0; z-index: -1;
      filter: saturate(1.1) contrast(1.05);
    }

    /* ====== Navbar ====== */
    .nav {
      position: sticky; top: 0; z-index: 30;
      backdrop-filter: blur(10px);
      background: color-mix(in oklab, var(--bg) 75%, transparent);
      border-bottom: 1px solid color-mix(in oklab, var(--text) 12%, transparent);
    }
    .nav-inner {
      max-width: 1100px; margin: 0 auto; display: flex; align-items: center; justify-content: space-between; padding: 14px 20px;
    }
    .brand { font-weight: 800; letter-spacing: .3px; display: flex; gap: 10px; align-items: center; }
    .brand .dot { width: 10px; height: 10px; border-radius: 999px; background: linear-gradient(135deg, var(--accent), var(--accent-2)); box-shadow: 0 0 0 4px color-mix(in oklab, var(--accent) 30%, transparent); }
    .menu { display: flex; gap: 18px; }
    .menu a { position: relative; padding: 8px 10px; font-weight: 600; color: var(--muted); }
    .menu a::after { content: ""; position: absolute; left: 10px; right: 10px; bottom: 6px; height: 2px; border-radius: 2px; background: linear-gradient(90deg, var(--accent), var(--accent-2)); transform: scaleX(0); transform-origin: left; transition: .35s ease; }
    .menu a:hover { color: var(--text); }
    .menu a:hover::after { transform: scaleX(1); }

    .actions { display:flex; gap:10px; align-items:center; }
    .toggle { border: 1px solid color-mix(in oklab, var(--text) 12%, transparent); background: var(--card); color: var(--text); padding: 8px 12px; border-radius: 999px; cursor: pointer; box-shadow: var(--shadow); display:flex; gap:8px; align-items:center; }

    /* ====== Layout blocks ====== */
    .wrap { max-width: 1100px; margin: 0 auto; padding: 60px 20px; }
    section { padding: 80px 0; position: relative; }

    /* ====== Hero ====== */
    .hero { display: grid; grid-template-columns: 1.2fr .8fr; gap: 40px; align-items: center; }
    .kicker { color: var(--muted); font-weight: 700; letter-spacing: .22em; text-transform: uppercase; font-size: .8rem; }
    .title { font-size: clamp(2rem, 4vw, 3.6rem); line-height: 1.1; font-weight: 900; margin: 8px 0 10px; }
    .title .gradient { background: linear-gradient(90deg, var(--accent), var(--accent-2)); -webkit-background-clip: text; background-clip: text; color: transparent; position: relative; }
    .shine { background: linear-gradient(120deg, transparent 0%, transparent 30%, #fff8 50%, transparent 70%, transparent 100%); -webkit-background-clip: text; background-clip: text; animation: shine 4s linear infinite; }
    @keyframes shine { 0%{ background-position: -200% 0 } 100%{ background-position: 200% 0 } }

    .lead { color: var(--muted); font-size: 1.08rem; }
    .cta { display:flex; gap:12px; margin-top: 22px; }
    .btn { padding: 12px 16px; border-radius: 12px; font-weight: 700; border: 1px solid color-mix(in oklab, var(--text) 14%, transparent); background: var(--card); box-shadow: var(--shadow); }
    .btn.primary { background: linear-gradient(135deg, var(--accent), var(--accent-2)); color:#071017; border: none; }
    .terminal {
      border-radius: var(--radius); background: var(--card); border: 1px solid color-mix(in oklab, var(--text) 12%, transparent); padding: 18px; box-shadow: var(--shadow);
      font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, "Liberation Mono", monospace; overflow: hidden; position: relative;
    }
    .dots { display:flex; gap:8px; margin-bottom: 10px; }
    .dot-r { width:10px; height:10px; border-radius:999px; background:#ff5f56; }
    .dot-y { width:10px; height:10px; border-radius:999px; background:#ffbd2e; }
    .dot-g { width:10px; height:10px; border-radius:999px; background:#27c93f; }
    .type { white-space: pre-wrap; font-size: .96rem; }

    /* ====== Skills ====== */
    .grid { display: grid; gap: 16px; grid-template-columns: repeat(12, 1fr); }
    .col-4 { grid-column: span 4; }
    .col-6 { grid-column: span 6; }
    .col-12 { grid-column: span 12; }
    @media (max-width: 900px) {
      .hero { grid-template-columns: 1fr; }
      .col-4 { grid-column: span 6; }
      .col-6 { grid-column: span 12; }
    }
    @media (max-width: 640px) {
      .col-4 { grid-column: span 12; }
    }

    .card { background: var(--card); border: 1px solid color-mix(in oklab, var(--text) 12%, transparent); border-radius: var(--radius); padding: 18px; box-shadow: var(--shadow); transition: transform .3s ease, border-color .3s ease; position: relative; }
    .card:hover { transform: translateY(-6px); border-color: color-mix(in oklab, var(--accent) 40%, transparent); }
    .card h3 { margin: 0 0 8px; }
    .badges { display:flex; flex-wrap: wrap; gap: 8px; }
    .badge { padding: 6px 10px; border-radius: 999px; border: 1px solid color-mix(in oklab, var(--text) 14%, transparent); font-size: .85rem; }

    /* tilt indicator */
    .card::after { content:""; position:absolute; inset:0; border-radius: inherit; pointer-events:none; background: radial-gradient(600px circle at var(--mx,50%) var(--my,50%), color-mix(in oklab, var(--accent) 20%, transparent), transparent 40%); opacity: 0; transition: opacity .2s; }
    .card:hover::after { opacity: 1; }

    /* ====== Proyectos ====== */
    .projects { display:grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 16px; }
    .repo { position: relative; }
    .repo h4 { margin: 0 0 6px; font-size: 1.05rem; }
    .repo p { margin: 0 0 10px; color: var(--muted); min-height: 2.4em; }
    .repo .meta { display:flex; gap:12px; align-items:center; color: var(--muted); font-size: .9rem; }

    /* ====== Reveal on scroll ====== */
    .reveal { opacity: 0; transform: translateY(14px); transition: opacity .6s ease, transform .6s ease; }
    .reveal.show { opacity: 1; transform: none; }

    /* ====== Footer ====== */
    footer { padding: 40px 0 60px; text-align: center; color: var(--muted); }

    /* ====== Floating icons ====== */
    .float-icons span { position: absolute; font-size: clamp(16px, 3vw, 26px); opacity: .16; user-select:none; }
    .float-icons span:nth-child(1){ top:8%; left:6%; animation: drift 18s linear infinite; }
    .float-icons span:nth-child(2){ top:30%; right:8%; animation: drift 22s linear infinite reverse; }
    .float-icons span:nth-child(3){ bottom:14%; left:14%; animation: drift 25s linear infinite; }
    .float-icons span:nth-child(4){ bottom:8%; right:12%; animation: drift 19s linear infinite reverse; }
    @keyframes drift { 0%{ transform: translateY(0) } 50%{ transform: translateY(-14px) } 100%{ transform: translateY(0) } }

    /* ====== Utility ====== */
    .sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0, 0, 1, 1); white-space: nowrap; border: 0; }
    .center { text-align: center; }
  </style>
</head>
<body>
  <canvas id="bg" aria-hidden="true"></canvas>

  <nav class="nav" role="navigation" aria-label="principal">
    <div class="nav-inner">
      <a href="#" class="brand" aria-label="Inicio">
        <span class="dot" aria-hidden="true"></span>
        <span>TuNombre.dev</span>
      </a>
      <div class="menu" role="menubar">
        <a role="menuitem" href="#sobre">Sobre mí</a>
        <a role="menuitem" href="#skills">Skills</a>
        <a role="menuitem" href="#proyectos">Proyectos</a>
        <a role="menuitem" href="#contacto">Contacto</a>
      </div>
      <div class="actions">
        <button id="themeToggle" class="toggle" title="Cambiar tema" aria-label="Cambiar tema"><span id="themeIcon">🌙</span><span class="hide@sm">Tema</span></button>
      </div>
    </div>
  </nav>

  <main class="wrap">
    <section class="hero" id="inicio">
      <div>
        <div class="kicker">Backend • Java • Spring Boot</div>
        <h1 class="title">
          Hola, soy <span class="gradient shine">Tu Nombre</span>
        </h1>
        <p class="lead">Desarrollo APIs robustas, microservicios y sistemas escalables. Me enfoco en <strong>Java</strong>, <strong>Spring Boot</strong>, performance y buenas prácticas DevOps.</p>
        <div class="cta">
          <a class="btn primary" href="#proyectos">Ver proyectos</a>
          <a class="btn" href="#contacto">Contactame</a>
        </div>
      </div>
      <div class="terminal reveal" aria-label="ventana de código" role="region">
        <div class="dots"><span class="dot-r"></span><span class="dot-y"></span><span class="dot-g"></span></div>
        <pre class="type" id="typewriter" aria-live="polite"></pre>
      </div>
      <div class="float-icons" aria-hidden="true">
        <span>☕</span>
        <span>🍃</span>
        <span>{ }</span>
        <span>⚙️</span>
      </div>
    </section>

    <section id="sobre" class="reveal">
      <div class="grid">
        <div class="col-6">
          <div class="card">
            <h3>Sobre mí</h3>
            <p>Ingeniero backend con <strong>Java 17+</strong>, <strong>Spring Boot</strong>, <strong>JPA/Hibernate</strong> y <strong>Docker</strong>. Me encantan los patrones de diseño, la observabilidad y automatizar todo lo posible.</p>
            <div class="badges">
              <span class="badge">REST</span>
              <span class="badge">OpenAPI</span>
              <span class="badge">PostgreSQL</span>
              <span class="badge">Redis</span>
              <span class="badge">Kafka/RabbitMQ</span>
              <span class="badge">JUnit</span>
            </div>
          </div>
        </div>
        <div class="col-6">
          <div class="card">
            <h3>Lo que estoy buscando</h3>
            <p>Equipos con cultura de código limpio, CI/CD y foco en impacto de negocio. Si tu stack usa Spring y microservicios, ¡hablemos!</p>
            <a class="btn" href="#contacto">Disponibilidad</a>
          </div>
        </div>
      </div>
    </section>

    <section id="skills" class="reveal">
      <h2 class="center" style="margin:0 0 16px">Stack favorito</h2>
      <div class="grid">
        <div class="col-4"><div class="card tilt"><h3>Java & JVM</h3><p>Java 17+, Records, Streams, Loom (cuando aplique).</p><div class="badges"><span class="badge">Gradle/Maven</span><span class="badge">JMH</span><span class="badge">GC Tuning</span></div></div></div>
        <div class="col-4"><div class="card tilt"><h3>Spring Boot</h3><p>WebFlux/MVC, Data JPA, Security, Validation, Actuator.</p><div class="badges"><span class="badge">OpenAPI</span><span class="badge">MapStruct</span><span class="badge">Flyway</span></div></div></div>
        <div class="col-4"><div class="card tilt"><h3>Infra & DevOps</h3><p>Docker, K8s básico, Observabilidad y CI/CD.</p><div class="badges"><span class="badge">Docker</span><span class="badge">GitHub Actions</span><span class="badge">Grafana</span></div></div></div>
      </div>
    </section>

    <section id="proyectos" class="reveal">
      <div style="display:flex; align-items:end; justify-content:space-between; gap:16px; flex-wrap:wrap; margin-bottom:10px">
        <h2 style="margin:0">Proyectos</h2>
        <label for="ghuser" class="sr-only">Usuario de GitHub</label>
        <div style="display:flex; gap:8px; align-items:center">
          <input id="ghuser" type="text" placeholder="tu usuario de GitHub" style="padding:10px 12px; border-radius:10px; border:1px solid color-mix(in oklab, var(--text) 14%, transparent); background:var(--card); color:var(--text)"/>
          <button class="btn" id="loadRepos">Cargar</button>
        </div>
      </div>
      <div class="projects" id="repos" aria-live="polite"></div>
    </section>

    <section id="contacto" class="reveal">
      <div class="grid">
        <div class="col-6"><div class="card">
          <h3>Contacto</h3>
          <p>¿Charlamos? Enviame un mensaje y coordinamos.</p>
          <div class="badges">
            <a class="badge" href="mailto:tuemail@ejemplo.com" id="email">tuemail@ejemplo.com</a>
            <a class="badge" href="https://www.linkedin.com/in/TU_LINKEDIN/" target="_blank" rel="noopener">LinkedIn</a>
            <a class="badge" href="https://github.com/TU_USUARIO" target="_blank" rel="noopener">GitHub</a>
          </div>
        </div></div>
        <div class="col-6"><div class="card">
          <h3>CV</h3>
          <p>Descargá mi CV actualizado en PDF.</p>
          <a class="btn primary" id="cvBtn" href="#">Descargar CV</a>
        </div></div>
      </div>
    </section>
  </main>

  <footer>
    Hecho con Java ☕ y Spring 🍃 — © <span id="year"></span>
  </footer>

  <script>
    // ========= Config rápida =========
    const CONFIG = {
      GITHUB_USERNAME: "TU_USUARIO", // ← cambialo o usa el input de la sección
      RESUME_URL: "#" // coloca aquí el enlace a tu CV en PDF
    };

    // ========= Theme toggle (persistente) =========
    const root = document.documentElement;
    const saved = localStorage.getItem('theme');
    if (saved) root.classList.toggle('light', saved === 'light');
    const isLight = () => root.classList.contains('light');
    const toggleBtn = document.getElementById('themeToggle');
    const themeIcon = document.getElementById('themeIcon');
    const setIcon = () => themeIcon.textContent = isLight() ? '☀️' : '🌙';
    setIcon();
    toggleBtn.addEventListener('click', () => {
      root.classList.toggle('light');
      localStorage.setItem('theme', isLight() ? 'light' : 'dark');
      setIcon();
    });

    // ========= Background Particles =========
    const canvas = document.getElementById('bg');
    const ctx = canvas.getContext('2d');
    let particles = [], W, H, RAF;
    const DPR = Math.min(2, window.devicePixelRatio || 1);
    function resize(){
      W = canvas.width = innerWidth * DPR; H = canvas.height = innerHeight * DPR; canvas.style.width = innerWidth + 'px'; canvas.style.height = innerHeight + 'px';
      particles = Array.from({length: Math.max(40, Math.min(120, Math.floor(innerWidth/14)))}).map(()=>({
        x: Math.random()*W, y: Math.random()*H, vx: (Math.random()-.5)*.25*DPR, vy: (Math.random()-.5)*.25*DPR, r: Math.random()*2*DPR + .6*DPR
      }));
    }
    window.addEventListener('resize', resize, {passive:true}); resize();
    function draw(){
      const accent = getComputedStyle(root).getPropertyValue('--accent').trim();
      ctx.clearRect(0,0,W,H);
      ctx.globalAlpha = .75; ctx.fillStyle = accent; ctx.strokeStyle = accent;
      for (let i=0;i<particles.length;i++){
        const p = particles[i]; p.x+=p.vx; p.y+=p.vy;
        if (p.x<0||p.x>W) p.vx*=-1; if (p.y<0||p.y>H) p.vy*=-1;
        ctx.beginPath(); ctx.arc(p.x, p.y, p.r, 0, Math.PI*2); ctx.fill();
        for (let j=i+1;j<particles.length;j++){
          const q = particles[j];
          const dx=p.x-q.x, dy=p.y-q.y, d=Math.hypot(dx,dy);
          if (d < 120*DPR) { ctx.globalAlpha = (1 - d/(120*DPR)) * .45; ctx.beginPath(); ctx.moveTo(p.x,p.y); ctx.lineTo(q.x,q.y); ctx.stroke(); ctx.globalAlpha=.75; }
        }
      }
      RAF = requestAnimationFrame(draw);
    }
    draw();

    // ========= Typewriter =========
    const lines = [
      'record UserDto(Long id, String email) {}',
      '@RestController\nclass HealthController {\n  @GetMapping("/health") String ok(){ return "UP"; }\n}',
      '@Transactional\npublic void updateMembership(User u) { /* ... */ }',
      'docker compose up -d  # Postgres + Redis + Grafana',
    ];
    let li=0, ci=0, forward=true; const out = document.getElementById('typewriter');
    function type(){
      const curr = lines[li];
      out.textContent = curr.slice(0,ci) + (ci%2 ? '▍' : ' ');
      ci += forward ? 1 : -1;
      if (ci > curr.length + 6) forward = false;
      if (!forward && ci <= 0) { forward = true; li = (li+1) % lines.length; }
      setTimeout(type, forward ? 30 : 12);
    }
    type();

    // ========= Reveal on scroll =========
    const io = new IntersectionObserver((entries)=> entries.forEach(e=> e.isIntersecting && e.target.classList.add('show')), { threshold:.12 });
    document.querySelectorAll('.reveal').forEach(el=> io.observe(el));

    // ========= Tilt cards =========
    document.querySelectorAll('.tilt').forEach(card=>{
      card.addEventListener('pointermove', (e)=>{
        const r = card.getBoundingClientRect();
        const mx = (e.clientX - r.left)/r.width; const my = (e.clientY - r.top)/r.height;
        card.style.transform = `rotateX(${(0.5 - my)*6}deg) rotateY(${(mx - 0.5)*6}deg)`;
        card.style.setProperty('--mx', `${mx*100}%`); card.style.setProperty('--my', `${my*100}%`);
      });
      card.addEventListener('pointerleave', ()=> card.style.transform = 'translateY(-6px)');
    });

    // ========= GitHub Repos =========
    const repoList = document.getElementById('repos');
    const ghInput = document.getElementById('ghuser');
    document.getElementById('loadRepos').addEventListener('click', ()=> loadRepos(ghInput.value.trim()||CONFIG.GITHUB_USERNAME));
    ghInput.placeholder = CONFIG.GITHUB_USERNAME;

    async function loadRepos(user){
      if (!user) return; repoList.textContent = 'Cargando repos...';
      try {
        const res = await fetch(`https://api.github.com/users/${user}/repos?per_page=12&sort=updated`);
        if (!res.ok) throw new Error('No se pudo obtener repos');
        const data = (await res.json()).filter(r=>!r.fork);
        if (!data.length){ repoList.textContent = 'Sin proyectos públicos aún.'; return; }
        repoList.innerHTML = data.map(r=> `
          <article class="card repo">
            <h4><a href="${r.html_url}" target="_blank" rel="noopener">${r.name}</a></h4>
            <p>${(r.description||'Proyecto de código abierto').slice(0,120)}</p>
            <div class="meta">
              <span>★ ${r.stargazers_count}</span>
              <span>⑂ ${r.forks_count}</span>
              ${r.language ? `<span>● ${r.language}</span>` : ''}
            </div>
          </article>`).join('');
      } catch(err){ repoList.textContent = 'Error cargando proyectos 😵'; console.error(err); }
    }
    // Carga inicial
    loadRepos(CONFIG.GITHUB_USERNAME);

    // ========= Util =========
    document.getElementById('cvBtn').href = CONFIG.RESUME_URL;
    document.getElementById('year').textContent = new Date().getFullYear();

    // Suave scroll
    document.querySelectorAll('a[href^="#"]').forEach(a=> a.addEventListener('click', e=>{ const id=a.getAttribute('href'); if (id.length>1){ e.preventDefault(); document.querySelector(id).scrollIntoView({behavior:'smooth'}); } }));
  </script>
</body>
</html>

