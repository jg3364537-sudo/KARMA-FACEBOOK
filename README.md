# Karma facebook - Repositorio oficial

App donde K es el logo. Foto + Canción juntas.

## Estructura del repositorio
- index.html - App universal telefono y tablet
- manifest.json - Para instalar como APK
- sw.js - Funciona offline
- icons/icon-192.png y icon-512.png - Logo K azul #2D88FF

## Funciones cada una independiente
- btnSubirFoto() - solo foto
- btnSubirAudio() - solo cancion
- cambiarBrillo(id) - brillo independiente por foto
- cambiarVol(id) - volumen independiente por cancion
- likePost(id) - like independiente
- btnPublicar() - publica

## Instalar
1. Sube este repo a github.com/new -> Karma-facebook
2. Activa GitHub Pages
3. O sube a netlify.com/drop para APK

Autor: Karma Musical 2026
Logo: K en circulo azul #2D88FF
import { useState, useRef } from 'react';

const EMOJIS_ILIMITADOS = ["😀","😂","❤️","🔥","👏","😍","😭","🥰","😎","🤩","🙏","💯","😱","🤣","😮","🥺","😡","👍","👎","💀","🎉","✨","🌟","💖","😢","😅","🤔","🫶","👌","🫡"];

export default function LiveYComentarios() {
  const [enVivo, setEnVivo] = useState(false);
  const [comentario, setComentario] = useState('');
  const [comentarios, setComentarios] = useState([]);
  const [showEmojis, setShowEmojis] = useState(true);
  const videoRef = useRef(null);
  const streamRef = useRef(null);

  // INICIAR SESIÓN LIVE
  const iniciarLive = async () => {
    try {
      const stream = await navigator.mediaDevices.getUserMedia({ 
        video: { width: 1280, height: 720 }, 
        audio: true 
      });
      streamRef.current = stream;
      if (videoRef.current) {
        videoRef.current.srcObject = stream;
      }
      setEnVivo(true);
    } catch {
      alert('Permite cámara y micrófono para el live');
    }
  };

  // TERMINAR LIVE
  const terminarLive = () => {
    if (streamRef.current) {
      streamRef.current.getTracks().forEach(t => t.stop());
    }
    setEnVivo(false);
  };

  // COMENTAR - ILIMITADO
  const enviarComentario = () => {
    if (!comentario.trim()) return;
    const nuevo = {
      id: Date.now(),
      texto: comentario,
      usuario: 'Tú',
      hora: new Date().toLocaleTimeString()
    };
    setComentarios(prev => [...prev, nuevo]); // sin límite
    setComentario('');
  };

  const agregarEmoji = (emoji) => {
    setComentario(prev => prev + emoji);
  };

  return (
    <div style={{ maxWidth: 420, margin: '0 auto', background: '#111', color: 'white', borderRadius: 16, overflow: 'hidden', fontFamily: 'sans-serif' }}>
      
      {/* VIDEO LIVE */}
      <div style={{ position: 'relative', background: 'black', height: 380 }}>
        <video ref={videoRef} autoPlay muted playsInline style={{ width: '100%', height: '100%', objectFit: 'cover' }} />
        {!enVivo && <div style={{ position: 'absolute', inset: 0, display: 'flex', alignItems: 'center', justifyContent: 'center', background: '#222' }}>Cámara apagada</div>}
        
        <div style={{ position: 'absolute', top: 10, left: 10, background: enVivo ? 'red' : 'gray', padding: '4px 10px', borderRadius: 20, fontSize: 12, fontWeight: 'bold' }}>
          {enVivo ? '● EN VIVO' : 'OFFLINE'}
        </div>

        <div style={{ position: 'absolute', bottom: 0, left: 0, right: 0, maxHeight: 150, overflowY: 'auto', padding: 10, background: 'linear-gradient(transparent, rgba(0,0,0,0.8))' }}>
          {comentarios.map(c => (
            <div key={c.id} style={{ marginBottom: 4, fontSize: 14 }}>
              <b>{c.usuario}: </b>{c.texto}
            </div>
          ))}
        </div>
      </div>

      {/* CONTROLES LIVE */}
      <div style={{ padding: 10, display: 'flex', gap: 8 }}>
        {!enVivo ? (
          <button onClick={iniciarLive} style={{ flex: 1, padding: 12, background: 'red', color: 'white', border: 'none', borderRadius: 10, fontWeight: 'bold' }}>🔴 Iniciar Live</button>
        ) : (
          <button onClick={terminarLive} style={{ flex: 1, padding: 12, background: '#333', color: 'white', border: 'none', borderRadius: 10, fontWeight: 'bold' }}>Finalizar Live</button>
        )}
      </div>

      {/* CAJA DE COMENTARIOS */}
      <div style={{ padding: 10, background: '#1a1a1a' }}>
        <div style={{ display: 'flex', gap: 8, marginBottom: 8 }}>
          <input
            value={comentario}
            onChange={e => setComentario(e.target.value)}
            onKeyDown={e => e.key === 'Enter' && enviarComentario()}
            placeholder="Escribe un comentario..."
            style={{ flex: 1, padding: 12, borderRadius: 20, border: 'none', background: '#2a2a2a', color: 'white', outline: 'none' }}
          />
          <button onClick={enviarComentario} style={{ padding: '10px 16px', background: '#0084ff', color: 'white', border: 'none', borderRadius: 20, fontWeight: 'bold' }}>Enviar</button>
        </div>

        {/* EMOJIS ILIMITADOS */}
        <div style={{ display: 'flex', flexWrap: 'wrap', gap: 6, maxHeight: 120, overflowY: 'auto', background: '#222', padding: 8, borderRadius: 12 }}>
          {EMOJIS_ILIMITADOS.map((em, i) => (
            <button key={i} onClick={() => agregarEmoji(em)} style={{ fontSize: 22, background: 'transparent', border: 'none', cursor: 'pointer' }}>{em}</button>
          ))}
          <button onClick={() => {
            const mas = prompt('Escribe cualquier emoji que quieras agregar:');
            if (mas) agregarEmoji(mas);
          }} style={{ fontSize: 12, background: '#333', color: 'white', border: 'none', borderRadius: 20, padding: '4px 10px' }}>+ Más emojis</button>
        </div>
        <p style={{ fontSize: 11, color: '#888', marginTop: 6, textAlign: 'center' }}>Comentarios y emojis ILIMITADOS - sin restricción</p>
      </div>
    </div>
  );import { useState } from 'react';

// SIMULA TU USUARIO LOGUEADO
const YO = { id: 'jaime_123', nombre: 'Jaime' };

export default function PublicacionesTipoFacebook() {
  const [publicaciones, setPublicaciones] = useState([
    { id: 1, texto: 'Mi primera foto', tipo: 'estado', likes: 0, likedBy: [], comentarios: [] },
    { id: 2, texto: 'Chequen este video', tipo: 'video', likes: 2, likedBy: ['otro1', 'otro2'], comentarios: [] },
  ]);
  const [textoComentario, setTextoComentario] = useState({});

  // LIKE TIPO FACEBOOK - 1 POR USUARIO POR PUBLICACIÓN
  const darLike = (idPublicacion) => {
    setPublicaciones(prev => prev.map(pub => {
      if (pub.id!== idPublicacion) return pub;

      const yaDioLike = pub.likedBy.includes(YO.id);

      if (yaDioLike) {
        // Si ya dio like, lo quita (como Facebook)
        return {
         ...pub,
          likes: pub.likes - 1,
          likedBy: pub.likedBy.filter(uid => uid!== YO.id)
        };
      } else {
        // Si no ha dado like, se lo agrega - SOLO 1
        return {
         ...pub,
          likes: pub.likes + 1,
          likedBy: [...pub.likedBy, YO.id]
        };
      }
    }));
  };

  // COMENTARIOS ILIMITADOS
  const comentar = (idPublicacion) => {
    const texto = textoComentario[idPublicacion];
    if (!texto?.trim()) return;

    setPublicaciones(prev => prev.map(pub => {
      if (pub.id!== idPublicacion) return pub;
      return {
       ...pub,
        comentarios: [...pub.comentarios, {
          id: Date.now(),
          usuario: YO.nombre,
          texto: texto
        }]
      };
    }));
    setTextoComentario(prev => ({...prev, [idPublicacion]: '' }));
  };

  return (
    <div style={{ maxWidth: 440, margin: '0 auto', fontFamily: 'sans-serif', background: '#f0f2f5', padding: 10 }}>
      {publicaciones.map(pub => {
        const yoLeDiLike = pub.likedBy.includes(YO.id);
        return (
          <div key={pub.id} style={{ background: 'white', borderRadius: 10, padding: 12, marginBottom: 12, boxShadow: '0 1px 2px rgba(0,0,0,0.1)' }}>
            <p>{pub.texto}</p>

            <div style={{ display: 'flex', justifyContent: 'space-between', fontSize: 13, color: '#65676b', padding: '8px 0', borderTop: '1px solid #eee' }}>
              <span>👍 {pub.likes} likes</span>
              <span>{pub.comentarios.length} comentarios</span>
            </div>

            <div style={{ display: 'flex', gap: 5, borderTop: '1px solid #eee', borderBottom: '1px solid #eee', padding: '5px 0' }}>
              <button
                onClick={() => darLike(pub.id)}
                style={{ flex: 1, padding: 8, border: 'none', borderRadius: 6, background: yoLeDiLike? '#e7f3ff' : 'transparent', color: yoLeDiLike? '#0064d1' : '#65676b', fontWeight: 'bold', cursor: 'pointer' }}
              >
                {yoLeDiLike? '👍 Te gusta' : '👍 Me gusta'}
              </button>
              <button style={{ flex: 1, padding: 8, border: 'none', borderRadius: 6, background: 'transparent', color: '#65676b', fontWeight: 'bold' }}>💬 Comentar</button>
            </div>

            {/* COMENTARIOS ILIMITADOS */}
            <div style={{ marginTop: 8 }}>
              {pub.comentarios.map(c => (
                <div key={c.id} style={{ background: '#f0f2f5', padding: '6px 10px', borderRadius: 15, marginBottom: 4, fontSize: 14 }}>
                  <b>{c.usuario}: </b>{c.texto}
                </div>
              ))}
              <div style={{ display: 'flex', gap: 6, marginTop: 6 }}>
                <input
                  value={textoComentario[pub.id] || ''}
                  onChange={e => setTextoComentario(prev => ({...prev, [pub.id]: e.target.value }))}
                  onKeyDown={e => e.key === 'Enter' && comentar(pub.id)}
                  placeholder="Escribe un comentario..."
                  style={{ flex: 1, borderRadius: 20, border: 'none', background: '#f0f2f5', padding: '8px 12px', outline: 'none' }}
                />
              </div>
            </div>
          </div>
        )
      })}
    </div>
  );
}
}
const alCargarArchivo = (e) => {
  const file = e.target.files[0];
  if (!file) return;

  const url = URL.createObjectURL(file);

  // DETECTA BIEN SI ES MP3 AUNQUE VENGA COMO application/octet-stream
  const esAudio = file.type.startsWith('audio/') || file.name.toLowerCase().endsWith('.mp3') || file.name.toLowerCase().endsWith('.wav') || file.name.toLowerCase().endsWith('.m4a');
  const esVideo = file.type.startsWith('video/');
  const esFoto = file.type.startsWith('image/');

  let tipoFinal = 'archivo';
  if (esFoto) tipoFinal = 'foto';
  else if (esVideo) tipoFinal = 'video';
  else if (esAudio) tipoFinal = 'audio';

  const nuevaPub = {
    id: Date.now(),
    tipo: tipoFinal,
    texto: texto,
    archivo: url,
    nombreArchivo: file.name,
    fecha: new Date().toLocaleString(),
    likes: 0,
    likedBy: [],
    comentarios: []
  };

  setPublicaciones(prev => [nuevaPub,...prev]);
  setTexto('');
  e.target.value = ''; // limpia el input para poder subir el mismo MP3 de nuevo
};
import { useState } from 'react';

const YO = { id: 'jaime_123', nombre: 'Jaime' };

export default function App() {
  const [texto, setTexto] = useState('');
  const [publicaciones, setPublicaciones] = useState([]);

  // ESTADOS DE LOS BOTONES DEL PERFIL
  const [estadoSolicitud, setEstadoSolicitud] = useState('nada'); // nada | enviada | amigos
  const [siguiendo, setSiguiendo] = useState(false);
  const [likePagina, setLikePagina] = useState(false);

  const alCargarArchivo = (e) => {
    const file = e.target.files[0];
    if (!file) return;
    const url = URL.createObjectURL(file);

    const esAudio = file.type.startsWith('audio/') || file.name.toLowerCase().endsWith('.mp3') || file.name.toLowerCase().endsWith('.m4a') || file.name.toLowerCase().endsWith('.wav');
    const esVideo = file.type.startsWith('video/');
    const esFoto = file.type.startsWith('image/');

    let tipo = 'archivo';
    if (esFoto) tipo = 'foto';
    else if (esVideo) tipo = 'video';
    else if (esAudio) tipo = 'audio';

    setPublicaciones(p => [{ id: Date.now(), tipo, texto, archivo: url, nombreArchivo: file.name, likes: 0, likedBy: [], comentarios: [] },...p]);
    setTexto('');
    e.target.value = '';
  };

  const publicarEstado = () => {
    if (!texto.trim()) return;
    setPublicaciones(p => [{ id: Date.now(), tipo: 'estado', texto, archivo: null, likes: 0, likedBy: [], comentarios: [] },...p]);
    setTexto('');
  };

  const darLike = (id) => {
    setPublicaciones(prev => prev.map(pub => {
      if (pub.id!== id) return pub;
      const yaDioLike = pub.likedBy.includes(YO.id);
      return yaDioLike
       ? {...pub, likes: pub.likes - 1, likedBy: pub.likedBy.filter(u => u!== YO.id) }
        : {...pub, likes: pub.likes + 1, likedBy: [...pub.likedBy, YO.id] };
    }));
  };

  return (
    <div style={{ background: '#f0f2f5', minHeight: '100vh', fontFamily: 'sans-serif' }}>
      {/* PORTADA PERFIL */}
      <div style={{ background: 'white', paddingBottom: 15, boxShadow: '0 1px 2px rgba(0,0,0,0.1)' }}>
        <div style={{ height: 180, background: 'linear-gradient(#1877f2, #ddd)' }}></div>
        <div style={{ padding: '0 20px', marginTop: -30, display: 'flex', alignItems: 'end', gap: 15, flexWrap: 'wrap' }}>
          <img src="https://i.pravatar.cc/150" style={{ width: 100, height: 100, borderRadius: '50%', border: '4px solid white' }} />
          <div>
            <h2 style={{ margin: 0 }}>Karma Musical</h2>
            <p style={{ margin: 0, color: '#65676b' }}>{likePagina? '2.5K Me gusta' : '2.4K Me gusta'} • 5K seguidores</p><!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FACEMEX</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Inter,system-ui}
body{background:#0f0f0f;color:white}
.top{background:#1a1a1a;padding:14px;text-align:center;border-bottom:2px solid #2D88FF;position:sticky;top:0;z-index:10}
.top h1{color:#2D88FF;font-size:32px;letter-spacing:3px;font-weight:900}
.box{max-width:420px;margin:20px auto;padding:16px}
.card{background:#1e1e1e;border-radius:18px;padding:14px;margin-bottom:12px;border:1px solid #2a2a2a}
input,textarea{width:100%;background:#111;border:1px solid #333;color:white;padding:12px;border-radius:12px;margin-bottom:8px}
.btn{width:100%;background:#2D88FF;color:white;border:none;padding:12px;border-radius:12px;font-weight:800}
.row{display:flex;gap:6px;flex-wrap:wrap;margin-top:8px}
.chip{background:#2a2a2a;padding:8px 12px;border-radius:20px;font-size:13px;border:none;color:white}
.oculto{display:none}
.feed-img,.feed-video{width:100%;border-radius:14px;margin-top:8px}
.emoji-grid{display:grid;grid-template-columns:repeat(8,1fr);gap:4px;background:#111;padding:8px;border-radius:12px;max-height:140px;overflow:auto}
</style>
</head>
<body>
<div class="top"><h1>FACEMEX</h1></div>

<div class="box" id="p1">
  <div class="card">
    <h3 style="text-align:center;margin-bottom:10px">Crear cuenta FACEMEX</h3>
    <input id="cor" placeholder="Correo">
    <input id="pas" type="password" placeholder="Contraseña">
    <button class="btn" onclick="genCod()">Continuar</button>
  </div>
</div>

<div class="box oculto" id="p2">
  <div class="card" style="text-align:center">
    <p>Código enviado a <b id="tCor"></b></p>
    <p id="tCod" style="color:#2D88FF;font-weight:900;font-size:18px;margin:8px 0"></p>
    <input id="cod" placeholder="000000" style="text-align:center;letter-spacing:8px;font-size:22px">
    <button class="btn" onclick="entrar()">Entrar a FACEMEX</button>
  </div>
</div>

<div class="box oculto" id="p3">
  <div class="card" style="text-align:center">
    <div style="font-size:50px">👤</div>
    <h3>Bienvenido</h3>
    <p id="segTxt" style="color:#888;font-size:12px">0 Me gusta • 0 seguidores</p>
    <div class="row" style="justify-content:center"><button class="chip" onclick="seguir()">+ Seguir</button><button class="chip" onclick="likePage()">👍 Like</button></div>
  </div>
  <div class="card">
    <textarea id="txt" placeholder="¿Qué pasa en México?"></textarea>
    <div class="emoji-grid" id="emGrid"></div>
    <div class="row">
      <input type="file" id="f1" accept="image/*" hidden onchange="upF(event)"><input type="file" id="f2" accept="video/*" hidden onchange="upV(event)"><input type="file" id="f3" accept="audio/*" hidden onchange="upA(event)">
      <button class="chip" onclick="f1.click()">📷 Foto</button><button class="chip" onclick="f2.click()">🎬 Video</button><button class="chip" onclick="f3.click()">🎵 Audio</button><button class="btn" style="flex:1" onclick="post()">Publicar</button>
    </div>
    <div id="prev"></div>
  </div>
  <div id="feed"></div>
</div>

<script>
let codG='', tf=null,tv=null,ta=null, posts=[], seg=0, lk=0;
let EMO=["😀","😂","❤️","🔥","🇲🇽","💚","🤍","😍","🥺","😎","🫶","✨","🎉","👏","💯","😭","🤩","🙏","😱","💀"];
function genCod(){ if(!cor.value) return alert('correo'); codG=Math.floor(100000+Math.random()*900000)+''; tCor.innerText=cor.value; tCod.innerText='CODIGO: '+codG; p1.classList.add('oculto'); p2.classList.remove('oculto'); emGrid.innerHTML=EMO.map(e=>`<button onclick="txt.value+= '${e}'" style="background:transparent;border:none;font-size:20px">${e}</button>`).join('');}
function entrar(){ if(cod.value===codG){ p2.classList.add('oculto'); p3.classList.remove('oculto'); } else alert('codigo mal'); }
function upF(e){ tf=URL.createObjectURL(e.target.files[0]); verP(); } function upV(e){ tv=URL.createObjectURL(e.target.files[0]); verP(); } function upA(e){ ta=URL.createObjectURL(e.target.files[0]); verP(); }
function verP(){ prev.innerHTML=`${tf?`<img src="${tf}" class="feed-img">`:''}${tv?`<video src="${tv}" controls class="feed-video">`:''}${ta?`<audio src="${ta}" controls style="width:100%">`:''}`; }
function seguir(){ seg++; upS(); } function likePage(){ lk++; upS(); } function upS(){ segTxt.innerText=`${lk} Me gusta • ${seg} seguidores`; }
function post(){ if(!txt.value &&!tf &&!tv &&!ta) return; posts.unshift({id:Date.now(),t:txt.value,f:tf,v:tv,a:ta,l:0,lc:false,com:[]}); txt.value=''; prev.innerHTML=''; tf=tv=ta=null; render(); }
function like(id){ let p=posts.find(x=>x.id===id); p.lc=!p.lc; p.l+=p.lc?1:-1; render(); }
function render(){ feed.innerHTML=posts.map(p=>`
<div class="card">
<p>${p.t}</p>
${p.f?`<img src="${p.f}" class="feed-img" id="im${p.id}"><input type="range" min="30" max="150" value="100" style="width:100%" oninput="document.getElementById('im${p.id}').style.filter='brightness('+this.value+'%)'">`:''}
${p.v?`<video src="${p.v}" controls class="feed-video"></video>`:''}
${p.a?`<audio src="${p.a}" controls style="width:100%;margin-top:6px"></audio>`:''}
<div style="display:flex;justify-content:space-between;font-size:12px;color:#888;margin-top:8px;border-top:1px solid #2a2a2a;padding-top:6px"><span>👍 ${p.l} likes ilimitados</span><span>${p.com.length} comentarios</span></div>
<div class="row"><button class="chip" style="flex:1;color:${p.lc?'#2D88FF':''}" onclick="like(${p.id})">${p.lc?'Te gusta':'Me gusta'}</button><button class="chip" style="flex:1" onclick="let c=prompt('comenta'); if(c){ posts.find(x=>x.id===${p.id}).com.push(c); render(); }">Comentar</button></div>
${p.com.map(c=>`<div style="background:#111;padding:6px 10px;border-radius:12px;margin-top:4px;font-size:13px">${c}</div>`).join('')}
</div>`).join(''); }
</script>
</body>
</html><!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>KAR FACEMEX</title>
<script src="https://unpkg.com/peerjs@1.5.2/dist/peerjs.min.js"></script>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:system-ui}
body{background:#0a0a0a;color:#fff;height:100vh;display:flex;flex-direction:column}
.header{background:#121212;padding:12px;display:flex;justify-content:space-between;border-bottom:1px solid #222}
.main{flex:1;display:flex;overflow:hidden}
.sidebar{width:320px;background:#121212;border-right:1px solid #222;display:flex;flex-direction:column}
.list{flex:1;overflow:auto;padding:8px}
.user-card{background:#1e1e1e;margin-bottom:8px;padding:10px;border-radius:12px;display:flex;justify-content:space-between;cursor:pointer}
.avatar{width:36px;height:36px;background:linear-gradient(45deg,#00d4ff,#7c3aed);border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:bold}
.chat-area{flex:1;display:flex;flex-direction:column;position:relative}
.messages{flex:1;overflow:auto;padding:16px;display:flex;flex-direction:column;gap:8px}
.msg{max-width:75%;padding:10px 14px;border-radius:18px;font-size:14px}
.msg.me{align-self:flex-end;background:#00d4ff;color:#000}
.msg.other{align-self:flex-start;background:#1e1e1e}
.input-area{padding:12px;background:#121212;display:flex;gap:8px}
.input-area input{flex:1;background:#1e1e1e;border:1px solid #333;padding:12px 16px;border-radius:25px;color:#fff;outline:none}
.video-container{position:absolute;inset:0;background:#000;z-index:20;display:none;flex-direction:column}
.video-container.active{display:flex}
#remoteVideo{width:100%;height:100%;object-fit:cover}
#localVideo{position:absolute;bottom:20px;right:20px;width:110px;height:150px;border-radius:12px;object-fit:cover;border:2px solid #fff}
.call-controls{padding:20px;background:#121212;display:flex;justify-content:center;gap:16px}
.ctrl{width:56px;height:56px;border-radius:50%;border:none;font-size:22px;cursor:pointer}
.ctrl.hang{background:#ff3040;color:#fff}
#login{position:fixed;inset:0;background:#000;z-index:100;display:flex;align-items:center;justify-content:center;padding:20px}
.login-card{background:#121212;padding:28px;border-radius:20px;width:100%;max-width:340px;border:1px solid #222}
</style>
</head>
<body>
<div id="login">
  <div class="login-card">
    <h2>KAR <span style="color:#00d4ff">FACEMEX</span></h2>
    <p style="color:#888;font-size:13px;margin:10px 0 20px">Con cámara, mic, chat y solicitudes</p>
    <input id="nameInput" placeholder="Tu nombre" style="width:100%;padding:14px;background:#0a0a0a;border:1px solid #333;border-radius:12px;color:#fff;margin-bottom:12px">
    <button onclick="iniciar()" style="width:100%;padding:14px;background:#00d4ff;border:none;border-radius:12px;font-weight:bold;cursor:pointer">Activar cámara y entrar</button>
  </div>
</div>

<div class="header">
  <div style="font-weight:900">KAR <span style="color:#00d4ff">FACEMEX</span></div>
  <div style="background:#1e1e1e;padding:6px 10px;border-radius:20px;font-size:11px">ID: <b id="myId">...</b> <span onclick="navigator.clipboard.writeText(myId.innerText)">📋</span></div>
</div>

<div class="main">
  <div class="sidebar">
    <div style="padding:10px;display:flex;gap:6px"><input id="peerIdInput" placeholder="Pega ID KAR-XXXX" style="flex:1;background:#0a0a0a;border:1px solid #333;padding:8px;border-radius:8px;color:#fff"><button onclick="conectar()" style="background:#00d4ff;border:none;padding:8px 12px;border-radius:8px;font-weight:bold">Conectar</button></div>
    <div class="list" id="userList"></div>
  </div>
  <div class="chat-area">
    <div style="padding:12px;background:#121212;display:flex;gap:10px;align-items:center">
      <div class="avatar" id="chatAvatar">K</div><div id="chatName" style="font-weight:bold">Selecciona un chat</div>
      <div style="margin-left:auto;display:flex;gap:8px"><button onclick="llamar(false)" style="width:42px;height:42px;border-radius:50%;border:none;background:#00ff88">📞</button><button onclick="llamar(true)" style="width:42px;height:42px;border-radius:50%;border:none;background:#7c3aed;color:#fff">📹</button></div>
    </div>
    <div class="messages" id="messages"><div style="text-align:center;color:#666;margin-top:40px">Conecta con un ID para empezar</div></div>
    <div class="input-area"><input id="msgInput" placeholder="Mensaje..." onkeypress="if(event.key==='Enter') enviar()"><button onclick="enviar()" style="width:42px;height:42px;border-radius:50%;border:none;background:#00d4ff">➤</button></div>
    <div class="video-container" id="videoBox"><video id="remoteVideo" autoplay playsinline></video><video id="localVideo" autoplay playsinline muted></video><div class="call-controls"><button class="ctrl hang" onclick="colgar()">✕</button></div></div>
  </div>
</div>

<script>
let peer, localStream, currentCall, currentConn, amigos=JSON.parse(localStorage.getItem('kar_amigos')||'[]'), chats=JSON.parse(localStorage.getItem('kar_chats')||'{}'), currentChatId=null, miNombre='';
function iniciar(){miNombre=nameInput.value||'Jaime';login.style.display='none';peer=new Peer('KAR-'+Math.random().toString(36).substr(2,6).toUpperCase());peer.on('open',id=>myId.innerText=id);peer.on('connection',c=>setup(c));peer.on('call',async call=>{if(confirm('Llamada de '+call.peer+' ¿Aceptar?')){localStream=await navigator.mediaDevices.getUserMedia({audio:true,video:call.metadata.video});localVideo.srcObject=localStream;call.answer(localStream);setupCall(call);}});render();}
function conectar(){const id=peerIdInput.value.trim();if(!id)return;setup(peer.connect(id,{metadata:{nombre:miNombre}}));}
function setup(conn){conn.on('open',()=>{currentConn=conn;if(!amigos.find(a=>a.id===conn.peer))amigos.push({id:conn.peer,nombre:conn.metadata.nombre||conn.peer});currentChatId=conn.peer;chatName.innerText=conn.metadata.nombre||conn.peer;chatAvatar.innerText=(conn.metadata.nombre||'K')[0];render();save();conn.on('data',d=>{if(!chats[conn.peer])chats[conn.peer]=[];chats[conn.peer].push({me:false,texto:d.texto});save();renderMsg();});});}
function enviar(){const t=msgInput.value.trim();if(!t||!currentChatId)return;if(!chats[currentChatId])chats[currentChatId]=[];chats[currentChatId].push({me:true,texto:t});if(currentConn)currentConn.send({texto:t});msgInput.value='';save();renderMsg();}
function renderMsg(){const b=messages;if(!currentChatId||!chats[currentChatId])return;b.innerHTML=chats[currentChatId].map(m=>`<div class="msg ${m.me?'me':'other'}">${m.texto}</div>`).join('');b.scrollTop=b.scrollHeight;}
async function llamar(v){if(!currentChatId)return alert('Conecta primero');localStream=await navigator.mediaDevices.getUserMedia({audio:true,video:v});localVideo.srcObject=localStream;videoBox.classList.add('active');const call=peer.call(currentChatId,localStream,{metadata:{video:v}});setupCall(call);}
function setupCall(call){currentCall=call;videoBox.classList.add('active');call.on('stream',s=>remoteVideo.srcObject=s);call.on('close',colgar);}
function colgar(){if(currentCall)currentCall.close();if(localStream)localStream.getTracks().forEach(t=>t.stop());videoBox.classList.remove('active');}
function render(){userList.innerHTML=amigos.map(u=>`<div class="user-card" onclick="currentChatId='${u.id}';chatName.innerText='${u.nombre}';chatAvatar.innerText='${u.nombre[0]}';currentConn=peer.connections['${u.id}']?.[0];renderMsg()"><div style="display:flex;gap:10px;align-items:center"><div class="avatar">${u.nombre[0]}</div><div><b>${u.nombre}</b><br><span style="font-size:11px;color:#888">${u.id}</span></div></div><span style="color:#00ff88">●</span></div>`).join('');}
function save(){localStorage.setItem('kar_amigos',JSON.stringify(amigos));localStorage.setItem('kar_chats',JSON.stringify(chats));}
</script>
</body>
</html>android {
  namespace 'com.kar.facemex'
  compileSdk 34
  buildFeatures { buildConfig true }
  packaging {
    resources { excludes += '/META-INF/{AL2.0,LGPL2.1}' }
  }
}

android.buildTypes.release {
  minifyEnabled false // <--- Apágalo temporal para compilar
  // si lo quieres con minify:
  // minifyEnabled true
  // proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
}
<video 
  src="URL_DEL_VIDEO" 
  controls 
  playsinline 
  preload="metadata" 
  poster="miniatura.jpg"
  style="width:100%;border-radius:12px">
  <source src="URL_DEL_VIDEO" type="video/mp4">
</video>
<div style="font-size:10px;color:#888">
  📹 1920x1080 • 2.5s • video/mp4 • 3.2 MB • 👁️ 1.2k
</div>
<script>
  const v = document.createElement('video');
  v.preload='metadata';
  v.src=url;
  v.onloadedmetadata=()=>{
    console.log(v.duration, v.videoWidth, v.videoHeight);
  };
</script><div style="background:#111;border:1px solid #222;border-radius:16px;padding:12px">
  <span style="background:#ffcc00;color:#000;font-size:10px;padding:2px 8px;border-radius:10px">PATROCINADO</span>
  <h3>Título de tu anuncio</h3>
  <video src="video_anuncio.mp4" controls preload="metadata" style="width:100%;border-radius:10px;margin-top:8px"></video>
  <button onclick="window.open('https://tu-link.com','_blank')" style="width:100%;margin-top:8px;background:#ffcc00;color:#000;border:none;padding:10px;border-radius:10px;font-weight:800">Ver oferta →</button>
</div>// VOLUMEN
video.volume = 0.5; // 0 a 1
audio.volume = 0.8;
video.muted = true/false;

// CAPTURA DE PANTALLA DE VIDEO
function capturarPantalla(){
  const canvas = document.createElement('canvas');
  canvas.width = video.videoWidth;
  canvas.height = video.videoHeight;
  canvas.getContext('2d').drawImage(video,0,0);
  const imagen = canvas.toDataURL('image/png');
  document.getElementById('poster').src = imagen;
  video.poster = imagen; // para que se vea como miniatura
}

// SLIDER HTML
<input type="range" min="0" max="1" step="0.05" value="1" 
 oninput="video.volume=this.value">
<!DOCTYPE html>
<html lang="es"><head><style>
@font-face {
  font-family: "Optimistic";
  font-style: normal;
  font-weight: 400 600;
  font-display: swap;
  src: url("/fonts/OptimisticAI_VF_Optimized.woff2") format("woff2");
}
@font-face {
  font-family: "Optimistic Mono";
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("/fonts/OptimisticMono_W_TextRegular.woff2") format("woff2");
}
:where(html) {
  font-family: "Optimistic", system-ui, sans-serif;
}
:where(code, pre, kbd, samp) {
  font-family: "Optimistic Mono", ui-monospace, monospace;
}
</style><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>KAR FACEMEX FINAL DEFINITIVO</title><script src="https://unpkg.com/peerjs@1.5.2/dist/peerjs.min.js"></script><style>*{margin:0;padding:0;box-sizing:border-box;font-family:system-ui}body{background:#050505;color:#fff;min-height:100vh;display:flex;flex-direction:column}.header{background:#111;padding:10px 14px;display:flex;justify-content:space-between;border-bottom:1px solid #222;position:sticky;top:0;z-index:20}.logo{font-weight:900;font-size:16px}.logo span{color:#00d4ff}.main{display:flex;flex:1;overflow:hidden}.left{width:270px;background:#111;border-right:1px solid #222;display:flex;flex-direction:column}.center{flex:1;overflow:auto;padding:10px;max-width:650px;margin:0 auto;width:100%}.right{width:300px;background:#111;border-left:1px solid #222;display:flex;flex-direction:column}.post-creator{background:#111;border:1px solid #222;border-radius:14px;padding:10px;margin-bottom:10px}.post-creator textarea{width:100%;background:#000;border:1px solid #333;border-radius:10px;color:#fff;padding:8px;min-height:50px;resize:none;font-size:13px}.tools{display:flex;gap:5px;margin-top:6px;flex-wrap:wrap}.tbtn{background:#1e1e1e;border:1px solid #333;padding:5px 9px;border-radius:20px;font-size:10px;cursor:pointer;color:#fff}.post{background:#111;border:1px solid #222;border-radius:14px;margin-bottom:10px;overflow:hidden}.phead{padding:8px;display:flex;gap:6px;align-items:center}.avatar{width:32px;height:32px;background:linear-gradient(45deg,#00d4ff,#7c3aed);border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:800;font-size:12px}.verif{background:#1d9bf0;width:12px;height:12px;border-radius:50%;display:inline-flex;align-items:center;justify-content:center;font-size:7px}.pbody{padding:0 8px 8px}.vwrap{position:relative;background:#000;border-radius:10px;overflow:hidden;margin-top:6px}.vwrap video{width:100%;max-height:360px;display:block}.vmeta{position:absolute;bottom:0;left:0;right:0;background:linear-gradient(transparent,rgba(0,0,0,.8));padding:8px;font-size:9px;display:flex;justify-content:space-between}.awrap{background:#000;border-radius:10px;padding:8px;margin-top:6px;border:1px solid #222}.volctrl{display:flex;gap:6px;align-items:center;margin-top:6px;background:#0f0f0f;padding:6px 8px;border-radius:8px}.volctrl input[type=range]{flex:1;accent-color:#00d4ff;height:4px}.icon{width:32px;height:32px;border-radius:50%;border:none;display:flex;align-items:center;justify-content:center;cursor:pointer;font-size:14px}.giftbar{display:flex;gap:4px;padding:6px;background:#0f0f0f;border-top:1px solid #222;overflow:auto}.gbtn{background:#1e1e1e;border:1px solid #333;padding:4px 7px;border-radius:20px;font-size:10px;cursor:pointer;white-space:nowrap}.adbadge{background:#ffcc00;color:#000;font-size:8px;padding:2px 5px;border-radius:8px;font-weight:900;margin-left:4px}.video-box{position:fixed;inset:0;background:#000;z-index:50;display:none;flex-direction:column}.video-box.on{display:flex}#remoteV{width:100%;height:100%;object-fit:cover}#localV{position:absolute;bottom:70px;right:10px;width:90px;height:120px;border-radius:8px;object-fit:cover;border:2px solid #fff}.list{flex:1;overflow:auto;padding:4px}.ucard{background:#1a1a1a;margin-bottom:4px;padding:6px;border-radius:10px;display:flex;justify-content:space-between;align-items:center;cursor:pointer;border:1px solid #222}.chatbox{display:flex;flex-direction:column;flex:1}.msgs{flex:1;overflow:auto;padding:8px;display:flex;flex-direction:column;gap:5px}.msg{max-width:78%;padding:6px 10px;border-radius:14px;font-size:12px}.msg.me{align-self:flex-end;background:#00d4ff;color:#000}.msg.other{align-self:flex-start;background:#1e1e1e}.msg.gift{align-self:center;background:linear-gradient(45deg,#7c3aed,#00d4ff)}.inputbar{padding:6px;background:#111;display:flex;gap:4px;align-items:center;border-top:1px solid #222}.inputbar input{flex:1;background:#1e1e1e;border:1px solid #333;padding:8px 12px;border-radius:20px;color:#fff;outline:none;font-size:12px}#login{position:fixed;inset:0;background:#000;z-index:100;display:flex;align-items:center;justify-content:center;padding:20px}.card{background:#111;padding:20px;border-radius:16px;width:100%;max-width:350px;border:1px solid #222}input[type=file]{display:none}</style></head><body>
<div id="login"><div class="card"><h2>KAR <span style="color:#00d4ff">FINAL</span></h2><p style="color:#888;font-size:10px;margin:6px 0 10px">Verificación ✅ Regalos 🎁 Voz 🎤 Cámara 📹 Video/Audio metadatos 📊 Volumen 🔊 Captura 📸 Anuncios 📢 Tokens 💰 Logros 🏆</p><input id="nameInput" placeholder="Tu nombre" style="width:100%;padding:10px;background:#000;border:1px solid #333;border-radius:8px;color:#fff;margin-bottom:6px"><label style="display:flex;gap:5px;font-size:10px;color:#aaa;margin-bottom:8px"><input type="checkbox" id="verifCheck"> Verificado 100 KAR</label><button onclick="iniciar()" style="width:100%;padding:10px;background:#00d4ff;border:none;border-radius:8px;font-weight:900">ENTRAR</button></div></div>
<div class="header"><div class="logo">KAR <span>FINAL DEFINITIVO</span></div><div style="display:flex;gap:5px"><div style="background:#1e1e1e;padding:4px 8px;border-radius:20px;font-size:10px">💰 <b id="tok">500</b></div><div style="background:#1e1e1e;padding:4px 8px;border-radius:20px;font-size:9px">ID: <b id="myId">...</b></div></div></div>
<div class="main">
<div class="left"><div style="padding:6px;display:flex;gap:3px"><input id="peerInput" placeholder="KAR-XXXX" style="flex:1;background:#000;border:1px solid #333;padding:6px;border-radius:6px;color:#fff;font-size:10px"><button onclick="conectar()" style="background:#00d4ff;border:none;padding:6px 8px;border-radius:6px;font-weight:700;font-size:10px">OK</button></div><div class="list" id="userList"></div><div style="padding:6px;border-top:1px solid #222"><div style="font-size:9px;color:#888">🏆 LOGROS</div><div id="logros" style="font-size:9px;color:#aaa;margin-top:3px"></div></div></div>
<div class="center"><div class="post-creator"><textarea id="postText" placeholder="Publica video/audio/anuncio..."></textarea><div class="tools"><label class="tbtn">📹 Video<input type="file" accept="video/*" onchange="handleVideo(this)"></label><label class="tbtn">🎵 Audio<input type="file" accept="audio/*" onchange="handleAudio(this)"></label><button class="tbtn" onclick="adBox.style.display=adBox.style.display==='none'?'block':'none'">📢 Anuncio</button><button class="tbtn" style="background:#00d4ff;color:#000;margin-left:auto" onclick="publicar()">Publicar</button></div><div id="prev"></div><div id="adBox" style="display:none;margin-top:8px;padding:8px;background:#000;border-radius:8px;border:1px solid #333"><input id="adTitle" placeholder="Título" style="width:100%;padding:6px;background:#111;border:1px solid #333;border-radius:6px;color:#fff;margin-bottom:4px;font-size:11px"><input id="adLink" placeholder="Link" style="width:100%;padding:6px;background:#111;border:1px solid #333;border-radius:6px;color:#fff;margin-bottom:4px;font-size:11px"><input id="adBudget" type="number" placeholder="20 KAR min" style="width:100%;padding:6px;background:#111;border:1px solid #333;border-radius:6px;color:#fff;font-size:11px"><label class="tbtn" style="margin-top:4px;display:inline-block">🖼️ Media<input type="file" accept="image/*,video/*" onchange="handleAd(this)"></label><div id="adPrev"></div></div></div><div id="feed"></div></div>
<div class="right"><div class="chatbox"><div style="padding:8px;background:#111;border-bottom:1px solid #222;display:flex;gap:6px;align-items:center"><div class="avatar" id="chatAv">K</div><div><div id="chatName" style="font-weight:700;font-size:12px">Chat</div><div style="font-size:9px;color:#0f0">● en línea</div></div><div style="margin-left:auto;display:flex;gap:3px"><button class="icon" style="background:#00ff88;font-size:12px" onclick="llamar(false)">📞</button><button class="icon" style="background:#7c3aed;color:#fff;font-size:12px" onclick="llamar(true)">📹</button></div></div><div class="msgs" id="msgs"><div style="text-align:center;color:#555;margin-top:15px;font-size:10px">Conecta</div></div><div class="giftbar"><button class="gbtn" onclick="gift('❤️',5)">❤️5</button><button class="gbtn" onclick="gift('🔥',10)">🔥10</button><button class="gbtn" onclick="gift('⭐',20)">⭐20</button><button class="gbtn" onclick="gift('💎',50)">💎50</button><button class="gbtn" onclick="gift('👑',100)">👑100</button><button class="gbtn" onclick="gift('🚀',200)">🚀200</button></div><div class="inputbar"><button class="icon" style="background:#222;font-size:12px" id="voiceBtn" onmousedown="vozOn()" onmouseup="vozOff()" ontouchstart="vozOn()" ontouchend="vozOff()">🎤</button><input id="msgInput" placeholder="Mensaje..." onkeypress="if(event.key==='Enter') enviar()"><button class="icon" style="background:#00d4ff;font-size:12px" onclick="enviar()">➤</button></div></div><div style="margin-top:6px;padding:6px;background:#000;border-radius:8px"><b style="font-size:10px">📊 Metadatos + Volumen</b><div id="meta" style="font-size:9px;color:#888;margin-top:3px">Sube video/audio</div><div id="volCapture" style="display:none;margin-top:6px"><div class="volctrl"><span style="font-size:9px">🔊</span><input type="range" min="0" max="1" step="0.05" value="1" oninput="setPreviewVol(this.value)"><span id="volTxt" style="font-size:9px">100%</span></div><div style="display:flex;gap:4px;margin-top:4px"><button class="tbtn" onclick="captureScreen()">📸 Captura</button><button class="tbtn" onclick="downloadCapture()">💾 Guardar</button></div><img id="captureImg" style="width:100%;border-radius:6px;margin-top:4px;display:none"></div></div></div>
</div>
<div class="video-box" id="vbox"><video id="remoteV" autoplay playsinline></video><video id="localV" autoplay playsinline muted></video><div style="padding:10px;background:#111;display:flex;justify-content:center;gap:8px"><span style="font-size:10px;color:#ffcc00;margin-right:auto">⏱️ <b id="ct">00:00</b> 💰 <b id="cc">0</b></span><div class="volctrl" style="margin:0"><span style="font-size:9px">🔊</span><input type="range" min="0" max="1" step="0.05" value="1" oninput="remoteV.volume=this.value"></div><button style="width:44px;height:44px;border-radius:50%;border:none;background:#ff3040;color:#fff" onclick="colgar()">✕</button></div></div>
<div id="pop" style="position:fixed;top:50px;right:8px;z-index:60;display:flex;flex-direction:column;gap:4px"></div>
<script>
let peer,localStream,currentCall,currentConn,currentChatId=null,miNombre='',verif=false,tokens=500,callSec=0,callCost=0,ci=null,previewEl=null,capturedData=null;
let amigos=JSON.parse(localStorage.getItem('kar_amigos')||'[]'),chats=JSON.parse(localStorage.getItem('kar_chats')||'{}'),posts=JSON.parse(localStorage.getItem('kar_posts')||'[]'),logrosDes=JSON.parse(localStorage.getItem('kar_logros')||'[]');
let pendingMedia=null,pendingAdMedia=null;
const LOGROS=[{id:'msg',n:'Palabra',i:'💬',t:10},{id:'gift',n:'Generoso',i:'🎁',t:15},{id:'call',n:'Hola',i:'📞',t:20},{id:'video',n:'Te Veo',i:'📹',t:30},{id:'verif',n:'Verificado',i:'✅',t:100},{id:'post',n:'Creador',i:'📝',t:20},{id:'ad',n:'Emprendedor',i:'📢',t:50},{id:'capture',n:'Fotógrafo',i:'📸',t:25}];
function iniciar(){miNombre=nameInput.value||'Jaime';verif=verifCheck.checked;let s=localStorage.getItem('kar_tokens');tokens=s?parseInt(s):500;if(verif){if(tokens<100)return alert('Necesitas 100');tokens-=100;}login.style.display='none';peer=new Peer('KAR-'+Math.random().toString(36).substr(2,6).toUpperCase());peer.on('open',id=>myId.innerText=id);peer.on('connection',c=>setup(c));peer.on('call',async call=>{if(confirm('Llamada '+call.peer+'?')){localStream=await navigator.mediaDevices.getUserMedia({audio:true,video:call.metadata.video});localV.srcObject=localStream;call.answer(localStream);setupCall(call);}});update();renderFeed();renderUsers();renderLogros();if(verif)unlock('verif');}
function update(){tok.innerText=tokens;localStorage.setItem('kar_tokens',tokens);}
function handleVideo(input){const f=input.files[0];if(!f)return;const url=URL.createObjectURL(f);const v=document.createElement('video');v.preload='metadata';v.src=url;v.onloadedmetadata=()=>{pendingMedia={type:'video',name:f.name,size:(f.size/1024/1024).toFixed(2)+' MB',duration:v.duration.toFixed(1)+'s',w:v.videoWidth,h:v.videoHeight,codec:f.type,url:url};prev.innerHTML=`<div class="vwrap"><video id="prevVideo" src="${url}" controls playsinline preload="metadata"></video><div class="vmeta"><span>📹 ${pendingMedia.w}x${pendingMedia.h} • ${pendingMedia.duration}</span><span>${pendingMedia.size}</span></div></div><div class="volctrl"><span style="font-size:9px">🔊 Volumen:</span><input type="range" min="0" max="1" step="0.05" value="1" oninput="document.getElementById('prevVideo').volume=this.value"><button class="tbtn" onclick="captureScreenPrev()">📸 Captura Pantalla</button><button class="tbtn" onclick="document.getElementById('prevVideo').requestFullscreen()">⛶ Full</button></div>`;previewEl=document.getElementById('prevVideo');meta.innerHTML=`<b>VIDEO:</b><br>${f.name}<br>${pendingMedia.size} • ${pendingMedia.duration}<br>${pendingMedia.w}x${pendingMedia.h} • ${pendingMedia.codec}<br>Volumen controlable 0-100%`;volCapture.style.display='block';};}
function handleAudio(input){const f=input.files[0];if(!f)return;const url=URL.createObjectURL(f);const a=new Audio();a.preload='metadata';a.src=url;a.onloadedmetadata=()=>{pendingMedia={type:'audio',name:f.name,size:(f.size/1024/1024).toFixed(2)+' MB',duration:a.duration.toFixed(1)+'s',codec:f.type,url:url};prev.innerHTML=`<div class="awrap"><div style="display:flex;justify-content:space-between;font-size:9px;color:#888"><span>🎵 ${f.name}</span><span>${pendingMedia.duration} • ${pendingMedia.size}</span></div><audio id="prevAudio" src="${url}" controls style="width:100%;margin-top:4px" preload="metadata"></audio><div class="volctrl"><span style="font-size:9px">🔊</span><input type="range" min="0" max="1" step="0.05" value="1" oninput="document.getElementById('prevAudio').volume=this.value"><span style="font-size:9px">Volumen</span></div></div>`;meta.innerHTML=`<b>AUDIO:</b><br>${f.name}<br>${pendingMedia.size} • ${pendingMedia.duration}<br>${pendingMedia.codec} • Volumen 0-100%`;}}
function handleAd(input){const f=input.files[0];const url=URL.createObjectURL(f);pendingAdMedia={url:url,type:f.type.startsWith('video')?'video':'image'};adPrev.innerHTML=pendingAdMedia.type==='video'?`<video src="${url}" controls style="width:100%;border-radius:6px;max-height:100px;margin-top:4px"></video>`:`<img src="${url}" style="width:100%;border-radius:6px;max-height:100px;margin-top:4px">`;}
function setPreviewVol(v){if(previewEl)previewEl.volume=v;document.getElementById('volTxt').innerText=Math.round(v*100)+'%';}
function captureScreenPrev(){const v=document.getElementById('prevVideo');if(!v)return;const c=document.createElement('canvas');c.width=v.videoWidth;c.height=v.videoHeight;c.getContext('2d').drawImage(v,0,0);capturedData=c.toDataURL('image/png');const img=document.getElementById('captureImg');img.src=capturedData;img.style.display='block';unlock('capture');}
function captureScreen(){captureScreenPrev();}
function downloadCapture(){if(!capturedData)return alert('Primero captura');const a=document.createElement('a');a.href=capturedData;a.download='kar-captura-'+Date.now()+'.png';a.click();}
function publicar(){const texto=postText.value;const isAd=adTitle.value.trim()!=='';if(!texto&&!pendingMedia&&!isAd)return alert('Agrega algo');if(isAd){const b=parseInt(adBudget.value||'0');if(b<20)return alert('Min 20');if(tokens<b)return alert('Sin tokens');tokens-=b;update();}const post={id:Date.now(),texto:texto,media:pendingMedia?{...pendingMedia,capture:capturedData}:null,ad:isAd?{title:adTitle.value,link:adLink.value,budget:adBudget.value,media:pendingAdMedia}:null,time:new Date().toLocaleString(),user:miNombre,verif:verif};posts.unshift(post);localStorage.setItem('kar_posts',JSON.stringify(posts));postText.value='';prev.innerHTML='';pendingMedia=null;adBox.style.display='none';adTitle.value='';adLink.value='';adBudget.value='';adPrev.innerHTML='';volCapture.style.display='none';capturedData=null;renderFeed();unlock('post');if(isAd)unlock('ad');}
function renderFeed(){const b=document.getElementById('feed');if(posts.length===0){b.innerHTML='<div style="text-align:center;color:#555;padding:15px;font-size:11px">Sin posts</div>';return;}b.innerHTML=posts.map((p,i)=>`<div class="post"><div class="phead"><div class="avatar">${p.user[0]}</div><div><div style="font-weight:700;font-size:12px">${p.user} ${p.verif?'<span class="verif">✓</span>':''} ${p.ad?'<span class="adbadge">ANUNCIO</span>':''}</div><div style="font-size:9px;color:#888">${p.time} ${p.media?`• ${p.media.type} ${p.media.duration} 🔊`:''}</div></div></div><div class="pbody">${p.texto?`<div style="font-size:12px">${p.texto}</div>`:''}${p.media?.type==='video'?`<div class="vwrap"><video id="vid${p.id}" src="${p.media.url}" controls playsinline preload="metadata" ${p.media.capture?`poster="${p.media.capture}"`:''}></video><div class="vmeta"><span>📹 ${p.media.w}x${p.media.h} • ${p.media.duration} • ${p.media.codec}</span><span>${p.media.size} 👁️1.2k</span></div></div><div class="volctrl"><button class="tbtn" onclick="document.getElementById('vid${p.id}').muted=!document.getElementById('vid${p.id}').muted">🔇</button><input type="range" min="0" max="1" step="0.05" value="1" oninput="document.getElementById('vid${p.id}').volume=this.value"><span style="font-size:9px">Volumen</span><button class="tbtn" onclick="capFeed('vid${p.id}')">📸</button><button class="tbtn" onclick="document.getElementById('vid${p.id}').requestFullscreen()">⛶</button></div>${p.media.capture?`<img src="${p.media.capture}" style="width:100%;border-radius:6px;margin-top:4px;border:1px solid #222">`:''}`:''}${p.media?.type==='audio'?`<div class="awrap"><div style="display:flex;justify-content:space-between;font-size:9px;color:#888"><span>🎵 ${p.media.name}</span><span>${p.media.duration} • ${p.media.size}</span></div><audio id="aud${p.id}" src="${p.media.url}" controls style="width:100%;margin-top:4px" preload="metadata"></audio><div class="volctrl"><button class="tbtn" onclick="document.getElementById('aud${p.id}').muted=!document.getElementById('aud${p.id}').muted">🔇</button><input type="range" min="0" max="1" step="0.05" value="1" oninput="document.getElementById('aud${p.id}').volume=this.value"><span style="font-size:9px">🔊 Volumen</span></div></div>`:''}${p.ad?`<div style="margin-top:6px;border-top:1px solid #222;padding-top:6px"><b style="font-size:11px">${p.ad.title}</b>${p.ad.media? (p.ad.media.type==='video'?`<video src="${p.ad.media.url}" controls style="width:100%;border-radius:6px;margin-top:4px;max-height:180px"></video><div class="volctrl"><input type="range" min="0" max="1" step="0.05" value="1" oninput="this.previousElementSibling.volume=this.value"><span style="font-size:9px">🔊</span></div>`:`<img src="${p.ad.media.url}" style="width:100%;border-radius:6px;margin-top:4px;max-height:180px">`):''}<button style="width:100%;margin-top:4px;background:#ffcc00;color:#000;border:none;padding:7px;border-radius:6px;font-weight:800;font-size:11px" onclick="window.open('${p.ad.link}','_blank')">Ver →</button></div>`:''}</div></div>`).join('');}
function capFeed(vidId){const v=document.getElementById(vidId);const c=document.createElement('canvas');c.width=v.videoWidth;c.height=v.videoHeight;c.getContext('2d').drawImage(v,0,0);const url=c.toDataURL();const w=window.open();w.document.write(`<img src="${url}" style="width:100%"><br><a href="${url}" download="captura.png">Descargar</a>`);}
function conectar(){const id=peerInput.value.trim();if(!id)return;setup(peer.connect(id,{metadata:{nombre:miNombre,verif:verif}}));}
function setup(conn){conn.on('open',()=>{currentConn=conn;currentChatId=conn.peer;if(!amigos.find(a=>a.id===conn.peer))amigos.push({id:conn.peer,nombre:conn.metadata.nombre||conn.peer,verif:conn.metadata.verif});chatName.innerText=conn.metadata.nombre||conn.peer;chatAv.innerText=(conn.metadata.nombre||'K')[0];renderUsers();save();conn.on('data',d=>{if(!chats[conn.peer])chats[conn.peer]=[];if(d.tipo==='msg')chats[conn.peer].push({me:false,texto:d.texto});if(d.tipo==='gift')chats[conn.peer].push({me:false,texto:`Te envió ${d.gift}`,gift:true});save();renderMsgs();});});}
function enviar(){const t=msgInput.value.trim();if(!t||!currentChatId)return;if(!chats[currentChatId])chats[currentChatId]=[];chats[currentChatId].push({me:true,texto:t});if(currentConn)currentConn.send({tipo:'msg',texto:t});msgInput.value='';save();renderMsgs();unlock('msg');}
function gift(icon,cost){if(tokens<cost)return alert('Sin KAR');if(!currentChatId)return alert('Conecta');tokens-=cost;update();if(!chats[currentChatId])chats[currentChatId]=[];chats[currentChatId].push({me:true,texto:`Enviaste ${icon}`,gift:true});if(currentConn)currentConn.send({tipo:'gift',gift:icon});save();renderMsgs();unlock('gift');}
function renderMsgs(){const b=msgs;if(!currentChatId||!chats[currentChatId])return;b.innerHTML=chats[currentChatId].map(m=>m.gift?`<div class="msg gift">${m.texto}</div>`:`<div class="msg ${m.me?'me':'other'}">${m.texto}</div>`).join('');b.scrollTop=b.scrollHeight;}
async function llamar(v){if(!currentChatId)return alert('Conecta');if(tokens<2)return alert('2 KAR');localStream=await navigator.mediaDevices.getUserMedia({audio:true,video:v});localV.srcObject=localStream;vbox.classList.add('on');const call=peer.call(currentChatId,localStream,{metadata:{video:v}});setupCall(call);}
function setupCall(call){currentCall=call;callSec=0;callCost=0;vbox.classList.add('on');ci=setInterval(()=>{callSec++;callCost=Math.floor(callSec/60)*2+2;ct.innerText=new Date(callSec*1000).toISOString().substr(14,5);cc.innerText=callCost;if(callCost>tokens){alert('Sin tokens');colgar();}},1000);call.on('stream',s=>remoteV.srcObject=s);call.on('close',colgar);if(call.metadata.video)unlock('video');else unlock('call');}
function colgar(){clearInterval(ci);if(tokens>=callCost)tokens-=callCost,update();if(currentCall)currentCall.close();if(localStream)localStream.getTracks().forEach(t=>t.stop());vbox.classList.remove('on');}
let rec=null;function vozOn(){voiceBtn.style.background='#ff3040';const SR=window.SpeechRecognition||window.webkitSpeechRecognition;if(!SR)return alert('Usa Chrome');rec=new SR();rec.lang='es-MX';rec.onresult=e=>msgInput.value=e.results[0][0].transcript;rec.start();}function vozOff(){voiceBtn.style.background='#222';if(rec)rec.stop();}
function unlock(id){if(logrosDes.includes(id))return;const l=LOGROS.find(x=>x.id===id);if(!l)return;logrosDes.push(id);localStorage.setItem('kar_logros',JSON.stringify(logrosDes));tokens+=l.t;update();const d=document.createElement('div');d.style.cssText='background:#111;border:1px solid #00d4ff;padding:6px 10px;border-radius:8px;font-size:10px';d.innerHTML=`🏆 ${l.i} ${l.n} +${l.t}`;pop.appendChild(d);setTimeout(()=>d.remove(),3000);renderLogros();}
function renderLogros(){logros.innerHTML=LOGROS.map(l=>`${logrosDes.includes(l.id)?'✅':'⬜'} ${l.i} ${l.n}`).join('<br>');}
function renderUsers(){userList.innerHTML=amigos.length?amigos.map(a=>`<div class="ucard" onclick="currentChatId='${a.id}';chatName.innerText='${a.nombre}';chatAv.innerText='${a.nombre[0]}';currentConn=peer.connections['${a.id}']?.[0];renderMsgs()"><div style="display:flex;gap:5px;align-items:center"><div class="avatar">${a.nombre[0]}</div><div><b style="font-size:11px">${a.nombre}</b><br><span style="font-size:8px;color:#888">${a.id}</span></div></div><span style="color:#0f0;font-size:8px">●</span></div>`).join(''):'<div style="color:#555;text-align:center;padding:8px;font-size:10px">Pega ID</div>';}
function save(){localStorage.setItem('kar_amigos',JSON.stringify(amigos));localStorage.setItem('kar_chats',JSON.stringify(chats));}
</script><script>(function(){var loc=location.href.replace(/#.*$/,"");var ATTR_NAMES=["data-product-id","data-productid","data-product_id","product-id","productid","product_id","data-source-entity-id","source-entity-id","source_entity_id","data-product","data-metadata","data-meta"];var DATASET_KEYS=["productId","productid","product_id","sourceEntityId","sourceentityid","source_entity_id","product","metadata","meta"];function readProductId(value){if(typeof value!=="string"||value.length===0)return null;if(/^[0-9]{6,}$/.test(value))return value;var match=value.match(/(?:product(?:_|-)?id|source(?:_|-)?entity(?:_|-)?id)["'=:\s]+([0-9]{6,})/i);return match?match[1]:null}function extractProductId(start){for(var node=start;node&&node!==document.body;node=node.parentElement){for(var i=0;i<ATTR_NAMES.length;i++){var attrValue=node.getAttribute&&node.getAttribute(ATTR_NAMES[i]);var attrProductId=readProductId(attrValue);if(attrProductId)return attrProductId}var dataset=node.dataset||null;if(dataset){for(var j=0;j<DATASET_KEYS.length;j++){var dataValue=dataset[DATASET_KEYS[j]];var dataProductId=readProductId(dataValue);if(dataProductId)return dataProductId}}}return null}function isInlineMediaSlotElement(node){return !!(node&&node.getAttribute&&node.getAttribute("data-clippy-inline-media-slot")!==null)}function findInlineMediaSlot(start){for(var node=start;node&&node!==document.body;node=node.parentElement){if(isInlineMediaSlotElement(node))return node}return null}function readInlineMediaUrl(node){if(!node)return null;return node.getAttribute&&((node.getAttribute("data-clippy-inline-media-url")||node.getAttribute("data-url")||node.getAttribute("data_url")))||node.href||null}function stripHash(url){return String(url).replace(/#.*$/,"")}function urlsMatch(a,b){if(!a||!b)return false;try{return stripHash(new URL(a,loc).href)===stripHash(new URL(b,loc).href)}catch(_){return stripHash(a)===stripHash(b)}}function isFirstPartyReelUrl(value){try{var url=new URL(value,loc);if(url.protocol!=="https:")return false;var host=url.hostname.toLowerCase();var supported=host==="instagram.com"||host.endsWith(".instagram.com")||host==="facebook.com"||host.endsWith(".facebook.com");return supported&&/\/reels?\//i.test(url.pathname)}catch(_){return false}}function isInlineMediaUrlClick(node,href){var slot=findInlineMediaSlot(node);if(!slot)return false;var slotUrl=readInlineMediaUrl(slot);if(slotUrl)return urlsMatch(href,slotUrl);return isFirstPartyReelUrl(href)}function findDataHref(start){for(var node=start;node&&node!==document.body;node=node.parentElement){if(node.getAttribute){var href=node.getAttribute("data-href")||node.getAttribute("data-url");if(href)return{href:href,node:node}}}return null}var nativeOpen=window.open;window.open=function(url){if(parent!==window&&typeof url==="string"&&/^https?:\/\//.test(url)){parent.postMessage({type:"ecto:usercontent-link-click",href:url},"*");return null}return nativeOpen?nativeOpen.apply(window,arguments):null};document.addEventListener("click",function(e){var target=e.target instanceof Element?e.target:null;if(!target)return;if(parent===window)return;var a=target.closest?target.closest("a[href]"):null;if(a&&a.href&&/^https?:\/\//.test(a.href)&&a.href.replace(/#.*$/,"")!==loc){if(isInlineMediaUrlClick(a,a.href))return;var productId=extractProductId(target)||extractProductId(a);if(productId){e.preventDefault();parent.postMessage({type:"ecto-artifact-link-click",productId:productId},"*");return}e.preventDefault();parent.postMessage({type:"ecto:usercontent-link-click",href:a.href},"*");return}var dataHref=findDataHref(target);if(dataHref&&/^https?:\/\//.test(dataHref.href)&&dataHref.href.replace(/#.*$/,"")!==loc){if(isInlineMediaUrlClick(dataHref.node,dataHref.href))return;e.preventDefault();parent.postMessage({type:"ecto:usercontent-link-click",href:dataHref.href},"*")}},true)})();</script><script>(function(){var FOCUS_TYPE="ecto:artifact-focus-request";var CLOSE_TYPE="ecto:artifact-close-request";function focusArtifactDocument(){var body=document.body;if(!body)return;try{window.focus();}catch(e){}if(!body.hasAttribute("tabindex"))body.setAttribute("tabindex","-1");try{body.focus({preventScroll:true});}catch(e){try{body.focus();}catch(e2){}}}window.addEventListener("message",function(event){if(event.source!==window.parent)return;var data=event.data;if(!data||typeof data!=="object"||data.type!==FOCUS_TYPE)return;if(document.readyState==="loading"){document.addEventListener("DOMContentLoaded",focusArtifactDocument,{once:true});return;}focusArtifactDocument();});window.addEventListener("keydown",function(event){if(event.key!=="Escape")return;window.setTimeout(function(){if(event.defaultPrevented)return;window.parent.postMessage({type:CLOSE_TYPE},"*");},0);});})();</script></body></html>
