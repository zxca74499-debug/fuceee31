<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>❤️</title>
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;1,400&display=swap" rel="stylesheet">
  <style>
    *{margin:0;padding:0;box-sizing:border-box}
    html,body{width:100%;height:100%}
    body{
      background:#080006;
      min-height:100vh;
      display:flex;flex-direction:column;
      align-items:center;justify-content:center;
      overflow:hidden;
    }
    .wrap{
      position:relative;z-index:1;
      display:flex;flex-direction:column;
      align-items:center;gap:30px;
    }
    #c{display:block;}
    .lbl{
      font-family:'Cormorant Garamond',serif;
      font-size:clamp(17px,5vw,28px);
      font-style:italic;color:#ffe0e6;
      letter-spacing:.14em;display:flex;
    }
    .lbl .ch{display:inline-block;opacity:0;transform:translateY(14px);animation:ci .5s ease forwards}
    .lbl .sp{display:inline-block;width:.28em}
    @keyframes ci{to{opacity:1;transform:translateY(0)}}
    .lbl .ch.v{animation:ci .5s ease forwards,sh 3s ease-in-out infinite}
    @keyframes sh{0%,100%{opacity:1;color:#ffe0e6}50%{opacity:.45;color:#ff9aaa}}
    .pts{position:fixed;inset:0;pointer-events:none;overflow:hidden;z-index:0}
    .pt{position:absolute;bottom:-20px;opacity:0;color:#ff2d55;animation:fu linear infinite}
    @keyframes fu{
      0%{opacity:0;transform:translateY(0) scale(.6)}
      10%{opacity:.6}90%{opacity:.1}
      100%{opacity:0;transform:translateY(-100vh) scale(1.4)}
    }
  </style>
</head>
<body>
<div class="pts" id="pts"></div>
<div class="wrap">
  <canvas id="c"></canvas>
  <div class="lbl" id="lbl"></div>
</div>
<script>
if(window.Telegram?.WebApp){
  Telegram.WebApp.ready();Telegram.WebApp.expand();
  Telegram.WebApp.setHeaderColor('#080006');
  Telegram.WebApp.setBackgroundColor('#080006');
}
 
// ── label ──
const text="я очень дорожу тобой";
const lblEl=document.getElementById('lbl');
text.split('').forEach((ch,i)=>{
  if(ch===' '){const s=document.createElement('span');s.className='sp';lblEl.appendChild(s);return}
  const span=document.createElement('span');
  span.className='ch';span.textContent=ch;
  const d=900+i*75;span.style.animationDelay=d+'ms';
  setTimeout(()=>span.classList.add('v'),d+500);
  lblEl.appendChild(span);
});
 
// ── art ──
const ART=`   +    ·                     *                      ·     +
 
               . (                          ) .
              . _ ~                        ~ _ .
              ···········             ···········
 *         ····         ····       ····         ····        *
         ···               ··     ··               ···
        ··                  ··   ··                  ··
       ··                    ·· ··                    ··
       ·                      ···                      ·
      ··                       ·                       ··
+     ·                                                 ·    +
      ·                                                 ·
      ··                                               ··
       ·                                               ·
       ··                                             ··
        ··                                           ··
  ·      ···                                       ···     ·
           ··                                     ··
            ···                                 ···
              ···                             ···
     +          ··                           ··         +
                 ···                       ···
                   ···                   ···
                     ···               ···
                       ···           ···
                         ···       ···
                           ··     ··
                            ··   ··
                             ·· ··
                              ···
                               ·`;
 
const canvas=document.getElementById('c');
const ctx=canvas.getContext('2d');
 
const vw=window.innerWidth;
const FS=Math.min(vw*0.032,14);
ctx.font=`${FS}px "Courier New",monospace`;
const CW=ctx.measureText('·').width*1.05;
const CH=FS*1.22;
 
const rows=ART.split('\n');
const COLS=Math.max(...rows.map(r=>r.length));
const ROWS=rows.length;
 
canvas.width =Math.ceil(COLS*CW);
canvas.height=Math.ceil(ROWS*CH);
 
// Build cell list
const STARS=new Set(['+','*','.','~','(',')','-','_']);
const cells=[];
rows.forEach((row,r)=>{
  [...row].forEach((ch,c)=>{
    if(ch===' ')return;
    cells.push({
      ch,
      x:c*CW, y:r*CH,
      isStar: STARS.has(ch),
      phase:(r*0.5+c*0.3),
      sparkPhase:Math.random()*Math.PI*2,
      sparkSpeed:0.5+Math.random()*2,
    });
  });
});
 
function lerp(a,b,t){
  const p=s=>[parseInt(s.slice(1,3),16),parseInt(s.slice(3,5),16),parseInt(s.slice(5,7),16)];
  const[ar,ag,ab]=p(a),[br,bg,bb]=p(b);
  return`rgb(${Math.round(ar+(br-ar)*t)},${Math.round(ag+(bg-ag)*t)},${Math.round(ab+(bb-ab)*t)})`;
}
 
let t0=null;
function draw(ts){
  if(!t0)t0=ts;
  const t=(ts-t0)/1000;
  ctx.clearRect(0,0,canvas.width,canvas.height);
  ctx.font=`${FS}px "Courier New",monospace`;
  ctx.textBaseline='top';
 
  // global pulse
  const pulse=1+0.055*Math.sin(t*1.75);
  canvas.style.transform=`scale(${pulse})`;
 
  const cc=0.5+0.5*Math.sin(t*0.55);
 
  for(const cell of cells){
    const wave=0.5+0.5*Math.sin(t*2.6-cell.phase);
    const spark=Math.pow(Math.max(0,Math.sin(t*cell.sparkSpeed*Math.PI+cell.sparkPhase)),5);
 
    if(cell.isStar){
      const bri=wave*0.3+spark*0.9;
      ctx.fillStyle=lerp('#cc2244','#ffffff',Math.min(1,bri));
      ctx.shadowColor='#ff88aa';
      ctx.shadowBlur=4+bri*18;
      const sc=0.8+bri*0.45;
      ctx.save();
      ctx.translate(cell.x+CW/2,cell.y+CH/2);
      ctx.scale(sc,sc);
      ctx.fillText(cell.ch,-CW/2,-CH/2);
      ctx.restore();
    } else {
      // dots: warm wave from deep crimson to bright rose
      const bri=wave*0.7+spark*0.4;
      const base=lerp('#ff1144','#ffaacc',cc);
      ctx.fillStyle=lerp('#3a0015',base,Math.min(1,bri+0.15));
      ctx.shadowColor='#ff3366';
      ctx.shadowBlur=bri>0.5?3+bri*12:2;
      ctx.fillText(cell.ch,cell.x,cell.y);
    }
  }
  ctx.shadowBlur=0;
  requestAnimationFrame(draw);
}
requestAnimationFrame(draw);
 
// ── particles ──
const pc=document.getElementById('pts');
const sym=['♥','❤','♡','❥','·','+'];
function spawn(){
  const el=document.createElement('span');
  el.className='pt';
  el.textContent=sym[Math.floor(Math.random()*sym.length)];
  el.style.left=Math.random()*100+'vw';
  el.style.fontSize=(7+Math.random()*12)+'px';
  const d=5+Math.random()*8;
  el.style.animationDuration=d+'s';
  el.style.animationDelay=Math.random()*2+'s';
  pc.appendChild(el);
  setTimeout(()=>el.remove(),(d+3)*1000);
}
for(let i=0;i<10;i++)setTimeout(spawn,i*300);
setInterval(spawn,650);
</script>
</body>
</html>
 
