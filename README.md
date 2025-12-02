/*
Netflix-style Starter (single-file React component)
- File: NetflixClone.jsx
- Type: React component (default export)
- Styling: Tailwind CSS (assumes Tailwind is configured in your project)
- Notes: This is a frontend-only mock. Replace mockData with your API endpoints.

How to use:
1. Create a new React app (Vite or CRA). Example with Vite:
   npm create vite@latest my-netflix -- --template react
   cd my-netflix
   npm install
2. Install Tailwind and follow Tailwind setup for your project.
3. Add this file to src/ (src/NetflixClone.jsx) and import it in App.jsx:
   import NetflixClone from './NetflixClone';
   export default function App(){ return <NetflixClone/> }
4. npm run dev / npm start

Features included:
- Responsive navbar with search
- Hero banner
- Horizontal scrollable rows of movie cards
- Hover effects and simple modal (lightbox)
- Mock user avatar and sign-in button (UI only)

Limitations / next steps:
- No real streaming (use a secure streaming backend / CDN)
- Add authentication (Auth0/Firebase) and payments (Stripe)
- Build an API for movie metadata (title, duration, video URL, subtitles)
- Add DRM/widevine if you plan production streaming
*/

import React, {useState} from 'react';

const mockMovies = [
  {id:1, title:'Midnight Train', year:2023, img:'https://images.unsplash.com/photo-1542204165-3c4c5b5f0b05?w=800&q=60', desc:'A moody thriller about choices.'},
  {id:2, title:'Sunrise Riders', year:2021, img:'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=800&q=60', desc:'Road trip drama.'},
  {id:3, title:'Neon Nights', year:2024, img:'https://images.unsplash.com/photo-1515879218367-8466d910aaa4?w=800&q=60', desc:'Cyber-romance in the city.'},
  {id:4, title:'The Last Letter', year:2019, img:'https://images.unsplash.com/photo-1524985069026-dd778a71c7b4?w=800&q=60', desc:'Heartfelt family story.'},
  {id:5, title:'Deep Blue', year:2022, img:'https://images.unsplash.com/photo-1505765051609-2e6d4a3b8c1b?w=800&q=60', desc:'Ocean exploration doc.'},
  {id:6, title:'Starlit', year:2020, img:'https://images.unsplash.com/photo-1451187580459-43490279c0fa?w=800&q=60', desc:'Coming-of-age tale.'}
];

function Navbar({onSearch}){
  const [q, setQ] = useState('');
  return (
    <nav className="flex items-center justify-between px-6 py-4 bg-gradient-to-r from-black/80 via-black/60 to-transparent fixed w-full z-40">
      <div className="flex items-center gap-6">
        <div className="text-red-600 font-bold text-2xl">STREAMFLIX</div>
        <ul className="hidden md:flex gap-4 text-sm text-gray-200">
          <li className="hover:underline cursor-pointer">Home</li>
          <li className="hover:underline cursor-pointer">TV Shows</li>
          <li className="hover:underline cursor-pointer">Movies</li>
          <li className="hover:underline cursor-pointer">My List</li>
        </ul>
      </div>
      <div className="flex items-center gap-4">
        <input value={q} onChange={e=>{setQ(e.target.value); onSearch && onSearch(e.target.value)}} placeholder="Search" className="hidden sm:block bg-black/60 placeholder-gray-400 text-white px-3 py-1 rounded" />
        <button className="text-white bg-red-600 px-3 py-1 rounded">Sign In</button>
        <div className="w-9 h-9 rounded-full bg-gray-600" title="avatar"></div>
      </div>
    </nav>
  );
}

function Hero({movie, onPlay}){
  if(!movie) return null;
  return (
    <header className="relative h-[55vh] md:h-[70vh] bg-black text-white flex items-end" style={{backgroundImage:`linear-gradient(180deg, rgba(0,0,0,0.2), rgba(0,0,0,0.9)), url(${movie.img})`, backgroundSize:'cover', backgroundPosition:'center'}}>
      <div className="p-8 max-w-3xl">
        <h1 className="text-3xl md:text-5xl font-bold">{movie.title}</h1>
        <p className="mt-3 text-sm md:text-base text-gray-200">{movie.desc}</p>
        <div className="mt-6 flex gap-3">
          <button onClick={()=>onPlay(movie)} className="bg-white text-black px-4 py-2 rounded font-semibold">Play</button>
          <button className="bg-gray-700/60 text-white px-3 py-2 rounded">My List</button>
        </div>
      </div>
    </header>
  );
}

function MovieCard({m, onOpen}){
  return (
    <div className="group relative w-44 flex-shrink-0 mr-4 cursor-pointer" onClick={()=>onOpen(m)}>
      <img src={m.img} alt={m.title} className="w-44 h-28 object-cover rounded-md shadow-lg group-hover:scale-105 transition-transform" />
      <div className="mt-2 text-sm text-gray-200">{m.title}</div>
      <div className="text-xs text-gray-400">{m.year}</div>
      <div className="absolute inset-0 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity">
        <div className="bg-black/60 p-2 rounded">▶</div>
      </div>
    </div>
  );
}

function Row({title, movies, onOpen}){
  return (
    <section className="px-6 py-4">
      <h3 className="text-white font-semibold mb-2">{title}</h3>
      <div className="flex overflow-x-auto no-scrollbar py-2">
        {movies.map(m => <MovieCard key={m.id} m={m} onOpen={onOpen} />)}
      </div>
    </section>
  );
}

function Modal({movie, onClose}){
  if(!movie) return null;
  return (
    <div className="fixed inset-0 bg-black/70 flex items-center justify-center z-50" onClick={onClose}>
      <div className="bg-gray-900 rounded p-6 max-w-2xl w-full" onClick={e=>e.stopPropagation()}>
        <div className="flex justify-between items-start">
          <h2 className="text-2xl font-bold text-white">{movie.title} <span className="text-sm text-gray-400">({movie.year})</span></h2>
          <button onClick={onClose} className="text-gray-300">✕</button>
        </div>
        <div className="mt-4">
          <img src={movie.img} alt={movie.title} className="w-full h-52 object-cover rounded" />
          <p className="text-gray-300 mt-3">{movie.desc}</p>
          <div className="mt-4 flex gap-3">
            <button className="bg-white text-black px-4 py-2 rounded">Play Trailer</button>
            <button className="bg-gray-700/60 text-white px-3 py-2 rounded">Add to List</button>
          </div>
        </div>
      </div>
    </div>
  );
}

export default function NetflixClone(){
  const [selected, setSelected] = useState(mockMovies[0]);
  const [query, setQuery] = useState('');
  const [open, setOpen] = useState(null);

  const rows = [
    {title:'Trending Now', movies: mockMovies},
    {title:'New Releases', movies: mockMovies.slice().reverse()},
    {title:'Because you watched Midnight Train', movies: mockMovies}
  ];

  const filteredRows = rows.map(r => ({...r, movies: r.movies.filter(m => m.title.toLowerCase().includes(query.toLowerCase()))}));

  return (
    <div className="min-h-screen bg-gradient-to-b from-gray-900 to-black text-white pb-20">
      <Navbar onSearch={setQuery} />
      <main className="pt-20">
        <Hero movie={selected} onPlay={(m)=>{alert('Play '+m.title+' — add player integration')}} />

        <div className="mt-6">
          {filteredRows.map((r, i)=>(
            <Row key={i} title={r.title} movies={r.movies} onOpen={(m)=>setOpen(m)} />
          ))}
        </div>

        <footer className="text-gray-400 text-sm px-6 mt-12 pb-12">© Streamflix - demo UI only. Not for production use without proper licensing.</footer>
      </main>

      <Modal movie={open} onClose={()=>setOpen(null)} />
    </div>
  );
}
