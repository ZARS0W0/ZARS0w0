<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Interactive Cloud Types — Identify Clouds</title>
  <meta name="description" content="Interactive visual guide to cloud types and what they mean. Click clouds, filter by altitude, take a tiny quiz, and copy README badges for your GitHub." />
  <style>
    :root{
      --bg:#0f1724; --card:#0b1220; --glass:rgba(255,255,255,0.04);
      --accent:#7dd3fc; --muted:#94a3b8; --glass-2:rgba(255,255,255,0.03);
      --card-radius:14px;
      color-scheme: dark;
    }
    html,body{height:100%;margin:0;font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial; background:linear-gradient(180deg,#071226 0%, #071826 50%, #072033 100%);color:#e6eef6}
    .wrap{max-width:1100px;margin:28px auto;padding:28px}
    header{display:flex;gap:16px;align-items:center;justify-content:space-between}
    h1{font-size:1.5rem;margin:0}
    p.lead{margin:4px 0 0;color:var(--muted)}
    .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:16px;margin-top:22px}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:var(--card-radius);padding:14px;backdrop-filter:blur(6px);box-shadow:0 6px 20px rgba(0,0,0,0.35);cursor:pointer;transition:transform .18s ease,box-shadow .18s ease}
    .card:hover{transform:translateY(-6px);box-shadow:0 12px 36px rgba(0,0,0,0.5)}
    .card.small{padding:10px}
    .meta{display:flex;align-items:center;gap:10px}
    .badge{background:var(--glass);padding:6px 10px;border-radius:999px;font-size:.8rem;color:var(--muted)}
    .sky{height:220px;border-radius:12px;margin-top:12px;display:flex;align-items:center;justify-content:center;position:relative;overflow:hidden;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));}
    svg.cloud{width:100%;height:120px}
    .details{margin-top:16px;padding:14px;background:linear-gradient(180deg, rgba(255,255,255,0.015), transparent);border-radius:10px}
    .muted{color:var(--muted)}
    .controls{display:flex;gap:8px;flex-wrap:wrap;margin-top:12px}
    .pill{padding:8px 12px;border-radius:999px;background:var(--glass);cursor:pointer;font-weight:600}
    .pill.active{background:linear-gradient(90deg,var(--accent),#60a5fa);color:#04111a}
    .panel{display:grid;grid-template-columns:1fr 360px;gap:16px;margin-top:18px}
    .info{padding:16px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:12px}
    .right{padding:16px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:12px}
    .skill-row{display:flex;gap:10px;align-items:center}
    .tiny{font-size:.85rem}
    .quiz{display:flex;flex-direction:column;gap:8px}
    button.btn{background:var(--accent);border:none;padding:10px 12px;border-radius:10px;font-weight:700;cursor:pointer}
    input[type=range]{width:100%}
    footer{margin-top:22px;color:var(--muted);font-size:.9rem}
    .copy{background:transparent;border:1px dashed var(--glass);padding:10px;border-radius:10px;cursor:pointer}
    @media (max-width:900px){.panel{grid-template-columns:1fr}}

    /* small illustrative cloud SVG styles */
    .cloud-svg {filter:drop-shadow(0 8px 18px rgba(2,6,23,0.6))}
    .cloud-anim{animation:float 6s ease-in-out infinite}
    @keyframes float{0%{transform:translateY(0)}50%{transform:translateY(-8px)}100%{transform:translateY(0)}}

    /* colored altitude labels */
    .alt-high{background:linear-gradient(90deg,#c7f9fb44,#7dd3fc33)}
    .alt-mid{background:linear-gradient(90deg,#fef3c744,#fbbf24aa)}
    .alt-low{background:linear-gradient(90deg,#d1fae544,#34d39966)}
    .alt-vert{background:linear-gradient(90deg,#fda4af66,#fb718566)}

  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div>
        <h1>☁️ Interactive Cloud Guide</h1>
        <p class="lead">Learn cloud types, filter by altitude, see animated examples, and copy a ready-to-add README badge for your GitHub profile.</p>
      </div>
      <div style="text-align:right">
        <div class="badge">Made for ZARS0W0 • Belgium</div>
        <div style="height:6px"></div>
        <div class="muted tiny">Tip: save this file to <code>docs/clouds.html</code> and enable GitHub Pages to host it.</div>
      </div>
    </header>

    <section class="controls">
      <div class="pill" data-filter="all">All</div>
      <div class="pill" data-filter="high">High</div>
      <div class="pill" data-filter="mid">Middle</div>
      <div class="pill" data-filter="low">Low</div>
      <div class="pill" data-filter="vertical">Vertical</div>
      <div style="flex:1"></div>
      <div class="pill" id="quiz-toggle">Take tiny quiz</div>
      <div class="pill" id="copy-badge">Copy README badge</div>
    </section>

    <div class="grid" id="cards">
      <!-- Cards will be injected by JS -->
    </div>

    <div class="panel" id="panel">
      <div class="info" id="info">
        <h3 id="info-title">Click a cloud to see details</h3>
        <p class="muted tiny" id="info-sub">Altitude, formation, how to spot it, and what it usually means for the weather.</p>
        <div id="info-body" style="margin-top:12px">Select a card on the left.</div>
      </div>
      <aside class="right">
        <h4 style="margin:0">Interactive tools</h4>
        <div style="height:12px"></div>
        <div class="muted tiny">Altitude slider — simulate where clouds form</div>
        <input id="altitude" type="range" min="0" max="4" value="0" />
        <div style="height:8px"></div>
        <div id="altitude-label" class="badge">Altitude: Any</div>

        <div style="height:12px"></div>
        <div class="muted tiny">Quick ID helper</div>
        <div style="height:8px"></div>
        <div class="skill-row">
          <select id="appearance" style="flex:1;padding:8px;border-radius:8px;background:transparent;border:1px solid var(--glass)">
            <option value="puffy">Puffy, cotton-like (isolated)</option>
            <option value="tower">Towering, vertical</option>
            <option value="sheet">Thick sheet/dim sun</option>
            <option value="wispy">Thin, wispy</option>
            <option value="lumpy">Lumpy rolls/patches</option>
            <option value="blanket">Blanket/low gray layer</option>
          </select>
          <button class="btn" id="identify">Identify</button>
        </div>

        <div style="height:12px"></div>
        <div class="muted tiny">Quick quiz (3 questions)</div>
        <div id="quiz" style="display:none;margin-top:8px" class="quiz"></div>

        <div style="height:12px"></div>
        <div class="muted tiny">Badge snippet for README</div>
        <pre id="badge-snippet" style="background:var(--glass);padding:8px;border-radius:8px;overflow:auto;font-size:.9rem;margin-top:8px;"><!-- click "Copy README badge" -->
</pre>

      </aside>
    </div>

    <footer>
      <div>Built with ❤️ for <strong>ZARS0W0</strong>. Drop it into your repo's <code>docs/</code> folder and enable GitHub Pages (branch: main / folder: /docs) or host on any static host.</div>
    </footer>
  </div>

  <script>
    /* CLOUD DATA */
    const CLOUDS = [
      {id:'ci', name:'Cirrus', group:'High', altitude:'>6 km', key:'high', desc:'Thin, wispy strands of ice crystals. Often indicate fair weather but can precede a warm front when they thicken.', formation:['Ice crystals','Stable high winds','Thin sheets or filaments'], meaning:'Fair weather now; change likely in 12–36h if they thicken.'},
      {id:'cs', name:'Cirrostratus', group:'High', altitude:'>6 km', key:'high', desc:'Transparent, veil-like layer that can produce halos around sun or moon. Signals an approaching warm front and likely precipitation within 12–24 hours.', formation:['Widespread ice-crystal sheet','Thin enough for halos'], meaning:'Often a sign of an approaching frontal system; precipitation possible within a day.'},
      {id:'cc', name:'Cirrocumulus', group:'High', altitude:'>6 km', key:'high', desc:'Small, white patches resembling fish scales — a "mackerel sky". Fair but cold aloft; may indicate instability at that level.', formation:['Tiny ice crystal or supercooled droplets','Small ripples or patches'], meaning:'Usually fair; may suggest changes aloft or instability.'},

      {id:'as', name:'Altostratus', group:'Middle', altitude:'2–6 km', key:'mid', desc:'Gray-blue sheet covering the sky, sun appears as a dim disk. Often precedes steady rain or snow.', formation:['Layered water droplets or ice','Thickening mid-level cloud'], meaning:'Precursor to widespread precipitation.'},
      {id:'ac', name:'Altocumulus', group:'Middle', altitude:'2–6 km', key:'mid', desc:'White or gray patches/rolls. If seen on warm humid morning, might mean thunderstorms later in the day.', formation:['Localized convection at mid-levels','Rolls, patches, waves'], meaning:'Fair to unsettled weather; watch for afternoon storms if humid.'},

      {id:'st', name:'Stratus', group:'Low', altitude:'<2 km', key:'low', desc:'Uniform gray layer like fog but not reaching ground. Causes overcast skies, drizzle or mist.', formation:['Shallow layer mixing','Low-level cooling or moist air'], meaning:'Overcast with light precipitation or drizzle.'},
      {id:'sc', name:'Stratocumulus', group:'Low', altitude:'<2 km', key:'low', desc:'Low, lumpy cloud with breaks of blue sky. Usually stable, may produce light rain.', formation:['Broken low sheet, lumps or rolls'], meaning:'Generally stable; light rain possible.'},
      {id:'ns', name:'Nimbostratus', group:'Low', altitude:'<2 km', key:'low', desc:'Thick, dark, featureless layer that produces steady precipitation.', formation:['Very thick stratus layer','Extensive water content'], meaning:'Steady, sometimes heavy precipitation for long duration.'},

      {id:'cu', name:'Cumulus', group:'Vertical', altitude:'Surface to 6 km', key:'vertical', desc:'Puffy, cotton-like clouds — fair-weather when small. When they grow taller they indicate stronger convection.', formation:['Convective updrafts','Moist rising air'], meaning:'Small: fair weather. Growing: potential showers.'},
      {id:'cb', name:'Cumulonimbus', group:'Vertical', altitude:'Surface to >10 km', key:'vertical', desc:'Towering thunderstorm cloud with anvil top. Produces heavy rain, lightning, hail, and sometimes tornadoes.', formation:['Strong convection','Very unstable atmosphere'], meaning:'Severe weather: thunderstorms, lightning, hail, gusts, tornado risk.'}
    ];

    const cardsEl = document.getElementById('cards');
    const infoTitle = document.getElementById('info-title');
    const infoBody = document.getElementById('info-body');
    const infoSub = document.getElementById('info-sub');

    function makeCloudSVG(kind){
      // small illustrative SVG for each kind
      const templates = {
        ci:`<svg viewBox="0 0 200 80" class="cloud-svg cloud-anim" xmlns="http://www.w3.org/2000/svg"><g fill="none" stroke="rgba(255,255,255,0.6)" stroke-width="2"><path d="M10 30 q30 -20 60 0 q30 20 60 0" /></g></svg>`,
        cs:`<svg viewBox="0 0 200 80" class="cloud-svg" xmlns="http://www.w3.org/2000/svg"><rect x="0" y="10" width="200" height="40" rx="8" fill="rgba(255,255,255,0.03)" stroke="rgba(255,255,255,0.08)"/></svg>`,
        cc:`<svg viewBox="0 0 200 80" class="cloud-svg" xmlns="http://www.w3.org/2000/svg"><g fill="rgba(255,255,255,0.06)" stroke="rgba(255,255,255,0.08)"><circle cx="40" cy="35" r="10"/><circle cx="70" cy="30" r="8"/><circle cx="95" cy="36" r="9"/><circle cx="125" cy="32" r="7"/></g></svg>`,
        as:`<svg viewBox="0 0 200 80" class="cloud-svg" xmlns="http://www.w3.org/2000/svg"><rect x="0" y="18" width="200" height="28" rx="6" fill="rgba(255,255,255,0.02)" stroke="rgba(255,255,255,0.05)"/></svg>`,
        ac:`<svg viewBox="0 0 200 80" class="cloud-svg" xmlns="http://www.w3.org/2000/svg"><g fill="rgba(255,255,255,0.06)"><ellipse cx="40" cy="40" rx="22" ry="9"/><ellipse cx="85" cy="34" rx="18" ry="7"/><ellipse cx="125" cy="40" rx="16" ry="8"/></g></svg>`,
        st:`<svg viewBox="0 0 200 80" class="cloud-svg" xmlns="http://www.w3.org/2000/svg"><rect x="0" y="26" width="200" height="24" rx="6" fill="rgba(255,255,255,0.03)"/></svg>`,
        sc:`<svg viewBox="0 0 200 80" class="cloud-svg" xmlns="http://www.w3.org/2000/svg"><g fill="rgba(255,255,255,0.06)"><ellipse cx="40" cy="40" rx="18" ry="8"/><ellipse cx="80" cy="44" rx="18" ry="8"/><ellipse cx="120" cy="40" rx="18" ry="8"/></g></svg>`,
        ns:`<svg viewBox="0 0 200 80" class="cloud-svg" xmlns="http://www.w3.org/2000/svg"><rect x="0" y="10" width="200" height="60" rx="6" fill="rgba(0,0,0,0.25)"/></svg>`,
        cu:`<svg viewBox="0 0 200 120" class="cloud-svg cloud-anim" xmlns="http://www.w3.org/2000/svg"><g fill="rgba(255,255,255,0.09)"><ellipse cx="60" cy="60" rx="36" ry="22"/><ellipse cx="100" cy="46" rx="30" ry="20"/><ellipse cx="140" cy="62" rx="28" ry="18"/></g></svg>`,
        cb:`<svg viewBox="0 0 200 160" class="cloud-svg cloud-anim" xmlns="http://www.w3.org/2000/svg"><g fill="rgba(255,255,255,0.09)"><ellipse cx="60" cy="80" rx="38" ry="24"/><ellipse cx="100" cy="60" rx="34" ry="22"/><ellipse cx="140" cy="86" rx="30" ry="20"/></g><rect x="82" y="96" width="36" height="54" rx="6" fill="rgba(255,255,255,0.03)"/></svg>`
      };
      return templates[kind] || templates['cu'];
    }

    function renderCards(filter='all'){
      cardsEl.innerHTML = '';
      const list = CLOUDS.filter(c => filter==='all' ? true : (filter===c.key));
      for(const c of list){
        const el = document.createElement('div'); el.className='card'; el.dataset.id=c.id;
        el.innerHTML = `
          <div class="meta"><div style="font-weight:800">${c.name}</div><div style="flex:1"></div><div class="badge ${c.key==='high'?'alt-high':c.key==='mid'?'alt-mid':c.key==='low'?'alt-low':'alt-vert'}">${c.group}</div></div>
          <div class="sky">${makeCloudSVG(c.id)}</div>
          <div class="details tiny muted">${c.desc}</div>
        `;
        el.addEventListener('click',()=>showInfo(c.id));
        cardsEl.appendChild(el);
      }
    }

    function showInfo(id){
      const c = CLOUDS.find(x=>x.id===id);
      if(!c) return;
      infoTitle.textContent = c.name + ' — ' + c.group;
      infoSub.textContent = `Typical altitude: ${c.altitude}`;
      infoBody.innerHTML = `
        <div style="display:flex;gap:12px;align-items:center"><div style="flex:1"><div style="font-weight:700">How it forms</div><ul>${c.formation.map(x=>`<li>${x}</li>`).join('')}</ul></div><div style="width:140px">${makeCloudSVG(c.id)}</div></div>
        <div style="height:10px"></div>
        <div style="font-weight:700">What it means</div>
        <p class="muted">${c.meaning}</p>
        <div style="height:8px"></div>
        <div style="display:flex;gap:8px"><button class="btn" onclick="highlight('${c.key}')">Show similar</button><button class="copy" onclick="copyText('$(c.name)')">Bookmark</button></div>
      `;
      window.scrollTo({top:0,behavior:'smooth'});
    }

    function highlight(key){
      document.querySelectorAll('.pill').forEach(p=>p.classList.toggle('active', p.dataset.filter===key));
      renderCards(key==='all'? 'all' : key);
    }

    // initial render
    renderCards();

    // filter pills
    document.querySelectorAll('.pill').forEach(p=>{
      p.addEventListener('click',()=>{
        const f = p.dataset.filter||'all';
        document.querySelectorAll('.pill').forEach(x=>x.classList.remove('active'));
        p.classList.add('active');
        if(f==='all') renderCards(); else renderCards(f);
      })
    });
    document.querySelector('.pill[data-filter="all"]').classList.add('active');

    // altitude slider
    const alt = document.getElementById('altitude');
    const altLabel = document.getElementById('altitude-label');
    const altMap = ['Any','Low (<2 km)','Mid (2-6 km)','High (>6 km)','Vertical (surface→high)'];
    alt.addEventListener('input',()=>{
      altLabel.textContent = 'Altitude: ' + altMap[alt.value];
      const mapKey = (alt.value==0)?'all':(alt.value==1?'low':(alt.value==2?'mid':(alt.value==3?'high':'vertical')));
      document.querySelectorAll('.pill').forEach(x=>x.classList.toggle('active', x.dataset.filter===mapKey));
      renderCards(mapKey);
    });

    // identify helper
    document.getElementById('identify').addEventListener('click',()=>{
      const app = document.getElementById('appearance').value;
      let pick;
      if(app==='puffy') pick='cu';
      if(app==='tower') pick='cb';
      if(app==='sheet') pick='as';
      if(app==='wispy') pick='ci';
      if(app==='lumpy') pick='ac';
      if(app==='blanket') pick='st';
      showInfo(pick);
    });

    // quiz
    const quizToggle = document.getElementById('quiz-toggle');
    const quizEl = document.getElementById('quiz');
    quizToggle.addEventListener('click',()=>{
      quizEl.style.display = quizEl.style.display==='none' || quizEl.style.display==='' ? 'block' : 'none';
      if(quizEl.innerHTML.trim()==='') startQuiz();
    });

    function startQuiz(){
      const q = [
        {q:'Thin, wispy clouds high in the sky that often give halos — what are they?', a:'Cirrostratus', options:['Cumulus','Stratus','Cirrostratus','Altocumulus'], ans:2},
        {q:'A towering thunderstorm cloud with an anvil top?', a:'Cumulonimbus', options:['Stratocumulus','Cumulonimbus','Cirrus','Altostratus'], ans:1},
        {q:'A low, uniform gray layer causing drizzle?', a:'Stratus', options:['Altocumulus','Nimbostratus','Stratus','Cirrocumulus'], ans:2}
      ];
      quizEl.innerHTML='';
      q.forEach((qq,idx)=>{
        const block = document.createElement('div'); block.style.padding='10px'; block.style.borderRadius='8px'; block.style.background='var(--glass)';
        block.innerHTML = `<div style="font-weight:700">Q${idx+1}. ${qq.q}</div>`;
        qq.options.forEach((opt,i)=>{
          const b = document.createElement('button'); b.className='pill small'; b.textContent=opt; b.style.marginTop='8px';
          b.addEventListener('click',()=>{
            if(i===qq.ans){ b.style.background='linear-gradient(90deg,#7dd3fc,#60a5fa)'; alert('Correct! 🎉'); }
            else { b.style.background='rgba(255,80,80,0.12)'; alert('Not quite — try another question.'); }
          });
          block.appendChild(b);
        });
        quizEl.appendChild(block);
      });
    }

    // copy README badge
    document.getElementById('copy-badge').addEventListener('click',()=>copyBadge());
    function copyBadge(){
      const snippet = badgeSnippet();
      navigator.clipboard.writeText(snippet).then(()=>{alert('README badge markdown copied to clipboard!');});
    }

    function badgeSnippet(){
      // templated: user should replace USER and REPO and BRANCH as needed
      const urlTemplate = 'https://USERNAME.github.io/REPO/docs/clouds.html';
      const badge = `[![Clouds](${urlTemplate.replace('USERNAME','USERNAME').replace('REPO','REPO')}/assets/badge-clouds.svg)](${urlTemplate})`;
      const md = `<!-- Replace USERNAME / REPO above with your repo details -->\n${badge}\n\nVisit the interactive guide: <${urlTemplate}>`;
      document.getElementById('badge-snippet').textContent = md;
      return md;
    }
    badgeSnippet();

    function copyText(t){ navigator.clipboard.writeText(t); }

    // small helper for deep-linking
    (function checkHash(){
      const h = location.hash.slice(1);
      if(!h) return;
      const found = CLOUDS.find(c=>c.id===h || c.name.toLowerCase()===h.toLowerCase());
      if(found) showInfo(found.id);
    })();

  </script>

</body>
</html>
