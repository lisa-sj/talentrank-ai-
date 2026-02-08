<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TalentRank AI — Recruitment Screening Platform</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;0,9..40,700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
:root {
  --bg: #0B0F1A;
  --surface: #111827;
  --surface-light: #1F2937;
  --border: #2D3748;
  --primary: #3B82F6;
  --accent: #10B981;
  --warning: #F59E0B;
  --danger: #EF4444;
  --text: #F9FAFB;
  --text-muted: #9CA3AF;
  --text-dim: #6B7280;
  --gold: #FBBF24;
  --purple: #8B5CF6;
  --pink: #EC4899;
}
* { margin:0; padding:0; box-sizing:border-box; }
body { font-family:'DM Sans',sans-serif; background:var(--bg); color:var(--text); display:flex; height:100vh; overflow:hidden; }
::-webkit-scrollbar { width:6px; }
::-webkit-scrollbar-track { background:transparent; }
::-webkit-scrollbar-thumb { background:var(--border); border-radius:3px; }

/* Sidebar */
#sidebar { width:220px; background:var(--surface); border-right:1px solid var(--border); display:flex; flex-direction:column; flex-shrink:0; }
.sidebar-logo { padding:20px 20px 24px; border-bottom:1px solid var(--border); }
.sidebar-logo h1 { font-size:20px; font-weight:700; letter-spacing:-0.5px; }
.sidebar-logo h1 span { color:var(--primary); }
.sidebar-logo p { font-size:11px; color:var(--text-dim); margin-top:2px; }
#sidebar nav { flex:1; padding:12px 10px; }
.nav-btn { width:100%; display:flex; align-items:center; gap:10px; padding:10px 14px; border-radius:10px; border:none; cursor:pointer; background:transparent; color:var(--text-muted); font-size:14px; font-family:inherit; text-align:left; margin-bottom:2px; transition:all .2s; }
.nav-btn:hover { background:rgba(59,130,246,.08); }
.nav-btn.active { background:rgba(59,130,246,.12); color:var(--primary); font-weight:600; }
.nav-btn .icon { font-size:16px; width:20px; text-align:center; }
.sidebar-footer { padding:16px; border-top:1px solid var(--border); }
.btn-add { width:100%; padding:10px 0; border-radius:10px; border:none; background:linear-gradient(135deg,var(--primary),var(--purple)); color:#fff; font-size:13px; font-weight:600; cursor:pointer; font-family:inherit; }
.btn-add:hover { opacity:.9; }

/* Main */
#main { flex:1; overflow:auto; padding:28px; }
.page { display:none; }
.page.active { display:block; }
h1.page-title { font-size:24px; font-weight:700; margin-bottom:4px; }
p.page-sub { color:var(--text-dim); font-size:14px; margin-bottom:24px; }

/* Stat Cards */
.stats-grid { display:grid; grid-template-columns:repeat(4,1fr); gap:16px; margin-bottom:24px; }
.stat-card { background:linear-gradient(135deg,var(--surface),var(--surface-light)); border:1px solid var(--border); border-radius:16px; padding:20px 24px; position:relative; overflow:hidden; }
.stat-card .glow { position:absolute; top:-20px; right:-20px; width:80px; height:80px; border-radius:50%; opacity:.07; }
.stat-card .label { font-size:12px; color:var(--text-dim); letter-spacing:.5px; text-transform:uppercase; margin-bottom:4px; }
.stat-card .value { font-size:32px; font-weight:700; line-height:1.1; }
.stat-card .sub { font-size:12px; color:var(--text-muted); margin-top:4px; }

/* Panel */
.panel { background:var(--surface); border-radius:16px; padding:20px; border:1px solid var(--border); }
.panel h3 { font-size:15px; margin-bottom:16px; }

/* Table */
.data-table { width:100%; border-collapse:collapse; }
.data-table th { padding:8px 12px; text-align:left; font-size:11px; color:var(--text-dim); border-bottom:1px solid var(--border); font-weight:500; text-transform:uppercase; letter-spacing:.5px; }
.data-table td { padding:10px 12px; font-size:13px; }
.data-table tbody tr { cursor:pointer; transition:background .15s; }
.data-table tbody tr:hover { background:var(--surface-light); }
.cand-name { font-weight:500; }
.cand-title { font-size:11px; color:var(--text-dim); }

/* Badges */
.score-badge { display:inline-block; padding:3px 10px; border-radius:20px; font-weight:600; font-size:13px; }
.score-high { background:rgba(16,185,129,.1); color:var(--accent); }
.score-mid { background:rgba(245,158,11,.1); color:var(--warning); }
.score-low { background:rgba(239,68,68,.1); color:var(--danger); }
.status-badge { padding:2px 10px; border-radius:12px; font-size:11px; font-weight:600; text-transform:uppercase; letter-spacing:.5px; }
.status-new { background:rgba(59,130,246,.12); color:var(--primary); }
.status-reviewed { background:rgba(245,158,11,.12); color:var(--warning); }
.status-shortlisted { background:rgba(16,185,129,.12); color:var(--accent); }

/* Mini bar */
.mini-bar { display:flex; align-items:center; gap:8px; }
.mini-bar-track { flex:1; height:6px; background:var(--surface-light); border-radius:3px; overflow:hidden; }
.mini-bar-fill { height:100%; border-radius:3px; transition:width .6s ease; }
.mini-bar-val { font-size:12px; color:var(--text-muted); min-width:28px; text-align:right; }

/* Charts row */
.charts-row { display:grid; grid-template-columns:3fr 2fr; gap:16px; margin-bottom:24px; }
.benchmark-row { display:grid; grid-template-columns:1fr 1fr; gap:16px; margin-bottom:24px; }

/* Filters */
.filters { display:flex; gap:10px; margin-bottom:20px; flex-wrap:wrap; }
.filters input, .filters select { padding:9px 14px; border-radius:10px; border:1px solid var(--border); background:var(--surface); color:var(--text); font-size:13px; font-family:inherit; outline:none; }
.filters input { flex:1; min-width:200px; }
.filter-count { font-size:12px; color:var(--text-dim); margin-bottom:12px; }

/* Clusters */
.cluster-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:16px; }
.cluster-card { background:var(--surface); border-radius:16px; border:1px solid var(--border); padding:20px; position:relative; overflow:hidden; }
.cluster-card .glow-circle { position:absolute; top:-30px; right:-30px; width:100px; height:100px; border-radius:50%; opacity:.06; }
.cluster-header { display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:16px; }
.cluster-count-badge { width:36px; height:36px; border-radius:10px; display:flex; align-items:center; justify-content:center; font-weight:700; font-size:14px; }
.cluster-candidates { margin-top:16px; border-top:1px solid var(--border); padding-top:12px; }
.cluster-candidates .label { font-size:11px; color:var(--text-dim); margin-bottom:6px; }
.cluster-cand-row { display:flex; justify-content:space-between; align-items:center; padding:4px 0; cursor:pointer; font-size:12px; }

/* AHP */
.ahp-container { max-width:640px; }
.ahp-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:20px; }
.ahp-total { padding:6px 16px; border-radius:20px; font-size:13px; font-weight:600; }
.ahp-slider-card { background:var(--surface); border-radius:12px; padding:16px; border:1px solid var(--border); margin-bottom:12px; }
.ahp-slider-card .top { display:flex; justify-content:space-between; align-items:center; margin-bottom:8px; }
.ahp-slider-card .top .label { color:var(--text); font-size:14px; }
.ahp-slider-card .top .pct { color:var(--primary); font-weight:700; font-size:18px; }
.ahp-slider-card input[type=range] { width:100%; accent-color:var(--primary); cursor:pointer; }

/* Gap bar */
.gap-item { margin-bottom:16px; }
.gap-header { display:flex; justify-content:space-between; margin-bottom:6px; }
.gap-header .dim { font-size:13px; }
.gap-header .pts { font-size:13px; font-weight:600; }
.gap-bar-wrap { height:8px; background:var(--surface-light); border-radius:4px; overflow:hidden; position:relative; }
.gap-bar-fill { position:absolute; left:0; top:0; height:100%; border-radius:4px; opacity:.8; }
.gap-bar-marker { position:absolute; top:0; height:100%; border-right:2px solid var(--danger); }
.gap-labels { display:flex; justify-content:space-between; margin-top:2px; font-size:10px; }

/* Modal overlay */
.modal-overlay { position:fixed; inset:0; background:rgba(0,0,0,.7); z-index:1000; display:none; justify-content:center; align-items:flex-start; padding-top:40px; backdrop-filter:blur(4px); }
.modal-overlay.open { display:flex; }
.modal-content { background:var(--bg); border:1px solid var(--border); border-radius:20px; width:90%; max-width:900px; max-height:85vh; overflow:auto; padding:32px; }
.modal-close { background:none; border:none; color:var(--text-dim); font-size:24px; cursor:pointer; padding:4px; }
.modal-top { display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:24px; }
.modal-avatar { width:44px; height:44px; border-radius:12px; display:flex; align-items:center; justify-content:center; background:linear-gradient(135deg,var(--primary),var(--purple)); color:#fff; font-weight:700; font-size:18px; }
.modal-info { display:flex; align-items:center; gap:12px; }
.modal-name { font-size:22px; font-weight:600; }
.modal-title-text { color:var(--text-muted); font-size:14px; }
.modal-stats { display:grid; grid-template-columns:repeat(4,1fr); gap:12px; margin-bottom:24px; }
.modal-charts { display:grid; grid-template-columns:1fr 1fr; gap:20px; margin-bottom:24px; }
.skills-wrap { display:flex; flex-wrap:wrap; gap:6px; margin-bottom:20px; }
.skill-tag { padding:4px 12px; border-radius:20px; font-size:12px; font-weight:500; }
.detail-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:12px; }
.detail-item { background:var(--surface-light); border-radius:10px; padding:10px 14px; }
.detail-item .dlabel { font-size:11px; color:var(--text-dim); margin-bottom:2px; }
.detail-item .dvalue { font-size:14px; }

/* Upload modal */
.upload-modal { width:520px; max-width:95vw; }
.upload-steps { display:flex; gap:4px; margin-bottom:28px; }
.upload-step { flex:1; text-align:center; }
.upload-step .bar { height:4px; border-radius:2px; margin-bottom:6px; background:var(--surface-light); transition:background .3s; }
.upload-step .bar.active { background:var(--primary); }
.upload-step .slabel { font-size:11px; color:var(--text-dim); }
.upload-step .slabel.active { color:var(--primary); }
.drop-zone { border:2px dashed var(--border); border-radius:16px; padding:40px; text-align:center; cursor:pointer; transition:border-color .2s; }
.drop-zone:hover { border-color:var(--primary); }
.upload-field { margin-bottom:12px; }
.upload-field label { font-size:12px; color:var(--text-muted); display:block; margin-bottom:4px; }
.upload-field input { width:100%; padding:10px 14px; border-radius:10px; border:1px solid var(--border); background:var(--surface); color:var(--text); font-size:14px; outline:none; font-family:inherit; }
.btn-process { margin-top:8px; width:100%; padding:12px 0; border-radius:12px; border:none; background:linear-gradient(135deg,var(--primary),var(--purple)); color:#fff; font-size:15px; font-weight:600; cursor:pointer; font-family:inherit; }
.btn-skip { margin-top:12px; padding:10px 24px; border-radius:10px; border:1px solid var(--border); background:var(--surface); color:var(--text-muted); font-size:13px; cursor:pointer; font-family:inherit; }
.btn-view { padding:12px 32px; border-radius:12px; border:none; background:var(--accent); color:#fff; font-size:15px; font-weight:600; cursor:pointer; font-family:inherit; }
@keyframes spin { 0%{transform:rotate(0deg)} 100%{transform:rotate(360deg)} }
.spinner { display:inline-block; animation:spin 2s linear infinite; font-size:48px; }
@keyframes loading { 0%{width:20%} 50%{width:80%} 100%{width:20%} }
.loading-bar { animation:loading 2s ease-in-out infinite; }

.legend-row { display:flex; justify-content:center; gap:24px; margin-top:8px; }
.legend-item { display:flex; align-items:center; gap:6px; font-size:12px; color:var(--text-muted); }
.legend-dot { width:12px; height:3px; border-radius:2px; }

@media(max-width:900px){
  .stats-grid{grid-template-columns:repeat(2,1fr);}
  .charts-row,.benchmark-row{grid-template-columns:1fr;}
  .cluster-grid{grid-template-columns:1fr 1fr;}
  .modal-stats{grid-template-columns:repeat(2,1fr);}
  .modal-charts{grid-template-columns:1fr;}
}
</style>
</head>
<body>

<!-- Sidebar -->
<div id="sidebar">
  <div class="sidebar-logo">
    <h1><span>Talent</span>Rank</h1>
    <p>AI Recruitment Platform</p>
  </div>
  <nav>
    <button class="nav-btn active" data-page="dashboard"><span class="icon">◈</span> Dashboard</button>
    <button class="nav-btn" data-page="rankings"><span class="icon">◆</span> Rankings</button>
    <button class="nav-btn" data-page="clusters"><span class="icon">◉</span> Clusters</button>
    <button class="nav-btn" data-page="benchmark"><span class="icon">◎</span> Benchmark</button>
    <button class="nav-btn" data-page="ahp"><span class="icon">⚙</span> AHP Config</button>
  </nav>
  <div class="sidebar-footer">
    <button class="btn-add" onclick="openUpload()">+ Add Candidate</button>
  </div>
</div>

<!-- Main -->
<div id="main">

  <!-- DASHBOARD -->
  <div id="page-dashboard" class="page active">
    <h1 class="page-title">Dashboard</h1>
    <p class="page-sub">Senior Software Engineer — Pipeline Overview</p>
    <div class="stats-grid" id="dash-stats"></div>
    <div class="charts-row">
      <div class="panel"><h3>Application Trend (This Week)</h3><div style="position:relative;height:200px;width:100%"><canvas id="trendChart"></canvas></div></div>
      <div class="panel"><h3>Cluster Distribution</h3><div style="position:relative;height:200px;width:100%"><canvas id="clusterChart"></canvas></div></div>
    </div>
    <div class="panel"><h3>Top 10 Candidates</h3><table class="data-table"><thead><tr><th>Rank</th><th>Candidate</th><th>TOPSIS</th><th>Technical</th><th>Experience</th><th>Cluster</th><th>Status</th></tr></thead><tbody id="top10-body"></tbody></table></div>
  </div>

  <!-- RANKINGS -->
  <div id="page-rankings" class="page">
    <h1 class="page-title">Candidate Rankings</h1>
    <p class="page-sub">Full TOPSIS-ranked candidate list with filters</p>
    <div class="filters">
      <input type="text" id="searchInput" placeholder="Search by name, title, or skill..." oninput="renderRankings()">
      <select id="clusterFilter" onchange="renderRankings()"><option value="all">All Clusters</option></select>
      <select id="statusFilter" onchange="renderRankings()"><option value="all">All Status</option><option value="new">New</option><option value="reviewed">Reviewed</option><option value="shortlisted">Shortlisted</option></select>
      <select id="sortBy" onchange="renderRankings()"><option value="rank">Sort: TOPSIS Rank</option><option value="technical">Sort: Technical</option><option value="experience">Sort: Experience</option><option value="name">Sort: Name</option></select>
    </div>
    <div class="filter-count" id="filterCount"></div>
    <div class="panel" style="padding:0;overflow:hidden">
      <table class="data-table"><thead><tr><th>#</th><th>Candidate</th><th>TOPSIS</th><th>Tech</th><th>Exp</th><th>Edu</th><th>Soft</th><th>Culture</th><th>YoE</th><th>Cluster</th><th>Status</th></tr></thead><tbody id="rankings-body"></tbody></table>
    </div>
  </div>

  <!-- CLUSTERS -->
  <div id="page-clusters" class="page">
    <h1 class="page-title">Cluster Explorer</h1>
    <p class="page-sub">K-Means++ auto-classified candidate groups</p>
    <div class="cluster-grid" id="cluster-grid"></div>
  </div>

  <!-- BENCHMARK -->
  <div id="page-benchmark" class="page">
    <h1 class="page-title">Competitor Benchmark</h1>
    <p class="page-sub">Your talent pipeline vs. competitor talent (Google Engineering)</p>
    <div class="benchmark-row">
      <div class="panel">
        <h3>Talent Radar Comparison</h3>
        <div style="position:relative;height:300px;width:100%"><canvas id="radarChart"></canvas></div>
        <div class="legend-row">
          <div class="legend-item"><div class="legend-dot" style="background:var(--primary)"></div> Your Pipeline</div>
          <div class="legend-item"><div class="legend-dot" style="background:var(--danger)"></div> Google (Competitor)</div>
        </div>
      </div>
      <div class="panel">
        <h3>Gap Analysis</h3>
        <div id="gap-analysis"></div>
      </div>
    </div>
    <div class="stats-grid" style="grid-template-columns:repeat(3,1fr)" id="bench-stats"></div>
  </div>

  <!-- AHP CONFIG -->
  <div id="page-ahp" class="page">
    <div class="ahp-container">
      <div class="ahp-header">
        <div>
          <h1 class="page-title" style="margin-bottom:4px">AHP Weight Configuration</h1>
          <p style="color:var(--text-dim);font-size:13px">Adjust criteria weights using sliders. Total must equal 100%.</p>
        </div>
        <div class="ahp-total" id="ahp-total">Total: 100%</div>
      </div>
      <div id="ahp-sliders"></div>
    </div>
  </div>
</div>

<!-- Candidate Detail Modal -->
<div class="modal-overlay" id="candidateModal" onclick="closeModal(event)">
  <div class="modal-content" onclick="event.stopPropagation()">
    <div class="modal-top">
      <div class="modal-info">
        <div class="modal-avatar" id="modal-avatar"></div>
        <div><div class="modal-name" id="modal-name"></div><div class="modal-title-text" id="modal-title"></div></div>
      </div>
      <button class="modal-close" onclick="document.getElementById('candidateModal').classList.remove('open')">✕</button>
    </div>
    <div class="modal-stats" id="modal-stats"></div>
    <div class="modal-charts">
      <div class="panel"><h3>Score Breakdown</h3><div style="position:relative;height:220px;width:100%"><canvas id="modalRadar"></canvas></div></div>
      <div class="panel"><h3>Dimension Scores</h3><div id="modal-dimensions"></div></div>
    </div>
    <div class="panel" style="margin-bottom:20px"><h3>Skills</h3><div class="skills-wrap" id="modal-skills"></div></div>
    <div class="detail-grid" id="modal-details"></div>
  </div>
</div>

<!-- Upload Modal -->
<div class="modal-overlay" id="uploadModal" onclick="closeUpload(event)">
  <div class="modal-content upload-modal" onclick="event.stopPropagation()">
    <h2 style="margin-bottom:20px;font-size:20px">Add New Candidate</h2>
    <div class="upload-steps" id="upload-steps"></div>
    <div id="upload-content"></div>
  </div>
</div>

<script>
// ===== DATA GENERATION =====
const SKILLS_DB=["Python","JavaScript","TypeScript","React","Node.js","AWS","Docker","Kubernetes","PostgreSQL","MongoDB","GraphQL","REST API","Machine Learning","TensorFlow","PyTorch","Java","Go","Rust","C++","SQL","Redis","Elasticsearch","CI/CD","Terraform","System Design","Microservices","Agile","Leadership","Communication","Problem Solving"];
const COMPANIES=["Google","Meta","Amazon","Apple","Microsoft","Netflix","Stripe","Airbnb","Uber","Salesforce"];
const firstNames=["Alex","Jordan","Morgan","Taylor","Casey","Riley","Quinn","Avery","Sage","Rowan","Cameron","Dakota","Reese","Skyler","Jamie","Drew","Blake","Emery","Finley","Harper","Kai","Leo","Mia","Noah","Zoe","Liam","Emma","Oliver","Ava","Ethan","Luna","James","Ella","Logan","Chloe","Mason","Aria","Lucas","Lily","Henry","Grace","Jack","Nora","Owen","Hazel","Ryan","Maya","Nathan","Isla","Caleb"];
const lastNames=["Chen","Patel","Kim","Nguyen","Garcia","Martinez","Lee","Wang","Singh","Ali","Johnson","Williams","Brown","Jones","Davis","Miller","Wilson","Moore","Taylor","Anderson","Thomas","Jackson","White","Harris","Martin","Thompson","Young","Hall","Allen","King","Wright","Scott","Green","Baker","Hill","Adams","Nelson","Carter","Mitchell","Perez"];
const titles=["Senior Software Engineer","Full Stack Developer","ML Engineer","Data Scientist","Backend Engineer","Frontend Engineer","DevOps Engineer","Platform Engineer","Staff Engineer","Engineering Manager","Tech Lead","Solutions Architect","Cloud Engineer","Site Reliability Engineer","Security Engineer"];
const locations=["San Francisco, CA","New York, NY","Seattle, WA","Austin, TX","Boston, MA","Chicago, IL","Denver, CO","Los Angeles, CA","Portland, OR","Miami, FL","Remote","Toronto, ON","London, UK","Berlin, DE","Singapore"];
const eduLevels=["PhD","MS","BS","MBA"];
const universities=["MIT","Stanford","Carnegie Mellon","UC Berkeley","Georgia Tech","Caltech","U of Michigan","Cornell","Columbia","U of Washington","ETH Zurich","U of Toronto","Oxford","IIT Bombay","Tsinghua"];
const clusterNames=["Senior Full-Stack","ML/AI Specialist","Cloud/DevOps","Frontend Expert","Backend/Systems","Engineering Leadership"];
const clusterColors=["#3B82F6","#10B981","#8B5CF6","#EC4899","#F59E0B","#FBBF24"];

function genCandidates(n=50){
  let arr=[];
  for(let i=0;i<n;i++){
    const nsk=5+Math.floor(Math.random()*10);
    const sh=[...SKILLS_DB].sort(()=>Math.random()-.5);
    const skills=sh.slice(0,nsk).map(s=>({name:s,prof:Math.round((2+Math.random()*3)*10)/10}));
    const sc={technical:40+Math.random()*60,experience:30+Math.random()*70,education:40+Math.random()*60,softSkills:35+Math.random()*65,culturalFit:40+Math.random()*60};
    const w=[.35,.25,.15,.15,.10],raw=Object.values(sc);
    const norm=Math.sqrt(raw.reduce((s,v)=>s+v*v,0));
    const nrm=raw.map(v=>v/norm),wtd=nrm.map((v,j)=>v*w[j]);
    const idD=Math.sqrt(wtd.reduce((s,v)=>s+Math.pow(w[w.length-1]-v,2),0));
    const aiD=Math.sqrt(wtd.reduce((s,v)=>s+v*v,0));
    const topsis=Math.round((.3+aiD/(idD+aiD)*.65)*1000)/1000;
    const yoe=2+Math.floor(Math.random()*15);
    const ci=Math.floor(Math.random()*clusterNames.length);
    arr.push({
      id:i+1,
      name:firstNames[i%firstNames.length]+" "+lastNames[i%lastNames.length],
      title:titles[Math.floor(Math.random()*titles.length)],
      location:locations[Math.floor(Math.random()*locations.length)],
      yoe,
      education:eduLevels[Math.floor(Math.random()*eduLevels.length)]+", "+universities[Math.floor(Math.random()*universities.length)],
      prevCo:COMPANIES[Math.floor(Math.random()*COMPANIES.length)],
      skills,scores:sc,topsis,
      cluster:clusterNames[ci],clusterIdx:ci,
      status:Math.random()>.7?"reviewed":Math.random()>.5?"shortlisted":"new",
      applied:"2026-02-0"+(Math.floor(Math.random()*8)+1),
      ghStars:Math.floor(Math.random()*500),
      liConn:200+Math.floor(Math.random()*800),
    });
  }
  arr.sort((a,b)=>b.topsis-a.topsis);
  arr.forEach((c,i)=>c.rank=i+1);
  return arr;
}

const candidates=genCandidates(50);
const clusterMap={};
candidates.forEach(c=>{clusterMap[c.cluster]=(clusterMap[c.cluster]||0)+1});
const clusters=Object.entries(clusterMap).map(([name,count])=>({name,count})).sort((a,b)=>b.count-a.count);

const competitorData=[
  {dim:"Technical",yours:78,comp:82},{dim:"Experience",yours:72,comp:75},
  {dim:"Education",yours:81,comp:77},{dim:"Soft Skills",yours:69,comp:71},
  {dim:"Cultural Fit",yours:74,comp:68},{dim:"Innovation",yours:70,comp:80},
];

// ===== HELPERS =====
function scoreBadge(s){
  const cls=s>=.8?"score-high":s>=.6?"score-mid":"score-low";
  return `<span class="score-badge ${cls}">${s.toFixed(3)}</span>`;
}
function statusBadge(st){
  const map={new:"status-new",reviewed:"status-reviewed",shortlisted:"status-shortlisted"};
  return `<span class="status-badge ${map[st]||'status-new'}">${st.charAt(0).toUpperCase()+st.slice(1)}</span>`;
}
function miniBar(val,color="#3B82F6"){
  return `<div class="mini-bar"><div class="mini-bar-track"><div class="mini-bar-fill" style="width:${val}%;background:${color}"></div></div><span class="mini-bar-val">${Math.round(val)}</span></div>`;
}
function statCardHTML(label,value,sub,color){
  return `<div class="stat-card"><div class="glow" style="background:${color}"></div><div class="label">${label}</div><div class="value" style="color:${color}">${value}</div>${sub?`<div class="sub">${sub}</div>`:''}</div>`;
}

// ===== NAVIGATION =====
document.querySelectorAll('.nav-btn').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('.nav-btn').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
    document.getElementById('page-'+btn.dataset.page).classList.add('active');
    if(btn.dataset.page==='benchmark') setTimeout(initBenchmarkCharts, 50);
  });
});

// ===== DASHBOARD =====
function renderDashboard(){
  const avgT=(candidates.reduce((s,c)=>s+c.topsis,0)/candidates.length).toFixed(3);
  const shortlisted=candidates.filter(c=>c.status==="shortlisted").length;
  document.getElementById('dash-stats').innerHTML=
    statCardHTML("TOTAL CANDIDATES",candidates.length,"↑ 12 this week","#3B82F6")+
    statCardHTML("AVG TOPSIS SCORE",avgT,"Above threshold","#10B981")+
    statCardHTML("CLUSTERS",clusters.length,"Auto-classified","#8B5CF6")+
    statCardHTML("SHORTLISTED",shortlisted,"Ready for interview","#FBBF24");

  let html='';
  candidates.slice(0,10).forEach(c=>{
    const rc=c.rank<=3?"#FBBF24":"#6B7280";
    html+=`<tr onclick="openCandidate(${c.id})">
      <td style="font-weight:700;color:${rc}">#${c.rank}</td>
      <td><div class="cand-name">${c.name}</div><div class="cand-title">${c.title}</div></td>
      <td>${scoreBadge(c.topsis)}</td>
      <td style="width:120px">${miniBar(c.scores.technical,"#3B82F6")}</td>
      <td style="width:120px">${miniBar(c.scores.experience,"#8B5CF6")}</td>
      <td style="font-size:12px;color:#9CA3AF">${c.cluster}</td>
      <td>${statusBadge(c.status)}</td>
    </tr>`;
  });
  document.getElementById('top10-body').innerHTML=html;

  // Trend chart
  const days=["Mon","Tue","Wed","Thu","Fri","Sat","Sun"];
  const apps=days.map(()=>15+Math.floor(Math.random()*25));
  const proc=days.map(()=>10+Math.floor(Math.random()*20));
  new Chart(document.getElementById('trendChart'),{
    type:'line',
    data:{labels:days,datasets:[
      {label:'Applications',data:apps,borderColor:'#3B82F6',backgroundColor:'rgba(59,130,246,.08)',fill:true,borderWidth:2,pointRadius:0,tension:.4},
      {label:'Processed',data:proc,borderColor:'#10B981',backgroundColor:'transparent',fill:false,borderWidth:2,borderDash:[5,5],pointRadius:0,tension:.4},
    ]},
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:true,position:'bottom',labels:{color:'#9CA3AF',font:{size:11},boxWidth:12,padding:16}}},scales:{x:{grid:{display:false},ticks:{color:'#6B7280',font:{size:11}}},y:{grid:{color:'rgba(45,55,72,.3)'},ticks:{color:'#6B7280',font:{size:11}},beginAtZero:true}}}
  });

  // Cluster chart
  const clusterLabels=clusters.map(c=>{
    const words=c.name.split(' ');
    return words.length>2?words.slice(0,2).join(' '):c.name;
  });
  new Chart(document.getElementById('clusterChart'),{
    type:'bar',
    data:{labels:clusterLabels,datasets:[{data:clusters.map(c=>c.count),backgroundColor:clusters.map((_,i)=>clusterColors[i%clusterColors.length]),borderRadius:4,barThickness:18}]},
    options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{display:false},ticks:{color:'#6B7280',font:{size:11}},beginAtZero:true},y:{grid:{display:false},ticks:{color:'#9CA3AF',font:{size:11},padding:4}}}}
  });
}

// ===== RANKINGS =====
function renderRankings(){
  const q=document.getElementById('searchInput').value.toLowerCase();
  const cf=document.getElementById('clusterFilter').value;
  const sf=document.getElementById('statusFilter').value;
  const sb=document.getElementById('sortBy').value;
  let list=[...candidates];
  if(q) list=list.filter(c=>c.name.toLowerCase().includes(q)||c.title.toLowerCase().includes(q)||c.skills.some(s=>s.name.toLowerCase().includes(q)));
  if(cf!=='all') list=list.filter(c=>c.cluster===cf);
  if(sf!=='all') list=list.filter(c=>c.status===sf);
  if(sb==='rank') list.sort((a,b)=>a.rank-b.rank);
  else if(sb==='technical') list.sort((a,b)=>b.scores.technical-a.scores.technical);
  else if(sb==='experience') list.sort((a,b)=>b.scores.experience-a.scores.experience);
  else if(sb==='name') list.sort((a,b)=>a.name.localeCompare(b.name));

  document.getElementById('filterCount').textContent=`Showing ${list.length} of ${candidates.length} candidates`;
  let html='';
  list.slice(0,30).forEach(c=>{
    const rc=c.rank<=3?"#FBBF24":"#6B7280";
    const scoreColor=(v)=>v>=75?"#10B981":v>=55?"#F9FAFB":"#EF4444";
    html+=`<tr onclick="openCandidate(${c.id})">
      <td style="font-weight:700;color:${rc}">#${c.rank}</td>
      <td><div class="cand-name">${c.name}</div><div class="cand-title">${c.title}</div></td>
      <td>${scoreBadge(c.topsis)}</td>
      <td style="color:${scoreColor(c.scores.technical)}">${c.scores.technical.toFixed(0)}</td>
      <td style="color:${scoreColor(c.scores.experience)}">${c.scores.experience.toFixed(0)}</td>
      <td style="color:${scoreColor(c.scores.education)}">${c.scores.education.toFixed(0)}</td>
      <td style="color:${scoreColor(c.scores.softSkills)}">${c.scores.softSkills.toFixed(0)}</td>
      <td style="color:${scoreColor(c.scores.culturalFit)}">${c.scores.culturalFit.toFixed(0)}</td>
      <td style="font-size:12px;color:#9CA3AF">${c.yoe}y</td>
      <td style="font-size:11px;color:#9CA3AF">${c.cluster.split(' ').slice(0,2).join(' ')}</td>
      <td>${statusBadge(c.status)}</td>
    </tr>`;
  });
  document.getElementById('rankings-body').innerHTML=html;
}

function initClusterFilter(){
  const sel=document.getElementById('clusterFilter');
  clusters.forEach(c=>{const o=document.createElement('option');o.value=c.name;o.textContent=c.name;sel.appendChild(o);});
}

// ===== CLUSTERS =====
function renderClusters(){
  let html='';
  clusters.forEach((cl,i)=>{
    const cands=candidates.filter(c=>c.cluster===cl.name);
    const avgT=(cands.reduce((s,c)=>s+c.topsis,0)/cands.length).toFixed(3);
    const avgTech=(cands.reduce((s,c)=>s+c.scores.technical,0)/cands.length).toFixed(0);
    const avgExp=(cands.reduce((s,c)=>s+c.scores.experience,0)/cands.length).toFixed(0);
    const clr=clusterColors[i%clusterColors.length];
    let topHtml='';
    cands.slice(0,3).forEach(c=>{
      topHtml+=`<div class="cluster-cand-row" onclick="openCandidate(${c.id})"><span style="color:var(--text)">${c.name}</span>${scoreBadge(c.topsis)}</div>`;
    });
    html+=`<div class="cluster-card">
      <div class="glow-circle" style="background:${clr}"></div>
      <div class="cluster-header">
        <div><div style="font-size:16px;font-weight:600;margin-bottom:2px">${cl.name}</div><div style="font-size:12px;color:var(--text-dim)">${cl.count} candidates</div></div>
        <div class="cluster-count-badge" style="background:${clr}20;color:${clr}">${cl.count}</div>
      </div>
      <div style="margin-bottom:12px"><div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px"><span style="color:var(--text-dim)">Avg TOPSIS</span><span style="color:${clr};font-weight:600">${avgT}</span></div>${miniBar(avgT*100,clr)}</div>
      <div style="margin-bottom:12px"><div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px"><span style="color:var(--text-dim)">Avg Technical</span><span style="color:var(--text-muted)">${avgTech}</span></div>${miniBar(avgTech,"#3B82F6")}</div>
      <div><div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px"><span style="color:var(--text-dim)">Avg Experience</span><span style="color:var(--text-muted)">${avgExp}</span></div>${miniBar(avgExp,"#8B5CF6")}</div>
      <div class="cluster-candidates"><div class="label">Top Candidates</div>${topHtml}</div>
    </div>`;
  });
  document.getElementById('cluster-grid').innerHTML=html;
}

// ===== BENCHMARK =====
let radarChartInst=null;
function initBenchmarkCharts(){
  if(radarChartInst) radarChartInst.destroy();
  radarChartInst=new Chart(document.getElementById('radarChart'),{
    type:'radar',
    data:{
      labels:competitorData.map(d=>d.dim),
      datasets:[
        {label:'Your Pipeline',data:competitorData.map(d=>d.yours),borderColor:'#3B82F6',backgroundColor:'rgba(59,130,246,.15)',borderWidth:2,pointRadius:3,pointBackgroundColor:'#3B82F6'},
        {label:'Competitor',data:competitorData.map(d=>d.comp),borderColor:'#EF4444',backgroundColor:'rgba(239,68,68,.08)',borderWidth:2,borderDash:[5,5],pointRadius:3,pointBackgroundColor:'#EF4444'},
      ]
    },
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{r:{grid:{color:'#2D3748'},angleLines:{color:'#2D3748'},ticks:{display:false},suggestedMin:0,suggestedMax:100,pointLabels:{color:'#9CA3AF',font:{size:12}}}}}
  });
  radarChartInst.resize();

  // Gap analysis
  let gapHTML='';
  competitorData.forEach(d=>{
    const gap=d.yours-d.comp;
    const color=gap>=0?"#10B981":"#EF4444";
    gapHTML+=`<div class="gap-item">
      <div class="gap-header"><span class="dim" style="color:var(--text)">${d.dim}</span><span class="pts" style="color:${color}">${gap>=0?'+':''}${gap} pts</span></div>
      <div class="gap-bar-wrap"><div class="gap-bar-fill" style="width:${d.yours}%;background:#3B82F6"></div><div class="gap-bar-marker" style="width:${d.comp}%"></div></div>
      <div class="gap-labels"><span style="color:#3B82F6">You: ${d.yours}</span><span style="color:#EF4444">Comp: ${d.comp}</span></div>
    </div>`;
  });
  document.getElementById('gap-analysis').innerHTML=gapHTML;

  const aboveBench=candidates.filter(c=>c.topsis>.75).length;
  document.getElementById('bench-stats').innerHTML=
    statCardHTML("MARKET POSITION INDEX","74/100","Above average","#10B981")+
    statCardHTML("TALENT GAP","-2.3%","Narrowing (was -5.1%)","#F59E0B")+
    statCardHTML("ABOVE BENCHMARK",aboveBench,`of ${candidates.length} total`,"#3B82F6");
}

// ===== AHP =====
const ahpWeights={technical:35,experience:25,education:15,softSkills:15,culturalFit:10};
const ahpCriteria=[
  {key:"technical",label:"Technical Skills",icon:"⚡"},
  {key:"experience",label:"Experience",icon:"📊"},
  {key:"education",label:"Education",icon:"🎓"},
  {key:"softSkills",label:"Soft Skills",icon:"🤝"},
  {key:"culturalFit",label:"Cultural Fit",icon:"🎯"},
];
function renderAHP(){
  let html='';
  ahpCriteria.forEach(c=>{
    html+=`<div class="ahp-slider-card"><div class="top"><span class="label">${c.icon} ${c.label}</span><span class="pct" id="ahp-val-${c.key}">${ahpWeights[c.key]}%</span></div><input type="range" min="0" max="60" value="${ahpWeights[c.key]}" oninput="updateAHP('${c.key}',this.value)"></div>`;
  });
  document.getElementById('ahp-sliders').innerHTML=html;
  updateAHPTotal();
}
function updateAHP(key,val){
  ahpWeights[key]=parseInt(val);
  document.getElementById('ahp-val-'+key).textContent=val+'%';
  updateAHPTotal();
}
function updateAHPTotal(){
  const total=Object.values(ahpWeights).reduce((s,v)=>s+v,0);
  const el=document.getElementById('ahp-total');
  el.textContent='Total: '+total+'%';
  el.style.background=Math.abs(total-100)<2?'rgba(16,185,129,.12)':'rgba(239,68,68,.12)';
  el.style.color=Math.abs(total-100)<2?'#10B981':'#EF4444';
}

// ===== CANDIDATE MODAL =====
let modalRadarInst=null;
function openCandidate(id){
  const c=candidates.find(x=>x.id===id);
  if(!c) return;
  document.getElementById('modal-avatar').textContent=c.name.split(' ').map(n=>n[0]).join('');
  document.getElementById('modal-name').textContent=c.name;
  document.getElementById('modal-title').textContent=c.title;
  document.getElementById('modal-stats').innerHTML=
    statCardHTML("TOPSIS Score",c.topsis.toFixed(3),"","#10B981")+
    statCardHTML("Rank","#"+c.rank,`of ${candidates.length} candidates`,"#FBBF24")+
    statCardHTML("Experience",c.yoe+" yrs","","#3B82F6")+
    statCardHTML("Cluster",c.cluster.split(' ')[0],c.cluster,"#8B5CF6");

  // Radar
  if(modalRadarInst) modalRadarInst.destroy();
  modalRadarInst=new Chart(document.getElementById('modalRadar'),{
    type:'radar',
    data:{
      labels:["Technical","Experience","Education","Soft Skills","Cultural Fit"],
      datasets:[{data:[c.scores.technical,c.scores.experience,c.scores.education,c.scores.softSkills,c.scores.culturalFit],borderColor:'#3B82F6',backgroundColor:'rgba(59,130,246,.2)',borderWidth:2,pointRadius:3,pointBackgroundColor:'#3B82F6'}]
    },
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{r:{grid:{color:'#2D3748'},angleLines:{color:'#2D3748'},ticks:{display:false},suggestedMin:0,suggestedMax:100,pointLabels:{color:'#9CA3AF',font:{size:11}}}}}
  });

  // Dimensions
  const dims=[["technical","Technical"],["experience","Experience"],["education","Education"],["softSkills","Soft Skills"],["culturalFit","Cultural Fit"]];
  let dimHTML='';
  dims.forEach(([k,l])=>{
    const v=c.scores[k];
    const clr=v>=75?"#10B981":v>=55?"#F59E0B":"#EF4444";
    dimHTML+=`<div style="margin-bottom:12px"><div style="display:flex;justify-content:space-between;margin-bottom:4px"><span style="font-size:12px;color:var(--text-muted)">${l}</span><span style="font-size:12px;font-weight:600">${v.toFixed(1)}/100</span></div>${miniBar(v,clr)}</div>`;
  });
  document.getElementById('modal-dimensions').innerHTML=dimHTML;

  // Skills
  let skillsHTML='';
  c.skills.forEach(s=>{
    const bg=s.prof>=4?'rgba(16,185,129,.1)':s.prof>=3?'rgba(59,130,246,.1)':'#1F2937';
    const clr=s.prof>=4?'#10B981':s.prof>=3?'#3B82F6':'#9CA3AF';
    const bdr=s.prof>=4?'rgba(16,185,129,.2)':s.prof>=3?'rgba(59,130,246,.2)':'#2D3748';
    skillsHTML+=`<span class="skill-tag" style="background:${bg};color:${clr};border:1px solid ${bdr}">${s.name} <span style="opacity:.7">(${s.prof.toFixed(1)})</span></span>`;
  });
  document.getElementById('modal-skills').innerHTML=skillsHTML;

  // Details
  const details=[
    {l:"Location",v:c.location},{l:"Education",v:c.education},{l:"Previous Company",v:c.prevCo},
    {l:"GitHub Stars",v:c.ghStars},{l:"LinkedIn Connections",v:c.liConn},{l:"Applied",v:c.applied},
  ];
  document.getElementById('modal-details').innerHTML=details.map(d=>`<div class="detail-item"><div class="dlabel">${d.l}</div><div class="dvalue">${d.v}</div></div>`).join('');

  document.getElementById('candidateModal').classList.add('open');
}
function closeModal(e){if(e.target===e.currentTarget) document.getElementById('candidateModal').classList.remove('open');}

// ===== UPLOAD MODAL =====
let uploadStep=0;
const uploadStepLabels=["Upload Resume","Add Links","Processing","Complete"];
function openUpload(){uploadStep=0;renderUpload();document.getElementById('uploadModal').classList.add('open');}
function closeUpload(e){if(e.target===e.currentTarget) document.getElementById('uploadModal').classList.remove('open');}
function renderUpload(){
  document.getElementById('upload-steps').innerHTML=uploadStepLabels.map((l,i)=>`<div class="upload-step"><div class="bar ${i<=uploadStep?'active':''}"></div><div class="slabel ${i<=uploadStep?'active':''}">${l}</div></div>`).join('');
  let html='';
  if(uploadStep===0){
    html=`<div class="drop-zone" onclick="uploadStep=1;renderUpload()"><div style="font-size:40px;margin-bottom:8px">📄</div><div style="margin-bottom:4px">Drop resume here or click to upload</div><div style="color:var(--text-dim);font-size:12px">PDF, DOCX, or TXT (max 10MB)</div></div>`;
  } else if(uploadStep===1){
    html=`
      <div class="upload-field"><label>🔗 LinkedIn Profile URL</label><input placeholder="https://linkedin.com/in/..."></div>
      <div class="upload-field"><label>🐙 GitHub Username</label><input placeholder="github-username"></div>
      <div class="upload-field"><label>📁 Portfolio / Project URL</label><input placeholder="https://..."></div>
      <button class="btn-process" onclick="uploadStep=2;renderUpload()">Process Candidate →</button>`;
  } else if(uploadStep===2){
    html=`<div style="text-align:center;padding:20px"><div class="spinner">⚙️</div><div style="margin-top:12px">Processing candidate data...</div><div style="color:var(--text-dim);font-size:13px;margin-top:4px">Running NLP extraction, K-Means classification, and TOPSIS scoring</div><div style="margin:20px auto;width:80%;height:6px;background:var(--surface-light);border-radius:3px;overflow:hidden"><div class="loading-bar" style="width:60%;height:100%;background:var(--primary);border-radius:3px"></div></div><button class="btn-skip" onclick="uploadStep=3;renderUpload()">Skip (Demo)</button></div>`;
  } else {
    html=`<div style="text-align:center;padding:20px"><div style="font-size:48px;margin-bottom:12px">✅</div><div style="color:var(--accent);font-size:18px;font-weight:600;margin-bottom:8px">Candidate Processed!</div><div style="color:var(--text-muted);font-size:13px;margin-bottom:20px">TOPSIS Score: 0.847 | Cluster: Senior Full-Stack | Rank: #3</div><button class="btn-view" onclick="document.getElementById('uploadModal').classList.remove('open')">View in Dashboard</button></div>`;
  }
  document.getElementById('upload-content').innerHTML=html;
}

// ===== INIT =====
renderDashboard();
initClusterFilter();
renderRankings();
renderClusters();
renderAHP();
</script>
</body>
</html>
