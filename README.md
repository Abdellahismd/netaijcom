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
            --primary: #0284c7;
            --primary-light: #38bdf8;
            --accent: #d97706;
            --danger: #ef4444;
            --warning: #f59e0b;
            --info: #0ea5e9;
            --gold: #fbbf24;
            --silver: #cbd5e1;
            --bronze: #d97706;
            --bg-main: #f0f9ff;
            --text-dark: #0f172a;
            --text-gray: #475569;
            --card-bg: rgba(255, 255, 255, 0.75);
            --glass-bg: rgba(255, 255, 255, 0.65);
            --shadow-soft: 0 8px 32px 0 rgba(2, 132, 199, 0.08);
            --border-glass: 1px solid rgba(255, 255, 255, 0.4);
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Cairo', sans-serif; -webkit-tap-highlight-color: transparent; }
        body { background: radial-gradient(circle at top left, #e0f2fe 0%, #f0f9ff 100%); color: var(--text-dark); min-height: 100vh; overflow-x: hidden; }
        ::-webkit-scrollbar { display: none; }

        /* ================= الهيدر ================= */
        .curved-header { background: linear-gradient(135deg, var(--primary), var(--primary-light)); border-radius: 0 0 35px 35px; padding: 25px 15px 50px 15px; text-align: center; box-shadow: 0 10px 30px rgba(2, 132, 199, 0.15); margin-bottom: 30px; border-bottom: var(--border-glass); }
        .header-title { color: #ffffff; font-size: 1.4rem; font-weight: 800; margin-bottom: 20px; text-shadow: 0 2px 10px rgba(0,0,0,0.1); }
        .exam-tabs { display: flex; flex-direction: column; gap: 10px; max-width: 500px; margin: 0 auto; }
        .exam-btn { background: rgba(255, 255, 255, 0.15); border: 1px solid rgba(255, 255, 255, 0.3); color: #ffffff; padding: 12px; border-radius: 14px; font-size: 0.95rem; font-weight: 700; cursor: pointer; backdrop-filter: blur(5px); -webkit-backdrop-filter: blur(5px); transition: var(--transition); }
        .exam-btn.active { background: #ffffff; color: var(--primary); box-shadow: 0 8px 20px rgba(2, 132, 199, 0.2); transform: scale(1.02); border: none; font-weight: 800; }

        /* ================= الفلترة والبحث ================= */
        .controls-container { max-width: 1000px; margin: -40px auto 25px auto; padding: 0 15px; position: relative; z-index: 10; }
        .glass-panel { background: var(--card-bg); backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px); border: var(--border-glass); border-radius: 20px; padding: 20px; box-shadow: var(--shadow-soft); }
        .dropdown-filters { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 12px; margin-bottom: 15px; }
        .custom-select { width: 100%; padding: 12px 15px; border-radius: 14px; border: 1px solid rgba(2, 132, 199, 0.1); background: rgba(255, 255, 255, 0.8); font-size: 0.85rem; font-weight: 700; color: var(--text-dark); outline: none; appearance: none; transition: var(--transition); }
        .custom-select:focus { border-color: var(--primary); box-shadow: 0 0 0 3px rgba(2, 132, 199, 0.15); }
        .custom-select:disabled { background-color: rgba(241, 245, 249, 0.5); color: #94a3b8; cursor: not-allowed; }
        .search-box { position: relative; margin-bottom: 15px; }
        .search-input { width: 100%; padding: 14px 20px; border-radius: 14px; border: 1px solid rgba(2, 132, 199, 0.1); background: rgba(255, 255, 255, 0.85); font-size: 0.95rem; color: var(--text-dark); outline: none; transition: var(--transition); }
        .search-input:focus { border-color: var(--primary-light); box-shadow: 0 8px 25px rgba(2, 132, 199, 0.12), 0 0 0 3px rgba(2, 132, 199, 0.1); }
        .toggles-container { display: flex; justify-content: flex-start; align-items: center; }
        .toggle-label { display: flex; align-items: center; gap: 10px; font-size: 0.95rem; font-weight: 700; color: var(--primary); cursor: pointer; user-select: none; }
        .toggle-label input { width: 20px; height: 20px; accent-color: var(--primary); cursor: pointer; }

        /* ================= الأوائل ================= */
        .section-title { text-align: center; font-size: 1.3rem; font-weight: 800; color: var(--primary); margin: 30px 0 15px 0; position: relative; }
        .section-title::after { content: ""; display: block; width: 60px; height: 4px; background: var(--gold); margin: 6px auto 0 auto; border-radius: 5px; }
        .top-10-scroll { display: flex; overflow-x: auto; gap: 18px; padding: 15px; scroll-snap-type: x mandatory; scrollbar-width: none; }
        .top-10-scroll::-webkit-scrollbar { display: none; }
        .top-card { min-width: 280px; background: linear-gradient(135deg, #ffffff, rgba(240, 249, 255, 0.9)); border-radius: 20px; padding: 20px; box-shadow: 0 10px 25px rgba(2, 132, 199, 0.05); border: 2px solid rgba(251, 191, 36, 0.3); scroll-snap-align: center; position: relative; flex-shrink: 0; cursor: pointer; transition: var(--transition); }
        .top-card:hover { transform: translateY(-8px); box-shadow: 0 15px 30px rgba(251, 191, 36, 0.15); border-color: rgba(251, 191, 36, 0.8); }
        .top-rank-badge { position: absolute; top: -15px; right: 20px; width: 38px; height: 38px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 800; color: #fff; font-size: 1.15rem; box-shadow: 0 5px 15px rgba(0,0,0,0.1); }
        .rank-1 { background: linear-gradient(135deg, var(--gold), #d97706); }
        .rank-2 { background: linear-gradient(135deg, var(--silver), #64748b); }
        .rank-3 { background: linear-gradient(135deg, #b45309, var(--bronze)); }
        .rank-other { background: var(--primary-light); }
        .top-name { font-weight: 800; color: var(--text-dark); font-size: 1.05rem; margin-top: 15px; line-height: 1.4; }
        .top-score { font-size: 1.2rem; font-weight: 800; color: var(--accent); margin: 8px 0; }
        .top-details { font-size: 0.8rem; color: var(--text-gray); line-height: 1.6; }
        .top-details span { color: var(--text-dark); font-weight: 700; }

        /* ================= النتائج العامة ================= */
        main { padding: 0 15px 40px 15px; max-width: 1200px; margin: 0 auto; }
        #results-container { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 20px; }
        .student-card { background: var(--card-bg); backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px); padding: 18px; border-radius: 18px; cursor: pointer; box-shadow: var(--shadow-soft); border: var(--border-glass); transition: var(--transition); position: relative; overflow: hidden; }
        .student-card:hover { transform: translateY(-5px); box-shadow: 0 12px 35px rgba(2, 132, 199, 0.12); border-color: rgba(2, 132, 199, 0.3); background: rgba(255, 255, 255, 0.9); }
        .card-header-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; }
        .general-rank { background: rgba(2, 132, 199, 0.08); color: var(--primary); padding: 5px 12px; border-radius: 10px; font-size: 0.75rem; font-weight: 800; border: 1px solid rgba(2, 132, 199, 0.1); }
        .student-name { font-size: 1rem; font-weight: 800; color: var(--text-dark); line-height: 1.5; margin-bottom: 6px; word-wrap: break-word;}
        .student-number { font-size: 0.8rem; color: var(--text-gray); font-weight: 600; }
        .card-divider { height: 1px; background: rgba(2, 132, 199, 0.08); margin: 12px 0; }
        .card-footer { display: flex; justify-content: space-between; align-items: flex-end; }
        .card-meta { font-size: 0.8rem; color: var(--text-gray); line-height: 1.7; }
        .card-meta span { font-weight: 700; color: var(--text-dark); }
        .result-badge { padding: 5px 14px; border-radius: 12px; font-size: 0.75rem; font-weight: 800; white-space: nowrap; }
        .badge-success { background: rgba(16, 185, 129, 0.15); color: #059669; }
        .badge-danger { background: rgba(239, 68, 68, 0.15); color: var(--danger); }
        .badge-warning { background: rgba(245, 158, 11, 0.15); color: var(--accent); }

        /* ================= تحميل هيكلي ================= */
        .skeleton-card { background: rgba(255, 255, 255, 0.5); border-radius: 18px; padding: 18px; box-shadow: var(--shadow-soft); height: 150px; position: relative; overflow: hidden; border: var(--border-glass); }
        .skeleton-card::after { content: ""; position: absolute; top: 0; left: 0; right: 0; bottom: 0; background: linear-gradient(90deg, transparent, rgba(255,255,255,0.6), transparent); animation: loading 1.5s infinite; }
        @keyframes loading { 0% { transform: translateX(-100%); } 100% { transform: translateX(100%); } }
        .skeleton-line { height: 10px; background: #e2e8f0; border-radius: 6px; margin-bottom: 12px; }
        .skeleton-line.title { width: 60%; height: 14px; margin-bottom: 20px; }

        /* ================= صفحة التفاصيل ================= */
        #details-page { display: none; padding: 30px 15px; max-width: 750px; margin: 0 auto; animation: fadeIn 0.4s cubic-bezier(0.16, 1, 0.3, 1); }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .details-container { background: var(--card-bg); backdrop-filter: blur(20px); -webkit-backdrop-filter: blur(20px); border: var(--border-glass); border-radius: 24px; padding: 25px; box-shadow: var(--shadow-soft); }
        .details-header-bar { display: flex; justify-content: space-between; align-items: center; margin-bottom: 25px; border-bottom: 2px dashed rgba(2, 132, 199, 0.15); padding-bottom: 20px; }
        .details-header-bar h3 { font-size: 1.25rem; font-weight: 800; color: var(--primary); text-align: center; flex-grow: 1; }
        .btn-close-page { background: rgba(2, 132, 199, 0.1); border: none; color: var(--primary); padding: 10px 20px; border-radius: 12px; font-weight: 800; font-size: 0.9rem; cursor: pointer; transition: var(--transition); }
        .btn-close-page:hover { background: var(--primary); color: #fff; }
        .full-data-list { display: flex; flex-direction: column; gap: 12px; margin-top: 20px; }
        .data-row { display: flex; justify-content: space-between; padding: 14px 18px; background: rgba(255, 255, 255, 0.5); border: 1px solid rgba(2, 132, 199, 0.04); border-radius: 12px; font-size: 0.95rem; transition: var(--transition); }
        .data-row:hover { background: rgba(255,255,255,0.8); transform: scale(1.01); }
        .data-label { color: var(--text-gray); font-weight: 700; width: 40%; }
        .data-val { color: var(--text-dark); font-weight: 800; width: 60%; text-align: left; word-wrap: break-word; }

        #load-more { display: none; width: 100%; max-width: 250px; margin: 30px auto; background: var(--card-bg); color: var(--primary); border: 2px solid var(--primary); padding: 14px; border-radius: 14px; font-weight: 800; font-size: 0.95rem; cursor: pointer; transition: var(--transition); text-align: center; }
        #load-more:hover { background: var(--primary); color: #fff; transform: translateY(-3px); box-shadow: 0 8px 20px rgba(2, 132, 199, 0.2); }
        .hidden { display: none !important; }
    </style>
</head>
<body>

    <div id="main-view">
        <header class="curved-header">
            <h1 class="header-title">نتائج الامتحانات الوطنية الموريتانية 2026</h1>
            <div class="exam-tabs">
                <button class="exam-btn active" onclick="changeExam('concour', this)">مسابقة دخول الإعدادية (كونكور)</button>
                <button class="exam-btn" onclick="changeExam('brevet', this)">شهادة ختم الدروس الإعدادية (ابريفة)</button>
            </div>
        </header>

        <div class="controls-container">
            <div class="glass-panel">
                <div class="dropdown-filters">
                    <select id="filter-wilaya" class="custom-select" onchange="onWilayaChange()"><option value="">كل الولايات</option></select>
                    <select id="filter-center" class="custom-select" onchange="onCenterChange()" disabled><option value="">كل المقاطعات/المراكز</option></select>
                    <select id="filter-school" class="custom-select" onchange="applyFilters()" disabled><option value="">كل المدارس</option></select>
                </div>
                <div class="search-box">
                    <input type="text" class="search-input" id="search-input" placeholder="ابحث برقم المترشح، الاسم، المدرسة، المركز..." oninput="handleSearch()">
                </div>
                <div class="toggles-container">
                    <label class="toggle-label">
                        <input type="checkbox" id="pass-only-toggle" onchange="applyFilters()"> عرض الناجحين فقط ✨
                    </label>
                </div>
            </div>
        </div>

        <div id="top-10-section" class="hidden">
            <h2 class="section-title" id="top-10-title">العشرة الأوائل 🏆</h2>
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
            <div id="motivation-container" style="text-align: center; margin-bottom: 20px; font-weight: 800; font-size: 1.25rem;"></div>
            <div class="full-data-list" id="page-details-list"></div>
        </div>
    </div>

    <script>
        // روابط البيانات محدثة بدقة
        const DATA_URLS = {
            concour: "https://docs.google.com/spreadsheets/d/e/2PACX-1vTlJq68a973gbhoAwubzYAcdrMx1VVg3d14HH0aTZw6TBIRwU0FnVwU6fjCvHzxBg/pub?gid=361235812&single=true&output=csv",
            brevet: "https://docs.google.com/spreadsheets/d/e/2PACX-1vSmlFARDtbicSO0XnNpLZhHGGaroLusxOIx7NsN34rkKYSWdSDn5wltRPjpUk4CaQ/pub?gid=1962707704&single=true&output=csv"
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

        function fetchData(examType) {
            showSkeletonLoading();
            
            // استخدام التنزيل المباشر من PapaParse لحل مشكلة "يرجى التحقق من المعلومات" بشكل نهائي
            Papa.parse(DATA_URLS[examType], {
                download: true,
                header: true,
                skipEmptyLines: 'greedy',
                complete: function(results) {
                    if (results.data && results.data.length > 0) {
                        appState.columns = results.meta.fields || Object.keys(results.data[0]);
                        setupKeysAndParse(results.data); 
                    } else {
                        showErrorMsg("الملف فارغ، يرجى التأكد من الرابط.");
                    }
                }, 
                error: function(err) { 
                    console.error("خطأ في الجلب:", err);
                    showErrorMsg("تعذر سحب النتائج حالياً، يرجى التحقق من اتصال الإنترنت أو تحديث الصفحة."); 
                }
            });
        }

        function showErrorMsg(msg) {
            document.getElementById('results-container').innerHTML = `
                <div style="grid-column: 1/-1; text-align: center; padding: 25px; color: var(--danger); font-weight: bold; background: rgba(239, 68, 68, 0.1); border: 1px solid rgba(239, 68, 68, 0.2); border-radius: 16px;">
                    ${msg}
                </div>`;
        }

        function normalizeArabic(text) {
            if (!text) return "";
            return text.toString().toLowerCase()
                .replace(/[أإآأ]/g, 'ا').replace(/ة/g, 'ه').replace(/ى/g, 'ي').replace(/[\u064B-\u0652]/g, ""); 
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
            // تم إضافة moyg لمعدل ابريفة
            appState.keys.res = getMatchingKey(appState.columns, ['decision', 'décision', 'النتيجة', 'القرار', 'resultat', 'ملاحظة', 'قرار']);
            appState.keys.score = getMatchingKey(appState.columns, ['moyg', 'total', 'مجموع', 'mgex', 'moyenne', 'moy', 'المعدل', 'note', 'points']);
            appState.keys.name = getMatchingKey(appState.columns, ['nom', 'name', 'اسم', 'الاسم']);
            appState.keys.num = getMatchingKey(appState.columns, ['numero', 'num', 'رقم', 'المترشح']);
            
            appState.keys.wilaya = getMatchingKey(appState.columns, ['wilaya', 'ولاية', 'region', 'dren']);
            appState.keys.center = getMatchingKey(appState.columns, ['centre', 'مركز', 'moughataa', 'مقاطعة']);
            appState.keys.school = getMatchingKey(appState.columns, ['ecole', 'etablissement', 'مدرسة', 'مؤسسة']);

            data.forEach(student => {
                let resultStr = appState.keys.res && student[appState.keys.res] ? student[appState.keys.res].toString().trim().toLowerCase() : "";
                let scoreStr = appState.keys.score && student[appState.keys.score] ? student[appState.keys.score].toString().trim() : "";
                
                let scoreVal = -1;
                if (scoreStr !== "") {
                    let cleanStr = scoreStr.replace(/[^\d.,]/g, '').replace(',', '.');
                    let parsedScore = parseFloat(cleanStr);
                    if (!isNaN(parsedScore)) scoreVal = parsedScore;
                }

                let status = 'fail';
                
                // شروط قاطعة: آدمين ناجح، آجورني راسب، والغياب غياب.
                let isAdmis = resultStr.includes('ناجح') || resultStr.includes('admis') || resultStr.includes('مؤهل') || resultStr === 'a' || resultStr.includes('adm');
                let isAjourne = resultStr.includes('راسب') || resultStr.includes('ajourn') || resultStr.includes('آجورني') || resultStr.includes('echec') || resultStr.includes('مقصى');
                let isAbsent = resultStr.includes('غائب') || resultStr.includes('abs');
                
                if (isAbsent) {
                    status = 'absent';
                } else if (isAdmis) {
                    status = 'pass';
                } else if (isAjourne) {
                    status = 'fail';
                } else if (resultStr.includes('دورة') || resultStr.includes('session') || resultStr.includes('تكميلية')) {
                    status = 'session';
                } else {
                    // الاعتماد على النقاط (المعدل) إذا كان القرار فارغاً
                    if (appState.currentExam === 'concour') {
                        // الكونكور: من 80 إلى 200 ناجح
                        if (scoreVal >= 80 && scoreVal <= 200) status = 'pass';
                        else status = 'fail';
                    } else if (appState.currentExam === 'brevet') {
                        // ابريفة
                        if (scoreVal >= 10) status = 'pass';
                        else status = 'fail';
                    }
                }

                student._scoreVal = scoreVal;
                student._scoreStr = scoreStr;
                student._status = status;
                student._resStr = student[appState.keys.res] || "";
                student._nameStr = appState.keys.name && student[appState.keys.name] ? student[appState.keys.name].toString() : "بدون اسم";
                
                const searchData = [
                    student[appState.keys.name], 
                    student[appState.keys.num], 
                    student[appState.keys.school], 
                    student[appState.keys.center]
                ].join(" ");
                student._searchStr = normalizeArabic(searchData);
            });

            // الترتيب التلقائي للجميع من أعلى نقطة/معدل إلى أقل نقطة
            data.sort((a, b) => {
                let valA = a._scoreVal !== -1 ? a._scoreVal : -999;
                let valB = b._scoreVal !== -1 ? b._scoreVal : -999;
                return valB - valA;
            });

            appState.currentData = data;
            initDropdowns();
            applyFilters();
        }

        // ================= دوال الفلترة والبحث ================= //
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

        function applyFilters() {
            const wilaya = document.getElementById('filter-wilaya').value;
            const center = document.getElementById('filter-center').value;
            const school = document.getElementById('filter-school').value;
            const searchQuery = document.getElementById('search-input').value.trim();
            const passOnly = document.getElementById('pass-only-toggle').checked;
            const searchNormalized = normalizeArabic(searchQuery);

            appState.filteredData = appState.currentData.filter(s => {
                let match = true;
                if (wilaya && appState.keys.wilaya && s[appState.keys.wilaya] !== wilaya) match = false;
                if (center && appState.keys.center && s[appState.keys.center] !== center) match = false;
                if (school && appState.keys.school && s[appState.keys.school] !== school) match = false;
                if (passOnly && s._status !== 'pass') match = false;
                if (searchNormalized && !s._searchStr.includes(searchNormalized)) match = false;
                return match;
            });

            appState.currentPage = 1;
            renderResults(true);
            renderTop10();
        }

        function handleSearch() { applyFilters(); }

        // ================= دوال عرض النتائج ================= //
        function renderResults(reset = false) {
            const container = document.getElementById('results-container');
            if (reset) container.innerHTML = '';

            const start = (appState.currentPage - 1) * appState.itemsPerPage;
            const end = start + appState.itemsPerPage;
            const paginated = appState.filteredData.slice(start, end);

            if (paginated.length === 0 && reset) {
                container.innerHTML = '<div style="grid-column:1/-1;text-align:center;padding:20px;">لا توجد نتائج مطابقة لبحثك.</div>';
                document.getElementById('load-more').style.display = 'none';
                return;
            }

            paginated.forEach((student, index) => {
                const rank = start + index + 1;
                const card = document.createElement('div');
                card.className = 'student-card';
                card.onclick = () => openDetailsPage(student, rank);
                
                let badgeClass = 'badge-danger'; let statusText = 'راسب';
                if (student._status === 'pass') { badgeClass = 'badge-success'; statusText = 'ناجح'; }
                else if (student._status === 'absent') { badgeClass = 'badge-warning'; statusText = 'غائب'; }
                else if (student._status === 'session') { badgeClass = 'badge-info'; statusText = 'دورة'; }

                card.innerHTML = `
                    <div class="card-header-top">
                        <span class="general-rank">الترتيب #${rank}</span>
                        <span class="result-badge ${badgeClass}">${statusText}</span>
                    </div>
                    <div class="student-name">${student._nameStr}</div>
                    <div class="student-number">رقم المترشح: ${appState.keys.num ? (student[appState.keys.num] || '-') : '-'}</div>
                    <div class="card-divider"></div>
                    <div class="card-footer">
                        <div class="card-meta">
                            المعدل/المجموع: <span>${student._scoreStr || '-'}</span>
                        </div>
                    </div>
                `;
                container.appendChild(card);
            });

            if (end < appState.filteredData.length) document.getElementById('load-more').style.display = 'block';
            else document.getElementById('load-more').style.display = 'none';
        }

        function loadMoreData() { appState.currentPage++; renderResults(false); }

        function renderTop10() {
            const topSection = document.getElementById('top-10-section');
            const topContainer = document.getElementById('top-10-container');
            const topStudents = appState.filteredData.filter(s => s._status === 'pass').slice(0, 10);
            
            if (topStudents.length === 0) { topSection.classList.add('hidden'); return; }
            topSection.classList.remove('hidden');
            topContainer.innerHTML = '';
            
            topStudents.forEach((student, index) => {
                const rank = index + 1;
                let rankClass = 'rank-other';
                if(rank === 1) rankClass = 'rank-1'; else if(rank === 2) rankClass = 'rank-2'; else if(rank === 3) rankClass = 'rank-3';

                const card = document.createElement('div');
                card.className = 'top-card';
                card.onclick = () => openDetailsPage(student, rank);
                
                card.innerHTML = `
                    <div class="top-rank-badge ${rankClass}">${rank}</div>
                    <div class="top-name">${student._nameStr}</div>
                    <div class="top-score">${student._scoreStr || '-'}</div>
                    <div class="top-details">
                        مدرسة: <span>${appState.keys.school ? (student[appState.keys.school] || '-') : '-'}</span><br>
                        مركز: <span>${appState.keys.center ? (student[appState.keys.center] || '-') : '-'}</span>
                    </div>
                `;
                topContainer.appendChild(card);
            });
        }

        function openDetailsPage(student, rank) {
            document.getElementById('main-view').style.display = 'none';
            document.getElementById('details-page').style.display = 'block';
            
            const list = document.getElementById('page-details-list');
            list.innerHTML = '';
            
            const motivation = document.getElementById('motivation-container');
            if(student._status === 'pass') {
                motivation.innerHTML = '🎉 ألف مبروك النجاح! 🎉'; motivation.style.color = '#059669';
                if(typeof confetti === 'function') confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
            } else if(student._status === 'fail') {
                motivation.innerHTML = 'حظاً أوفر في المرة القادمة، لا تيأس! 💪'; motivation.style.color = '#ef4444';
            } else if(student._status === 'absent') {
                motivation.innerHTML = 'غائب - نرجو أن تكون بأفضل حال 🤍'; motivation.style.color = '#f59e0b';
            } else {
                motivation.innerHTML = 'نتمنى لك التوفيق 🌟'; motivation.style.color = '#d97706';
            }

            let statusText = 'راسب';
            if (student._status === 'pass') statusText = 'ناجح';
            else if (student._status === 'absent') statusText = 'غائب';
            else if (student._status === 'session') statusText = 'دورة';
            
            list.innerHTML = `
                <div class="data-row" style="background: rgba(2, 132, 199, 0.1);">
                    <div class="data-label">حالة النتيجة (القرار)</div>
                    <div class="data-val" style="color: var(--primary);">${statusText}</div>
                </div>
                <div class="data-row" style="background: rgba(2, 132, 199, 0.05);">
                    <div class="data-label">الترتيب العام</div>
                    <div class="data-val">${rank}</div>
                </div>
            `;

            appState.columns.forEach(col => {
                const val = student[col];
                if(val && val.trim() !== '') {
                    list.innerHTML += `
                        <div class="data-row">
                            <div class="data-label">${col}</div>
                            <div class="data-val">${val}</div>
                        </div>
                    `;
                }
            });
        }

        function closeDetailsPage() {
            document.getElementById('details-page').style.display = 'none';
            document.getElementById('main-view').style.display = 'block';
        }
    </script>
</body>
</html>
