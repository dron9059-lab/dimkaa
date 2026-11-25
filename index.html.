<!DOCTYPE html>
<html lang="kk">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#1f5fbf">
<link rel="manifest" href="./manifest.webmanifest">
<link rel="icon" href="./icon-192.png">
<title>IT-өтінімдер</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:system-ui,Arial,sans-serif;background:#f1f4f9;color:#1b2430}
header{background:#1f5fbf;color:#fff;padding:14px 16px;display:flex;justify-content:space-between;align-items:center}
h1{font-size:1.2rem;margin:0}
main{max-width:640px;margin:0 auto;padding:16px}
.card{background:#fff;border-radius:10px;padding:14px;margin-bottom:14px;box-shadow:0 1px 3px rgba(0,0,0,.12)}
label{display:block;font-size:.85rem;margin:8px 0 4px}
input,select,button{font:inherit;padding:9px;border-radius:8px;border:1px solid #b8c2d1;width:100%}
button{cursor:pointer;background:#1f5fbf;color:#fff;border:0;margin-top:10px}
#lang{width:auto;background:#fff;color:#1f5fbf;margin:0;padding:6px 12px;font-weight:700}
.err{color:#b3261e;font-size:.85rem;min-height:1.2em;margin-top:6px}
ul{list-style:none;margin:0;padding:0}
li{display:flex;gap:10px;align-items:center;padding:10px 0;border-bottom:1px solid #e3e8ef}
li:last-child{border:0}
li input{width:auto}
.t{flex:1}
.done .t strong{text-decoration:line-through;color:#6b7686}
.meta{font-size:.78rem;color:#6b7686}
.stat{font-size:.9rem;margin-bottom:8px}
.empty{color:#6b7686;text-align:center;padding:12px}
</style>
</head>
<body>
<header><h1 id="title"></h1><button id="lang" type="button">RU</button></header>
<main>
  <section class="card">
    <label for="name" id="lName"></label>
    <input id="name" maxlength="80" autocomplete="off">
    <label for="prio" id="lPrio"></label>
    <select id="prio"><option value="low"></option><option value="mid"></option><option value="high"></option></select>
    <button id="add" type="button"></button>
    <div class="err" id="err" role="alert"></div>
  </section>
  <section class="card">
    <div class="stat" id="stat"></div>
    <ul id="list"></ul>
  </section>
</main>
<script>
const T={
 kk:{title:"IT-өтінімдер тізімі",name:"Өтінім тақырыбы",prio:"Басымдығы",low:"Төмен",mid:"Орташа",high:"Жоғары",add:"Өтінімді тіркеу",
  empty:"Әзірге өтінім жоқ",stat:(a,d)=>`Ашық: ${a} · Орындалған: ${d}`,errEmpty:"Тақырыпты енгізіңіз",errLen:"Тақырып кемінде 3 таңба болуы керек",
  done:"Орындалды",open:"Ашық",ph:"Мысалы: Принтер жұмыс істемейді",locale:"kk-KZ"},
 ru:{title:"Список IT-заявок",name:"Тема заявки",prio:"Приоритет",low:"Низкий",mid:"Средний",high:"Высокий",add:"Зарегистрировать заявку",
  empty:"Заявок пока нет",stat:(a,d)=>`Открыто: ${a} · Выполнено: ${d}`,errEmpty:"Введите тему",errLen:"Тема должна быть не короче 3 символов",
  done:"Выполнено",open:"Открыта",ph:"Например: не работает принтер",locale:"ru-RU"}
};
const KEY_ITEMS="pwa13_items", KEY_LANG="pwa13_lang";
const $=id=>document.getElementById(id);
function load(k,def){try{const v=JSON.parse(localStorage.getItem(k));return v??def}catch(e){return def}}
function save(k,v){try{localStorage.setItem(k,JSON.stringify(v))}catch(e){}}
let items=load(KEY_ITEMS,[]); if(!Array.isArray(items))items=[];
let lang=load(KEY_LANG,"kk"); if(!T[lang])lang="kk";

function render(){
  const t=T[lang];
  document.documentElement.lang=lang;
  $("title").textContent=t.title; $("lName").textContent=t.name; $("lPrio").textContent=t.prio;
  $("add").textContent=t.add; $("name").placeholder=t.ph; $("lang").textContent=lang==="kk"?"RU":"KZ";
  const opts=$("prio").options; opts[0].textContent=t.low; opts[1].textContent=t.mid; opts[2].textContent=t.high;
  const d=items.filter(i=>i.done).length;
  $("stat").textContent=t.stat(items.length-d,d);
  const ul=$("list"); ul.textContent="";
  if(!items.length){const li=document.createElement("li");li.className="empty";li.textContent=t.empty;ul.appendChild(li);return}
  items.slice().reverse().forEach(i=>{
    const li=document.createElement("li"); if(i.done)li.className="done";
    const cb=document.createElement("input"); cb.type="checkbox"; cb.checked=i.done;
    cb.setAttribute("aria-label",t.done);
    cb.addEventListener("change",()=>{i.done=cb.checked;save(KEY_ITEMS,items);render()});
    const box=document.createElement("div"); box.className="t";
    const s=document.createElement("strong"); s.textContent=i.title;
    const m=document.createElement("div"); m.className="meta";
    m.textContent=`${t[i.prio]} · ${new Date(i.date).toLocaleDateString(t.locale)} · ${i.done?t.done:t.open}`;
    box.append(s,m); li.append(cb,box); ul.appendChild(li);
  });
}
$("add").addEventListener("click",()=>{
  const t=T[lang], v=$("name").value.trim();
  if(!v){$("err").textContent=t.errEmpty;return}
