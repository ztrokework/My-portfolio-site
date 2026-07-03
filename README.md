<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ZTROKE — Piyush Das · Mobile Video Editor</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@300;400;500;700&family=DM+Mono:wght@300;400&display=swap" rel="stylesheet">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --black:  #080808;
  --dark:   #0F0F0F;
  --panel:  #141414;
  --border: #222222;
  --mid:    #555555;
  --grey:   #999999;
  --light:  #CCCCCC;
  --white:  #F5F5F5;
  --yellow: #FFD600;
  --display: 'Bebas Neue', sans-serif;
  --sans: 'Inter', sans-serif;
  --mono: 'DM Mono', monospace;
}

html { scroll-behavior: smooth; }

body {
  background: var(--black);
  color: var(--light);
  font-family: var(--sans);
  overflow-x: hidden;
  cursor: none;
}

/* CURSOR */
.cur {
  position: fixed; width: 6px; height: 6px;
  background: var(--yellow); border-radius: 50%;
  pointer-events: none; z-index: 9999;
}
.cur-ring {
  position: fixed; width: 32px; height: 32px;
  border: 1px solid rgba(255,214,0,.35); border-radius: 50%;
  pointer-events: none; z-index: 9998;
  transition: width .3s, height .3s, border-color .3s;
}
body:has(a:hover) .cur-ring,
body:has(button:hover) .cur-ring {
  width: 52px; height: 52px; border-color: var(--yellow);
}

/* NOISE */
.noise {
  position: fixed; inset: 0; pointer-events: none; z-index: 9990;
  opacity: .03;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 512 512' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  background-size: 200px;
}

/* NAV */
nav {
  position: fixed; top: 0; left: 0; right: 0; z-index: 100;
  display: flex; justify-content: space-between; align-items: center;
  padding: 22px 48px;
  transition: background .4s, border-color .4s;
  border-bottom: 1px solid transparent;
}
nav.scrolled {
  background: rgba(8,8,8,.92);
  backdrop-filter: blur(16px);
  border-bottom-color: var(--border);
}
.nav-logo {
  font-family: var(--display);
  font-size: 1.5rem;
  letter-spacing: .12em;
  color: var(--white);
}
.nav-logo span { color: var(--yellow); }
.nav-links { display: flex; gap: 36px; list-style: none; }
.nav-links a {
  font-family: var(--mono);
  font-size: .65rem;
  letter-spacing: .16em;
  color: var(--mid);
  text-decoration: none;
  text-transform: uppercase;
  transition: color .2s;
}
.nav-links a:hover { color: var(--yellow); }
.nav-cta {
  padding: 10px 24px;
  background: var(--yellow);
  font-family: var(--mono);
  font-size: .65rem;
  letter-spacing: .14em;
  text-transform: uppercase;
  text-decoration: none;
  color: var(--black);
  font-weight: 700;
  transition: opacity .2s;
}
.nav-cta:hover { opacity: .85; }

/* TICKER */
.ticker-wrap {
  position: fixed; top: 0; left: 0; right: 0; z-index: 99;
  overflow: hidden;
  height: 32px;
  display: flex; align-items: center;
  background: var(--yellow);
  /* hide behind nav, ticker sits above hero only */
}
/* Actually let's put ticker at very top above nav */

/* HERO */
.hero {
  min-height: 100vh;
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  padding: 120px 48px 80px;
  gap: 60px;
  position: relative;
  overflow: hidden;
  border-bottom: 1px solid var(--border);
}

/* BIG BG LETTERS */
.hero-bg-text {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  font-family: var(--display);
  font-size: 28vw;
  color: transparent;
  -webkit-text-stroke: 1px #1A1A1A;
  line-height: 1;
  pointer-events: none;
  user-select: none;
  letter-spacing: .06em;
  white-space: nowrap;
}

.hero-left {
  position: relative; z-index: 1;
}

.hero-eyebrow {
  display: flex; align-items: center; gap: 12px;
  font-family: var(--mono);
  font-size: .65rem;
  letter-spacing: .22em;
  color: var(--mid);
  text-transform: uppercase;
  margin-bottom: 28px;
}
.eyebrow-dot {
  width: 6px; height: 6px;
  background: var(--yellow);
  border-radius: 50%;
}

.hero-name {
  font-family: var(--display);
  font-size: clamp(5rem, 10vw, 10rem);
  line-height: .88;
  letter-spacing: .04em;
  color: var(--white);
  margin-bottom: 16px;
}
.hero-name .yellow { color: var(--yellow); }

.hero-realname {
  font-family: var(--mono);
  font-size: .78rem;
  letter-spacing: .2em;
  color: var(--grey);
  text-transform: uppercase;
  margin-bottom: 36px;
  padding-bottom: 36px;
  border-bottom: 1px solid var(--border);
}

.hero-desc {
  font-size: .95rem;
  line-height: 1.8;
  color: var(--grey);
  font-weight: 300;
  margin-bottom: 40px;
  max-width: 440px;
}
.hero-desc strong { color: var(--white); font-weight: 500; }

.hero-chips {
  display: flex; flex-wrap: wrap; gap: 8px;
  margin-bottom: 44px;
}
.chip {
  padding: 6px 14px;
  border: 1px solid var(--border);
  font-family: var(--mono);
  font-size: .58rem;
  letter-spacing: .14em;
  color: var(--grey);
  text-transform: uppercase;
}
.chip.y { border-color: var(--yellow); color: var(--yellow); }

.hero-actions { display: flex; gap: 16px; align-items: center; }
.btn-y {
  padding: 14px 32px;
  background: var(--yellow);
  font-family: var(--mono);
  font-size: .68rem;
  letter-spacing: .14em;
  text-transform: uppercase;
  text-decoration: none;
  color: var(--black);
  font-weight: 700;
  transition: opacity .2s;
}
.btn-y:hover { opacity: .85; }
.btn-ghost {
  padding: 14px 0;
  font-family: var(--mono);
  font-size: .68rem;
  letter-spacing: .14em;
  text-transform: uppercase;
  text-decoration: none;
  color: var(--mid);
  border-bottom: 1px solid var(--border);
  transition: color .2s, border-color .2s;
}
.btn-ghost:hover { color: var(--yellow); border-color: var(--yellow); }

/* HERO RIGHT — PFP */
.hero-right {
  position: relative; z-index: 1;
  display: flex; justify-content: center; align-items: flex-end;
}
.pfp-wrap {
  position: relative;
  width: 100%; max-width: 440px;
}
.pfp-img {
  width: 100%;
  aspect-ratio: 1/1;
  object-fit: cover;
  display: block;
  filter: grayscale(15%);
}
/* Yellow accent corner */
.pfp-wrap::before {
  content: '';
  position: absolute;
  top: -10px; right: -10px;
  width: 60px; height: 60px;
  border-top: 2px solid var(--yellow);
  border-right: 2px solid var(--yellow);
}
.pfp-wrap::after {
  content: '';
  position: absolute;
  bottom: -10px; left: -10px;
  width: 60px; height: 60px;
  border-bottom: 2px solid var(--yellow);
  border-left: 2px solid var(--yellow);
}
.pfp-tag {
  position: absolute;
  bottom: 20px; right: -20px;
  background: var(--yellow);
  color: var(--black);
  font-family: var(--display);
  font-size: 1rem;
  letter-spacing: .1em;
  padding: 10px 18px;
  white-space: nowrap;
}

/* SECTION SHARED */
.sec-eye {
  font-family: var(--mono);
  font-size: .63rem;
  letter-spacing: .22em;
  color: var(--yellow);
  text-transform: uppercase;
  display: flex; align-items: center; gap: 12px;
  margin-bottom: 20px;
}
.sec-eye::after {
  content: ''; flex: 1; max-width: 40px;
  height: 1px; background: var(--yellow); opacity: .4;
}

/* ABOUT */
.about-section {
  padding: 120px 48px;
  border-bottom: 1px solid var(--border);
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 80px;
  align-items: center;
}
.about-title {
  font-family: var(--display);
  font-size: clamp(3rem, 5vw, 5rem);
  letter-spacing: .05em;
  color: var(--white);
  line-height: .95;
  margin-bottom: 28px;
}
.about-title .y { color: var(--yellow); }
.about-text {
  font-size: .9rem;
  line-height: 1.85;
  color: var(--grey);
  margin-bottom: 14px;
}
.about-text strong { color: var(--white); }

.about-stats {
  display: grid;
  grid-template-columns: repeat(3,1fr);
  gap: 1px;
  background: var(--border);
  margin-top: 40px;
}
.a-stat {
  background: var(--panel);
  padding: 24px 16px;
  text-align: center;
}
.a-num {
  font-family: var(--display);
  font-size: 2.2rem;
  letter-spacing: .08em;
  color: var(--yellow);
  line-height: 1;
}
.a-lbl {
  font-family: var(--mono);
  font-size: .55rem;
  letter-spacing: .14em;
  color: var(--mid);
  text-transform: uppercase;
  margin-top: 6px;
}

/* About right: tools showcase */
.tools-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1px;
  background: var(--border);
}
.tool-block {
  background: var(--panel);
  padding: 28px 24px;
  transition: background .25s;
}
.tool-block:hover { background: #1A1A1A; }
.tool-emoji { font-size: 1.6rem; margin-bottom: 12px; display: block; }
.tool-name {
  font-family: var(--sans);
  font-size: .88rem;
  font-weight: 700;
  color: var(--white);
  margin-bottom: 6px;
}
.tool-use {
  font-family: var(--mono);
  font-size: .58rem;
  letter-spacing: .12em;
  color: var(--mid);
  text-transform: uppercase;
}

/* SKILLS */
.skills-section {
  padding: 120px 48px;
  border-bottom: 1px solid var(--border);
}
.skills-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 60px;
}
.skills-title {
  font-family: var(--display);
  font-size: clamp(3rem, 5vw, 5.5rem);
  letter-spacing: .05em;
  color: var(--white);
  line-height: .95;
}
.skills-title .y { color: var(--yellow); }
.skills-sub {
  font-family: var(--mono);
  font-size: .65rem;
  letter-spacing: .14em;
  color: var(--mid);
  text-transform: uppercase;
  max-width: 200px;
  text-align: right;
  line-height: 1.6;
}

.skills-list { display: flex; flex-direction: column; gap: 1px; }
.skill-row {
  display: grid;
  grid-template-columns: 56px 1fr auto;
  gap: 32px;
  align-items: center;
  padding: 28px 0;
  border-bottom: 1px solid var(--border);
  transition: border-color .25s;
  cursor: default;
}
.skill-row:first-child { border-top: 1px solid var(--border); }
.skill-row:hover { border-bottom-color: var(--yellow); }
.skill-row:hover .skill-row-num { color: var(--yellow); }

.skill-row-num {
  font-family: var(--display);
  font-size: 1.2rem;
  letter-spacing: .08em;
  color: var(--border);
  transition: color .25s;
}
.skill-row-body { flex: 1; }
.skill-row-name {
  font-family: var(--sans);
  font-size: 1.05rem;
  font-weight: 700;
  color: var(--white);
  margin-bottom: 6px;
}
.skill-row-desc {
  font-size: .8rem;
  line-height: 1.65;
  color: var(--mid);
}
.skill-row-tools {
  display: flex; flex-wrap: wrap; gap: 6px;
  justify-content: flex-end;
}
.s-tag {
  padding: 5px 10px;
  border: 1px solid var(--border);
  font-family: var(--mono);
  font-size: .55rem;
  letter-spacing: .12em;
  color: var(--mid);
  text-transform: uppercase;
  white-space: nowrap;
}

/* BAR SECTION */
.bar-section {
  background: var(--dark);
  padding: 80px 48px;
  border-bottom: 1px solid var(--border);
  overflow: hidden;
}
.bar-ticker {
  display: flex;
  gap: 0;
  white-space: nowrap;
  animation: barScroll 20s linear infinite;
}
.bar-item {
  display: inline-flex;
  align-items: center;
  gap: 20px;
  padding: 0 32px;
  font-family: var(--display);
  font-size: 2.5rem;
  letter-spacing: .08em;
  color: #1E1E1E;
  text-transform: uppercase;
}
.bar-item.y { color: var(--yellow); }
.bar-sep { color: #1E1E1E !important; }
@keyframes barScroll {
  0% { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}

/* PRICING */
.pricing-section {
  padding: 120px 48px;
  border-bottom: 1px solid var(--border);
}
.pricing-title {
  font-family: var(--display);
  font-size: clamp(3rem, 5.5vw, 5.5rem);
  letter-spacing: .05em;
  color: var(--white);
  line-height: .95;
  margin-bottom: 12px;
}
.pricing-title .y { color: var(--yellow); }
.pricing-sub {
  font-family: var(--mono);
  font-size: .65rem;
  letter-spacing: .16em;
  color: var(--mid);
  text-transform: uppercase;
  margin-bottom: 60px;
}

.pricing-grid {
  display: grid;
  grid-template-columns: repeat(3,1fr);
  gap: 1px;
  background: var(--border);
  margin-bottom: 28px;
}
.p-card {
  background: var(--panel);
  padding: 44px 32px;
  display: flex;
  flex-direction: column;
  position: relative;
  transition: background .25s;
}
.p-card:hover { background: #161616; }
.p-card--star {
  background: var(--yellow);
}
.p-card--star:hover { background: #ffe033; }
.p-card--star .p-tag { color: rgba(0,0,0,.5); }
.p-card--star .p-icon { filter: none; }
.p-card--star .p-name { color: var(--black); }
.p-card--star .p-limit { color: rgba(0,0,0,.5); }
.p-card--star .p-amount { color: var(--black); }
.p-card--star .p-feat li { color: rgba(0,0,0,.7); }
.p-card--star .p-feat li::before { color: var(--black); }

.p-tag {
  font-family: var(--mono);
  font-size: .58rem;
  letter-spacing: .18em;
  color: var(--mid);
  text-transform: uppercase;
  margin-bottom: 24px;
}
.p-icon { font-size: 2rem; margin-bottom: 16px; display: block; }
.p-name {
  font-family: var(--sans);
  font-size: 1.05rem;
  font-weight: 700;
  color: var(--white);
  margin-bottom: 4px;
}
.p-limit {
  font-family: var(--mono);
  font-size: .6rem;
  letter-spacing: .12em;
  color: var(--mid);
  text-transform: uppercase;
  margin-bottom: 24px;
  padding-bottom: 24px;
  border-bottom: 1px solid rgba(255,255,255,.08);
}
.p-card--star .p-limit { border-bottom-color: rgba(0,0,0,.1); }
.p-amount {
  font-family: var(--display);
  font-size: 4rem;
  letter-spacing: .04em;
  line-height: 1;
  color: var(--white);
  margin-bottom: 28px;
}
.p-amount--contact {
  font-size: 2rem;
  color: var(--yellow);
}
.p-curr {
  font-size: 2rem;
  vertical-align: super;
}
.p-feat {
  list-style: none;
  display: flex; flex-direction: column; gap: 10px;
  flex: 1;
  margin-bottom: 32px;
}
.p-feat li {
  font-size: .82rem;
  color: var(--mid);
  padding-left: 16px;
  position: relative;
  line-height: 1.5;
}
.p-feat li::before {
  content: '—';
  position: absolute; left: 0;
  color: var(--yellow);
  font-size: .7rem;
}
.p-card--star .p-feat li::before { color: var(--black); }
.p-btn {
  display: flex; align-items: center; justify-content: center;
  padding: 13px 20px;
  border: 1px solid var(--border);
  font-family: var(--mono);
  font-size: .65rem;
  letter-spacing: .14em;
  text-transform: uppercase;
  text-decoration: none;
  color: var(--mid);
  transition: border-color .25s, color .25s;
  margin-top: auto;
}
.p-btn:hover { border-color: var(--yellow); color: var(--yellow); }
.p-btn--dark {
  background: var(--black);
  color: var(--black);
  border: none;
  font-weight: 700;
}
.p-btn--dark:hover { background: #0A0A0A; color: var(--black); }

.pricing-note {
  font-family: var(--mono);
  font-size: .62rem;
  letter-spacing: .12em;
  color: var(--mid);
  text-align: center;
}
.pricing-note a { color: var(--yellow); text-decoration: none; }

/* PROCESS */
.process-section {
  padding: 120px 48px;
  border-bottom: 1px solid var(--border);
}
.process-title {
  font-family: var(--display);
  font-size: clamp(3rem, 5vw, 5.5rem);
  letter-spacing: .05em;
  color: var(--white);
  line-height: .95;
  margin-bottom: 64px;
}
.process-title .y { color: var(--yellow); }

.proc-steps { display: flex; flex-direction: column; }
.proc-step {
  display: grid;
  grid-template-columns: 52px 1fr;
  gap: 28px;
  padding: 36px 0;
  border-bottom: 1px solid var(--border);
  transition: border-color .25s;
}
.proc-step:first-child { border-top: 1px solid var(--border); }
.proc-step:hover { border-bottom-color: var(--yellow); }
.proc-n {
  font-family: var(--display);
  font-size: 1.1rem;
  letter-spacing: .08em;
  color: var(--yellow);
  padding-top: 4px;
}
.proc-name {
  font-family: var(--sans);
  font-size: 1rem;
  font-weight: 700;
  color: var(--white);
  margin-bottom: 8px;
}
.proc-desc {
  font-size: .82rem;
  line-height: 1.7;
  color: var(--mid);
}

/* INSTAGRAM */
.insta-section {
  padding: 100px 48px;
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 40px;
  background: var(--dark);
}
.insta-section h3 {
  font-family: var(--display);
  font-size: clamp(2.5rem, 4vw, 4.5rem);
  letter-spacing: .05em;
  color: var(--white);
  line-height: .95;
  margin-bottom: 12px;
}
.insta-section h3 .y { color: var(--yellow); }
.insta-section p {
  font-size: .88rem;
  color: var(--mid);
  line-height: 1.7;
  max-width: 360px;
}
.insta-btn {
  display: inline-flex;
  align-items: center;
  gap: 14px;
  padding: 18px 36px;
  border: 1px solid var(--white);
  font-family: var(--mono);
  font-size: .7rem;
  letter-spacing: .14em;
  text-transform: uppercase;
  text-decoration: none;
  color: var(--white);
  white-space: nowrap;
  transition: background .25s, color .25s, border-color .25s;
  flex-shrink: 0;
}
.insta-btn:hover { background: var(--yellow); border-color: var(--yellow); color: var(--black); }

/* CONTACT */
.contact-section {
  padding: 140px 48px 100px;
  position: relative;
  overflow: hidden;
}
.contact-bg {
  position: absolute;
  bottom: -80px; right: -40px;
  font-family: var(--display);
  font-size: 20vw;
  color: transparent;
  -webkit-text-stroke: 1px #141414;
  line-height: 1;
  pointer-events: none;
  user-select: none;
  letter-spacing: .08em;
}
.contact-inner { position: relative; z-index: 1; max-width: 680px; }
.contact-title {
  font-family: var(--display);
  font-size: clamp(4rem, 8vw, 8rem);
  letter-spacing: .05em;
  color: var(--white);
  line-height: .9;
  margin-bottom: 28px;
}
.contact-title .y { color: var(--yellow); }
.contact-p {
  font-size: .9rem;
  line-height: 1.8;
  color: var(--grey);
  margin-bottom: 48px;
  max-width: 460px;
}
.contact-email {
  display: inline-flex;
  align-items: center;
  gap: 14px;
  font-family: var(--sans);
  font-size: clamp(1rem, 2vw, 1.5rem);
  font-weight: 700;
  color: var(--white);
  text-decoration: none;
  transition: color .25s;
}
.contact-email:hover { color: var(--yellow); }
.email-arrow {
  width: 40px; height: 40px;
  border: 1px solid var(--border);
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
  transition: background .25s, border-color .25s;
}
.contact-email:hover .email-arrow { background: var(--yellow); border-color: var(--yellow); }
.email-arrow svg { width: 14px; height: 14px; stroke: var(--white); }
.contact-email:hover .email-arrow svg { stroke: var(--black); }

.divider { width: 48px; height: 1px; background: var(--border); margin: 40px 0; }
.contact-links { display: flex; gap: 28px; }
.c-link {
  font-family: var(--mono);
  font-size: .65rem;
  letter-spacing: .14em;
  color: var(--mid);
  text-decoration: none;
  text-transform: uppercase;
  transition: color .25s;
}
.c-link:hover { color: var(--yellow); }

/* FOOTER */
footer {
  background: var(--dark);
  padding: 24px 48px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid var(--border);
}
footer span {
  font-family: var(--mono);
  font-size: .58rem;
  letter-spacing: .14em;
  color: var(--mid);
  text-transform: uppercase;
}
.f-logo {
  font-family: var(--display);
  font-size: 1.1rem;
  letter-spacing: .12em;
  color: var(--white);
}
.f-logo span { color: var(--yellow); }

/* REVEAL */
.rv { opacity: 0; transform: translateY(28px); transition: opacity .7s, transform .7s; }
.rv.on { opacity: 1; transform: none; }
.rv-l { opacity: 0; transform: translateX(-28px); transition: opacity .7s, transform .7s; }
.rv-l.on { opacity: 1; transform: none; }
.rv-r { opacity: 0; transform: translateX(28px); transition: opacity .7s, transform .7s; }
.rv-r.on { opacity: 1; transform: none; }

/* MOBILE */
@media (max-width: 900px) {
  nav { padding: 18px 20px; }
  .nav-links { display: none; }
  .hero { grid-template-columns: 1fr; padding: 100px 20px 60px; gap: 48px; }
  .hero-right { order: -1; }
  .pfp-tag { right: 0; bottom: 10px; }
  .about-section { grid-template-columns: 1fr; padding: 80px 20px; gap: 48px; }
  .skills-section, .pricing-section, .process-section,
  .insta-section, .contact-section, .bar-section {
