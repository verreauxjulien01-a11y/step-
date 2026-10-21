<!DOCTYPE html>
<html lang="fr"><head><meta charset="utf-8"><title>Carnet d’entraînement – Step</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
:root{--y:#fff36b;--b:#4a9bff;--g:#d9d9d9}
*{box-sizing:border-box}body{margin:0;font:15px system-ui,Segoe UI,Arial,sans-serif;background:#f2f2f2;color:#111}
#top{background:var(--b);padding:8px 12px;display:flex;flex-wrap:wrap;gap:6px;align-items:center;position:sticky;top:0;z-index:5}
#top b{margin-right:auto}
button,select{font:inherit;padding:5px 10px;border:1px solid #333;border-radius:6px;background:#fff;cursor:pointer}
button.on,button.pri{background:#111;color:#fff}button:disabled{opacity:.6;cursor:default}
#page{max-width:920px;margin:12px auto;padding:0 10px 50px}
h1{text-align:center;margin:8px 0}h2{margin:14px 0 6px}
.box{background:#fff;border:2px solid #000;padding:10px;margin:8px 0}
.id{background:var(--y);border:2px solid #000;padding:8px;display:flex;flex-wrap:wrap;gap:8px 14px;font-weight:700}
.bar{background:#000;color:#fff;text-align:center;font-weight:700;padding:5px;margin-top:12px;font-size:13px}
.row{display:flex;flex-wrap:wrap;gap:8px;align-items:center}.l{font-weight:700;font-size:13px}
input,textarea{font:inherit;padding:5px;border:1px solid #777;border-radius:4px;background:#fff}
input[type=number]{width:64px;background:var(--g);text-align:center}textarea{width:100%;min-height:56px}
.sc{display:inline-flex;gap:3px}.sc button{width:30px;height:30px;padding:0;font-weight:700;color:#111;opacity:.45}
.sc button.on{opacity:1;outline:3px solid #111}
.seq{display:grid;grid-template-columns:repeat(8,1fr);gap:4px;margin-top:6px}
.cell{width:100%;padding:6px 2px;text-align:center;background:var(--g);font-weight:700;font-size:13px}
.blocs{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:10px}
.bloc{border:2px solid #000;background:#fff}.bloc h4{margin:0;padding:4px;background:var(--y);border-bottom:2px solid #000}
.bloc h4 input{width:100%;border:0;background:transparent;font-weight:700;text-align:center}
.bloc .g{display:grid;grid-template-columns:1fr 1fr;gap:4px;padding:6px}.mir{border-style:dashed;opacity:.85}
.pal{margin-top:10px;padding:8px;background:#fff8c9;border:2px dashed #000;display:flex;flex-wrap:wrap;gap:5px}
.pal button{border-radius:14px;padding:2px 9px;font-size:13px}
.ser{display:grid;grid-template-columns:repeat(auto-fit,minmax(290px,1fr));gap:8px}
.ser>div{background:#fff;border:2px solid #000;padding:6px}.ser h4{margin:0 0 6px;background:#000;color:#fff;padding:4px}
.th{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:8px}
.th>div{border:2px solid #000;padding:8px;font-size:13px}.th h4{margin:0 0 6px;padding:5px;text-align:center}
table{border-collapse:collapse;width:100%;background:#fff}td,th{border:1px solid #888;padding:6px;text-align:left;vertical-align:top;font-size:14px}
th{background:#eee}.kv td{text-align:center}.hide{display:none}.bar input{width:62%;max-width:520px;margin-left:6px;padding:3px 6px;font-weight:400}.ms summary{border:1px solid #777;border-radius:4px;padding:5px;cursor:pointer;background:#fff}.ms div{border:1px solid #777;border-top:0;padding:6px;display:grid;gap:4px;background:#fff}small{color:#555}
#msg{position:fixed;bottom:14px;left:50%;transform:translateX(-50%);background:#111;color:#fff;padding:8px 14px;border-radius:8px;opacity:0;transition:.3s;pointer-events:none}#msg.on{opacity:1}
@media print{#top,.pal,.np{display:none!important}body{background:#fff}*{-webkit-print-color-adjust:exact;print-color-adjust:exact}}
</style></head><body>
<div id="top"></div><div id="page"></div><div id="msg"></div><datalist id="pasl"></datalist>
<script>
/* ===== PARAMÈTRES MODIFIABLES PAR LE PROFESSEUR ===== */
const CFG={
 annee:"2026-2027",
 simples:["Basic","Run","V-Step","Mambo","Traverse","Jazz Square","T-Step","Cheval","Lunge"],
 alternes:["Talons-fesses","Genou","Kick","Side","Élévation","Tap up","Tap down","Chassé","L-Step","Ciseaux","Twist","Charleston","3 genoux","3 talons"],
 themes:[
  {t:"Prolongés et modérés",c:"#8fd0ff",q:"J’ai besoin de faire du sport, pour me vider la tête",p:["BPM musique : 128-130","FCE : 50-65 %","Intensité : modérée","Durée de l’effort : 25-30 min","Séries : 2 à 4","Récupération : 5 min max"]},
  {t:"Long et soutenu",c:"#ffe27a",q:"Je me sens souvent fatigué, pas assez actif",p:["BPM musique : 135","FCE : 70-85 %","Intensité : forte","Durée de l’effort : 20-26 min","Séries : au moins 3","Récupération : 4 min max"]},
  {t:"Bref et intense",c:"#ff9f8f",q:"J’aime me dépasser, repousser mes limites",p:["BPM musique : 135-145","FCE : > 85 %","Intensité : très forte","Durée de l’effort : 14-22 min","Séries : 4 min max par série","Récupération : 4 min max"]}],
 lex:[["FC","Nombre de pulsations cardiaques par minute (unité : BPM). FC max : maximum de pulsations en 1 min. FC repos : pulsations en relaxation complète (ex : au réveil). FC d’entraînement : pulsations visées selon mon projet."],
  ["Phrase","Enchaînement de 8 temps consécutifs."],["Bloc","Enchaînement de 4 phrases consécutives, soit 32 temps."],
  ["BPM","Battements par minute : tempo de la musique et unité de mesure de la fréquence cardiaque."],
  ["Thème d’entraînement","Volonté de produire un effet sur son corps à plus ou moins long terme (masse musculaire, explosivité, endurance…)."],
  ["Pas simple","Pas SANS changement de pied directeur."],["Pas alterné","Pas AVEC changement de pied directeur."],["Pied directeur","Premier pied posé sur le step."]],
 mesures:["Respiration","Musculaire","Mémorisation","Transpiration"],
 params:["Tempo","Hauteur","Saut","Bras","Amplitude","Lest"],
 d:{bpmEch:128,dEch:5,fEch:50,bpm:135,series:4,dSerie:7,recup:5,fce:60,blocs:2,txtRecup:"Stretching sur le Step"}
};
/* ==================================================== */
const $=s=>document.querySelector(s),E=s=>String(s??"").replace(/[&<>"]/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c]));
const LS="stepCarnets_v2",PAS=[...CFG.simples,...CFG.alternes];
let DB={};try{DB=JSON.parse(localStorage.getItem(LS)||"{}")}catch(e){}
let D=null,ID=null,MODE="home",tab="seance",PV=null,PC="",CC=null;
const te=new TextEncoder(),b64=u=>btoa(Array.from(u,c=>String.fromCharCode(c)).join("")),ub=s=>Uint8Array.from(atob(s),c=>c.charCodeAt(0));
const sha=async s=>b64(new Uint8Array(await crypto.subtle.digest("SHA-256",te.encode(s))));
async function kdf(p,s){const m=await crypto.subtle.importKey("raw",te.encode(p),"PBKDF2",false,["deriveKey"]);return crypto.subtle.deriveKey({name:"PBKDF2",salt:s,iterations:150000,hash:"SHA-256"},m,{name:"AES-GCM",length:256},false,["encrypt","decrypt"])}
async function seal(o,p){const s=crypto.getRandomValues(new Uint8Array(16)),i=crypto.getRandomValues(new Uint8Array(12)),c=await crypto.subtle.encrypt({name:"AES-GCM",iv:i},await kdf(p,s),te.encode(JSON.stringify(o)));return JSON.stringify({v:1,s:b64(s),i:b64(i),d:b64(new Uint8Array(c))})}
async function unseal(t,p){const o=JSON.parse(t),c=await crypto.subtle.decrypt({name:"AES-GCM",iv:ub(o.i)},await kdf(p,ub(o.s)),ub(o.d));return JSON.parse(new TextDecoder().decode(c))}
const toast=t=>{const m=$("#msg");m.textContent=t;m.classList.add("on");setTimeout(()=>m.classList.remove("on"),2500)};
const persist=()=>{if(ID&&MODE==="stu"){DB[ID].data=D;try{localStorage.setItem(LS,JSON.stringify(DB))}catch(e){}}};
const getP=(o,p)=>p.split(".").reduce((a,k)=>a?.[k],o),setP=(o,p,v)=>{const k=p.split("."),l=k.pop();k.reduce((a,x)=>a[x],o)[l]=v};
const save=async(name,txt)=>{const dl=await claude.use("downloads");if(!dl)return toast("Téléchargement indisponible dans cette vue");try{await dl.save({filename:name,data:txt})}catch(e){if(e.code!=="declined")toast("Enregistrement impossible : "+(e.message||e.code))}};
const ask=(msg,inp)=>new Promise(r=>{const o=document.createElement("div");o.style.cssText="position:fixed;inset:0;background:#0008;display:flex;align-items:center;justify-content:center;z-index:99";o.innerHTML=`<div class="box" style="max-width:380px"><p>${E(msg)}</p>${inp?'<input type="password" style="width:100%">':""}<div class="row" style="margin-top:8px"><button class="pri" data-ok>OK</button><button data-no>Annuler</button></div></div>`;document.body.append(o);const i=o.querySelector("input");i&&i.focus();o.onclick=e=>{const b=e.target.closest("button");if(!b)return;o.remove();r(b.dataset.ok!==undefined?(inp?i.value:true):(inp?null:false))}});
const clean=s=>(s||"X").trim().replace(/[^A-Za-z0-9]+/g,"_");
const num=v=>v===""||v==null||isNaN(+v)?null:+v;
const avg=a=>{a=a.map(num).filter(x=>x!=null);return a.length?Math.round(a.reduce((x,y)=>x+y,0)/a.length*10)/10:""};
const kv=p=>{const r=num(D.perso.fcRepos),m=num(D.perso.fcMax),x=num(p);return r!=null&&m!=null&&x!=null?Math.round(r+x/100*(m-r)):"–"};
const bind=(p,v)=>`data-k="${p}" value="${E(v)}"`,ta=(p,ph)=>`<textarea data-k="${p}" placeholder="${ph||""}"></textarea>`;
const COL=["#e53935","#fb8c00","#fdd835","#9ccc3c","#2e9e44"];
const sc=(p,v,inv)=>`<span class="sc">`+[1,2,3,4,5].map(n=>`<button type="button" class="${v==n?"on":""}" style="background:${inv?COL[5-n]:COL[n-1]};color:#111" data-sc="${p}" data-v="${n}">${n}</button>`).join("")+`</span>`;
const newBloc=i=>({nom:"Bloc "+i,cells:Array(8).fill("")});
const newSeance=n=>({n,date:"",avant:{sc:"",txt:""},ech:{bpm:CFG.d.bpmEch,duree:CFG.d.dEch,seq:Array(8).fill(""),fce:CFG.d.fEch,reel:""},spec:"",
 corps:{bpm:CFG.d.bpm,series:CFG.d.series,duree:CFG.d.dSerie,recup:CFG.d.recup,fce:CFG.d.fce,miroir:true,blocs:Array.from({length:CFG.d.blocs},(_,i)=>newBloc(i+1)),
 ser:Array.from({length:CFG.d.series},()=>({fce:"",m:["","","",""],rem:"",par:""}))},recup:CFG.d.txtRecup,apres:{sc:"",txt:""}});
const newData=(e)=>({eleve:e,perso:{theme:"",just:"",age:"",fcRepos:"",fcMax:""},cur:0,seances:[newSeance(1)]});
const S=()=>D.seances[D.cur];
const PL=()=>[...PAS,...((D&&D.mesPas)||[])];
const TIM={i:-1,D:null,cur:-1,k:"s",left:0,end:0,run:false,iv:null},fmt=s=>String(Math.floor(s/60)).padStart(2,"0")+":"+String(s%60).padStart(2,"0");
const tmStop=()=>{clearInterval(TIM.iv);TIM.run=false;TIM.i=-1};
function beep(){try{const a=new(window.AudioContext||window.webkitAudioContext)(),o=a.createOscillator();o.connect(a.destination);o.frequency.value=880;o.start();setTimeout(()=>{o.stop();a.close()},800)}catch(e){}}
function tmShow(){if(!D)return;document.querySelectorAll("[data-tm]").forEach(e=>{const i=+e.dataset.tm,on=TIM.i===i&&TIM.D===D&&TIM.cur===D.cur;e.textContent=on?fmt(TIM.left)+(TIM.k==="r"?" récup.":""):fmt((+S().corps.duree||0)*60);e.style.color=on&&TIM.k==="r"?"#1565c0":"#111"})}
function tick(){TIM.left=Math.max(0,Math.ceil((TIM.end-Date.now())/1000));if(!TIM.left){TIM.run=false;clearInterval(TIM.iv);beep();toast("Temps écoulé")}tmShow()}

function bar(){
 const t=(k,l)=>`<button data-tab="${k}" class="${tab===k?"on":""}">${l}</button>`;
 if(MODE==="home")return`<b>Carnet d’entraînement – Step</b>`;
 if(MODE==="prof")return`<b>Espace professeur</b><button data-act="quit">Quitter</button>`;
 const who=`${E(D.eleve.prenom)} ${E(D.eleve.nom)}`;
 return`<b>${who}</b>${t("perso","Ma fiche")}${t("lex","Lexique")}${t("pas","Pas")}${t("seance","Séances")}`+
  (MODE==="view"?`<button data-act="back">Retour à la liste</button>`:`<button data-act="export" class="pri">Exporter pour le prof</button>`)+
  `<button data-act="print">Imprimer</button>`+(MODE==="stu"?`<button data-act="quit">Fermer</button>`:"");
}
function home(){
 const ids=Object.keys(DB);
 return`<h1>Carnet d’entraînement STEP</h1><p style="text-align:center">Année scolaire ${CFG.annee}</p>
 <div class="box"><h2>Ouvrir mon carnet</h2>${ids.length?ids.map(i=>{const e=DB[i].data.eleve;return`<div class="row" style="margin:6px 0"><span class="l" style="min-width:200px">${E(e.prenom)} ${E(e.nom)} (${E(e.classe)})</span><input type="password" id="p_${i}" placeholder="Mon code"><button data-act="open" data-id="${i}">Ouvrir</button></div>`}).join(""):"<small>Aucun carnet sur cet ordinateur.</small>"}</div>
 <div class="box"><h2>Créer mon carnet</h2><div class="row"><input id="nP" placeholder="Prénom"><input id="nN" placeholder="Nom"><input id="nC" placeholder="Classe" style="width:90px"><input id="nX" placeholder="Code de la classe (donné par le prof)" style="width:250px"><input id="nK" type="password" placeholder="Mon code personnel (4+ car.)"><button class="pri" data-act="new">Créer</button></div>
 <small>Retiens bien ton code : il ne peut pas être récupéré.</small></div>
 <div class="box"><button data-act="prof">Espace professeur</button></div>
 <div class="box"><small><b>Données personnelles (RGPD).</b> Rien n’est envoyé sur Internet : ton carnet reste sur cet ordinateur, protégé par ton code. Il contient ton identité, ta classe, ta fréquence cardiaque et ton ressenti à l’effort, utilisés uniquement pour le suivi en EPS. Quand tu exportes, le fichier est chiffré avec le code de ta classe : seul ton professeur peut le lire. Tu peux supprimer ton carnet à tout moment (onglet « Ma fiche »). Sur un ordinateur partagé, pense à fermer ton carnet.</small></div>`;
}
const PK="stepProf_v2";
const vsave=async()=>{try{localStorage.setItem(PK,await seal(PV,PC))}catch(e){toast("Sauvegarde impossible")}};
const cleanCode=s=>String(s||"").trim().toUpperCase();
function prof(){
 if(!PV){const has=!!localStorage.getItem(PK);return`<h1>Espace professeur</h1><div class="box"><h2>${has?"Entrer dans mon espace":"Créer mon espace professeur"}</h2><div class="row"><input type="password" id="pc" placeholder="${has?"Mon code professeur":"Choisis un code professeur (6 caractères min.)"}"><button class="pri" data-act="${has?"unlock":"mkvault"}">${has?"Entrer":"Créer"}</button></div>${has?`<div style="margin-top:10px"><button data-act="resetV">Code oublié : tout effacer</button></div>`:`<small>Ce code protège les données de toutes tes classes sur cet ordinateur. Il ne peut pas être récupéré : note-le.</small>`}</div>`}
 const cl=PV.classes,cur=cl.find(c=>c.id===CC),rows=PV.eleves.map((x,i)=>({d:x.data,c:x.cid,i})).filter(x=>x.c===CC).sort((x,y)=>(x.d.eleve.nom+x.d.eleve.prenom).localeCompare(y.d.eleve.nom+y.d.eleve.prenom));
 return`<h1>Espace professeur</h1>
 <div class="box"><b>Comment ça marche</b><ol style="margin:6px 0"><li>Crée une classe avec un code (ex : 3A-STEP).</li><li>Donne ce code aux élèves : ils le saisissent en créant leur carnet.</li><li>Ils cliquent sur « Exporter pour le prof » et t’envoient le fichier obtenu (Carnet_NOM_Prénom.json).</li><li>Choisis la classe, puis « Ajouter les carnets reçus » et sélectionne tous les fichiers de cette classe d’un coup.</li></ol></div>
 <div class="box"><h2>Mes classes</h2><div class="row">${cl.map(c=>`<button data-act="selC" data-id="${c.id}" class="${c.id===CC?"on":""}">${E(c.nom)} (${PV.eleves.filter(x=>x.cid===c.id).length})</button>`).join("")||"<small>Aucune classe pour l’instant.</small>"}</div>
 <div class="row" style="margin-top:10px"><input id="cn" placeholder="Nom de la classe (ex : 3A)" style="width:190px"><input id="ck" placeholder="Code de la classe (ex : 3A-STEP)"><button class="pri" data-act="addC">Créer la classe</button></div></div>
 ${cur?`<div class="box"><h2>Classe ${E(cur.nom)}</h2><div class="row"><span>Code à donner aux élèves : <b>${E(cur.code)}</b></span></div>
 <div class="row" style="margin-top:8px"><input type="file" id="fs" multiple accept=".json,.step"><button class="pri" data-act="addF">Ajouter les carnets reçus</button><button data-act="csv">Exporter la synthèse (CSV)</button><button data-act="delC">Supprimer la classe</button></div></div>
 ${rows.length?`<table><tr><th>Nom</th><th>Prénom</th><th>Thème</th><th>Séances</th><th>Dernière</th><th>Forme avant (moy.)</th><th>FC repos / max</th><th></th></tr>`+rows.map(r=>{const d=r.d,th=CFG.themes[d.perso.theme],ss=d.seances;return`<tr><td>${E(d.eleve.nom)}</td><td>${E(d.eleve.prenom)}</td><td>${th?E(th.t):"–"}</td><td>${ss.filter(s=>s.date).length}/${ss.length}</td><td>${E(ss.map(s=>s.date).filter(Boolean).sort().pop()||"–")}</td><td>${avg(ss.map(s=>s.avant.sc))||"–"}</td><td>${E(d.perso.fcRepos||"–")} / ${E(d.perso.fcMax||"–")}</td><td><button data-act="see" data-i="${r.i}">Voir</button> <button data-act="rmE" data-i="${r.i}">Retirer</button></td></tr>`}).join("")+`</table>`:`<small>Aucun carnet dans cette classe.</small>`}`:""}`;
}
function fiche(){
 const p=D.perso,h=[50,60,70,80,85,90,95,100],est=num(p.age)?220-p.age:"";
 return`<h1>Fiche de renseignements</h1><div class="id"><span>NOM : ${E(D.eleve.nom)}</span><span>Prénom : ${E(D.eleve.prenom)}</span><span>Classe : ${E(D.eleve.classe)}</span></div>
 <h2>Mon thème d’entraînement</h2><div class="th">${CFG.themes.map((t,i)=>`<div><h4 style="background:${t.c}">${E(t.t)}</h4><i>« ${E(t.q)} »</i><br><br>${t.p.map(E).join("<br>")}<br><br><button data-sc="perso.theme" data-v="${i}" class="${p.theme==i&&p.theme!==""?"on":""}">Je choisis ce thème</button></div>`).join("")}</div>
 <h2>Justification</h2>${ta("perso.just","Pourquoi ce thème et pas un autre ?")}
 <h2>Ma fréquence cardiaque (méthode de Karvonen)</h2><div class="box"><div class="row"><span class="l">Âge</span><input type="number" ${bind("perso.age",p.age)} data-r="1"><span class="l">FC repos</span><input type="number" ${bind("perso.fcRepos",p.fcRepos)} data-r="1"><span class="l">FC max</span><input type="number" ${bind("perso.fcMax",p.fcMax)} data-r="1"><small>${est?`Estimation : 220 – âge = ${est} bpm`:""}</small></div>
 <small>FC cible = FC repos + % × (FC max – FC repos). Mesure ta FC de repos au réveil, au calme.</small>
 <table class="kv" style="margin-top:8px"><tr><th>% effort</th>${h.map(x=>`<th>${x}%</th>`).join("")}</tr><tr><td>Karvonen (bpm)</td>${h.map(x=>`<td><b>${kv(x)}</b></td>`).join("")}</tr><tr><td>Classique (% FC max)</td>${h.map(x=>`<td>${num(p.fcMax)!=null?Math.round(x/100*p.fcMax):"–"}</td>`).join("")}</tr></table></div>
 <div class="box"><small>Pour faire varier la FC : ajout de bras, hauteur du step, tempo, sauts, lests…</small></div>
 ${MODE==="stu"?`<div class="np"><button data-act="chgCC">Changer le code de ma classe</button> <button data-act="del">Supprimer mon carnet de cet ordinateur</button></div>`:""}`;
}
const lex=()=>`<h1>Lexique</h1><table>${CFG.lex.map(r=>`<tr><th style="width:28%">${E(r[0])}</th><td>${E(r[1])}</td></tr>`).join("")}</table>`;
const pasv=()=>`<h1>Référentiel de pas</h1><div class="th"><div><h4 style="background:#ddd">Pas simples</h4><small>sans changement de pied directeur</small><br><br>${CFG.simples.map(E).join("<br>")}</div><div><h4 style="background:#ddd">Pas alternés</h4><small>avec changement de pied directeur</small><br><br>${CFG.alternes.map(E).join("<br>")}</div></div><div class="box"><small>Pour complexifier : rotation, déplacement hors du step, répétitions. Équilibre la charge entre côté droit et gauche en alternant le pied directeur.</small></div>`;
const mir=c=>c.replace(/\bD\b/g,"§").replace(/\bG\b/g,"D").replace(/§/g,"G");
function blocH(b,i,m){const P=`seances.${D.cur}.corps.blocs.${i}`;return`<div class="bloc ${m?"mir":""}"><h4>${m?E(b.nom)+" (à gauche)":`<input ${bind(P+".nom",b.nom)}>`}</h4><div class="g">${b.cells.map((c,j)=>m?`<input class="cell" readonly tabindex="-1" value="${E(mir(c))}">`:`<input class="cell" list="pasl" ${bind(P+".cells."+j,c)}>`).join("")}</div></div>`}
function seance(){
 const s=S(),P=`seances.${D.cur}`,c=s.corps,th=c.theme===""||c.theme==null?-1:+c.theme;
 return`<div class="row np"><select data-act="sel">${D.seances.map((x,i)=>`<option value="${i}" ${i===D.cur?"selected":""}>Leçon n°${E(x.n)}${x.date?" – "+E(x.date):""}</option>`).join("")}</select>${MODE==="stu"?`<button data-act="addS">+ Nouvelle séance</button><button data-act="dupS">Dupliquer</button><button data-act="delS">Supprimer</button>`:""}</div>
 <h1>FICHE DE SÉANCE</h1><div style="text-align:center;font-weight:700">Leçon n° <input type="number" ${bind(P+".n",s.n)}></div>
 <div class="id" style="margin-top:8px"><span>NOM : ${E(D.eleve.nom)}</span><span>Prénom : ${E(D.eleve.prenom)}</span><span>Date : <input type="date" ${bind(P+".date",s.date)} data-r="1"></span></div>
 <div class="box"><div class="l">État de forme général avant la séance (1 = très fatigué, 5 = en pleine forme)</div>${sc(P+".avant.sc",s.avant.sc)}${ta(P+".avant.txt","Comment je me sens ?")}</div>
 <div class="bar">ÉCHAUFFEMENT GÉNÉRAL</div><div class="box"><div class="row"><span class="l">Échauffement à</span><input type="number" ${bind(P+".ech.bpm",s.ech.bpm)}><span class="l">BPM, en boucle pendant</span><input type="number" ${bind(P+".ech.duree",s.ech.duree)}><span class="l">min</span>
 </div><div class="row" style="margin-top:6px"><span class="l">FCE attendue</span><input type="number" ${bind(P+".ech.fce",s.ech.fce)} data-r="1"><span class="l">% = ${kv(s.ech.fce)} bpm</span><span class="l" style="margin-left:14px">FCE réelle</span><input type="number" ${bind(P+".ech.reel",s.ech.reel)}></div>
 <div class="seq">${s.ech.seq.map((v,i)=>`<input class="cell" list="pasl" ${bind(P+".ech.seq."+i,v)}>`).join("")}</div></div>
 <div class="bar">ÉCHAUFFEMENT SPÉCIFIQUE : <input ${bind(P+".spec",s.spec)}></div><div class="box"><div class="l">Ce que j’attends</div>${ta(P+".specAtt","Ce que je dois réussir, sentir ou retenir à la fin de cet échauffement…")}</div>
 <div class="bar">CORPS DE SÉANCE : <input ${bind(P+".corps.titre",c.titre)}></div>
 <div class="box"><div class="l">Thème d’entraînement de cette séance</div><div class="row" style="margin:6px 0">${CFG.themes.map((t,i)=>`<button type="button" data-sc="${P}.corps.theme" data-v="${i}" style="${th===i?`background:${t.c};border-width:3px;font-weight:700`:""}">${E(t.t)}</button>`).join("")}</div><div class="l">Objectif de ce corps de séance</div><textarea data-k="${P}.corps.obj" placeholder="Ce que je cherche à travailler dans cette séance…"></textarea><div class="l" style="margin-top:8px">Ce que j’attends</div>${ta(P+".corps.att","Critères de réussite : ce que je dois réussir ou ressentir (FC visée, enchaînement sans erreur…)")}</div>
 <div class="box"><div class="row"><span class="l">Tempo</span><input type="number" ${bind(P+".corps.bpm",c.bpm)}><span class="l">BPM · Séries</span><input type="number" min="1" max="10" ${bind(P+".corps.series",c.series)} data-r="1"><span class="l">×</span><input type="number" ${bind(P+".corps.duree",c.duree)} data-r="1"><span class="l">min · Récup.</span><input type="number" ${bind(P+".corps.recup",c.recup)} data-r="1"><span class="l">min · Durée totale : ${(()=>{const n=+c.series||0;return n*(+c.duree||0)+Math.max(0,n-1)*(+c.recup||0)})()} min</span></div>
 <div class="row" style="margin-top:6px"><span class="l">FCE attendue</span><input type="number" ${bind(P+".corps.fce",c.fce)} data-r="1"><span class="l">% = ${kv(c.fce)} bpm</span><label class="np"><input type="checkbox" data-act="mir" ${c.miroir?"checked":""}> blocs miroirs (D ↔ G)</label></div></div>
 <div class="pal np"><span class="l" style="width:100%">Clique dans une case puis sur un pas (ou tape directement) :</span>${PAS.map(x=>`<button type="button" data-pas="${E(x)}">${E(x)}</button>`).join("")}${(D.mesPas||[]).map((x,i)=>`<button type="button" data-pas="${E(x)}" style="border-style:dashed">${E(x)}</button><button type="button" data-act="delPas" data-i="${i}" title="Supprimer ce pas" style="padding:0 6px;border-radius:14px">×</button>`).join("")}<button data-pas=" D"><b>D</b></button><button data-pas=" G"><b>G</b></button><button data-pas="" style="color:#b00">Vider</button><span class="row" style="width:100%;margin-top:4px"><input id="np" placeholder="Créer mon propre pas…" style="width:230px" maxlength="30"><button data-act="addPas">Créer ce pas</button></span></div>
 <div class="blocs" style="margin-top:8px">${c.blocs.map((b,i)=>blocH(b,i,0)).join("")}${c.miroir?c.blocs.map((b,i)=>blocH(b,i,1)).join(""):""}</div>
 <div class="row np" style="margin-top:6px"><button data-act="addB">+ Ajouter un bloc</button><button data-act="delB">– Retirer le dernier</button></div>
 <h2>Analyse de mes séries</h2><div class="row np" style="margin-bottom:8px"><button data-act="addR">+ Ajouter une série</button><button data-act="delR">– Retirer la dernière série</button></div><div class="ser">${c.ser.map((r,i)=>`<div><h4>SÉRIE ${i+1}</h4><div class="row np" style="margin-bottom:6px"><b data-tm="${i}" style="font-size:22px;min-width:92px">00:00</b>${[["start","Départ"],["pause","Pause"],["reset","Zéro"],["rec","Récup."]].map(([m,t])=>`<button type="button" data-act="tm" data-m="${m}" data-i="${i}">${t}</button>`).join("")}</div><div class="l" style="margin-bottom:6px;color:#1565c0">FCE à atteindre : ${c.fce||"–"} % = ${kv(c.fce)} bpm</div><div class="row"><span class="l">FC réelle</span><input type="number" ${bind(`${P}.corps.ser.${i}.fce`,r.fce)}></div>${CFG.mesures.map((m,j)=>`<div class="row" style="margin-top:4px"><span class="l" style="width:100px">${m}</span>${sc(`${P}.corps.ser.${i}.m.${j}`,r.m[j])}</div>`).join("")}<div class="l" style="margin-top:6px">Remarques / conseils</div>${ta(`${P}.corps.ser.${i}.rem`)}<div class="l">Paramètre(s) utilisé(s)</div>${((sel)=>`<details class="ms"><summary>${sel.length?E(sel.join(", ")):"Choisir…"}</summary><div>${CFG.params.map(x=>`<label><input type="checkbox" data-pm="${P}.corps.ser.${i}.pm" value="${x}" ${sel.includes(x)?"checked":""}> ${x}</label>`).join("")}</div></details>`)(r.pm||[])}</div>`).join("")}</div>
 <div class="bar">RÉCUPÉRATION</div><div class="box"><input style="width:100%" ${bind(P+".recup",s.recup)}></div>
 <div class="box" style="margin-top:12px"><div class="l">État de forme + perspectives</div>${sc(P+".apres.sc",s.apres.sc)}${ta(P+".apres.txt","Bilan, points à retravailler…")}</div>`;
}
let last=null;
function go(){
 const y=scrollY;$("#top").innerHTML=bar();$("#pasl").innerHTML=PL().map(x=>`<option value="${E(x)}">`).join("");
 $("#page").innerHTML=MODE==="home"?home():MODE==="prof"?prof():({perso:fiche,lex,pas:pasv,seance})[tab]();
 $("#page").querySelectorAll("textarea[data-k]").forEach(t=>t.value=getP(D,t.dataset.k)??"");
 if(MODE==="view")$("#page").querySelectorAll("input,textarea,[data-sc],.pal button,[data-act]").forEach(x=>x.disabled=true);
 tmShow();scrollTo(0,y);
}
document.addEventListener("input",e=>{const t=e.target;if(!t.dataset.k||!D)return;setP(D,t.dataset.k,t.value);
 if(t.dataset.k.endsWith(".corps.series")){const n=Math.max(1,Math.min(10,+t.value||1)),a=S().corps.ser;while(a.length<n)a.push({fce:"",m:["","","",""],rem:"",par:""});a.length=n}persist()});
document.addEventListener("change",e=>{const t=e.target,a=t.dataset.act;
 if(t.dataset.pm){const k=t.dataset.pm,v=[...(getP(D,k)||[])],i=v.indexOf(t.value);if(t.checked&&i<0)v.push(t.value);if(!t.checked&&i>=0)v.splice(i,1);setP(D,k,CFG.params.filter(x=>v.includes(x)));persist();t.closest("details").querySelector("summary").textContent=getP(D,k).join(", ")||"Choisir…";return}
 if(a==="sel"){D.cur=+t.value;persist();go()}else if(a==="mir"){S().corps.miroir=t.checked;persist();go()}else if(t.dataset.r){persist();go()}});
document.addEventListener("focusin",e=>{if(e.target.classList?.contains("cell")&&!e.target.readOnly)last=e.target});
document.addEventListener("click",async e=>{const t=e.target.closest("button");if(!t)return;const d=t.dataset;
 if(d.tab){tab=d.tab;go()}
 else if(d.sc){setP(D,d.sc,d.v);persist();go()}
 else if(d.pas!==undefined){if(!last||!document.body.contains(last))return toast("Clique d’abord dans une case");const a=d.pas;last.value=a===""?"":a[0]===" "?last.value.replace(/\s+[DG]$/,"")+a:a;last.dispatchEvent(new Event("input",{bubbles:true}));last.focus()}
 else if(d.act)act(d.act,d)});
async function act(a,d){
 try{
 if(a==="new"){const p=$("#nP").value.trim(),n=$("#nN").value.trim(),c=$("#nC").value.trim(),k=$("#nK").value,x=cleanCode($("#nX").value);if(!p||!n||!x||k.length<4)return toast("Prénom, nom, code de la classe et code personnel (4 caractères min.) requis");
  const id="c"+Date.now().toString(36),salt=b64(crypto.getRandomValues(new Uint8Array(12)));DB[id]={salt,cc:x,hash:await sha(salt+k),data:newData({prenom:p,nom:n,classe:c})};ID=id;D=DB[id].data;MODE="stu";tab="perso";persist();go()}
 else if(a==="open"){const r=DB[d.id],k=$("#p_"+d.id).value;if(await sha(r.salt+k)!==r.hash)return toast("Code incorrect");ID=d.id;D=r.data;MODE="stu";tab="seance";go()}
 else if(a==="quit"){tmStop();persist();D=null;ID=null;PV=null;PC="";CC=null;MODE="home";go()}
 else if(a==="back"){MODE="prof";D=null;go()}
 else if(a==="prof"){MODE="prof";go()}
 else if(a==="print")window.print();
 else if(a==="tm"){if(!D)return;const i=+d.i,c=S().corps,run=(s,k)=>{if(!s)return toast("Renseigne d’abord la durée (séries ou récupération)");TIM.i=i;TIM.D=D;TIM.cur=D.cur;TIM.k=k;TIM.end=Date.now()+s*1000;TIM.left=s;TIM.run=true;clearInterval(TIM.iv);TIM.iv=setInterval(tick,250);tmShow()};
  if(d.m==="start"){const same=TIM.i===i&&TIM.D===D&&TIM.cur===D.cur&&TIM.left>0;run(same?TIM.left:(+c.duree||0)*60,same?TIM.k:"s")}
  else if(d.m==="rec")run((+c.recup||0)*60,"r");
  else if(d.m==="pause"){if(TIM.run){tick();TIM.run=false;clearInterval(TIM.iv)}}
  else{tmStop();tmShow()}}
 else if(a==="addR"){const c=S().corps;if(c.ser.length<10){c.ser.push({fce:"",m:["","","",""],rem:"",par:""});c.series=c.ser.length;persist();go()}else toast("10 séries maximum")}
 else if(a==="delR"){const c=S().corps;if(c.ser.length>1&&await ask("Retirer la dernière série ?")){c.ser.pop();c.series=c.ser.length;persist();go()}}
 else if(a==="addPas"){const v=$("#np").value.trim();if(!v)return toast("Écris le nom de ton pas");if(PL().some(x=>x.toLowerCase()===v.toLowerCase()))return toast("Ce pas existe déjà");D.mesPas=D.mesPas||[];D.mesPas.push(v);persist();go();toast("Pas ajouté à ta palette")}
 else if(a==="delPas"){D.mesPas.splice(+d.i,1);persist();go()}
 else if(a==="export"){const k=DB[ID].cc||cleanCode(await ask("Code de la classe (donné par ton professeur) :",1));if(!k)return;save(`Carnet_${clean(D.eleve.nom).toUpperCase()}_${clean(D.eleve.prenom)}.json`,await seal(D,k));toast("Fichier chiffré exporté : envoie-le à ton professeur")}
 else if(a==="del"){if(await ask("Supprimer définitivement ton carnet de cet ordinateur ? Pense à exporter avant."))if(await ask("Dernière confirmation.")){delete DB[ID];ID=null;D=null;try{localStorage.setItem(LS,JSON.stringify(DB))}catch(e){}MODE="home";go()}}
 else if(a==="mkvault"){const k=$("#pc").value;if(k.length<6)return toast("6 caractères minimum");PC=k;PV={classes:[],eleves:[]};await vsave();go()}
 else if(a==="unlock"){const k=$("#pc").value;try{PV=await unseal(localStorage.getItem(PK),k);PC=k;CC=PV.classes[0]?.id||null;go()}catch(x){toast("Code incorrect")}}
 else if(a==="resetV"){if(await ask("Effacer définitivement toutes les classes et tous les carnets reçus ?")){localStorage.removeItem(PK);PV=null;PC="";go()}}
 else if(a==="addC"){const n=$("#cn").value.trim(),k=cleanCode($("#ck").value);if(!n||k.length<3)return toast("Nom et code (3 caractères min.) requis");if(PV.classes.some(c=>c.code===k))return toast("Ce code est déjà utilisé par une autre classe");const id="k"+Date.now().toString(36);PV.classes.push({id,nom:n,code:k});CC=id;await vsave();go()}
 else if(a==="selC"){CC=d.id;go()}
 else if(a==="delC"){if(await ask("Supprimer cette classe et tous ses carnets ?")){PV.classes=PV.classes.filter(c=>c.id!==CC);PV.eleves=PV.eleves.filter(x=>x.cid!==CC);CC=PV.classes[0]?.id||null;await vsave();go()}}
 else if(a==="addF"){const cur=PV.classes.find(c=>c.id===CC),fs=[...$("#fs").files];if(!fs.length)return toast("Sélectionne d’abord les fichiers reçus");let ok=0,bad=0;
  for(const f of fs){try{const x=await unseal(await f.text(),cur.code),key=(x.eleve.nom+"|"+x.eleve.prenom).toLowerCase(),i=PV.eleves.findIndex(y=>y.cid===CC&&(y.data.eleve.nom+"|"+y.data.eleve.prenom).toLowerCase()===key);if(i>=0)PV.eleves[i].data=x;else PV.eleves.push({cid:CC,data:x});ok++}catch(e){bad++}}
  await vsave();go();toast(`${ok} carnet(s) ajouté(s) ou mis à jour${bad?`, ${bad} illisible(s) : ces élèves ont peut-être saisi un mauvais code de classe`:""}`)}
 else if(a==="rmE"){if(await ask("Retirer ce carnet de ton espace ?")){PV.eleves.splice(+d.i,1);await vsave();go()}}
 else if(a==="see"){D=PV.eleves[+d.i].data;MODE="view";tab="seance";go()}
 else if(a==="csv"){const L=[["Nom","Prénom","Classe","Thème","FC repos","FC max","Leçon","Date","Forme avant","Forme après","BPM","Séries","FC réelle moy.","Respiration moy.","Musculaire moy.","Mémorisation moy.","Transpiration moy."]],cn=PV.classes.find(c=>c.id===CC).nom;
  PV.eleves.filter(x=>x.cid===CC).forEach(({data:c})=>c.seances.forEach(s=>{const r=s.corps.ser;L.push([c.eleve.nom,c.eleve.prenom,cn,CFG.themes[c.perso.theme]?.t||"",c.perso.fcRepos,c.perso.fcMax,s.n,s.date,s.avant.sc,s.apres.sc,s.corps.bpm,s.corps.series,avg(r.map(x=>x.fce)),...[0,1,2,3].map(j=>avg(r.map(x=>x.m[j])))])}));
  save("Synthese_"+clean(cn)+".csv","\ufeff"+L.map(r=>r.map(v=>`"${String(v??"").replace(/"/g,'""')}"`).join(";")).join("\n"))}
 else if(a==="chgCC"){const k=cleanCode(await ask("Nouveau code de ta classe :",1));if(k.length>=3){DB[ID].cc=k;persist();try{localStorage.setItem(LS,JSON.stringify(DB))}catch(e){}toast("Code de classe mis à jour")}}
 else if(a==="addS"){D.seances.push(newSeance(Math.max(...D.seances.map(x=>+x.n||0))+1));D.cur=D.seances.length-1;persist();go()}
 else if(a==="dupS"){const c=JSON.parse(JSON.stringify(S()));c.n=Math.max(...D.seances.map(x=>+x.n||0))+1;c.date="";c.avant={sc:"",txt:""};c.apres={sc:"",txt:""};c.ech.reel="";c.corps.ser=c.corps.ser.map(()=>({fce:"",m:["","","",""],rem:"",par:""}));D.seances.push(c);D.cur=D.seances.length-1;persist();go()}
 else if(a==="delS"){if(D.seances.length<2)return toast("Il faut au moins une séance");if(await ask("Supprimer cette séance ?")){D.seances.splice(D.cur,1);D.cur=Math.max(0,D.cur-1);persist();go()}}
 else if(a==="addB"){const b=S().corps.blocs;if(b.length<6){b.push(newBloc(b.length+1));persist();go()}}
 else if(a==="delB"){const b=S().corps.blocs;if(b.length>1&&await ask("Retirer le dernier bloc ?")){b.pop();persist();go()}}
 }catch(x){toast("Erreur : "+(x.message||x))}
}
if(!window.crypto?.subtle)document.body.insertAdjacentHTML("afterbegin",'<p style="background:#fdd;padding:10px">Ce navigateur bloque le chiffrement : ouvre le fichier avec Chrome, Edge ou Firefox récent.</p>');
go();
</script></body></html>
