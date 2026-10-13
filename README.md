<!DOCTYPE html>
<html lang="en"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Blockland</title>
<style>
:root{--ink:#f4efe2;--glass:rgba(22,26,18,.58);--line:rgba(255,255,255,.4);--hot:#f2c14e;--sky:#87ceeb;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media(prefers-color-scheme:dark){:root:not([data-theme="light"]){--glass:rgba(10,12,8,.68)}}
:root[data-theme="dark"]{--glass:rgba(10,12,8,.68)}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
*{box-sizing:border-box;-webkit-user-select:none;user-select:none;-webkit-touch-callout:none;-webkit-tap-highlight-color:transparent}
html,body{height:100%;margin:0;overflow:hidden;background:var(--sky);color:var(--ink);font:600 13px ui-monospace,Menlo,Consolas,monospace;touch-action:none}
canvas,#look{position:fixed;inset:0;width:100%;height:100%;display:block}
#look{z-index:1}
#vig{position:fixed;inset:0;z-index:2;pointer-events:none;background:radial-gradient(ellipse at center,transparent 58%,rgba(0,0,0,.34) 100%)}
#cross{position:fixed;left:50%;top:50%;width:20px;height:20px;margin:-10px;z-index:2;pointer-events:none;mix-blend-mode:difference}
#cross:before,#cross:after{content:"";position:absolute;background:#fff}
#cross:before{left:9px;top:0;width:2px;height:20px}
#cross:after{top:9px;left:0;height:2px;width:20px}
button{font:inherit;color:var(--ink);background:var(--glass);border:2px solid var(--line);border-radius:4px;padding:10px 12px;touch-action:none}
button:active{background:var(--hot);color:#222}
button:focus-visible{outline:3px solid var(--hot)}
button.on{background:var(--hot);color:#222;border-color:var(--hot)}
#top{position:fixed;z-index:3;left:8px;right:8px;top:calc(env(safe-area-inset-top,0px) + 8px);display:flex;justify-content:space-between;align-items:center;gap:8px;pointer-events:none}
#top>*{pointer-events:auto}
#nm{background:var(--glass);padding:8px 12px;border-radius:4px;pointer-events:none;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;min-width:0}
#hint{position:fixed;z-index:6;left:50%;top:22%;transform:translateX(-50%);width:min(86%,420px);text-align:center;line-height:1.5;background:var(--glass);padding:10px 14px;border-radius:4px;pointer-events:none;transition:opacity .6s}
#tgt{position:fixed;z-index:3;left:50%;top:calc(50% + 24px);transform:translateX(-50%);pointer-events:none;text-shadow:0 1px 2px #000;text-align:center;font-size:12px;white-space:nowrap}
#toast{position:fixed;z-index:6;left:50%;bottom:calc(env(safe-area-inset-bottom,0px) + 150px);transform:translateX(-50%);pointer-events:none;opacity:0;transition:opacity .35s;background:var(--glass);padding:6px 10px;border-radius:4px;max-width:90vw;text-align:center}
#joy{position:fixed;z-index:3;left:18px;bottom:calc(env(safe-area-inset-bottom,0px) + 88px);width:124px;height:124px;border-radius:50%;border:2px solid var(--line);background:var(--glass);touch-action:none}
#knob{position:absolute;left:36px;top:36px;width:48px;height:48px;border-radius:50%;background:rgba(255,255,255,.55);pointer-events:none}
#lbtns{position:fixed;z-index:3;left:18px;bottom:calc(env(safe-area-inset-bottom,0px) + 220px);display:flex;gap:8px}
#lbtns button{width:60px;height:38px;padding:4px}
#acts{position:fixed;z-index:3;right:14px;bottom:calc(env(safe-area-inset-bottom,0px) + 88px);display:flex;flex-direction:column;gap:10px}
#acts button{width:92px;height:52px}
#bb{border-color:var(--hot)}
#bar{position:fixed;z-index:3;left:0;right:0;bottom:env(safe-area-inset-bottom,0px);display:flex;gap:4px;padding:8px;overflow-x:auto;background:var(--glass);touch-action:pan-x;scrollbar-width:none}
#bar::-webkit-scrollbar{display:none}
.slot{flex:none;width:46px;height:46px;border:3px solid transparent;background:rgba(0,0,0,.25);position:relative}
.slot.on{border-color:var(--hot)}
.slot canvas{position:static;width:100%;height:100%;image-rendering:pixelated}
.slot i{position:absolute;right:2px;bottom:0;font:700 12px ui-monospace,monospace;font-style:normal;color:#fff;text-shadow:0 1px 2px #000}
#hp{position:fixed;z-index:3;top:calc(env(safe-area-inset-top,0px) + 54px);left:50%;transform:translateX(-50%);letter-spacing:2px;color:#e8403a;pointer-events:none;text-shadow:0 1px 2px #000}
#wet,#hurt{position:fixed;inset:0;z-index:2;pointer-events:none}
#wet{display:none;background:rgba(30,90,210,.38)}
#hurt{background:rgba(220,20,20,.45);opacity:0;transition:opacity .35s}
.rg{display:flex;gap:6px}#top button{padding:8px 10px}
#mini{position:fixed;z-index:3;left:8px;top:calc(env(safe-area-inset-top,0px) + 54px);pointer-events:none}
#mini canvas{position:static;inset:auto;width:88px;height:88px;border:2px solid var(--line);background:#000;image-rendering:pixelated;display:block}
#coords{font-size:10px;background:var(--glass);padding:2px 4px;margin-top:2px;white-space:nowrap;width:max-content}
#menu{position:fixed;inset:0;z-index:10;display:flex;align-items:center;justify-content:center;background:rgba(8,12,6,.72);padding:12px;overflow:auto;touch-action:pan-y}
#menu .card{width:min(100%,440px);background:var(--glass);border:2px solid var(--line);padding:16px;border-radius:6px;margin:auto}
#menu h1{margin:0 0 2px;font-size:22px}#menu p{margin:12px 0 6px;line-height:1.4}
#menu .row{display:flex;flex-wrap:wrap;gap:6px}
#menu .go{margin-top:14px;display:flex;gap:8px}#menu .go button{flex:1}
#menu .help{font-size:11px;opacity:.8;line-height:1.5}
#who{font-size:12px;opacity:.9;margin-bottom:4px}
input[type=range]{width:100%;accent-color:var(--hot);touch-action:pan-y}
#spawnbox{display:none}
.mrow{display:flex;gap:7px;flex-wrap:wrap;margin:8px 0}.mrow button{flex:1;min-width:110px}
#multi{position:fixed;z-index:20;inset:0;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,.72);padding:12px;touch-action:auto}
#multi .panel{width:min(94%,520px);max-height:88vh;overflow:auto;background:rgba(18,22,16,.96);border:2px solid var(--line);border-radius:8px;padding:16px}
#multi h2,#account h2{margin:0 0 8px}#multi input{font:inherit;width:100%;padding:11px;margin:5px 0;background:#111;color:#fff;border:1px solid var(--line);border-radius:4px;-webkit-user-select:text;user-select:text}
.status{padding:8px;background:rgba(255,255,255,.08);margin:8px 0;line-height:1.4;border-radius:4px}
#chat{position:fixed;z-index:4;left:10px;bottom:calc(env(safe-area-inset-bottom,0px) + 270px);width:min(320px,72vw);display:none;pointer-events:none}
#chatlog{max-height:130px;overflow:hidden;background:rgba(0,0,0,.42);padding:6px;font-size:11px;line-height:1.35}
#chatlog div{transition:opacity .6s}
#chatrow{display:flex;gap:4px;margin-top:4px;pointer-events:auto}.chatin{min-width:0;flex:1;padding:7px;border:1px solid #aaa;background:rgba(0,0,0,.7);color:#fff;font:inherit;-webkit-user-select:text;user-select:text}
#surv{font-size:11px;position:fixed;z-index:3;right:8px;top:calc(env(safe-area-inset-top,0px) + 58px);display:none;background:var(--glass);padding:7px 9px;border-radius:4px;line-height:1.5;text-shadow:0 1px 2px #000}
#craft{position:fixed;z-index:12;inset:0;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,.7);padding:12px;touch-action:auto}
#craft .panel{width:min(94%,480px);background:rgba(18,22,16,.97);border:2px solid var(--line);padding:14px;border-radius:8px}
#craft h2{margin:0 0 8px}
#tabs{display:flex;gap:6px;margin-bottom:8px;flex-wrap:wrap}#tabs button{flex:1;padding:8px 6px}
#recipes{display:grid;grid-template-columns:1fr 1fr;gap:7px;max-height:50vh;overflow:auto;touch-action:pan-y}
.recipe{text-align:left;padding:10px;touch-action:pan-y}.recipe small{display:block;opacity:.8;margin-top:4px}
.recipe.no{opacity:.45}.recipe.ok{border-color:var(--hot)}
#account{position:fixed;z-index:30;inset:0;display:none;align-items:center;justify-content:center;background:rgba(0,0,0,.8);padding:12px;overflow:auto;touch-action:pan-y}
#account .panel{width:min(94%,420px);background:rgba(18,22,16,.98);border:2px solid var(--line);padding:16px;border-radius:8px;margin:auto}
#account input[type=text],#account input[type=password]{width:100%;padding:11px;background:#111;color:#fff;border:1px solid #aaa;font:inherit;margin:6px 0;border-radius:4px;-webkit-user-select:text;user-select:text}
#account details{margin-top:10px;font-size:11px;line-height:1.5;opacity:.9}#account summary{cursor:pointer;padding:6px 0}
#authmsg{min-height:18px;color:#ffb36b;margin:6px 0 0;font-size:12px}
#gbox{display:none;margin-top:10px;text-align:center}
#players{position:fixed;z-index:4;left:10px;top:calc(env(safe-area-inset-top,0px) + 168px);background:var(--glass);padding:6px 8px;display:none;max-width:190px;font-size:11px;border-radius:4px}
#craftBtn{display:none;position:fixed;z-index:4;right:112px;bottom:calc(env(safe-area-inset-bottom,0px) + 88px);width:92px;height:52px}
</style></head><body>
<canvas id="c"></canvas><div id="look"></div><div id="vig"></div><div id="cross"></div>
<div id="top"><button id="nw">Menu</button><span id="nm"></span><span class="rg"><button id="vw">View</button><button id="fly">Fly: off</button></span></div>
<div id="hint" style="opacity:0"></div><div id="tgt"></div><div id="toast"></div>
<div id="hp"></div><div id="wet"></div><div id="hurt"></div>
<div id="mini"><canvas id="mm" width="72" height="72"></canvas><div id="coords"></div></div>
<div id="joy"><i id="knob"></i></div>
<div id="lbtns"><button id="runb">Run</button><button id="eatb">Eat</button></div>
<div id="acts"><button id="bj">Jump</button><button id="bp">Place</button><button id="bb">Break</button></div>
<div id="bar"></div>
<button id="craftBtn">Craft</button>
<div id="surv"><div id="food">Food: 20/20</div><div id="stam">Stamina: 100/100</div><div id="xp">XP: 0</div><div id="arm">Armor: none</div></div>
<div id="players"></div>
<div id="chat"><div id="chatlog"></div><div id="chatrow"><input id="chatin" class="chatin" maxlength="120" placeholder="Chat... (T)"><button id="chatsend">Send</button></div></div>

<div id="multi"><div class="panel">
<h2>Multiplayer</h2>
<div class="status" id="mstatus">Offline</div>
<input id="mname" maxlength="18" placeholder="Your player name">
<div class="mrow"><button id="host">Create Room</button><button id="join">Join Room</button></div>
<input id="room" maxlength="60" placeholder="Room code (e.g. K7M2QX)" autocapitalize="characters" autocomplete="off">
<div class="mrow"><button id="copyroom">Copy Code</button><button id="copylink">Copy Invite Link</button></div>
<div class="mrow"><button id="leave">Leave Room</button><button id="closemulti">Close</button></div>
<p style="opacity:.8;line-height:1.5;font-size:12px">Host a room, then send the 6-letter code (or invite link) to friends. Players, block edits, TNT, chat and the day/night clock are shared. Mobs run on each player's own device. Press <b>T</b> to chat on PC. Connections are peer-to-peer (WebRTC); some strict networks block it.</p>
</div></div>

<div id="craft"><div class="panel"><h2>Crafting</h2><div id="tabs"></div><div id="recipes"></div><div class="mrow"><button id="closecraft" style="flex:1">Close</button></div></div></div>

<div id="account"><div class="panel">
<h2>Player Account</h2>
<div class="status" id="acctstate">Not signed in</div>
<div class="mrow" style="margin-top:0"><button id="tabin" class="on">Sign in</button><button id="tabup">Create account</button></div>
<input id="acctname" type="text" maxlength="18" placeholder="Username" autocomplete="username" autocapitalize="off" spellcheck="false">
<input id="acctpass" type="password" maxlength="64" placeholder="Password (6+ characters)" autocomplete="current-password">
<input id="acctpass2" type="password" maxlength="64" placeholder="Confirm password" autocomplete="new-password" style="display:none">
<div class="mrow"><button id="authgo">Sign in</button><button id="guest">Play as guest</button></div>
<div id="gbox"><div id="gbtn"></div></div>
<div id="authmsg"></div>
<div class="mrow"><button id="logout" style="display:none">Log out</button><button id="closeacct">Close</button></div>
<details><summary>About accounts &amp; Google sign-in</summary>
<p>Accounts are stored on <b>this device/browser</b>: passwords are salted and hashed (PBKDF2) and each account has its own world, inventory and progress. Accounts do not follow you to other devices without a server.</p>
<p>To enable Google sign-in, create an OAuth Web Client ID in Google Cloud Console, add this page's address as an authorized JavaScript origin, and paste the Client ID below. It needs to be served over http(s), not opened as a file.</p>
<input id="gcid" type="text" placeholder="Google OAuth Client ID (optional)" autocomplete="off">
<div class="mrow"><button id="savegcid">Save Client ID</button></div>
</details>
</div></div>

<div id="menu"><div class="card"><h1>Blockland</h1><div id="who"></div><p>Mode</p><div class="row" id="modes"></div><p id="md"></p><p>Character</p><div class="row" id="chars"></div><p id="perk"></p>
<p>Render distance</p><div class="row" id="rds"></div>
<p>Graphics quality</p><div class="row" id="gqs"></div>
<p>Field of view <b id="fovv">75</b></p><input type="range" id="fov" min="60" max="105" value="75">
<div class="row" id="opts" style="margin-top:8px"></div>
<div id="spawnbox"><p>Spawn mobs <span style="opacity:.7">(Creative)</span></p><div class="row" id="spawns"></div></div>
<div class="go"><button id="play">Play</button><button id="nw2">New world</button></div><div class="go"><button id="openmulti">Multiplayer</button><button id="openacct">Account</button></div>
<p class="help">PC: WASD move, mouse look, click = attack/mine, right-click = use/place, 1-9 hotbar, E eat, C craft, F fly, V cycle view (first / third / front), Z hold to zoom, T chat, Shift run.<br>Phone: joystick + buttons. Break attacks mobs and mines blocks. Place also shoots the bow, eats food and wears armor. Breaking TNT lights it! Glowstone and lava light up the night.</p></div></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/peerjs@1.5.4/dist/peerjs.min.js"></script>
<script>
const AC=window.AudioContext||window.webkitAudioContext;let au;
function snd(f,d,t,v){try{au=au||new AC();const o=au.createOscillator(),g=au.createGain();o.type=t||"square";o.frequency.value=f;g.gain.value=.05*Math.max(.02,v==null?1:v);g.gain.exponentialRampToValueAtTime(.001,au.currentTime+d);o.connect(g);g.connect(au.destination);o.start();o.stop(au.currentTime+d)}catch(e){}}
function boom(v){try{au=au||new AC();const n=au.sampleRate*.7|0,b=au.createBuffer(1,n,au.sampleRate),d=b.getChannelData(0);for(let i=0;i<n;i++)d[i]=(Math.random()*2-1)*Math.pow(1-i/n,2);const s=au.createBufferSource(),g=au.createGain();s.buffer=b;g.gain.value=.4*Math.max(.05,v);s.connect(g);g.connect(au.destination);s.start()}catch(e){}}
const $=id=>document.getElementById(id);
let LS;try{LS=window.localStorage;LS.getItem('x')}catch(e){LS={getItem:()=>null,setItem(){},removeItem(){}}}
const jget=(k,d)=>{try{const v=JSON.parse(LS.getItem(k));return v==null?d:v}catch(e){return d}};
const coarse=!!(window.matchMedia&&matchMedia('(pointer:coarse)').matches);
let settings=Object.assign({rd:96,fov:75,gq:1,bob:true},jget('bl_settings',{}));
const saveSettings=()=>{try{LS.setItem('bl_settings',JSON.stringify(settings))}catch(e){}};

/* ---------- world: 256 x 256 x 48 ---------- */
const X=256,Y=48,Z=256,SEA=14,W=new Uint8Array(X*Y*Z),ix=(x,y,z)=>(y*Z+z)*X+x;
const HM=new Uint8Array(X*Z);let EM=[];
const blockSky=b=>b&&b!=9&&b!=10;
const get=(x,y,z)=>y<0?3:(x<0||z<0||x>=X||y>=Y||z>=Z)?0:W[ix(x,y,z)];
const emAdd=(x,y,z,s,r)=>{const k=(z>>4)*(X>>4)+(x>>4);(EM[k]||(EM[k]=[])).push([x+.5,y+.5,z+.5,s,r])};
const emDel=(x,y,z)=>{const l=EM[(z>>4)*(X>>4)+(x>>4)];if(l){const i=l.findIndex(e=>e[0]==x+.5&&e[1]==y+.5&&e[2]==z+.5);if(i>=0)l.splice(i,1)}};
const set=(x,y,z,b)=>{if(x>=0&&z>=0&&x<X&&y>=0&&y<Y&&z<Z){
  const i=ix(x,y,z),o=W[i];W[i]=b;
  if(o==16)emDel(x,y,z);if(b==16)emAdd(x,y,z,1,10);
  const k=z*X+x;if(blockSky(b)){if(y>HM[k])HM[k]=y}else if(blockSky(o)&&y==HM[k]){let t=y-1;while(t>0&&!blockSky(W[ix(x,t,z)]))t--;HM[k]=t}}};
const colTop=(x,z)=>{let y=Y-1;while(y>0&&!get(x,y,z))y--;return y};
function computeHM(){
  EM=[];
  for(let z=0;z<Z;z++)for(let x=0;x<X;x++){let y=Y-1;while(y>0&&!blockSky(W[ix(x,y,z)]))y--;HM[z*X+x]=y}
  for(let i=0;i<W.length;i++){const b=W[i];if(b!=16&&b!=19)continue;
    const x=i%X,z=((i/X)|0)%Z,y=(i/(X*Z))|0;
    if(b==16)emAdd(x,y,z,1,10);
    else if((x*7+y*3+z*5)%5==0&&(!get(x,y+1,z)||!get(x+1,y,z)||!get(x-1,y,z)||!get(x,y,z+1)||!get(x,y,z-1)))emAdd(x,y,z,.85,7)}}
const N=['','Grass','Dirt','Stone','Sand','Wood','Leaves','Planks','Cobblestone','Water','Glass','Coal ore','Iron ore','Gold ore','Diamond ore','Bricks','Glowstone','Bedrock','Snow','Lava','Cactus','TNT','Wool','Sandstone'];
const BT=[0,[0,1,2],[2,2,2],[3,3,3],[4,4,4],[6,5,6]];for(let b=6;b<18;b++)BT[b]=[b+1,b+1,b+1];BT[18]=[19,19,19];BT[19]=[20,20,20];BT[20]=[21,21,21];BT[21]=[22,22,22];BT[22]=[23,23,23];BT[23]=[24,24,24];

/* texture atlas, drawn in code: 8 x 4 tiles of 16px */
const ac=document.createElement('canvas');ac.width=128;ac.height=64;const ax=ac.getContext('2d');
const BASE=['#5fa23a','#8a5a35','#8a5a35','#808080','#e0d29a','#6b4f2a','#a98550','#2f7a2a','#b08850','#7d7d7d','#2d6cdf','#cfe8f0','#808080','#808080','#808080','#808080','#a5472f','#f4d35e','#333333','#f2f6fa','#e0541c','#2f8f3a','#c0392b','#eeeeee','#dccb8a'];
const ORE={12:'#1c1c1c',13:'#d8a67a',14:'#f5d000',15:'#4be3e3'};
const H=c=>[1,3,5].map(i=>parseInt(c.substr(i,2),16));
for(let i=0;i<25;i++)for(let y=0;y<16;y++)for(let x=0;x<16;x++){
  let [r,g,b]=H(BASE[i]),a=255,j=(.88+Math.random()*.2)*(.95+(((x>>1)*7+(y>>1)*13+i*3)%5)*.02);
  if(i==1&&y<4)[r,g,b]=H('#5fa23a');
  if(i==5)j*=x%4?1:.75;
  if(i==6)j*=Math.max(Math.abs(x-7.5),Math.abs(y-7.5))%3<1?.8:1;
  if(i==8)j*=y%4==3?.7:1;
  if(i==9)j=.65+Math.random()*.5;
  if(i==10)a=150;if(i==20){if((x*3+y*5)%7<2)[r,g,b]=H('#ffd23f');a=235}
  if(i==11)a=(x%15==0||y%15==0)?230:(x+y)%9==0?140:45;
  if(ORE[i]&&(x*7+y*13+i*5)%11<3)[r,g,b]=H(ORE[i]);
  if(i==16&&(y%4==3||(x+(y>>2)%2*4)%8==0))[r,g,b]=H('#cccccc');
  if(i==17)j=.8+Math.random()*.45;
  if(i==18)j=.5+Math.random();
  if(i==21){if(x<2||x>13)j*=.72;if(x%5==2&&y%4==1)[r,g,b]=H('#d7e8a0')}
  if(i==22){if(y>=5&&y<=10)[r,g,b]=H('#eeeeee');if(y>=6&&y<=9&&[3,4,7,8,11,12].includes(x))[r,g,b]=H('#222222');if(y==0||y==15)j*=.7}
  if(i==23){j=.92+Math.random()*.1;if((x+y)%4==0)j*=.94}
  if(i==24)j*=(y%5==4)?.8:1;
  ax.fillStyle=`rgba(${Math.min(255,r*j)|0},${Math.min(255,g*j)|0},${Math.min(255,b*j)|0},${a/255})`;ax.fillRect(x+i%8*16,y+(i>>3)*16,1,1);
}
const tex=new THREE.CanvasTexture(ac);tex.magFilter=THREE.NearestFilter;tex.minFilter=THREE.NearestMipmapLinearFilter;tex.generateMipmaps=true;tex.anisotropy=4;

/* ---------- item icons (separate 8x8 tile sheet) ---------- */
const ic=document.createElement('canvas');ic.width=128;ic.height=128;const icx=ic.getContext('2d');
const IT={};let itn=0;
function icon(id,fn){const ox=(itn%8)*16,oy=(itn>>3)*16;IT[id]=itn++;fn((x,y,c)=>{icx.fillStyle=c;icx.fillRect(ox+x,oy+y,1,1)})}
const ell=(p,cx,cy,rx,ry,c)=>{for(let y=0;y<16;y++)for(let x=0;x<16;x++){const dx=(x-cx)/rx,dy=(y-cy)/ry;if(dx*dx+dy*dy<=1)p(x,y,c)}};
const rect=(p,x0,y0,w,h,c)=>{for(let y=y0;y<y0+h;y++)for(let x=x0;x<x0+w;x++)p(x,y,c)};
const diag=(p,x0,y0,n,c)=>{for(let i=0;i<n;i++)p(x0+i,y0-i,c)};
function sword(p,bl,hi){diag(p,5,10,9,bl);diag(p,5,9,9,hi);for(let i=-2;i<=2;i++)p(5+i,10+i,'#5a4020');diag(p,2,13,3,'#6b4a22');p(1,14,'#4a3216')}
function chest(p,c,d){rect(p,2,2,4,5,c);rect(p,10,2,4,5,c);rect(p,4,5,8,9,c);rect(p,7,6,2,8,d);rect(p,4,13,8,1,d);rect(p,2,6,4,1,d);rect(p,10,6,4,1,d)}
function meat(p,base,hi){ell(p,8,8,5,4,base);ell(p,7,7,3,2,hi)}
function drum(p,base,hi){ell(p,6,6,4,3,base);ell(p,5,5,2,1,hi);diag(p,8,10,5,'#eeeeee');rect(p,12,4,2,2,'#eeeeee');rect(p,11,9,2,2,'#eeeeee')}
icon(30,p=>{diag(p,3,12,10,'#8a6030');diag(p,4,12,10,'#6a4820')});
icon(31,p=>sword(p,'#a67c52','#c49a6c'));
icon(32,p=>sword(p,'#7d7d7d','#a8a8a8'));
icon(33,p=>sword(p,'#c9ced3','#f2f4f6'));
icon(34,p=>sword(p,'#2fd6d6','#9af5f5'));
icon(35,p=>{diag(p,2,13,11,'#7a5426');rect(p,7,3,6,5,'#aeb7bf');rect(p,11,2,3,7,'#cfd6dc');p(13,9,'#cfd6dc')});
icon(36,p=>{const a=[[10,1],[11,2],[12,3],[12,4],[13,5],[13,6],[13,7],[13,8],[13,9],[12,10],[12,11],[11,12],[10,13],[9,14]];a.forEach(([x,y])=>p(x,y,'#7a5426'));a.forEach(([x,y])=>p(x-1,y,'#a0763a'));for(let y=2;y<=13;y++)p(8,y,'#e6e6e6')});
icon(37,p=>{diag(p,3,12,10,'#8b6b3a');[[13,2],[14,1],[13,3],[12,2]].forEach(([x,y])=>p(x,y,'#9aa0a6'));[[2,13],[3,14],[1,12],[2,12],[3,13],[4,14]].forEach(([x,y])=>p(x,y,'#f2f2f2'))});
icon(38,p=>{rect(p,3,3,10,10,'#8a5a2b');rect(p,3,3,10,1,'#a67a45');rect(p,3,12,10,1,'#6a4220');for(let i=0;i<4;i++)p(5+i*2,7,'#6a4220')});
icon(39,p=>chest(p,'#9a6a3a','#6a4220'));
icon(40,p=>chest(p,'#c9ced3','#8d949a'));
icon(41,p=>chest(p,'#2fd6d6','#1a8f8f'));
icon(42,p=>meat(p,'#e8909a','#f2b4bb'));
icon(43,p=>meat(p,'#9a5a32','#b9733f'));
icon(44,p=>meat(p,'#b8302a','#df6a60'));
icon(45,p=>{meat(p,'#7a4120','#9c5a2c');for(let i=0;i<4;i++)p(5+i*2,6+(i%2),'#4a2410')});
icon(46,p=>meat(p,'#cf6f7c','#e8a0aa'));
icon(47,p=>meat(p,'#8a5030','#a96a40'));
icon(48,p=>drum(p,'#f0c8a0','#fbe6cf'));
icon(49,p=>drum(p,'#c9822d','#e3a050'));
icon(50,p=>{ell(p,8,8,5,4,'#7a8a3a');[[5,7],[9,9],[7,10],[10,6],[6,9]].forEach(([x,y])=>p(x,y,'#5a6a2a'));[[8,6],[11,8]].forEach(([x,y])=>p(x,y,'#a05a3a'))});
icon(51,p=>{ell(p,8,9,5,5,'#d62d2d');p(5,7,'#f56a6a');p(5,8,'#f56a6a');rect(p,8,2,1,3,'#5a3a1a');rect(p,9,3,3,2,'#3a9a3a')});
icon(52,p=>{ell(p,8,9,5,5,'#f2c230');p(5,7,'#fff0a0');p(5,8,'#fff0a0');rect(p,8,2,1,3,'#5a3a1a');rect(p,9,3,3,2,'#3a9a3a')});
icon(53,p=>{for(let i=0;i<12;i++){p(2+i,8+Math.round(Math.sin(i*1.1)*3),'#f4f4f4');p(2+i,9+Math.round(Math.sin(i*1.1)*3),'#d0d0d0')}});
icon(54,p=>{ell(p,8,10,5,3,'#666666');ell(p,8,8,3,2,'#8a8a8a');[[5,10],[10,10],[7,7],[9,11]].forEach(([x,y])=>p(x,y,'#3c3c3c'))});
icon(55,p=>{diag(p,3,12,10,'#f4f4f4');diag(p,4,12,9,'#d8d8d8');diag(p,2,13,2,'#aaaaaa')});
function drawIcon(g,id,s){g.imageSmoothingEnabled=false;g.clearRect(0,0,s,s);if(id<30){const t=BT[id][1];g.drawImage(ac,t%8*16,(t>>3)*16,16,16,0,0,s,s)}else{const t=IT[id];g.drawImage(ic,t%8*16,(t>>3)*16,16,16,0,0,s,s)}}

/* ---------- world generation ---------- */
let sd=0,seed=0,rnd=Math.random,edits=[],st;
const mul=a=>()=>{a|=0;a=a+0x6D2B79F5|0;let t=Math.imul(a^a>>>15,1|a);t=t+Math.imul(t^t>>>7,61|t)^t;return((t^t>>>14)>>>0)/4294967296};
const hs=(x,z)=>{const n=Math.sin(x*127.1+z*311.7+sd)*43758.5453;return n-Math.floor(n)};
const vn=(x,z)=>{const i=Math.floor(x),j=Math.floor(z),u=x-i,v=z-j,a=u*u*(3-2*u),b=v*v*(3-2*v),p=hs(i,j),q=hs(i+1,j),r=hs(i,j+1),s=hs(i+1,j+1);return p+(q-p)*a+(r-p)*b+(p-q-r+s)*a*b};
function gen(s){
  seed=s;sd=s%997;rnd=mul(s);W.fill(0);
  for(let x=0;x<X;x++)for(let z=0;z<Z;z++){
    const mt=Math.max(0,vn(x/60+11,z/60+7)-.55);
    let h=Math.floor(8+vn(x/34,z/34)*12+vn(x/11,z/11)*4+vn(x/4,z/4)*1.3+mt*mt*150);
    h=Math.max(2,Math.min(h,Y-6));
    const t=vn(x/90+500,z/90+500),beach=h<=SEA+1,des=!beach&&t>.62&&h<30,snow=!beach&&!des&&(t<.34||h>=36),mtn=h>=29&&!des;
    for(let y=0;y<=Math.max(h,SEA);y++){
      let b=0;
      if(y==0)b=17;
      else if(y>h)b=9;
      else if(y<h-3){
        if(y>2&&vn(x/6+y*3.1,z/6-y*2.3)>.75)b=y<6?19:0;
        else{const r=rnd();b=r<.008&&y<9?14:r<.016&&y<13?13:r<.04?12:r<.08?11:3}
      }else if(y==h)b=(beach||des)?4:snow?18:mtn?3:1;
      else b=(beach||des)?(y>=h-1?4:23):snow?2:mtn?3:2;
      if(b&&b!=17&&y>=h-1&&y<=h&&!beach&&!des&&!snow&&!mtn&&vn(x/8+77,z/8+33)>.84)b=19;
      W[ix(x,y,z)]=b;
    }
  }
  /* trees, spruces, cacti */
  for(let x=3;x<X-3;x++)for(let z=3;z<Z-3;z++){
    const h=colTop(x,z),g=get(x,h,z);
    if(h<=SEA+1)continue;
    if(g==1){
      const m=vn(x/70+900,z/70+100);
      if(rnd()<.004+m*m*.05){
        const t=4+(rnd()*2|0);
        for(let y=1;y<=t;y++)set(x,h+y,z,5);
        for(let dx=-2;dx<=2;dx++)for(let dz=-2;dz<=2;dz++)for(let dy=t-1;dy<=t+1;dy++)
          if(Math.abs(dx)+Math.abs(dz)+(dy-t+1)<4&&!get(x+dx,h+dy,z+dz))set(x+dx,h+dy,z+dz,6);
      }
    }else if(g==18&&h<34){
      if(rnd()<.02){
        const t=5+(rnd()*3|0);
        for(let y=1;y<=t;y++)set(x,h+y,z,5);
        for(let dy=t+1;dy>=2;dy--){const k=t+1-dy,r=k==0?0:k<3?1:2;
          for(let dx=-r;dx<=r;dx++)for(let dz=-r;dz<=r;dz++)
            if(Math.abs(dx)+Math.abs(dz)<=r+(r==2?1:0)&&!get(x+dx,h+dy,z+dz))set(x+dx,h+dy,z+dz,6)}
      }
    }else if(g==4&&h>SEA+2){
      if(rnd()<.004){const c=1+(rnd()*3|0);for(let y=1;y<=c;y++)set(x,h+y,z,20)}
    }
  }
  /* wooden cabins with a bed, light and a little loot */
  for(let n=0,tries=0;n<32&&tries<500;tries++){
    const cx=10+(rnd()*(X-20)|0),cz=10+(rnd()*(Z-20)|0);
    let mn=99,mx=0,ok=true;
    for(let dx=-3;dx<=3&&ok;dx++)for(let dz=-3;dz<=3;dz++){
      const h=colTop(cx+dx,cz+dz),b=get(cx+dx,h,cz+dz);
      if(h<=SEA+1||!(b==1||b==4||b==18)){ok=false;break}
      mn=Math.min(mn,h);mx=Math.max(mx,h)}
    if(!ok||mx-mn>2)continue;
    const f=mx;
    for(let dx=-3;dx<=3;dx++)for(let dz=-3;dz<=3;dz++){
      const x=cx+dx,z=cz+dz,h0=colTop(x,z);
      for(let y=h0+1;y<=f;y++)set(x,y,z,8);
      set(x,f,z,7);
      for(let y=f+1;y<=f+5;y++)set(x,y,z,0);
      const edge=Math.abs(dx)==3||Math.abs(dz)==3;
      if(edge){
        for(let y=f+1;y<=f+3;y++){
          let b=(Math.abs(dx)==3&&Math.abs(dz)==3)?5:7;
          if(y==f+2&&((dx==0&&Math.abs(dz)==3)||(dz==0&&Math.abs(dx)==3)))b=10;
          if(dz==3&&dx==0&&y<=f+2)b=0;
          set(x,y,z,b)}
      }
      set(x,f+4,z,7);
    }
    set(cx,f+3,cz,16);
    set(cx-2,f+1,cz-2,22);set(cx-1,f+1,cz-2,22);
    set(cx+2,f+1,cz-2,12);set(cx+2,f+2,cz-2,12);set(cx+2,f+1,cz-1,11);
    n++;
  }
}

/* ---------- meshing: hidden-face culling, ambient occlusion, skylight and glow light ---------- */
const FD=[[[1,0,0],[[1,0,1],[1,0,0],[1,1,0],[1,1,1]],.8],[[-1,0,0],[[0,0,0],[0,0,1],[0,1,1],[0,1,0]],.8],[[0,1,0],[[0,1,1],[1,1,1],[1,1,0],[0,1,0]],1],[[0,-1,0],[[0,0,0],[1,0,0],[1,0,1],[0,0,1]],.5],[[0,0,1],[[0,0,1],[1,0,1],[1,1,1],[0,1,1]],.65],[[0,0,-1],[[1,0,0],[0,0,0],[0,1,0],[1,1,0]],.65]];
const op=(x,y,z)=>{const b=get(x,y,z);return b&&b!=9&&b!=10&&b!=19?1:0};
const skyAt=(x,y,z)=>{if(x<0||z<0||x>=X||z>=Z)return 1;const h=HM[z*X+x];return y>h?1:Math.max(.12,1-(h-y)*.1)};
const EOFF=[[0,0],[1,0],[-1,0],[0,1],[0,-1],[1,1],[1,-1],[-1,1],[-1,-1]];
function emFor(cx,cz){const out=[];for(const[a,b]of EOFF){const x=cx+a,z=cz+b;if(x<0||z<0||x>=X>>4||z>=Z>>4)continue;const l=EM[z*(X>>4)+x];if(l)for(const e of l){out.push(e);if(out.length>=48)return out}}return out}
function blLight(em,px,py,pz,d){let best=0;
  for(let i=0;i<em.length;i++){const e=em[i],dx=e[0]-px,dy=e[1]-py,dz=e[2]-pz,d2=dx*dx+dy*dy+dz*dz,r=e[4];if(d2>=r*r)continue;
    const l=Math.sqrt(d2)||.01,f=(dx*d[0]+dy*d[1]+dz*d[2])/l,fac=f>0?.4+.6*f:.12,t=1-l/r,v=t*t*e[3]*fac*1.5;if(v>best)best=v}
  return Math.min(1,best)}
function build(cx,cz){
  const S={p:[],u:[],l:[],i:[]},T={p:[],u:[],l:[],i:[]},em=emFor(cx,cz),hasE=em.length>0;
  for(let y=0;y<Y;y++)for(let z=cz*16;z<cz*16+16;z++)for(let x=cx*16;x<cx*16+16;x++){
    const b=W[ix(x,y,z)];if(!b)continue;
    const tr=b==9||b==10||b==19,o=tr?T:S,kind=b==9?1:b==19?2:b==6?4:0,wtop=b==9&&get(x,y+1,z)!=9;
    for(let f=0;f<6;f++){
      const [d,cs,sh]=FD[f],n=get(x+d[0],y+d[1],z+d[2]);
      if(tr?(n==0||(b==10&&n==9)):(n==0||n==9||n==10||n==19)){
        const t=BT[b][f==2?0:f==3?2:1],c0=t%8/8,r0=1-((t>>3)+1)/4,e=.0008,k=o.p.length/3;
        const na=d[0]?0:d[1]?1:2,a1=(na+1)%3,a2=(na+2)%3,q=[x+d[0],y+d[1],z+d[2]],ao=[],sq=skyAt(q[0],q[1],q[2]);
        cs.forEach((v,j)=>{let m=sh,sk=sq,bl=0;
          const px=x+v[0],py=y+v[1]-(wtop&&v[1]==1?.12:0),pz=z+v[2];
          if(!tr&&b!=16){
            const s1=v[a1]?1:-1,s2=v[a2]?1:-1,p1=q.slice(),p2=q.slice(),p3=q.slice();p1[a1]+=s1;p2[a2]+=s2;p3[a1]+=s1;p3[a2]+=s2;
            const A=op(p1[0],p1[1],p1[2]),B=op(p2[0],p2[1],p2[2]),C=op(p3[0],p3[1],p3[2]),a=A&&B?0:3-A-B-C;ao[j]=a;m=sh*(.55+.15*a);
            let ss=sq,cn=1;if(!A){ss+=skyAt(p1[0],p1[1],p1[2]);cn++}if(!B){ss+=skyAt(p2[0],p2[1],p2[2]);cn++}if(!C&&!(A&&B)){ss+=skyAt(p3[0],p3[1],p3[2]);cn++}
            sk=ss/cn
          }else ao[j]=3;
          if(b==16){m=1;sk=1;bl=1}
          else if(b==19){m=1;sk=1;bl=1}
          else if(hasE)bl=blLight(em,px+d[0]*.25,py+d[1]*.25,pz+d[2]*.25,d);
          o.p.push(px,py,pz);o.u.push(j==0||j==3?c0+e:c0+.125-e,j<2?r0+e:r0+.25-e);o.l.push(m,sk,bl,kind)});
        if(ao[1]+ao[3]>ao[0]+ao[2])o.i.push(k+1,k+2,k+3,k+1,k+3,k);else o.i.push(k,k+1,k+2,k,k+2,k+3);
      }
    }
  }
  return[S,T];
}
const mk=o=>{const g=new THREE.BufferGeometry();g.setAttribute('position',new THREE.Float32BufferAttribute(o.p,3));g.setAttribute('uv',new THREE.Float32BufferAttribute(o.u,2));g.setAttribute('lt',new THREE.Float32BufferAttribute(o.l,4));g.setIndex(o.i);return g};

/* ---------- scene ---------- */
const ren=new THREE.WebGLRenderer({canvas:$('c'),antialias:!coarse,powerPreference:'high-performance'});
const GQ=[['Low',1],['Medium',1.5],['High',2]];
const scene=new THREE.Scene();scene.fog=new THREE.Fog(0x8fd0f0,33,96);
const cam=new THREE.PerspectiveCamera(settings.fov,1,.05,130);cam.rotation.order='YXZ';scene.add(cam);
let RD=96,fovC=settings.fov;
function setRD(r){RD=r;cam.far=Math.max(r+30,130);cam.updateProjectionMatrix()}
/* chunk shader: per-vertex shade/AO, skylight, glow light; day/night, fog, water ripple, lava pulse, leaf sway */
const U={map:{value:tex},uDay:{value:1},uRain:{value:0},uTime:{value:0},uFogNear:{value:30},uFogFar:{value:96},uFog:{value:new THREE.Color(0x8fd0f0)}};
const VS=`attribute vec4 lt;uniform float uTime;varying vec2 vUv;varying vec4 vLt;varying float vDepth;varying vec3 vWp;
void main(){vec3 p=position;
 if(lt.w>3.5){p.x+=sin(uTime*1.3+position.x*.9+position.z*.7)*.035;p.z+=cos(uTime*1.1+position.z*.8+position.y*.6)*.035;}
 vUv=uv;vLt=lt;vWp=p;vec4 mv=modelViewMatrix*vec4(p,1.);vDepth=length(mv.xyz);gl_Position=projectionMatrix*mv;}`;
const FS=`uniform sampler2D map;uniform float uDay,uRain,uTime,uFogNear,uFogFar;uniform vec3 uFog;
varying vec2 vUv;varying vec4 vLt;varying float vDepth;varying vec3 vWp;
void main(){vec4 t=texture2D(map,vUv);if(t.a<.02)discard;
 float kind=vLt.w;vec3 c=t.rgb;float a=t.a;
 float sky=vLt.y*(.22+.78*uDay)*(1.-.2*uRain);
 vec3 L=vec3(sky)+vec3(1.,.78,.45)*vLt.z*1.1;
 L=clamp(L,vec3(.07,.075,.1),vec3(1.35));
 if(kind>.5&&kind<1.5){float w=sin(vWp.x*1.9+uTime*1.5)*sin(vWp.z*2.3+uTime*1.2)+sin((vWp.x+vWp.z)*3.1+uTime*2.1)*.5;c*=1.+w*.07;a=min(1.,t.a*1.15);}
 if(kind>1.5&&kind<2.5){c*=1.12+.2*sin(uTime*2.+vWp.x*2.+vWp.z*2.);L=vec3(1.);}
 vec3 col=c*vLt.x*L;
 col=mix(col,col*vec3(.78,.88,1.15),(1.-uDay)*.5*(1.-clamp(vLt.z*2.,0.,1.)));
 col=mix(col,uFog,smoothstep(uFogNear,uFogFar,vDepth));
 gl_FragColor=vec4(col,a);}`;
const matS=new THREE.ShaderMaterial({uniforms:U,vertexShader:VS,fragmentShader:FS});
const matT=new THREE.ShaderMaterial({uniforms:U,vertexShader:VS,fragmentShader:FS,transparent:true,side:THREE.DoubleSide});
const NCH=X>>4,CH=[];for(let i=0;i<NCH*NCH;i++){const a=new THREE.Mesh(new THREE.BufferGeometry(),matS),c=new THREE.Mesh(new THREE.BufferGeometry(),matT);scene.add(a);scene.add(c);CH.push([a,c])}
const hl=new THREE.LineSegments(new THREE.EdgesGeometry(new THREE.BoxGeometry(1.004,1.004,1.004)),new THREE.LineBasicMaterial({color:0}));scene.add(hl);
/* lights for entities (mobs, players) */
const amb=new THREE.AmbientLight(0xffffff,.6),sunL=new THREE.DirectionalLight(0xfff2d8,.6);scene.add(amb);scene.add(sunL);
/* sky dome with horizon gradient and sun/moon glow */
const skyMat=new THREE.ShaderMaterial({uniforms:{uTop:{value:new THREE.Color()},uHor:{value:new THREE.Color()},uGlow:{value:new THREE.Color(1,.8,.5)},uSun:{value:new THREE.Vector3(0,1,0)},uGI:{value:1}},side:THREE.BackSide,depthWrite:false,depthTest:false,fog:false,
 vertexShader:`varying vec3 vP;void main(){vP=position;gl_Position=projectionMatrix*modelViewMatrix*vec4(position,1.);}`,
 fragmentShader:`uniform vec3 uTop,uHor,uGlow,uSun;uniform float uGI;varying vec3 vP;
 void main(){vec3 d=normalize(vP);float h=d.y;vec3 c=mix(uHor,uTop,pow(clamp(h,0.,1.),.55));float g=max(dot(d,uSun),0.);c+=uGlow*(pow(g,5.)*.22+pow(g,48.)*.55)*uGI;gl_FragColor=vec4(c,1.);}`});
const dome=new THREE.Mesh(new THREE.SphereGeometry(100,24,12),skyMat);dome.renderOrder=-100;dome.frustumCulled=false;scene.add(dome);
const sunM=new THREE.Mesh(new THREE.PlaneGeometry(16,16),new THREE.MeshBasicMaterial({color:0xfff2a8,fog:false})),moonM=new THREE.Mesh(new THREE.PlaneGeometry(11,11),new THREE.MeshBasicMaterial({color:0xdfe6f0,fog:false}));scene.add(sunM);scene.add(moonM);
const sp=[];for(let i=0;i<300;i++){const a=Math.random()*6.28,b=Math.acos(Math.random());sp.push(100*Math.sin(b)*Math.cos(a),100*Math.cos(b),100*Math.sin(b)*Math.sin(a))}
const sg=new THREE.BufferGeometry();sg.setAttribute('position',new THREE.Float32BufferAttribute(sp,3));
const stars=new THREE.Points(sg,new THREE.PointsMaterial({size:2,sizeAttenuation:false,fog:false,transparent:true,color:0xffffff}));scene.add(stars);
const cm=new THREE.MeshBasicMaterial({color:0xffffff,transparent:true,opacity:.85}),clouds=[];
for(let i=0;i<34;i++){const g=new THREE.Group(),n=3+(Math.random()*4|0);
  for(let k=0;k<n;k++){const c=new THREE.Mesh(new THREE.BoxGeometry(7+Math.random()*10,1.6+Math.random(),5+Math.random()*8),cm);c.position.set((Math.random()-.5)*14,(Math.random()-.3)*1.2,(Math.random()-.5)*10);g.add(c)}
  g.position.set(Math.random()*340-40,Y+14+Math.random()*4,Math.random()*Z);scene.add(g);clouds.push(g)}
/* rain */
const RN=320,rpos=new Float32Array(RN*6),rq=[];for(let i=0;i<RN;i++)rq.push([Math.random()*30-15,Math.random()*18,Math.random()*30-15]);
const rainG=new THREE.BufferGeometry();rainG.setAttribute('position',new THREE.BufferAttribute(rpos,3));
const rainM=new THREE.LineBasicMaterial({color:0xa8c0ff,transparent:true,opacity:0,fog:false}),rainL=new THREE.LineSegments(rainG,rainM);rainL.frustumCulled=false;rainL.visible=false;scene.add(rainL);
/* particles */
const pg=new THREE.BoxGeometry(.12,.12,.12),parts=[];
function burstC(x,y,z,col,n,spd){const c=new THREE.Color(col);for(let i=0;i<n;i++){const m=new THREE.Mesh(pg,new THREE.MeshBasicMaterial({color:c}));m.position.set(x+.2+Math.random()*.6,y+.2+Math.random()*.6,z+.2+Math.random()*.6);scene.add(m);parts.push({m,vx:(Math.random()*4-2)*spd,vy:(Math.random()*4+1)*spd,vz:(Math.random()*4-2)*spd,t:.7})}}
function burst(x,y,z,b){burstC(x,y,z,BASE[BT[b][1]],8,1)}
/* chunk build queue */
let bq=[],dirty=new Set();
function rebuild(i){for(const k of i===undefined?[...CH.keys()]:[i]){const[S,T]=build(k%NCH,k/NCH|0);CH[k][0].geometry.dispose();CH[k][0].geometry=mk(S);CH[k][1].geometry.dispose();CH[k][1].geometry=mk(T)}}
function mark(x,z,r){r=r||2;const off=[];for(let a=-r;a<r;a+=8)off.push(a);off.push(r);
  for(const a of off)for(const b of off){const cx=(x+a)>>4,cz=(z+b)>>4;if(cx>=0&&cx<NCH&&cz>=0&&cz<NCH)dirty.add(cz*NCH+cx)}}
const markB=(x,z,b0,b1)=>mark(x,z,(b0==16||b1==16)?10:2);
function cullChunks(){const lim=RD+18;for(let k=0;k<CH.length;k++){const cx=(k%NCH)*16+8,cz=((k/NCH)|0)*16+8,v=Math.hypot(cx-P.x,cz-P.z)<lim;CH[k][0].visible=CH[k][1].visible=v}}
function queueAll(sync){
  CH.forEach(c=>{c[0].geometry.dispose();c[0].geometry=new THREE.BufferGeometry();c[1].geometry.dispose();c[1].geometry=new THREE.BufferGeometry()});
  const pcx=P.x>>4,pcz=P.z>>4,d2=k=>(k%NCH-pcx)**2+((k/NCH|0)-pcz)**2;
  bq=[...CH.keys()].sort((a,b)=>d2(a)-d2(b));dirty.clear();
  if(sync)for(let i=0;i<9&&bq.length;i++)rebuild(bq.shift());
  cullChunks()}
function rs(){const w=innerWidth,h=innerHeight;ren.setSize(w,h,false);cam.aspect=w/h;cam.updateProjectionMatrix()}
addEventListener('resize',rs);
function setGQ(i){settings.gq=i;ren.setPixelRatio(Math.min(window.devicePixelRatio||1,GQ[i][1]));rs();markGQ();saveSettings()}
/* first-person held item */
const hc=document.createElement('canvas');hc.width=hc.height=32;const hx=hc.getContext('2d'),handTex=new THREE.CanvasTexture(hc);handTex.magFilter=handTex.minFilter=THREE.NearestFilter;handTex.generateMipmaps=false;
const hand=new THREE.Mesh(new THREE.PlaneGeometry(.55,.55),new THREE.MeshBasicMaterial({map:handTex,transparent:true,depthTest:false,fog:false,side:THREE.DoubleSide}));
hand.renderOrder=999;hand.position.set(.42,-.4,-.8);cam.add(hand);
/* primed TNT texture */
const tc=document.createElement('canvas');tc.width=tc.height=16;tc.getContext('2d').drawImage(ac,22%8*16,(22>>3)*16,16,16,0,0,16,16);
const tntTex=new THREE.CanvasTexture(tc);tntTex.magFilter=tntTex.minFilter=THREE.NearestFilter;tntTex.generateMipmaps=false;

/* ---------- player & physics ---------- */
const P={x:X/2,y:30,z:Z/2,vy:0,fly:false,ground:false,hw:.3,h:1.75};
let yaw=0,pitch=0,jx=0,jy=0,jump=false,sel=1,time=1,last=performance.now();const keys={};
const solid=(x,y,z)=>{if(x<0||z<0||x>=X||z>=Z||y<0)return true;if(y>=Y)return false;const b=W[ix(x,y,z)];return b&&b!=9&&b!=19};
function hit(E=P){for(let x=Math.floor(E.x-E.hw);x<=Math.floor(E.x+E.hw);x++)for(let z=Math.floor(E.z-E.hw);z<=Math.floor(E.z+E.hw);z++)for(let y=Math.floor(E.y);y<=Math.floor(E.y+E.h);y++)if(solid(x,y,z))return true;return false}
function mv(a,d,E=P){const n=Math.ceil(Math.abs(d)/.2)||1,s=d/n;for(let i=0;i<n;i++){E[a]+=s;if(hit(E)){E[a]-=s;return true}}return false}
function spawn(){
  for(let i=0;i<800;i++){const x=X/2-30+Math.random()*60|0,z=Z/2-30+Math.random()*60|0,h=colTop(x,z),b=get(x,h,z);
    if(h>SEA&&(b==1||b==4||b==18)&&!get(x,h+1,z)&&!get(x,h+2,z)){P.x=x+.5;P.z=z+.5;P.y=h+1.01;P.vy=0;return}}
  for(let i=0;i<4000;i++){const x=Math.random()*X|0,z=Math.random()*Z|0,h=colTop(x,z),b=get(x,h,z);
    if(h>SEA&&(b==1||b==4||b==18||b==3)&&!get(x,h+1,z)&&!get(x,h+2,z)){P.x=x+.5;P.z=z+.5;P.y=h+1.01;P.vy=0;return}}
  P.x=X/2;P.z=Z/2;P.y=colTop(X/2,Z/2)+1.01;P.vy=0}
/* ---------- state ---------- */
let lum=1,zt=0,wet=0,hp=10,lc=0,mode=0,viewMode=0,wt=0,inv={},ci=0,paused=true,plr=null,legs=[];
let food=20,stamina=100,xp=0,armorId=0,fc=0,hh=0,sv2=0,ct=0,atkT=0,bowT=0,hurtT=0,swing=0,nt=0,tsync=0,pgt=0,rain=0,runT=false,RAY=null,wasNight=false,fr=0,lastTgt='';
let camD=4,bobT=0,bobA=0,handBob=0,ssG=0;
let username=LS.getItem('blockland_name')||'Player';
const mobs=[],arrows=[],tnts=[];
const day=()=>Math.floor(time/6.2832)+1;
const lv=()=>Math.floor(Math.sqrt(xp/6));
let toastT=0;
function toast(m){const e=$('toast');e.textContent=m;e.style.opacity=1;clearTimeout(toastT);toastT=setTimeout(()=>e.style.opacity=0,1800)}
function msg(m,ms){const h=$('hint');h.textContent=m;h.style.opacity=1;setTimeout(()=>h.style.opacity=0,ms||4000)}

/* ---------- items ---------- */
const IN={30:'Stick',31:'Wooden Sword',32:'Stone Sword',33:'Iron Sword',34:'Diamond Sword',35:'Battle Axe',36:'Bow',37:'Arrow',38:'Leather',39:'Leather Armor',40:'Iron Armor',41:'Diamond Armor',42:'Raw Pork',43:'Cooked Pork',44:'Raw Beef',45:'Steak',46:'Raw Mutton',47:'Cooked Mutton',48:'Raw Chicken',49:'Cooked Chicken',50:'Rotten Flesh',51:'Apple',52:'Golden Apple',53:'String',54:'Gunpowder',55:'Feather'};
for(const k in IN)N[k]=IN[k];
const WS={0:{d:1,cd:.35,r:3.5,kb:6},31:{d:3,cd:.4,r:4,kb:6},32:{d:4,cd:.4,r:4,kb:6},33:{d:5,cd:.4,r:4,kb:7},34:{d:7,cd:.4,r:4,kb:7},35:{d:6,cd:.8,r:3.8,kb:11}};
const FOOD={42:3,43:8,44:3,45:8,46:3,47:8,48:2,49:6,50:2,51:4,52:10};
const ARMP={39:{n:'Leather',a:1},40:{n:'Iron',a:2},41:{n:'Diamond',a:3}};
const BLK=[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,18,19,20,21,22,23];
const WPN=[31,32,33,34,35,36,37],OTH=[30,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55];
const ALL=[...WPN,...BLK,...OTH],CRW=[31,32,33,34,35,36];
const vis=()=>ALL.filter(id=>id<30?true:mode===0?CRW.includes(id):(inv[id]||0)>0);
function addItem(id,n,quiet){inv[id]=(inv[id]||0)+n;if(!quiet)toast('+'+n+' '+N[id]);badges()}
const R=(cat,need,out)=>({cat,need,out});
const recipes=[
 R(0,{5:1},{7:4}),R(0,{7:2},{30:4}),R(0,{3:1},{8:4}),R(0,{4:2},{10:2}),R(0,{3:2},{15:4}),R(0,{4:4},{23:4}),R(0,{11:1,13:1},{16:2}),R(0,{54:3,4:2},{21:1}),
 R(1,{7:2,30:1},{31:1}),R(1,{8:2,30:1},{32:1}),R(1,{12:2,30:1},{33:1}),R(1,{14:2,30:1},{34:1}),R(1,{12:3,30:2},{35:1}),R(1,{30:3,53:3},{36:1}),R(1,{30:1,55:1,8:1},{37:4}),
 R(2,{38:5},{39:1}),R(2,{12:6},{40:1}),R(2,{14:5},{41:1}),
 R(3,{42:1,5:1},{43:1}),R(3,{44:1,5:1},{45:1}),R(3,{46:1,5:1},{47:1}),R(3,{48:1,5:1},{49:1}),R(3,{51:1,13:2},{52:1})
];
const TABS=['Basics','Weapons','Armor','Food'];let ctab=0;
const rname=r=>{const[id,n]=Object.entries(r.out)[0];return N[id]+(n>1?' x'+n:'')};
function canCraft(r){return Object.entries(r.need).every(([b,n])=>(inv[b]||0)>=n)}
function craft(r){
  if(!canCraft(r)){toast('Missing materials');return}
  for(const[b,n]of Object.entries(r.need))inv[b]-=n;
  for(const[b,n]of Object.entries(r.out))inv[b]=(inv[b]||0)+n;
  addXP(2);badges();save();renderRecipes();toast('Crafted '+rname(r));
  const o=+Object.keys(r.out)[0];if(CRW.includes(o)||(o>=31&&o<=36))pick(o)}
function renderRecipes(){
  const box=$('recipes');box.innerHTML='';
  const tb=$('tabs');tb.innerHTML='';
  TABS.forEach((t,i)=>{const b=document.createElement('button');b.textContent=t;b.className=i==ctab?'on':'';b.onclick=()=>{ctab=i;renderRecipes()};tb.appendChild(b)});
  recipes.filter(r=>r.cat==ctab).forEach(r=>{
    const b=document.createElement('button'),ok=canCraft(r);b.className='recipe '+(ok?'ok':'no');b.type='button';
    b.innerHTML='<b>'+rname(r)+'</b><small>Need: '+Object.entries(r.need).map(([id,n])=>n+' '+N[id]).join(', ')+'</small>';
    b.onclick=()=>craft(r);box.appendChild(b)})}

/* ---------- XP / damage / death ---------- */
const drawHp=()=>{$('hp').textContent='♥'.repeat(Math.max(0,hp))+'♡'.repeat(Math.max(0,10-hp))};
function addXP(n){const o=lv();xp+=n;if(lv()>o&&mode){hp=Math.min(10,hp+3);drawHp();toast('Level up! Level '+lv())}uiNow()}
function hurt(n,dx,dz){
  if(!mode||hurtT>0)return;
  const ap=(armorId?ARMP[armorId].a:0)+C.ar;
  n=Math.max(1,Math.floor(n*(1-Math.min(.7,ap*.14))+1e-6));
  hp-=n;hurtT=.45;P.vy=5;mv('x',dx*.8);mv('z',dz*.8);snd(70,.25,"sawtooth");
  const e=$('hurt');e.style.opacity=1;setTimeout(()=>e.style.opacity=0,120);
  if(hp<=0)die();drawHp()}
function die(){
  hp=10;food=20;stamina=100;spawn();clearHostile();clearProjectiles();hurtT=1;
  time=Math.ceil((time-1)/6.2832)*6.2832+1;
  msg('You were knocked out. Back at spawn, it is morning again.')}

/* ---------- mob models (lit, with blob shadows) ---------- */
const MM={},mm=c=>MM[c]||(MM[c]=new THREE.MeshLambertMaterial({color:c}));
const flashM=new THREE.MeshBasicMaterial({color:0xff4040}),whiteM=new THREE.MeshBasicMaterial({color:0xffffff});
const shG=new THREE.CircleGeometry(.5,14),shM=new THREE.MeshBasicMaterial({color:0x000000,transparent:true,opacity:.3,depthWrite:false});
function addShadow(g,r){const s=new THREE.Mesh(shG,shM);s.rotation.x=-Math.PI/2;s.position.y=.03;s.scale.setScalar(r);s.userData.noflash=1;g.add(s)}
function bx(g,w,h,d,c,x,y,z){const q=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),mm(c));q.position.set(x,y,z);g.add(q);return q}
function lb(g,w,h,d,c,x,y,z){const geo=new THREE.BoxGeometry(w,h,d);geo.translate(0,-h/2,0);const q=new THREE.Mesh(geo,mm(c));q.position.set(x,y,z);g.add(q);return q}
function mPig(g,o){const c='#f0a0a8',d='#d98890';
  bx(g,.6,.5,.95,c,0,.55,0);bx(g,.46,.42,.4,c,0,.66,-.62);bx(g,.22,.16,.1,d,0,.6,-.84);bx(g,.07,.07,.02,'#111111',-.12,.72,-.83);bx(g,.07,.07,.02,'#111111',.12,.72,-.83);
  o.legs=[lb(g,.17,.3,.17,c,-.2,.3,-.3),lb(g,.17,.3,.17,c,.2,.3,-.3),lb(g,.17,.3,.17,c,-.2,.3,.3),lb(g,.17,.3,.17,c,.2,.3,.3)]}
function mCow(g,o){const b='#4a3320',w='#e8e2d4';
  bx(g,.7,.65,1.15,b,0,.95,0);bx(g,.72,.3,.4,w,0,1.0,.2);bx(g,.5,.48,.42,b,0,1.28,-.78);bx(g,.34,.2,.1,'#d9b9a0',0,1.2,-1.02);
  bx(g,.08,.14,.08,'#dddddd',-.26,1.58,-.74);bx(g,.08,.14,.08,'#dddddd',.26,1.58,-.74);bx(g,.07,.07,.02,'#111111',-.14,1.34,-.99);bx(g,.07,.07,.02,'#111111',.14,1.34,-.99);
  o.legs=[lb(g,.2,.55,.2,b,-.25,.55,-.4),lb(g,.2,.55,.2,b,.25,.55,-.4),lb(g,.2,.55,.2,b,-.25,.55,.4),lb(g,.2,.55,.2,b,.25,.55,.4)]}
function mSheep(g,o){const cl=['#f2f2f2','#f2f2f2','#f2f2f2','#c9c9c9','#7a7a7a','#4a3a2e','#d9a066'],w=cl[Math.random()*cl.length|0],s='#c8a98a';
  bx(g,.75,.65,1,w,0,.9,0);bx(g,.38,.38,.4,s,0,1.05,-.68);bx(g,.42,.15,.44,w,0,1.28,-.66);bx(g,.06,.06,.02,'#111111',-.1,1.08,-.89);bx(g,.06,.06,.02,'#111111',.1,1.08,-.89);
  o.legs=[lb(g,.16,.55,.16,s,-.22,.55,-.34),lb(g,.16,.55,.16,s,.22,.55,-.34),lb(g,.16,.55,.16,s,-.22,.55,.34),lb(g,.16,.55,.16,s,.22,.55,.34)]}
function mChick(g,o){
  bx(g,.35,.35,.5,'#f4f4f4',0,.45,0);bx(g,.2,.25,.2,'#f4f4f4',0,.75,-.3);bx(g,.1,.06,.12,'#f0a020',0,.72,-.43);bx(g,.05,.1,.05,'#d03030',0,.64,-.4);
  bx(g,.05,.2,.3,'#e0e0e0',-.21,.5,0);bx(g,.05,.2,.3,'#e0e0e0',.21,.5,0);bx(g,.1,.2,.1,'#e8e8e8',0,.6,.28);bx(g,.04,.04,.02,'#111111',-.06,.8,-.41);bx(g,.04,.04,.02,'#111111',.06,.8,-.41);
  o.legs=[lb(g,.05,.25,.05,'#e0a020',-.08,.25,0),lb(g,.05,.25,.05,'#e0a020',.08,.25,0)]}
function mZom(g,o){const s='#5f9a4a',sh='#2f6f9f',pn='#3a3f8f';
  bx(g,.5,.75,.3,sh,0,1.1,0);bx(g,.46,.46,.46,s,0,1.7,0);bx(g,.08,.06,.02,'#a02020',-.11,1.72,-.24);bx(g,.08,.06,.02,'#a02020',.11,1.72,-.24);bx(g,.2,.04,.02,'#264d1d',0,1.58,-.24);
  o.arms=[lb(g,.18,.7,.18,s,-.34,1.45,0),lb(g,.18,.7,.18,s,.34,1.45,0)];
  o.legs=[lb(g,.22,.72,.24,pn,-.12,.72,0),lb(g,.22,.72,.24,pn,.12,.72,0)]}
function mSkel(g,o){const b='#dcdccc',d='#8a8a7a';
  bx(g,.4,.7,.2,b,0,1.1,0);[1.25,1.1,.95].forEach(y=>bx(g,.42,.06,.22,d,0,y,0));bx(g,.44,.44,.44,b,0,1.7,0);bx(g,.1,.1,.02,'#111111',-.11,1.72,-.23);bx(g,.1,.1,.02,'#111111',.11,1.72,-.23);bx(g,.2,.04,.02,'#333333',0,1.58,-.23);
  bx(g,.04,.55,.06,'#7a5a2a',.3,1.15,-.42);
  o.arms=[lb(g,.1,.68,.1,b,-.27,1.42,0),lb(g,.1,.68,.1,b,.27,1.42,0)];
  o.legs=[lb(g,.12,.75,.12,b,-.1,.75,0),lb(g,.12,.75,.12,b,.1,.75,0)]}
function mSpid(g,o){const d='#2b2420';
  bx(g,.7,.45,.85,d,0,.5,.1);bx(g,.5,.4,.45,'#3b312b',0,.5,-.5);bx(g,.85,.5,.9,d,0,.55,.7);
  [[-.1,.58],[.1,.58],[-.1,.44],[.1,.44]].forEach(([x,y])=>bx(g,.07,.07,.02,'#e02020',x,y,-.73));
  o.legs=[];for(let i=0;i<4;i++)for(const sd2 of[-1,1]){const q=bx(g,.8,.07,.07,'#1b1613',sd2*.6,.42,-.35+i*.28);q.rotation.z=sd2*.45;q.userData.b=sd2*.45;o.legs.push(q)}}
function mCreep(g,o){const c='#4aa84a',d='#2f7a2f';
  bx(g,.5,.78,.3,c,0,.85,0);bx(g,.5,.5,.5,c,0,1.5,0);bx(g,.12,.12,.02,'#111111',-.12,1.58,-.26);bx(g,.12,.12,.02,'#111111',.12,1.58,-.26);bx(g,.1,.2,.02,'#111111',0,1.4,-.26);bx(g,.22,.08,.02,'#111111',0,1.47,-.26);
  o.legs=[lb(g,.22,.46,.22,d,-.14,.46,-.2),lb(g,.22,.46,.22,d,.14,.46,-.2),lb(g,.22,.46,.22,d,-.14,.46,.2),lb(g,.22,.46,.22,d,.14,.46,.2)]}
const MT={
 pig:{n:'Pig',hp:4,hw:.35,h:.9,sp:1.2,cy:.45,xp:1,dr:[[42,1,2]],mk:mPig,s:[170,'square']},
 cow:{n:'Cow',hp:6,hw:.45,h:1.3,sp:1.0,cy:.7,xp:1,dr:[[44,1,3],[38,0,2]],mk:mCow,s:[110,'sawtooth']},
 sheep:{n:'Sheep',hp:4,hw:.4,h:1.2,sp:1.1,cy:.6,xp:1,dr:[[46,1,2],[22,1,1]],mk:mSheep,s:[260,'triangle']},
 chicken:{n:'Chicken',hp:2,hw:.22,h:.7,sp:1.3,cy:.4,xp:1,dr:[[48,1,1],[55,0,2]],mk:mChick,s:[520,'square']},
 zombie:{n:'Zombie',hp:8,hw:.3,h:1.9,sp:2.3,cy:.95,xp:5,hos:1,dr:[[50,0,2],[12,0,1,.25],[51,0,1,.08]],mk:mZom,s:[95,'sawtooth']},
 skeleton:{n:'Skeleton',hp:6,hw:.3,h:1.9,sp:1.8,cy:.95,xp:6,hos:1,dr:[[37,0,3],[30,0,2]],mk:mSkel,s:[320,'square']},
 spider:{n:'Spider',hp:6,hw:.45,h:.85,sp:3.3,cy:.45,xp:4,hos:1,jmp:8.6,dr:[[53,0,2]],mk:mSpid,s:[420,'sawtooth']},
 creeper:{n:'Creeper',hp:6,hw:.3,h:1.75,sp:2.4,cy:.9,xp:7,hos:1,dr:[[54,0,2]],mk:mCreep,s:[140,'sawtooth']}
};
function spawnMob(type,x,y,z){
  const t=MT[type],g=new THREE.Group(),o={legs:[],arms:[]};t.mk(g,o);addShadow(g,t.hw*2.6+.4);g.position.set(x,y,z);scene.add(g);
  const p={type,t,x,y,z,vy:0,ry:Math.random()*6.28,tm:0,ws:0,hp:t.hp,g,hw:t.hw,h:t.h,ground:false,kx:0,kz:0,cy:t.cy,cd:1,lp:0,fl:0,wt:Math.random()*6,pn:0,fuse:-1,burn:0,snt:2+Math.random()*5,angry:false,aim:false,hos:!!t.hos,legs:o.legs,arms:o.arms};
  mobs.push(p);return p}
function removeMob(p){const i=mobs.indexOf(p);if(i>=0)mobs.splice(i,1);scene.remove(p.g);p.g.traverse(q=>{if(q.geometry&&!q.userData.noflash)q.geometry.dispose()})}
const clearHostile=()=>{for(let i=mobs.length-1;i>=0;i--)if(mobs[i]&&mobs[i].hos)removeMob(mobs[i])};
function clearProjectiles(){while(arrows.length)rmArrow(arrows[0]);while(tnts.length)rmTnt(tnts[0])}
function setFlash(p,on,m){p.g.children.forEach(q=>{if(q.userData.noflash)return;if(!q.userData.m)q.userData.m=q.material;q.material=on?m:q.userData.m})}
const passiveN=()=>mobs.reduce((a,m)=>a+(m.hos?0:1),0);
const passiveNear=()=>mobs.reduce((a,m)=>a+(!m.hos&&Math.hypot(m.x-P.x,m.z-P.z)<80?1:0),0);
function solidTop(x,z){if(x<2||z<2||x>=X-2||z>=Z-2)return -1;const h=colTop(x,z),b=get(x,h,z);return((b==1||b==2||b==3||b==4||b==18||b==23)&&!get(x,h+1,z)&&!get(x,h+2,z))?h:-1}
function spawnPassive(far){
  for(let i=0;i<20;i++){
    let x,z;
    if(far){x=Math.random()*X|0;z=Math.random()*Z|0}else{const a=Math.random()*6.283,r=30+Math.random()*34;x=Math.floor(P.x+Math.cos(a)*r);z=Math.floor(P.z+Math.sin(a)*r)}
    if(x<2||z<2||x>=X-2||z>=Z-2)continue;
    const h=colTop(x,z),b=get(x,h,z);
    if(h>SEA&&(b==1||b==18)&&!get(x,h+1,z)&&!get(x,h+2,z)){
      const r=Math.random(),type=r<.28?'pig':r<.52?'cow':r<.82?'sheep':'chicken',n=1+(Math.random()*3|0);
      for(let k=0;k<n;k++){const xx=x+((Math.random()*5-2)|0),zz=z+((Math.random()*5-2)|0),hh2=solidTop(xx,zz);if(hh2>SEA)spawnMob(type,xx+.5,hh2+1.01,zz+.5)}
      return true}}
  return false}
function spawnHostile(){
  const under=P.y<colTop(Math.floor(P.x),Math.floor(P.z))-5;
  let x=0,y=0,z=0,ok=false;
  for(let i=0;i<14&&!ok;i++){
    const a=Math.random()*6.283,r=(under?8:14)+Math.random()*(under?10:18);
    x=Math.floor(P.x+Math.cos(a)*r);z=Math.floor(P.z+Math.sin(a)*r);
    if(x<2||z<2||x>=X-2||z>=Z-2)continue;
    if(under){y=Math.max(2,Math.floor(P.y)+((Math.random()*9)|0)-4);
      for(let k=0;k<8;k++){const b=get(x,y,z);if(solid(x,y-1,z)&&!solid(x,y,z)&&!solid(x,y+1,z)&&b!=9&&b!=19&&get(x,y-1,z)!=19){ok=true;break}y--;if(y<2)break}}
    else{const h=solidTop(x,z);if(h>0){y=h+1;ok=true}}
  }
  if(!ok)return;
  const r=Math.random(),type=r<.5?'zombie':r<.7?'skeleton':r<.88?'spider':'creeper',n=type=='zombie'?1+(Math.random()*3|0):1;
  for(let i=0;i<n;i++){
    const xx=x+((Math.random()*5-2)|0),zz=z+((Math.random()*5-2)|0);
    if(under){if(i==0)spawnMob(type,x+.5,y+.01,z+.5)}
    else{const h=solidTop(xx,zz);if(h>0)spawnMob(type,xx+.5,h+1.01,zz+.5)}}}
function popMobs(){while(mobs.length)removeMob(mobs[0]);clearProjectiles();for(let i=0;i<300&&passiveN()<48;i++)spawnPassive(true)}

/* ---------- mob AI ---------- */
function sees(ax,ay,az,bx,by,bz){const dx=bx-ax,dy=by-ay,dz=bz-az,l=Math.hypot(dx,dy,dz),n=Math.ceil(l/.5);for(let i=1;i<n;i++){const t=i/n;if(solid(Math.floor(ax+dx*t),Math.floor(ay+dy*t),Math.floor(az+dz*t)))return false}return true}
const arrG=new THREE.BoxGeometry(.05,.05,.55),arrM=new THREE.MeshBasicMaterial({color:0xcaa66a});
function launch(x,y,z,vx,vy,vz,own,dmg){const m=new THREE.Mesh(arrG,arrM);m.position.set(x,y,z);scene.add(m);arrows.push({m,x,y,z,vx,vy,vz,own,dmg,life:6,st:0})}
function rmArrow(a){const i=arrows.indexOf(a);if(i>=0)arrows.splice(i,1);scene.remove(a.m)}
function fireAtPlayer(p){
  const ox=p.x,oy=p.y+1.5,oz=p.z,tx=P.x+(Math.random()-.5)*1.2,ty=P.y+1.2+(Math.random()-.5)*.6,tz=P.z+(Math.random()-.5)*1.2,dx=tx-ox,dy=ty-oy,dz=tz-oz,d=Math.hypot(dx,dy,dz),T=Math.max(.2,d/20);
  launch(ox,oy,oz,dx/T,dy/T+.5*12*T,dz/T,'m',2);snd(600,.12,'square',.5)}
function updArrows(dt){
  for(let i=arrows.length-1;i>=0;i--){const a=arrows[i];if(!a)continue;
    if(a.st>0){if((a.st-=dt)<=0)rmArrow(a);continue}
    if((a.life-=dt)<=0){rmArrow(a);continue}
    a.vy-=12*dt;
    const n=Math.max(1,Math.ceil(Math.hypot(a.vx,a.vy,a.vz)*dt/.25));let done=false;
    for(let s=0;s<n&&!done;s++){
      a.x+=a.vx*dt/n;a.y+=a.vy*dt/n;a.z+=a.vz*dt/n;
      if(solid(Math.floor(a.x),Math.floor(a.y),Math.floor(a.z))){a.st=3;done=true;snd(200,.05,'square',.4);break}
      if(a.own=='p'){
        for(const p of mobs){if(Math.abs(a.x-p.x)<p.hw+.12&&Math.abs(a.z-p.z)<p.hw+.12&&a.y>p.y-.1&&a.y<p.y+p.h+.1){hitMob(p,a.dmg,a.vx*.25,a.vz*.25,true);rmArrow(a);done=true;break}}
      }else if(Math.abs(a.x-P.x)<P.hw+.12&&Math.abs(a.z-P.z)<P.hw+.12&&a.y>P.y&&a.y<P.y+1.8){
        const l=Math.hypot(a.vx,a.vz)||1;hurt(a.dmg,a.vx/l,a.vz/l);rmArrow(a);done=true;break}
    }
    if(!done&&a.st<=0){a.m.position.set(a.x,a.y,a.z);a.m.lookAt(a.x+a.vx,a.y+a.vy,a.z+a.vz)}
    else if(a.st>0)a.m.position.set(a.x,a.y,a.z)}}
function spawnTnt(x,y,z,fuse,net){
  const m=new THREE.Mesh(new THREE.BoxGeometry(.98,.98,.98),new THREE.MeshBasicMaterial({map:tntTex}));m.position.set(x+.5,y+.5,z+.5);scene.add(m);
  tnts.push({m,x:x+.5,y:y+.5,z:z+.5,t:fuse,net:!!net});snd(300,.5,'sawtooth',.8)}
function ignite(x,y,z,fuse){
  if(get(x,y,z)!=21)return;
  set(x,y,z,0);edits.push(ix(x,y,z),0);mark(x,z);netBlock(x,y,z,0);
  spawnTnt(x,y,z,fuse,false);
  const m={t:'tnt',x,y,z,f:fuse};if(roomHost)netBroadcast(m);else if(hostConn&&hostConn.open)hostConn.send(m)}
function rmTnt(a){const i=tnts.indexOf(a);if(i>=0)tnts.splice(i,1);scene.remove(a.m);a.m.geometry.dispose();a.m.material.dispose()}
function updTnt(dt){
  for(let i=tnts.length-1;i>=0;i--){const a=tnts[i];if(!a)continue;
    a.t-=dt;const s=1+Math.sin(a.t*18)*.04;a.m.scale.set(s,s,s);a.m.material.color.setScalar(Math.floor(a.t*6)%2?1:2.6);
    if(a.t<=0){const x=a.x,y=a.y,z=a.z,nt2=a.net;rmTnt(a);explode(x,y,z,4.5,true,nt2)}}}
function explode(x,y,z,r,byP,fromNet){
  boom(1-Math.min(.9,Math.hypot(x-P.x,z-P.z)/60));
  const R=Math.ceil(r)+1,cx=Math.floor(x),cy=Math.floor(y),cz=Math.floor(z);
  for(let dx=-R;dx<=R;dx++)for(let dy=-R;dy<=R;dy++)for(let dz=-R;dz<=R;dz++){
    const bx_=cx+dx,by_=cy+dy,bz_=cz+dz;
    if(by_<=0||by_>=Y||bx_<0||bz_<0||bx_>=X||bz_>=Z)continue;
    const d=Math.hypot(bx_+.5-x,by_+.5-y,bz_+.5-z);if(d>r*(.8+Math.random()*.3))continue;
    const b=get(bx_,by_,bz_);if(!b||b==9||b==19||b==17)continue;
    if(b==21){if(!fromNet)ignite(bx_,by_,bz_,.3+Math.random()*.6);continue}
    set(bx_,by_,bz_,0);edits.push(ix(bx_,by_,bz_),0);markB(bx_,bz_,b,0);if(!fromNet)netBlock(bx_,by_,bz_,0);
    if(Math.random()<.1)burst(bx_,by_,bz_,b)}
  burstC(x-.5,y-.5,z-.5,'#8a8a8a',14,1.6);burstC(x-.5,y-.5,z-.5,'#ff9a2a',10,1.8);
  const d=Math.hypot(P.x-x,P.y+1-y,P.z-z);
  if(d<r*2){const n=Math.round(9*(1-d/(r*2)));if(n>0)hurt(n,(P.x-x)/(d||1),(P.z-z)/(d||1))}
  for(let i=mobs.length-1;i>=0;i--){const p=mobs[i];if(!p)continue;
    const dd=Math.hypot(p.x-x,p.y+p.cy-y,p.z-z);
    if(dd<r*2){const n=Math.max(1,Math.round(10*(1-dd/(r*2))));hitMob(p,n,(p.x-x)/(dd||1)*8,(p.z-z)/(dd||1)*8,!!byP)}}
  save()}
function mobStep(p,dt){
  const t=p.t,fx=Math.floor(p.x),fz=Math.floor(p.z);
  if(get(fx,Math.floor(p.y+.2),fz)==19||p.y<-10){removeMob(p);return}
  const dx=P.x-p.x,dz=P.z-p.z,dist=Math.hypot(dx,dz)||.001,dy=P.y-p.y,ang=Math.atan2(-dx,-dz);
  p.cd-=dt;p.tm-=dt;p.lp-=dt;
  if(p.fl>0&&(p.fl-=dt)<=0)setFlash(p,false);
  let want=0;
  const wan=m=>{if(p.tm<0){p.tm=1+Math.random()*3;p.ws=Math.random()<.5?0:t.sp*m;p.ry=Math.random()*6.28}want=p.ws};
  if(p.hos){
    const near=dist<(p.type=='spider'?22:26);
    if(p.type=='zombie'){
      if(near){p.ry=ang;want=2.1+Math.min(.9,day()*.1);
        if(dist<1.3&&Math.abs(dy)<1.7&&p.cd<=0){p.cd=1.3;hurt(2+(day()>6?1:0),dx/dist,dz/dist)}}
      else wan(.6)
    }else if(p.type=='skeleton'){
      p.aim=near;
      if(near){p.ry=ang;
        if(dist>10)want=t.sp;else if(dist<5)want=-t.sp*.9;
        if(dist<20&&p.cd<=0&&sees(p.x,p.y+1.5,p.z,P.x,P.y+1.4,P.z)){p.cd=2+Math.random()*1.5;fireAtPlayer(p)}}
      else wan(.6)
    }else if(p.type=='spider'){
      const act=near&&(lum<.5||p.angry||dist<3);
      if(act){p.ry=ang;want=t.sp;
        if(dist>1.8&&dist<4.2&&p.ground&&p.lp<=0&&Math.abs(dy)<2){p.lp=1.4;p.vy=6.2;p.kx=dx/dist*6;p.kz=dz/dist*6}
        if(dist<1.5&&Math.abs(dy)<1.5&&p.cd<=0){p.cd=.9;hurt(1,dx/dist,dz/dist)}}
      else wan(.5)
    }else if(p.type=='creeper'){
      if(p.fuse>=0){
        p.fuse-=dt;p.ry=ang;
        const k=1+(1.5-p.fuse)*.18;p.g.scale.set(k,k,k);setFlash(p,Math.floor(p.fuse*7)%2===0,whiteM);
        if(dist>6.5){p.fuse=-1;p.g.scale.set(1,1,1);setFlash(p,false)}
        else if(p.fuse<=0){const x=p.x,y=p.y+.9,z=p.z;removeMob(p);explode(x,y,z,3.2,false);return}
      }else if(near){p.ry=ang;want=t.sp;if(dist<2.8&&Math.abs(dy)<2){p.fuse=1.5;snd(200,.6,'sawtooth',.8)}}
      else wan(.5)
    }
    if(p.type=='zombie'||p.type=='skeleton'){
      p.burn+=dt;
      if(p.burn>1.2){p.burn=0;if(lum>.55&&rain<.3&&colTop(fx,fz)<=Math.floor(p.y+1)){burstC(p.x-.5,p.y+1,p.z-.5,'#ff8a1a',4,.5);hitMob(p,1,0,0,false,true);if(p.hp<=0)return}}}
  }else{
    if(p.pn>0){p.pn-=dt;want=t.sp*2.4}else wan(1)
  }
  const w=get(fx,Math.floor(p.y+.3),fz)==9,spd=w?want*.7:want;
  const bx_=mv('x',(-Math.sin(p.ry)*spd+p.kx)*dt,p),bz_=mv('z',(-Math.cos(p.ry)*spd+p.kz)*dt,p);
  const kd=Math.max(0,1-5*dt);p.kx*=kd;p.kz*=kd;
  if((bx_||bz_)&&p.ground&&want!==0)p.vy=t.jmp||7.5;
  p.vy=w?Math.min(p.vy+20*dt,2):Math.max(p.vy-26*dt,-30);
  if(p.type=='chicken'&&p.vy<-2.5)p.vy=-2.5;
  if(mv('y',p.vy*dt,p)){p.ground=p.vy<0;p.vy=0}else p.ground=false;
  const mvg=Math.abs(want)>.1;p.wt+=dt*(mvg?(3+Math.abs(want)*2.4):0);
  const sw=mvg?Math.sin(p.wt)*.7:0,L=p.legs;
  if(p.type=='spider')L.forEach((q,i)=>{q.rotation.z=q.userData.b+(mvg?Math.sin(p.wt*1.4+i*1.3)*.25:0)});
  else L.forEach((q,i)=>{q.rotation.x=L.length==4?((i==0||i==3)?sw:-sw):(i?-sw:sw)});
  if(p.arms.length){const a=p.type=='zombie'?1.45:p.aim?1.4:sw*.8;p.arms.forEach((q,i)=>{q.rotation.x=p.type=='zombie'||p.aim?a+Math.sin(p.wt*.5)*.04:(i?-a:a)})}
  p.g.position.set(p.x,p.y,p.z);p.g.rotation.y=p.ry;
  if((p.snt-=dt)<0){p.snt=4+Math.random()*7;if(dist<18&&p.type!='creeper'&&(p.hos||Math.random()<.5))snd(t.s[0]*(.9+Math.random()*.2),.18,t.s[1],.7*(1-dist/20))}}
function updMobs(dt){
  for(let i=mobs.length-1;i>=0;i--){const p=mobs[i];if(!p)continue;
    const d=Math.hypot(P.x-p.x,P.z-p.z);
    if(d>78){if(p.hos&&d>90)removeMob(p);else p.g.visible=false;continue}
    p.g.visible=d<RD+6;mobStep(p,dt)}}

/* ---------- player combat ---------- */
function hitMob(p,n,kx,kz,byP,soft){
  p.hp-=n;p.fl=.25;setFlash(p,true,flashM);snd(110,.12,"sawtooth");
  if(!soft)p.vy=Math.max(p.vy,4.5);
  p.kx=kx;p.kz=kz;
  if(!p.hos){p.pn=4;if(kx||kz)p.ry=Math.atan2(-kx,-kz)}else p.angry=true;
  if(p.hp<=0){
    const x=p.x,y=p.y,z=p.z,col=p.type=='creeper'?'#4aa84a':p.type=='zombie'?'#5f9a4a':p.hos?'#aaaaaa':'#f0a0a8';
    removeMob(p);burstC(x-.5,y+.3,z-.5,col,10,1.3);snd(80,.3,"sawtooth",.8);
    if(byP&&mode!==0){
      const got=[];for(const[id,mn,mx,ch]of p.t.dr){if(Math.random()<(ch==null?1:ch)){const c=mn+Math.floor(Math.random()*(mx-mn+1));if(c>0){inv[id]=(inv[id]||0)+c;got.push('+'+c+' '+N[id])}}}
      if(got.length)toast(got.join(', '));badges();addXP(p.t.xp)}}}
function aimMob(range){
  const cp=Math.cos(pitch),d={x:-Math.sin(yaw)*cp,y:Math.sin(pitch),z:-Math.cos(yaw)*cp},c={x:P.x,y:P.y+1.62,z:P.z};
  let best=null,bt=range;
  for(const p of mobs){if(!p.g.visible)continue;const vx=p.x-c.x,vy=p.y+p.cy-c.y,vz=p.z-c.z,t=vx*d.x+vy*d.y+vz*d.z;
    if(t>0&&t<bt){const q=(vx-d.x*t)**2+(vy-d.y*t)**2+(vz-d.z*t)**2,rr=Math.max(.45,(Math.max(p.hw,p.h*.3)+.2)**2);if(q<rr){best=p;bt=t}}}
  if(best&&RAY&&RAY.t<bt-.4)return null;
  return best}
function punch(){
  const w=WS[sel]||WS[0],tg=aimMob(w.r);
  if(!tg)return false;
  if(atkT>0)return true;
  atkT=w.cd;swing=1;
  let dmg=w.d+(C.dm-1)+(lv()>>2);
  if(!P.ground&&P.vy<0&&!P.fly){dmg=Math.ceil(dmg*1.5);burstC(tg.x-.5,tg.y+tg.cy,tg.z-.5,'#ffffff',4,.8)}
  hitMob(tg,dmg,-Math.sin(yaw)*w.kb,-Math.cos(yaw)*w.kb,true);
  return true}
function shoot(){
  if(bowT>0)return;
  if(mode&&!(inv[37]>0)){toast('No arrows');return}
  bowT=.55;if(mode){inv[37]--;badges()}
  const cp=Math.cos(pitch),dx=-Math.sin(yaw)*cp,dy=Math.sin(pitch),dz=-Math.cos(yaw)*cp;
  launch(P.x+dx*.5,P.y+1.55+dy*.5,P.z+dz*.5,dx*28,dy*28,dz*28,'p',5+(lv()>>2));snd(500,.1,'triangle')}
function eatItem(id){
  if(!mode){toast('Creative: no hunger');return}
  if(!(inv[id]>0))return;
  if(id==52){if(hp>=10&&food>=20){toast('Already full');return}hp=10;food=20}
  else{if(food>=20){toast('Not hungry');return}food=Math.min(20,food+FOOD[id])}
  inv[id]--;snd(240,.08,'square');drawHp();badges();uiNow()}
function eatBest(){
  if(!mode)return;
  const ids=Object.keys(FOOD).map(Number).filter(i=>(inv[i]||0)>0).sort((a,b)=>FOOD[b]-FOOD[a]);
  if(!ids.length){toast('No food');return}
  eatItem(food>=18?ids[ids.length-1]:ids[0])}
function equip(id){
  if(!mode)return;
  if(!(inv[id]>0))return;
  const a=ARMP[id];
  if(armorId&&ARMP[armorId].a>=a.a){toast('Already wearing '+ARMP[armorId].n+' armor');return}
  if(armorId)inv[armorId]=(inv[armorId]||0)+1;
  armorId=id;inv[id]--;toast(a.n+' armor equipped');snd(400,.1,'square');badges();uiNow();save()}
function useItem(){
  if(sel<30)return plc();
  swing=1;
  if(sel==36)return shoot();
  if(FOOD[sel])return eatItem(sel);
  if(ARMP[sel])return equip(sel);
  punch()}
function touchCactus(){
  const e=P.hw+.15;
  for(const[ox,oz]of[[e,0],[-e,0],[0,e],[0,-e]])for(const oy of[.3,1.2])
    if(get(Math.floor(P.x+ox),Math.floor(P.y+oy),Math.floor(P.z+oz))==20){hurt(1,-ox*3,-oz*3);return}}
/* ---------- modes & characters ---------- */
const MODES=['Creative','Survival','Peaceful'],MD=['Fly, unlimited blocks, no damage. Spawn any mob from this menu.','Hunger, zombies, skeletons, spiders and creepers at night and in caves. Craft weapons and armor.','Gather and build. No hostile mobs, slow hunger-free healing, but lava and falls still hurt.'];
const CHR=[{n:'Explorer',d:'Runs 20% faster and jumps higher.',sp:1.2,jp:1.1,dm:1,ar:0,rc:6,c:['#e8b894','#3aa655','#3b5b8c','#5a3b1e']},
{n:'Builder',d:'Reaches 8 blocks to build and mine.',sp:1,jp:1,dm:1,ar:0,rc:8,c:['#c68a5e','#f2a516','#555a60','#ffd23f']},
{n:'Knight',d:'Hits harder and takes less damage.',sp:1,jp:1,dm:2,ar:1,rc:6,c:['#e8b894','#9aa4ad','#5b6670','#8d98a2']},
{n:'Diver',d:'Swims fast and never takes fall damage.',sp:1,jp:1,dm:1,ar:0,rc:6,sw:5,fd:0,c:['#8a5a35','#1d6fd1','#0f3d7a','#111111']}];
let C=CHR[0];
function mkPlr(){if(plr)scene.remove(plr);plr=new THREE.Group();const k=C.c.map(c=>new THREE.MeshLambertMaterial({color:c})),ey=new THREE.MeshBasicMaterial({color:'#111111'});
  const B=(w,h,d,m,x,y,z)=>{const q=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),m);q.position.set(x,y,z);plr.add(q);return q};
  B(.5,.65,.26,k[1],0,1,0);B(.18,.62,.2,k[1],-.35,1.02,0);B(.18,.62,.2,k[1],.35,1.02,0);
  legs=[B(.24,.68,.26,k[2],-.13,.34,0),B(.24,.68,.26,k[2],.13,.34,0)];
  B(.46,.46,.46,k[0],0,1.55,0);B(.5,.16,.5,k[3],0,1.8,0);B(.08,.08,.02,ey,-.11,1.57,-.24);B(.08,.08,.02,ey,.11,1.57,-.24);addShadow(plr,1);scene.add(plr)}

/* ---------- multiplayer ---------- */
let peer=null,hostConn=null,connections=new Map(),remotePlayers=new Map(),roomHost=false,roomCode='',joined=false,joinTO=0,pingMs=0;
const RCOL=['#55aaff','#ff7755','#77dd66','#cc77ff','#ffcc33'],PFX='blockland-';
const ICE={iceServers:[{urls:'stun:stun.l.google.com:19302'},{urls:'stun:stun1.l.google.com:19302'},{urls:'stun:global.stun.twilio.com:3478'}]};
const mstat=t=>{$('mstatus').textContent=t};
const num=(v,d)=>Number.isFinite(v)?v:d;
function code6(){const a='ABCDEFGHJKLMNPQRSTUVWXYZ23456789';let s='';for(let i=0;i<6;i++)s+=a[Math.random()*a.length|0];return s}
function netBroadcast(msg,except){connections.forEach((c,id)=>{if(id!==except&&c.open)c.send(msg)});if(hostConn&&hostConn.open&&except!=='host')hostConn.send(msg)}
function netBlock(x,y,z,b){const m={t:'block',x,y,z,b};if(roomHost)netBroadcast(m);else if(hostConn&&hostConn.open)hostConn.send(m)}
function chatMsg(name,msg){const d=document.createElement('div');d.textContent=name+': '+msg;d.dataset.t=Date.now();$('chatlog').appendChild(d);while($('chatlog').children.length>8)$('chatlog').firstChild.remove()}
setInterval(()=>{const f=document.activeElement===$('chatin'),n=Date.now();[...$('chatlog').children].forEach(d=>{d.style.opacity=(f||n-d.dataset.t<9000)?1:0})},700);
function nameTag(name){const c=document.createElement('canvas');c.width=256;c.height=48;const g=c.getContext('2d');g.font='bold 28px monospace';g.textAlign='center';g.fillStyle='rgba(0,0,0,.55)';g.fillRect(0,4,256,40);g.fillStyle='#ffffff';g.fillText(String(name).slice(0,16),128,36);
  const s=new THREE.Sprite(new THREE.SpriteMaterial({map:new THREE.CanvasTexture(c),depthTest:false,fog:false,transparent:true}));s.scale.set(1.7,.32,1);s.position.y=2.3;s.renderOrder=998;return s}
function setRName(r,name){name=String(name||'Player').slice(0,18);if(r.name===name&&r.tag)return;r.name=name;if(r.tag){r.g.remove(r.tag);r.tag.material.map.dispose();r.tag.material.dispose()}r.tag=nameTag(name);r.g.add(r.tag)}
function makeRemote(id,data){
  if(remotePlayers.has(id))return remotePlayers.get(id);
  let h=0;for(const ch of String(id))h=(h*31+ch.charCodeAt(0))|0;
  const col=RCOL[Math.abs(h)%5],g=new THREE.Group(),head=new THREE.Group();
  bx(g,.5,.65,.26,col,0,1,0);
  head.position.set(0,1.55,0);g.add(head);bx(head,.46,.46,.46,'#e8b894',0,0,0);bx(head,.5,.16,.5,'#3a2a1a',0,.25,0);bx(head,.08,.08,.02,'#111111',-.11,.02,-.24);bx(head,.08,.08,.02,'#111111',.11,.02,-.24);
  const lg=[lb(g,.24,.68,.26,'#33415c',-.13,.68,0),lb(g,.24,.68,.26,'#33415c',.13,.68,0)],ar=[lb(g,.18,.62,.2,col,-.35,1.33,0),lb(g,.18,.62,.2,col,.35,1.33,0)];
  addShadow(g,1);scene.add(g);
  const r={g,head,lg,ar,tx:num(data.x,0),ty:num(data.y,0),tz:num(data.z,0),yaw:num(data.yaw,0),pitch:0,name:'',tag:null,wt:0,sp:0};g.position.set(r.tx,r.ty,r.tz);g.rotation.y=r.yaw;setRName(r,data.name);remotePlayers.set(id,r);return r}
function removeRemote(id){const r=remotePlayers.get(id);if(r){scene.remove(r.g);remotePlayers.delete(id)}}
function playerList(){
  const names=[username,...[...remotePlayers.values()].map(r=>r.name)];
  $('players').innerHTML='<b>Players ('+names.length+')</b><br>'+names.map(n=>String(n).replace(/[<>&"]/g,'')).join('<br>')}
function sendPose(){
  if(!peer||!peer.open)return;
  const m={t:'pose',id:peer.id,name:username,x:+P.x.toFixed(2),y:+P.y.toFixed(2),z:+P.z.toFixed(2),yaw:+yaw.toFixed(2),pitch:+pitch.toFixed(2)};
  if(roomHost)netBroadcast(m);else if(hostConn&&hostConn.open)hostConn.send(m)}
function placeNear(x,z){
  for(let i=0;i<60;i++){const a=Math.random()*6.28,r=2+i*.15,xx=Math.floor(x+Math.cos(a)*r),zz=Math.floor(z+Math.sin(a)*r),h=solidTop(xx,zz);
    if(h>SEA){P.x=xx+.5;P.z=zz+.5;P.y=h+1.01;P.vy=0;queueAll(true);return}}}
function applyNet(m,conn){
  if(!m||typeof m!=='object')return;
  if(m.t==='hello'){
    if(!roomHost)return;
    const r=makeRemote(conn.peer,m);setRName(r,m.name);
    conn.send({t:'world',seed,edits,mode,time,x:P.x,y:P.y,z:P.z});
    conn.send({t:'join',id:peer.id,name:username,x:P.x,y:P.y,z:P.z,yaw});
    remotePlayers.forEach((rr,rid)=>{if(rid!==conn.peer)conn.send({t:'join',id:rid,name:rr.name,x:rr.tx,y:rr.ty,z:rr.tz,yaw:rr.yaw})});
    netBroadcast({t:'join',id:conn.peer,name:r.name,x:num(m.x,0),y:num(m.y,0),z:num(m.z,0),yaw:num(m.yaw,0)},conn.peer);
    chatMsg('System',r.name+' joined');netBroadcast({t:'chat',name:'System',msg:r.name+' joined'});playerList();
    mstat('Hosting '+roomCode+' · '+(remotePlayers.size)+' other player'+(remotePlayers.size==1?'':'s'));return}
  if(m.t==='world'){
    if(roomHost||!Number.isFinite(m.seed))return;
    joined=true;start(m.seed,Array.isArray(m.edits)?m.edits:[],inv);time=num(m.time,time);setMode(m.mode|0);placeNear(num(m.x,P.x),num(m.z,P.z));
    mstat('Connected to '+roomCode);toast('Joined room '+roomCode);return}
  if(m.t==='time'){if(!roomHost&&Math.abs(m.time-time)>.05)time=num(m.time,time);return}
  if(m.t==='ping'){if(roomHost)conn.send({t:'pong',ts:m.ts});return}
  if(m.t==='pong'){pingMs=Date.now()-num(m.ts,Date.now());mstat('Connected to '+roomCode+' · '+pingMs+' ms');return}
  if(m.t==='join'){if(!m.id||(peer&&m.id===peer.id))return;const r=makeRemote(m.id,m);setRName(r,m.name);playerList();return}
  if(m.t==='leave'){const r=remotePlayers.get(m.id);if(r)chatMsg('System',r.name+' left');removeRemote(m.id);playerList();return}
  if(m.t==='pose'){
    const id=roomHost?conn.peer:(m.id||conn.peer);if(peer&&id===peer.id)return;
    const had=remotePlayers.has(id),r=makeRemote(id,m);
    if(r.name!==String(m.name||'Player').slice(0,18)){setRName(r,m.name);playerList()}else if(!had)playerList();
    r.tx=num(m.x,r.tx);r.ty=num(m.y,r.ty);r.tz=num(m.z,r.tz);r.yaw=num(m.yaw,r.yaw);r.pitch=num(m.pitch,0);
    if(roomHost){m.id=id;netBroadcast(m,id)}return}
  if(m.t==='block'){
    if(Number.isInteger(m.x)&&Number.isInteger(m.y)&&Number.isInteger(m.z)&&m.x>=0&&m.z>=0&&m.x<X&&m.z<Z&&m.y>0&&m.y<Y&&Number.isInteger(m.b)&&m.b>=0&&m.b<=23){
      const o=get(m.x,m.y,m.z);set(m.x,m.y,m.z,m.b);edits.push(ix(m.x,m.y,m.z),m.b);markB(m.x,m.z,o,m.b)}
    if(roomHost)netBroadcast(m,conn.peer);return}
  if(m.t==='tnt'){
    if(Number.isInteger(m.x)&&Number.isInteger(m.y)&&Number.isInteger(m.z)&&m.x>=0&&m.z>=0&&m.x<X&&m.z<Z&&m.y>0&&m.y<Y){
      if(get(m.x,m.y,m.z)==21){set(m.x,m.y,m.z,0);mark(m.x,m.z)}
      spawnTnt(m.x,m.y,m.z,Math.min(5,Math.max(.1,num(m.f,2.4))),true)}
    if(roomHost)netBroadcast(m,conn.peer);return}
  if(m.t==='chat'){const n=String(m.name||'Player').slice(0,18),s=String(m.msg||'').slice(0,120);chatMsg(n,s);if(roomHost)netBroadcast({t:'chat',name:n,msg:s},conn.peer);return}
}
function wireConn(c,isHostLink){
  c.on('open',()=>{
    if(isHostLink){clearTimeout(joinTO);c.send({t:'hello',name:username,x:P.x,y:P.y,z:P.z,yaw});mstat('Connected, loading world...');$('chat').style.display='block';$('players').style.display='block';playerList()}});
  c.on('data',m=>applyNet(m,c));
  c.on('close',()=>{
    if(isHostLink){if(hostConn===c)hostLeft();return}
    const r=remotePlayers.get(c.peer),nm=r?r.name:'A player';
    removeRemote(c.peer);connections.delete(c.peer);
    if(roomHost){netBroadcast({t:'leave',id:c.peer},c.peer);chatMsg('System',nm+' left');netBroadcast({t:'chat',name:'System',msg:nm+' left'});mstat('Hosting '+roomCode+' · '+remotePlayers.size+' other player'+(remotePlayers.size==1?'':'s'))}
    playerList()});
  c.on('error',()=>{mstat('Connection error')})}
function hostLeft(){toast('The host closed the room');leaveRoom();mstat('The host left the room.')}
function createPeer(host,tryN){
  tryN=tryN||0;
  if(typeof Peer==='undefined'){mstat('Multiplayer library failed to load (check your internet connection).');return false}
  if(peer){try{peer.destroy()}catch(e){}peer=null}
  peer=new Peer(host?PFX+code6():undefined,{config:ICE,debug:0});
  peer.on('open',pid=>{if(host){roomCode=pid.slice(PFX.length);roomHost=true;$('room').value=roomCode;mstat('Room ready. Code: '+roomCode+' (share it or copy the invite link)');$('chat').style.display='block';$('players').style.display='block';playerList()}});
  peer.on('connection',c=>{connections.set(c.peer,c);wireConn(c,false)});
  peer.on('disconnected',()=>{try{peer.reconnect()}catch(e){}});
  peer.on('error',e=>{
    if(e.type==='unavailable-id'&&host&&tryN<5){createPeer(true,tryN+1);return}
    mstat(({'peer-unavailable':'No room with that code. Check it and make sure the host still has the room open.','network':'Network error: cannot reach the matchmaking server.','browser-incompatible':'This browser does not support WebRTC.','server-error':'Matchmaking server error. Try again.'})[e.type]||('Peer error: '+(e.type||'unknown')))});
  return true}
function setUser(){if(!session||session.type==='guest'){username=($('mname').value.trim()||username||'Player').slice(0,18);LS.setItem('blockland_name',username)}}
function hostRoom(){
  setUser();if(peer)leaveRoom();
  if(!createPeer(true))return;roomHost=true;mstat('Creating room...')}
function joinRoom(){
  let id=$('room').value.trim();if(!id){mstat('Enter a room code first.');return}
  setUser();if(peer)leaveRoom();roomHost=false;
  id=/^[A-Za-z0-9]{4,8}$/.test(id)?PFX+id.toUpperCase():id;roomCode=id.replace(PFX,'');
  if(!createPeer(false))return;mstat('Connecting...');
  let n=0;const wait=setInterval(()=>{
    if(!peer){clearInterval(wait);return}
    if(peer.open){clearInterval(wait);hostConn=peer.connect(id,{reliable:true});wireConn(hostConn,true);
      joinTO=setTimeout(()=>{if(!joined){mstat('Could not connect. The host may be offline, or your network blocks peer-to-peer connections.');try{hostConn&&hostConn.close()}catch(e){}}},15000)}
    else if(++n>120){clearInterval(wait);mstat('Could not reach the matchmaking server.')}},100)}
function leaveRoom(){
  const reload=joined&&!roomHost;joined=false;clearTimeout(joinTO);
  connections.forEach(c=>{try{c.close()}catch(e){}});connections.clear();
  if(hostConn){const h=hostConn;hostConn=null;try{h.close()}catch(e){}}
  if(peer){try{peer.destroy()}catch(e){}peer=null}
  [...remotePlayers.keys()].forEach(removeRemote);roomHost=false;roomCode='';
  $('chat').style.display='none';$('players').style.display='none';mstat('Offline');
  if(reload)loadProfile()}
function inviteLink(){let u='';try{u=location.origin+location.pathname+'?room='+encodeURIComponent($('room').value.trim())}catch(e){}return u&&u.indexOf('http')==0?u:$('room').value.trim()}
const clip=t=>{try{navigator.clipboard.writeText(t).catch(()=>{})}catch(e){}};

/* ---------- accounts ---------- */
let accounts=jget('bl_accounts',{}),session=null,SK='blockland4',booted=false,authMode='in',pendingRoom=null;
function rndSalt(){const a=new Uint8Array(12);try{crypto.getRandomValues(a)}catch(e){for(let i=0;i<12;i++)a[i]=Math.random()*256}return [...a].map(b=>b.toString(16).padStart(2,'0')).join('')}
async function hashPw(pw,salt){
  try{if(window.crypto&&crypto.subtle){const enc=new TextEncoder(),km=await crypto.subtle.importKey('raw',enc.encode(pw),'PBKDF2',false,['deriveBits']),
    bits=await crypto.subtle.deriveBits({name:'PBKDF2',salt:enc.encode(salt),iterations:100000,hash:'SHA-256'},km,256);
    return 'p'+[...new Uint8Array(bits)].map(b=>b.toString(16).padStart(2,'0')).join('')}}catch(e){}
  let h=5381;const s=salt+pw;for(let i=0;i<s.length;i++)h=((h<<5)+h+s.charCodeAt(i))|0;return 'f'+(h>>>0).toString(16)}
const amsg=t=>{$('authmsg').textContent=t||''};
function setAuth(m){authMode=m;$('tabin').classList.toggle('on',m=='in');$('tabup').classList.toggle('on',m=='up');$('acctpass2').style.display=m=='up'?'':'none';$('authgo').textContent=m=='up'?'Create account':'Sign in';amsg('')}
function refreshAcct(){
  const s=session;$('acctstate').textContent=s?'Signed in as '+s.name+' ('+({local:'local account',google:'Google',guest:'guest'}[s.type]||s.type)+')':'Not signed in. Sign in, create an account, or play as a guest.';
  $('logout').style.display=s&&s.type!=='guest'?'':'none'}
function showAccount(force){refreshAcct();$('account').style.display='flex';$('closeacct').style.display=force?'none':'';initGoogle()}
function updateWho(){
  $('who').textContent=session?'Signed in as '+session.name+({guest:' (guest)',google:' (Google)',local:''}[session.type]||''):'Not signed in';
  $('mname').value=username;$('mname').readOnly=!!(session&&session.type!=='guest');playerList()}
function login(s,temp){
  if(booted)saveNow();
  joined=false;if(peer)leaveRoom();
  session=temp?null:s;if(!temp){try{LS.setItem('bl_session',JSON.stringify(s))}catch(e){}}
  SK=s.type==='guest'?'blockland4':'blockland4:'+s.id;username=s.name;
  loadProfile();booted=true;updateWho();
  $('account').style.display='none';$('acctpass').value=$('acctpass2').value='';amsg('');
  if(pendingRoom&&!temp){const r=pendingRoom;pendingRoom=null;$('multi').style.display='flex';$('room').value=r;setTimeout(joinRoom,400)}}
async function authGo(){
  const n=$('acctname').value.trim(),p=$('acctpass').value,key=n.toLowerCase();
  if(!/^[A-Za-z0-9_ -]{3,18}$/.test(n)){amsg('Username: 3-18 letters, numbers, spaces, _ or -');return}
  if(p.length<6){amsg('Password needs at least 6 characters');return}
  if(authMode==='up'){
    if(accounts[key]){amsg('That username is already taken on this device');return}
    if(p!==$('acctpass2').value){amsg('Passwords do not match');return}
    const salt=rndSalt(),first=!Object.keys(accounts).length;
    accounts[key]={name:n,salt,hash:await hashPw(p,salt),created:Date.now()};
    try{LS.setItem('bl_accounts',JSON.stringify(accounts))}catch(e){}
    if(first&&LS.getItem('blockland4')&&!LS.getItem('blockland4:u:'+key)){try{LS.setItem('blockland4:u:'+key,LS.getItem('blockland4'))}catch(e){}}
    login({id:'u:'+key,name:n,type:'local'})
  }else{
    const a=accounts[key];
    if(!a||a.hash!==await hashPw(p,a.salt)){amsg('Wrong username or password');return}
    login({id:'u:'+key,name:a.name,type:'local'})}}
function initGoogle(){
  const cid=LS.getItem('bl_gcid');$('gcid').value=cid||'';
  if(!cid){$('gbox').style.display='none';return}
  $('gbox').style.display='block';
  const go=()=>{try{google.accounts.id.initialize({client_id:cid,callback:r=>{try{
      const p=JSON.parse(decodeURIComponent(escape(atob(r.credential.split('.')[1].replace(/-/g,'+').replace(/_/g,'/')))));
      login({id:'g:'+p.sub,name:String(p.given_name||p.name||'Player').slice(0,18),type:'google',email:p.email})}catch(e){amsg('Google sign-in failed')}}});
    $('gbtn').innerHTML='';google.accounts.id.renderButton($('gbtn'),{theme:'filled_black',size:'large',text:'signin_with'})}catch(e){amsg('Google sign-in could not start (check the Client ID and origin)')}};
  if(window.google&&google.accounts&&google.accounts.id)go();
  else if(!document.getElementById('gsi')){const s=document.createElement('script');s.id='gsi';s.src='https://accounts.google.com/gsi/client';s.async=true;s.onload=go;s.onerror=()=>amsg('Could not load Google sign-in');document.head.appendChild(s)}}

/* ---------- sky, light & per-frame systems ---------- */
const SC={tn:new THREE.Color('#02040f'),td:new THREE.Color('#3f86d6'),hn:new THREE.Color('#0d1430'),hd:new THREE.Color('#a9d8f2'),ss:new THREE.Color('#ff8f4a'),st:new THREE.Color('#6a5a9c'),gr:new THREE.Color('#7d8590'),wt:new THREE.Color('#1c5aa8')};
const skyTop=new THREE.Color(),skyHor=new THREE.Color(),fogC=new THREE.Color(),tmpC=new THREE.Color();
const clamp01=v=>Math.max(0,Math.min(1,v));
function updSky(sa,ca,l){
  skyTop.copy(SC.tn).lerp(SC.td,l);skyHor.copy(SC.hn).lerp(SC.hd,l);
  const ss=Math.max(0,1-Math.abs(sa)/.3)*(1-rain*.7);ssG=ss;
  skyHor.lerp(SC.ss,ss*.75);skyTop.lerp(SC.st,ss*.25);
  tmpC.copy(SC.gr).multiplyScalar(.15+.85*l);skyHor.lerp(tmpC,rain*.65);skyTop.lerp(tmpC,rain*.75);
  const u=skyMat.uniforms;u.uTop.value.copy(skyTop);u.uHor.value.copy(skyHor);
  const sunI=clamp01((sa+.12)/.2)*(1-rain*.8),moonI=clamp01((-sa+.12)/.2)*.35*(1-rain);
  if(sunI>=moonI){u.uSun.value.set(0,sa,ca);u.uGlow.value.setRGB(1,.82-ss*.25,.55-ss*.3);u.uGI.value=sunI}
  else{u.uSun.value.set(0,-sa,-ca);u.uGlow.value.setRGB(.55,.65,1);u.uGI.value=moonI}
  amb.intensity=.2+.4*l*(1-.15*rain);amb.color.setRGB(.75+.25*l,.8+.2*l,1);
  if(sa>=0){sunL.position.set(0,sa+.15,ca);sunL.intensity=.7*l*(1-.5*rain);sunL.color.setRGB(1,.95-ss*.25,.85-ss*.4)}
  else{sunL.position.set(0,-sa+.15,-ca);sunL.intensity=.18*(1-l);sunL.color.setRGB(.5,.6,1)}
  sunM.material.color.setRGB(1,.95-ss*.35,.66-ss*.4)}
const mmc=$('mm'),mmx=mmc.getContext('2d'),MS=72;let mmi=null;
const MC=[];for(let b=1;b<=23;b++)MC[b]=H(BASE[BT[b][0]]);
function drawMini(){
  if(!mmi)mmi=mmx.createImageData(MS,MS);
  const d=mmi.data,cx=Math.floor(P.x),cz=Math.floor(P.z);
  for(let py=0;py<MS;py++)for(let px=0;px<MS;px++){
    const wx=cx+(px-MS/2)*2,wz=cz+(py-MS/2)*2,o=(py*MS+px)*4;
    if(wx<0||wz<0||wx>=X||wz>=Z){d[o]=d[o+1]=d[o+2]=0;d[o+3]=255;continue}
    const h=colTop(wx,wz),b=get(wx,h,wz),c=MC[b]||[0,0,0],s=.7+h/Y*.5;
    d[o]=c[0]*s;d[o+1]=c[1]*s;d[o+2]=c[2]*s;d[o+3]=255}
  mmx.putImageData(mmi,0,0);
  mobs.forEach(p=>{const mx=(p.x-P.x)/2+MS/2,my=(p.z-P.z)/2+MS/2;if(mx>1&&my>1&&mx<MS-1&&my<MS-1){mmx.fillStyle=p.hos?'#ff3030':'#ffe066';mmx.fillRect(mx-1,my-1,2,2)}});
  remotePlayers.forEach(r=>{const mx=(r.tx-P.x)/2+MS/2,my=(r.tz-P.z)/2+MS/2;if(mx>1&&my>1&&mx<MS-1&&my<MS-1){mmx.fillStyle='#55aaff';mmx.fillRect(mx-2,my-2,4,4)}});
  mmx.save();mmx.translate(MS/2,MS/2);mmx.rotate(-yaw);mmx.fillStyle='#ffffff';mmx.strokeStyle='#000000';mmx.beginPath();mmx.moveTo(0,-5);mmx.lineTo(3.5,4);mmx.lineTo(-3.5,4);mmx.closePath();mmx.fill();mmx.stroke();mmx.restore()}
function uiNow(){
  if(mode){$('food').textContent='Food: '+food+'/20';$('stam').textContent='Stamina: '+Math.round(stamina)+'/100';$('xp').textContent='Lv '+lv()+' | XP '+xp;$('arm').textContent='Armor: '+(armorId?ARMP[armorId].n:'none')}
  $('coords').textContent='X'+Math.floor(P.x)+' Y'+Math.floor(P.y)+' Z'+Math.floor(P.z)+'  Day '+day()+(lum<.28?' (night)':'')}
const angDiff=(a,b)=>{let d=(a-b)%(Math.PI*2);if(d>Math.PI)d-=Math.PI*2;if(d<-Math.PI)d+=Math.PI*2;return d};
function extra(dt,rdt){
  const c=cam.position,sa=Math.sin(time),ca=Math.cos(time);
  sunM.position.set(c.x,c.y+sa*80,c.z+ca*80);sunM.lookAt(c);sunM.visible=sa>-.05;
  moonM.position.set(c.x,c.y-sa*80,c.z-ca*80);moonM.lookAt(c);moonM.visible=sa<.05;
  dome.position.copy(c);stars.position.copy(c);stars.material.opacity=Math.max(0,.95-lum*2.2);
  clouds.forEach(q=>{q.position.x+=dt*1.5;if(q.position.x>X+60)q.position.x=-60});cm.color.setScalar(.4+.6*lum);
  for(let i=parts.length-1;i>=0;i--){const q=parts[i];q.vy-=18*dt;q.m.position.x+=q.vx*dt;q.m.position.y+=q.vy*dt;q.m.position.z+=q.vz*dt;if((q.t-=dt)<0){scene.remove(q.m);q.m.material.dispose();parts.splice(i,1)}}
  zt-=dt;
  if(zt<0){zt=2.2;
    const night=lum<.28;
    if(mode===1){
      const hostN=mobs.reduce((a,m)=>a+(m.hos?1:0),0),under=P.y<colTop(Math.floor(P.x),Math.floor(P.z))-5,cap=under?7:night?Math.min(18,7+day()*2):0;
      if(hostN<cap)spawnHostile();
      if(night&&!wasNight)toast('Night falls... zombies are coming!')}
    if(mode===2)clearHostile();
    if(passiveN()<90&&passiveNear()<16)spawnPassive(false);
    wasNight=night}
  const g=get(Math.floor(c.x),Math.floor(c.y),Math.floor(c.z)),w=g==9?1:g==19?2:0;
  if(w!==wet){wet=w;const e=$('wet');e.style.display=w?'block':'none';e.style.background=w==2?'rgba(230,80,10,.6)':'rgba(30,90,210,.38)'}
  const open=rain>.05&&colTop(Math.floor(c.x),Math.floor(c.z))<=c.y+1;
  if(open){rainL.visible=true;rainM.opacity=.5*rain;
    for(let i=0;i<RN;i++){const q=rq[i];q[1]-=24*dt;if(q[1]<0)q[1]+=18;const x=c.x+q[0],y=c.y-8+q[1],z=c.z+q[2],o=i*6;rpos[o]=x;rpos[o+1]=y;rpos[o+2]=z;rpos[o+3]=x-.06;rpos[o+4]=y+.7;rpos[o+5]=z}
    rainG.attributes.position.needsUpdate=true}else rainL.visible=false;
  const kk=Math.min(1,rdt*12);
  remotePlayers.forEach(r=>{const p=r.g.position,ox=p.x,oz=p.z;
    p.x+=(r.tx-p.x)*kk;p.y+=(r.ty-p.y)*kk;p.z+=(r.tz-p.z)*kk;
    r.g.rotation.y+=angDiff(r.yaw,r.g.rotation.y)*kk;r.head.rotation.x=r.pitch;
    const spd=rdt>0?Math.hypot(p.x-ox,p.z-oz)/rdt:0;r.sp+=(spd-r.sp)*Math.min(1,rdt*8);
    r.wt+=rdt*Math.min(r.sp,8)*1.8;const a=Math.sin(r.wt)*.7*Math.min(1,r.sp/2);
    r.lg[0].rotation.x=a;r.lg[1].rotation.x=-a;r.ar[0].rotation.x=-a*.8;r.ar[1].rotation.x=a*.8});
  hand.visible=viewMode==0;const s2=swing;hand.position.set(.42-s2*.1,-.4+s2*.05+handBob,-.8-s2*.12);hand.rotation.set(-s2*.9,-.3,.35-s2*.9)}
function step(dt,rdt){
  fr++;
  const k=keys,f=(k.w||k.arrowup?1:0)-(k.s||k.arrowdown?1:0)+jy,s=(k.d?1:0)-(k.a?1:0)+jx;
  const L=Math.hypot(f,s)||1,m=Math.min(1,L)/L,fa=f*m,sa=s*m;
  const fb=get(Math.floor(P.x),Math.floor(P.y+.4),Math.floor(P.z)),inL=fb==19,inW=fb==9||inL;
  const run=(k.shift||runT)&&!P.fly,canRun=run&&(!mode||stamina>0);
  const sp=(P.fly?10:inL?1.6:inW?(C.sw||2.6):(canRun?6.4:4.4))*(inW?1:C.sp),up=jump||k[' '];
  const c=P.fly?Math.cos(pitch):1;
  const dx=(-Math.sin(yaw)*fa*c+Math.cos(yaw)*sa)*sp,dz=(-Math.cos(yaw)*fa*c-Math.sin(yaw)*sa)*sp;
  if(P.fly)P.vy=fa*Math.sin(pitch)*sp+(up?sp:0)-(k.shift?sp:0);
  else if(inW){P.vy=Math.max(P.vy-8*dt,-3);if(up)P.vy=3.2}
  else{P.vy=Math.max(P.vy-26*dt,-40);if(up&&P.ground)P.vy=8.2*C.jp}
  const bx_=mv('x',dx*dt),bz_=mv('z',dz*dt);
  if((bx_||bz_)&&P.ground&&(jx||jy)&&!P.fly&&!inW)P.vy=8.2;
  const by=mv('y',P.vy*dt);P.ground=false;
  if(by){if(P.vy<0){P.ground=true;if(P.vy<-15&&!P.fly&&C.fd!==0)hurt(Math.ceil((-P.vy-14)/3),0,0)}P.vy=0}
  if(P.y<-10){spawn()}
  atkT-=dt;bowT-=dt;hurtT-=dt;swing=Math.max(0,swing-dt*4);
  if(inL&&mode&&(lc+=dt)>.5){lc=0;hurt(2,0,0)}
  updMobs(dt);updArrows(dt);updTnt(dt);
  if(mode){
    const moving=!!(fa||sa),sprinting=canRun&&moving&&!inW&&!P.fly;
    if(sprinting)stamina=Math.max(0,stamina-dt*16);else stamina=Math.min(100,stamina+dt*12);
    if(mode===1){fc+=dt*(sprinting?2.2:1);if(fc>24){fc=0;if(food>0)food--}}
    if(food>=14&&hp<10){hh+=dt*(mode===2?2:1);if(hh>4){hh=0;hp++;drawHp()}}else hh=0;
    if(food<=0&&mode===1){sv2+=dt;if(sv2>3){sv2=0;if(hp>2)hurt(1,0,0)}}else sv2=0;
    if(!P.fly&&(ct-=dt)<0){ct=.5;touchCactus()}
  }
  if(peer&&peer.open){
    if((nt+=rdt)>.08){nt=0;sendPose()}
    if(roomHost&&(tsync+=rdt)>8){tsync=0;netBroadcast({t:'time',time})}
    if(hostConn&&!roomHost&&(pgt+=rdt)>3){pgt=0;if(hostConn.open)hostConn.send({t:'ping',ts:Date.now()})}}
  /* camera: first person with bob, third person behind, or front view; smoothed FOV and zoom */
  const ey=P.y+1.62,moving2=!!(fa||sa),walking=moving2&&P.ground&&!P.fly&&!inW;
  if(viewMode){
    const dir=viewMode==2?-1:1,cp=Math.cos(pitch),ddx=-Math.sin(yaw)*cp*dir,ddy=Math.sin(pitch)*dir,ddz=-Math.cos(yaw)*cp*dir;let t=4;
    for(let u=.3;u<=4;u+=.2)if(solid(Math.floor(P.x-ddx*u),Math.floor(ey-ddy*u),Math.floor(P.z-ddz*u))){t=Math.max(.3,u-.3);break}
    camD=t<camD?t:camD+(t-camD)*Math.min(1,rdt*8);
    cam.position.set(P.x-ddx*camD,ey-ddy*camD,P.z-ddz*camD);
    cam.rotation.y=yaw+(dir<0?Math.PI:0);cam.rotation.x=dir<0?-pitch:pitch;handBob=0}
  else{
    bobT+=dt*(walking?(canRun?11:8):0);bobA+=((walking&&settings.bob?1:0)-bobA)*Math.min(1,dt*8);
    const by2=Math.sin(bobT)*.045*bobA,bsx=Math.cos(bobT*.5)*.02*bobA;handBob=by2*.6;
    cam.position.set(P.x+Math.cos(yaw)*bsx,ey+by2,P.z-Math.sin(yaw)*bsx);cam.rotation.y=yaw;cam.rotation.x=pitch}
  plr.visible=viewMode>0;
  if(viewMode){const mvv=moving2?1:0;wt+=dt*9*mvv;plr.position.set(P.x,P.y,P.z);plr.rotation.y=yaw;legs[0].rotation.x=Math.sin(wt)*.7*mvv;legs[1].rotation.x=-Math.sin(wt)*.7*mvv}
  const fovT=(settings.fov+((canRun&&moving2&&!P.fly)?6:0)+(wet==1?-4:0))*(keys.z?.4:1);
  fovC+=(fovT-fovC)*Math.min(1,rdt*10);if(Math.abs(cam.fov-fovC)>.05){cam.fov=fovC;cam.updateProjectionMatrix()}
  /* time, weather, sky */
  time+=dt*.025;const l=lum=Math.max(0,Math.min(1,.5+Math.sin(time)*.9));
  const rt=(Math.sin(time*1.7+sd)+Math.sin(time*.61+sd*1.3))>1?1:0;rain+=(rt-rain)*Math.min(1,rdt*.35);
  const sa2=Math.sin(time),ca2=Math.cos(time);updSky(sa2,ca2,l);
  let fN=RD*.35*(1-.5*rain),fF=RD*(1-.35*rain);
  if(wet==1){fogC.copy(SC.wt).multiplyScalar(.25+.75*l);fN=.3;fF=20;dome.visible=false}else{fogC.copy(skyHor);dome.visible=true}
  scene.fog.color.copy(fogC);scene.fog.near=fN;scene.fog.far=fF;ren.setClearColor(fogC);
  U.uFog.value.copy(fogC);U.uFogNear.value=fN;U.uFogFar.value=fF;U.uDay.value=l;U.uRain.value=rain;U.uTime.value=performance.now()/1000;
  for(let i=0;i<3&&bq.length;i++)rebuild(bq.shift());
  if(dirty.size){dirty.forEach(rebuild);dirty.clear()}
  if(fr%20==0)cullChunks();
  extra(dt,rdt);
  RAY=ray();hl.visible=!!RAY;if(RAY)hl.position.set(RAY.x+.5,RAY.y+.5,RAY.z+.5);
  if(fr%4==0){const tg=aimMob(6),s2=tg?tg.t.n+'  '+Math.max(0,tg.hp)+'/'+tg.t.hp:'';if(s2!==lastTgt){lastTgt=s2;$('tgt').textContent=s2}}
  if(fr%20==0){drawMini();uiNow()}
}
function ray(){const cp=Math.cos(pitch),d={x:-Math.sin(yaw)*cp,y:Math.sin(pitch),z:-Math.cos(yaw)*cp},o={x:P.x,y:P.y+1.62,z:P.z};let prev=null;
  for(let t=0;t<C.rc;t+=.03){const x=Math.floor(o.x+d.x*t),y=Math.floor(o.y+d.y*t),z=Math.floor(o.z+d.z*t),b=get(x,y,z);
    if(b&&b!=9&&b!=19)return{x,y,z,prev,t};prev=[x,y,z]}return null}
function saveNow(){if(hostConn&&!roomHost)return;try{LS.setItem(SK,JSON.stringify({seed,edits,mode,ci,inv,xp,ar:armorId,time:+time.toFixed(2),hp,food,p:[+P.x.toFixed(2),+P.y.toFixed(2),+P.z.toFixed(2)]}))}catch(e){}}
function save(){clearTimeout(st);st=setTimeout(saveNow,600)}
function brk(){
  swing=1;
  if(punch())return;
  const r=ray();
  if(r&&r.y>0){const b0=get(r.x,r.y,r.z);if(b0==17)return;
    if(b0==21){ignite(r.x,r.y,r.z,2.4);save();return}
    burst(r.x,r.y,r.z,b0);
    if(mode){inv[b0]=(inv[b0]||0)+1;addXP(1);if(b0==6&&Math.random()<.08)addItem(51,1);badges()}
    snd(150,.1,"sawtooth");set(r.x,r.y,r.z,0);edits.push(ix(r.x,r.y,r.z),0);markB(r.x,r.z,b0,0);netBlock(r.x,r.y,r.z,0);save()}}
function plc(){
  swing=1;const r=ray();if(!r||!r.prev||(mode&&!(inv[sel]>0)))return;
  const[x,y,z]=r.prev,o=get(x,y,z);if(o&&o!=9&&o!=19)return;
  set(x,y,z,sel);if(hit()){set(x,y,z,o);return}
  snd(300,.07);if(mode){inv[sel]--;badges()}edits.push(ix(x,y,z),sel);markB(x,z,o,sel);netBlock(x,y,z,sel);save()}
function tf(){if(mode)return;P.fly=!P.fly;$('fly').textContent='Fly: '+(P.fly?'on':'off')}
function cycleView(){viewMode=(viewMode+1)%3;camD=4;toast(['First person','Third person','Front view'][viewMode])}

/* ---------- hotbar ---------- */
const bar=$('bar');
const bpLabel=()=>sel<30?'Place':sel==36?'Shoot':FOOD[sel]?'Eat':ARMP[sel]?'Wear':'Swing';
function pick(b){
  sel=b;[...bar.children].forEach(s=>s.classList.toggle('on',+s.dataset.id==b));
  $('nm').textContent=N[b];$('bp').textContent=bpLabel();
  const s=bar.querySelector('.slot.on');
  if(s&&s.offsetWidth){if(s.offsetLeft<bar.scrollLeft)bar.scrollLeft=s.offsetLeft-4;else if(s.offsetLeft+s.offsetWidth>bar.scrollLeft+bar.clientWidth)bar.scrollLeft=s.offsetLeft+s.offsetWidth-bar.clientWidth+4}
  drawIcon(hx,b,32);handTex.needsUpdate=true}
for(const id of ALL){const s=document.createElement('div'),c=document.createElement('canvas');s.className='slot';s.dataset.id=id;c.width=c.height=32;
  drawIcon(c.getContext('2d'),id,32);s.appendChild(c);s.appendChild(document.createElement("i"));s.onclick=()=>pick(id);bar.appendChild(s)}
function badges(){
  const v=vis();
  [...bar.children].forEach(s=>{const id=+s.dataset.id,n=inv[id]||0;s.style.display=v.includes(id)?'':'none';s.lastChild.textContent=mode?n:'';s.style.opacity=mode&&!n&&id<30?.45:1});
  if(!v.includes(sel))pick(v[0]||1)}

/* ---------- input ---------- */
const Lk=$('look');let lid=null,lx=0,ly=0;
const look=(dx,dy)=>{yaw-=dx*.005;pitch=Math.max(-1.55,Math.min(1.55,pitch-dy*.005))};
Lk.oncontextmenu=e=>e.preventDefault();
Lk.addEventListener('pointerdown',e=>{
  if(e.pointerType=='mouse'){if(document.pointerLockElement!=Lk){try{Lk.requestPointerLock()}catch(_){}}else e.button==2?useItem():brk()}
  lid=e.pointerId;lx=e.clientX;ly=e.clientY;Lk.setPointerCapture(e.pointerId)});
Lk.addEventListener('pointermove',e=>{
  if(document.pointerLockElement==Lk)look(e.movementX,e.movementY);
  else if(e.pointerId==lid){look(e.clientX-lx,e.clientY-ly);lx=e.clientX;ly=e.clientY}});
Lk.addEventListener('pointerup',()=>{lid=null});Lk.addEventListener('pointercancel',()=>{lid=null});
const J=$('joy'),K=$('knob');let jid=null;
function jm(e){const r=J.getBoundingClientRect();let dx=(e.clientX-r.left-62)/50,dy=(e.clientY-r.top-62)/50;const l=Math.hypot(dx,dy);if(l>1){dx/=l;dy/=l}
  jx=Math.abs(dx)<.12?0:dx;jy=Math.abs(dy)<.12?0:-dy;K.style.transform=`translate(${dx*40}px,${dy*40}px)`}
J.onpointerdown=e=>{jid=e.pointerId;J.setPointerCapture(jid);jm(e)};
J.onpointermove=e=>{if(e.pointerId==jid)jm(e)};
J.onpointerup=J.onpointercancel=e=>{if(e.pointerId==jid){jid=null;jx=jy=0;K.style.transform=''}};
$('bj').onpointerdown=()=>jump=true;$('bj').onpointerup=$('bj').onpointercancel=$('bj').onpointerleave=()=>jump=false;
const rep=(id,fn,ms)=>{const b=$(id);let t;b.onpointerdown=()=>{fn();clearInterval(t);t=setInterval(fn,ms)};b.onpointerup=b.onpointercancel=b.onpointerleave=()=>clearInterval(t)};
rep('bb',brk,230);rep('bp',useItem,300);$('fly').onclick=tf;
$('vw').onclick=cycleView;
$('eatb').onclick=eatBest;
$('runb').onclick=()=>{runT=!runT;$('runb').classList.toggle('on',runT)};
function openCraft(){$('craft').style.display='flex';renderRecipes();paused=true}
addEventListener('keydown',e=>{
  if(e.target&&/INPUT|TEXTAREA/.test(e.target.tagName))return;
  const k=e.key.toLowerCase();keys[k]=1;
  if(k=='f')tf();if(k=='v')cycleView();if(k=='e')eatBest();if(k=='c'&&mode&&!paused)openCraft();
  if(k=='t'&&peer&&!paused){e.preventDefault();try{document.exitPointerLock()}catch(_){}$('chatin').focus()}
  if(k.length==1&&k>='1'&&k<='9'){const v=vis();if(v[+k-1])pick(v[+k-1])}
  if(k==' ')e.preventDefault()});
addEventListener('keyup',e=>{keys[e.key.toLowerCase()]=0});
addEventListener('wheel',e=>{const v=vis(),i=v.indexOf(sel);if(!v.length)return;pick(v[(i+(e.deltaY>0?1:v.length-1))%v.length])},{passive:true});

/* ---------- world / menu ---------- */
function start(s,ed,iv,pos){
  gen(s);edits=ed;inv=iv||{1:8,2:16,3:12,5:8,7:12,10:4};
  for(let i=0;i<ed.length;i+=2)W[ed[i]]=ed[i+1];
  computeHM();
  hp=10;food=20;stamina=100;drawHp();spawn();
  if(pos&&pos.length==3&&pos[0]>1&&pos[2]>1&&pos[0]<X-1&&pos[2]<Z-1){const o={x:P.x,y:P.y,z:P.z};P.x=pos[0];P.y=pos[1];P.z=pos[2];if(hit(P)){P.x=o.x;P.y=o.y;P.z=o.z}}
  popMobs();queueAll(true);badges();save()}
function newWorld(){xp=0;armorId=0;time=1;start(Math.random()*1e9|0,[])}
function loadProfile(){
  const sv=jget(SK,null);
  if(sv&&sv.edits){xp=sv.xp||0;armorId=ARMP[sv.ar]?sv.ar:0;time=sv.time||1;start(sv.seed,sv.edits,sv.inv,sv.p);hp=Math.max(1,Math.min(10,sv.hp||10));food=sv.food==null?20:Math.max(0,Math.min(20,sv.food))}
  else{xp=0;armorId=0;time=1;start(Math.random()*1e9|0,[])}
  setMode(sv&&sv.mode!=null?sv.mode:0);setChar(sv&&sv.ci||0);pick(sel>0?sel:1);drawHp();uiNow()}
function setMode(m){
  mode=m;if(m)P.fly=false;
  $('fly').style.display=m?'none':'';$('fly').textContent='Fly: '+(P.fly?'on':'off');$('hp').style.display=m?'':'none';$('md').textContent=MD[m];
  [...$('modes').children].forEach((b,i)=>b.classList.toggle('on',i==m));
  $('surv').style.display=m?'block':'none';$('craftBtn').style.display=m?'block':'none';$('eatb').style.display=m?'':'none';$('spawnbox').style.display=m==0?'block':'none';
  if(m===0){food=20;stamina=100}
  if(m===2)clearHostile();
  drawHp();badges();save()}
function setChar(i){ci=i;C=CHR[i];mkPlr();$('perk').textContent=C.d;[...$('chars').children].forEach((b,j)=>b.classList.toggle('on',j==i));save()}
const mkBtn=(box,t,f)=>{const b=document.createElement('button');b.textContent=t;b.onclick=f;$(box).appendChild(b);return b};
MODES.forEach((m,i)=>mkBtn('modes',m,()=>setMode(i)));CHR.forEach((c,i)=>mkBtn('chars',c.n,()=>setChar(i)));
const RDS=[['Short',64],['Normal',96],['Far',140],['Ultra',190]];
function markRD(){[...$('rds').children].forEach((b,i)=>b.classList.toggle('on',RDS[i][1]==RD))}
RDS.forEach(([n,v])=>mkBtn('rds',n,()=>{setRD(v);settings.rd=v;saveSettings();markRD();cullChunks()}));
function markGQ(){[...$('gqs').children].forEach((b,i)=>b.classList.toggle('on',i==settings.gq))}
GQ.forEach(([n],i)=>mkBtn('gqs',n,()=>setGQ(i)));
const bobB=mkBtn('opts','Head bob: '+(settings.bob?'on':'off'),()=>{settings.bob=!settings.bob;bobB.textContent='Head bob: '+(settings.bob?'on':'off');saveSettings()});
$('fov').value=settings.fov;$('fovv').textContent=settings.fov;
$('fov').oninput=()=>{settings.fov=+$('fov').value;$('fovv').textContent=settings.fov;fovC=settings.fov;cam.fov=fovC;cam.updateProjectionMatrix();saveSettings()};
function spawnNear(type){
  let n=0;
  for(let i=0;i<3;i++){const d=6+i*1.5,x=Math.floor(P.x-Math.sin(yaw)*d+Math.cos(yaw)*(i-1)*2),z=Math.floor(P.z-Math.cos(yaw)*d-Math.sin(yaw)*(i-1)*2),h=solidTop(x,z);
    if(h>0){spawnMob(type,x+.5,h+1.01,z+.5);n++}}
  if(!n)toast('No room to spawn here');else closeMenu()}
Object.keys(MT).forEach(t=>mkBtn('spawns',MT[t].n,()=>spawnNear(t)));
mkBtn('spawns','Clear all',()=>{while(mobs.length)removeMob(mobs[0]);toast('Mobs cleared')});
const openMenu=()=>{paused=true;$('menu').style.display='flex'},closeMenu=()=>{paused=false;$('menu').style.display='none'};
$('nw').onclick=openMenu;$('play').onclick=closeMenu;$('nw2').onclick=()=>{newWorld();closeMenu()};
$('openmulti').onclick=()=>{$('multi').style.display='flex';$('mname').value=username};
$('closemulti').onclick=()=>{$('multi').style.display='none'};
$('host').onclick=hostRoom;$('join').onclick=joinRoom;$('leave').onclick=leaveRoom;
$('copyroom').onclick=()=>{const v=$('room').value.trim();if(v){clip(v);mstat('Room code copied: '+v)}};
$('copylink').onclick=()=>{const v=$('room').value.trim();if(v){clip(inviteLink());mstat('Invite link copied. Friends open it and tap Join.')}};
$('room').addEventListener('keydown',e=>{if(e.key==='Enter')joinRoom()});
$('openacct').onclick=()=>showAccount(false);
$('closeacct').onclick=()=>{$('account').style.display='none'};
$('tabin').onclick=()=>setAuth('in');$('tabup').onclick=()=>setAuth('up');
$('authgo').onclick=authGo;
$('acctname').addEventListener('keydown',e=>{if(e.key==='Enter')authGo()});$('acctpass').addEventListener('keydown',e=>{if(e.key==='Enter')authGo()});$('acctpass2').addEventListener('keydown',e=>{if(e.key==='Enter')authGo()});
$('guest').onclick=()=>login({id:'guest',name:(LS.getItem('blockland_name')||'Player').slice(0,18),type:'guest'});
$('logout').onclick=()=>{saveNow();joined=false;if(peer)leaveRoom();session=null;try{LS.removeItem('bl_session')}catch(e){}login({id:'guest',name:(LS.getItem('blockland_name')||'Player').slice(0,18),type:'guest'},true);showAccount(true)};
$('savegcid').onclick=()=>{const v=$('gcid').value.trim();if(v)LS.setItem('bl_gcid',v);else LS.removeItem('bl_gcid');amsg(v?'Client ID saved':'Client ID cleared');initGoogle()};
$('craftBtn').onclick=openCraft;
$('closecraft').onclick=()=>{$('craft').style.display='none';paused=false};
$('chatsend').onclick=()=>{const v=$('chatin').value.trim();if(!v)return;chatMsg(username,v);const m={t:'chat',name:username,msg:v};if(roomHost)netBroadcast(m);else if(hostConn&&hostConn.open)hostConn.send(m);$('chatin').value=''};
$('chatin').addEventListener('keydown',e=>{if(e.key==='Enter')$('chatsend').click();if(e.key==='Escape')$('chatin').blur()});

/* ---------- boot ---------- */
setRD(settings.rd);markRD();rs();setGQ(Math.min(2,Math.max(0,settings.gq|0)));
(function(){
  let s0=jget('bl_session',null);
  if(s0&&s0.id&&(s0.type!=='local'||accounts[String(s0.id).slice(2)])){
    login(s0)}
  else{
    login({id:'guest',name:(LS.getItem('blockland_name')||'Player').slice(0,18),type:'guest'},true);
    showAccount(true)}
  try{const q=new URLSearchParams(location.search).get('room');if(q)pendingRoom=q.slice(0,40)}catch(e){}
  if(pendingRoom&&session){const r=pendingRoom;pendingRoom=null;$('multi').style.display='flex';$('room').value=r}
})();
setAuth('in');openMenu();
setInterval(saveNow,10000);addEventListener('pagehide',saveNow);
(function loop(now){requestAnimationFrame(loop);const dt=Math.min(.05,(now-last)/1000);last=now;try{step(paused?0:dt,dt)}catch(e){console.error(e)}ren.render(scene,cam)})(last);
</script></body></html>
