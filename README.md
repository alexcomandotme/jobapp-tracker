
<h2 class="sr-only">The temptation to quit will be the greatest just before you're about to succeed</h2>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
.page { background: #f0c800; min-height: 100vh; padding: 2rem 1rem; }
.wrap { max-width: 800px; margin: 0 auto; background: #f2f2f2; border-radius: 12px; padding: 2rem 1.5rem 3rem; }
.stats { display: grid; grid-template-columns: repeat(4, minmax(0,1fr)); gap: 2rem; margin-bottom: 3rem; }
.stat-val { font-size: 32px; font-weight: 500; color: #111; line-height: 1; }
.stat-lbl { font-size: 12px; color: #888; margin-top: 4px; letter-spacing: 0.04em; }
.bar-wrap { margin-bottom: 3rem; }
.bar-track { height: 1px; background: #d0d0d0; margin-bottom: 8px; }
.bar-fill { height: 1px; background: #111; transition: width 0.4s; }
.bar-lbl { font-size: 12px; color: #888; }
.form { margin-bottom: 2.5rem; display: grid; gap: 8px; }
.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
.form-row2 { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
.form-row3 { display: grid; grid-template-columns: 1fr auto; gap: 8px; }
input, select, textarea { font-size: 14px; background: #ebebeb; color: #111; border: 0.5px solid #ccc; border-radius: 8px; padding: 0 12px; height: 36px; outline: none; font-family: inherit; }
textarea { height: 60px; padding: 8px 12px; resize: none; }
input::placeholder, textarea::placeholder { color: #aaa; }
input:focus, select:focus, textarea:focus { border-color: #999; }
select option { background: #ebebeb; color: #111; }
button { font-size: 13px; background: #111; color: #fff; border: none; border-radius: 8px; padding: 0 16px; height: 36px; cursor: pointer; letter-spacing: 0.02em; }
button:hover { background: #333; }
.app-item { padding: 12px 0; border-bottom: 0.5px solid #ddd; }
.app-main { display: flex; align-items: center; gap: 12px; }
.app-company { font-size: 14px; font-weight: 500; color: #111; }
.app-company a { color: #1a6fc4; text-decoration: none; }
.app-company a:hover { text-decoration: underline; }
.app-role { font-size: 12px; color: #888; }
.app-note { font-size: 12px; color: #888; margin-top: 5px; font-style: italic; }
.app-date { font-size: 12px; color: #888; margin-left: auto; white-space: nowrap; }
.app-del { background: none; border: none; cursor: pointer; color: #bbb; font-size: 16px; padding: 0 0 0 8px; line-height: 1; height: auto; }
.app-del:hover { color: #555; background: none; }
.badge { font-size: 11px; padding: 2px 7px; border-radius: 3px; white-space: nowrap; }
.status-select { font-size: 11px; background: transparent; border: none; cursor: pointer; padding: 2px 4px; outline: none; color: inherit; font-family: inherit; height: auto; border-radius: 3px; }
.empty { font-size: 13px; color: #aaa; padding: 1.5rem 0; }
.io-bar { display: flex; gap: 8px; margin-top: 2.5rem; padding-top: 1.5rem; border-top: 0.5px solid #ddd; align-items: center; }
.io-btn { font-size: 11px; background: none; color: #888; border: 0.5px solid #ccc; border-radius: 6px; padding: 0 12px; height: 28px; cursor: pointer; letter-spacing: 0.02em; }
.io-btn:hover { color: #111; border-color: #999; background: none; }
.sync-status { font-size: 11px; color: #aaa; display: flex; align-items: center; gap: 5px; }
.sync-dot { width: 6px; height: 6px; border-radius: 50%; background: #ccc; display: inline-block; flex-shrink: 0; }
.sync-dot.syncing { background: #f0c800; }
.sync-dot.ok { background: #5a9e5a; }
.sync-dot.err { background: #c0392b; }
.io-lbl { font-size: 11px; color: #aaa; margin-left: auto; }
</style>

<div class="page">
<div class="wrap">
  <div class="stats">
    <div><div class="stat-val" id="total">0</div><div class="stat-lbl">aplicate</div></div>
    <div><div class="stat-val">70</div><div class="stat-lbl">target</div></div>
    <div><div class="stat-val" id="days-left">0</div><div class="stat-lbl">zile rămase</div></div>
    <div><div class="stat-val" id="per-day">0</div><div class="stat-lbl">necesar/zi</div></div>
  </div>

  <div class="bar-wrap">
    <div class="bar-track"><div class="bar-fill" id="bar" style="width:0%"></div></div>
    <div class="bar-lbl" id="bar-lbl">0 / 70</div>
  </div>

  <div class="form">
    <div class="form-row">
      <input type="text" id="company" placeholder="companie" />
      <input type="text" id="role" placeholder="rol" />
    </div>
    <div class="form-row2">
      <input type="text" id="link" placeholder="link (opțional)" />
      <select id="status">
        <option value="de aplicat">de aplicat</option>
        <option value="aplicat">aplicat</option>
        <option value="interviu">interviu</option>
        <option value="respins">respins</option>
        <option value="fără răspuns">fără răspuns</option>
      </select>
    </div>
    <textarea id="note" placeholder="notiță (opțional)"></textarea>
    <div class="form-row3">
      <div></div>
      <button onclick="addApp()">adaugă</button>
    </div>
  </div>

  <div id="list"></div>

  <div class="io-bar">
    <button class="io-btn" onclick="exportCSV()">export CSV</button>
    <button class="io-btn" onclick="document.getElementById('import-file').click()">import CSV</button>
    <input type="file" id="import-file" accept=".csv" style="display:none" onchange="importCSV(this)" />
    <div class="sync-status"><span class="sync-dot" id="sync-dot"></span><span id="io-msg"></span></div>
    <span class="io-lbl"></span>
  </div>
</div>
</div>

<script>
const TARGET = 70;
const END = new Date('2026-05-31');
const SHEETS_URL = 'https://script.google.com/macros/s/AKfycbwqDk4uoRiA7gnNy8NQEqHo1pKHWfqZP5D5JUYbBFjjCbJ9n9RxxIJaAfktAhKqaASW/exec';

let apps = [];
try { apps = JSON.parse(localStorage.getItem('job_apps_v3') || '[]'); } catch(e) {}

const SC = {
  'de aplicat':   { bg: '#FFF8DC', color: '#7a6000' },
  'aplicat':      { bg: '#E6F1FB', color: '#0C447C' },
  'interviu':     { bg: '#EAF3DE', color: '#27500A' },
  'respins':      { bg: '#FCEBEB', color: '#791F1F' },
  'fără răspuns': { bg: '#F1EFE8', color: '#5F5E5A' }
};
const STATUSES = ['de aplicat','aplicat','interviu','respins','fără răspuns'];

function normalizeLink(raw) {
  if (!raw) return '';
  raw = raw.trim();
  if (!raw) return '';
  if (/^https?:\/\//i.test(raw)) return raw;
  return 'https://' + raw;
}

function save() { try { localStorage.setItem('job_apps_v3', JSON.stringify(apps)); } catch(e) {} }

function setSyncStatus(state, text) {
  const dot = document.getElementById('sync-dot');
  const msg = document.getElementById('io-msg');
  dot.className = 'sync-dot' + (state ? ' ' + state : '');
  msg.textContent = text || '';
}

async function syncToSheets(app) {
  setSyncStatus('syncing', 'sync...');
  try {
    await fetch(SHEETS_URL, {
      method: 'POST',
      mode: 'no-cors',
      headers: { 'Content-Type': 'text/plain' },
      body: JSON.stringify(app)
    });
    setSyncStatus('ok', 'sincronizat');
    setTimeout(() => setSyncStatus('', ''), 3000);
  } catch(e) {
    setSyncStatus('err', 'eroare sync');
    setTimeout(() => setSyncStatus('', ''), 4000);
  }
}

function daysLeft() {
  const today = new Date(); today.setHours(0,0,0,0);
  let n = 0, d = new Date(today);
  while (d <= END) { if (d.getDay()!==0 && d.getDay()!==6) n++; d.setDate(d.getDate()+1); }
  return n;
}

function countApplied() { return apps.filter(a => a.status !== 'de aplicat').length; }

function render() {
  const total = countApplied();
  const dl = daysLeft();
  const rem = Math.max(0, TARGET - total);
  const pd = dl > 0 ? Math.ceil(rem / dl) : 0;
  const pct = Math.min(100, Math.round(total / TARGET * 100));
  document.getElementById('total').textContent = total;
  document.getElementById('days-left').textContent = dl;
  document.getElementById('per-day').textContent = pd;
  document.getElementById('bar').style.width = pct + '%';
  document.getElementById('bar-lbl').textContent = total + ' / ' + TARGET;

  const list = document.getElementById('list');
  list.innerHTML = '';
  if (!apps.length) { list.innerHTML = '<div class="empty">nicio aplicație încă</div>'; return; }

  [...apps].reverse().forEach((app, i) => {
    const ri = apps.length - 1 - i;
    const sc = SC[app.status] || SC['aplicat'];
    const opts = STATUSES.map(s => `<option value="${s}" ${s===app.status?'selected':''}>${s}</option>`).join('');
    const normalizedLink = normalizeLink(app.link);
    const companyHtml = normalizedLink
      ? `<a href="${normalizedLink}" target="_blank" rel="noopener">${app.company}</a>`
      : app.company;
    const row = document.createElement('div');
    row.className = 'app-item';
    row.innerHTML = `
      <div class="app-main">
        <div style="flex:1;min-width:0;">
          <div class="app-company">${companyHtml}</div>
          ${app.role ? `<div class="app-role">${app.role}</div>` : ''}
        </div>
        <span class="badge" style="background:${sc.bg};color:${sc.color};padding:0;">
          <select class="status-select" style="background:${sc.bg};color:${sc.color};" onchange="changeStatus(${ri},this.value)">${opts}</select>
        </span>
        <div class="app-date">${app.date}</div>
        <button class="app-del" onclick="del(${ri})" aria-label="șterge">×</button>
      </div>
      ${app.note ? `<div class="app-note">${app.note}</div>` : ''}
    `;
    list.appendChild(row);
  });
}

function addApp() {
  const c = document.getElementById('company').value.trim();
  if (!c) return;
  const app = {
    company: c,
    role: document.getElementById('role').value.trim(),
    status: document.getElementById('status').value,
    link: normalizeLink(document.getElementById('link').value),
    note: document.getElementById('note').value.trim(),
    date: new Date().toLocaleDateString('ro-RO', { day:'2-digit', month:'2-digit' })
  };
  apps.push(app);
  save(); render();
  ['company','role','link','note'].forEach(id => document.getElementById(id).value = '');
  syncToSheets(app);
}

function changeStatus(i, val) {
  apps[i].status = val;
  save(); render();
  syncToSheets(apps[i]);
}

function del(i) { apps.splice(i, 1); save(); render(); }

function exportCSV() {
  const rows = [['companie','rol','status','link','notita','data']];
  apps.forEach(a => rows.push([a.company, a.role||'', a.status, a.link||'', (a.note||'').replace(/,/g,';'), a.date]));
  const csv = rows.map(r => r.map(v => `"${v}"`).join(',')).join('\n');
  const a = document.createElement('a');
  a.href = 'data:text/csv;charset=utf-8,' + encodeURIComponent(csv);
  a.download = 'aplicatii_' + new Date().toISOString().slice(0,10) + '.csv';
  a.click();
  setSyncStatus('ok', 'exportat');
  setTimeout(() => setSyncStatus('', ''), 3000);
}

function importCSV(input) {
  const file = input.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    const lines = e.target.result.trim().split('\n').slice(1);
    let count = 0;
    lines.forEach(line => {
      const cols = line.match(/"([^"]*)"/g);
      if (!cols || cols.length < 6) return;
      const [company, role, status, link, note, date] = cols.map(c => c.replace(/"/g,''));
      if (!company) return;
      const exists = apps.some(a => a.company === company && a.date === date);
      if (!exists) { apps.push({ company, role, status, link, note, date }); count++; }
    });
    save(); render();
    setSyncStatus('ok', count + ' importate');
    setTimeout(() => setSyncStatus('', ''), 3000);
    input.value = '';
  };
  reader.readAsText(file);
}

render();
</script>
