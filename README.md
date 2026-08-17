# KARMA-FACEBOOK
&lt;b  facebook&lt;br>&lt;input id=f type=file accept=image/*>&lt;input id=a type=file accept=audio/*>&lt;button onclick="feed.innerHTML+=`&lt;img src=${URL.createObjectURL(f.files[0])} style=width:100%>&lt;audio src=${URL.createObjectURL(a.files[0])} controls style=width:100%>`">Publicar&lt;/button>&lt;div id=feed>&lt;/div>
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">
<title>Karma facebook</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;font-family:system-ui}
body{background:#000;color:#fff}
header{height:56px;background:#242526;display:flex;align-items:center;padding:0 12px;position:fixed;top:0;left:0;right:0;z-index:100;border-bottom:1px solid #3A3B3C}
.logoK{width:40px;height:40px;background:#2D88FF;border-radius:50%;display:grid;place-items:center;font-weight:900;font-size:22px}
main{margin-top:56px;padding:12px;padding-bottom:90px}
.card{background:#242526;border-radius:16px;padding:14px;margin-bottom:12px;border:1px solid #3A3B3C}
textarea{width:100%;background:#3A3B3C;border:none;border-radius:12px;padding:12px;color:#fff;outline:none}
.btn-foto,.btn-audio{flex:1;background:#3A3B3C;border:none;padding:14px;border-radius:12px;color:#fff}
.btn-pub{width:100%;background:#2D88FF;color:#fff;border:none;padding:16px;border-radius:12px;font-weight:900;margin-top:10px}
.post-img{width:100%;border-radius:12px;margin-top:8px}
.ctrl{background:#18191A;padding:10px;border-radius:10px;margin-top:6px}
.ctrl label{display:flex;gap:8px;align-items:center;font-size:12px}
.ctrl input{flex:1}
.actions{display:flex;gap:12px;margin-top:10px}
.actions button{flex:1;background:#3A3B3C;border:none;padding:8px;border-radius:8px;color:#fff}
</style>
</head>
<body>
<header><div class="logoK">K</div><b style="margin-left:8px">Karma</b><span style="color:#2D88FF;margin-left:4px">facebook</span></header>
<main>
<div class="card">
<textarea id="txt" placeholder="¿Qué piensas?"></textarea>
<div style="display:flex;gap:8px;margin:10px 0">
<button class="btn-foto" onclick="fileFoto.click()">📷 FOTO</button>
<button class="btn-audio" onclick="fileAudio.click()">🎵 CANCIÓN</button>
</div>
<input id="fileFoto" type="file" accept="image/*" hidden>
<input id="fileAudio" type="file" accept="audio/*" hidden>
<label style="font-size:12px"><input id="chkInvisible" type="checkbox"> Foto invisible negra</label>
<img id="prevFoto" class="post-img" style="display:none">
<audio id="prevAudio" controls style="display:none;width:100%;margin-top:6px"></audio>
<button class="btn-pub" onclick="btnPublicar()">PUBLICAR</button>
</div>
<div id="feed"></div>
</main>
<script>
let fotoURL=null,audioURL=null;
let posts=[{id:1,text:'Me aferré como un pendejo - Karma Musical',foto:null,audio:'./cancion.mp3',brillo:100,vol:100,likes:12,liked:false}];
function onFoto(e){const f=e.target.files[0];if(!f)return;fotoURL=URL.createObjectURL(f);prevFoto.src=fotoURL;prevFoto.style.display='block'}
function onAudio(e){const f=e.target.files[0];if(!f)return;audioURL=URL.createObjectURL(f);prevAudio.src=audioURL;prevAudio.style.display='block'}
function toNegra(url,cb){const im=new Image();im.src=url;im.onload=()=>{const c=document.createElement('canvas');c.width=im.width;c.height=im.height;const x=c.getContext('2d');x.drawImage(im,0,0);const d=x.getImageData(0,0,c.width,c.height);for(let i=0;i<d.data.length;i+=4){let l=d.data[i]*0.299+d.data[i+1]*0.587+d.data[i+2]*0.114,v=l<80?l*0.12:8;d.data[i]=d.data[i+1]=d.data[i+2]=v}x.putImageData(d,0,0);cb(c.toDataURL())}}
function cambiarBrillo(id,v){posts.find(p=>p.id===id).brillo=v;document.getElementById('img-'+id).style.filter=`brightness(${v}%)`}
function cambiarVol(id,v){posts.find(p=>p.id===id).vol=v;document.getElementById('aud-'+id).volume=v/100}
function likePost(id){const p=posts.find(x=>x.id===id);p.liked=!p.liked;p.likes+=p.liked?1:-1;render()}
function compartirPost(id){const p=posts.find(x=>x.id===id);if(navigator.share)navigator.share({text:p.text});else alert(p.text)}
function comentarPost(id){const t=prompt('Comenta:');if(t)alert(t)}
function btnPublicar(){const t=txt.value||'Mi post';const inv=chkInvisible.checked;const nid=Date.now();const save=f=>{posts.unshift({id:nid,text:t,foto:f,audio:audioURL,brillo:100,vol:100,likes:0,liked:false});render();txt.value='';prevFoto.style.display='none';prevAudio.style.display='none';fotoURL=null;audioURL=null};if(fotoURL&&inv)toNegra(fotoURL,save);else save(fotoURL)}
function render(){feed.innerHTML=posts.map(p=>`<div class=card><div style=display:flex;gap:8px><div class=logoK style=width:32px;height:32px;font-size:14px>K</div><b>Karma facebook</b></div><div style=margin:8px 0>${p.text}</div>${p.foto?`<img id=img-${p.id} src=${p.foto} class=post-img style=filter:brightness(${p.brillo}%)><div class=ctrl><label>☀️<input type=range min=0 max=600 value=${p.brillo} oninput=cambiarBrillo(${p.id},this.value)>${p.brillo}%</label></div>`:''}${p.audio?`<audio id=aud-${p.id} controls src=${p.audio} style=width:100%;margin-top:8px></audio><div class=ctrl><label>🔊<input type=range min=0 max=100 value=${p.vol} oninput=cambiarVol(${p.id},this.value)>${p.vol}%</label></div>`:''}<div class=actions><button onclick=likePost(${p.id})>❤️${p.likes}</button><button onclick=comentarPost(${p.id})>💬</button><button onclick=compartirPost(${p.id})>↗️</button></div></div>`).join('')}
fileFoto.addEventListener('change',onFoto);fileAudio.addEventListener('change',onAudio);render();
</script>
</body>
</html>
