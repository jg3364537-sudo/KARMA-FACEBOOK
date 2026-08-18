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
