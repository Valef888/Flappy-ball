<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
  <title>Flappy Ball ⚽️</title>
  <style>
    :root{
      --bg:#0b0d12; --panel:#121826; --text:#e7eefc; --muted:#9fb0d0;
    }
    html,body{
      height:100%; margin:0; background:var(--bg); color:var(--text);
      font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;
      overscroll-behavior:none;
    }
    .app{height:100%; display:flex; flex-direction:column;}
    .header{
      padding:12px 16px;
      background: linear-gradient(180deg, rgba(18,24,38,.95), rgba(18,24,38,.70));
      border-bottom:1px solid rgba(255,255,255,.08);
      box-shadow: 0 8px 30px rgba(0,0,0,.25);
      display:flex; align-items:center; justify-content:space-between; gap:12px;
    }
    .title{font-weight:900; letter-spacing:.3px; font-size:15px;}
    .hud{font-size:12px; color:var(--muted); text-align:right; min-width:210px;}
    .hud b{color:rgba(231,238,252,.96); font-weight:900;}
    .main{
      position:relative; flex:1; display:grid; place-items:center;
      padding:10px 10px calc(14px + env(safe-area-inset-bottom)) 10px;
    }
    canvas{
      width:min(96vw, 900px);
      height:min(84vh, 760px);
      background:
        radial-gradient(1200px 800px at 20% 10%, rgba(90,167,255,.12), transparent 55%),
        radial-gradient(900px 600px at 80% 90%, rgba(255,255,255,.06), transparent 50%),
        rgba(255,255,255,.02);
      border:1px solid rgba(255,255,255,.10);
      border-radius:22px;
      box-shadow: 0 18px 60px rgba(0,0,0,.35);
      touch-action:none;
    }
    .hint{
      position:absolute;
      left:50%; transform:translateX(-50%);
      bottom: calc(14px + env(safe-area-inset-bottom));
      max-width:min(92vw, 900px);
      font-size:12px; color:var(--muted);
      text-align:center;
      pointer-events:none;
      padding:6px 10px;
      border-radius:12px;
      background: rgba(0,0,0,.25);
      border: 1px solid rgba(255,255,255,.10);
      backdrop-filter: blur(8px);
    }
    .badge{
      display:inline-block; padding:1px 6px; border-radius:999px;
      border:1px solid rgba(255,255,255,.14);
      background:rgba(255,255,255,.05);
      margin:0 4px; color:var(--text); font-size:11px;
    }
  </style>
</head>
<body>
<div class="app">
  <div class="header">
    <div class="title">Flappy Ball <span style="opacity:.85">⚽️</span></div>
    <div class="hud" id="hud">—</div>
  </div>

  <div class="main">
    <canvas id="c" width="900" height="760" aria-label="Flappy Ball"></canvas>
    <div class="hint" id="hint"></div>
  </div>
</div>

<script>
(() => {
  "use strict";

  const clamp = (v,a,b)=>Math.max(a,Math.min(b,v));

  const Game = {
    canvas:null, ctx:null, w:0, h:0,
    state:"menu", // menu | playing | gameover
    score:0, best:0,
    t:0, last:0,
    gravity: 1700,
    flapV:   -580,
    scroll:  280,
    pipeGap: 220,
    pipeW:   92,
    pipeEvery: 1.35, // secondes
    pipes: [],
    _spawnTimer: 0,

    ball: { x:0, y:0, r:22, vy:0, rot:0 },
    input: { pressed:false, consumed:false },
    hudEl:null, hintEl:null,

    init(){
      this.canvas = document.getElementById("c");
      this.ctx = this.canvas.getContext("2d",{alpha:true});
      this.hudEl = document.getElementById("hud");
      this.hintEl = document.getElementById("hint");

      try { this.best = Number(localStorage.getItem("flappy_ball_best") || "0") || 0; } catch(_){}

      this.bindInput();
      this.onResize();
      window.addEventListener("resize", ()=>this.onResize());
      window.addEventListener("orientationchange", ()=>this.onResize());

      this.resetMenu();

      this.last = performance.now();
      requestAnimationFrame((ts)=>this.loop(ts));
    },

    bindInput(){
      const tap = () => { this.input.pressed = true; };

      this.canvas.addEventListener("pointerdown", (e)=>{ e.preventDefault(); tap(); }, {passive:false});
      window.addEventListener("keydown", (e)=>{
        if(e.code==="Space"||e.code==="ArrowUp"){ e.preventDefault(); tap(); }
        if(e.code==="KeyR"){ e.preventDefault(); this.resetMenu(); }
      }, {passive:false});

      // évite le scroll de la page au trackpad pendant qu'on joue
      window.addEventListener("wheel", (e)=>{ e.preventDefault(); }, {passive:false});
    },

    onResize(){
      const rect = this.canvas.getBoundingClientRect();
      const dpr = Math.max(1, Math.min(2.5, window.devicePixelRatio || 1));
      const w = Math.round(Math.max(1, rect.width) * dpr);
      const h = Math.round(Math.max(1, rect.height) * dpr);
      if(this.canvas.width!==w||this.canvas.height!==h){ this.canvas.width=w; this.canvas.height=h; }
      this.w = this.canvas.width;
      this.h = this.canvas.height;

      const m = Math.min(this.w,this.h);
      this.ball.r = clamp(m*0.03, 18, 28);
      this.pipeW  = clamp(m*0.12, 70, 112);
      this.pipeGap= clamp(m*0.26, 175, 270);
      this.scroll = clamp(m*0.36, 220, 350);
    },

    setHint(html){ this.hintEl.innerHTML = html; },
    updateHud(){
      const label = this.state==="menu"?"Menu":(this.state==="playing"?"Jeu":"Fin");
      this.hudEl.innerHTML = `${label} · Score: <b>${this.score}</b> · Best: <b>${this.best}</b>`;
    },

    resetMenu(){
      this.state="menu";
      this.score=0;
      this.pipes=[];
      this._spawnTimer=0;
      this.t=0;
      this.ball.x=this.w*0.32;
      this.ball.y=this.h*0.52;
      this.ball.vy=0;
      this.ball.rot=0;
      this.input.pressed=false; this.input.consumed=false;
      this.setHint(`Tap/clic/trackpad ou <span class="badge">Espace</span> pour sauter · <span class="badge">R</span> reset`);
      this.updateHud();
    },

    start(){
      this.state="playing";
      this.score=0;
      this.pipes=[];
      this._spawnTimer=0;
      this.ball.x=this.w*0.32;
      this.ball.y=this.h*0.45;
      this.ball.vy=0;
      this.ball.rot=0;
      this.spawnPipe(true);
      this.setHint(`Tap/clic/trackpad ou <span class="badge">Espace</span> pour sauter`);
      this.updateHud();
    },

    gameOver(){
      this.state="gameover";
      if(this.score>this.best){
        this.best=this.score;
        try { localStorage.setItem("flappy_ball_best", String(this.best)); } catch(_){}
      }
      this.setHint(`Perdu ! Tap/<span class="badge">Espace</span> pour rejouer · <span class="badge">R</span> menu`);
      this.updateHud();
    },

    consumeTap(){
      if(!this.input.pressed || this.input.consumed) return false;
      this.input.consumed=true;
      return true;
    },
    releaseTap(){
      this.input.pressed=false;
      this.input.consumed=false;
    },

    spawnPipe(first=false){
      const margin = Math.max(60, this.h*0.12);
      const ceilY  = Math.max(20, this.h*0.03);
      const groundY= this.h - Math.max(26, this.h*0.05);

      const top = Math.max(ceilY + 40, margin);
      const bottom = Math.min(groundY - 40, this.h - margin);
      const gap = this.pipeGap;

      const gapY = top + gap*0.5 + Math.random() * Math.max(1, (bottom - top - gap));
      const x = first ? this.w*0.85 : (this.w + this.pipeW + 10);

      this.pipes.push({ x, gapY, passed:false });
    },

    rectCircle(rx,ry,rw,rh,cx,cy,cr){
      const px = Math.max(rx, Math.min(cx, rx+rw));
      const py = Math.max(ry, Math.min(cy, ry+rh));
      const dx = cx - px, dy = cy - py;
      return dx*dx + dy*dy <= cr*cr;
    },

    loop(ts){
      const dt = clamp((ts - this.last)/1000, 0, 0.033);
      this.last = ts;

      this.update(dt);
      this.render();

      this.releaseTap();
      requestAnimationFrame((t)=>this.loop(t));
    },

    update(dt){
      this.t += dt;
      const tap = this.consumeTap();

      if(this.state==="menu"){
        this.ball.y = this.h*0.52 + Math.sin(this.t*2.2) * (this.ball.r*0.35);
        this.ball.rot = Math.sin(this.t*1.8) * 0.12;
        if(tap) this.start();
        return;
      }

      if(this.state==="gameover"){
        this.ball.vy += this.gravity*dt;
        this.ball.y  += this.ball.vy*dt;
        this.ball.rot = Math.min(1.2, this.ball.rot + 1.8*dt);
        if(tap) this.start();
        return;
      }

      // playing
      if(tap) this.ball.vy = this.flapV;

      this.ball.vy += this.gravity*dt;
      this.ball.y  += this.ball.vy*dt;

      const targetRot = clamp(this.ball.vy/600, -0.6, 1.2);
      this.ball.rot += (targetRot - this.ball.rot) * Math.min(1, 10*dt);

      // spawn pipes (timer)
      this._spawnTimer += dt;
      if(this.pipes.length===0) this.spawnPipe(true);
      while(this._spawnTimer >= this.pipeEvery){
        this._spawnTimer -= this.pipeEvery;
        this.spawnPipe(false);
      }

      const cx=this.ball.x, cy=this.ball.y, cr=this.ball.r;
      const ceilY  = Math.max(20, this.h*0.03);
      const groundY= this.h - Math.max(26, this.h*0.05);

      if(cy-cr < ceilY) { this.gameOver(); return; }
      if(cy+cr > groundY){ this.gameOver(); return; }

      for(const p of this.pipes){
        p.x -= this.scroll*dt;

        const gapTop = p.gapY - this.pipeGap*0.5;
        const gapBot = p.gapY + this.pipeGap*0.5;

        const rx=p.x, rw=this.pipeW;

        const topRect = { x:rx, y:ceilY, w:rw, h:(gapTop-ceilY) };
        const botRect = { x:rx, y:gapBot, w:rw, h:(groundY-gapBot) };

        if(topRect.h>0 && this.rectCircle(topRect.x,topRect.y,topRect.w,topRect.h,cx,cy,cr)){ this.gameOver(); return; }
        if(botRect.h>0 && this.rectCircle(botRect.x,botRect.y,botRect.w,botRect.h,cx,cy,cr)){ this.gameOver(); return; }

        const mid = p.x + rw*0.5;
        if(!p.passed && cx>mid){
          p.passed=true;
          this.score += 1;
          this.updateHud();
        }
      }

      while(this.pipes.length && this.pipes[0].x + this.pipeW < -60) this.pipes.shift();
    },

    render(){
      const ctx=this.ctx, w=this.w, h=this.h;
      ctx.clearRect(0,0,w,h);

      this.drawBackground(ctx);
      this.drawPipes(ctx);
      this.drawBall(ctx, this.ball.x, this.ball.y, this.ball.r, this.ball.rot);

      // score top
      ctx.save();
      ctx.fillStyle="rgba(231,238,252,.92)";
      ctx.font=`900 ${Math.max(18,(w*0.030)|0)}px system-ui`;
      const s=String(this.score);
      const tw=ctx.measureText(s).width;
      ctx.fillText(s,(w-tw)*0.5, Math.max(42, h*0.09));
      ctx.restore();

      if(this.state!=="playing"){
        const msg = this.state==="menu" ? "Tap pour commencer" : "Game Over";
        ctx.save();
        ctx.fillStyle="rgba(0,0,0,.30)";
        const bw=Math.min(w*0.78,520), bh=Math.min(h*0.20,150);
        const bx=(w-bw)*0.5, by=(h-bh)*0.48;
        this.roundRect(ctx,bx,by,bw,bh,18); ctx.fill();
        ctx.strokeStyle="rgba(255,255,255,.12)"; ctx.stroke();

        ctx.fillStyle="rgba(231,238,252,.96)";
        ctx.font=`900 ${Math.max(18,(w*0.032)|0)}px system-ui`;
        const t1=ctx.measureText(msg).width;
        ctx.fillText(msg,(w-t1)*0.5, by+bh*0.54);

        ctx.fillStyle="rgba(159,176,208,.95)";
        ctx.font=`${Math.max(12,(w*0.017)|0)}px system-ui`;
        const sub = this.state==="menu"
          ? "⚽️ Évite les poteaux et passe dans les trous"
          : `Score: ${this.score} · Best: ${this.best}`;
        const t2=ctx.measureText(sub).width;
        ctx.fillText(sub,(w-t2)*0.5, by+bh*0.78);
        ctx.restore();
      }
    },

    drawBackground(ctx){
      const w=this.w,h=this.h;
      const ceilY  = Math.max(20, h*0.03);
      const groundY= h - Math.max(26, h*0.05);

      ctx.save();
      const g=ctx.createLinearGradient(0,0,0,h);
      g.addColorStop(0,"rgba(90,167,255,.16)");
      g.addColorStop(0.55,"rgba(255,255,255,.02)");
      g.addColorStop(1,"rgba(0,0,0,.22)");
      ctx.fillStyle=g;
      ctx.fillRect(0,0,w,h);

      ctx.globalAlpha=0.16;
      ctx.strokeStyle="rgba(231,238,252,.35)";
      ctx.lineWidth=Math.max(1,(w*0.002)|0);
      const stripe=Math.max(28,(h*0.06)|0);
      for(let y=ceilY;y<groundY;y+=stripe){
        ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(w,y); ctx.stroke();
      }
      ctx.restore();

      ctx.save();
      ctx.fillStyle="rgba(0,0,0,.35)";
      ctx.fillRect(0,groundY,w,h-groundY);
      ctx.fillStyle="rgba(231,238,252,.10)";
      ctx.fillRect(0,groundY,w,3);
      ctx.fillStyle="rgba(231,238,252,.08)";
      ctx.fillRect(0,ceilY-2,w,2);
      ctx.restore();
    },

    drawPipes(ctx){
      const h=this.h;
      const ceilY  = Math.max(20, h*0.03);
      const groundY= h - Math.max(26, h*0.05);

      for(const p of this.pipes){
        const gapTop=p.gapY - this.pipeGap*0.5;
        const gapBot=p.gapY + this.pipeGap*0.5;

        const x=p.x, pw=this.pipeW;

        const drawPost=(x,y,hh)=>{
          if(hh<=0) return;
          ctx.save();
          ctx.fillStyle="rgba(231,238,252,.14)";
          this.roundRect(ctx,x,y,pw,hh,14); ctx.fill();
          ctx.strokeStyle="rgba(255,255,255,.16)"; ctx.stroke();

          ctx.globalAlpha=0.25;
          ctx.fillStyle="rgba(90,167,255,.55)";
          const bandH=Math.max(12, hh*0.08);
          for(let yy=y+bandH; yy<y+hh; yy+=bandH*2){
            this.roundRect(ctx,x+8,yy,pw-16,bandH,10); ctx.fill();
          }
          ctx.restore();
        };

        drawPost(x, ceilY, gapTop - ceilY);
        drawPost(x, gapBot, groundY - gapBot);

        ctx.save();
        ctx.globalAlpha=0.22;
        ctx.strokeStyle="rgba(255,255,255,.22)";
        ctx.lineWidth=2;
        ctx.beginPath(); ctx.moveTo(x+10,gapTop); ctx.lineTo(x+pw-10,gapTop); ctx.stroke();
        ctx.beginPath(); ctx.moveTo(x+10,gapBot); ctx.lineTo(x+pw-10,gapBot); ctx.stroke();
        ctx.restore();
      }
    },

    drawBall(ctx,x,y,r,rot){
      ctx.save();
      ctx.translate(x,y);
      ctx.rotate(rot);

      // ombre
      ctx.save();
      ctx.globalAlpha=0.25;
      ctx.beginPath();
      ctx.ellipse(0, r*0.95, r*0.9, r*0.28, 0, 0, Math.PI*2);
      ctx.fillStyle="rgba(0,0,0,.55)";
      ctx.fill();
      ctx.restore();

      // sphère
      const grad=ctx.createRadialGradient(-r*0.25,-r*0.35,r*0.3, 0,0,r*1.2);
      grad.addColorStop(0,"rgba(255,255,255,.98)");
      grad.addColorStop(0.55,"rgba(231,238,252,.90)");
      grad.addColorStop(1,"rgba(160,170,190,.85)");
      ctx.fillStyle=grad;
      ctx.beginPath(); ctx.arc(0,0,r,0,Math.PI*2); ctx.fill();

      // couture
      ctx.strokeStyle="rgba(0,0,0,.28)";
      ctx.lineWidth=Math.max(1.2, r*0.06);
      ctx.beginPath(); ctx.arc(0,0,r*0.72,0.2,Math.PI*1.9); ctx.stroke();

      // patch central (pentagone)
      ctx.fillStyle="rgba(0,0,0,.28)";
      this.polygon(ctx, 0, -r*0.12, r*0.34, 5); ctx.fill();

      // 5 hexagones autour
      ctx.fillStyle="rgba(0,0,0,.16)";
      for(let i=0;i<5;i++){
        const a=(Math.PI*2/5)*i + 0.3;
        this.polygon(ctx, Math.cos(a)*r*0.45, Math.sin(a)*r*0.45, r*0.24, 6);
        ctx.fill();
      }

      // highlight
      ctx.globalAlpha=0.35;
      ctx.fillStyle="rgba(255,255,255,.95)";
      ctx.beginPath();
      ctx.ellipse(-r*0.28,-r*0.36, r*0.28, r*0.18, -0.4, 0, Math.PI*2);
      ctx.fill();

      ctx.restore();
    },

    polygon(ctx,x,y,rad,sides){
      ctx.beginPath();
      for(let i=0;i<sides;i++){
        const a=(Math.PI*2/sides)*i - Math.PI/2;
        const px=x+Math.cos(a)*rad;
        const py=y+Math.sin(a)*rad;
        if(i===0) ctx.moveTo(px,py); else ctx.lineTo(px,py);
      }
      ctx.closePath();
    },

    roundRect(ctx,x,y,w,h,r){
      const rr=Math.min(r, w*0.5, h*0.5);
      ctx.beginPath();
      ctx.moveTo(x+rr,y);
      ctx.arcTo(x+w,y,x+w,y+h,rr);
      ctx.arcTo(x+w,y+h,x,y+h,rr);
      ctx.arcTo(x,y+h,x,y,rr);
      ctx.arcTo(x,y,x+w,y,rr);
      ctx.closePath();
    }
  };

  // 1 global max (optionnel, utile pour debug)
  window.FlappyBall = Game;
  Game.init();
})();
</script>
</body>
</html>
