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
            <p style={{ margin: 0, color: '#65676b' }}>{likePagina? '2.5K Me gusta' : '2.4K Me gusta'} • 5K seguidores</p>
