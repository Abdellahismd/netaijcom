<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>نتائج الامتحانات الوطنية</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800&display=swap" rel="stylesheet">
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

    <style>
        /* ================= المتغيرات ================= */
        :root {
            --primary: #0284c7;
            --primary-light: #38bdf8;
            --accent: #d97706;
            --danger: #ef4444;
            --success: #059669;
            --warning: #f59e0b;
            --info: #0ea5e9;
            --gold: #fbbf24;
            --silver: #cbd5e1;
            --bronze: #d97706;
            --bg-main: #f8fafc;
            --text-dark: #1e293b;
            --text-gray: #64748b;
            --card-bg: #ffffff;
            --border-color: #e2e8f0;
            --transition: all 0.2s ease-in-out;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Cairo', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background-color: var(--bg-main); color: var(--text-dark); min-height: 100vh; overflow-x: hidden; font-size: 12px; }
        ::-webkit-scrollbar { display: none; }

        /* ================= الهيدر والتبويبات ================= */
        .header { background: linear-gradient(135deg, var(--primary), var(--primary-light)); padding: 16px 8px 24px 8px; text-align: center; box-shadow: 0 4px 15px rgba(0,0,0,0.08); margin-bottom: 16px; border-bottom-left-radius: 24px; border-bottom-right-radius: 24px; }
        .header-title { color: #ffffff; font-size: 1.1rem; font-weight: 800; margin-bottom: 12px; }
        .exam-tabs { display: flex; justify-content: center; gap: 8px; max-width: 400px; margin: 0 auto; }
        .exam-btn { background: rgba(255, 255, 255, 0.2); border: 1px solid rgba(255, 255, 255, 0.3); color: #ffffff; padding: 6px 8px; border-radius: 8px; font-size: 0.75rem; font-weight: 700; cursor: pointer; transition: var(--transition); flex: 1; text-align: center; line-height: 1.2; }
        .exam-btn.active { background: #ffffff; color: var(--primary); font-weight: 800; border-color: #ffffff; box-shadow: 0 2px 6px rgba(0,0,0,0.1); transform: scale(1.02); }
        .stats-main-btn { flex: none; background: var(--accent); border-color: var(--warning); padding: 6px 12px; }
        .stats-main-btn:hover { background: var(--warning); }

        /* ================= الإحصائيات ================= */
        .stats-container { max-width: 800px; margin: 0 auto 12px auto; padding: 0 8px; }
        .stats-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 6px; }
        .stat-box { background: var(--card-bg); border: 1px solid var(--border-color); border-radius: 6px; padding: 6px; text-align: center; box-shadow: 0 1px 3px rgba(0,0,0,0.02); }
        .stat-box span { display: block; font-size: 0.6rem; color: var(--text-gray); font-weight: 700; margin-bottom: 2px; }
        .stat-box strong { display: block; font-size: 0.85rem; font-weight: 800; color: var(--text-dark); }

        /* ================= أدوات الفلترة والبحث ================= */
        .controls-container { max-width: 800px; margin: 0 auto 12px auto; padding: 0 8px; }
        .filters-box { background: var(--card-bg); border: 1px solid var(--border-color); border-radius: 8px; padding: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.02); }
        .search-input { width: 100%; padding: 8px 10px; border-radius: 6px; border: 1px solid var(--border-color); background: #f8fafc; font-size: 0.8rem; font-weight: 600; color: var(--text-dark); outline: none; margin-bottom: 8px; transition: var(--transition); }
        .search-input:focus { border-color: var(--primary); background: #fff; box-shadow: 0 0 0 2px rgba(2, 132, 199, 0.1); }
        .dropdowns { display: grid; grid-template-columns: repeat(auto-fit, minmax(110px, 1fr)); gap: 6px; margin-bottom: 8px; }
        .custom-select { width: 100%; padding: 8px 20px 8px 8px; border-radius: 6px; border: 1px solid var(--border-color); background-color: #f8fafc; font-size: 0.75rem; font-weight: 700; color: var(--text-dark); outline: none; appearance: none; transition: var(--transition); background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%2364748b' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e"); background-repeat: no-repeat; background-position: left 6px center; background-size: 12px; }
        .custom-select:focus { border-color: var(--primary); background-color: #fff; }
        .custom-select:disabled { background-color: #e2e8f0; color: #94a3b8; cursor: not-allowed; opacity: 0.7; }
        .status-filters { display: flex; flex-wrap: wrap; gap: 4px; justify-content: center; }
        .status-btn { background: #f1f5f9; border: 1px solid var(--border-color); color: var(--text-gray); padding: 4px 10px; border-radius: 20px; font-size: 0.65rem; font-weight: 800; cursor: pointer; transition: var(--transition); flex: 1; min-width: 60px; text-align: center; }
        .status-btn:hover { background: #e2e8f0; }
        .status-btn.active { background: var(--primary); color: #fff; border-color: var(--primary); box-shadow: 0 2px 6px rgba(2, 132, 199, 0.2); }

        /* ================= الأول وطنياً ================= */
        .national-first { max-width: 800px; margin: 0 auto 16px auto; padding: 0 8px; }
        .nf-card { background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%); color: #fff; border-radius: 16px; padding: 20px; text-align: center; cursor: pointer; box-shadow: 0 10px 25px rgba(15, 23, 42, 0.2); position: relative; overflow: hidden; transition: var(--transition); border: 1px solid rgba(255,255,255,0.1); }
        .nf-card:hover { transform: translateY(-3px); box-shadow: 0 15px 30px rgba(15, 23, 42, 0.3); }
        .nf-card::before { content: '👑'; position: absolute; top: -15px; right: -15px; font-size: 80px; opacity: 0.08; transform: rotate(15deg); pointer-events: none; }
        .nf-badge { display: inline-block; background: linear-gradient(135deg, #fbbf24, #d97706); color: #fff; font-size: 0.75rem; font-weight: 800; padding: 4px 16px; border-radius: 20px; margin-bottom: 12px; box-shadow: 0 2px 8px rgba(217, 119, 6, 0.4); text-shadow: 0 1px 2px rgba(0,0,0,0.1); }
        .nf-name { font-size: 1.25rem; font-weight: 800; margin-bottom: 10px; color: #f8fafc; }
        .nf-details { display: flex; justify-content: center; gap: 8px; flex-wrap: wrap; font-size: 0.7rem; color: #cbd5e1; }
        .nf-details span { background: rgba(255,255,255,0.1); padding: 4px 8px; border-radius: 6px; }
        .nf-score { font-size: 1.1rem; font-weight: 800; margin-top: 15px; color: #38bdf8; }

        /* ================= الأوائل لكل مركز ================= */
        .section-title { text-align: center; font-size: 0.85rem; font-weight: 800; color: var(--text-dark); margin: 10px 0 8px 0; }
        .top-scroll-container { display: flex; overflow-x: auto; gap: 8px; padding: 4px 8px 12px 8px; scrollbar-width: none; scroll-snap-type: x mandatory; }
        .center-wrapper { background: #fff; border: 1px solid var(--border-color); border-radius: 6px; padding: 8px; flex-shrink: 0; box-shadow: 0 1px 3px rgba(0,0,0,0.02); scroll-snap-align: start; }
        .center-title { font-size: 0.75rem; font-weight: 800; color: var(--primary); text-align: center; margin-bottom: 6px; padding-bottom: 4px; border-bottom: 1px dashed var(--border-color); }
        .center-cards-flex { display: flex; gap: 6px; }
        .top-card { width: 120px; background: #f8fafc; border: 1px solid var(--border-color); border-radius: 6px; padding: 6px; text-align: center; position: relative; cursor: pointer; transition: var(--transition); }
        .top-card:hover { border-color: var(--primary); background: #f0f9ff; }
        .top-rank-badge { position: absolute; top: -6px; right: -6px; width: 18px; height: 18px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 800; color: #fff; font-size: 0.6rem; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        .rank-1 { background: var(--gold); }
        .rank-2 { background: var(--silver); color: #333; }
        .rank-3 { background: var(--bronze); }
        .top-name { font-weight: 800; font-size: 0.7rem; margin-top: 4px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; color: var(--text-dark); }
        .top-school { font-size: 0.6rem; color: var(--text-gray); margin-top: 2px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
        .top-score { font-weight: 800; color: var(--primary); font-size: 0.7rem; margin-top: 4px; }

        /* ================= النتائج العامة ================= */
        main { padding: 0 8px 30px 8px; max-width: 800px; margin: 0 auto; }
        .results-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); gap: 6px; }
        .student-card { background: var(--card-bg); padding: 8px; border-radius: 6px; cursor: pointer; border: 1px solid var(--border-color); box-shadow: 0 1px 2px rgba(0,0,0,0.02); transition: var(--transition); position: relative; }
        .student-card:hover { border-color: var(--primary); transform: translateY(-1px); box-shadow: 0 3px 8px rgba(2, 132, 199, 0.08); }
        .card-header-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 4px; }
        .general-rank { background: #f0f9ff; color: var(--primary); padding: 2px 4px; border-radius: 4px; font-size: 0.6rem; font-weight: 800; border: 1px solid rgba(2, 132, 199, 0.1); }
        .student-name { font-size: 0.75rem; font-weight: 800; color: var(--text-dark); margin-bottom: 4px; line-height: 1.2; }
        .student-details-compact { font-size: 0.65rem; color: var(--text-gray); margin-bottom: 4px; font-weight: 600; display: flex; justify-content: space-between; align-items: center;}
        .student-score-val { color: var(--primary); font-weight: 800; font-size: 0.75rem; }
        .result-badge { padding: 2px 6px; border-radius: 4px; font-size: 0.6rem; font-weight: 800; }
        .badge-success { background: #d1fae5; color: var(--success); }
        .badge-danger { background: #fee2e2; color: var(--danger); }
        .badge-warning { background: #fef3c7; color: var(--accent); }
        .badge-info { background: #e0f2fe; color: var(--info); }
        .card-divider { height: 1px; background: var(--border-color); margin: 6px 0; }
        .card-footer { text-align: center; font-size: 0.6rem; color: var(--primary); font-weight: 800; }

        /* ================= حالة التحميل ================= */
        .loading-container { text-align: center; padding: 30px; grid-column: 1/-1; }
        .loader-spinner { display: inline-block; width: 24px; height: 24px; border: 3px solid rgba(2, 132, 199, 0.2); border-radius: 50%; border-top-color: var(--primary); animation: spin 0.8s linear infinite; }
        @keyframes spin { to { transform: rotate(360deg); } }

        /* ================= صفحة التفاصيل الأصيلة ================= */
        #details-page { display: none; padding: 15px 8px; max-width: 450px; margin: 0 auto; animation: slideUp 0.3s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        
        .details-top-bar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
        .btn-close { background: #f1f5f9; border: none; width: 36px; height: 36px; border-radius: 50%; font-size: 1.2rem; color: var(--text-dark); cursor: pointer; display: flex; align-items: center; justify-content: center; transition: var(--transition); }
        .btn-close:hover { background: #e2e8f0; }
        .btn-share { background: #ecfdf5; border: 1px solid #a7f3d0; color: #059669; padding: 6px 12px; border-radius: 20px; font-weight: 800; font-size: 0.8rem; cursor: pointer; display: flex; align-items: center; gap: 6px; transition: var(--transition); }
        .btn-share:hover { background: #d1fae5; }
        
        .details-card-main { background: #fff; border-radius: 16px; box-shadow: 0 4px 20px rgba(0,0,0,0.08); padding: 20px; text-align: center; border: 1px solid var(--border-color); }
        
        .d-header-colored { display: flex; justify-content: space-between; align-items: center; background: #ecfdf5; padding: 10px 15px; border-radius: 12px; margin-bottom: 20px; transition: var(--transition); }
        .d-badge { background: var(--success); color: #fff; padding: 4px 12px; border-radius: 20px; font-weight: 800; font-size: 0.85rem; display: flex; align-items: center; gap: 4px; }
        .d-logo { font-weight: 900; color: #064e3b; font-size: 1.1rem; letter-spacing: -0.5px; }
        
        .d-name { font-size: 1.25rem; font-weight: 800; color: var(--text-dark); margin-bottom: 24px; line-height: 1.4; }
        
        .d-score-section { background: #f8fafc; border: 1px solid var(--border-color); border-radius: 12px; padding: 15px; display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px; text-align: right; }
        
        .d-circular-progress { width: 90px; height: 90px; border-radius: 50%; background: conic-gradient(var(--success) 0%, #e2e8f0 0deg); display: flex; align-items: center; justify-content: center; position: relative; flex-shrink: 0; margin-left: 15px; transition: background 1s ease-out; }
        .d-circular-progress::before { content: ''; position: absolute; width: 74px; height: 74px; background: #f8fafc; border-radius: 50%; }
        .d-circular-content { position: relative; z-index: 1; text-align: center; }
        .d-score-val { font-size: 1.2rem; font-weight: 800; color: var(--success); line-height: 1; }
        .d-score-lbl { font-size: 0.65rem; color: var(--text-gray); font-weight: 700; margin-top: 2px; }
        
        .d-location-info { flex: 1; }
        .d-location-info div { font-size: 0.85rem; font-weight: 700; color: var(--text-dark); margin-bottom: 6px; }
        .d-location-info div span { color: var(--text-gray); font-weight: 600; font-size: 0.75rem; display: inline-block; width: 55px; }
        
        .d-school-section { display: flex; background: #fff; border: 1px solid var(--border-color); border-radius: 12px; padding: 12px; margin-bottom: 12px; }
        .d-school-col { flex: 1; text-align: center; }
        .d-school-col span { display: block; font-size: 0.65rem; color: var(--text-gray); font-weight: 600; margin-bottom: 4px; }
        .d-school-col strong { display: block; font-size: 0.85rem; color: var(--text-dark); font-weight: 800; }
        .d-divider { width: 1px; background: var(--border-color); margin: 0 10px; }
        
        .d-ranks-section { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-bottom: 15px; }
        .d-rank-card { background: #fff; border: 1px solid var(--border-color); border-radius: 12px; padding: 12px 5px; text-align: center; }
        .d-rank-card strong { display: block; font-size: 1.1rem; color: var(--primary); font-weight: 800; margin-bottom: 2px; }
        .d-rank-card span { display: block; font-size: 0.6rem; color: var(--text-gray); font-weight: 700; }
        
        .d-extra-details { margin-top: 15px; border-top: 1px dashed var(--border-color); padding-top: 15px; }
        .d-extra-row { display: flex; justify-content: space-between; padding: 6px 0; font-size: 0.75rem; border-bottom: 1px solid #f1f5f9; }
        .d-extra-row:last-child { border-bottom: none; }
        .d-extra-label { color: var(--text-gray); font-weight: 700; }
        .d-extra-val { color: var(--text-dark); font-weight: 800; }

        .status-fail .d-header-colored { background: #fee2e2; }
        .status-fail .d-badge { background: var(--danger); }
        .status-fail .d-circular-progress { background: conic-gradient(var(--danger) 0%, #e2e8f0 0deg); }
        .status-fail .d-score-val { color: var(--danger); }
        
        .status-session .d-header-colored { background: #e0f2fe; }
        .status-session .d-badge { background: var(--info); }
        .status-session .d-circular-progress { background: conic-gradient(var(--info) 0%, #e2e8f0 0deg); }
        .status-session .d-score-val { color: var(--info); }
        
        .status-absent .d-header-colored { background: #fef3c7; }
        .status-absent .d-badge { background: var(--warning); color: #b45309; }
        .status-absent .d-circular-progress { background: conic-gradient(var(--warning) 0%, #e2e8f0 0deg); }
        .status-absent .d-score-val { color: #b45309; }

        /* ================= صفحة الإحصائيات ================= */
        #stats-page { display: none; padding: 15px 8px 30px 8px; max-width: 800px; margin: 0 auto; animation: slideUp 0.3s ease; }
        .stats-page-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
        .stats-page-title { font-size: 1.2rem; font-weight: 800; color: var(--text-dark); }
        .stats-grid-large { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; margin-bottom: 24px; }
        .stat-card-lg { background: #fff; border-radius: 12px; padding: 15px; text-align: center; border: 1px solid var(--border-color); box-shadow: 0 4px 10px rgba(0,0,0,0.02); }
        .stat-card-lg span { display: block; font-size: 0.75rem; color: var(--text-gray); font-weight: 800; margin-bottom: 6px; }
        .stat-card-lg strong { display: block; font-size: 1.4rem; font-weight: 800; }
        
        .stats-table-wrapper { background: #fff; border-radius: 12px; border: 1px solid var(--border-color); overflow: hidden; box-shadow: 0 2px 8px rgba(0,0,0,0.02); margin-bottom: 24px; }
        .stats-table { width: 100%; border-collapse: collapse; }
        .stats-table th, .stats-table td { padding: 12px 10px; text-align: right; border-bottom: 1px solid var(--border-color); font-size: 0.75rem; }
        .stats-table th { background: #f8fafc; font-weight: 800; color: var(--text-dark); }
        .stats-table tr:last-child td { border-bottom: none; }
        .stats-table tr:hover td { background: #f0f9ff; }
        .stats-section-title { font-size: 1rem; font-weight: 800; color: var(--primary); margin: 0 0 12px 0; display: flex; align-items: center; gap: 6px; }
        .stats-section-title::before { content: ''; display: block; width: 4px; height: 16px; background: var(--primary); border-radius: 4px; }

        .btn-load-more { display: none; width: 100%; margin: 12px 0; background: #fff; color: var(--primary); border: 1px solid var(--primary); padding: 6px; border-radius: 6px; font-weight: 800; font-size: 0.75rem; cursor: pointer; transition: var(--transition); text-align: center; }
        .btn-load-more:hover { background: var(--primary); color: #fff; }
        .hidden { display: none !important; }
    </style>
</head>
<body>

    <div id="main-view">
        <header class="header">
            <h1 class="header-title">نتائج الامتحانات الوطنية</h1>
            <div class="exam-tabs">
                <button class="exam-btn active" onclick="changeExam('concour', this)">مسابقة دخول الإعدادية</button>
                <button class="exam-btn" onclick="changeExam('brevet', this)">شهادة ختم الدروس الإعدادية</button>
                <button class="exam-btn stats-main-btn" onclick="openStatsPage()">الإحصائيات 📊</button>
            </div>
        </header>

        <div class="controls-container">
            <div class="filters-box">
                <input type="text" class="search-input" id="search-input" placeholder="بحث بالاسم، رقم المترشح، المدرسة، المركز..." oninput="handleSearch()">
                <div class="dropdowns">
                    <select id="filter-wilaya" class="custom-select" onchange="onWilayaChange()"><option value="">كل الولايات</option></select>
                    <select id="filter-center" class="custom-select" onchange="onCenterChange()" disabled><option value="">كل المراكز</option></select>
                    <select id="filter-school" class="custom-select" onchange="applyFilters()" disabled><option value="">كل المدارس</option></select>
                </div>
                <div class="status-filters" id="status-filters">
                    <button class="status-btn active" onclick="setStatusFilter('all', this)">الكل</button>
                    <button class="status-btn" onclick="setStatusFilter('pass', this)">ناجح</button>
                    <button class="status-btn" onclick="setStatusFilter('fail', this)">راسب</button>
                    <button class="status-btn" onclick="setStatusFilter('absent', this)">غائب</button>
                    <button class="status-btn" onclick="setStatusFilter('session', this)">دورة/مقصى</button>
                </div>
            </div>
        </div>

        <div id="national-first-container" class="national-first hidden"></div>

        <div id="top-section" class="hidden">
            <h2 class="section-title">الثلاثة الأوائل من كل مركز 🏆</h2>
            <div class="top-scroll-container" id="top-container"></div>
        </div>

        <main>
            <div id="results-container" class="results-grid"></div>
            <button id="load-more" class="btn-load-more" onclick="loadMoreData()">عرض المزيد</button>
        </main>
    </div>

    <div id="details-page">
        <div class="details-top-bar">
            <button class="btn-close" onclick="closeDetailsPage()">✕</button>
        </div>
        
        <div class="details-card-main" id="details-card-main">
            <div class="d-header-colored" id="d-header-colored">
                <span class="d-badge" id="d-status-badge">ناجح ✔</span>
                <div class="d-logo">🎓 مراجعي</div>
            </div>
            
            <h2 class="d-name" id="d-name">-</h2>
            
            <div class="d-score-section">
                <div class="d-circular-progress" id="d-circular-progress">
                    <div class="d-circular-content">
                        <div class="d-score-val" id="d-score-val">-</div>
                        <div class="d-score-lbl" id="d-score-lbl">نقطة</div>
                    </div>
                </div>
                <div class="d-location-info">
                    <div><span>الرقم:</span> <span id="d-num" style="color:var(--text-dark); width:auto;">-</span></div>
                    <div><span>المركز:</span> <span id="d-center" style="color:var(--text-dark); width:auto;">-</span></div>
                    <div><span>المقاطعة:</span> <span id="d-moughataa" style="color:var(--text-dark); width:auto;">-</span></div>
                </div>
            </div>
            
            <div class="d-school-section">
                <div class="d-school-col">
                    <span>الولاية</span>
                    <strong id="d-wilaya">-</strong>
                </div>
                <div class="d-divider"></div>
                <div class="d-school-col">
                    <span>المدرسة</span>
                    <strong id="d-school">-</strong>
                </div>
            </div>
            
            <div class="d-ranks-section">
                <div class="d-rank-card">
                    <strong id="d-school-rank">-</strong>
                    <span>ترتيب المدرسة</span>
                </div>
                <div class="d-rank-card">
                    <strong id="d-center-rank">-</strong>
                    <span>ترتيب المركز</span>
                </div>
                <div class="d-rank-card">
                    <strong id="d-wilaya-rank">-</strong>
                    <span>ترتيب الولاية</span>
                </div>
            </div>
            
            <div class="d-extra-details" id="d-extra-details"></div>
        </div>
    </div>

    <div id="stats-page">
        <div class="stats-page-header">
            <h2 class="stats-page-title">الإحصائيات العامة</h2>
            <button class="btn-close" onclick="closeStatsPage()">✕</button>
        </div>
        <div id="stats-page-content"></div>
    </div>

    <script>
        const DATA_URLS = {
            concour: "https://docs.google.com/spreadsheets/d/e/2PACX-1vTlJq68a973gbhoAwubzYAcdrMx1VVg3d14HH0aTZw6TBIRwU0FnVwU6fjCvHzxBg/pub?gid=361235812&single=true&output=csv",
            brevet: "https://docs.google.com/spreadsheets/d/e/2PACX-1vSmlFARDtbicSO0XnNpLZhHGGaroLusxOIx7NsN34rkKYSWdSDn5wltRPjpUk4CaQ/pub?gid=1962707704&single=true&output=csv"
        };

        // نظام تخزين مؤقت للبيانات لتسريع التبديل بين الصفحات
        let cacheData = { concour: null, brevet: null };

        let appState = {
            currentData: [],     
            filteredData: [],    
            columns: [],         
            keys: {},            
            currentPage: 1,
            itemsPerPage: 60,
            currentExam: 'concour',
            statusFilter: 'all'
        };

        document.addEventListener('DOMContentLoaded', () => { changeExam('concour', document.querySelector('.exam-btn')); });

        function changeExam(type, btnElement) {
            document.querySelectorAll('.exam-btn:not(.stats-main-btn)').forEach(btn => btn.classList.remove('active'));
            if(btnElement) btnElement.classList.add('active');
            
            document.getElementById('search-input').value = '';
            appState.statusFilter = 'all';
            document.querySelectorAll('.status-btn').forEach(b => {
                if(b.innerText === 'الكل') b.classList.add('active');
                else b.classList.remove('active');
            });
            resetDropdowns();
            appState.currentExam = type;

            if (cacheData[type]) {
                // استرجاع البيانات فوراً من الذاكرة
                appState.columns = cacheData[type].columns;
                appState.keys = cacheData[type].keys;
                appState.currentData = cacheData[type].data;
                initDropdowns();
                applyFilters();
            } else {
                fetchData(type);
            }
        }

        function showLoading() {
            const container = document.getElementById('results-container');
            document.getElementById('load-more').style.display = 'none';
            document.getElementById('top-section').classList.add('hidden');
            document.getElementById('national-first-container').classList.add('hidden');
            
            container.innerHTML = `
                <div class="loading-container">
                    <div class="loader-spinner"></div>
                </div>`;
        }

        function fetchData(examType) {
            showLoading();
            setTimeout(() => {
                Papa.parse(DATA_URLS[examType], {
                    download: true,
                    header: true,
                    skipEmptyLines: 'greedy',
                    worker: true,
                    complete: function(results) {
                        if (results.data && results.data.length > 0) {
                            appState.columns = results.meta.fields || Object.keys(results.data[0]);
                            setTimeout(() => {
                                setupKeysAndParse(results.data, examType); 
                            }, 50);
                        } else {
                            showErrorMsg("الملف فارغ، يرجى التأكد من المصدر.");
                        }
                    }, 
                    error: function(err) { 
                        console.error("خطأ في الجلب:", err);
                        showErrorMsg("تعذر سحب النتائج حالياً، يرجى التحقق من اتصال الإنترنت."); 
                    }
                });
            }, 50);
        }

        function showErrorMsg(msg) {
            document.getElementById('results-container').innerHTML = `
                <div class="loading-container" style="color: var(--danger);">
                    ${msg}
                </div>`;
        }

        function normalizeArabic(text) {
            if (!text) return "";
            return text.toString().toLowerCase()
                .replace(/[أإآأ]/g, 'ا').replace(/ة/g, 'ه').replace(/ى/g, 'ي').replace(/[ً-ْ]/g, ""); 
        }

        function getMatchingKey(keysArray, possibleKeys) {
            for (let pk of possibleKeys) {
                const exactMatch = keysArray.find(k => k.trim().toLowerCase() === pk.toLowerCase());
                if (exactMatch) return exactMatch;
            }
            for (let pk of possibleKeys) {
                const partialMatch = keysArray.find(k => k.trim().toLowerCase().includes(pk.toLowerCase()));
                if (partialMatch) return partialMatch;
            }
            return undefined; 
        }

        function setupKeysAndParse(data, examType) {
            const resKey = getMatchingKey(appState.columns, ['decision', 'décision', 'النتيجة', 'القرار', 'resultat', 'ملاحظة', 'قرار']);
            const scoreKey = getMatchingKey(appState.columns, ['moyg', 'moyenne', 'moy', 'المعدل', 'mgex', 'total', 'مجموع', 'note', 'points']);
            const nameKey = getMatchingKey(appState.columns, ['nom', 'name', 'اسم', 'الاسم']);
            const numKey = getMatchingKey(appState.columns, ['numero', 'num', 'رقم', 'المترشح']);
            const wilayaKey = getMatchingKey(appState.columns, ['wilaya', 'ولاية', 'region', 'dren']);
            const centerKey = getMatchingKey(appState.columns, ['centre', 'مركز', 'moughataa', 'مقاطعة']);
            const schoolKey = getMatchingKey(appState.columns, ['ecole', 'etablissement', 'مدرسة', 'مؤسسة']);

            appState.keys = {
                res: resKey, score: scoreKey, name: nameKey, num: numKey, 
                wilaya: wilayaKey, center: centerKey, school: schoolKey
            };

            const isConcour = examType === 'concour';

            data.forEach(student => {
                let resultStr = resKey && student[resKey] ? student[resKey].toString().trim().toLowerCase() : "";
                let scoreStrRaw = scoreKey && student[scoreKey] ? student[scoreKey].toString() : "";
                
                let scoreVal = -1;
                let scoreStrDisplay = "";
                
                // تصحيح مشكلة الأصفار الإضافية وتحويل النقطة إلى فاصلة
                if (scoreStrRaw !== "") {
                    let cleanStr = scoreStrRaw.replace(/\s+/g, '').replace(/[٫,]/g, '.').replace(/[^\d.]/g, '');
                    
                    let parsedScore = parseFloat(cleanStr);
                    if (!isNaN(parsedScore)) {
                        if (isConcour) {
                            if (parsedScore > 200 && parsedScore <= 2000) parsedScore /= 10;
                            else if (parsedScore > 2000 && parsedScore <= 20000) parsedScore /= 100;
                            else if (parsedScore > 20000 && parsedScore <= 200000) parsedScore /= 1000;
                            else if (parsedScore > 200000) parsedScore /= 10000;
                            scoreVal = Number(parsedScore.toFixed(2));
                        } else {
                            if (parsedScore > 20 && parsedScore <= 200) parsedScore /= 10;
                            else if (parsedScore > 200 && parsedScore <= 2000) parsedScore /= 100;
                            else if (parsedScore > 2000 && parsedScore <= 20000) parsedScore /= 1000;
                            else if (parsedScore > 20000) parsedScore /= 10000;
                            scoreVal = Number(parsedScore.toFixed(2));
                        }
                        scoreStrDisplay = scoreVal.toLocaleString('en-US', {minimumFractionDigits: 0, maximumFractionDigits: 2});
                    }
                }

                let status = 'fail';
                
                let isAjourne = resultStr.includes('راسب') || resultStr.includes('ajourn') || resultStr.includes('echec') || resultStr.includes('مقصى') || resultStr.includes('غير ناجح') || resultStr.includes('غير مؤهل') || resultStr.includes('échoué') || resultStr.includes('non admis');
                let isAbsent = resultStr.includes('غائب') || resultStr.includes('abs');
                let isAdmis = (!isAjourne && !isAbsent) && (resultStr.includes('ناجح') || resultStr.includes('admis') || resultStr.includes('مؤهل') || resultStr === 'a' || resultStr.includes('adm'));

                if (isConcour) {
                    if (resultStr) {
                        if (isAbsent) status = 'absent';
                        else if (isAdmis) status = 'pass';
                        else if (isAjourne) status = 'fail';
                        else status = scoreVal >= 85 ? 'pass' : 'fail';
                    } else {
                        status = scoreVal >= 85 ? 'pass' : 'fail';
                    }
                } else {
                    if (isAbsent) status = 'absent';
                    else if (isAdmis) status = 'pass';
                    else if (isAjourne) status = 'fail';
                    else if (resultStr.includes('دورة') || resultStr.includes('session') || resultStr.includes('تكميلية')) status = 'session';
                    else status = scoreVal >= 10 ? 'pass' : 'fail';
                }

                student._scoreVal = scoreVal;
                student._scoreStr = scoreStrDisplay;
                student._status = status;
                student._resStr = student[resKey] || "";
                student._nameStr = nameKey && student[nameKey] ? student[nameKey].toString() : "بدون اسم";
                
                const searchData = (student[nameKey] || "") + " " + (student[numKey] || "") + " " + (student[schoolKey] || "") + " " + (student[centerKey] || "");
                student._searchStr = normalizeArabic(searchData);
            });

            data.sort((a, b) => {
                const statusOrder = { 'pass': 1, 'session': 2, 'fail': 3, 'absent': 4 };
                const orderA = statusOrder[a._status] || 5;
                const orderB = statusOrder[b._status] || 5;
                if (orderA !== orderB) return orderA - orderB;
                let valA = a._scoreVal !== -1 ? a._scoreVal : -999;
                let valB = b._scoreVal !== -1 ? b._scoreVal : -999;
                return valB - valA;
            });

            // Assign overall rank logically: same score = same rank
            let currentRank = 1;
            let actualRank = 1;
            let currentScore = -999;

            data.forEach((student, index) => {
                if (student._scoreVal !== currentScore) {
                    currentRank = actualRank;
                    currentScore = student._scoreVal;
                }
                
                // For failing students, we don't necessarily want to share rank if we don't have scores
                if (student._scoreVal === -1) {
                    student._overallRank = actualRank;
                } else {
                    student._overallRank = currentRank;
                }
                actualRank++;
            });

            // حفظ في الذاكرة
            cacheData[examType] = {
                data: data,
                columns: appState.columns,
                keys: appState.keys
            };

            appState.currentData = data;
            initDropdowns();
            applyFilters();
        }

        // ================= الفلترة وتحديث القوائم ================= //
        let filterTimeout;
        
        function setStatusFilter(status, btn) {
            document.querySelectorAll('.status-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            appState.statusFilter = status;
            applyFilters();
        }
        function resetDropdowns() {
            document.getElementById('filter-wilaya').innerHTML = '<option value="">كل الولايات</option>';
            document.getElementById('filter-center').innerHTML = '<option value="">كل المراكز</option>';
            document.getElementById('filter-center').disabled = true;
            document.getElementById('filter-school').innerHTML = '<option value="">كل المدارس</option>';
            document.getElementById('filter-school').disabled = true;
        }

        function populateDropdown(selectId, optionsSet) {
            const select = document.getElementById(selectId);
            const currentVal = select.value;
            const defaultOption = select.options[0].outerHTML;
            if (optionsSet.size === 0) { select.innerHTML = defaultOption; select.disabled = true; return; }
            select.disabled = false;
            let optionsHTML = defaultOption;
            Array.from(optionsSet).sort().forEach(val => { if (val && val.trim() !== "") optionsHTML += `<option value="${val}">${val}</option>`; });
            select.innerHTML = optionsHTML;
            if (optionsSet.has(currentVal)) select.value = currentVal;
        }

        function initDropdowns() {
            resetDropdowns();
            if (appState.keys.wilaya) {
                const wilayas = new Set(appState.currentData.map(s => s[appState.keys.wilaya]).filter(Boolean));
                populateDropdown('filter-wilaya', wilayas);
            }
        }

        function onWilayaChange() {
            const selectedWilaya = document.getElementById('filter-wilaya').value;
            document.getElementById('filter-center').innerHTML = '<option value="">كل المراكز</option>';
            document.getElementById('filter-school').innerHTML = '<option value="">كل المدارس</option>';
            document.getElementById('filter-school').disabled = true;

            if (selectedWilaya && appState.keys.center) {
                const centers = new Set(appState.currentData.filter(s => s[appState.keys.wilaya] === selectedWilaya).map(s => s[appState.keys.center]).filter(Boolean));
                populateDropdown('filter-center', centers);
            } else { document.getElementById('filter-center').disabled = true; }
            applyFilters();
        }

        function onCenterChange() {
            const selectedWilaya = document.getElementById('filter-wilaya').value;
            const selectedCenter = document.getElementById('filter-center').value;
            document.getElementById('filter-school').innerHTML = '<option value="">كل المدارس</option>';

            if (selectedCenter && appState.keys.school) {
                const schools = new Set(appState.currentData.filter(s => {
                    let match = s[appState.keys.center] === selectedCenter;
                    if (selectedWilaya) match = match && s[appState.keys.wilaya] === selectedWilaya;
                    return match;
                }).map(s => s[appState.keys.school]).filter(Boolean));
                populateDropdown('filter-school', schools);
            } else { document.getElementById('filter-school').disabled = true; }
            applyFilters();
        }

        function applyFilters() {
            clearTimeout(filterTimeout);
            filterTimeout = setTimeout(() => {
                const wilaya = document.getElementById('filter-wilaya').value;
                const center = document.getElementById('filter-center').value;
                const school = document.getElementById('filter-school').value;
                const searchQuery = document.getElementById('search-input').value.trim();
                const searchNormalized = normalizeArabic(searchQuery);

                appState.filteredData = appState.currentData.filter(s => {
                    if (wilaya && appState.keys.wilaya && s[appState.keys.wilaya] !== wilaya) return false;
                    if (center && appState.keys.center && s[appState.keys.center] !== center) return false;
                    if (school && appState.keys.school && s[appState.keys.school] !== school) return false;
                    if (appState.statusFilter !== 'all' && s._status !== appState.statusFilter) return false;
                    if (searchNormalized && !s._searchStr.includes(searchNormalized)) return false;
                    return true;
                });

                appState.currentPage = 1;
                renderNationalFirst();
                renderTopPerCenter();
                renderResults(true);
            }, 150); // debounce slightly for performance
        }

        function handleSearch() { applyFilters(); }

        // ================= العرض والترتيب ================= //
        function renderNationalFirst() {
            const container = document.getElementById('national-first-container');
            const passedStudents = appState.filteredData.filter(s => s._status === 'pass');
            
            // نعرض الأول وطنياً فقط إذا لم تكن هناك فلاتر بحث نشطة للمقاطعة أو المدرسة
            const hasStrictFilters = document.getElementById('filter-wilaya').value || document.getElementById('filter-center').value || document.getElementById('filter-school').value || document.getElementById('search-input').value;
            
            if (passedStudents.length === 0 || hasStrictFilters) { 
                container.classList.add('hidden'); 
                return; 
            }

            const firstStudent = passedStudents[0];
            const scoreLabel = appState.currentExam === 'concour' ? 'المجموع' : 'المعدل';
            const scoreDisplay = `<div class="nf-score">${scoreLabel}: ${firstStudent._scoreStr}</div>`;

            container.innerHTML = `
                <div class="nf-card" onclick='openDetailsPage(${JSON.stringify(firstStudent).replace(/'/g, "\\'")})'>
                    <div class="nf-badge">الأول على المستوى الوطني</div>
                    <div class="nf-name">${firstStudent._nameStr}</div>
                    <div class="nf-details">
                        <span>الرقم: ${appState.keys.num ? (firstStudent[appState.keys.num] || '-') : '-'}</span> 
                        <span>المدرسة: ${appState.keys.school ? (firstStudent[appState.keys.school] || '-') : '-'}</span> 
                        <span>المركز: ${appState.keys.center ? (firstStudent[appState.keys.center] || '-') : '-'}</span>
                    </div>
                    ${scoreDisplay}
                </div>
            `;
            container.classList.remove('hidden');
        }

        function renderTopPerCenter() {
            const topSection = document.getElementById('top-section');
            const topContainer = document.getElementById('top-container');
            const passedStudents = appState.filteredData.filter(s => s._status === 'pass');
            
            if (passedStudents.length === 0) { 
                topSection.classList.add('hidden'); 
                return; 
            }
            
            topSection.classList.remove('hidden');
            topContainer.innerHTML = '';
            
            const centerGroups = {};
            passedStudents.forEach(s => {
                const center = appState.keys.center && s[appState.keys.center] ? s[appState.keys.center].trim() : 'غير محدد';
                if (!centerGroups[center]) centerGroups[center] = [];
                centerGroups[center].push(s);
            });
            
            const scoreLabel = appState.currentExam === 'concour' ? 'المجموع' : 'المعدل';
            
            Object.keys(centerGroups).sort().forEach(centerName => {
                const students = centerGroups[centerName];
                students.sort((a, b) => b._scoreVal - a._scoreVal);
                const top3 = students.slice(0, 3);
                
                const wrapper = document.createElement('div');
                wrapper.className = 'center-wrapper';
                
                const title = document.createElement('div');
                title.className = 'center-title';
                title.innerText = centerName;
                wrapper.appendChild(title);
                
                const cardsFlex = document.createElement('div');
                cardsFlex.className = 'center-cards-flex';
                
                top3.forEach((student, index) => {
                    const rank = index + 1;
                    const card = document.createElement('div');
                    card.className = 'top-card';
                    card.onclick = () => openDetailsPage(student);
                    
                    const scoreDisplay = `<div class="top-score">${scoreLabel}: ${student._scoreStr}</div>`;
                    
                    card.innerHTML = `
                        <div class="top-rank-badge rank-${rank}">${rank}</div>
                        <div class="top-name" title="${student._nameStr}">${student._nameStr}</div>
                        <div class="top-school" title="${appState.keys.school && student[appState.keys.school] ? student[appState.keys.school] : ''}">${appState.keys.school ? (student[appState.keys.school] || '-') : '-'}</div>
                        ${scoreDisplay}
                    `;
                    cardsFlex.appendChild(card);
                });
                wrapper.appendChild(cardsFlex);
                topContainer.appendChild(wrapper);
            });
        }

        function renderResults(reset = false) {
            const container = document.getElementById('results-container');
            if (reset) container.innerHTML = '';

            const start = (appState.currentPage - 1) * appState.itemsPerPage;
            const end = start + appState.itemsPerPage;
            const paginated = appState.filteredData.slice(start, end);

            if (paginated.length === 0 && reset) {
                container.innerHTML = '<div class="loading-container" style="color:var(--text-gray);">لا توجد نتائج مطابقة لبحثك.</div>';
                document.getElementById('load-more').style.display = 'none';
                return;
            }

            const scoreLabel = appState.currentExam === 'concour' ? 'المجموع' : 'المعدل';

            paginated.forEach((student) => {
                const card = document.createElement('div');
                card.className = 'student-card';
                card.onclick = () => openDetailsPage(student);
                
                let badgeClass = 'badge-danger'; let statusText = 'راسب';
                if (student._status === 'pass') { badgeClass = 'badge-success'; statusText = 'ناجح'; }
                else if (student._status === 'absent') { badgeClass = 'badge-warning'; statusText = 'غائب'; }
                else if (student._status === 'session') { badgeClass = 'badge-info'; statusText = 'دورة'; }

                const finalDecisionText = student._resStr ? student._resStr : statusText;
                const scoreDisplay = `<span>${scoreLabel}: <span class="student-score-val">${student._scoreStr !== "" ? student._scoreStr : '-'}</span></span>`;

                card.innerHTML = `
                    <div class="card-header-top">
                        <span class="general-rank">#${student._overallRank} وطنياً</span>
                        <span class="result-badge ${badgeClass}">${finalDecisionText}</span>
                    </div>
                    <div class="student-name">${student._nameStr}</div>
                    <div class="student-details-compact">
                        <span>رقم: ${appState.keys.num ? (student[appState.keys.num] || '-') : '-'}</span>
                        ${scoreDisplay}
                    </div>
                    <div class="card-divider"></div>
                    <div class="card-footer">انقر للتفاصيل</div>
                `;
                container.appendChild(card);
            });

            if (end < appState.filteredData.length) document.getElementById('load-more').style.display = 'block';
            else document.getElementById('load-more').style.display = 'none';
        }

        function loadMoreData() { appState.currentPage++; renderResults(false); }

        // ================= صفحة التفاصيل الأصيلة ================= //
        let savedScrollY = 0;
        let currentStudentForShare = null;

        function getRank(list, target) {
            return list.findIndex(s => s._status === target._status && s._scoreVal === target._scoreVal) + 1;
        }

        function openDetailsPage(student) {
            savedScrollY = window.scrollY;
            currentStudentForShare = student;
            
            document.getElementById('main-view').style.display = 'none';
            document.getElementById('details-page').style.display = 'block';
            window.scrollTo(0, 0);
            
            const cardMain = document.getElementById('details-card-main');
            cardMain.className = 'details-card-main'; // reset classes
            
            let statusText = '';
            let progressColor = '';
            
            if (student._status === 'pass') {
                statusText = 'ناجح ✔';
                progressColor = 'var(--success)';
            } else if (student._status === 'fail') {
                statusText = 'راسب ✖';
                cardMain.classList.add('status-fail');
                progressColor = 'var(--danger)';
            } else if (student._status === 'absent') {
                statusText = 'غائب';
                cardMain.classList.add('status-absent');
                progressColor = 'var(--warning)';
            } else if (student._status === 'session') {
                statusText = 'دورة تكميلية';
                cardMain.classList.add('status-session');
                progressColor = 'var(--info)';
            }
            
            // Set basic info
            document.getElementById('d-status-badge').innerText = student._resStr || statusText;
            document.getElementById('d-name').innerText = student._nameStr;
            
            // Set Score
            const scoreValEl = document.getElementById('d-score-val');
            const scoreLblEl = document.getElementById('d-score-lbl');
            const circularProgress = document.getElementById('d-circular-progress');
            
            if (appState.currentExam === 'brevet' && student._scoreVal === -1) {
                scoreValEl.innerText = '-';
                circularProgress.style.background = `conic-gradient(${progressColor} 0%, #e2e8f0 0deg)`;
            } else {
                scoreValEl.innerText = student._scoreStr !== "" ? student._scoreStr : '-';
                scoreLblEl.innerText = appState.currentExam === 'concour' ? 'نقطة' : 'معدل';
                
                // Calculate percentage
                let maxScore = appState.currentExam === 'concour' ? 200 : 20;
                let percentage = student._scoreVal !== -1 ? (student._scoreVal / maxScore) * 100 : 0;
                if (percentage > 100) percentage = 100;
                if (percentage < 0) percentage = 0;
                
                setTimeout(() => {
                    circularProgress.style.background = `conic-gradient(${progressColor} ${percentage}%, #e2e8f0 0deg)`;
                }, 50);
            }
            
            // Set locations
            document.getElementById('d-num').innerText = appState.keys.num ? (student[appState.keys.num] || '-') : '-';
            document.getElementById('d-center').innerText = appState.keys.center ? (student[appState.keys.center] || '-') : '-';
            
            let moughataaKey = appState.columns.find(c => c.toLowerCase().includes('moughataa') || c.includes('مقاطعة'));
            document.getElementById('d-moughataa').innerText = moughataaKey ? (student[moughataaKey] || '-') : (appState.keys.center ? (student[appState.keys.center] || '-') : '-');
            
            document.getElementById('d-wilaya').innerText = appState.keys.wilaya ? (student[appState.keys.wilaya] || '-') : '-';
            document.getElementById('d-school').innerText = appState.keys.school ? (student[appState.keys.school] || '-') : '-';
            
            // Calculate ranks dynamically
            const sameWilaya = appState.currentData.filter(s => s[appState.keys.wilaya] === student[appState.keys.wilaya]);
            const sameCenter = sameWilaya.filter(s => s[appState.keys.center] === student[appState.keys.center]);
            const sameSchool = sameCenter.filter(s => s[appState.keys.school] === student[appState.keys.school]);
            
            document.getElementById('d-wilaya-rank').innerText = getRank(sameWilaya, student);
            document.getElementById('d-center-rank').innerText = getRank(sameCenter, student);
            document.getElementById('d-school-rank').innerText = getRank(sameSchool, student);
            
            // Extra details
            const extraDetails = document.getElementById('d-extra-details');
            extraDetails.innerHTML = '';
            
            const addRow = (label, val) => {
                extraDetails.innerHTML += `<div class="d-extra-row"><span class="d-extra-label">${label}</span><span class="d-extra-val">${val}</span></div>`;
            };
            
            addRow('الترتيب الوطني العام', student._overallRank);
            
            let birthKey = appState.columns.find(c => c.toLowerCase().includes('niss') || c.toLowerCase().includes('naiss') || c.includes('تاريخ'));
            if (birthKey && student[birthKey]) {
                addRow('تاريخ الميلاد', student[birthKey]);
            }
            
            if (student._status === 'pass' && typeof confetti === 'function') {
                confetti({ particleCount: 100, spread: 70, origin: { y: 0.6 } });
            }
        }

        function closeDetailsPage() {
            document.getElementById('details-page').style.display = 'none';
            document.getElementById('main-view').style.display = 'block';
            window.scrollTo(0, savedScrollY);
        }

        // ================= صفحة الإحصائيات ================= //
        function openStatsPage() {
            document.getElementById('main-view').style.display = 'none';
            document.getElementById('stats-page').style.display = 'block';
            window.scrollTo(0, 0);
            renderDetailedStats();
        }

        function closeStatsPage() {
            document.getElementById('stats-page').style.display = 'none';
            document.getElementById('main-view').style.display = 'block';
        }

        function renderDetailedStats() {
            const content = document.getElementById('stats-page-content');
            
            const total = appState.currentData.length;
            if (total === 0) {
                content.innerHTML = '<div class="loading-container">لا توجد بيانات للإحصاء</div>';
                return;
            }
            
            const passed = appState.currentData.filter(s => s._status === 'pass').length;
            const failed = appState.currentData.filter(s => s._status === 'fail').length;
            const absent = appState.currentData.filter(s => s._status === 'absent').length;
            const session = appState.currentData.filter(s => s._status === 'session').length;
            
            const rate = ((passed / total) * 100).toFixed(2);
            
            let html = `
                <div class="stats-grid-large">
                    <div class="stat-card-lg"><span>إجمالي المترشحين</span><strong style="color:var(--text-dark);">${total.toLocaleString('en-US')}</strong></div>
                    <div class="stat-card-lg"><span>نسبة النجاح العامة</span><strong style="color:var(--success);">${rate}%</strong></div>
                    <div class="stat-card-lg"><span>الناجحون</span><strong style="color:var(--success);">${passed.toLocaleString('en-US')}</strong></div>
                    <div class="stat-card-lg"><span>الراسبون</span><strong style="color:var(--danger);">${failed.toLocaleString('en-US')}</strong></div>
                    <div class="stat-card-lg"><span>الغائبون</span><strong style="color:var(--warning);">${absent.toLocaleString('en-US')}</strong></div>
                    <div class="stat-card-lg"><span>مقصى / دورة</span><strong style="color:var(--info);">${session.toLocaleString('en-US')}</strong></div>
                </div>
            `;
            
            if (appState.keys.wilaya) {
                const wilayas = [...new Set(appState.currentData.map(s => s[appState.keys.wilaya]).filter(Boolean))].sort();
                
                html += `<h3 class="stats-section-title">نسب النجاح حسب الولايات</h3>`;
                html += `<div class="stats-table-wrapper"><table class="stats-table"><thead><tr><th>الولاية</th><th>المترشحين</th><th>الناجحين</th><th>النسبة</th></tr></thead><tbody>`;
                
                let wStats = wilayas.map(w => {
                    const wData = appState.currentData.filter(s => s[appState.keys.wilaya] === w);
                    const wTotal = wData.length;
                    const wPassed = wData.filter(s => s._status === 'pass').length;
                    const wRate = wTotal > 0 ? ((wPassed / wTotal) * 100).toFixed(2) : 0;
                    return { w, wTotal, wPassed, wRate: parseFloat(wRate) };
                });
                
                wStats.sort((a,b) => b.wRate - a.wRate);
                
                wStats.forEach(stat => {
                    html += `<tr>
                        <td>${stat.w}</td>
                        <td>${stat.wTotal.toLocaleString('en-US')}</td>
                        <td style="color:var(--success); font-weight:800;">${stat.wPassed.toLocaleString('en-US')}</td>
                        <td dir="ltr" style="font-weight:800; color:${stat.wRate >= 50 ? 'var(--success)' : 'var(--danger)'}">${stat.wRate}%</td>
                    </tr>`;
                });
                html += `</tbody></table></div>`;
            }
            
            if (appState.keys.school) {
                html += `<h3 class="stats-section-title">أفضل 50 مدرسة (حسب نسبة النجاح)</h3>`;
                html += `<div style="font-size:0.7rem; color:var(--text-gray); margin-bottom:10px;">* المدارس التي شاركت بـ 10 مترشحين على الأقل</div>`;
                html += `<div class="stats-table-wrapper"><table class="stats-table"><thead><tr><th>المدرسة</th><th>الولاية</th><th>المترشحين</th><th>النسبة</th></tr></thead><tbody>`;
                
                const schoolMap = {};
                appState.currentData.forEach(s => {
                    const sc = s[appState.keys.school];
                    const wi = s[appState.keys.wilaya] || '';
                    if (!sc || sc.trim() === '') return;
                    const key = sc + "||" + wi;
                    if (!schoolMap[key]) schoolMap[key] = { sc, wi, total: 0, passed: 0 };
                    schoolMap[key].total++;
                    if (s._status === 'pass') schoolMap[key].passed++;
                });
                
                let scStats = Object.values(schoolMap)
                    .filter(st => st.total >= 10)
                    .map(st => {
                        st.rate = parseFloat(((st.passed / st.total) * 100).toFixed(2));
                        return st;
                    });
                    
                scStats.sort((a,b) => {
                    if (b.rate !== a.rate) return b.rate - a.rate;
                    return b.passed - a.passed;
                });
                
                scStats.slice(0, 50).forEach(stat => {
                    html += `<tr>
                        <td>${stat.sc}</td>
                        <td>${stat.wi}</td>
                        <td>${stat.total.toLocaleString('en-US')}</td>
                        <td dir="ltr" style="font-weight:800; color:var(--success)">${stat.rate}%</td>
                    </tr>`;
                });
                html += `</tbody></table></div>`;
            }
            
            content.innerHTML = html;
        }
    </script>
</body>
</html>
