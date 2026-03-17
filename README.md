# har02old2003.githud.io
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>HAXX — Full Stack Developer</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;700;900&family=Rajdhani:wght@300;400;500;600;700&family=Share+Tech+Mono&family=Exo+2:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
:root {
  --bg-base:    #0a0f1e;
  --bg-mid:     #0d1428;
  --bg-light:   #111d3a;
  --glass-bg:   rgba(255,255,255,0.04);
  --glass-border: rgba(100,180,255,0.18);
  --glass-shine: rgba(255,255,255,0.08);
  --cyan:       #00d4ff;
  --cyan-dim:   #0090cc;
  --cyan-glow:  rgba(0,212,255,0.35);
  --blue:       #4a9eff;
  --blue-dim:   rgba(74,158,255,0.15);
  --green:      #00ff9d;
  --green-dim:  rgba(0,255,157,0.1);
  --text-bright: #e8f4ff;
  --text-mid:   #8ab4d4;
  --text-dim:   #445d7a;
  --chrome:     #d0e8ff;
  --border-soft: rgba(100,180,255,0.12);
}

*{margin:0;padding:0;box-sizing:border-box;}
html{scroll-behavior:smooth;}

body {
  background: var(--bg-base);
  color: var(--text-bright);
  font-family: 'Exo 2', sans-serif;
  overflow-x: hidden;
  cursor: none;
}

/* ═══════════════════════════════════════
   BACKGROUND SYSTEM
═══════════════════════════════════════ */
.bg-layer {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 0;
}

/* Gradient mesh background - slightly lighter */
.bg-mesh {
  background:
    radial-gradient(ellipse 80% 50% at 20% 10%, rgba(0,100,200,0.12) 0%, transparent 60%),
    radial-gradient(ellipse 60% 40% at 80% 80%, rgba(0,180,255,0.08) 0%, transparent 50%),
    radial-gradient(ellipse 100% 80% at 50% 50%, rgba(10,25,60,0.6) 0%, transparent 100%),
    linear-gradient(160deg, #0c1530 0%, #0a0f1e 40%, #080d1a 100%);
}

/* Grid */
.bg-grid {
  background-image:
    linear-gradient(rgba(0,180,255,0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,180,255,0.05) 1px, transparent 1px);
  background-size: 50px 50px;
}

/* Moving rain/data stream canvas */
#matrixCanvas {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  pointer-events: none;
  z-index: 0;
  opacity: 0.12;
}

/* ═══════════════════════════════════════
   CURSOR
═══════════════════════════════════════ */
#cur { position:fixed; width:10px; height:10px; background:var(--cyan); border-radius:50%; pointer-events:none; z-index:9999; transform:translate(-50%,-50%); box-shadow:0 0 15px var(--cyan), 0 0 40px var(--cyan-glow); transition:width .15s,height .15s,background .15s; mix-blend-mode:screen; }
#curRing { position:fixed; width:40px; height:40px; border:1.5px solid rgba(0,212,255,0.5); border-radius:50%; pointer-events:none; z-index:9998; transform:translate(-50%,-50%); transition:all .08s linear; }
#curTrail { position:fixed; width:4px; height:4px; background:rgba(0,212,255,0.3); border-radius:50%; pointer-events:none; z-index:9997; transform:translate(-50%,-50%); }

/* ═══════════════════════════════════════
   GLASSMORPHISM SYSTEM
═══════════════════════════════════════ */
.glass {
  background: var(--glass-bg);
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  border: 1px solid var(--glass-border);
  position: relative;
  overflow: hidden;
}

.glass::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent 0%, rgba(255,255,255,0.15) 30%, rgba(0,212,255,0.3) 50%, rgba(255,255,255,0.15) 70%, transparent 100%);
}

.glass::after {
  content: '';
  position: absolute;
  top: 0; left: -100%;
  width: 60%; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.03), transparent);
  transform: skewX(-15deg);
  animation: glassShine 6s ease infinite;
}

@keyframes glassShine {
  0%,70% { left: -100%; }
  100% { left: 200%; }
}

/* Glass variants */
.glass-cyan {
  background: rgba(0,150,210,0.07);
  border-color: rgba(0,212,255,0.25);
  box-shadow: 0 0 30px rgba(0,212,255,0.06), inset 0 1px 0 rgba(255,255,255,0.08);
}

.glass-blue {
  background: rgba(74,100,200,0.07);
  border-color: rgba(74,158,255,0.2);
  box-shadow: 0 0 30px rgba(74,100,200,0.06), inset 0 1px 0 rgba(255,255,255,0.06);
}

.glass-green {
  background: rgba(0,200,100,0.06);
  border-color: rgba(0,255,157,0.2);
  box-shadow: 0 0 30px rgba(0,200,100,0.05), inset 0 1px 0 rgba(255,255,255,0.06);
}

/* ═══════════════════════════════════════
   NAVBAR
═══════════════════════════════════════ */
nav {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 1000;
  padding: 16px 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgba(8,13,28,0.7);
  backdrop-filter: blur(30px);
  border-bottom: 1px solid rgba(0,180,255,0.1);
  transition: all .4s;
}

nav.scrolled {
  background: rgba(8,13,28,0.95);
  border-bottom-color: rgba(0,212,255,0.2);
  box-shadow: 0 4px 40px rgba(0,0,0,0.4);
}

.nav-brand {
  display: flex;
  align-items: center;
  gap: 14px;
}

.nav-logo-text {
  font-family: 'Orbitron', monospace;
  font-weight: 900;
  font-size: 20px;
  letter-spacing: 6px;
  background: linear-gradient(135deg, #fff 0%, var(--cyan) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.nav-status {
  display: flex;
  align-items: center;
  gap: 6px;
  font-family: 'Share Tech Mono', monospace;
  font-size: 9px;
  letter-spacing: 3px;
  color: var(--green);
}

.nav-status-dot {
  width: 5px; height: 5px;
  background: var(--green);
  border-radius: 50%;
  box-shadow: 0 0 8px var(--green);
  animation: pulse 2s ease infinite;
}

.nav-links { display:flex; gap:36px; list-style:none; }

.nav-links a {
  color: var(--text-mid);
  text-decoration: none;
  font-family: 'Share Tech Mono', monospace;
  font-size: 11px;
  letter-spacing: 3px;
  text-transform: uppercase;
  transition: color .3s;
  position: relative;
  cursor: none;
}

.nav-links a::before {
  content: attr(data-text);
  position: absolute;
  top: 0; left: 0;
  color: var(--cyan);
  clip-path: inset(0 100% 0 0);
  transition: clip-path .4s;
}

.nav-links a:hover { color: transparent; }
.nav-links a:hover::before { clip-path: inset(0 0% 0 0); }

/* Glass CTA button */
.btn-glass {
  padding: 10px 28px;
  background: rgba(0,180,255,0.1);
  border: 1px solid rgba(0,212,255,0.4);
  color: var(--cyan);
  text-decoration: none;
  font-family: 'Share Tech Mono', monospace;
  font-size: 11px;
  letter-spacing: 3px;
  text-transform: uppercase;
  cursor: none;
  position: relative;
  overflow: hidden;
  transition: all .3s;
  backdrop-filter: blur(10px);
}

.btn-glass::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(0,212,255,0.15), rgba(0,212,255,0.05));
  opacity: 0;
  transition: opacity .3s;
}

.btn-glass:hover { border-color: var(--cyan); box-shadow: 0 0 25px var(--cyan-glow), 0 0 50px rgba(0,212,255,0.1), inset 0 0 25px rgba(0,212,255,0.05); transform: translateY(-2px); }
.btn-glass:hover::before { opacity: 1; }

/* ═══════════════════════════════════════
   HERO
═══════════════════════════════════════ */
#hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding: 110px 60px 80px;
  position: relative;
  z-index: 1;
  overflow: hidden;
}

/* Floating particles */
.particles-container {
  position: absolute;
  inset: 0;
  overflow: hidden;
  pointer-events: none;
}

.particle {
  position: absolute;
  width: 2px; height: 2px;
  background: var(--cyan);
  border-radius: 50%;
  box-shadow: 0 0 6px var(--cyan);
  animation: floatParticle linear infinite;
  opacity: 0;
}

@keyframes floatParticle {
  0% { opacity: 0; transform: translateY(0) translateX(0); }
  10% { opacity: 1; }
  90% { opacity: 0.6; }
  100% { opacity: 0; transform: translateY(-100vh) translateX(var(--dx)); }
}

/* Glitch scanline hero overlay */
.hero-scanline {
  position: absolute;
  inset: 0;
  background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.04) 2px, rgba(0,0,0,0.04) 4px);
  pointer-events: none;
  z-index: 2;
  animation: scanMove 8s linear infinite;
}

@keyframes scanMove {
  0% { background-position: 0 0; }
  100% { background-position: 0 100px; }
}

/* Moving highlight beam */
.hero-beam {
  position: absolute;
  top: 0;
  left: -20%;
  width: 40%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(0,180,255,0.03), transparent);
  transform: skewX(-15deg);
  animation: beamMove 10s ease-in-out infinite;
  pointer-events: none;
}

@keyframes beamMove {
  0%, 100% { left: -40%; }
  50% { left: 100%; }
}

.hero-inner {
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  gap: 60px;
  align-items: center;
  max-width: 1400px;
  width: 100%;
  margin: 0 auto;
  position: relative;
  z-index: 3;
}

/* Hero text */
.hero-tag-wrap {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 28px;
  animation: slideDown .8s ease both;
}

.hero-tag {
  padding: 6px 18px;
  background: rgba(0,212,255,0.08);
  border: 1px solid rgba(0,212,255,0.3);
  backdrop-filter: blur(10px);
  font-family: 'Share Tech Mono', monospace;
  font-size: 10px;
  letter-spacing: 4px;
  color: var(--cyan);
  text-transform: uppercase;
  display: flex;
  align-items: center;
  gap: 8px;
}

.hero-tag-dot {
  width: 6px; height: 6px;
  background: var(--cyan);
  border-radius: 50%;
  box-shadow: 0 0 8px var(--cyan);
  animation: pulse 2s ease infinite;
}

@keyframes pulse { 0%,100%{opacity:1;} 50%{opacity:0.3;} }

.hero-line-small {
  flex: 1;
  height: 1px;
  background: linear-gradient(90deg, rgba(0,212,255,0.4), transparent);
}

.hero-name {
  font-family: 'Orbitron', monospace;
  font-weight: 900;
  font-size: clamp(52px, 7vw, 90px);
  line-height: 0.9;
  letter-spacing: 2px;
  margin-bottom: 20px;
  animation: slideDown .8s .1s both;
  position: relative;
}

.hero-name-text {
  background: linear-gradient(160deg, #ffffff 0%, #a0d8ff 40%, var(--cyan) 80%, #0090cc 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  filter: drop-shadow(0 0 30px rgba(0,212,255,0.4));
}

/* Glitch effect on name */
.hero-name::before, .hero-name::after {
  content: 'HAXX';
  position: absolute;
  top: 0; left: 0;
  font-family: 'Orbitron', monospace;
  font-weight: 900;
  font-size: inherit;
  letter-spacing: 2px;
  -webkit-text-fill-color: transparent;
}

.hero-name::before {
  -webkit-text-stroke: 1px rgba(0,212,255,0.4);
  clip-path: inset(40% 0 45% 0);
  transform: translate(-3px);
  animation: glitch1 4s ease infinite;
}

.hero-name::after {
  -webkit-text-stroke: 1px rgba(255,100,100,0.3);
  clip-path: inset(55% 0 30% 0);
  transform: translate(3px);
  animation: glitch2 4s ease infinite;
}

@keyframes glitch1 {
  0%,94%,100% { clip-path:inset(40% 0 45% 0); transform:translate(-3px); opacity:0; }
  95%,97% { clip-path:inset(20% 0 65% 0); transform:translate(-5px); opacity:1; }
  96%,98% { clip-path:inset(70% 0 10% 0); transform:translate(-2px); opacity:1; }
}

@keyframes glitch2 {
  0%,94%,100% { clip-path:inset(55% 0 30% 0); transform:translate(3px); opacity:0; }
  95%,97% { clip-path:inset(10% 0 75% 0); transform:translate(5px); opacity:1; }
  96%,98% { clip-path:inset(80% 0 5% 0); transform:translate(2px); opacity:1; }
}

.hero-subtitle {
  font-family: 'Rajdhani', sans-serif;
  font-weight: 300;
  font-size: clamp(14px, 2vw, 20px);
  letter-spacing: 10px;
  text-transform: uppercase;
  color: var(--text-mid);
  margin-bottom: 32px;
  animation: slideDown .8s .2s both;
}

.hero-subtitle span { color: var(--cyan); font-weight: 600; }

/* Typing text */
.hero-typing-wrap {
  min-height: 60px;
  margin-bottom: 36px;
  animation: slideDown .8s .3s both;
}

.typing-label {
  font-family: 'Share Tech Mono', monospace;
  font-size: 12px;
  color: var(--green);
  letter-spacing: 2px;
  margin-bottom: 8px;
}

.typing-label::before { content: '> '; opacity: 0.6; }

.typing-text {
  font-family: 'Share Tech Mono', monospace;
  font-size: 15px;
  color: var(--text-bright);
  min-height: 24px;
}

.typing-cursor {
  display: inline-block;
  width: 9px; height: 18px;
  background: var(--cyan);
  vertical-align: middle;
  margin-left: 2px;
  animation: blinkCursor .7s step-end infinite;
  box-shadow: 0 0 8px var(--cyan);
}

@keyframes blinkCursor { 0%,100%{opacity:1;} 50%{opacity:0;} }

/* Hero actions */
.hero-actions {
  display: flex;
  gap: 16px;
  flex-wrap: wrap;
  margin-bottom: 52px;
  animation: slideDown .8s .4s both;
}

.btn-primary-glass {
  padding: 15px 38px;
  background: linear-gradient(135deg, rgba(0,180,255,0.18), rgba(0,80,200,0.1));
  border: 1px solid rgba(0,212,255,0.5);
  backdrop-filter: blur(15px);
  color: var(--cyan);
  text-decoration: none;
  font-family: 'Share Tech Mono', monospace;
  font-size: 11px;
  letter-spacing: 4px;
  text-transform: uppercase;
  cursor: none;
  position: relative;
  overflow: hidden;
  transition: all .35s;
  display: flex;
  align-items: center;
  gap: 10px;
}

.btn-primary-glass::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(0,212,255,0.2), transparent);
  opacity: 0;
  transition: opacity .3s;
}

.btn-primary-glass::after {
  content: '';
  position: absolute;
  top: 0; left: -100%;
  width: 100%; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.08), transparent);
  transition: left .5s;
}

.btn-primary-glass:hover { border-color:var(--cyan); box-shadow:0 0 30px var(--cyan-glow), 0 0 60px rgba(0,212,255,0.08), inset 0 0 30px rgba(0,212,255,0.05); transform:translateY(-3px); }
.btn-primary-glass:hover::before { opacity:1; }
.btn-primary-glass:hover::after { left:100%; }

.btn-outline-glass {
  padding: 15px 38px;
  background: rgba(255,255,255,0.03);
  border: 1px solid rgba(100,180,255,0.2);
  backdrop-filter: blur(10px);
  color: var(--text-mid);
  text-decoration: none;
  font-family: 'Share Tech Mono', monospace;
  font-size: 11px;
  letter-spacing: 4px;
  text-transform: uppercase;
  cursor: none;
  transition: all .3s;
  display: flex;
  align-items: center;
  gap: 10px;
}

.btn-outline-glass:hover { border-color:rgba(100,180,255,0.4); color:var(--text-bright); background:rgba(255,255,255,0.05); transform:translateY(-3px); }

.btn-icon { font-size: 14px; }

/* Hero stats glass */
.hero-stats {
  display: flex;
  gap: 0;
  animation: slideDown .8s .5s both;
  border: 1px solid var(--glass-border);
  background: rgba(255,255,255,0.03);
  backdrop-filter: blur(15px);
  overflow: hidden;
}

.stat-glass {
  flex: 1;
  padding: 20px 24px;
  border-right: 1px solid var(--border-soft);
  text-align: center;
  position: relative;
  transition: background .3s;
}

.stat-glass:last-child { border-right: none; }
.stat-glass:hover { background: rgba(0,212,255,0.04); }

.stat-n {
  font-family: 'Orbitron', monospace;
  font-size: 30px;
  font-weight: 700;
  background: linear-gradient(135deg, #fff, var(--cyan));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  line-height: 1;
  filter: drop-shadow(0 0 10px rgba(0,212,255,0.4));
}

.stat-l {
  font-family: 'Share Tech Mono', monospace;
  font-size: 9px;
  letter-spacing: 3px;
  color: var(--text-dim);
  text-transform: uppercase;
  margin-top: 6px;
}

/* ═══════════════════════════════════════
   HERO RIGHT - LOGO
═══════════════════════════════════════ */
.hero-right {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  animation: slideRight 1s .3s both;
}

/* HEX grid behind logo */
.hex-grid {
  position: absolute;
  inset: -40px;
  opacity: 0.06;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='60' height='104'%3E%3Cpath d='M30 0L60 17v34L30 68 0 51V17z' fill='none' stroke='%2300d4ff' stroke-width='0.5'/%3E%3Cpath d='M30 36L60 53v34L30 104 0 87V53z' fill='none' stroke='%2300d4ff' stroke-width='0.5'/%3E%3Cpath d='M-30 18L0 1v34L-30 52l-30-17V1z' fill='none' stroke='%2300d4ff' stroke-width='0.5'/%3E%3C/svg%3E");
}

.logo-frame {
  position: relative;
  width: 400px; height: 400px;
}

/* Outer ring - data ring */
.data-ring {
  position: absolute;
  inset: 0;
  border-radius: 50%;
  border: 1px solid rgba(0,212,255,0.15);
  animation: spin 25s linear infinite;
}

.data-ring::before {
  content: '';
  position: absolute;
  top: -3px; left: 50%;
  transform: translateX(-50%);
  width: 6px; height: 6px;
  background: var(--cyan);
  border-radius: 50%;
  box-shadow: 0 0 12px var(--cyan), 0 0 24px rgba(0,212,255,0.5);
}

.data-ring-2 {
  position: absolute;
  inset: 25px;
  border-radius: 50%;
  border: 1px dashed rgba(0,180,255,0.1);
  animation: spin 15s linear infinite reverse;
}

.data-ring-2::before {
  content: '';
  position: absolute;
  bottom: -3px; left: 50%;
  transform: translateX(-50%);
  width: 4px; height: 4px;
  background: var(--blue);
  border-radius: 50%;
  box-shadow: 0 0 8px var(--blue);
}

.data-ring-3 {
  position: absolute;
  inset: 50px;
  border-radius: 50%;
  border: 1px solid rgba(0,255,157,0.08);
  animation: spin 10s linear infinite;
}

@keyframes spin { from{transform:rotate(0)} to{transform:rotate(360deg)} }

/* Orbit dots */
.orbit-dot {
  position: absolute;
  width: 100%; height: 100%;
  top: 0; left: 0;
  animation: spin 8s linear infinite;
}

.orbit-dot::after {
  content: '';
  position: absolute;
  top: 50%;
  right: -4px;
  transform: translateY(-50%);
  width: 8px; height: 8px;
  background: rgba(0,212,255,0.6);
  border-radius: 50%;
  box-shadow: 0 0 10px var(--cyan);
}

/* Logo center glass card */
.logo-center {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  width: 260px; height: 260px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(0,60,120,0.3) 0%, rgba(0,20,60,0.15) 60%, transparent 100%);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(0,212,255,0.1);
  box-shadow: 0 0 60px rgba(0,100,200,0.1), inset 0 0 40px rgba(0,212,255,0.04);
  animation: logoFloat 6s ease-in-out infinite;
}

@keyframes logoFloat {
  0%,100%{transform:translate(-50%,-50%) translateY(0);}
  50%{transform:translate(-50%,-50%) translateY(-15px);}
}

.logo-center img {
  width: 200px; height: 200px;
  object-fit: contain;
  filter: drop-shadow(0 0 20px rgba(0,180,255,0.5)) drop-shadow(0 0 50px rgba(0,100,255,0.2));
}

/* Corner brackets */
.bracket {
  position: absolute;
  width: 24px; height: 24px;
  border-color: rgba(0,212,255,0.5);
  border-style: solid;
  transition: all .4s;
}
.logo-frame:hover .bracket { width: 32px; height: 32px; border-color: var(--cyan); }
.bracket.tl { top: 60px; left: 60px; border-width: 2px 0 0 2px; }
.bracket.tr { top: 60px; right: 60px; border-width: 2px 2px 0 0; }
.bracket.bl { bottom: 60px; left: 60px; border-width: 0 0 2px 2px; }
.bracket.br { bottom: 60px; right: 60px; border-width: 0 2px 2px 0; }

/* Data labels around logo */
.data-label {
  position: absolute;
  font-family: 'Share Tech Mono', monospace;
  font-size: 9px;
  letter-spacing: 2px;
  color: rgba(0,212,255,0.4);
  white-space: nowrap;
}
.dl-top { top: 30px; left: 50%; transform: translateX(-50%); }
.dl-right { right: 10px; top: 50%; transform: translateY(-50%) rotate(90deg); }
.dl-bottom { bottom: 30px; left: 50%; transform: translateX(-50%); }
.dl-left { left: 10px; top: 50%; transform: translateY(-50%) rotate(-90deg); }

/* ═══════════════════════════════════════
   HACKER TERMINAL SECTION
═══════════════════════════════════════ */
.hacker-bar {
  position: relative;
  z-index: 1;
  padding: 0 60px;
  margin: 0;
}

.hacker-inner {
  max-width: 1400px;
  margin: 0 auto;
}

.hacker-strip {
  background: rgba(0,8,20,0.8);
  border: 1px solid rgba(0,255,157,0.15);
  backdrop-filter: blur(20px);
  padding: 0 30px;
  display: flex;
  align-items: center;
  gap: 0;
  overflow: hidden;
  position: relative;
}

.hacker-strip::before {
  content: '';
  position: absolute;
  top: 0; left: 0;
  width: 4px; height: 100%;
  background: var(--green);
  box-shadow: 0 0 15px var(--green);
}

.hs-label {
  font-family: 'Share Tech Mono', monospace;
  font-size: 10px;
  letter-spacing: 3px;
  color: var(--green);
  white-space: nowrap;
  padding: 14px 20px 14px 16px;
  border-right: 1px solid rgba(0,255,157,0.15);
}

.hs-scroll-wrap {
  overflow: hidden;
  flex: 1;
}

.hs-scroll {
  display: flex;
  animation: hackScroll 25s linear infinite;
  white-space: nowrap;
}

.hs-scroll span {
  font-family: 'Share Tech Mono', monospace;
  font-size: 11px;
  color: rgba(0,255,157,0.5);
  letter-spacing: 2px;
  padding: 14px 30px;
}

.hs-scroll span.active { color: var(--green); }

@keyframes hackScroll {
  from { transform: translateX(0); }
  to { transform: translateX(-50%); }
}

/* ═══════════════════════════════════════
   SECTIONS BASE
═══════════════════════════════════════ */
section {
  position: relative;
  z-index: 1;
  padding: 100px 60px;
}

.section-max { max-width: 1400px; margin: 0 auto; }

.section-head {
  margin-bottom: 64px;
}

.section-eyebrow {
  font-family: 'Share Tech Mono', monospace;
  font-size: 10px;
  letter-spacing: 6px;
  color: var(--cyan);
  text-transform: uppercase;
  margin-bottom: 14px;
  display: flex;
  align-items: center;
  gap: 14px;
}

.section-eyebrow::before {
  content: '//';
  color: rgba(0,212,255,0.4);
}

.section-eyebrow::after {
  content: '';
  flex: 0 0 40px;
  height: 1px;
  background: linear-gradient(90deg, var(--cyan), transparent);
}

.section-h2 {
  font-family: 'Orbitron', monospace;
  font-size: clamp(28px, 3.5vw, 48px);
  font-weight: 700;
  letter-spacing: 2px;
  line-height: 1.1;
  color: var(--chrome);
}

.section-h2 .c { color: var(--cyan); }
.section-h2 .g { background: linear-gradient(135deg, var(--cyan), var(--blue)); -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text; }

/* ═══════════════════════════════════════
   ABOUT
═══════════════════════════════════════ */
#about { background: linear-gradient(180deg, transparent, rgba(0,80,180,0.03), transparent); }

.about-grid {
  display: grid;
  grid-template-columns: 1fr 1.2fr;
  gap: 60px;
  align-items: start;
}

/* Terminal glass card */
.terminal-glass {
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 30px 80px rgba(0,0,0,0.4), 0 0 40px rgba(0,100,200,0.06);
  border: 1px solid rgba(0,180,255,0.15);
  background: rgba(5,10,25,0.8);
  backdrop-filter: blur(20px);
}

.tg-header {
  padding: 12px 20px;
  background: rgba(0,20,50,0.6);
  border-bottom: 1px solid rgba(0,180,255,0.12);
  display: flex;
  align-items: center;
  gap: 8px;
}

.tdot { width: 10px; height: 10px; border-radius: 50%; }
.tdot.r { background: #ff5f57; box-shadow: 0 0 6px rgba(255,95,87,0.6); }
.tdot.y { background: #febc2e; box-shadow: 0 0 6px rgba(254,188,46,0.6); }
.tdot.g { background: #28c840; box-shadow: 0 0 6px rgba(40,200,64,0.6); }

.tg-title {
  font-family: 'Share Tech Mono', monospace;
  font-size: 11px;
  color: var(--text-dim);
  letter-spacing: 2px;
  margin-left: 8px;
}

.tg-body {
  padding: 24px;
  font-family: 'Share Tech Mono', monospace;
  font-size: 12.5px;
  line-height: 2.2;
}

.tl { display: flex; gap: 8px; }
.tp { color: var(--cyan); white-space: nowrap; }
.tc { color: var(--text-bright); }
.to { color: var(--text-mid); padding-left: 16px; }
.to .h { color: var(--cyan); }
.to .g { color: var(--green); }
.tcur { display: inline-block; width: 8px; height: 14px; background: var(--cyan); box-shadow: 0 0 8px var(--cyan); animation: blinkCursor .7s step-end infinite; vertical-align: middle; }

/* About right */
.about-text { font-size: 15px; line-height: 1.9; color: var(--text-mid); margin-bottom: 20px; }
.about-text strong { color: var(--text-bright); font-weight: 600; }

/* Glass highlight cards */
.highlights-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 14px;
  margin-top: 36px;
}

.hl-card {
  padding: 18px 20px;
  background: rgba(255,255,255,0.03);
  border: 1px solid rgba(100,180,255,0.12);
  backdrop-filter: blur(15px);
  display: flex;
  align-items: center;
  gap: 14px;
  position: relative;
  overflow: hidden;
  cursor: none;
  transition: all .35s cubic-bezier(.23,1,.32,1);
}

.hl-card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(0,212,255,0.3), transparent);
  opacity: 0;
  transition: opacity .3s;
}

.hl-card::after {
  content: '';
  position: absolute;
  bottom: 0; left: 0;
  width: 3px; height: 100%;
  background: linear-gradient(180deg, transparent, var(--cyan), transparent);
  opacity: 0;
  transition: opacity .3s;
}

.hl-card:hover {
  background: rgba(0,180,255,0.05);
  border-color: rgba(0,212,255,0.3);
  transform: translateY(-3px) translateX(4px);
  box-shadow: 0 10px 40px rgba(0,100,200,0.1), 0 0 20px rgba(0,212,255,0.05);
}
.hl-card:hover::before { opacity: 1; }
.hl-card:hover::after { opacity: 1; }

.hl-icon {
  width: 42px; height: 42px;
  background: rgba(0,180,255,0.08);
  border: 1px solid rgba(0,180,255,0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  flex-shrink: 0;
  font-style: normal;
}

.hl-text { font-size: 12px; font-weight: 600; letter-spacing: 1.5px; color: var(--text-bright); }
.hl-sub { font-size: 11px; color: var(--text-dim); letter-spacing: 1px; margin-top: 2px; font-family: 'Share Tech Mono', monospace; }

/* ═══════════════════════════════════════
   SKILLS
═══════════════════════════════════════ */
#skills { background: rgba(0,5,20,0.3); }

.skills-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 20px;
}

.skill-card {
  background: rgba(255,255,255,0.03);
  border: 1px solid rgba(100,180,255,0.1);
  backdrop-filter: blur(20px);
  padding: 28px;
  position: relative;
  overflow: hidden;
  cursor: none;
  transition: all .4s cubic-bezier(.23,1,.32,1);
}

.skill-card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--cyan), transparent);
  opacity: 0;
  transition: opacity .4s;
}

.skill-card::after {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at var(--mx,50%) var(--my,50%), rgba(0,180,255,0.06) 0%, transparent 60%);
  opacity: 0;
  transition: opacity .3s;
}

.skill-card:hover {
  border-color: rgba(0,212,255,0.3);
  background: rgba(0,120,200,0.06);
  transform: translateY(-5px);
  box-shadow: 0 20px 60px rgba(0,0,0,0.3), 0 0 40px rgba(0,100,200,0.08);
}
.skill-card:hover::before { opacity: 1; }
.skill-card:hover::after { opacity: 1; }

.sc-header {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 24px;
  padding-bottom: 18px;
  border-bottom: 1px solid rgba(100,180,255,0.08);
}

.sc-icon {
  width: 44px; height: 44px;
  background: rgba(0,180,255,0.08);
  border: 1px solid rgba(0,180,255,0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  font-style: normal;
  flex-shrink: 0;
}

.sc-name {
  font-family: 'Orbitron', monospace;
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 3px;
  color: var(--chrome);
}

.sc-count {
  margin-left: auto;
  font-family: 'Share Tech Mono', monospace;
  font-size: 10px;
  color: var(--cyan);
  letter-spacing: 2px;
}

/* Skill tags - glass pills */
.stags { display: flex; flex-wrap: wrap; gap: 8px; }

.stag {
  padding: 5px 14px;
  background: rgba(0,120,200,0.08);
  border: 1px solid rgba(0,180,255,0.12);
  backdrop-filter: blur(8px);
  font-family: 'Share Tech Mono', monospace;
  font-size: 10px;
  letter-spacing: 2px;
  color: var(--text-mid);
  cursor: none;
  transition: all .25s;
  position: relative;
  overflow: hidden;
}

.stag::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(0,212,255,0.1), transparent);
  opacity: 0;
  transition: opacity .25s;
}

.stag:hover {
  background: rgba(0,180,255,0.12);
  border-color: rgba(0,212,255,0.4);
  color: var(--cyan);
  transform: translateY(-3px);
  box-shadow: 0 6px 20px rgba(0,100,200,0.15), 0 0 12px rgba(0,212,255,0.15);
}
.stag:hover::before { opacity: 1; }

/* ═══════════════════════════════════════
   PROJECTS
═══════════════════════════════════════ */
#projects { background: linear-gradient(180deg, transparent, rgba(0,60,180,0.03), transparent); }

.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(380px, 1fr));
  gap: 24px;
}

.proj-card {
  background: rgba(5,12,30,0.7);
  border: 1px solid rgba(100,180,255,0.1);
  backdrop-filter: blur(25px);
  overflow: hidden;
  cursor: none;
  transition: all .45s cubic-bezier(.23,1,.32,1);
  position: relative;
}

.proj-card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 3px;
  background: linear-gradient(90deg, transparent, var(--cyan), var(--blue), transparent);
  opacity: 0;
  transition: opacity .4s;
}

.proj-card:hover {
  border-color: rgba(0,212,255,0.25);
  transform: translateY(-10px) scale(1.01);
  box-shadow: 0 40px 80px rgba(0,0,0,0.4), 0 0 50px rgba(0,100,200,0.08);
}
.proj-card:hover::before { opacity: 1; }

.pv { /* project visual */
  height: 180px;
  position: relative;
  overflow: hidden;
  background: linear-gradient(135deg, rgba(5,12,35,1), rgba(10,25,60,1));
  display: flex;
  align-items: center;
  justify-content: center;
}

.pv-grid {
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(0,180,255,0.08) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,180,255,0.08) 1px, transparent 1px);
  background-size: 25px 25px;
}

.pv-glow {
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at 50% 50%, rgba(0,120,200,0.12) 0%, transparent 70%);
  transition: opacity .4s;
}

.proj-card:hover .pv-glow { opacity: 2; }

.pv-icon {
  font-size: 58px;
  position: relative;
  z-index: 2;
  filter: drop-shadow(0 0 20px rgba(0,180,255,0.5));
  font-style: normal;
  transition: transform .4s;
}
.proj-card:hover .pv-icon { transform: scale(1.15) translateY(-5px); }

.pv-tags {
  position: absolute;
  bottom: 14px; right: 14px;
  display: flex; gap: 6px; z-index: 2;
}

.pvt {
  padding: 4px 10px;
  background: rgba(0,80,150,0.5);
  border: 1px solid rgba(0,212,255,0.3);
  backdrop-filter: blur(10px);
  font-family: 'Share Tech Mono', monospace;
  font-size: 9px;
  letter-spacing: 2px;
  color: var(--cyan);
}

/* Project number badge */
.pv-num {
  position: absolute;
  top: 14px; left: 14px;
  font-family: 'Share Tech Mono', monospace;
  font-size: 10px;
  color: rgba(0,212,255,0.4);
  letter-spacing: 2px;
  z-index: 2;
}

.pb { padding: 26px 28px; }

.pb-name {
  font-family: 'Orbitron', monospace;
  font-size: 17px;
  font-weight: 700;
  letter-spacing: 1.5px;
  color: var(--chrome);
  margin-bottom: 10px;
  line-height: 1.3;
  transition: color .3s;
}
.proj-card:hover .pb-name { color: #fff; }

.pb-desc {
  font-size: 13px;
  line-height: 1.8;
  color: var(--text-mid);
  margin-bottom: 22px;
}

.pb-links { display: flex; gap: 12px; }

.pl {
  padding: 8px 20px;
  border: 1px solid rgba(0,180,255,0.25);
  background: rgba(0,100,200,0.06);
  backdrop-filter: blur(8px);
  color: var(--cyan);
  text-decoration: none;
  font-family: 'Share Tech Mono', monospace;
  font-size: 10px;
  letter-spacing: 2px;
  text-transform: uppercase;
  cursor: none;
  transition: all .3s;
  position: relative;
  overflow: hidden;
}

.pl::before {
  content: '';
  position: absolute;
  top: 0; left: -100%;
  width: 100%; height: 100%;
  background: linear-gradient(90deg, transparent, rgba(0,212,255,0.1), transparent);
  transition: left .4s;
}

.pl:hover { border-color:rgba(0,212,255,.6); box-shadow:0 0 20px rgba(0,212,255,.2); background:rgba(0,180,255,0.1); }
.pl:hover::before { left: 100%; }

.pl.g { border-color:rgba(100,180,255,0.15); color:var(--text-dim); }
.pl.g:hover { border-color:rgba(100,180,255,0.3); color:var(--text-mid); background:rgba(100,180,255,0.04); }

/* ═══════════════════════════════════════
   KNOWLEDGE / BLOG
═══════════════════════════════════════ */
#knowledge { background: rgba(0,5,20,0.4); }

.knowledge-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(310px, 1fr));
  gap: 20px;
}

.kc {
  padding: 28px;
  background: rgba(255,255,255,0.03);
  border: 1px solid rgba(100,180,255,0.1);
  backdrop-filter: blur(20px);
  position: relative;
  overflow: hidden;
  cursor: none;
  transition: all .35s cubic-bezier(.23,1,.32,1);
}

.kc::before {
  content: '';
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--cyan), transparent);
  opacity: 0;
  transition: opacity .3s;
}

/* Animated corner */
.kc::after {
  content: '';
  position: absolute;
  bottom: 0; right: 0;
  width: 0; height: 0;
  border-left: 20px solid transparent;
  border-bottom: 20px solid rgba(0,180,255,0.15);
  transition: all .3s;
}

.kc:hover {
  background: rgba(0,100,200,0.05);
  border-color: rgba(0,212,255,0.25);
  transform: translateY(-5px);
  box-shadow: 0 20px 50px rgba(0,0,0,0.3), 0 0 30px rgba(0,100,200,0.06);
}
.kc:hover::before { opacity: 1; }
.kc:hover::after { border-bottom-color: rgba(0,180,255,0.4); }

.kc-cat {
  font-family: 'Share Tech Mono', monospace;
  font-size: 9px;
  letter-spacing: 4px;
  color: var(--cyan);
  text-transform: uppercase;
  margin-bottom: 14px;
  display: flex;
  align-items: center;
  gap: 8px;
}
.kc-cat::before { content:'⬡'; font-size: 12px; }

.kc-title {
  font-family: 'Orbitron', monospace;
  font-size: 14px;
  font-weight: 700;
  color: var(--chrome);
  letter-spacing: 0.5px;
  margin-bottom: 12px;
  line-height: 1.4;
  transition: color .3s;
}
.kc:hover .kc-title { color: #fff; }

.kc-excerpt { font-size: 13px; line-height: 1.8; color: var(--text-mid); margin-bottom: 22px; }

.kc-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.kc-meta {
  font-family: 'Share Tech Mono', monospace;
  font-size: 9px;
  color: var(--text-dim);
  letter-spacing: 2px;
}

.kc-arrow {
  width: 34px; height: 34px;
  border: 1px solid rgba(0,180,255,0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-dim);
  font-size: 14px;
  transition: all .3s;
  background: rgba(0,100,200,0.05);
}
.kc:hover .kc-arrow { background: rgba(0,180,255,0.12); border-color: rgba(0,212,255,0.5); color: var(--cyan); transform: translate(2px,-2px); box-shadow: 0 0 15px rgba(0,212,255,0.2); }

/* ═══════════════════════════════════════
   CONTACT
═══════════════════════════════════════ */
#contact {
  text-align: center;
  padding: 120px 60px;
  background: linear-gradient(180deg, transparent, rgba(0,60,180,0.04), transparent);
}

.contact-wrap { max-width: 800px; margin: 0 auto; }

.contact-h {
  font-family: 'Orbitron', monospace;
  font-size: clamp(30px, 4.5vw, 58px);
  font-weight: 700;
  letter-spacing: 2px;
  line-height: 1.1;
  margin-bottom: 24px;
}

.contact-h span {
  background: linear-gradient(135deg, var(--cyan), var(--blue));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.contact-sub { font-size: 16px; color: var(--text-mid); margin-bottom: 56px; line-height: 1.8; }

.contact-btns { display: flex; gap: 18px; justify-content: center; flex-wrap: wrap; margin-bottom: 56px; }

/* Social glass buttons */
.socials { display: flex; gap: 12px; justify-content: center; flex-wrap: wrap; }

.soc-btn {
  padding: 12px 24px;
  background: rgba(255,255,255,0.03);
  border: 1px solid rgba(100,180,255,0.12);
  backdrop-filter: blur(15px);
  color: var(--text-dim);
  text-decoration: none;
  font-family: 'Share Tech Mono', monospace;
  font-size: 10px;
  letter-spacing: 3px;
  text-transform: uppercase;
  cursor: none;
  transition: all .3s;
  display: flex;
  align-items: center;
  gap: 8px;
  position: relative;
  overflow: hidden;
}

.soc-btn::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(0,212,255,0.4), transparent);
  opacity: 0;
  transition: opacity .3s;
}

.soc-btn:hover {
  background: rgba(0,180,255,0.07);
  border-color: rgba(0,212,255,0.35);
  color: var(--cyan);
  transform: translateY(-3px);
  box-shadow: 0 10px 30px rgba(0,0,0,0.2), 0 0 20px rgba(0,212,255,0.08);
}
.soc-btn:hover::before { opacity: 1; }

/* ═══════════════════════════════════════
   FOOTER
═══════════════════════════════════════ */
footer {
  padding: 32px 60px;
  border-top: 1px solid rgba(100,180,255,0.08);
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
  z-index: 1;
  background: rgba(5,8,20,0.6);
  backdrop-filter: blur(20px);
}

.f-logo { font-family:'Orbitron',monospace; font-weight:900; font-size:15px; letter-spacing:5px; background:linear-gradient(135deg,#fff,var(--cyan)); -webkit-background-clip:text; -webkit-text-fill-color:transparent; background-clip:text; }
.f-copy { font-family:'Share Tech Mono',monospace; font-size:10px; letter-spacing:2px; color:var(--text-dim); }
.f-status { display:flex; align-items:center; gap:8px; font-family:'Share Tech Mono',monospace; font-size:10px; letter-spacing:2px; color:var(--green); }
.f-dot { width:6px; height:6px; background:var(--green); border-radius:50%; box-shadow:0 0 8px var(--green); animation:pulse 2s ease infinite; }

/* ═══════════════════════════════════════
   DIVIDER
═══════════════════════════════════════ */
.div-line {
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(0,180,255,0.15), transparent);
  margin: 0 60px;
  position: relative;
  z-index: 1;
}

/* ═══════════════════════════════════════
   SCROLL INDICATOR
═══════════════════════════════════════ */
.scroll-ind {
  position: absolute;
  bottom: 32px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 3;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
}

.si-txt { font-family:'Share Tech Mono',monospace; font-size:9px; letter-spacing:4px; color:var(--text-dim); }
.si-line { width:1px; height:44px; background:linear-gradient(180deg, var(--text-dim), transparent); animation:siDown 2s ease infinite; }

@keyframes siDown {
  0%{transform:scaleY(0);transform-origin:top;}
  50%{transform:scaleY(1);transform-origin:top;}
  51%{transform:scaleY(1);transform-origin:bottom;}
  100%{transform:scaleY(0);transform-origin:bottom;}
}

/* ═══════════════════════════════════════
   ANIMATIONS
═══════════════════════════════════════ */
@keyframes slideDown { from{opacity:0;transform:translateY(-24px);} to{opacity:1;transform:translateY(0);} }
@keyframes slideRight { from{opacity:0;transform:translateX(40px);} to{opacity:1;transform:translateX(0);} }

.reveal { opacity:0; transform:translateY(32px); transition:opacity .7s ease, transform .7s ease; }
.reveal.vis { opacity:1; transform:translateY(0); }

/* ═══════════════════════════════════════
   HACK EFFECT OVERLAY
═══════════════════════════════════════ */
#hackOverlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.97);
  z-index: 10000;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 24px;
  transition: opacity .8s;
}

.ho-logo {
  font-family: 'Orbitron', monospace;
  font-size: 60px;
  font-weight: 900;
  background: linear-gradient(135deg, #fff, var(--cyan));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  filter: drop-shadow(0 0 30px rgba(0,212,255,0.6));
  opacity: 0;
  transition: opacity .5s .3s;
}

.ho-text {
  font-family: 'Share Tech Mono', monospace;
  font-size: 13px;
  color: var(--cyan);
  letter-spacing: 4px;
  min-height: 20px;
}

.ho-bar-wrap {
  width: 300px;
  height: 2px;
  background: rgba(0,180,255,0.1);
  overflow: hidden;
}

.ho-bar {
  height: 100%;
  background: linear-gradient(90deg, var(--cyan), var(--blue));
  width: 0%;
  box-shadow: 0 0 10px var(--cyan);
  transition: width .05s linear;
}

.ho-skip {
  font-family: 'Share Tech Mono', monospace;
  font-size: 10px;
  color: var(--text-dim);
  letter-spacing: 3px;
  cursor: pointer;
  border: 1px solid rgba(100,180,255,0.15);
  padding: 8px 20px;
  background: transparent;
  transition: all .3s;
  margin-top: 10px;
}
.ho-skip:hover { color: var(--cyan); border-color: rgba(0,212,255,0.4); }

/* ═══════════════════════════════════════
   RESPONSIVE
═══════════════════════════════════════ */
@media(max-width:900px){
  nav{padding:16px 24px;}
  .nav-links{display:none;}
  section{padding:60px 24px;}
  #hero{padding:100px 24px 70px;}
  .hero-inner{grid-template-columns:1fr;gap:40px;}
  .hero-right{display:none;}
  .about-grid{grid-template-columns:1fr;}
  .projects-grid{grid-template-columns:1fr;}
  footer{flex-direction:column;gap:16px;text-align:center;}
  .div-line{margin:0 24px;}
  .hacker-bar{padding:0 24px;}
}
</style>
</head>
<body>

<!-- HACK INTRO OVERLAY -->
<div id="hackOverlay">
  <div class="ho-logo" id="hoLogo">HAXX</div>
  <div class="ho-text" id="hoText">&nbsp;</div>
  <div class="ho-bar-wrap"><div class="ho-bar" id="hoBar"></div></div>
  <button class="ho-skip" onclick="skipIntro()">[ SKIP ]</button>
</div>

<!-- CURSORS -->
<div id="cur"></div>
<div id="curRing"></div>
<div id="curTrail"></div>

<!-- BG LAYERS -->
<div class="bg-layer bg-mesh"></div>
<div class="bg-layer bg-grid"></div>
<canvas id="matrixCanvas"></canvas>

<!-- NAV -->
<nav id="mainNav">
  <div class="nav-brand">
    <div class="nav-logo-text">HAXX</div>
    <div class="nav-status"><div class="nav-status-dot"></div>ONLINE</div>
  </div>
  <ul class="nav-links">
    <li><a href="#about" data-text="ABOUT">ABOUT</a></li>
    <li><a href="#skills" data-text="SKILLS">SKILLS</a></li>
    <li><a href="#projects" data-text="PROJECTS">PROJECTS</a></li>
    <li><a href="#knowledge" data-text="BLOG">BLOG</a></li>
    <li><a href="#contact" data-text="CONTACT">CONTACT</a></li>
  </ul>
  <a href="#contact" class="btn-glass">HIRE ME</a>
</nav>

<!-- HERO -->
<section id="hero">
  <div class="particles-container" id="particles"></div>
  <div class="hero-scanline"></div>
  <div class="hero-beam"></div>

  <div class="hero-inner">
    <!-- LEFT -->
    <div class="hero-left">
      <div class="hero-tag-wrap">
        <div class="hero-tag"><div class="hero-tag-dot"></div>FULL STACK DEVELOPER</div>
        <div class="hero-line-small"></div>
      </div>

      <h1 class="hero-name">
        <span class="hero-name-text">HAXX</span>
      </h1>

      <div class="hero-subtitle">BUILDING THE <span>FUTURE</span> / LINE BY LINE</div>

      <div class="hero-typing-wrap">
        <div class="typing-label">STATUS</div>
        <div class="typing-text" id="typingText"><span class="typing-cursor"></span></div>
      </div>

      <div class="hero-actions">
        <a href="#projects" class="btn-primary-glass"><span class="btn-icon">⬡</span> VER PROYECTOS</a>
        <a href="#knowledge" class="btn-outline-glass"><span class="btn-icon">◈</span> BLOG & TUTORIALES</a>
      </div>

      <div class="hero-stats glass">
        <div class="stat-glass">
          <div class="stat-n" data-target="5">0</div>
          <div class="stat-l">AÑOS EXP</div>
        </div>
        <div class="stat-glass">
          <div class="stat-n" data-target="30">0</div>
          <div class="stat-l">PROYECTOS</div>
        </div>
        <div class="stat-glass">
          <div class="stat-n" data-target="12">0</div>
          <div class="stat-l">TECH STACK</div>
        </div>
        <div class="stat-glass">
          <div class="stat-n">∞</div>
          <div class="stat-l">APRENDIZAJE</div>
        </div>
      </div>
    </div>

    <!-- RIGHT -->
    <div class="hero-right">
      <div class="hex-grid"></div>
      <div class="logo-frame">
        <div class="data-ring"></div>
        <div class="data-ring-2"></div>
        <div class="data-ring-3"></div>
        <div class="orbit-dot"></div>

        <div class="bracket tl"></div>
        <div class="bracket tr"></div>
        <div class="bracket bl"></div>
        <div class="bracket br"></div>

        <span class="data-label dl-top">SYS_ACTIVE</span>
        <span class="data-label dl-right">0x4841585858</span>
        <span class="data-label dl-bottom">HAXX_v2.5</span>
        <span class="data-label dl-left">PROTOCOL_OK</span>

        <div class="logo-center">
          <img id="logoImg" src="" alt="HAXX">
        </div>
      </div>
    </div>
  </div>

  <div class="scroll-ind">
    <span class="si-txt">SCROLL</span>
    <div class="si-line"></div>
  </div>
</section>

<!-- HACKER BAR -->
<div class="hacker-bar">
  <div class="hacker-inner">
    <div class="hacker-strip">
      <span class="hs-label">// LIVE</span>
      <div class="hs-scroll-wrap">
        <div class="hs-scroll" id="hackScroll">
          <span>$ git commit -m "feat: nueva feature épica"</span>
          <span class="active">◈ BUILDING PRODUCTION SYSTEMS</span>
          <span>npm run deploy --prod</span>
          <span>docker build -t haxx/app:latest .</span>
          <span class="active">◈ ARQUITECTURA ESCALABLE</span>
          <span>kubectl apply -f deployment.yaml</span>
          <span>SELECT * FROM knowledge WHERE level > 9000;</span>
          <span class="active">◈ FULL STACK | WEB3 | DEVOPS</span>
          <span>$ ssh root@prod-server "zero downtime deploy"</span>
          <span>ping haxx.dev — TTL 64ms</span>
          <span>$ git commit -m "feat: nueva feature épica"</span>
          <span class="active">◈ BUILDING PRODUCTION SYSTEMS</span>
          <span>npm run deploy --prod</span>
          <span>docker build -t haxx/app:latest .</span>
          <span class="active">◈ ARQUITECTURA ESCALABLE</span>
          <span>kubectl apply -f deployment.yaml</span>
        </div>
      </div>
    </div>
  </div>
</div>

<div class="div-line" style="margin-top:60px"></div>

<!-- ABOUT -->
<section id="about">
  <div class="section-max">
    <div class="about-grid">
      <div class="reveal">
        <div class="terminal-glass">
          <div class="tg-header">
            <div class="tdot r"></div>
            <div class="tdot y"></div>
            <div class="tdot g"></div>
            <span class="tg-title">haxx@dev — zsh — 80×24</span>
          </div>
          <div class="tg-body">
            <div class="tl"><span class="tp">❯</span><span class="tc">&nbsp;whoami</span></div>
            <div class="tl to"><span class="h">HAXX</span> — Full Stack Developer & Educator</div>
            <br>
            <div class="tl"><span class="tp">❯</span><span class="tc">&nbsp;cat ./profile.json</span></div>
            <div class="tl to">{ name: <span class="h">"HAXX"</span>,</div>
            <div class="tl to">&nbsp;&nbsp;role: <span class="g">"FullStack + DevOps"</span>,</div>
            <div class="tl to">&nbsp;&nbsp;mission: <span class="h">"build & share"</span>,</div>
            <div class="tl to">&nbsp;&nbsp;available: <span class="g">true</span> }</div>
            <br>
            <div class="tl"><span class="tp">❯</span><span class="tc">&nbsp;ls ./skills/</span></div>
            <div class="tl to"><span class="h">frontend/</span>&nbsp;&nbsp;<span class="g">backend/</span>&nbsp;&nbsp;<span class="h">devops/</span>&nbsp;&nbsp;<span class="g">web3/</span></div>
            <br>
            <div class="tl"><span class="tp">❯</span><span class="tc">&nbsp;uptime</span></div>
            <div class="tl to">coding since <span class="h">5+ years</span>, <span class="g">always learning</span></div>
            <br>
            <div class="tl"><span class="tp">❯</span><span class="tc">&nbsp;<span class="tcur"></span></span></div>
          </div>
        </div>
      </div>

      <div class="reveal">
        <div class="section-head">
          <div class="section-eyebrow">SOBRE MÍ</div>
          <h2 class="section-h2">CÓDIGO, <span class="c">PASIÓN</span><br>& CONOCIMIENTO</h2>
        </div>
        <p class="about-text">Soy <strong>HAXX</strong>, un desarrollador full stack con foco en crear experiencias digitales que combinan rendimiento brutal, elegancia técnica y arquitecturas que escalan. No solo construyo software — <strong>comparto el camino</strong>.</p>
        <p class="about-text">Mi misión es democratizar el conocimiento técnico a través de contenido de alta calidad, proyectos open source y tutoriales profundos que eleven a la comunidad dev.</p>

        <div class="highlights-grid">
          <div class="hl-card">
            <span class="hl-icon">⚡</span>
            <div><div class="hl-text">ALTA PERFORMANCE</div><div class="hl-sub">CORE_WEB_VITALS_100</div></div>
          </div>
          <div class="hl-card">
            <span class="hl-icon">🔐</span>
            <div><div class="hl-text">SECURITY FIRST</div><div class="hl-sub">OWASP_COMPLIANT</div></div>
          </div>
          <div class="hl-card">
            <span class="hl-icon">📡</span>
            <div><div class="hl-text">SCALABLE ARCH</div><div class="hl-sub">MICROSERVICES_READY</div></div>
          </div>
          <div class="hl-card">
            <span class="hl-icon">🎓</span>
            <div><div class="hl-text">EDU CONTENT</div><div class="hl-sub">OPEN_KNOWLEDGE</div></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<div class="div-line"></div>

<!-- SKILLS -->
<section id="skills">
  <div class="section-max">
    <div class="section-head reveal">
      <div class="section-eyebrow">TECNOLOGÍAS</div>
      <h2 class="section-h2">MI <span class="g">STACK</span> TÉCNICO</h2>
    </div>
    <div class="skills-grid">
      <div class="skill-card reveal">
        <div class="sc-header">
          <div class="sc-icon">⚛</div>
          <span class="sc-name">FRONTEND</span>
          <span class="sc-count">08 TECH</span>
        </div>
        <div class="stags">
          <span class="stag">React</span><span class="stag">Next.js</span><span class="stag">Vue 3</span>
          <span class="stag">TypeScript</span><span class="stag">Tailwind</span><span class="stag">Framer</span>
          <span class="stag">WebGL</span><span class="stag">Three.js</span>
        </div>
      </div>
      <div class="skill-card reveal">
        <div class="sc-header">
          <div class="sc-icon">⚙</div>
          <span class="sc-name">BACKEND</span>
          <span class="sc-count">08 TECH</span>
        </div>
        <div class="stags">
          <span class="stag">Node.js</span><span class="stag">Python</span><span class="stag">Go</span>
          <span class="stag">Express</span><span class="stag">FastAPI</span><span class="stag">GraphQL</span>
          <span class="stag">REST</span><span class="stag">WebSockets</span>
        </div>
      </div>
      <div class="skill-card reveal">
        <div class="sc-header">
          <div class="sc-icon">🗄</div>
          <span class="sc-name">DATABASE</span>
          <span class="sc-count">07 TECH</span>
        </div>
        <div class="stags">
          <span class="stag">PostgreSQL</span><span class="stag">MongoDB</span><span class="stag">Redis</span>
          <span class="stag">MySQL</span><span class="stag">Prisma</span><span class="stag">Supabase</span>
          <span class="stag">Firebase</span>
        </div>
      </div>
      <div class="skill-card reveal">
        <div class="sc-header">
          <div class="sc-icon">☁</div>
          <span class="sc-name">DEVOPS & CLOUD</span>
          <span class="sc-count">08 TECH</span>
        </div>
        <div class="stags">
          <span class="stag">Docker</span><span class="stag">Kubernetes</span><span class="stag">AWS</span>
          <span class="stag">GCP</span><span class="stag">CI/CD</span><span class="stag">GitHub Actions</span>
          <span class="stag">Nginx</span><span class="stag">Linux</span>
        </div>
      </div>
    </div>
  </div>
</section>

<div class="div-line"></div>

<!-- PROJECTS -->
<section id="projects">
  <div class="section-max">
    <div class="section-head reveal">
      <div class="section-eyebrow">PORTAFOLIO</div>
      <h2 class="section-h2">PROYECTOS <span class="c">DESTACADOS</span></h2>
    </div>
    <div class="projects-grid">
      <div class="proj-card reveal">
        <div class="pv">
          <div class="pv-grid"></div>
          <div class="pv-glow"></div>
          <span class="pv-num">PROJECT_01</span>
          <span class="pv-icon">🚀</span>
          <div class="pv-tags"><span class="pvt">NEXT.JS</span><span class="pvt">AI</span></div>
        </div>
        <div class="pb">
          <div class="pb-name">SAAS PLATFORM WITH AI CORE</div>
          <p class="pb-desc">Plataforma SaaS completa con integración de IA, autenticación avanzada, dashboard en tiempo real y arquitectura serverless.</p>
          <div class="pb-links"><a href="#" class="pl">DEMO</a><a href="#" class="pl g">GITHUB</a></div>
        </div>
      </div>
      <div class="proj-card reveal">
        <div class="pv">
          <div class="pv-grid"></div>
          <div class="pv-glow"></div>
          <span class="pv-num">PROJECT_02</span>
          <span class="pv-icon">💎</span>
          <div class="pv-tags"><span class="pvt">WEB3</span><span class="pvt">SOLANA</span></div>
        </div>
        <div class="pb">
          <div class="pb-name">WEB3 MARKETPLACE PROTOCOL</div>
          <p class="pb-desc">Marketplace descentralizado sobre Solana con smart contracts, wallet integration y trading en tiempo real.</p>
          <div class="pb-links"><a href="#" class="pl">DEMO</a><a href="#" class="pl g">GITHUB</a></div>
        </div>
      </div>
      <div class="proj-card reveal">
        <div class="pv">
          <div class="pv-grid"></div>
          <div class="pv-glow"></div>
          <span class="pv-num">PROJECT_03</span>
          <span class="pv-icon">📊</span>
          <div class="pv-tags"><span class="pvt">REACT</span><span class="pvt">D3.JS</span></div>
        </div>
        <div class="pb">
          <div class="pb-name">ANALYTICS ENGINE DASHBOARD</div>
          <p class="pb-desc">Motor de analytics con visualizaciones D3, procesamiento de grandes datasets y reportes automáticos en PDF.</p>
          <div class="pb-links"><a href="#" class="pl">DEMO</a><a href="#" class="pl g">GITHUB</a></div>
        </div>
      </div>
    </div>
  </div>
</section>

<div class="div-line"></div>

<!-- KNOWLEDGE -->
<section id="knowledge">
  <div class="section-max">
    <div class="section-head reveal">
      <div class="section-eyebrow">BLOG & TUTORIALES</div>
      <h2 class="section-h2">COMPARTIENDO <span class="g">CONOCIMIENTO</span></h2>
    </div>
    <div class="knowledge-grid">
      <div class="kc reveal"><div class="kc-cat">ARQUITECTURA</div><div class="kc-title">Microservicios con Node.js: Guía Completa 2024</div><p class="kc-excerpt">Todo lo que necesitas para migrar de monolito a microservicios: patrones, comunicación y deployment.</p><div class="kc-footer"><span class="kc-meta">12 MIN READ</span><div class="kc-arrow">→</div></div></div>
      <div class="kc reveal"><div class="kc-cat">FRONTEND</div><div class="kc-title">React Performance: Optimizaciones que Cambian Todo</div><p class="kc-excerpt">Técnicas avanzadas de optimización en React: memoización, lazy loading y code splitting.</p><div class="kc-footer"><span class="kc-meta">8 MIN READ</span><div class="kc-arrow">→</div></div></div>
      <div class="kc reveal"><div class="kc-cat">DEVOPS</div><div class="kc-title">Docker + K8s: Del Desarrollo a Producción</div><p class="kc-excerpt">Pipeline completo con Docker, Kubernetes y GitHub Actions. Zero-downtime deployments.</p><div class="kc-footer"><span class="kc-meta">15 MIN READ</span><div class="kc-arrow">→</div></div></div>
      <div class="kc reveal"><div class="kc-cat">SEGURIDAD</div><div class="kc-title">JWT vs Sessions: La Decisión Que Importa</div><p class="kc-excerpt">Análisis profundo de ambas estrategias de auth. Cuándo usar cada una y cómo implementarlas.</p><div class="kc-footer"><span class="kc-meta">10 MIN READ</span><div class="kc-arrow">→</div></div></div>
      <div class="kc reveal"><div class="kc-cat">DATABASE</div><div class="kc-title">PostgreSQL Avanzado: Queries que Escalan</div><p class="kc-excerpt">Indexing, partitioning y query optimization para cuando tu DB crece a millones de filas.</p><div class="kc-footer"><span class="kc-meta">11 MIN READ</span><div class="kc-arrow">→</div></div></div>
      <div class="kc reveal"><div class="kc-cat">AI & ML</div><div class="kc-title">Integrando LLMs en Tu App: Guía Práctica</div><p class="kc-excerpt">Cómo integrar OpenAI, Anthropic y modelos locales en aplicaciones reales. RAG y embeddings.</p><div class="kc-footer"><span class="kc-meta">13 MIN READ</span><div class="kc-arrow">→</div></div></div>
    </div>
  </div>
</section>

<div class="div-line"></div>

<!-- CONTACT -->
<section id="contact">
  <div class="contact-wrap">
    <div class="section-eyebrow" style="justify-content:center;margin-bottom:20px;">CONTACTO</div>
    <h2 class="contact-h reveal">¿LISTO PARA<br><span>CONSTRUIR</span><br>ALGO GRANDE?</h2>
    <p class="contact-sub reveal">Disponible para proyectos freelance, colaboraciones y oportunidades remotas.<br>Hablemos sobre tu próxima idea.</p>
    <div class="contact-btns reveal">
      <a href="mailto:haxx@dev.com" class="btn-primary-glass"><span>📨</span> ENVIAR EMAIL</a>
      <a href="#" class="btn-outline-glass"><span>📄</span> DESCARGAR CV</a>
    </div>
    <div class="socials reveal">
      <a href="#" class="soc-btn">⬡ GITHUB</a>
      <a href="#" class="soc-btn">◈ TWITTER</a>
      <a href="#" class="soc-btn">▶ YOUTUBE</a>
      <a href="#" class="soc-btn">◆ LINKEDIN</a>
      <a href="#" class="soc-btn">◉ DISCORD</a>
      <a href="#" class="soc-btn">⬟ TIKTOK</a>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="f-logo">HAXX</div>
  <div class="f-copy">© 2025 HAXX — ALL RIGHTS RESERVED</div>
  <div class="f-status"><div class="f-dot"></div>DISPONIBLE PARA PROYECTOS</div>
</footer>

<script>
// ═══════════════════════════════════════
// LOGO BASE64
// ═══════════════════════════════════════
const LOGO_B64 = '/9j/4AAQSkZJRgABAQAAAQABAAD/4gHYSUNDX1BST0ZJTEUAAQEAAAHIAAAAAAQwAABtbnRyUkdCIFhZWiAH4AABAAEAAAAAAABhY3NwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAA9tYAAQAAAADTLQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAlkZXNjAAAA8AAAACRyWFlaAAABFAAAABRnWFlaAAABKAAAABRiWFlaAAABPAAAABR3dHB0AAABUAAAABRyVFJDAAABZAAAAChnVFJDAAABZAAAAChiVFJDAAABZAAAAChjcHJ0AAABjAAAADxtbHVjAAAAAAAAAAEAAAAMZW5VUwAAAAgAAAAcAHMAUgBHAEJYWVogAAAAAAAAb6IAADj1AAADkFhZWiAAAAAAAABimQAAt4UAABjaWFlaIAAAAAAAACSgAAAPhAAAts9YWVogAAAAAAAA9tYAAQAAAADTLXBhcmEAAAAAAAQAAAACZmYAAPKnAAANWQAAE9AAAApbAAAAAAAAAABtbHVjAAAAAAAAAAEAAAAMZW5VUwAAACAAAAAcAEcAbwBvAGcAbABlACAASQBuAGMALgAgADIAMAAxADb/2wBDAAUDBAQEAwUEBAQFBQUGBwwIBwcHBw8LCwkMEQ8SEhEPERETFhwXExQaFRERGCEYGh0dHx8fExciJCIeJBweHx7/2wBDAQUFBQcGBw4ICA4eFBEUHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh7/wAARCAQABAADASIAAhEBAxEB/8QAHQABAAEEAwEAAAAAAAAAAAAAAAgEBQYHAgMJAf/EAGgQAAEDAwIDBAQGCAwQDQQBBQEAAgMEBREGIQcSMRNBUWEIIjJxFBVCUoGRFiMzYqGx0dIkQ1NygpKTorLB0/AXGCUmNTZVY3OFlJWztMLhNDdEVmRldHWDhKOkwwlFRlTxKEdXdsT/xAAZAQEAAwEBAAAAAAAAAAAAAAAAAQIDBAX/xAAqEQEAAgICAQIGAwEBAQEAAAAAAQIDERIxIQRREyIyQWFxFDNCkYFSsf/aAAwDAQACEQMRAD8AhkiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIsy4NaHufELX9Bp22QteZCZJ3veWRxRtBLnPcAS0bYzjqQBuQgw1FfuIGlLvonV1fpu907oKujlLDno9vyXtPeCMEFWFAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREH1oLnBrQSScADvXoV6GXCsaE4ffZFdacNv1+ibI4EAugpurGDwLshzv2AIy1Rr9DThWNf8RG3e60xksFjLaioa4Atnlz9riIPUEgk7YIaRsSF6D3u5UFptdVdrtWwUVBSxmSeomfysjaO8k/V9SlWZRx9OXhQNTaSZryy04ddrNHy1rGjeel683m5h/ek74aoHr2Eljp6qndHJHFU00zC1zHAPZLG4bgjoQQfpXml6UPDGbhpxMqqOnD32evzVW6Uj9Lcd2E/OacjzxnABChMS1QiIiRERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBVtjtlbebxSWm3U8lRV1czYYYoxlz3OOAAO85PRUSmV6AvCsNhl4oXeH1nF9PaGOH7GSYfQSwdd+bbLQiJlIbgVw8ouGfDuh0zTcklUB21fO3ftqhw9Yg/NGA0bDYZxklRy9PXim9r6fhpZapzOXlqbs+NxGT+lxZHh7R67kdC1SO438QKHhrw6uGpaksdUsHY0MTt+1qHA8oxtkDBcRkZAx1K8vL9da++XqsvFzqH1NbWTOmnlecue5xySSpRCe/oUcU/s10E3St1qOe+WGFsbC53rT0ow1jvMs2afIs3JJWaektwyg4ocNam2QRsF6os1NrkIGe0A9aLJ6B4GOoHMGk7Bee3B3XVx4c8QLbqm3EuNNJ9vg5sNniOz2Hr1BIzg46gZAXqPpW+WzU2m7dqGyzCeguFO2eB4IyGkeycdHA5BHcQQhLyMqYJaaokp543RyxuLXtcMEELrUmvTp4U/Y1q0a9s0BFqvchNU1jdoKrGXZ/X7uHU5D+gAUZVC0CIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIi5RsdJI2Ngy5xAAz3lBm/A7h7ceJnEOg01QhzIXO7WtqOXIp4Gn13n8QBxkkDIyvUSwWm32Cw0VltUDaagoYGwQR9zWNGNz3nvJ7zkrUXogcLm8PeHEVdcKflvt5Y2oquZuHRRkZZHvuNvWIwDkgHdqsnpucUhozQo0raKgMvV8jLXlrvWhpujjj78+r0xgP7yFKqNXpgcVBxD4huoLVUF2nrNzQUYBHLK/Pry7fOI2+9DcgEFaRjY+SRscbS97iA1oGSSegC+Pc57y97i5zjkknJJW8PQ84az634gSXeaGN9vsMXwpwmZzRyz79jG4d7S4ZI72tcE7T00cpb+gXxWbRV0nDS9T4p6txmtUj3YbHN8qL3P6joOYY3L1oPjboy56Q1jM2vhaIq4mohkjyY38xyeU4G2fxrDbXXVVsuNPcKGeSnqqaRssMsbi17HNOQ5pG4IPQjcJ0dw9XuI+krXrnRFy0rd2D4PXRFjZOXJhkG7JB03a7BxncZB2JXlzxF0lddD6xuOmrxD2dVRylh3yHN+S4HwIwRsMgg969I/R34k0vE/htR3ntGm604FPc48AETAe3gdGvG47geYfJK1J6dvCo37TsXEGzU4dcLYwRXFrBvJBn1ZPe0nB8iCSAxERKCyIihYREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQFI30JeEQ1pq52sL5TB9hssrS2N7SW1VR1azwLW4DnAnoWggh+Ro/QWmLnrLV1u03aKcz1lbM2NjAcbZ3JODgAbk42GSdgvU/hvpC2aG0VbdL2iNvYUUQa6QN5TNIfbkIyd3HJx3DAGwClEqzWOobZpTTNx1HeJTHRUEJll3GXdzWNzsXOOGgeJC8s+LWtbjr/Xt01NcZMuqp3OjYHEtjjGzWtyBsGhoGwOAM7qRHp8cUjcLvBw5s9RmkoXCa4Pjd90nwRy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duad08WSS4v8S7Pw4t8UdTCLnqKsjL7faGv5cs6dvOf0uEHYd7yCG9CREvWep7jfrzWaq1XcRVV0zRGXhvLHDGPZghZ8iNudmjcklzsklW/Vl6kFTWXy+181yulfJ2tRUzOzLVSYx7gxowAB6rQAAFrm+Xeru1UZah2GD2Ix0aP596ibRi/a0VnJ+lRqK/VN1k5ATFSt2ZEDtjzVnRFzTMzO5dMRERqBERQkREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBZFpHWupdK1Ha2W61FO1xzJFzc0cn65hy130grHUUxOkTET2kTpDjdZ7iGU2pKM26pO3wmmaXQk+LmZ5m/sS73BbFpamjvEDrnbqqnuMTmgOqKdwecdwf8oY++UMVc9P3+8WCuZW2i4VFHO3o6N+MjwI6EeR2W1c0x2xthiekuZY6eop5qSrgZUUtRG6GeF3syRuGC0+Hke4gHuUWuJWlJ9Jakkosvlo5fttHO5uO0jJ2z3cw6EeI8FsvSPG+OfkptW25pJ2NdRtDX+98ezXeZby+4rNtWWmx8StGyU1nuNHXzRtM1DNG7EkUmN2PafWa13Q5HUNO+Fe2skeO1K7xz56RXRdtXTzUtVLTVEboponlkjHDBa4HBBC6lyuoREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBXWhvtwt9rqLdbqiSmiqsfCSw4dKBnDSevLv06HZWpE2iY2+uc5x5nOLj4kr4iIkREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBXzTWpa6yPIZ9vgPWJ52VjRTEzHSJiJ7VFwrKivqXVFTIXvcfoHkFToihIiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAqy0XS42itZW2ytqKOpj9mWGQscPHcbqjRBedWX+fUley5V0ETK90bW1E0Y5fhDgMdo4dOc95GM9cZyTZkRCI0IiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIi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vYfo7irO64OLSQ5dMUienNNphtPSHE/4inbRXRr5rMermAvkpPNoG7meLeoHTPRZJxN0Pori1pKGG6CCqinh57Zd6Uhz4gehY7vb4sO3uIBEfprhIScPI9xVforXV50PWvdbYhX2ed5fW2Z7+VrnE7ywO6RS95Hsv78H1lS+DfmO1qZteJRv4ycLdS8MdROt95pzLRSkuobhG09jUsB6g9zhkZadxkdxBOCL07nn0RxX0FJFIyG92CrPLLDKOSWmmA6H5UMzcn3g94KhD6QHBK78Na51xoHyXTS88nLTVwHrRE79lMB7Lx49HDcd4bx2rMO2t4lqJERVXEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREHVBum+v/re020npaYvxuV29HyYR8TZnE55rFcm7f4JqtVVartfhpu3WajfVzixU5kweWOFuXetI87MHv3PctsaW09pvhpZzXXm7UdFcKuMxz19a4sMzSN44It39n+xy7qe4D0txw087WrbYhUzNdeaylY8Pl7VgEbPWccxs7h5q/W/Tt8qA13xfLTtcMh9QRH+A7/gVPeeOGhNP05pbBR1NycMgCKMUsI/Bzn6gtb6l9IHVteXstFNQWaI9DTw5k+l7+Yn6wq29RWE19PaW9tMacq7ZUPu09SXNZE6N7sCKBjXEZLnvIHcNyVon0prhbrjqy3TW65UVextE1j3UszZGtcO4lu2d1rjUGqtQX+czXW51FS7u53khvu8PoVnke+R3NI9z3YxlxyVzZM3Pw6ceHhO3FERYNxVNqaH3OlaSADK3OTgdfFUyIJnXjipwwZDTUlwvsFW2npqdrmR0PwhhkbE1rshxaDhwPcrY3jtwwoGuFHTXOTHQ01spacH635CiIi1+NZj8CqVsnpK6RiHLBY784+dTDGB+1jKtt29Jq1Oafgejpqp+dn1tzLiPobEB+FRkRR8Wyfg1bK01xEoLXxLrOIFwtZu1yfVOrKankJbCydzi/mfvlwa88waOpa05Czmv9J++VLg5ulbAzlzyh0Urw365VHxFEXmFpx1luuq9JTXkm0FBpylH96tURP78OXRL6R3ERw+1TW2m/wNtph/CjK02ic7e58Ovs2hqTjxxFvun6uxVd2hZRVg5Z2wUcMLpG/NJjY3I8R0PetYEknJ3K+IqzMz2tFYjoREUJEREBERBUW6tq7dWw1tDUS01TC8PiljcWuY4HIII6EHfKyeTifxGkJL9dajcT3m5TfnLEEUxMwiYiWWO4lcQ3OLna41GSep+Mpd/wB8juJfEJwcDrbUOHDDv6oy7+/1liaJyk4x7MnpeIGtKSKaOk1Nc6btyDO6Gocx0uOnORu76V2HiTxCJ31vqL/OUv5yxRE3Jxj2ZjFxS4kRDEeu9StHldJx/tLmeK3Enm5hrnUYPlc5h/tLC0TcnGGR3zXWs75RGivOqbxcacuDjHU1j5ASOh9YnoscRFG060IiICzHh5xK1doWWU2C6ywwz8vbQOw+OTAwMtcC3ONs4yO7Cw5FMTpExE9t4M9JnXZx29FY6k97paCMk/tQF203pL6qDh2+n9NOA6ltAWk/tXhaKRW+Jb3V+HX2SKpvSjucZzNpO2y+PZyyxf7ZVVB6UNOKwVcmgKQ1OOXt21pZIG+HOGZI9+VGtFPxLI+FVJPiPxs4Z8RtKOtOpNJ3eCthY82+shqWyPpHkezktBdEXAZYfDIwcFUvB/itwq4a2pvxXp661N+kby1F1m5C4AjdsQyORviep7zjYR2RV5ynhGtJZjjPwpvtTHUajqdT1b4iXQx1NHTimpye9kTZcF33zslXBnEjgPV+q+ucwn9Vtcuf3jioeIrxmtCk4KymjSXrgvUjMF8tP/iNq4z+EKovHEfhFo23PrqOakuVXynsaeh5nPe776R4AY3z3PgCoTIrTntKIwRDYHFjipqLX9xBq5WUltgkLqShpyRFD99vu5/i92/hyjZa/RFjMzPbaIiPECIihLNuFfEW8aFujpKZ3wm3VGG1lFIT2czR0z4OGThw3HmMgyVoBY9b2g6h0bV88mxrKGTAexx8QNg7wI9V3vUNFf8AQ2rr5oy+w3mw1j6api+lr297XN6Oae8Hr78Ea48s0Y5MUXSHvLKm2ylldDLTnu7VpaD7iQre+rEjfUka79aQVytHpNUcmGXnTL2txhzqWpwCfEMe1w+jmV9g4qcGb+0G40rqGWTr8Itzc/toXk/vV2RnrLknBaGNaf1De9L34XvT1d8DrCAyeORnaQVcY/S54/lt64Ozm52IW9dB8QLBxBt1VZa2208dwmhLbhYao9tFVxDcvgJ+6sHXl2ew92wK1mLJw2vO1n1jFBI7pELg0n9pMAfwqjr+E96a2Oust/ZVsieJIZewcx0bxuHMmhc7lcD0IwptFLorNq+GE+kJwNk0uJ9U6OjnqtOk809O4881vz84/Lj7g/u6O3wXaJIIJBBBGxBU7dG6s1TSFtv11bXR1JbyxXykAmpqoHZzKljR9rcR1cRyO7wDutP8e+B9O2Go1bw/gBpQ0y1tpiJeYB1MkPe6PxbuW+bfZ474pjp148u/Eo4ovpBBIIwR1XxYtxERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAXOENMzA93K3mGT4BcEQbXu3GGuo7PS2fSFM21shp2Qy1xAdUTFoxkEjDB1wGjI+ctb3m83W810lbdbhU1lTJ7Uk0pe4jwyd8KgRWtabdq1pFehERVWEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQEREBERAREQco5JIyTG9zM7HlOFcrRqC92iUSWy61tG4d8E7oyfeWkZVrRNomNtjWvjXxHocNfqKetiG3Z1jWTtPv7QOP4Vl9i9JC90jmGv05bJw05PwV76dx+olo/arRSK8ZLR91Zx1n7NicTbvw/1ZVm96foavTlzndzVdE5gdSPd3vjc05Znvby48MdFrxww4jIOD3L4irM7WiNCIihIiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIgIiICIiAiIg//2Q==';
document.getElementById('logoImg').src = 'data:image/png;base64,' + LOGO_B64;

// ═══════════════════════════════════════
// HACK INTRO
// ═══════════════════════════════════════
const hackLines = [
  'INITIALIZING HAXX PROTOCOL...',
  'LOADING SKILL MATRIX...',
  'CONNECTING TO DEV SERVER...',
  'DEPLOYING PORTFOLIO...',
  'ACCESS GRANTED ✓'
];

let lineIdx = 0;
let charIdx = 0;
let progress = 0;
const hoText = document.getElementById('hoText');
const hoBar = document.getElementById('hoBar');
const hoLogo = document.getElementById('hoLogo');
const overlay = document.getElementById('hackOverlay');

// Show logo
setTimeout(() => { hoLogo.style.opacity = '1'; }, 200);

function typeHackLine() {
  if (lineIdx >= hackLines.length) {
    hoBar.style.width = '100%';
    setTimeout(() => { overlay.style.opacity = '0'; setTimeout(() => { overlay.style.display = 'none'; startPage(); }, 800); }, 400);
    return;
  }
  const line = hackLines[lineIdx];
  if (charIdx <= line.length) {
    hoText.textContent = line.substring(0, charIdx);
    charIdx++;
    progress = ((lineIdx / hackLines.length) + (charIdx / line.length / hackLines.length)) * 100;
    hoBar.style.width = progress + '%';
    setTimeout(typeHackLine, 40);
  } else {
    lineIdx++;
    charIdx = 0;
    setTimeout(typeHackLine, 180);
  }
}

setTimeout(typeHackLine, 600);

function skipIntro() {
  overlay.style.opacity = '0';
  setTimeout(() => { overlay.style.display = 'none'; startPage(); }, 500);
}

function startPage() {
  document.body.style.cursor = 'none';
  initParticles();
  initCounters();
}

// ═══════════════════════════════════════
// MATRIX RAIN
// ═══════════════════════════════════════
(function initMatrix() {
  const canvas = document.getElementById('matrixCanvas');
  const ctx = canvas.getContext('2d');
  let W, H, cols, drops;
  const chars = '0123456789ABCDEF<>{}[]()$#@!./\\|:;?=+-*&^%アイウエオカキクケコサシスセソ01HAXX';

  function resize() {
    W = canvas.width = window.innerWidth;
    H = canvas.height = window.innerHeight;
    cols = Math.floor(W / 18);
    drops = Array(cols).fill(0).map(() => Math.random() * H / 14);
  }
  resize();
  window.addEventListener('resize', resize);

  function draw() {
    ctx.fillStyle = 'rgba(10,15,30,0.05)';
    ctx.fillRect(0, 0, W, H);
    ctx.font = '13px Share Tech Mono, monospace';

    for (let i = 0; i < cols; i++) {
      const char = chars[Math.floor(Math.random() * chars.length)];
      const x = i * 18;
      const y = drops[i] * 14;

      // Cyan for recent, dimmer for older
      const alpha = Math.random() > 0.97 ? 1 : 0.4;
      ctx.fillStyle = Math.random() > 0.95
        ? `rgba(0,255,157,${alpha})`
        : `rgba(0,180,255,${alpha * 0.7})`;

      ctx.fillText(char, x, y);

      if (y > H && Math.random() > 0.975) drops[i] = 0;
      drops[i] += 0.5;
    }
  }
  setInterval(draw, 50);
})();

// ═══════════════════════════════════════
// CURSOR
// ═══════════════════════════════════════
const cur = document.getElementById('cur');
const ring = document.getElementById('curRing');
const trail = document.getElementById('curTrail');
let mx = 0, my = 0, rx = 0, ry = 0, tx = 0, ty = 0;

document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; });

function animCursor() {
  cur.style.left = mx + 'px'; cur.style.top = my + 'px';
  rx += (mx - rx) * 0.1; ry += (my - ry) * 0.1;
  ring.style.left = rx + 'px'; ring.style.top = ry + 'px';
  tx += (mx - tx) * 0.05; ty += (my - ty) * 0.05;
  trail.style.left = tx + 'px'; trail.style.top = ty + 'px';
  requestAnimationFrame(animCursor);
}
animCursor();

// Skill card mouse tracking
document.querySelectorAll('.skill-card').forEach(card => {
  card.addEventListener('mousemove', e => {
    const r = card.getBoundingClientRect();
    const x = ((e.clientX - r.left) / r.width) * 100;
    const y = ((e.clientY - r.top) / r.height) * 100;
    card.style.setProperty('--mx', x + '%');
    card.style.setProperty('--my', y + '%');
  });
});

// Hover cursor
document.querySelectorAll('a,button,.stag,.kc,.proj-card,.hl-card').forEach(el => {
  el.addEventListener('mouseenter', () => {
    cur.style.width = '16px'; cur.style.height = '16px'; cur.style.background = '#fff';
    ring.style.width = '52px'; ring.style.height = '52px'; ring.style.borderColor = 'rgba(0,212,255,0.9)';
  });
  el.addEventListener('mouseleave', () => {
    cur.style.width = '10px'; cur.style.height = '10px'; cur.style.background = 'var(--cyan)';
    ring.style.width = '40px'; ring.style.height = '40px'; ring.style.borderColor = 'rgba(0,212,255,0.5)';
  });
});

// ═══════════════════════════════════════
// NAV SCROLL
// ═══════════════════════════════════════
window.addEventListener('scroll', () => {
  document.getElementById('mainNav').classList.toggle('scrolled', window.scrollY > 60);
});

// ═══════════════════════════════════════
// PARTICLES
// ═══════════════════════════════════════
function initParticles() {
  const container = document.getElementById('particles');
  for (let i = 0; i < 35; i++) {
    const p = document.createElement('div');
    p.className = 'particle';
    const x = Math.random() * 100;
    const dur = 8 + Math.random() * 20;
    const dx = (Math.random() - 0.5) * 200 + 'px';
    p.style.cssText = `
      left:${x}%;bottom:-5px;
      animation-duration:${dur}s;
      animation-delay:${Math.random() * dur}s;
      --dx:${dx};
      width:${Math.random() > 0.7 ? '3' : '1.5'}px;
      height:${Math.random() > 0.7 ? '3' : '1.5'}px;
      background:${Math.random() > 0.8 ? 'var(--green)' : 'var(--cyan)'};
      box-shadow:0 0 ${4 + Math.random()*6}px currentColor;
    `;
    container.appendChild(p);
  }
}

// ═══════════════════════════════════════
// TYPING
// ═══════════════════════════════════════
const typingLines = [
  'Construyendo el futuro, una línea a la vez...',
  'Full Stack | DevOps | Web3 | AI Integration',
  'Open to remote opportunities worldwide.',
  'Sharing knowledge to elevate the dev community.',
];
let tIdx = 0, tChar = 0, tDel = false;
const typingEl = document.getElementById('typingText');

function typeLoop() {
  const line = typingLines[tIdx];
  if (!tDel) {
    tChar++;
    typingEl.innerHTML = line.substring(0, tChar) + '<span class="typing-cursor"></span>';
    if (tChar >= line.length) { tDel = true; setTimeout(typeLoop, 2500); return; }
  } else {
    tChar--;
    typingEl.innerHTML = line.substring(0, tChar) + '<span class="typing-cursor"></span>';
    if (tChar <= 0) { tDel = false; tIdx = (tIdx + 1) % typingLines.length; setTimeout(typeLoop, 300); return; }
  }
  setTimeout(typeLoop, tDel ? 25 : 55);
}

// ═══════════════════════════════════════
// COUNTERS
// ═══════════════════════════════════════
function initCounters() {
  const obs = new IntersectionObserver(entries => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        const els = e.target.querySelectorAll('[data-target]');
        els.forEach(el => {
          const target = +el.dataset.target;
          const suffix = el.parentElement.querySelector('.stat-l')?.textContent.includes('AÑOS') ? '+' : '+';
          let cur = 0;
          const step = target / 40;
          const t = setInterval(() => {
            cur = Math.min(cur + step, target);
            el.textContent = Math.floor(cur) + suffix;
            if (cur >= target) clearInterval(t);
          }, 40);
        });
        obs.unobserve(e.target);
      }
    });
  }, {threshold: 0.5});

  const statsEl = document.querySelector('.hero-stats');
  if (statsEl) obs.observe(statsEl);
}

// ═══════════════════════════════════════
// SCROLL REVEAL
// ═══════════════════════════════════════
const revObs = new IntersectionObserver(entries => {
  entries.forEach((e, i) => {
    if (e.isIntersecting) {
      setTimeout(() => e.target.classList.add('vis'), i * 70);
      revObs.unobserve(e.target);
    }
  });
}, { threshold: 0.08, rootMargin: '0px 0px -40px 0px' });

document.querySelectorAll('.reveal').forEach(el => revObs.observe(el));

// ═══════════════════════════════════════
// GLITCH RANDOM
// ═══════════════════════════════════════
function randomGlitch() {
  const hero = document.querySelector('.hero-name');
  if (hero) {
    hero.style.filter = 'blur(1px)';
    setTimeout(() => { hero.style.filter = ''; }, 80);
  }
  setTimeout(randomGlitch, 4000 + Math.random() * 8000);
}

// ═══════════════════════════════════════
// SKILL CARD 3D TILT
// ═══════════════════════════════════════
document.querySelectorAll('.proj-card, .kc, .skill-card').forEach(card => {
  card.addEventListener('mousemove', e => {
    const r = card.getBoundingClientRect();
    const x = (e.clientX - r.left) / r.width - 0.5;
    const y = (e.clientY - r.top) / r.height - 0.5;
    card.style.transform = `translateY(-6px) rotateX(${-y*6}deg) rotateY(${x*6}deg)`;
  });
  card.addEventListener('mouseleave', () => {
    card.style.transform = '';
    card.style.transition = 'transform .5s ease';
  });
  card.addEventListener('mouseenter', () => {
    card.style.transition = 'transform .1s ease';
  });
});

// ═══════════════════════════════════════
// STAG CLICK RIPPLE
// ═══════════════════════════════════════
document.querySelectorAll('.stag').forEach(tag => {
  tag.style.position = 'relative';
  tag.style.overflow = 'hidden';
  tag.addEventListener('click', e => {
    const r = tag.getBoundingClientRect();
    const rip = document.createElement('span');
    rip.style.cssText = `position:absolute;width:2px;height:2px;background:rgba(0,212,255,0.8);border-radius:50%;transform:scale(0);left:${e.clientX-r.left}px;top:${e.clientY-r.top}px;animation:ripple .5s ease;pointer-events:none;`;
    tag.appendChild(rip);
    setTimeout(() => rip.remove(), 500);
  });
});

const rippleStyle = document.createElement('style');
rippleStyle.textContent = '@keyframes ripple{to{transform:scale(60);opacity:0;}}';
document.head.appendChild(rippleStyle);

// ═══════════════════════════════════════
// START
// ═══════════════════════════════════════
window.addEventListener('load', () => {
  setTimeout(typeLoop, 500);
  setTimeout(randomGlitch, 5000);
});
</script>
</body>
</html>
