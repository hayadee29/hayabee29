<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>hayabee29 - Sync Edition (Multi-Year)</title>
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">

    <style>
        :root { --primary-blue: #0078d4; --light-blue: #e1f5fe; --accent-red: #ff5252; --text-dark: #333; --bg-gray: #f9f9f9; }
        body { font-family: 'Segoe UI', sans-serif; margin: 0; padding: 0 0 50px 0; background: var(--bg-gray); color: var(--text-dark); }
        header { background: linear-gradient(135deg, #0078d4, #00b0ff); color: white; padding: 25px 10px; text-align: center; position: sticky; top: 0; z-index: 100; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        h1 { margin: 0; font-size: 26px; letter-spacing: 2px; }
        .container { max-width: 500px; margin: 20px auto; padding: 0 10px; }
        .card { background: white; border-radius: 15px; box-shadow: 0 10px 25px rgba(0,0,0,0.05); padding: 20px; margin-bottom: 20px; }
        h2 { border-left: 5px solid var(--primary-blue); padding-left: 15px; color: var(--primary-blue); margin-top: 0; margin-bottom: 20px; font-size: 18px; }
        .clock-container { background: var(--light-blue); padding: 15px; border-radius: 10px; text-align: center; margin-bottom: 15px; }
        .clock-time { font-size: 28px; font-weight: bold; color: var(--primary-blue); }
        .schedule-card { background: #fffde7; border-left: 5px solid #ffd600; padding: 15px; margin-bottom: 20px; border-radius: 5px; font-size: 14px; }
        
        /* カレンダー調整 */
        .calendar-header { display: flex; justify-content: center; align-items: center; gap: 20px; margin-bottom: 10px; }
        .cal-btn { background: var(--primary-blue); color: white; border: none; padding: 5px 15px; border-radius: 5px; cursor: pointer; font-weight: bold; }
        .calendar-table { width: 100%; border-collapse: separate; border-spacing: 4px; }
        .calendar-table th { font-size: 12px; color: #888; }
        .calendar-table td { height: 55px; background: #fff; border: 1px solid #eee; border-radius: 8px; vertical-align: top; padding: 5px; font-size: 12px; text-align: center; cursor: pointer; position: relative; }
        .calendar-table td.today { border: 2px solid var(--primary-blue); background: #e3f2fd; }
        
        .dot { width: 8px; height: 8px; border-radius: 50%; display: inline-block; margin: 1px; }
        .dot-school { background: var(--accent-red); }
        .dot-train { background: #00e676; }
        .item-row { display: flex; justify-content: space-between; align-items: center; padding: 12px 0; border-bottom: 1px dotted #eee; }
        input[type="date"], select, input[type="password"], input[type="text"] { border: 1px solid #ddd; padding: 8px; border-radius: 6px; font-size: 14px; }
        input[type="date"] { width: 55%; }
        .btn-blue { background: var(--primary-blue); color: white; border: none; padding: 12px; border-radius: 8px; cursor: pointer; width: 100%; margin-top: 10px; font-weight: bold; }
        .soji-highlight { border: 2px solid var(--accent-red); background: #fff5f5; padding: 10px; border-radius: 10px; margin-bottom: 10px; }
        .url-card { border: 1px solid #eee; padding: 10px; border-radius: 8px; margin-bottom: 8px; display: flex; justify-content: space-between; align-items: center; }
        .url-card a { color: var(--primary-blue); text-decoration: none; font-weight: bold; }
        .year-display-badge { background: rgba(255,255,255,0.2); padding: 4px 10px; border-radius: 12px; font-size: 14px; display: inline-block; margin-top: 5px; }
    </style>
</head>
<body>

    <header>
        <h1>hayabee29</h1>
        <div id="header-year-badge" class="year-display-badge">表示中: ----年</div>
    </header>

    <div class="container">
        <!-- ホーム / カレンダーセクション -->
        <div class="card">
            <h2>ホーム・カレンダー</h2>
            <div class="clock-container">
                <div id="current-date">----年--月--日</div>
                <div id="current-time" class="clock-time">00:00:00</div>
            </div>
            <div class="schedule-card"><strong>選択日の予定:</strong> <span id="today-schedule">なし</span></div>
            
            <div class="calendar-header">
                <button class="cal-btn" onclick="changeMonth(-1)">◀</button>
                <h3 id="cal-month-title" style="margin:0; min-width:120px; text-align:center; font-size: 16px;"></h3>
                <button class="cal-btn" onclick="changeMonth(1)">▶</button>
            </div>

            <table class="calendar-table">
                <thead><tr><th>日</th><th>月</th><th>火</th><th>水</th><th>木</th><th>金</th><th>土</th></tr></thead>
                <tbody id="calendar-body"></tbody>
            </table>
        </div>

        <!-- 学校関係セクション -->
        <div class="card">
            <h2>学校関係</h2>
            <div class="soji-highlight">🧹 掃除当番 <input type="date" class="save-date" id="date-掃除当番"></div>
            <div class="item-row"><span>レポート提出日</span><input type="date" class="save-date" id="date-レポート提出日"></div>
            <div class="item-row"><span>プリント提出日</span><input type="date" class="save-date" id="date-プリント提出日"></div>
            <div class="item-row"><span>4時間授業日</span><input type="date" class="save-date" id="date-4時間授業日"></div>
            <div class="item-row"><span>40分授業日</span><input type="date" class="save-date" id="date-40分授業日"></div>
            <div class="item-row"><span>30分授業日</span><input type="date" class="save-date" id="date-30分授業日"></div>
            <div class="item-row" style="background:#fff3e0; border-radius:5px; padding: 8px;"><span>🍱 弁当いらない日</span><input type="date" class="save-date" id="date-弁当いらない日"></div>
        </div>

        <!-- 電車関係セクション -->
        <div class="card">
            <h2>電車関係</h2>
            <div class="item-row"><span>臨時列車</span><input type="date" class="save-date" id="date-臨時列車"></div>
            <div class="item-row"><span>一人旅</span><input type="date" class="save-date" id="date-一人旅"></div>
            <div class="item-row"><span>二人旅</span><input type="date" class="save-date" id="date-二人旅"></div>
        </div>

        <!-- URLリストセクション -->
        <div class="card">
            <h2>URLリスト</h2>
            <div id="url-list-container"></div>
            <div style="margin-top:20px; border-top:1px dashed #ccc; padding-top:15px;">
                <input type="text" id="new-name" placeholder="サイト名" style="width:100%; margin-bottom:8px; box-sizing: border-box;">
                <input type="text" id="new-url" placeholder="https://..." style="width:100%; box-sizing: border-box;">
                <button class="btn-blue" onclick="addUrl()">URLを追加</button>
            </div>
        </div>

        <!-- 設定・同期セクション -->
        <div class="card">
            <h2>同期・年選択設定</h2>
            
            <div style="margin-bottom: 15px; background: #f1f8ff; padding: 12px; border-radius: 8px;">
                <label style="font-size: 13px; font-weight: bold; display: block; margin-bottom: 5px;">🔒 年変更パスワード</label>
                <div style="display: flex; gap: 8px;">
                    <input type="password" id="settings-password" placeholder="パスワードを入力" style="flex: 1;">
                    <button class="cal-btn" onclick="verifyPassword()">認証</button>
                </div>
            </div>

            <div id="year-selection-area" style="margin-bottom: 15px; opacity: 0.5; pointer-events: none; transition: 0.3s;">
                <label style="font-size: 13px; font-weight: bold; display: block; margin-bottom: 5px;">📅 編集・表示する年を選択 (2020年〜)</label>
                <select id="target-year-select" style="width: 100%; padding: 8px;" onchange="changeTargetYear()">
                    <!-- 2020年から現在+5年までを動的生成 -->
                </select>
            </div>

            <button class="btn-blue" onclick="syncSave()">今の予定を保存 (送信)</button>
            <button class="btn-blue" style="background:#4caf50;" onclick="syncLoad()">最新の予定を読込 (受信)</button>
            <hr style="margin:20px 0; border:none; border-top:1px solid #eee;">
            <button class="btn-blue" style="background:#999;" onclick="if(confirm('全データを初期化しますか？')){localStorage.clear(); location.reload();}">全データ初期化</button>
        </div>
    </div>

    <script>
        const API_URL = "https://script.google.com/macros/s/AKfycbxBxVnIGXV8VPExeVmcSRSw-0fDUVtBztFZDeH4MMpLDITsXpJ3J15ZuPIKSWwWmUOM/exec";
        const ADMIN_PASSWORD = "hayabee29"; // パスワードを hayabee29 に設定
        
        let myUrls = [];
        let currentYear = new Date().getFullYear();
        let displayDate = new Date();

        // 年のセレクトボックスを生成 (2020年〜2035年など)
        function initYearSelect() {
            const select = document.getElementById('target-year-select');
            select.innerHTML = "";
            for (let y = 2020; y <= 2035; y++) {
                let opt = document.createElement('option');
                opt.value = y;
                opt.innerText = `${y}年`;
                if (y === currentYear) opt.selected = true;
                select.appendChild(opt);
            }
        }

        function verifyPassword() {
            const pass = document.getElementById('settings-password').value;
            if (pass === ADMIN_PASSWORD) {
                document.getElementById('year-selection-area').style.opacity = '1';
                document.getElementById('year-selection-area').style.pointerEvents = 'auto';
                alert("パスワード認証成功！年を選択できるようになりました。");
            } else {
                alert("パスワードが違います。");
            }
        }

        function changeTargetYear() {
            currentYear = parseInt(document.getElementById('target-year-select').value);
            displayDate.setFullYear(currentYear);
            document.getElementById('header-year-badge').innerText = `表示中: ${currentYear}年`;
            loadLocalData();
            renderCalendar();
        }

        function updateClock() {
            const now = new Date();
            document.getElementById('current-date').innerText = now.toLocaleDateString('ja-JP', {year:'numeric',month:'long',day:'numeric',weekday:'short'});
            document.getElementById('current-time').innerText = now.toLocaleTimeString('ja-JP', {hour12:false});
        }

        function changeMonth(diff) {
            displayDate.setMonth(displayDate.getMonth() + diff);
            renderCalendar();
        }

        function getStorageKey(id) {
            return `${currentYear}_${id}`;
        }

        function loadLocalData() {
            document.querySelectorAll('.save-date').forEach(input => {
                input.value = localStorage.getItem(getStorageKey(input.id)) || "";
            });
            myUrls = JSON.parse(localStorage.getItem(`${currentYear}_myUrls`) || localStorage.getItem('myUrls') || '[]');
            renderUrls();
        }

        function renderCalendar() {
            const year = displayDate.getFullYear(), month = displayDate.getMonth();
            document.getElementById('cal-month-title').innerText = `${year}年 ${month + 1}月`;
            
            const firstDay = new Date(year, month, 1).getDay();
            const lastDate = new Date(year, month + 1, 0).getDate();
            const tbody = document.getElementById('calendar-body');
            tbody.innerHTML = "";

            let date = 1;
            const today = new Date();

            for (let i = 0; i < 6; i++) {
                let row = document.createElement("tr");
                for (let j = 0; j < 7; j++) {
                    let cell = document.createElement("td");
                    if (i === 0 && j < firstDay) {} 
                    else if (date <= lastDate) {
                        const d = date;
                        const dStr = `${year}-${String(month+1).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
                        cell.innerHTML = `<div>${d}</div><div class="dot-container" id="dot-${dStr}"></div>`;
                        
                        if (d === today.getDate() && month === today.getMonth() && year === today.getFullYear()) {
                            cell.classList.add('today');
                        }
                        
                        const dotDiv = cell.querySelector(`#dot-${dStr}`);
                        document.querySelectorAll('.save-date').forEach(input => {
                            if(input.value === dStr) {
                                const isSchool = input.closest('.card').innerHTML.includes('学校関係');
                                dotDiv.innerHTML += `<span class="dot ${isSchool ? 'dot-school' : 'dot-train'}"></span>`;
                            }
                        });

                        cell.onclick = () => {
                            let tasks = [];
                            document.querySelectorAll('.save-date').forEach(input => {
                                if(input.value === dStr) tasks.push(input.id.replace('date-',''));
                            });
                            const msg = tasks.length ? tasks.join(', ') : 'なし';
                            alert(`${year}年${month+1}月${d}日の予定: ${msg}`);
                            document.getElementById('today-schedule').innerText = msg;
                        };
                        date++;
                    }
                    row.appendChild(cell);
                }
                tbody.appendChild(row);
                if (date > lastDate) break;
            }
        }

        async function syncSave() {
            const data = { year: currentYear, dates: {}, urls: myUrls };
            document.querySelectorAll('.save-date').forEach(input => data.dates[input.id] = input.value);
            try {
                await fetch(API_URL, { method: 'POST', body: JSON.stringify(data) });
                alert(`${currentYear}年のデータを保存しました！`);
            } catch (e) { alert("保存に失敗しました。"); }
        }

        async function syncLoad() {
            try {
                const res = await fetch(`${API_URL}?year=${currentYear}`);
                const data = await res.json();
                if (data.dates) {
                    for (let id in data.dates) {
                        const el = document.getElementById(id);
                        if (el) {
                            el.value = data.dates[id];
                            localStorage.setItem(getStorageKey(id), data.dates[id]);
                        }
                    }
                }
                if (data.urls) {
                    myUrls = data.urls;
                    localStorage.setItem(`${currentYear}_myUrls`, JSON.stringify(myUrls));
                    renderUrls();
                }
                renderCalendar();
                alert(`${currentYear}年のデータを同期しました！`);
            } catch (e) { alert("読込に失敗しました。"); }
        }

        function renderUrls() {
            const container = document.getElementById('url-list-container');
            container.innerHTML = "";
            myUrls.forEach((item, index) => {
                container.innerHTML += `<div class="url-card"><a href="${item.url}" target="_blank">${item.name}</a><button onclick="deleteUrl(${index})" style="color:red;border:none;background:none;cursor:pointer;">[削除]</button></div>`;
            });
        }

        function addUrl() {
            const n = document.getElementById('new-name'), u = document.getElementById('new-url');
            if(!n.value || !u.value) return;
            myUrls.push({name: n.value, url: u.value});
            localStorage.setItem(`${currentYear}_myUrls`, JSON.stringify(myUrls));
            n.value = ""; u.value = ""; renderUrls();
        }

        function deleteUrl(index) {
            myUrls.splice(index, 1);
            localStorage.setItem(`${currentYear}_myUrls`, JSON.stringify(myUrls));
            renderUrls();
        }

        window.onload = () => {
            initYearSelect();
            document.getElementById('header-year-badge').innerText = `表示中: ${currentYear}年`;
            
            document.querySelectorAll('.save-date').forEach(input => {
                input.onchange = () => {
                    localStorage.setItem(getStorageKey(input.id), input.value);
                    renderCalendar();
                };
            });
            
            loadLocalData();
            renderCalendar();
            setInterval(updateClock, 1000);
            updateClock();
        };
    </script>
</body>
</html>
