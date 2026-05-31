<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Worker App - Malegaon</title>
    
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
    
    <style>
        /* General Styles */
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; margin: 0; padding: 0; background-color: #f8fafc; overflow-x: hidden; }
        * { box-sizing: border-box; }
        
        /* Header */
        header { background-color: #0f172a; color: white; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 50; }
        .header-title { font-size: 18px; font-weight: bold; border-bottom: 2px solid white; padding-bottom: 2px; }
        .menu-icon { font-size: 24px; cursor: pointer; color: #60a5fa; }
        
        /* Filters */
        .filters-container { padding: 15px; display: flex; flex-direction: column; gap: 10px; background: white; border-bottom: 1px solid #e2e8f0; }
        select, input { padding: 10px; border: 1px solid #cbd5e1; border-radius: 6px; width: 100%; font-size: 14px; background: #f8fafc; outline: none; }
        
        /* Worker List & Cards */
        #workerList { padding: 10px; padding-bottom: 90px; }
        .worker-card { background: white; margin-bottom: 12px; padding: 12px; border-radius: 8px; border-left: 5px solid #10b981; display: flex; gap: 12px; align-items: center; box-shadow: 0 2px 4px rgba(0,0,0,0.05); }
        .worker-card.busy { border-left-color: #ef4444; opacity: 0.85; background: #fef2f2; }
        .profile-img { width: 60px; height: 60px; border-radius: 50%; object-fit: cover; background: #e2e8f0; }
        .worker-info { flex: 1; min-width: 0; }
        .worker-info b { font-size: 16px; color: #1e293b; display: block; margin-bottom: 2px; }
        .worker-info small { color: #64748b; font-size: 13px; display: block; margin-bottom: 6px; }
        
        /* Badges */
        .badge-container { display: flex; flex-wrap: wrap; gap: 5px; }
        .badge { font-size: 11px; padding: 3px 8px; border-radius: 12px; display: inline-block; font-weight: 500; }
        .badge-loc { background: #f1f5f9; color: #475569; }
        .badge-shift-din { background: #fef08a; color: #854d0e; }
        .badge-shift-raat { background: #1e293b; color: white; }
        .badge-rate { background: #fef3c7; color: #b45309; }
        
        /* Action Buttons on Card */
        .card-actions { display: flex; flex-direction: column; gap: 6px; min-width: 75px; align-items: center; justify-content: center; }
        .action-btn { width: 100%; border: none; padding: 7px 5px; border-radius: 6px; font-size: 12px; font-weight: 600; color: white; cursor: pointer; text-align: center; }
        .call-btn { background-color: #3b82f6; }
        .rate-btn { background-color: #f59e0b; }
        .busy-text { color: #ef4444; font-weight: 700; font-size: 14px; }
        
        /* Floating Action Buttons */
        .fab-container { position: fixed; bottom: 20px; right: 20px; display: flex; gap: 12px; z-index: 40; }
        .fab { width: 55px; height: 55px; border-radius: 50%; border: none; font-size: 24px; cursor: pointer; display: flex; justify-content: center; align-items: center; box-shadow: 0 4px 10px rgba(0,0,0,0.25); color: white; }
        .fab-busy { background-color: #ef4444; }
        .fab-chat { background-color: #2563eb; }

        /* Rating Modal */
        #ratingModal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); justify-content: center; align-items: center; z-index: 100; }
        .modal-box { background: white; padding: 25px; border-radius: 12px; width: 85%; max-width: 350px; text-align: center; }
        .star-container { font-size: 35px; margin: 15px 0; display: flex; justify-content: center; gap: 5px; }
        .star-container span { cursor: pointer; color: #f59e0b; }
        .modal-btns { display: flex; gap: 10px; margin-top: 20px; }
        .modal-btns button { flex: 1; padding: 10px; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; }
        .save-btn { background: #10b981; color: white; }
        .cancel-btn { background: #e2e8f0; color: #475569; }

        /* Side Menus */
        .side-menu { position: fixed; top: 0; width: 80%; max-width: 300px; height: 100%; background: white; z-index: 60; transition: 0.3s; padding: 20px; overflow-y: auto; box-shadow: 0 0 15px rgba(0,0,0,0.2); }
        .left-menu { left: -100%; }
        .right-menu { right: -100%; }
        .open-left { left: 0; }
        .open-right { right: 0; }
        #overlay { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); z-index: 55; }
        
        .menu-title { font-size: 18px; font-weight: bold; border-bottom: 2px solid #e2e8f0; padding-bottom: 10px; margin-bottom: 15px; }
        .form-group { margin-bottom: 15px; }
        .form-group label { display: block; font-size: 13px; color: #64748b; margin-bottom: 5px; }
        .btn-full { width: 100%; padding: 12px; background: #0f172a; color: white; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; margin-top: 10px; }
        
        /* Chat Box */
        #chatPage { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: white; z-index: 80; flex-direction: column; }
        .chat-header { background: #0f172a; color: white; padding: 15px; display: flex; justify-content: space-between; align-items: center; }
        #chatBox { flex: 1; padding: 15px; overflow-y: auto; background: #f1f5f9; display: flex; flex-direction: column; gap: 8px; }
        .msg { padding: 10px 15px; border-radius: 15px; max-width: 80%; font-size: 14px; box-shadow: 0 1px 2px rgba(0,0,0,0.05); }
        .chat-input-area { padding: 10px; background: white; border-top: 1px solid #e2e8f0; display: flex; gap: 10px; }
        .chat-input-area input { flex: 1; padding: 12px; border: 1px solid #cbd5e1; border-radius: 20px; outline: none; }
        .chat-input-area button { background: #2563eb; color: white; border: none; width: 45px; height: 45px; border-radius: 50%; font-size: 18px; display: flex; align-items: center; justify-content: center; cursor: pointer; }
    </style>
</head>
<body>

    <header>
        <div class="menu-icon" onclick="toggleMenu('left')">⚙️</div>
        <div class="header-title">Worker App</div>
        <div class="menu-icon" onclick="toggleMenu('right')">👤</div>
    </header>

    <div class="filters-container">
        <select id="catFilter" onchange="fetchWorkers()">
            <option value="All">All Categories</option>
        </select>
        <div style="display: flex; gap: 10px;">
            <select id="locFilter" onchange="fetchWorkers()" style="flex: 1;">
                <option value="All">All Areas</option>
            </select>
            <select id="shiftFilter" onchange="fetchWorkers()" style="flex: 1;">
                <option value="All">All Shifts</option>
                <option value="Din">☀️ Din</option>
                <option value="Raat">🌙 Raat</option>
            </select>
        </div>
    </div>

    <div id="busyNotification" style="display:none; background:#fee2e2; color:#ef4444; padding:10px; text-align:center; font-weight:bold; font-size:14px;">
        Aap Busy mode mein hain. Time left: <span id="timerDisplay"></span>
        <button onclick="setFree()" style="margin-left:10px; padding:3px 8px; border:none; background:#ef4444; color:white; border-radius:4px;">Set Free</button>
    </div>

    <div id="workerList">
        <div style="text-align:center; padding:20px; color:#64748b;">Loading workers...</div>
    </div>

    <div class="fab-container">
        <button class="fab fab-busy" onclick="directBusy()">🔴</button>
        <button class="fab fab-chat" onclick="openChat()">💬</button>
    </div>

    <div id="ratingModal">
        <div class="modal-box">
            <h3 id="rateWorkerName" style="margin-top:0;">Rate Worker</h3>
            <p style="font-size:13px; color:#64748b;">Apna anubhav batayein (1 se 5 star)</p>
            <div class="star-container" id="starSelector">
                <span onclick="selectStars(1)">★</span>
                <span onclick="selectStars(2)">★</span>
                <span onclick="selectStars(3)">★</span>
                <span onclick="selectStars(4)">★</span>
                <span onclick="selectStars(5)">★</span>
            </div>
            <div class="modal-btns">
                <button class="cancel-btn" onclick="closeRatingModal()">Cancel</button>
                <button class="save-btn" onclick="submitWorkerRating()">Save Rating</button>
            </div>
        </div>
    </div>

    <div id="overlay" onclick="closeMenus()"></div>

    <div id="leftMenu" class="side-menu left-menu">
        <div class="menu-title">Admin Panel <span style="float:right; cursor:pointer;" onclick="closeMenus()">✖</span></div>
        
        <div id="adminLoginArea">
            <p style="font-size:13px;">Sirf Admin ke liye</p>
            <input type="password" id="adminPass" placeholder="Admin Password" style="margin-bottom:10px;">
            <button class="btn-full" onclick="checkAdmin()">Login</button>
        </div>

        <div id="adminPanel" style="display:none;">
            <button class="btn-full" onclick="logoutAdmin()" style="background:#ef4444; margin-bottom:15px;">Logout Admin</button>
            
            <div class="form-group">
                <label>Add New Category</label>
                <div style="display:flex; gap:5px;">
                    <input type="text" id="newCat" placeholder="Eg: Taag Jodne">
                    <button onclick="addCat()" style="padding:10px; background:#10b981; color:white; border:none; border-radius:6px;">Add</button>
                </div>
                <div id="catEditor" style="margin-top:10px; font-size:12px; display:flex; flex-wrap:wrap; gap:5px;"></div>
            </div>

            <div class="form-group">
                <label>Add New Area/Location</label>
                <div style="display:flex; gap:5px;">
                    <input type="text" id="newLoc" placeholder="Eg: 60 Futi">
                    <button onclick="addLoc()" style="padding:10px; background:#10b981; color:white; border:none; border-radius:6px;">Add</button>
                </div>
                <div id="locEditor" style="margin-top:10px; font-size:12px; display:flex; flex-wrap:wrap; gap:5px;"></div>
            </div>

            <div style="border-top:1px solid #e2e8f0; margin-top:20px; padding-top:15px;">
                <b>Add Worker (Admin)</b>
                <input type="text" id="admName" placeholder="Worker Name" style="margin-top:10px;">
                <input type="number" id="admPhone" placeholder="Phone Number (No +91)" style="margin-top:10px;">
                <select id="admCat" style="margin-top:10px;"></select>
                <select id="admLoc" style="margin-top:10px;"></select>
                <select id="admShift" style="margin-top:10px;">
                    <option value="Din">Din</option>
                    <option value="Raat">Raat</option>
                </select>
                <button class="btn-full" onclick="adminSaveWorker()" style="background:#3b82f6;">Save Worker</button>
            </div>
        </div>
    </div>

    <div id="rightMenu" class="side-menu right-menu">
        <div class="menu-title">My Profile <span style="float:right; cursor:pointer;" onclick="closeMenus()">✖</span></div>
        
        <div class="form-group">
            <label>Full Name</label>
            <input type="text" id="wName" placeholder="Aapka Naam">
        </div>
        <div class="form-group">
            <label>Phone Number</label>
            <input type="number" id="wPhone" placeholder="Mobile Number">
        </div>
        <div class="form-group">
            <label>Select Category</label>
            <select id="wCat"></select>
        </div>
        <div class="form-group">
            <label>Select Area</label>
            <select id="wLoc"></select>
        </div>
        <div class="form-group">
            <label>Duty Shift</label>
            <select id="wShift">
                <option value="Din">Din</option>
                <option value="Raat">Raat</option>
            </select>
        </div>
        <button class="btn-full" onclick="saveProfile()" style="background:#10b981;">Save Profile</button>
    </div>

    <div id="chatPage">
        <div class="chat-header">
            <span style="font-weight:bold; font-size:18px;">Community Chat</span>
            <div>
                <button id="adminClearChat" onclick="clearAllChat()" style="display:none; background:red; color:white; border:none; padding:5px 10px; border-radius:4px; margin-right:10px;">Clear All</button>
                <span style="font-size:24px; cursor:pointer;" onclick="closeChat()">✖</span>
            </div>
        </div>
        <div id="chatBox"></div>
        <div class="chat-input-area">
            <input type="text" id="chatMsg" placeholder="Type message..." autocomplete="off">
            <button onclick="sendChatMsg()">➤</button>
        </div>
    </div>

    <script>
        // Firebase Configuration (Aapki Same keys)
        const firebaseConfig = { 
            apiKey: "AIzaSyAi-eed_fIWaj3GQVleMzD7Fz4ppS_voxQ", 
            databaseURL: "https://gathni-app-default-rtdb.firebaseio.com" 
        };
        if (!firebase.apps.length) { firebase.initializeApp(firebaseConfig); }
        const db = firebase.database();

        let myWorkerId = localStorage.getItem('my_worker_id');
        let isAdmin = localStorage.getItem('admin_auth') === 'true';
        let base64Photo = "";
        let activeRatingId = "";
        let selectedStarCount = 5;

        // Admin checks
        function checkAdmin() { 
            if(document.getElementById('adminPass').value === 'malegaongathni.app') { 
                localStorage.setItem('admin_auth', 'true'); location.reload(); 
            } else { alert("Wrong Password!"); } 
        }
        function logoutAdmin() { localStorage.removeItem('admin_auth'); location.reload(); }
        
        if(isAdmin) { 
            document.getElementById('adminLoginArea').style.display = 'none'; 
            document.getElementById('adminPanel').style.display = 'block'; 
        }

        // Side Menus Logic
        function toggleMenu(side) { 
            if(side === 'left') document.getElementById('leftMenu').classList.add('open-left'); 
            if(side === 'right') {
                document.getElementById('rightMenu').classList.add('open-right');
                loadMyProfile(); 
            }
            document.getElementById('overlay').style.display = 'block'; 
        }
        function closeMenus() { 
            document.getElementById('leftMenu').classList.remove('open-left'); 
            document.getElementById('rightMenu').classList.remove('open-right'); 
            document.getElementById('overlay').style.display = 'none'; 
        }

        // Load Categories & Locations dynamically from Firebase
        db.ref('categories').on('value', snap => { 
            let cats = snap.val() || { "default": "Gathni / Taag Jodne" }; 
            let opt = "", optF = "<option value='All'>All Categories</option>", ed = ""; 
            for(let id in cats) {
                opt += `<option value="${cats[id]}">${cats[id]}</option>`; 
                optF += `<option value="${cats[id]}">${cats[id]}</option>`;
                ed += `<span style="background:#f1f5f9; padding:4px 8px; border-radius:4px;">${cats[id]} <b onclick="db.ref('categories/${id}').remove()" style="color:red; cursor:pointer; margin-left:5px;">✖</b></span>`;
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
                ed += `<span style="background:#f1f5f9; padding:4px 8px; border-radius:4px;">${locs[id]} <b onclick="db.ref('locations/${id}').remove()" style="color:red; cursor:pointer; margin-left:5px;">✖</b></span>`;
            }
            document.getElementById('wLoc').innerHTML = opt; document.getElementById('admLoc').innerHTML = opt; document.getElementById('locFilter').innerHTML = optF; 
            if(isAdmin) document.getElementById('locEditor').innerHTML = ed; fetchWorkers(); 
        });
        function addLoc() { let v = document.getElementById('newLoc').value; if(v) { db.ref('locations').push(v); document.getElementById('newLoc').value=""; } }

        // Fetch & Display Workers
        function fetchWorkers() { 
            let fc = document.getElementById('catFilter').value, fl = document.getElementById('locFilter').value, fs = document.getElementById('shiftFilter').value; 
            db.ref('workers').on('value', snap => { 
                let list = document.getElementById('workerList'), d = snap.val(); 
                list.innerHTML = ""; if(!d) { list.innerHTML = "<div style='text-align:center; padding:20px;'>Koi worker nahi mila.</div>"; return; }
                
                let freeWorkers = []; let busyWorkers = [];
                for(let id in d) { 
                    let w = d[id]; w.id = id; let workerShift = w.shift || "Din"; 
                    if((fc==='All' || w.cat===fc) && (fl==='All' || w.loc===fl) && (fs==='All' || workerShift===fs)) {
                        if (w.busyUntil > Date.now()) { busyWorkers.push(w); } else { freeWorkers.push(w); }
                    }
                }
                
                // Sort by time
                freeWorkers.sort((a, b) => (a.busyUntil || 0) - (b.busyUntil || 0));
                busyWorkers.sort((a, b) => (a.busyUntil || 0) - (b.busyUntil || 0));

                let htmlString = "";
                function createCardDesign(w, isBusy) {
                    let shiftBadge = w.shift === 'Raat' ? '<span class="badge badge-shift-raat">🌙 Raat</span>' : '<span class="badge badge-shift-din">☀️ Din</span>';
                    let avgRating = w.ratingCount > 0 ? (w.totalStars / w.ratingCount).toFixed(1) : "New";
                    let ratingDisplay = w.ratingCount > 0 ? `⭐ ${avgRating} (${w.ratingCount})` : `⭐ New`;

                    return `<div class="worker-card ${isBusy?'busy':''}">
                        <img src="${w.photo || 'https://cdn-icons-png.flaticon.com/512/3135/3135715.png'}" class="profile-img">
                        <div class="worker-info">
                            <b>${w.name}</b>
                            <small>${w.cat}</small>
                            <div class="badge-container">
                                <span class="badge badge-loc">${w.loc}</span>
                                ${shiftBadge}
                                <span class="badge badge-rate">${ratingDisplay}</span>
                            </div>
                        </div>
                        <div class="card-actions">
                            ${!isBusy ? `
                                <button class="action-btn call-btn" onclick="makeCall('${w.id}','${w.phone}')">📞 Call</button>
                                <button class="action-btn rate-btn" onclick="openRatingModal('${w.id}', '${w.name}')">⭐ Rate</button>
                            ` : '<span class="busy-text">BUSY</span>'}
                            ${isAdmin ? `<button onclick="db.ref('workers/${w.id}').remove()" style="background:#ef4444; color:white; border:none; padding:4px 8px; font-size:10px; border-radius:4px; width:100%; margin-top:5px; cursor:pointer;">Delete</button>` : ''}
                        </div>
                    </div>`;
                }
                freeWorkers.forEach(w => { htmlString += createCardDesign(w, false); });
                busyWorkers.forEach(w => { htmlString += createCardDesign(w, true); });
                list.innerHTML = htmlString || "<div style='text-align:center; padding:20px;'>Is filter mein koi worker nahi mila.</div>";
            }); 
        }

        // Profile Logic
        function loadMyProfile() {
            if(myWorkerId) {
                db.ref('workers/' + myWorkerId).once('value', snap => {
                    let d = snap.val();
                    if(d) {
                        document.getElementById('wName').value = d.name || "";
                        document.getElementById('wPhone').value = d.phone || "";
                        document.getElementById('wLoc').value = d.loc || "";
                        document.getElementById('wCat').value = d.cat || "";
                        document.getElementById('wShift').value = d.shift || "Din";
                    }
                });
            }
        }

        function saveProfile() {
            let data = { name: document.getElementById('wName').value, phone: document.getElementById('wPhone').value, loc: document.getElementById('wLoc').value, cat: document.getElementById('wCat').value, shift: document.getElementById('wShift').value, photo: base64Photo, calls: 0 };
            if(!data.name || !data.phone) return alert("Naam aur Number zaruri hai!");
            
            if(myWorkerId) { 
                db.ref('workers/'+myWorkerId).update(data).then(()=> { alert("Profile Update Ho Gayi!"); closeMenus(); }); 
            } else { 
                data.busyUntil = Date.now(); data.totalStars = 0; data.ratingCount = 0;
                let ref = db.ref('workers').push(); 
                ref.set(data).then(()=>{ localStorage.setItem('my_worker_id', ref.key); alert("Profile Ban Gayi!"); closeMenus(); location.reload(); }); 
            }
        }

        function adminSaveWorker() {
            let name = document.getElementById('admName').value, phone = document.getElementById('admPhone').value, loc = document.getElementById('admLoc').value, cat = document.getElementById('admCat').value, shift = document.getElementById('admShift').value;
            if(!name || !phone) return alert("Details bhariye!");
            db.ref('workers').push({ name: name, phone: phone, loc: loc, cat: cat, shift: shift, photo: "", calls: 0, busyUntil: Date.now(), totalStars: 0, ratingCount: 0 }).then(() => { alert(name + " add ho gaya!"); document.getElementById('admName').value = ""; document.getElementById('admPhone').value = ""; });
        }

        // Call & Busy System (3 Call = 2.5 Hrs Busy)
        function makeCall(id, ph) { 
            let workerRef = db.ref('workers/' + id);
            workerRef.child('calls').transaction(function(currentCalls) {
                return (currentCalls || 0) + 1;
            }, function(error, committed, snapshot) {
                if (committed) {
                    let totalCalls = snapshot.val();
                    if (totalCalls >= 3) {
                        workerRef.update({ busyUntil: Date.now() + (2.5 * 3600000), calls: 0 }); // 2.5 hours busy
                    }
                }
            });
            window.location.href = "tel:" + ph; 
        }
        
        function directBusy() {
            if(!myWorkerId) return alert("Pehle right side se apni Profile banayein!");
            if(confirm("Kya aap 2.5 ghante ke liye BUSY hona chahte hain?")) {
                db.ref('workers/' + myWorkerId).update({ busyUntil: Date.now() + (2.5 * 3600000), calls: 0 }).then(() => alert("Aap 2.5 ghante ke liye BUSY ho gaye hain!"));
            }
        }
        function setFree() { if(myWorkerId) db.ref('workers/'+myWorkerId).update({busyUntil: Date.now(), calls:0}); }

        // Busy Notification Checker
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

        // Rating System (1 Device = 1 Rating)
        function openRatingModal(id, name) {
            if (localStorage.getItem('rated_' + id)) { alert("Aap is worker ko pehle hi rate kar chuke hain!"); return; }
            activeRatingId = id;
            document.getElementById('rateWorkerName').innerText = name;
            selectStars(5); 
            document.getElementById('ratingModal').style.display = 'flex';
        }
        function closeRatingModal() { document.getElementById('ratingModal').style.display = 'none'; activeRatingId = ""; }
        function selectStars(count) {
            selectedStarCount = count;
            let stars = document.getElementById('starSelector').children;
            for(let i=0; i<5; i++) {
                if(i < count) { stars[i].innerText = "★"; stars[i].style.color = "#f59e0b"; } 
                else { stars[i].innerText = "☆"; stars[i].style.color = "#cbd5e1"; }
            }
        }
        function submitWorkerRating() {
            if(!activeRatingId) return;
            let ref = db.ref('workers/' + activeRatingId);
            ref.transaction((worker) => {
                if (worker) { worker.totalStars = (worker.totalStars || 0) + selectedStarCount; worker.ratingCount = (worker.ratingCount || 0) + 1; }
                return worker;
            }, (error, committed) => {
                if (committed) {
                    localStorage.setItem('rated_' + activeRatingId, 'true'); // Save in device
                    alert("Aapki rating save ho gayi! Shukriya.");
                    closeRatingModal();
                } else { alert("Kuch problem aayi, baad mein try karein."); }
            });
        }

        // Chat System
        function openChat() { document.getElementById('chatPage').style.display='flex'; loadChat(); if(isAdmin) document.getElementById('adminClearChat').style.display='inline-block'; }
        function closeChat() { document.getElementById('chatPage').style.display='none'; }
        function loadChat() { 
            db.ref('chat').on('value', snap => { 
                let b = document.getElementById('chatBox'), d = snap.val(); b.innerHTML = ""; if(!d) return; 
                for(let k in d) { 
                    let m = d[k]; let del = (isAdmin || (m.senderId && m.senderId === myWorkerId)) ? `<span onclick="deleteMsg('${k}')" style="float:right; color:red; cursor:pointer; font-size:12px; margin-left:10px;">✖</span>` : ""; 
                    b.innerHTML += `<div class="msg" style="align-self: ${m.senderId === myWorkerId ? 'flex-end' : 'flex-start'}; background: ${m.senderId === myWorkerId ? '#dbeafe' : 'white'};">${m.text} ${del}</div>`; 
                } 
                b.scrollTop = b.scrollHeight; 
            }); 
        }
        function sendChatMsg() { let t = document.getElementById('chatMsg').value; if(t) { db.ref('chat').push({text:t, senderId: myWorkerId || 'admin'}); document.getElementById('chatMsg').value=""; } }
        function deleteMsg(k) { if(confirm("Message delete karein?")) db.ref('chat/'+k).remove(); }
        function clearAllChat() { if(confirm("Saare messages delete karein?")) db.ref('chat').remove(); }

    </script>
</body>
</html>
