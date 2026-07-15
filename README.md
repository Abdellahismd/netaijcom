<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>نتائج الامتحانات الوطنية 2026</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800&display=swap" rel="stylesheet">
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

    <style>
        /* ================= المتغيرات والألوان ================= */
        :root {
            --primary: #1e3a8a;
            --primary-light: #3b82f6;
            --accent: #10b981;
            --danger: #ef4444;
            --warning: #f59e0b;
            --info: #0ea5e9;
            --gold: #fbbf24;
            --silver: #9ca3af;
            --bronze: #b45309;
            --bg-main: #f4f7f6;
            --text-dark: #1e293b;
            --text-gray: #64748b;
            --card-bg: #ffffff;
            --glass-bg: rgba(255, 255, 255, 0.9);
            --shadow-soft: 0 4px 20px rgba(0, 0, 0, 0.05);
            --transition: all 0.25s ease;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Cairo', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg-main);
            color: var(--text-dark);
            min-height: 100vh;
            overflow-x: hidden;
        }
        ::-webkit-scrollbar { display: none; } /* إخفاء شريط التمرير */

        /* ================= الهيدر والتبويبات ================= */
        .curved-header {
            background: linear-gradient(135deg, var(--primary), var(--primary-light));
            border-radius: 0 0 35px 35px;
            padding: 20px 15px 45px 15px;
            text-align: center;
            box-shadow: 0 10px 25px rgba(59, 130, 246, 0.15);
            margin-bottom: 25px;
        }

        .header-title {
            color: #ffffff;
            font-size: 1.2rem; 
            font-weight: 800;
            margin-bottom: 15px;
        }

        .exam-tabs {
            display: flex;
            flex-direction: column;
            gap: 8px;
            max-width: 500px;
            margin: 0 auto;
        }

        .exam-btn {
            background: rgba(255, 255, 255, 0.15);
            border: 1px solid rgba(255, 255, 255, 0.3);
            color: #ffffff;
            padding: 10px;
            border-radius: 12px;
            font-size: 0.9rem;
            font-weight: 700;
            cursor: pointer;
            transition: var(--transition);
        }

        .exam-btn.active {
            background: #ffffff;
            color: var(--primary);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
            transform: scale(1.02);
            border: none;
        }

        /* ================= أدوات الفلترة والبحث ================= */
        .controls-container {
            max-width: 1000px;
            margin: -35px auto 20px auto;
            padding: 0 15px;
            position: relative;
            z-index: 10;
        }

        .dropdown-filters {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 10px;
            margin-bottom: 12px;
        }

        .custom-select {
            width: 100%;
            padding: 10px;
            border-radius: 12px;
            border: 1px solid rgba(0,0,0,0.08);
            background: var(--card-bg);
            font-size: 0.85rem;
            font-weight: 700;
            color: var(--text-dark);
            outline: none;
            box-shadow: var(--shadow-soft);
            appearance: none;
            background-image: url("data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%231e3a8a%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E");
            background-repeat: no-repeat;
            background-position: left 10px center;
            background-size: 10px auto;
        }
        .custom-select:disabled { background-color: #f1f5f9; color: #94a3b8; cursor: not-allowed; }

        .search-box {
            position: relative;
            margin-bottom: 12px;
        }

        .search-input {
            width: 100%;
            padding: 12px 15px;
            border-radius: 12px;
            border: 1px solid rgba(0,0,0,0.05);
            background: var(--glass-bg);
            box-shadow: var(--shadow-soft);
            font-size: 0.9rem;
            color: var(--text-dark);
            outline: none;
            transition: var(--transition);
        }
        .search-input:focus { border-color: var(--primary-light); box-shadow: 0 8px 25px rgba(59, 130, 246, 0.15); }

        .toggles-container {
            display: flex;
            justify-content: flex-start;
            align-items: center;
            background: var(--card-bg);
            padding: 10px 15px;
            border-radius: 12px;
            box-shadow: var(--shadow-soft);
        }

        .toggle-label {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9rem;
            font-weight: 700;
            color: var(--primary);
            cursor: pointer;
        }
        .toggle-label input { width: 18px; height: 18px; accent-color: var(--accent); }

        /* ================= قسم الأوائل ================= */
        .section-title {
            text-align: center;
            font-size: 1.2rem;
            font-weight: 800;
            color: var(--primary);
            margin: 25px 0 15px 0;
            position: relative;
        }
        .section-title::after {
            content: "";
            display: block;
            width: 50px;
            height: 3px;
            background: var(--accent);
            margin: 5px auto 0 auto;
            border-radius: 5px;
        }

        .top-10-scroll {
            display: flex;
            overflow-x: auto;
            gap: 15px;
            padding: 10px 15px 20px 15px;
            scroll-snap-type: x mandatory;
        }
        
        .top-card {
            min-width: 260px;
            background: linear-gradient(145deg, #ffffff, #f8fafc);
            border-radius: 16px;
            padding: 15px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.06);
            border: 1px solid rgba(0,0,0,0.04);
            scroll-snap-align: center;
            position: relative;
            flex-shrink: 0;
            cursor: pointer;
            transition: var(--transition);
        }
        .top-card:hover { transform: translateY(-5px); }

        .top-rank-badge {
            position: absolute;
            top: -10px;
            right: 15px;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 800;
            color: #fff;
            font-size: 1.1rem;
            box-shadow: 0 4px 10px rgba(0,0,0,0.15);
        }
        .rank-1 { background: var(--gold); }
        .rank-2 { background: var(--silver); }
        .rank-3 { background: var(--bronze); }
        .rank-other { background: var(--primary-light); }

        .top-name { font-weight: 800; color: var(--primary); font-size: 1rem; margin-top: 15px; line-height: 1.3; }
        .top-score { font-size: 1.1rem; font-weight: 800; color: var(--accent); margin: 5px 0; }
        .top-details { font-size: 0.75rem; color: var(--text-gray); line-height: 1.5; }
        .top-details span { color: var(--text-dark); font-weight: 700; }

        /* ================= بطاقات النتائج العامة ================= */
        main { padding: 0 15px 30px 15px; max-width: 1200px; margin: 0 auto; }
        
        #results-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 15px;
        }

        .student-card {
            background: var(--card-bg);
            padding: 15px;
            border-radius: 14px;
            cursor: pointer;
            box-shadow: var(--shadow-soft);
            border: 1px solid rgba(0,0,0,0.03);
            transition: var(--transition);
            position: relative;
            overflow: hidden;
        }

        .student-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
            border-color: rgba(59, 130, 246, 0.2);
        }

        .card-header-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
        
        .general-rank {
            background: var(--bg-main);
            color: var(--primary);
            padding: 4px 10px;
            border-radius: 8px;
            font-size: 0.75rem;
            font-weight: 800;
            border: 1px solid rgba(0,0,0,0.05);
        }

        .student-name { font-size: 0.95rem; font-weight: 800; color: var(--text-dark); line-height: 1.4; margin-bottom: 4px; word-wrap: break-word;}
        .student-number { font-size: 0.75rem; color: var(--text-gray); }

        .card-divider { height: 1px; background: rgba(0,0,0,0.05); margin: 10px 0; }

        .card-footer { display: flex; justify-content: space-between; align-items: flex-end; }
        .card-meta { font-size: 0.75rem; color: var(--text-gray); line-height: 1.6; }
        .card-meta span { font-weight: 700; color: var(--text-dark); }
        
        .result-badge { padding: 4px 12px; border-radius: 12px; font-size: 0.75rem; font-weight: 800; white-space: nowrap; }
        .badge-success { background: rgba(16, 185, 129, 0.1); color: var(--accent); }
        .badge-danger { background: rgba(239, 68, 68, 0.1); color: var(--danger); }
        .badge-warning { background: rgba(245, 158, 11, 0.1); color: var(--warning); }
        .badge-info { background: rgba(14, 165, 233, 0.1); color: var(--info); }

        /* ================= حالة التحميل ================= */
        .skeleton-card {
            background: #fff; padding: 15px; border-radius: 14px;
            box-shadow: var(--shadow-soft); height: 130px; position: relative; overflow: hidden;
        }
        .skeleton-card::after {
            content: ""; position: absolute; top: 0; left: 0; right: 0; bottom: 0;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.6), transparent);
            animation: loading 1.5s infinite;
        }
        @keyframes loading { 0% { transform: translateX(-100%); } 100% { transform: translateX(100%); } }
        .skeleton-line { height: 8px; background: #e2e8f0; border-radius: 4px; margin-bottom: 10px; }

        /* ================= صفحة التفاصيل ================= */
        #details-page {
            display: none; padding: 20px 15px; max-width: 700px; margin: 0 auto;
            animation: fadeIn 0.3s ease;
        }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

        .details-container { background: var(--card-bg); border-radius: 20px; padding: 20px; box-shadow: var(--shadow-soft); }
        .details-header-bar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; border-bottom: 2px dashed rgba(0,0,0,0.05); padding-bottom: 15px; }
        .details-header-bar h3 { font-size: 1.1rem; font-weight: 800; color: var(--primary); text-align: center; flex-grow: 1; }
        .btn-close-page { background: #f1f5f9; border: none; color: var(--text-dark); padding: 8px 15px; border-radius: 10px; font-weight: 800; font-size: 0.85rem; cursor: pointer; }

        .full-data-list { display: flex; flex-direction: column; gap: 10px; margin-top: 20px; }
        .data-row { display: flex; justify-content: space-between; padding: 12px; background: var(--bg-main); border-radius: 10px; font-size: 0.9rem; }
        .data-label { color: var(--text-gray); font-weight: 700; width: 40%; }
        .data-val { color: var(--text-dark); font-weight: 800; width: 60%; text-align: left; word-wrap: break-word; }

        #load-more {
            display: none; width: 100%; max-width: 250px; margin: 25px auto; 
            background: #fff; color: var(--primary); border: 2px solid var(--primary);
            padding: 12px; border-radius: 12px; font-weight: 800; font-size: 0.9rem; cursor: pointer; transition: var(--transition); text-align: center;
        }
        #load-more:hover { background: var(--primary); color: #fff; }

        .hidden { display: none !important; }
    </style>
</head>
<body>

    <div id="main-view">
        <header class="curved-header">
            <h1 class="header-title">نتائج الامتحانات الوطنية الموريتانية</h1>
            <div class="exam-tabs">
                <button class="exam-btn active" onclick="changeExam('concour', this)">مسابقة دخول الإعدادية</button>
            </div>
        </header>

        <div class="controls-container">
            <div class="dropdown-filters">
                <select id="filter-wilaya" class="custom-select" onchange="onWilayaChange()">
                    <option value="">كل الولايات</option>
                </select>
                <select id="filter-center" class="custom-select" onchange="onCenterChange()" disabled>
                    <option value="">كل المقاطعات/المراكز</option>
                </select>
                <select id="filter-school" class="custom-select" onchange="applyFilters()" disabled>
                    <option value="">كل المدارس</option>
                </select>
            </div>

            <div class="search-box">
                <input type="text" class="search-input" id="search-input" placeholder="ابحث برقم المترشح، الاسم، المدرسة، المركز..." oninput="handleSearch()">
            </div>

            <div class="toggles-container">
                <label class="toggle-label">
                    <input type="checkbox" id="pass-only-toggle" onchange="applyFilters()">
                    عرض الناجحين فقط
                </label>
            </div>
        </div>

        <div id="top-10-section" class="hidden">
            <h2 class="section-title" id="top-10-title">العشرة الأوائل</h2>
            <div class="top-10-scroll" id="top-10-container"></div>
        </div>

        <main>
            <div id="results-container"></div>
            <button id="load-more" onclick="loadMoreData()">عرض المزيد</button>
        </main>
    </div>

    <div id="details-page">
        <div class="details-container">
            <div class="details-header-bar">
                <h3 id="page-name">التفاصيل الكاملة</h3>
                <button class="btn-close-page" onclick="closeDetailsPage()">رجوع</button>
            </div>
            <div id="motivation-container" style="text-align: center; margin-bottom: 15px; font-weight: 800; font-size: 1.1rem;"></div>
            <div class="full-data-list" id="page-details-list"></div>
        </div>
    </div>

    <script>
        // تم الإبقاء على رابط الكونكور فقط كما طلبت
        const DATA_URLS = {
            concour: "https://docs.google.com/spreadsheets/d/e/2PACX-1vTlJq68a973gbhoAwubzYAcdrMx1VVg3d14HH0aTZw6TBIRwU0FnVwU6fjCvHzxBg/pub?gid=361235812&single=true&output=csv"
        };

        let appState = {
            currentData: [],     
            filteredData: [],    
            columns: [],         
            keys: {},            
            currentPage: 1,
            itemsPerPage: 40,
            currentExam: 'concour'
        };

        document.addEventListener('DOMContentLoaded', () => { fetchData('concour'); });

        function changeExam(type, btnElement) {
            document.querySelectorAll('.exam-btn').forEach(btn => btn.classList.remove('active'));
            btnElement.classList.add('active');
            
            document.getElementById('search-input').value = '';
            document.getElementById('pass-only-toggle').checked = false;
            resetDropdowns();

            appState.currentExam = type;
            fetchData(type);
        }

        function showSkeletonLoading() {
            const container = document.getElementById('results-container');
            document.getElementById('load-more').style.display = 'none';
            document.getElementById('top-10-section').classList.add('hidden');
            container.innerHTML = '';
            for(let i=0; i<8; i++){
                container.innerHTML += `<div class="skeleton-card"><div class="skeleton-line title"></div><div class="skeleton-line"></div><div class="skeleton-line"></div></div>`;
            }
        }

        async function fetchData(examType) {
            showSkeletonLoading();
            try {
                const response = await fetch(DATA_URLS[examType]);
                if (!response.ok) throw new Error();
                const csvText = await response.text();

                Papa.parse(csvText, {
                    header: true, 
                    skipEmptyLines: true,
                    complete: function(results) {
                        if (results.data && results.data.length > 0) {
                            appState.columns = results.meta.fields || Object.keys(results.data[0]);
                            setupKeysAndParse(results.data); 
                        }
                    }, 
                    error: () => showErrorMsg()
                });
            } catch (error) { showErrorMsg(); }
        }

        function showErrorMsg() {
            document.getElementById('results-container').innerHTML = `<div style="grid-column: 1/-1; text-align: center; padding: 20px; color: var(--danger); font-weight: bold; background: #fff; border-radius: 12px;">تعذر سحب النتائج حالياً، يرجى التحقق من اتصال الإنترنت.</div>`;
        }

        function normalizeArabic(text) {
            if (!text) return "";
            // إزالة التشكيل البسيط وتوحيد الهمزات بشكل سريع
            return text.toString().toLowerCase().replace(/[أإآأ]/g, 'ا').replace(/ة/g, 'ه').replace(/ى/g, 'ي').replace(/[\u064B-\u0652]/g, ""); 
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

        function setupKeysAndParse(data) {
            appState.keys.res = getMatchingKey(appState.columns, ['decision', 'décision', 'النتيجة', 'القرار', 'resultat', 'ملاحظة', 'قرار']);
            appState.keys.score = getMatchingKey(appState.columns, ['total', 'مجموع', 'mgex', 'moyenne', 'moy', 'المعدل', 'note', 'points']);
            appState.keys.name = getMatchingKey(appState.columns, ['nom', 'name', 'اسم', 'الاسم']);
            appState.keys.num = getMatchingKey(appState.columns, ['numero', 'num', 'رقم', 'المترشح']);
            
            appState.keys.wilaya = getMatchingKey(appState.columns, ['wilaya', 'ولاية', 'region', 'dren']);
            appState.keys.center = getMatchingKey(appState.columns, ['centre', 'مركز', 'moughataa', 'مقاطعة']);
            appState.keys.school = getMatchingKey(appState.columns, ['ecole', 'etablissement', 'مدرسة', 'مؤسسة']);

            data.forEach(student => {
                // تسريع المعالجة بتجنب الدوال الثقيلة للـ regex إلا للضرورة
                let resultStr = appState.keys.res && student[appState.keys.res] ? student[appState.keys.res].toString().trim().toLowerCase() : "";
                let scoreStr = appState.keys.score && student[appState.keys.score] ? student[appState.keys.score].toString().trim() : "";
                
                let scoreVal = -1;
                if (scoreStr !== "") {
                    let cleanStr = scoreStr.replace(/[^\d.,]/g, '').replace(',', '.');
                    let parsedScore = parseFloat(cleanStr);
                    if (!isNaN(parsedScore)) scoreVal = parsedScore;
                }

                let status = 'fail';
                if (resultStr.includes('غائب') || resultStr.includes('abs')) {
                    status = 'absent';
                } else if (resultStr.includes('ناجح') || resultStr.includes('admis') || resultStr.includes('مؤهل')) {
                    status = 'pass';
                } else if (resultStr.includes('دورة') || resultStr.includes('session') || resultStr.includes('تكميلية')) {
                    status = 'session';
                } else if (resultStr.includes('راسب') || resultStr.includes('ajourne') || resultStr.includes('echec') || resultStr.includes('مقصى')) {
                    status = 'fail';
                } else {
                    if (appState.currentExam === 'concour') {
                        if (scoreVal >= 80) status = 'pass'; else if (scoreVal >= 0) status = 'fail'; else status = 'absent';
                    }
                }

                student._scoreVal = scoreVal;
                student._scoreStr = scoreStr;
                student._status = status;
                student._resStr = student[appState.keys.res] || "";
                student._nameStr = appState.keys.name && student[appState.keys.name] ? student[appState.keys.name].toString() : "بدون اسم";
                
                // تم تحسين الأداء بجمع بيانات محددة للبحث بدلاً من كل الأعمدة
                const searchData = [
                    student[appState.keys.name], 
                    student[appState.keys.num], 
                    student[appState.keys.school], 
                    student[appState.keys.center]
                ].join(" ");
                student._searchStr = normalizeArabic(searchData);
            });

            appState.currentData = data;
            initDropdowns();
            applyFilters();
        }

        function resetDropdowns() {
            document.getElementById('filter-wilaya').innerHTML = '<option value="">كل الولايات</option>';
            document.getElementById('filter-center').innerHTML = '<option value="">كل المقاطعات/المراكز</option>';
            document.getElementById('filter-center').disabled = true;
            document.getElementById('filter-school').innerHTML = '<option value="">كل المدارس</option>';
            document.getElementById('filter-school').disabled = true;
        }

        function populateDropdown(selectId, optionsSet) {
            const select = document.getElementById(selectId);
            const defaultOption = select.options[0].outerHTML;
            if (optionsSet.size === 0) {
                select.innerHTML = defaultOption;
                select.disabled = true;
                return;
            }
            select.disabled = false;
            let optionsHTML = defaultOption;
            Array.from(optionsSet).sort().forEach(val => {
                if (val && val.trim() !== "") optionsHTML += `<option value="${val}">${val}</option>`;
            });
            select.innerHTML = optionsHTML;
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
            document.getElementById('filter-center').innerHTML = '<option value="">كل المقاطعات/المراكز</option>';
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

        function handleSearch() { applyFilters(); }

        function applyFilters() {
            const query = normalizeArabic(document.getElementById('search-input').value.trim());
            const selectedWilaya = document.getElementById('filter-wilaya').value;
            const selectedCenter = document.getElementById('filter-center').value;
            const selectedSchool = document.getElementById('filter-school').value;
            const passOnly = document.getElementById('pass-only-toggle').checked;

            let filtered = appState.currentData.filter(student => {
                let match = true;
                if (selectedWilaya && appState.keys.wilaya && student[appState.keys.wilaya] !== selectedWilaya) match = false;
                if (selectedCenter && appState.keys.center && student[appState.keys.center] !== selectedCenter) match = false;
                if (selectedSchool && appState.keys.school && student[appState.keys.school] !== selectedSchool) match = false;
                if (query && !student._searchStr.includes(query)) match = false;
                if (passOnly && student._status !== 'pass') match = false;
                return match;
            });

            filtered.sort((a, b) => {
                if (b._scoreVal !== a._scoreVal) return b._scoreVal - a._scoreVal;
                return a._nameStr.localeCompare(b._nameStr, 'ar');
            });

            let currentRank = 1;
            for (let i = 0; i < filtered.length; i++) {
                if (i > 0 && filtered[i]._scoreVal === filtered[i-1]._scoreVal) {
                    filtered[i]._computedRank = filtered[i-1]._computedRank;
                } else {
                    filtered[i]._computedRank = currentRank;
                }
                currentRank++;
            }

            appState.filteredData = filtered;
            appState.currentPage = 1;
            
            renderTop10();
            renderResults(true);
        }

        function renderTop10() {
            const top10Section = document.getElementById('top-10-section');
            const top10Container = document.getElementById('top-10-container');
            
            if (appState.filteredData.length === 0) {
                top10Section.classList.add('hidden');
                return;
            }

            top10Section.classList.remove('hidden');
            top10Container.innerHTML = '';
            
            const top10 = appState.filteredData.slice(0, 10);
            const scoreLabel = 'المجموع';
            const fragment = document.createDocumentFragment(); // استخدام جزء الصفحة للتسريع

            top10.forEach((student, index) => {
                const rank = index + 1; 
                let rankClass = 'rank-other';
                if (rank === 1) rankClass = 'rank-1';
                else if (rank === 2) rankClass = 'rank-2';
                else if (rank === 3) rankClass = 'rank-3';

                const school = appState.keys.school && student[appState.keys.school] ? student[appState.keys.school] : 'غير مدرج';
                const wilaya = appState.keys.wilaya && student[appState.keys.wilaya] ? student[appState.keys.wilaya] : '';

                const card = document.createElement('div');
                card.className = 'top-card';
                card.onclick = () => openDetailsPage(student);
                card.innerHTML = `
                    <div class="top-rank-badge ${rankClass}">${rank}</div>
                    <div class="top-name">${student._nameStr}</div>
                    <div class="top-score">${scoreLabel}: ${student._scoreStr}</div>
                    <div class="top-details">
                        <p>المدرسة: <span>${school}</span></p>
                        ${wilaya ? `<p>الولاية: <span>${wilaya}</span></p>` : ''}
                    </div>
                `;
                fragment.appendChild(card);
            });
            top10Container.appendChild(fragment);
        }

        function renderResults(reset = false) {
            const container = document.getElementById('results-container');
            const loadMoreBtn = document.getElementById('load-more');
            
            if (reset) { container.innerHTML = ""; appState.currentPage = 1; }

            const startIndex = (appState.currentPage - 1) * appState.itemsPerPage;
            const endIndex = startIndex + appState.itemsPerPage;
            const itemsToRender = appState.filteredData.slice(startIndex, endIndex);

            if (itemsToRender.length === 0 && reset) {
                container.innerHTML = `<div style="grid-column: 1/-1; text-align: center; padding: 30px; color: var(--text-gray); font-weight:700; background: #fff; border-radius: 12px;">لا توجد نتائج مطابقة لبحثك ضمن الفلاتر المحددة.</div>`;
                loadMoreBtn.style.display = 'none'; return;
            }

            const scoreLabel = 'المجموع';
            const fragment = document.createDocumentFragment(); // تسريع الإضافة لـ DOM

            itemsToRender.forEach((student) => {
                const num = appState.keys.num && student[appState.keys.num] ? student[appState.keys.num] : "غير متوفر";
                const location = (appState.keys.school && student[appState.keys.school]) || (appState.keys.center && student[appState.keys.center]) || "غير مدرج";
                
                let badgeClass = 'badge-success';
                let badgeText = student._resStr || 'ناجح';
                if (student._status === 'fail') { badgeClass = 'badge-danger'; badgeText = student._resStr || 'راسب'; }
                else if (student._status === 'session') { badgeClass = 'badge-warning'; badgeText = student._resStr || 'دورة'; }
                else if (student._status === 'absent') { badgeClass = 'badge-info'; badgeText = student._resStr || 'غائب'; }

                const card = document.createElement('div');
                card.className = 'student-card';
                card.onclick = () => openDetailsPage(student);
                card.innerHTML = `
                    <div class="card-header-top">
                        <div class="general-rank">الترتيب: ${student._computedRank || '-'}</div>
                        <div class="result-badge ${badgeClass}">${badgeText}</div>
                    </div>
                    <div class="student-name">${student._nameStr}</div>
                    <div class="student-number">رقم الترشح: ${num}</div>
                    <div class="card-divider"></div>
                    <div class="card-footer">
                        <div class="card-meta">
                            المؤسسة/المركز:<br><span>${location}</span>
                        </div>
                        <div class="card-meta" style="text-align: left;">
                            ${scoreLabel}:<br><span style="color: var(--primary); font-size: 1rem;">${student._scoreStr}</span>
                        </div>
                    </div>
                `;
                fragment.appendChild(card);
            });
            
            // إضافة كل البطاقات دفعة واحدة لتسريع العرض
            container.appendChild(fragment);

            if (appState.filteredData.length > endIndex) {
                loadMoreBtn.style.display = 'block';
            } else {
                loadMoreBtn.style.display = 'none';
            }
        }

        function loadMoreData() {
            appState.currentPage++;
            renderResults();
        }

        function openDetailsPage(student) {
            document.getElementById('main-view').style.display = 'none';
            const detailsPage = document.getElementById('details-page');
            detailsPage.style.display = 'block';
            
            document.getElementById('page-name').textContent = student._nameStr;
            
            const list = document.getElementById('page-details-list');
            list.innerHTML = '';
            
            appState.columns.forEach(col => {
                if(student[col] && student[col].toString().trim() !== '') {
                    list.innerHTML += `<div class="data-row"><div class="data-label">${col}</div><div class="data-val">${student[col]}</div></div>`;
                }
            });

            // إطلاق الاحتفال إذا كان الطالب ناجحاً
            if(student._status === 'pass' && typeof confetti === 'function') {
                confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
            }
        }

        function closeDetailsPage() {
            document.getElementById('details-page').style.display = 'none';
            document.getElementById('main-view').style.display = 'block';
        }
    </script>
</body>
</html>
