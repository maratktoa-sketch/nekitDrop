<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NekitDrop</title>
<style>
:root {
  --bg: #0b0d14;
  --panel: #171c2b;
  --line: #30384e;
  --text: #f4f5ff;
  --muted: #a1abc1;
  --accent: #895cf6;
  --cyan: #37d8ff;
  --gold: #ffc857;
}
* { box-sizing: border-box; }
body {
  margin: 0;
  background: radial-gradient(ellipse at top,#20203c,#0b0d14 65%);
  color: var(--text);
  font-family: Arial, sans-serif;
  min-height: 100vh;
}
button,input { font: inherit; }
button {
  border: 0;
  padding: 11px 15px;
  border-radius: 10px;
  background: var(--accent);
  color: white;
  font-weight: bold;
  cursor: pointer;
}
button:hover { filter: brightness(1.2); }
.secondary { background: #282f43; }
.wrap { max-width: 1100px; margin: auto; padding: 16px; }
header,.row,.wallet,.nav,.actions {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  flex-wrap: wrap;
}
header { padding: 12px 0 20px; }
.logo { font-size: 26px; font-weight: 900; }
.logo span { color: var(--cyan); }
.pill,.panel,.card {
  background: var(--panel);
  border: 1px solid var(--line);
  border-radius: 15px;
}
.pill { padding: 10px 14px; font-weight: bold; }
.pill b { color: var(--gold); }
.panel { padding: 20px; margin-bottom: 15px; }
.nav { justify-content: flex-start; margin-bottom: 18px; }
.nav button { background: #1b2030; }
.nav button.active { background: var(--accent); }
.view { display: none; }
.view.active { display: block; }
.hero { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
h1 { font-size: clamp(30px,5vw,48px); }
.muted { color: var(--muted); }
.cyan { color: var(--cyan); }
.gold { color: var(--gold); }
.stats { display: grid; grid-template-columns: repeat(3,1fr); gap: 10px; margin-top: 15px; }
.stat { padding: 13px; background: #101421; border-radius: 12px; }
.stat strong { display: block; font-size: 22px; }
.grid { display: grid; grid-template-columns: repeat(auto-fit,minmax(145px,1fr)); gap: 10px; }
.card { padding: 13px; background: #111522; }
.card .emoji { font-size: 30px; }
.card h3 { margin: 8px 0; }
.rarity { font-size: 11px; text-transform: uppercase; font-weight: bold; }
.secret { border-color: #ff5c8d; }
.ultra { border-color: #bd72ff; }
.legend { border-color: #ffc857; }
.mega { border-color: #ff8e52; }
.arcana { border-color: #4dd8ff; }
.mythic { border-color: #a6e86b; }
.epic { border-color: #8997ff; }
.rare { border-color: #667085; }
table { width: 100%; border-collapse: collapse; }
td,th { text-align: left; padding: 10px; border-bottom: 1px solid var(--line); }
th { color: var(--muted); }
.table-wrap { overflow-x: auto; }
.item,.quest,.achievement {
  background: #111522;
  border: 1px solid var(--line);
  border-radius: 12px;
  padding: 12px;
  margin: 8px 0;
}
.item { display: flex; align-items: center; gap: 10px; flex-wrap: wrap; }
.grow { flex: 1; }
input {
  min-width: 0;
  padding: 12px;
  background: #0d111c;
  color: white;
  border: 1px solid var(--line);
  border-radius: 10px;
}
.inputrow { display: flex; gap: 8px; flex-wrap: wrap; }
.inputrow input { flex: 1; }
.progress { height: 7px; background: #30384e; border-radius: 10px; margin: 10px 0; overflow: hidden; }
.progress i { display: block; height: 100%; background: var(--cyan); }
.badge { background: #282f43; padding: 5px 8px; border-radius: 8px; font-size: 12px; }
#roulette { width: 100%; max-width: 440px; display: block; margin: auto; }
#result { min-height: 30px; color: var(--gold); font-weight: bold; text-align: center; }
.modal-bg {
  position: fixed; inset: 0; z-index: 10;
  display: flex; align-items: center; justify-content: center;
  padding: 20px; background: #000c;
}
.modal { max-width: 430px; width: 100%; text-align: center; }#toast {
  position: fixed; bottom: 15px; left: 50%; transform: translateX(-50%);
  padding: 12px 18px; border-radius: 12px;
  background: #282f43; border: 1px solid var(--line); z-index: 20;
}
.hidden { display: none !important; }
@media(max-width:720px) {
  .hero { grid-template-columns: 1fr; }
  .panel { padding: 14px; }
  .stats { grid-template-columns: repeat(2,1fr); }
}
</style>
</head>
<body>
<div class="wrap">
<header>
  <div class="logo">Nekit<span>Drop</span> ⚡</div>
  <div class="wallet">
    <div class="pill">⚡ <b id="lights">3</b></div>
    <div class="pill">$ <b id="dollars">0</b></div>
  </div>
</header>

<nav class="nav">
  <button class="active" data-view="home">Главная</button>
  <button data-view="game">🎰 Играть</button>
  <button data-view="inventory">🎒 Инвентарь</button>
  <button data-view="profile">👤 Профиль</button>
  <button data-view="shop">🛒 Магазин</button>
  <button data-view="quests">📜 Квесты</button>
  <button data-view="achievements" id="achButton">🏆 Достижения</button>
</nav>

<section class="view active" id="home">
  <div class="hero">
    <div class="panel">
      <div class="cyan">РУЛЕТКА ПЕРСОНАЖЕЙ</div>
      <h1>Поймай своего <span class="cyan">легендарного</span></h1>
      <p class="muted">Крути рулетку, собирай персонажей, продавай их и получай внутриигровые доллары!</p>
      <div class="actions">
        <button data-go="game">Начать играть ⚡</button>
        <button class="secondary" data-go="shop">Магазин</button>
      </div>
      <div class="stats">
        <div class="stat"><strong>11</strong><span class="muted">Персонажей</span></div>
        <div class="stat"><strong>100%</strong><span class="muted">Шансы</span></div>
        <div class="stat"><strong>1 ⚡</strong><span class="muted">Одна крутка</span></div>
      </div>
    </div>
    <div class="panel">
      <h2>🔗 Бонусы</h2>
      <p class="muted">Забирай молнии за переходы.</p>
      <div class="item">
        <div class="grow"><b>Telegram</b><br><span class="muted">+3 ⚡</span></div>
        <a href="https://t.me/ymarat123tube" target="_blank">Открыть ↗</a>
        <button id="telegram">Забрать</button>
      </div>
      <div class="item">
        <div class="grow"><b>TikTok</b><br><span class="muted">+3 ⚡</span></div>
        <a href="https://www.tiktok.com/@k0tenok500" target="_blank">Открыть ↗</a>
        <button id="tiktok">Забрать</button>
      </div>
      <p class="muted">Демо-версия не проверяет подписку автоматически.</p>
    </div>
  </div>
  <h2>Шансы выпадения</h2>
  <div class="panel table-wrap">
    <table>
      <thead><tr><th>Редкость</th><th>Персонаж</th><th>Шанс</th><th>Цена</th></tr></thead>
      <tbody id="chanceTable"></tbody>
    </table>
  </div>
</section>

<section class="view" id="game">
  <div class="hero">
    <div class="panel">
      <h2>🎰 Рулетка NekitDrop</h2>
      <canvas id="roulette" width="440" height="440"></canvas>
      <div id="result">Готов крутить? Удачи!</div>
      <div class="actions">
        <button id="spin">Крутить за 1 ⚡</button>
        <button id="spin10" class="secondary">Крутить ×10</button>
      </div>
    </div>
    <div class="panel">
      <h2>Как играть?</h2>
      <p class="muted">1. Используй молнии для круток.</p>
      <p class="muted">2. Продавай персонажей за доллары.</p>
      <p class="muted">3. Покупай молнии в магазине.</p>
      <p class="muted">4. Выполняй квесты и достижения.</p>
      <h3>Последние выпадения</h3>
      <div id="lastDrops" class="muted">Пока пусто.</div>
      <button class="secondary" data-go="home">?</button>
    </div>
  </div>
</section>

<section class="view" id="inventory">
  <div class="row"><h1>🎒 Инвентарь</h1><button id="sellAll">Продать всё</button></div>
  <div class="panel" id="inventoryList"></div>
</section><section class="view" id="profile">
  <div class="panel">
    <h1>👤 Профиль</h1>
    <label>Никнейм</label>
    <div class="inputrow" style="margin:8px 0 20px">
      <input id="nickname" maxlength="24" placeholder="Твой никнейм">
      <button id="saveNick">Сохранить</button>
    </div>
    <div class="stats">
      <div class="stat"><strong id="spinsCount">0</strong><span class="muted">Круток</span></div>
      <div class="stat"><strong id="foundCount">0/11</strong><span class="muted">Найдено</span></div>
      <div class="stat"><strong id="soldCount">$0</strong><span class="muted">Продано на</span></div>
    </div>
    <h2>Промокод</h2>
    <div class="inputrow">
      <input id="promoInput" placeholder="#mrtPromBalanc30">
      <button id="promoButton">Активировать</button>
    </div>
    <h2>Ежедневная награда</h2>
    <button id="daily">Получить $3 + 5 ⚡</button>
    <p id="dailyStatus" class="muted"></p>
    <h2>Быстрые действия</h2>
    <button data-go="quests">Квесты</button>
    <button data-go="achievements" class="secondary">Достижения</button>
    <button id="reset" class="secondary">Сбросить прогресс</button>
  </div>
</section>

<section class="view" id="shop">
  <div class="panel">
    <h1>🛒 Магазин молний</h1>
    <p class="muted">Покупай молнии за доллары. Лимит одной покупки — $1000.</p>
    <p>Баланс: <b id="shopBalance">$0</b></p>
    <label>Количество молний</label>
    <input id="shopAmount" type="number" min="1" max="10000" value="10" style="width:100%;margin-top:8px">
    <p>Стоимость: <b id="shopCost">$0</b></p>
    <button id="buy">Купить молнии</button>
  </div>
</section>

<section class="view" id="quests">
  <h1>📜 Ежедневные квесты</h1>
  <p class="muted">Каждый день выбираются 4 случайных задания.</p>
  <div id="questList"></div>
</section>

<section class="view" id="achievements">
  <h1>🏆 Достижения</h1>
  <div id="achievementList"></div>
</section>
</div>

<div id="modalRoot"></div>
<div id="toast" class="hidden"></div>

<script>
"use strict";

const SAVE_KEY = "nekitsave_v7";

const characters = [
 {name:"Nekit",chance:.5,price:67,rarity:"Секретный",cls:"secret",emoji:"👑"},
 {name:"Sygak",chance:1.5,price:40,rarity:"Ультра",cls:"ultra",emoji:"🦊"},
 {name:"Mellstroy",chance:1.5,price:40,rarity:"Ультра",cls:"ultra",emoji:"💎"},
 {name:"Воздухан",chance:3,price:26,rarity:"Легенда",cls:"legend",emoji:"🪽"},
 {name:"ДунДук",chance:3,price:26,rarity:"Легенда",cls:"legend",emoji:"🐸"},
 {name:"Типо Каутой",chance:8,price:13,rarity:"Мега",cls:"mega",emoji:"🔥"},
 {name:"floatup",chance:11,price:10,rarity:"Аркана",cls:"arcana",emoji:"🌊"},
 {name:"MUSOR",chance:14,price:6,rarity:"Мифик",cls:"mythic",emoji:"☠️"},
 {name:"67",chance:16,price:3,rarity:"Эпик",cls:"epic",emoji:"🎭"},
 {name:"МАНGO",chance:19,price:1.5,rarity:"Сверхредкий",cls:"rare",emoji:"🥭"},
 {name:"Розетка",chance:22.5,price:1,rarity:"Редкий",cls:"rare",emoji:"🔌"}
];

const promoCodes = {
 "#mrtPromBalanc30":{type:"lights",amount:30,limit:1},
 "#mrtPromDollar20":{type:"dollars",amount:20,limit:1},
 "#mrtPromBalanc2":{type:"lights",amount:2,limit:2},
 "#mrtPromBalanc10":{type:"lights",amount:10,limit:2},
 "#mrtPromBalanc15":{type:"lights",amount:15,limit:2},
 "#mrtPromBalanc100":{type:"lights",amount:100,limit:1},
 "#mrtPromDollar10":{type:"dollars",amount:10,limit:2},
 "#mrtPromDollar50":{type:"dollars",amount:50,limit:1}
};

const questTemplates = [
 {id:"spinsRandom",title:"Прокрутить дроп",goal:()=>rand(50,140),kind:"spins",reward:"$40"},
 {id:"lightsRandom",title:"Получить 30–80 молний",goal:()=>rand(30,80),kind:"lights",reward:"20 ⚡ + $5"},
 {id:"earn100",title:"Получить $100",goal:100,kind:"earned",reward:"10 ⚡"},
 {id:"daily",title:"Забрать ежедневную награду",goal:1,kind:"daily",reward:"$3 + 5 ⚡"},
 {id:"spin200",title:"Крутнуть дроп 200 раз",goal:200,kind:"spins",reward:"x2 к продаже на 1 минуту"},{id:"spin80",title:"Крутнуть дроп 80 раз",goal:80,kind:"spins",reward:"x3 к продаже на 30 секунд"},
 {id:"bonuses",title:"Забрать Telegram, TikTok и ежедневку",goal:3,kind:"bonuses",reward:"$5"},
 {id:"secret",title:"Ввести #stsProm8t",goal:1,kind:"secret",reward:"$30"}
];

const achievementTemplates = [
 {id:"finger",title:"Чёткий палец!",desc:"Прокрутить рулетку 1000 раз.",reward:"$5"},
 {id:"legend",title:"Легенда (ты)",desc:"Выбить всех 11 персонажей.",reward:"20 ⚡"},
 {id:"ultra2",title:"КАК",desc:"Выбить Ультра-персонажа 2 раза подряд.",reward:"$50"},
 {id:"mell",title:"Хамам",desc:"Выбить Mellstroy.",reward:"$20"}
];

function rand(a,b){return Math.floor(Math.random()*(b-a+1))+a}
function today(){return new Date().toLocaleDateString("sv-SE")}
function defaultState(){
 return {
  lights:3,dollars:0,nickname:"Игрок",inventory:[],
  spins:0,soldTotal:0,found:{},promoUses:{},
  bonuses:{telegram:false,tiktok:false},welcome:false,
  dailyDate:"",questDate:"",quests:[],achievementDone:{},
  achievementClaimed:{},lastDrops:[],ultraStreak:0,
  multiplier:1,multiplierUntil:0
 };
}
function load(){
 try{
  const data=JSON.parse(localStorage.getItem(SAVE_KEY));
  return data ? {...defaultState(),...data} : defaultState();
 }catch(e){return defaultState()}
}
let state=load();
let spinning=false;

function save(){
 localStorage.setItem(SAVE_KEY,JSON.stringify(state));
 renderAll();
}
function money(n){
 return "$"+Number(n||0).toFixed(2).replace(/\.00$/,"").replace(/(\.\d)0$/,"$1");
}
function toast(message){
 const el=document.getElementById("toast");
 el.textContent=message;
 el.classList.remove("hidden");
 clearTimeout(toast.timer);
 toast.timer=setTimeout(()=>el.classList.add("hidden"),2500);
}
function go(id){
 document.querySelectorAll(".view").forEach(v=>v.classList.toggle("active",v.id===id));
 document.querySelectorAll("[data-view]").forEach(b=>b.classList.toggle("active",b.dataset.view===id));
 if(id==="game") drawWheel();
 window.scrollTo({top:0,behavior:"smooth"});
}
document.querySelectorAll("[data-view]").forEach(b=>b.onclick=()=>go(b.dataset.view));
document.querySelectorAll("[data-go]").forEach(b=>b.onclick=()=>go(b.dataset.go));

function findChar(name){return characters.find(c=>c.name===name)}
function addLights(n){state.lights+=n;progressQuest("lights",n)}
function addDollars(n){state.dollars=+(state.dollars+n).toFixed(2);progressQuest("earned",n)}
function progressQuest(kind,n){
 state.quests.forEach(q=>{
  const t=questTemplates.find(t=>t.id===q.id);
  if(t && t.kind===kind && !q.claimed) q.progress=Math.min(q.goal,q.progress+n);
 });
}
function ensureQuests(){
 if(state.questDate===today() && state.quests.length) return;
 state.questDate=today();
 const pool=[...questTemplates],selected=[];
 while(selected.length<4) selected.push(pool.splice(rand(0,pool.length-1),1)[0]);
 state.quests=selected.map(t=>({
  id:t.id,goal:typeof t.goal==="function"?t.goal():t.goal,
  progress:0,claimed:false
 }));
 localStorage.setItem(SAVE_KEY,JSON.stringify(state));
}
function renderChances(){
 document.getElementById("chanceTable").innerHTML=characters.map(c=>
 <tr><td class="${c.cls}">${c.rarity}</td><td>${c.emoji} ${c.name}</td>
 <td>${c.chance}%</td><td>${money(c.price)}</td></tr>).join("");
}
function renderInventory(){
 const el=document.getElementById("inventoryList");
 if(!state.inventory.length){el.innerHTML='<p class="muted">Инвентарь пуст. Крути рулетку!</p>';return}
 const counts={};
 state.inventory.forEach(it=>{
  if(!counts[it.name]) counts[it.name]={count:0,price:it.price};
  counts[it.name].count++;
  counts[it.name].price=it.price;
 });
 el.innerHTML=Object.entries(counts).map(([name,v])=>{
  const c=findChar(name);
  return `<div class="item"><span>${c.emoji}</span>
   <div class="grow"><b>${name} ×${v.count}</b><br><span class="muted">${c.rarity} · ${money(v.price)} за штуку</span></div>
   <button data-sell="${name}">Продать 1</button>
   <button class="secondary" data-sellall="${name}">Продать все</button></div>;
 }).join("");
 el.querySelectorAll("[data-sell]").forEach(b=>b.onclick=()=>sellOne(b.dataset.sell));
 el.querySelectorAll("[data-sellall]").forEach(b=>b.onclick=()=>sellNamed(b.dataset.sellall));
}
function renderProfile(){
 document.getElementById("nickname").value=state.nickname;
 document.getElementById("spinsCount").textContent=state.spins;
 document.getElementById("foundCount").textContent=Object.keys(state.found).filter(k=>state.found[k]>0).length+"/11";
 document.getElementById("soldCount").textContent=money(state.soldTotal);
 document.getElementById("dailyStatus").textContent=state.dailyDate===today()?"Награда уже получена сегодня.":"";
 document.getElementById("daily").disabled=state.dailyDate===today();
}
function shopPrice(n){
 let sum=0;
 for(let i=0;i<n;i++)sum+=Math.max(.1,1.25-i*.001);
 return Math.round(sum*100)/100;
}
function renderShop(){
 const n=Math.max(1,Math.min(10000,Math.floor(+document.getElementById("shopAmount").value||1)));
 const cost=shopPrice(n);
 document.getElementById("shopBalance").textContent=money(state.dollars);
 document.getElementById("shopCost").textContent=money(cost);
 document.getElementById("buy").disabled=cost>state.dollars||cost>1000;
}
function renderQuests(){
 ensureQuests();
 document.getElementById("questList").innerHTML=state.quests.map(q=>{
  const t=questTemplates.find(t=>t.id===q.id);
  let progress=q.progress;
  if(q.id==="bonuses") progress=Number(state.bonuses.telegram)+Number(state.bonuses.tiktok)+Number(state.dailyDate===today());
  return <div class="quest"><div class="row"><b>${t.title}</b><span class="badge">${t.reward}</span></div>
   <div class="progress"><i style="width:${Math.min(100,progress/q.goal*100)}%"></i></div>
   <div class="row"><span class="muted">${Math.min(progress,q.goal)} / ${q.goal}</span>
   ${q.claimed?"Получено ✓":progress>=q.goal?<button data-quest="${q.id}">Забрать</button>:"В процессе"}</div></div>;
 }).join("");
 document.querySelectorAll("[data-quest]").forEach(b=>b.onclick=()=>claimQuest(b.dataset.quest));
}
function checkAchievements(){
 if(state.spins>=1000)state.achievementDone.finger=true;
 if(characters.every(c=>state.found[c.name]>0))state.achievementDone.legend=true;
}
function renderAchievements(){
 checkAchievements();
 const count=achievementTemplates.filter(a=>state.achievementDone[a.id]&&!state.achievementClaimed[a.id]).length;
 document.getElementById("achButton").textContent="🏆 Достижения"+(count?" 🔴 "+count:"");
 document.getElementById("achievementList").innerHTML=achievementTemplates.map(a=>
 <div class="achievement"><div class="row"><b>${a.title}</b><span class="badge">${a.reward}</span></div>
 <p class="muted">${a.desc}</p><div class="row">
 <span>${state.achievementClaimed[a.id]?"Получено ✓":state.achievementDone[a.id]?"Выполнено":"Не выполнено"}</span>
 ${state.achievementDone[a.id]&&!state.achievementClaimed[a.id]?<button data-ach="${a.id}">Забрать</button>:""}</div></div>).join("");
 document.querySelectorAll("[data-ach]").forEach(b=>b.onclick=()=>claimAchievement(b.dataset.ach));
}
function renderDrops(){
 document.getElementById("lastDrops").innerHTML=state.lastDrops.length?
 state.lastDrops.map(n=>{const c=findChar(n);return <div>${c.emoji} ${c.name}</div>`}).join(""):"Пока пусто.";
}
function renderAll(){
 document.getElementById("lights").textContent=state.lights;
 document.getElementById("dollars").textContent=money(state.dollars);
 renderInventory();renderProfile();renderShop();renderQuests();renderAchievements();renderDrops();
}

function drawWheel(angle=0){
 const canvas=document.getElementById("roulette");const ctx=canvas.getContext("2d"),cx=220,cy=220,r=185;
 ctx.clearRect(0,0,440,440);
 ctx.save();ctx.translate(cx,cy);ctx.rotate(angle);
 const colors=["#ff5c8d","#bd72ff","#8c6cff","#ffc857","#ff8e52","#4dd8ff","#a6e86b","#ff7b7b","#8997ff","#e6a94d","#667085"];
 let start=-Math.PI/2;
 characters.forEach((c,i)=>{
  const end=start+Math.PI*2*c.chance/100;
  ctx.beginPath();ctx.moveTo(0,0);ctx.arc(0,0,r,start,end);ctx.closePath();
  ctx.fillStyle=colors[i];ctx.fill();ctx.strokeStyle="#111522";ctx.lineWidth=2;ctx.stroke();
  ctx.save();ctx.rotate((start+end)/2);ctx.textAlign="right";ctx.fillStyle="#111";
  ctx.font="bold 12px Arial";ctx.fillText(c.name,r-8,4);ctx.restore();
  start=end;
 });
 ctx.beginPath();ctx.arc(0,0,45,0,Math.PI*2);ctx.fillStyle="#111522";ctx.fill();
 ctx.fillStyle="white";ctx.textAlign="center";ctx.font="bold 16px Arial";ctx.fillText("DROP",0,6);
 ctx.restore();
 ctx.beginPath();ctx.moveTo(220,5);ctx.lineTo(205,32);ctx.lineTo(235,32);ctx.fillStyle="white";ctx.fill();
}
function pickCharacter(){
 let n=Math.random()*100;
 for(const c of characters){n-=c.chance;if(n<0)return c}
 return characters[characters.length-1];
}
function spin(times=1){
 if(spinning)return;
 if(state.lights<times){toast("Не хватает молний!");go("shop");return}
 state.lights-=times;
 state.spins+=times;
 progressQuest("spins",times);
 const results=[];
 for(let i=0;i<times;i++){
  const c=pickCharacter();
  const mult=Date.now()<state.multiplierUntil?state.multiplier:1;
  state.inventory.push({name:c.name,price:+(c.price*mult).toFixed(2)});
  state.found[c.name]=(state.found[c.name]||0)+1;
  results.push(c);
  if(c.rarity==="Ультра"){
   state.ultraStreak++;
   if(state.ultraStreak>=2)state.achievementDone.ultra2=true;
  }else state.ultraStreak=0;
  if(c.name==="Mellstroy")state.achievementDone.mell=true;
 }
 state.lastDrops=results.map(c=>c.name).concat(state.lastDrops).slice(0,6);
 spinning=true;
 document.getElementById("spin").disabled=true;
 document.getElementById("spin10").disabled=true;
 let start=performance.now();
 function frame(now){
  let p=Math.min(1,(now-start)/1300);
  drawWheel(p*Math.PI*10);
  if(p<1)requestAnimationFrame(frame);
  else{
   spinning=false;
   document.getElementById("spin").disabled=false;
   document.getElementById("spin10").disabled=false;
   drawWheel();
   document.getElementById("result").textContent="Выпало: "+results.map(c=>c.name).join(", ");
   if(times===1){
    const c=results[0];
    showModal(c.emoji+" "+c.name,Редкость: ${c.rarity}<br>Шанс: ${c.chance}%<br>Цена: ${money(c.price)});
   }else toast("Выпало "+times+" персонажей!");
   save();
  }
 }
 requestAnimationFrame(frame);
 localStorage.setItem(SAVE_KEY,JSON.stringify(state));
}
function sellOne(name){
 const i=state.inventory.findIndex(x=>x.name===name);
 if(i<0)return;
 const [it]=state.inventory.splice(i,1);
 state.dollars=+(state.dollars+it.price).toFixed(2);
 state.soldTotal=+(state.soldTotal+it.price).toFixed(2);
 progressQuest("earned",it.price);save();
 toast("Продано за "+money(it.price));
}
function sellNamed(name){
 const items=state.inventory.filter(x=>x.name===name);
 const total=items.reduce((s,x)=>s+x.price,0);
 state.inventory=state.inventory.filter(x=>x.name!==name);
 state.dollars=+(state.dollars+total).toFixed(2);
 state.soldTotal=+(state.soldTotal+total).toFixed(2);
 progressQuest("earned",total);save();toast("Продано за "+money(total));
}
function sellEverything(){
 if(!state.inventory.length){toast("Инвентарь пуст.");return}
 const total=state.inventory.reduce((s,x)=>s+x.price,0);
 state.inventory=[];
 state.dollars=+(state.dollars+total).toFixed(2);
 state.soldTotal=+(state.soldTotal+total).toFixed(2);
 progressQuest("earned",total);save();toast("Продано всё за "+money(total));
}
function showModal(title,body){document.getElementById("modalRoot").innerHTML=<div class="modal-bg"><div class="panel modal"><h2>${title}</h2><p>${body}</p><button id="modalOK">Круто!</button></div></div>;
 document.getElementById("modalOK").onclick=()=>document.getElementById("modalRoot").innerHTML="";
}
function claimQuest(id){
 const q=state.quests.find(q=>q.id===id);
 if(!q||q.claimed)return;
 const t=questTemplates.find(t=>t.id===id);
 let progress=q.progress;
 if(id==="bonuses")progress=Number(state.bonuses.telegram)+Number(state.bonuses.tiktok)+Number(state.dailyDate===today());
 if(id==="secret"&&progress<1){toast("Введи секретный промокод во время квеста.");return}
 if(progress<q.goal){toast("Квест ещё не выполнен.");return}
 q.claimed=true;
 switch(id){
  case"spinsRandom":addDollars(40);break;
  case"lightsRandom":addLights(20);addDollars(5);break;
  case"earn100":addLights(10);break;
  case"daily":addDollars(3);addLights(5);break;
  case"spin200":state.multiplier=2;state.multiplierUntil=Date.now()+60000;break;
  case"spin80":state.multiplier=3;state.multiplierUntil=Date.now()+30000;break;
  case"bonuses":addDollars(5);break;
  case"secret":addDollars(30);break;
 }
 save();toast("Награда получена!");
}
function claimAchievement(id){
 if(!state.achievementDone[id]||state.achievementClaimed[id])return;
 state.achievementClaimed[id]=true;
 if(id==="finger")addDollars(5);
 if(id==="legend")addLights(20);
 if(id==="ultra2")addDollars(50);
 if(id==="mell")addDollars(20);
 save();toast("Награда за достижение получена!");
}
function usePromo(code){
 code=code.trim();
 if(code==="#stsProm8t"){
  const q=state.quests.find(q=>q.id==="secret");
  if(!q||q.claimed){toast("Код работает только во время секретного квеста.");return}
  q.progress=1;save();toast("Квест выполнен! Забери награду.");return;
 }
 const p=promoCodes[code];
 if(!p){toast("Промокод не найден.");return}
 const used=state.promoUses[code]||0;
 if(used>=p.limit){toast("Лимит использований исчерпан.");return}
 state.promoUses[code]=used+1;
 if(p.type==="lights")addLights(p.amount);else addDollars(p.amount);
 save();toast("Промокод активирован!");
}
function dailyReward(){
 if(state.dailyDate===today())return;
 state.dailyDate=today();addDollars(3);addLights(5);progressQuest("daily",1);save();
 toast("Получено $3 и 5 молний!");
}
function bonus(which){
 if(state.bonuses[which]){toast("Бонус уже получен.");return}
 state.bonuses[which]=true;addLights(3);save();toast("+3 ⚡");
}
document.getElementById("spin").onclick=()=>spin(1);
document.getElementById("spin10").onclick=()=>spin(10);
document.getElementById("sellAll").onclick=sellEverything;
document.getElementById("telegram").onclick=()=>bonus("telegram");
document.getElementById("tiktok").onclick=()=>bonus("tiktok");
document.getElementById("daily").onclick=dailyReward;
document.getElementById("promoButton").onclick=()=>usePromo(document.getElementById("promoInput").value);
document.getElementById("saveNick").onclick=()=>{
 state.nickname=document.getElementById("nickname").value.trim().slice(0,24)||"Игрок";
 save();toast("Никнейм сохранён!");
};
document.getElementById("shopAmount").oninput=renderShop;
document.getElementById("buy").onclick=()=>{
 const n=Math.max(1,Math.min(10000,Math.floor(+document.getElementById("shopAmount").value||1)));
 const cost=shopPrice(n);
 if(cost>1000){toast("Лимит покупки — $1000.");return}
 if(cost>state.dollars){toast("Недостаточно долларов.");return}
 state.dollars=+(state.dollars-cost).toFixed(2);addLights(n);save();toast("Куплено "+n+" молний!");
};
document.getElementById("reset").onclick=()=>{
 showModal("Сбросить прогресс?","Закрой окно и подтверди сброс отдельной кнопкой в профиле.");
 const modal=document.querySelector(".modal");
 const b=document.createElement("button");b.textContent="Сбросить";b.style.marginLeft="8px";
 b.onclick=()=>{localStorage.removeItem(SAVE_KEY);state=defaultState();location.reload()};
 modal.appendChild(b);
};renderChances();
ensureQuests();
renderAll();
drawWheel();
if(!state.welcome){
 state.welcome=true;state.lights+=1;save();
 showModal("Добро пожаловать в NekitDrop! ⚡","Тебе начислена дополнительная молния за первый вход.");
}
</script>
</body>
</html>
