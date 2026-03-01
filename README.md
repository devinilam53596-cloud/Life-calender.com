# Life-calender.com
A high-performance, minimalist Digital Life Calendar designed to visualize time and enforce consistency. This tool tracks every single day from the start of 2026 until the end of 2039, serving as a visual reminder of your progress and the finite nature of time.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Consistency 2026-2039</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        :root { --bg: #000000; --past: #0a0a0a; --present: #00ffff; --future-bg: #111111; --future-border: #333; --text: #ffffff; }
        body, html { margin: 0; padding: 0; background: var(--bg); color: var(--text); font-family: sans-serif; height: 100%; overflow: hidden; }
        .menu-btn { position: fixed; top: 15px; left: 15px; z-index: 1001; cursor: pointer; background: #1a1a1a; padding: 10px; border-radius: 8px; border: 1px solid var(--present); color: var(--present); }
        .sidebar { position: fixed; left: -260px; top: 0; width: 250px; height: 100%; background: #050505; transition: 0.4s; z-index: 1000; border-right: 1px solid #222; overflow-y: auto; }
        .sidebar.active { left: 0; }
        .year-link { padding: 15px 25px; cursor: pointer; border-bottom: 1px solid #111; color: #888; }
        .app-container { width: 100%; height: 100vh; display: flex; flex-direction: column; justify-content: space-between; padding: 20px; box-sizing: border-box; }
        .header { text-align: center; height: 12%; }
        .header h1 { margin: 0; font-size: 1.6rem; letter-spacing: 4px; color: var(--present); }
        .calendar-wrapper { width: 100%; height: 75%; display: grid; grid-template-columns: repeat(15, 1fr); gap: 3px; padding: 10px; background: #080808; border: 1px solid #1a1a1a; border-radius: 12px; overflow-y: auto; box-sizing: border-box; }
        .day-box { aspect-ratio: 1; background: var(--future-bg); border: 1px solid var(--future-border); border-radius: 2px; font-size: 8px; display: flex; align-items: center; justify-content: center; color: #444; }
        .day-box.past { background: var(--past); border-color: #000; color: #222; }
        .day-box.today { background: var(--present); border-color: #fff; box-shadow: 0 0 15px rgba(0, 255, 255, 0.5); color: #000; font-weight: bold; }
        .footer { height: 10%; display: flex; flex-direction: column; align-items: center; }
        .save-btn { background: var(--present); color: #000; border: none; padding: 12px 30px; border-radius: 25px; font-weight: bold; cursor: pointer; width: 80%; }
    </style>
</head>
<body>
    <div class="menu-btn" onclick="document.getElementById('sidebar').classList.toggle('active')">☰</div>
    <div class="sidebar" id="sidebar">
        <div style="padding: 25px; font-weight: bold; color: var(--present);">YEARS 2026-2039</div>
        <div id="year-list"></div>
    </div>
    <div class="app-container" id="capture-area">
        <div class="header">
            <h1 id="year-title">2026</h1>
            <div id="stats" style="font-size: 11px; color: #666; margin-top: 5px;"></div>
        </div>
        <div class="calendar-wrapper" id="grid"></div>
        <div class="footer">
            <button class="save-btn" onclick="downloadImage()">SAVE WALLPAPER</button>
            <div id="countdown" style="font-size: 10px; color: #444; margin-top: 8px;"></div>
        </div>
    </div>
    <script>
        let viewYear = new Date().getFullYear();
        function render() {
            const grid = document.getElementById('grid');
            grid.innerHTML = '';
            document.getElementById('year-title').innerText = viewYear;
            const now = new Date();
            const totalDays = (viewYear % 4 === 0 && viewYear % 100 !== 0) || (viewYear % 400 === 0) ? 366 : 365;
            let todayIdx = (now.getFullYear() === viewYear) ? Math.floor((now - new Date(viewYear, 0, 0)) / 86400000) : (now.getFullYear() > viewYear ? 400 : -1);
            for(let i=1; i<=totalDays; i++) {
                const div = document.createElement('div');
                div.className = 'day-box' + (i < todayIdx ? ' past' : (i === todayIdx ? ' today' : ''));
                div.innerText = i;
                grid.appendChild(div);
            }
            document.getElementById('stats').innerText = `${todayIdx > 0 ? (todayIdx > totalDays ? totalDays : todayIdx) : 0} DAYS FAST | YEAR ${viewYear}`;
        }
        function downloadImage() {
            html2canvas(document.getElementById('capture-area'), {backgroundColor: '#000', scale: 2}).then(canvas => {
                const link = document.createElement('a');
                link.download = `Calendar_${viewYear}.png`;
                link.href = canvas.toDataURL();
                link.click();
            });
        }
        const list = document.getElementById('year-list');
        for(let y=2026; y<=2039; y++) {
            const d = document.createElement('div');
            d.className = 'year-link'; d.innerText = 'Year ' + y;
            d.onclick = () => { viewYear = y; render(); document.getElementById('sidebar').classList.remove('active'); };
            list.appendChild(d);
        }
        render();
    </script>
</body>
</html>
