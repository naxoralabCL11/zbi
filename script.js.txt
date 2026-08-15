/* ---------------- custom cursor ---------------- */
  const cursor = document.getElementById('cursor');
  let cx = window.innerWidth/2, cy = window.innerHeight/2;
  window.addEventListener('mousemove', e=>{
    cx = e.clientX; cy = e.clientY;
    cursor.style.left = cx+'px';
    cursor.style.top = cy+'px';
  });
  document.querySelectorAll('button, .girl-card, .node, a').forEach(el=>{
    el.addEventListener('mouseenter', ()=>cursor.classList.add('grow'));
    el.addEventListener('mouseleave', ()=>cursor.classList.remove('grow'));
  });

  /* ---------------- fullscreen API ---------------- */
  const fsBtn = document.getElementById('fs-toggle');
  const fsIcon = document.getElementById('fs-icon');
  const expandPath = 'M8 3H5a2 2 0 0 0-2 2v3M21 8V5a2 2 0 0 0-2-2h-3M3 16v3a2 2 0 0 0 2 2h3M16 21h3a2 2 0 0 0 2-2v-3';
  const collapsePath = 'M9 3v4a2 2 0 0 1-2 2H3M15 3v4a2 2 0 0 1 2 2h4M15 21v-4a2 2 0 0 1 2-2h4M9 21v-4a2 2 0 0 1-2-2H3';

  fsBtn.addEventListener('click', ()=>{
    if(!document.fullscreenElement){
      document.documentElement.requestFullscreen().catch(()=>{});
    } else {
      document.exitFullscreen();
    }
  });
  document.addEventListener('fullscreenchange', ()=>{
    fsIcon.querySelector('path').setAttribute('d', document.fullscreenElement ? collapsePath : expandPath);
  });
  document.getElementById('enter-btn').addEventListener('click', ()=>{
    document.documentElement.requestFullscreen?.().catch(()=>{});
    document.getElementById('gallery').scrollIntoView({behavior:'smooth'});
  });
  document.getElementById('explore-btn').addEventListener('click', ()=>{
    document.getElementById('map').scrollIntoView({behavior:'smooth'});
  });

  /* ---------------- nav dots + scroll sync ---------------- */
  const frame = document.getElementById('frame');
  const sections = [...document.querySelectorAll('section')];
  const navBtns = [...document.querySelectorAll('#nav button')];
  navBtns.forEach(b=> b.addEventListener('click', ()=>{
    document.getElementById(b.dataset.target).scrollIntoView({behavior:'smooth'});
  }));
  const io = new IntersectionObserver((entries)=>{
    entries.forEach(entry=>{
      if(entry.isIntersecting){
        navBtns.forEach(b=>b.classList.toggle('active', b.dataset.target===entry.target.id));
      }
    });
  }, {threshold:0.6, root:frame});
  sections.forEach(s=>io.observe(s));

  /* ---------------- gallery data ---------------- */
  const residents = [
    {name:'Nova Vex', role:'Skyward Pilot', hue:'linear-gradient(160deg,#ff2ea6,#a855f7)'},
    {name:'Iris Kade', role:'Storm Cartographer', hue:'linear-gradient(160deg,#22e5ff,#a855f7)'},
    {name:'Selene Rho', role:'Crystal Smith', hue:'linear-gradient(160deg,#ff2ea6,#22e5ff)'},
    {name:'Zara Wynn', role:'Voltage Keeper', hue:'linear-gradient(160deg,#a855f7,#ff2ea6)'},
    {name:'Lumi Sato', role:'Archive Warden', hue:'linear-gradient(160deg,#22e5ff,#ff2ea6)'},
    {name:'Kira Onyx', role:'Bazaar Herald', hue:'linear-gradient(160deg,#a855f7,#22e5ff)'}
  ];
  const grid = document.getElementById('gallery-grid');
  residents.forEach(r=>{
    const initials = r.name.split(' ').map(w=>w[0]).join('');
    const card = document.createElement('div');
    card.className = 'girl-card glass';
    card.innerHTML = `
      <div class="avatar" style="background:${r.hue}">${initials}</div>
      <h3>${r.name}</h3>
      <span>${r.role}</span>
    `;
    grid.appendChild(card);
  });

  /* ---------------- planet map ---------------- */
  const districts = [
    {name:'Neon Bazaar', desc:'Where every trade is lit like a festival.', x:30, y:35},
    {name:'Thunder Fields', desc:'Open plains that hum before every storm.', x:68, y:28},
    {name:'Crystal Spire', desc:'The tallest structure, grown not built.', x:50, y:55},
    {name:'The Quiet Coast', desc:'The only place on Barq Barq that whispers.', x:25, y:72},
    {name:'Voltage Archive', desc:'Every citizen\\'s story, stored in light.', x:75, y:68}
  ];
  const mapWrap = document.getElementById('map-wrap');
  districts.forEach((d,i)=>{
    const node = document.createElement('button');
    node.className = 'node';
    node.style.left = d.x+'%';
    node.style.top = d.y+'%';
    node.setAttribute('aria-label', d.name);

    const label = document.createElement('div');
    label.className = 'node-label glass';
    label.style.left = d.x+'%';
    label.style.top = d.y+'%';
    label.innerHTML = `<strong>${d.name}</strong>${d.desc}`;

    node.addEventListener('click', ()=>{
      const wasActive = node.classList.contains('active');
      mapWrap.querySelectorAll('.node').forEach(n=>n.classList.remove('active'));
      if(!wasActive) node.classList.add('active');
    });

    mapWrap.appendChild(node);
    mapWrap.appendChild(label);
  });
