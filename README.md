# github.io<!DOCTYPE html>
<html lang="hi">
<head>
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-TLFHVV2V7M"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
      gtag('config', 'G-TLFHVV2V7M', { 'send_page_view': true, 'transport_type': 'xhr', 'debug_mode': true });
    </script>

    <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-1744546374560361"
     crossorigin="anonymous"></script>

    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Worker App - Malegaon</title>
    
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <link rel="icon" href="https://cdn-icons-png.flaticon.com/512/3135/3135715.png">
    <link rel="manifest" href="manifest.json">

    <script src="https://www.gstatic.com/firebasejs/9.15.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.15.0/firebase-database-compat.js"></script>

    <style>
        :root { --primary: #2563eb; --primary-light: #eff6ff; --success: #10b981; --success-light: #ecfdf5; --danger: #ef4444; --danger-light: #fef2f2; --bg: #f8fafc; --text-dark: #0f172a; --text-muted: #64748b; }
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; background: var(--bg); margin: 0; padding-bottom: 90px; overflow-x: hidden; color: var(--text-dark); -webkit-tap-highlight-color: transparent; }
        #installBanner { background: linear-gradient(135deg, #10b981, #059669); color: white; padding: 14px 16px; font-weight: 600; font-size: 14px; display: none; justify-content: space-between; align-items: center; box-shadow: 0 4px 12px rgba(16, 185, 129, 0.2); z-index: 1001; position: relative; border-radius: 0 0 12px 12px; margin: 0 8px; }
        .header { background: #0f172a; color: white; padding: 16px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 1000; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
        .header h1 { margin: 0; font-size: 20px; font-weight: 700; letter-spacing: 0.5px; }
        .container { padding: 16px; max-width: 600px; margin: auto; }
        
        .worker-card { background: #ffffff; border-radius: 16px; padding: 16px; margin-bottom: 14px; display: flex; align-items: center; box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03); border-left: 6px solid var(--success); position: relative; transition: transform 0.2s, box-shadow 0.2s; }
        .worker-card:active { transform: scale(0.99); }
        .worker-card.busy { border-left-color: var(--danger); background: #fdfdfd; }
        .profile-img { width: 68px; height: 68px; border-radius: 12px; object-fit: cover; margin-right: 16px; background: #f1f5f9; border: 1px solid #e2e8f0; }
        .worker-info { flex-grow: 1; display: flex; flex-direction: column; gap: 4px; }
        .worker-info b { font-size: 17px; color: var(--text-dark); font-weight: 600; }
        .worker-info small { font-size: 13px; color: var(--text-muted); font-weight: 500; }
        
        .loc-badge { background: #f1f5f9; color: #475569; font-size: 11px; padding: 4px 8px; border-radius: 6px; font-weight: 600; display: inline-block; width: fit-content; }
        .shift-badge-din { background: #fef9c3; color: #713f12; font-size: 11px; padding: 4px 8px; border-radius: 6px; font-weight: 600; display: inline-block; width: fit-content; }
        .shift-badge-raat { background: #1e293b; color: #f8fafc; font-size: 11px; padding: 4px 8px; border-radius: 6px; font-weight: 600; display: inline-block; width: fit-content; }
        .rate-badge { background: #fffbeb; color: #b45309; border: 1px solid #fde68a; font-size: 11px; padding: 4px 8px; border-radius: 6px; font-weight: 700; display: inline-block; width: fit-content; }
        
        .call-action-btn { background: linear-gradient(135deg, #10b981, #059669); color: white; padding: 10px 16px; font-size: 14px; border-radius: 10px; font-weight: 700; box-shadow: 0 4px 6px rgba(16, 185, 129, 0.15); transition: 0.2s; border: none; }
        .call-action-btn:active { transform: scale(0.95); }
        .rate-action-btn { background: #fffbeb; color: #b45309; border: 1px solid #fde68a; padding: 10px 14px; font-size: 14px; border-radius: 10px; font-weight: 700; transition: 0.2s; cursor: pointer; }
        .rate-action-btn:active { transform: scale(0.95); background: #fef3c7; }
        .busy-status-text { color: var(--danger); font-weight: 700; font-size: 13px; letter-spacing: 0.5px; background: var(--danger-light); padding: 6px 12px; border-radius: 8px; }
        
        .side-menu { position: fixed; top: 0; width: 88%; max-width: 380px; height: 100%; background: white; z-index: 2000; transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1); padding: 20px; box-sizing: border-box; overflow-y: auto; box-shadow: 0 0 30px rgba(0,0,0,0.15); }
        .left-menu { left: 0; transform: translateX(-105%); border-right: 6px solid var(--primary); }
        .right-menu { right: 0; transform: translateX(105%); border-left: 6px solid var(--success); }
        .side-menu.open-left { transform: translateX(0); }
        .side-menu.open-right { transform: translateX(0); }
        .overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(15, 23, 42, 0.4); backdrop-filter: blur(4px); display: none; z-index: 1500; }
        
        /* Modal for Rating */
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); backdrop-filter: blur(4px); display: none; z-index: 4000; align-items: center; justify-content: center; }
        .modal-box { background: white; padding: 24px; border-radius: 16px; width: 85%; max-width: 320px; text-align: center; box-shadow: 0 10px 25px rgba(0,0,0,0.2); }
        .star-container { font-size: 38px; margin: 16px 0; user-select: none; }
        .star-container span { cursor: pointer; color: #cbd5e1; transition: 0.2s; display: inline-block; }
        
        input, select, textarea { width: 100%; padding: 12px; margin-bottom: 12px; border: 1px solid #cbd5e1; border-radius: 10px; font-size: 14px; font-weight: 500; box-sizing: border-box; background: #f8fafc; color: var(--text-dark); transition: border-color 0.2s; }
        input:focus, select:focus { border-color: var(--primary); outline: none; background: #fff; }
        button { cursor: pointer; border: none; border-radius: 10px; font-weight: 600; padding: 12px; transition: 0.2s; }
        .admin-btn { background: var(--primary); color: white; width: 100%; margin-bottom: 12px; box-shadow: 0 4px 6px rgba(37, 99, 235, 0.15); }
        
        .filter-box select { padding: 12px; border: 2px solid #e2e8f0; font-weight: 600; font-size: 14px; border-radius: 12px; margin-bottom: 10px; width: 100%; background-color: #fff; }
        .filter-box select:focus { border-color: var(--primary); }
        .filter-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }

        .rate-table { width: 100%; font-size: 12px; border-collapse: collapse; margin-top: 14px; background: white; border-radius: 8px; overflow: hidden; }
        .rate-table th { background: #f1f5f9; padding: 10px 6px; border: 1px solid #e2e8f0; color: #475569; font-weight: 700; }
        .rate-table td { padding: 8px 6px; border: 1px solid #e2e8f0; text-align: center; }
        .rate-in { width: 48px; padding: 6px 4px; border: 1px solid #cbd5e1; text-align: center; border-radius: 6px; font-size: 11px; margin-bottom: 0; }
        
        .photo-preview { width: 100%; height: 130px; background: #f1f5f9; border: 2px dashed #cbd5e1; border-radius: 12px; margin-bottom: 12px; display: flex; align-items: center; justify-content: center; overflow: hidden; color: var(--text-muted); font-size: 14px; font-weight: 500; }
        .photo-preview img { width: 100%; height: 100%; object-fit: cover; }
        
        .busy-btn { position: fixed; bottom: 24px; right: 96px; background: linear-gradient(135deg, #ef4444, #dc2626); color: white; width: 60px; height: 60px; border-radius: 50%; font-size: 24px; box-shadow: 0 4px 14px rgba(239, 68, 68, 0.4); z-index: 900; display: flex; align-items: center; justify-content: center; }
        .chat-btn { position: fixed; bottom: 24px; right: 24px; background: linear-gradient(135deg, #2563eb, #1d4ed8); color: white; width: 60px; height: 60px; border-radius: 50%; font-size: 24px; box-shadow: 0 4px 14px rgba(37, 99, 235, 0.4); z-index: 900; display: flex; align-items: center; justify-content: center; }
        
        #busyNotification { display: none; position: fixed; top: 60px; left: 0; width: 100%; background: var(--danger); color: white; padding: 14px; z-index: 1050; text-align: center; font-weight: 700; box-shadow: 0 4px 15px rgba(0,0,0,0.15); box-sizing: border-box; font-size: 15px; }
        
        #chatPage { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: var(--bg); z-index: 3000; display: none; flex-direction: column; }
        .chat-header { background: #0f172a; color: white; padding: 16px; display: flex; justify-content: space-between; align-items: center; }
        #chatBox { flex-grow: 1; padding: 16px; overflow-y: auto; display: flex; flex-direction: column; gap: 12px; }
        .msg { max-width: 80%; padding: 12px 16px; border-radius: 14px; background: white; box-shadow: 0 2px 4px rgba(0,0,0,0.04); font-size: 15px; line-height: 1.4; }
        .chat-input-area { background: white; padding: 14px; display: flex; gap: 10px; align-items: center; box-shadow: 0 -2px 10px rgba(0,0,0,0.05); }
        .chip-container { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 16px; }
        .chip { background: #e2e8f0; color: #334155; padding: 6px 10px; border-radius: 6px; font-size: 13px; font-weight: 500; display: flex; align-items: center; gap: 4px; }
    </style>
</head>
<body>

<div id="installBanner">
    <span onclick="installApp()" style="flex-grow:1; cursor:pointer;">📲 App Phone Mein Install Karein</span>
    <button onclick="document.getElementById('installBanner').style.display='none'" style="padding:4px 8px; background:white; color:black; border-radius: 4px;">X</button>
</div>

<div class="header">
    <button onclick="toggleMenu('left')" style="background:none; color:white; font-size:22px; padding:0;">⚙️</button>
    <h1>Worker App</h1>
    <button onclick="toggleMenu('right')" style="background:none; color:white; font-size:22px; padding:0;">👤</button>
</div>

<div id="busyNotification">
    ⚠️ Aap BUSY hain: <span id="timerDisplay">00:00:00</span> 
    <button onclick="setFree()" style="background:white; color:var(--danger); padding:6px 12px; margin-left:12px; font-size:12px; border-radius:6px; font-weight:bold; border:none; cursor:pointer;">FREE NOW</button>
</div>

<div class="overlay" id="overlay" onclick="closeMenus()"></div>

<div class="modal-overlay" id="ratingModal">
    <div class="modal-box">
        <h3 id="rateWorkerName" style="margin-top:0; color:var(--text-dark);">Worker ko Rate Karein</h3>
        <p style="font-size:13px; color:var(--text-muted); margin-top:-5px;">Apna experience share karein</p>
        
        <div class="star-container" id="starSelector">
            <span onclick="selectStars(1)">★</span>
            <span onclick="selectStars(2)">★</span>
            <span onclick="selectStars(3)">★</span>
            <span onclick="selectStars(4)">★</span>
            <span onclick="selectStars(5)">★</span>
        </div>
        
        <div style="display:flex; gap:10px; margin-top:10px;">
            <button onclick="closeRatingModal()" style="background:#e2e8f0; color:#334155; flex:1;">Cancel</button>
            <button onclick="submitWorkerRating()" style="background:var(--primary); color:white; flex:1;">Submit</button>
        </div>
    </div>
</div>

<div class="side-menu left-menu" id="leftMenu">
    <h3 style="margin-top:0;">⚙️ Admin / Setting</h3>
    <div id="adminLoginArea">
        <input type="password" id="adminPass" placeholder="Admin Password">
        <button onclick="checkAdmin()" class="admin-btn">Login</button>
    </div>
    <div id="adminPanel" style="display:none;">
        <button onclick="logoutAdmin()" style="background:var(--danger); color:white; width:100%; margin-bottom:14px;">Logout</button>
        
        <div style="background:#e0f2fe; padding:12px; border-radius:10px; margin-bottom:14px; text-align:center; border: 1px solid #bae6fd;">
            <b style="font-size:15px; color:#0369a1;">👥 Total Profiles: <span id="totalWorkerCount" style="font-size:18px;">0</span></b>
        </div>
        
        <div style="background:#f1f5f9; padding:14px; border-radius:12px; margin-bottom:16px; border:1px solid #e2e8f0;">
            <b style="color:var(--primary); font-size:15px;">➕ Add New Worker</b>
            <input type="text" id="admName" placeholder="Worker ka Naam" style="margin-top:10px;">
            <input type="number" id="admPhone" placeholder="Mobile Number">
            <select id="admLoc"></select>
            <select id="admCat"></select>
            <select id="admShift"><option value="Din">☀️ Din (Day Shift)</option><option value="Raat">🌙 Raat (Night Shift)</option></select>
            <button onclick="adminSaveWorker()" style="background:var(--success); color:white; width:100%;">SAVE TO LIST</button>
        </div>

        <hr style="border:0; border-top:1px solid #e2e8f0; margin:16px 0;">
        <b>📁 Categories</b>
        <div style="display:flex; gap:6px; margin-bottom:12px; margin-top:8px;">
            <input type="text" id="newCat" placeholder="Nayi Category" style="margin-bottom:0;">
            <button onclick="addCat()" style="background:var(--success); color:white; padding:0 16px;">Add</button>
        </div>
        <div id="catEditor" class="chip-container"></div>
        
        <hr style="border:0; border-top:1px solid #e2e8f0; margin:16px 0;">
        <b>📍 Address / Locations</b>
        <div style="display:flex; gap:6px; margin-bottom:12px; margin-top:8px;">
            <input type="text" id="newLoc" placeholder="Naya Address" style="margin-bottom:0;">
            <button onclick="addLoc()" style="background:var(--success); color:white; padding:0 16px;">Add</button>
        </div>
        <div id="locEditor" class="chip-container"></div>
    </div>
    
    <div style="background:#fff; padding:12px; border-radius:12px; margin-top:16px; border:1px solid #e2e8f0;">
        <h4 style="margin-top:0; margin-bottom:10px;">📒 Judai Rate Chart (Din)</h4>
        <table class="rate-table">
            <thead><tr><th>Taag</th><th>Cott</th><th>C-Ang</th><th>PC</th><th>P-Ang</th></tr></thead>
            <tbody id="rateBody"></tbody>
        </table>
        <button id="saveRateBtn" onclick="saveRates()" class="admin-btn" style="display:none; margin-top:12px; background:var(--success);">Save Rates</button>
    </div>
</div>

<div class="side-menu right-menu" id="rightMenu">
    <h3 style="margin-top:0;">📝 My Profile</h3>
    <div id="workerProfileUI">
        <div class="photo-preview" onclick="document.getElementById('photoInput').click()">
            <span id="photoText">📷 Select Photo (Optional)</span>
            <img id="previewImg" style="display:none;">
        </div>
        <input type="file" id="photoInput" accept="image/*" style="display:none;" onchange="previewPhoto(this)">
        <input type="text" id="wName" placeholder="Aapka Naam">
        <input type="number" id="wPhone" placeholder="Mobile Number">
        <select id="wLoc"></select>
        <select id="wCat"></select>
        <select id="wShift"><option value="Din">☀️ Din (Day Shift)</option><option value="Raat">🌙 Raat (Night Shift)</option></select>
        <button onclick="saveProfile()" style="background:var(--success); color:white; width:100%; padding:14px; font-size:15px; margin-bottom:16px;">SAVE MY PROFILE</button>
    </div>
    
    <div id="myStatusArea" style="display:none; background:#f1f5f9; padding:14px; border-radius:12px; border:1px solid #e2e8f0; margin-top:12px;">
        <b style="font-size:14px;">🔴 Set Busy Timer:</b>
        <div style="display:flex; gap:6px; margin-top:10px;">
            <select id="busyHours" style="margin-bottom:0;">
                <option value="2.5">2.5h</option><option value="5">5h</option>
                <option value="12">12h</option><option value="24">24h</option><option value="999">Holiday</option>
            </select>
            <button onclick="setBusy()" style="background:var(--danger); color:white; padding:0 20px;">Set</button>
        </div>
        <button onclick="setFree()" style="background:var(--success); color:white; width:100%; margin-top:12px;">🟢 I am FREE Now</button>
    </div>
</div>

<div class="container">
    <div class="filter-box">
        <select id="catFilter" onchange="fetchWorkers()"><option value="All">All Categories</option></select>
        <div class="filter-row">
            <select id="locFilter" onchange="fetchWorkers()"><option value="All">All Areas</option></select>
            <select id="shiftFilter" onchange="fetchWorkers()">
                <option value="All">All Shifts</option><option value="Din">☀️ Din (Day)</option><option value="Raat">🌙 Raat (Night)</option>
            </select>
        </div>
    </div>
    <div id="workerList" style="margin-top:10px;">Loading...</div>
</div>

<button class="busy-btn" onclick="directBusy()">🔴</button>
<button class="chat-btn" onclick="openChat()">💬</button>

<div id="chatPage">
    <div class="chat-header">
        <h3 style="margin:0; font-size:18px;">Community Chat</h3>
        <div>
            <button id="adminClearChat" onclick="clearAllChat()" style="display:none; background:var(--danger); color:white; padding:6px 12px; border-radius:6px; margin-right:10px; font-weight:bold;">Clear All</button>
            <button onclick="closeChat()" style="background:none; color:white; font-size:18px;">❌</button>
        </div>
    </div>
    <div id="chatBox"></div>
    <div class="chat-input-area">
        <input type="text" id="chatMsg" placeholder="Message..." style="margin:0;">
        <button onclick="sendChatMsg()" style="background:var(--primary); color:white; padding:0 20px; height:45px;">Send</button>
    </div>
</div>

<script>
    if ('serviceWorker' in navigator) { navigator.serviceWorker.register('sw.js').then(() => console.log("SW Registered")); }

    const firebaseConfig = { apiKey: "AIzaSyAi-eed_fIWaj3GQVleMzD7Fz4ppS_voxQ", databaseURL: "https://gathni-app-default-rtdb.firebaseio.com" };
    firebase.initializeApp(firebaseConfig); const db = firebase.database();

    let base64Photo = "", myWorkerId = localStorage.getItem('my_worker_id'), isAdmin = localStorage.getItem('admin_auth') === 'true';

    let deferredPrompt;
    window.addEventListener('beforeinstallprompt', (e) => { e.preventDefault(); deferredPrompt = e; document.getElementById('installBanner').style.display = 'flex'; });
    async function installApp() { if (deferredPrompt) { deferredPrompt.prompt(); deferredPrompt = null; } }

    let rBody = "";
    for(let t=2000; t<=3400; t+=100) rBody += `<tr><td><b>${t}</b></td><td><input type="number" id="c${t}" class="rate-in" disabled></td><td><input type="number" id="ca${t}" class="rate-in" disabled></td><td><input type="number" id="p${t}" class="rate-in" disabled></td><td><input type="number" id="pa${t}" class="rate-in" disabled></td></tr>`;
    document.getElementById('rateBody').innerHTML = rBody;

    function checkAdmin() { if(document.getElementById('adminPass').value === 'malegaongathni.app') { localStorage.setItem('admin_auth', 'true'); location.reload(); } else { alert("Wrong!"); } }
    function logoutAdmin() { localStorage.removeItem('admin_auth'); location.reload(); }
    if(isAdmin) { 
        document.getElementById('adminLoginArea').style.display = 'none'; 
        document.getElementById('adminPanel').style.display = 'block'; 
        document.getElementById('saveRateBtn').style.display = 'block'; 
        document.querySelectorAll('.rate-in').forEach(i => i.disabled = false); 
    }

    let activeRatingId = "";
    let selectedStarCount = 5;

    function openRatingModal(id, name) {
        activeRatingId = id;
        document.getElementById('rateWorkerName').innerText = name + " ko Rate Karein";
        selectStars(5); 
        document.getElementById('ratingModal').style.display = 'flex';
    }

    function closeRatingModal() {
        document.getElementById('ratingModal').style.display = 'none';
        activeRatingId = "";
    }

    function selectStars(count) {
        selectedStarCount = count;
        let stars = document.getElementById('starSelector').children;
        for(let i=0; i<5; i++) {
            if(i < count) {
                stars[i].innerText = "★";
                stars[i].style.color = "#f59e0b"; 
            } else {
                stars[i].innerText = "☆";
                stars[i].style.color = "#cbd5e1"; 
            }
        }
    }

    function submitWorkerRating() {
        if(!activeRatingId) return;
        
        let ref = db.ref('workers/' + activeRatingId);
        ref.transaction((worker) => {
            if (worker) {
                worker.totalStars = (worker.totalStars || 0) + selectedStarCount;
                worker.ratingCount = (worker.ratingCount || 0) + 1;
            }
            return worker;
        }, (error, committed) => {
            if (committed) {
                alert("Aapki rating save ho gayi! Shukriya.");
                closeRatingModal();
            } else {
                alert("Kuch problem aayi, baad mein try karein.");
            }
        });
    }

    function saveRates() {
        let ratesData = {};
        for(let t=2000; t<=3400; t+=100) {
            ratesData[t] = {
                c: document.getElementById('c'+t).value || "", ca: document.getElementById('ca'+t).value || "",
                p: document.getElementById('p'+t).value || "", pa: document.getElementById('pa'+t).value || ""
            };
        }
        db.ref('rates').set(ratesData).then(() => { alert("✅ Rate Chart Save Ho Gaya!"); });
    }

    function loadRates() {
        db.ref('rates').once('value', snap => {
            let data = snap.val();
            if(!data) return;
            for(let t=2000; t<=3400; t+=100) {
                if(data[t]) {
                    if(document.getElementById('c'+t)) document.getElementById('c'+t).value = data[t].c || "";
                    if(document.getElementById('ca'+t)) document.getElementById('ca'+t).value = data[t].ca || "";
                    if(document.getElementById('p'+t)) document.getElementById('p'+t).value = data[t].p || "";
                    if(document.getElementById('pa'+t)) document.getElementById('pa'+t).value = data[t].pa || "";
                }
            }
        });
    }

    function toggleMenu(s) { 
        if(s==='left') document.getElementById('leftMenu').classList.add('open-left'); 
        if(s==='right') document.getElementById('rightMenu').classList.add('open-right'); 
        document.getElementById('overlay').style.display = 'block'; 
        if(s==='left') loadRates(); 
        if(s==='right' && myWorkerId) { document.getElementById('myStatusArea').style.display = 'block'; } 
    }
    function closeMenus() { document.getElementById('leftMenu').classList.remove('open-left'); document.getElementById('rightMenu').classList.remove('open-right'); document.getElementById('overlay').style.display = 'none'; }
    function previewPhoto(input) { if (input.files && input.files[0]) { let r = new FileReader(); r.onload = function(e) { document.getElementById('previewImg').src = e.target.result; document.getElementById('previewImg').style.display = 'block'; document.getElementById('photoText').style.display = 'none'; base64Photo = e.target.result; }; r.readAsDataURL(input.files[0]); } }

    db.ref('categories').on('value', snap => { 
        let cats = snap.val() || { "default": "Gathni / Taag Jodne" }; 
        let opt = "", optF = "<option value='All'>All Categories</option>", ed = ""; 
        for(let id in cats) {
            opt += `<option value="${cats[id]}">${cats[id]}</option>`; 
            optF += `<option value="${cats[id]}">${cats[id]}</option>`;
            ed += `<span class="chip">${cats[id]} <b onclick="db.ref('categories/${id}').remove()" style="color:red; cursor:pointer;">❌</b></span>`;
        }
        document.getElementById('wCat').innerHTML = opt; document.getElementById('admCat').innerHTML = opt; document.getElementById('catFilter').innerHTML = optF; 
        if(isAdmin) document.getElementById('catEditor').innerHTML = ed; fetchWorkers(); 
    });
    function addCat() { let v = document.getElementById('newCat').value; if(v) { db.ref('categories').push(v); document.getElementById('newCat').value=""; } }

    db.ref('locations').on('value', snap => { 
        let locs = snap.val() || { "default": "All Over Malegaon" }; 
        let opt = "", optF = "<option value='All'>All Areas</option>", ed = ""; 
        for(let id in locs) {
            opt += `<option value="${locs[id]}">${locs[id]}</option>`; 
            optF += `<option value="${locs[id]}">${locs[id]}</option>`;
            ed += `<span class="chip">${locs[id]} <b onclick="db.ref('locations/${id}').remove()" style="color:red; cursor:pointer;">❌</b></span>`;
        }
        document.getElementById('wLoc').innerHTML = opt; document.getElementById('admLoc').innerHTML = opt; document.getElementById('locFilter').innerHTML = optF; 
        if(isAdmin) document.getElementById('locEditor').innerHTML = ed; fetchWorkers(); 
    });
    function addLoc() { let v = document.getElementById('newLoc').value; if(v) { db.ref('locations').push(v); document.getElementById('newLoc').value=""; } }

    function adminSaveWorker() {
        let name = document.getElementById('admName').value, phone = document.getElementById('admPhone').value, loc = document.getElementById('admLoc').value, cat = document.getElementById('admCat').value, shift = document.getElementById('admShift').value;
        if(!name || !phone) return alert("Details bhariye!");
        db.ref('workers').push({ name: name, phone: phone, loc: loc, cat: cat, shift: shift, photo: "", calls: 0, busyUntil: Date.now(), totalStars: 0, ratingCount: 0 }).then(() => { alert(name + " add ho gaya!"); document.getElementById('admName').value = ""; document.getElementById('admPhone').value = ""; });
    }

    function saveProfile() {
        let data = { name: document.getElementById('wName').value, phone: document.getElementById('wPhone').value, loc: document.getElementById('wLoc').value, cat: document.getElementById('wCat').value, shift: document.getElementById('wShift').value, photo: base64Photo, calls: 0 };
        if(!data.name || !data.phone) return alert("Zaruri hai!");
        
        if(myWorkerId) { 
            db.ref('workers/'+myWorkerId).update(data).then(()=>alert("Updated!")); 
        } else { 
            data.busyUntil = Date.now();
            data.totalStars = 0; data.ratingCount = 0;
            let ref = db.ref('workers').push(); 
            ref.set(data).then(()=>{ localStorage.setItem('my_worker_id', ref.key); location.reload(); }); 
        }
    }

    function fetchWorkers() { 
        let fc = document.getElementById('catFilter').value, fl = document.getElementById('locFilter').value, fs = document.getElementById('shiftFilter').value; 
        db.ref('workers').on('value', snap => { 
            let list = document.getElementById('workerList'), d = snap.val(); 
            if(isAdmin && document.getElementById('totalWorkerCount')) { document.getElementById('totalWorkerCount').innerText = d ? Object.keys(d).length : 0; }
            list.innerHTML = ""; if(!d) return; 
            
            let freeWorkers = []; let busyWorkers = [];
            for(let id in d) { 
                let w = d[id]; w.id = id; let workerShift = w.shift || "Din"; 
                if((fc==='All' || w.cat===fc) && (fl==='All' || w.loc===fl) && (fs==='All' || workerShift===fs)) {
                    if (w.busyUntil > Date.now()) { busyWorkers.push(w); } else { freeWorkers.push(w); }
                }
            }
            
            freeWorkers.sort((a, b) => (a.busyUntil || 0) - (b.busyUntil || 0));
            busyWorkers.sort((a, b) => (a.busyUntil || 0) - (b.busyUntil || 0));

            let htmlString = "";
            function createCardDesign(w, isBusy) {
                let shiftBadge = w.shift === 'Raat' ? '<span class="shift-badge-raat">🌙 Raat</span>' : '<span class="shift-badge-din">☀️ Din</span>';
                
                let avgRating = w.ratingCount > 0 ? (w.totalStars / w.ratingCount).toFixed(1) : "New";
                let ratingDisplay = w.ratingCount > 0 ? `⭐ ${avgRating} (${w.ratingCount})` : `⭐ New`;

                return `<div class="worker-card ${isBusy?'busy':''}">
                    <img src="${w.photo || 'https://cdn-icons-png.flaticon.com/512/3135/3135715.png'}" class="profile-img">
                    <div class="worker-info">
                        <b>${w.name}</b>
                        <small>${w.cat}</small>
                        <div style="display:flex; gap:4px; flex-wrap:wrap; margin-top:2px;">
                            <span class="loc-badge">${w.loc}</span>
                            ${shiftBadge}
                            <span class="rate-badge">${ratingDisplay}</span>
                        </div>
                    </div>
                    <div style="display:flex; flex-direction:column; gap:5px; align-items: center;">
                        ${!isBusy ? `
                            <div style="display:flex; gap: 5px;">
                                <button class="call-action-btn" onclick="makeCall('${w.id}','${w.phone}')">📞 Call</button>
                                <button class="rate-action-btn" onclick="openRatingModal('${w.id}', '${w.name}')">⭐</button>
                            </div>
                        ` : '<span class="busy-status-text">BUSY</span>'}
                        ${isAdmin ? `<button onclick="db.ref('workers/${w.id}').remove()" style="background:var(--danger); color:white; padding:4px 8px; font-size:10px; border-radius:6px; margin-top:4px;">Del</button>` : ''}
                    </div>
                </div>`;
            }
            freeWorkers.forEach(w => { htmlString += createCardDesign(w, false); });
            busyWorkers.forEach(w => { htmlString += createCardDesign(w, true); });
            list.innerHTML = htmlString;
        }); 
    }

    function makeCall(id, ph) { 
        if(typeof gtag === 'function') gtag('event', 'worker_call', { 'worker_id': id });
        db.ref('workers/'+id+'/calls').transaction(c => (c||0)+1, (error, committed, snapshot) => {
            if (committed && snapshot.val() >= 3) db.ref('workers/'+id).update({ busyUntil: Date.now() + (2.5 * 3600000), calls: 0 }); 
        });
        window.location.href = "tel:"+ph; 
    }
    
    function directBusy() {
        if(!myWorkerId) return alert("Pehle profile banayein!");
        if(confirm("Kya aap 2.5 ghante ke liye BUSY hona chahte hain?")) db.ref('workers/' + myWorkerId).update({ busyUntil: Date.now() + (2.5 * 3600000), calls: 0 }).then(() => alert("Aap 2.5 ghante ke liye BUSY ho gaye hain!"));
    }

    function setBusy() { let h = parseFloat(document.getElementById('busyHours').value), u = h===999?9999999999999:Date.now()+(h*3600000); db.ref('workers/'+myWorkerId).update({busyUntil:u, calls:0}); }
    function setFree() { db.ref('workers/'+myWorkerId).update({busyUntil: Date.now(), calls:0}); }

    function openChat() { document.getElementById('chatPage').style.display='flex'; loadChat(); if(isAdmin) document.getElementById('adminClearChat').style.display='inline-block'; if(typeof gtag === 'function') gtag('event', 'screen_view', { 'screen_name': 'Community Chat' }); }
    function closeChat() { document.getElementById('chatPage').style.display='none'; }
    function loadChat() { 
        db.ref('chat').on('value', snap => { 
            let b = document.getElementById('chatBox'), d = snap.val(); b.innerHTML = ""; if(!d) return; 
            for(let k in d) { 
                let m = d[k]; let del = (isAdmin || (m.senderId && m.senderId === myWorkerId)) ? `<span onclick="deleteMsg('${k}')" style="float:right; color:red; cursor:pointer; font-size:14px; margin-left:10px;">🗑️</span>` : ""; 
                b.innerHTML += `<div class="msg" style="margin-bottom:8px; align-self: ${m.senderId === myWorkerId ? 'flex-end' : 'flex-start'}; background: ${m.senderId === myWorkerId ? '#dbeafe' : 'white'};">${m.text} ${del}</div>`; 
            } 
            b.scrollTop = b.scrollHeight; 
        }); 
    }
    function sendChatMsg() { let t = document.getElementById('chatMsg').value; if(t) { db.ref('chat').push({text:t, senderId: myWorkerId || 'admin'}); document.getElementById('chatMsg').value=""; } }
    function deleteMsg(k) { if(confirm("Apna message delete karein?")) db.ref('chat/'+k).remove(); }
    function clearAllChat() { if(confirm("Saare messages delete karein?")) db.ref('chat').remove(); }

    setInterval(() => {
        if(!myWorkerId) return;
        db.ref('workers/'+myWorkerId).once('value', snap => {
            let d = snap.val(), n = document.getElementById('busyNotification');
            if(d && d.busyUntil > Date.now()) {
                n.style.display = 'block';
                let diff = d.busyUntil - Date.now();
                document.getElementById('timerDisplay').innerText = Math.floor(diff/3600000) + "h " + Math.floor((diff%3600000)/60000) + "m";
            } else { n.style.display = 'none'; }
        });
    }, 5000);
</script>
</body>
</html>
