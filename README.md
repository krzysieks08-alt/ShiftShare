<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ShiftShare - Platforma dla Pracowników Zmianowych</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #2563eb;
            --primary-hover: #1d4ed8;
            --primary-light: #eff6ff;
            --secondary: #059669;
            --secondary-light: #ecfdf5;
            --wa-color: #25d366;
            --wa-hover: #128c7e;
            --bg: #f1f5f9;
            --card: #ffffff;
            --text: #0f172a;
            --muted: #64748b;
            --border: #cbd5e1;
            --danger: #ef4444;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', sans-serif; }
        body { background-color: var(--bg); color: var(--text); display: flex; justify-content: center; min-height: 100vh; }

        .app-container {
            width: 100%; max-width: 580px; background: var(--card);
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1); display: flex; flex-direction: column; min-height: 100vh;
        }

        .top-bar {
            background: #0f172a; color: #fff; padding: 10px 16px;
            display: flex; justify-content: space-between; align-items: center; font-size: 0.85rem;
        }
        .user-badge { display: flex; align-items: center; gap: 8px; }
        .avatar-sm {
            width: 28px; height: 28px; border-radius: 50%; background: var(--primary);
            color: #fff; display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 0.75rem;
        }
        .lang-select {
            background: #1e293b; color: #fff; border: 1px solid #334155;
            border-radius: 6px; padding: 4px 6px; font-size: 0.8rem; outline: none; cursor: pointer;
        }
        .btn-link { background: none; border: none; color: #94a3b8; cursor: pointer; text-decoration: underline; font-size: 0.8rem; }

        .header { background: #1e293b; color: #fff; padding: 16px; }
        .header h1 { font-size: 1.35rem; font-weight: 800; }
        .header p { color: #94a3b8; font-size: 0.8rem; margin-top: 2px; }

        .app-warning-box {
            background: rgba(239, 68, 68, 0.12); border: 1px solid rgba(239, 68, 68, 0.35);
            padding: 10px 12px; border-radius: 10px; font-size: 0.76rem; color: #fca5a5; line-height: 1.4;
            margin-top: 10px; text-align: left;
        }
        .app-warning-box strong { color: #ef4444; display: block; margin-bottom: 2px; }

        .search-box { margin-top: 10px; position: relative; }
        .search-input {
            width: 100%; padding: 10px 12px 10px 36px; background: #0f172a;
            border: 1px solid #334155; border-radius: 10px; color: #fff; font-size: 0.85rem; outline: none;
        }
        .search-icon { position: absolute; left: 12px; top: 50%; transform: translateY(-50%); color: #64748b; }

        .filter-section {
            background: #0f172a; padding: 10px; border-radius: 10px; margin-top: 10px;
            display: flex; flex-direction: column; gap: 8px;
        }
        .filter-row { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
        .filter-select, .filter-input {
            width: 100%; padding: 8px; background: #1e293b; border: 1px solid #334155;
            color: #fff; border-radius: 8px; font-size: 0.8rem; outline: none;
        }

        .time-filter-container {
            display: flex; flex-direction: column; gap: 6px; background: #1e293b;
            padding: 8px; border-radius: 8px; border: 1px solid #334155;
        }
        .time-filter-container label { color: #94a3b8; font-size: 0.75rem; font-weight: 600; }
        .time-filter-controls { display: flex; gap: 6px; flex-wrap: wrap; align-items: center; }
        .time-filter-container input[type="time"], .time-filter-container input[type="date"] {
            background: #0f172a; color: #fff; border: 1px solid #334155;
            padding: 4px 6px; border-radius: 6px; font-size: 0.8rem; outline: none;
        }

        .shift-mode-btn {
            padding: 5px 10px; border: 1px solid #334155; background: #0f172a;
            color: #94a3b8; font-size: 0.72rem; font-weight: 600; border-radius: 6px; cursor: pointer;
        }
        .shift-mode-btn.active { background: var(--primary); color: #fff; border-color: var(--primary); }

        /* WIDOCZNY PASEK 5 ZAKŁADEK */
        .nav-tabs {
            display: flex;
            background: #ffffff;
            border-bottom: 2px solid var(--border);
            width: 100%;
            overflow-x: auto;
            flex-shrink: 0;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
        }
        
        .tab {
            flex: 1;
            min-width: 75px;
            padding: 12px 6px;
            text-align: center;
            font-weight: 700;
            font-size: 0.75rem;
            color: var(--muted);
            cursor: pointer;
            border-bottom: 3px solid transparent;
            position: relative;
            background: #f8fafc;
            border-right: 1px solid var(--border);
            transition: all 0.2s;
            white-space: nowrap;
        }
        .tab:hover { background: #f1f5f9; color: var(--text); }
        .tab.active { color: var(--primary); background: #ffffff; border-bottom-color: var(--primary); }
        
        .badge-counter {
            position: absolute; top: 2px; right: 2px; background: var(--danger); color: #fff;
            font-size: 0.55rem; padding: 1px 4px; border-radius: 10px; font-weight: 700; display: none;
        }

        .content { padding: 16px; flex: 1; display: flex; flex-direction: column; background: var(--bg); }
        .view-section { display: none; flex: 1; flex-direction: column; }
        .view-section.active { display: flex; }

        .card {
            background: #fff; border: 1px solid var(--border); border-radius: 12px;
            padding: 12px; margin-bottom: 10px; display: flex; gap: 10px; align-items: flex-start;
        }
        .avatar {
            width: 40px; height: 40px; border-radius: 50%; background: var(--primary-light);
            color: var(--primary); display: flex; align-items: center; justify-content: center;
            font-weight: 700; font-size: 0.9rem; flex-shrink: 0;
        }
        .info { flex: 1; min-width: 0; }
        .info h4 { font-size: 0.9rem; font-weight: 700; }
        .info p { font-size: 0.8rem; color: var(--muted); margin-top: 2px; }
        .meta-info { font-size: 0.73rem; color: #475569; margin-top: 3px; font-weight: 600; display: flex; gap: 6px; flex-wrap: wrap; }

        .new-badge { background: #22c55e; color: #fff; font-size: 0.6rem; padding: 1px 5px; border-radius: 4px; font-weight: 700; }
        .date-badge { font-size: 0.68rem; color: #94a3b8; float: right; }

        .lang-badge { display: inline-block; padding: 2px 5px; border-radius: 4px; background: #f1f5f9; color: #334155; font-size: 0.7rem; font-weight: 600; margin-top: 4px; }
        .tag { display: inline-block; padding: 2px 6px; border-radius: 4px; font-size: 0.7rem; font-weight: 600; margin-top: 4px; }
        .tag-car { background: var(--primary-light); color: var(--primary); }
        .tag-food { background: var(--secondary-light); color: var(--secondary); }
        .fuel-box { background: #f8fafc; border: 1px dashed var(--border); padding: 5px; border-radius: 6px; font-size: 0.72rem; margin-top: 4px; }

        .status-badge { display: inline-block; padding: 2px 6px; border-radius: 4px; font-size: 0.72rem; font-weight: 700; margin-top: 4px; }
        .status-pending { background: #fef3c7; color: #d97706; }
        .status-accepted { background: #dcfce7; color: #15803d; }
        .status-rejected { background: #fee2e2; color: #b91c1c; }
        .status-completed { background: #e0f2fe; color: #0369a1; }

        .rating-stars { color: #f59e0b; font-size: 0.8rem; font-weight: 700; }
        .stars-interactive { font-size: 1.2rem; cursor: pointer; color: #cbd5e1; }
        .stars-interactive span.active { color: #f59e0b; }

        .btn-group { display: flex; flex-direction: column; gap: 4px; align-items: flex-end; flex-shrink: 0; }
        .btn-action {
            padding: 6px 10px; border: none; background: var(--primary); color: #fff;
            font-weight: 600; font-size: 0.75rem; border-radius: 6px; cursor: pointer; white-space: nowrap;
        }
        .btn-action.secondary { background: var(--secondary); }
        .btn-action.danger { background: var(--danger); }
        .btn-wa { background: var(--wa-color); color: #fff; text-decoration: none; font-size: 0.7rem; font-weight: 700; padding: 4px 6px; border-radius: 6px; display: inline-flex; align-items: center; gap: 3px; }

        .nav-link-btn {
            display: inline-flex; align-items: center; gap: 2px; padding: 2px 5px; border-radius: 4px;
            font-size: 0.65rem; font-weight: 700; text-decoration: none; color: #fff;
        }
        .btn-gmaps { background: #ea4335; }
        .btn-waze { background: #0099ff; }

        .add-form { background: #f8fafc; border: 1px solid var(--border); padding: 12px; border-radius: 12px; margin-bottom: 12px; }
        .add-form h4 { font-size: 0.85rem; margin-bottom: 8px; }
        .add-form input, .add-form select {
            width: 100%; padding: 8px; margin-bottom: 6px; border: 1px solid var(--border);
            border-radius: 6px; font-size: 0.8rem; outline: none; background: #fff;
        }
        .form-row-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 6px; }
        .calc-preview { background: var(--primary-light); color: var(--primary); padding: 6px; border-radius: 6px; font-size: 0.75rem; font-weight: 600; margin-bottom: 6px; text-align: center; }

        .chat-list-item { padding: 10px; border-bottom: 1px solid var(--border); cursor: pointer; display: flex; align-items: center; gap: 10px; background: #fff; }
        .chat-window { display: none; flex-direction: column; height: 350px; border: 1px solid var(--border); border-radius: 10px; overflow: hidden; background: #fff; }
        .chat-header { background: #0f172a; color: #fff; padding: 8px 12px; font-weight: 600; display: flex; justify-content: space-between; font-size: 0.85rem; }
        .chat-messages { flex: 1; padding: 10px; overflow-y: auto; background: #f8fafc; display: flex; flex-direction: column; gap: 6px; }
        .msg-bubble { max-width: 80%; padding: 6px 10px; border-radius: 10px; font-size: 0.8rem; }
        .msg-sent { align-self: flex-end; background: var(--primary); color: #fff; }
        .msg-received { align-self: flex-start; background: #e2e8f0; color: var(--text); }
        .chat-input-box { display: flex; padding: 6px; background: #fff; border-top: 1px solid var(--border); }
        .chat-input-box input { flex: 1; padding: 6px; border: 1px solid var(--border); border-radius: 6px; font-size: 0.8rem; outline: none; }

        .legal-content { background: #fff; border: 1px solid var(--border); padding: 14px; border-radius: 12px; font-size: 0.8rem; line-height: 1.4; }
        .legal-content h3 { font-size: 0.9rem; font-weight: 700; color: var(--text); margin-bottom: 6px; margin-top: 10px; }
        .legal-content h3:first-child { margin-top: 0; }

        .modal { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(15, 23, 42, 0.7); display: none; align-items: center; justify-content: center; z-index: 100; padding: 16px; }
        .modal-body { background: #fff; border-radius: 14px; padding: 20px; width: 100%; max-width: 360px; text-align: center; }
        .empty-state { text-align: center; padding: 30px 16px; color: var(--muted); font-size: 0.85rem; }

        .support-footer-box {
            background: #0f172a; color: #fff; padding: 12px; border-radius: 10px;
            margin-top: auto; margin-bottom: 6px; text-align: center; border: 1px solid #334155;
        }
    </style>
</head>
<body>

<div class="app-container">
    <div class="top-bar">
        <div class="user-badge">
            <div class="avatar-sm" id="userAvatar">?</div>
            <span id="userNameDisplay">Niezalogowany</span>
            <button class="btn-link" onclick="openAuthModal()" id="authBtn">Zaloguj</button>
        </div>
        <select id="langSelect" class="lang-select" onchange="changeLanguage(this.value)">
            <option value="pl">PL 🇵🇱</option>
            <option value="nl">NL 🇳🇱</option>
            <option value="en">EN 🇬🇧</option>
            <option value="uk">UK 🇺🇦</option>
            <option value="ro">RO 🇷🇴</option>
            <option value="es">ES 🇪🇸</option>
            <option value="pt">PT 🇵🇹</option>
        </select>
    </div>

    <div class="header">
        <h1>ShiftShare</h1>
        <p id="t-subtitle">Logistyka i żywienie dla pracowników zmianowych</p>

        <div class="app-warning-box">
            <strong id="t-warn-title">⚠️ Ważna informacja o bezpieczeństwie:</strong>
            <span id="t-warn-desc">Korzystasz z aplikacji na własną odpowiedzialność. Twórca nie odpowiada za bezpieczeństwo przejazdów ani zachowanie użytkowników. Miej włączony GPS i w razie obaw udostępnij swoją lokalizację komuś zaufanemu.</span>
        </div>

        <div class="search-box">
            <span class="search-icon">🔍</span>
            <input type="text" id="searchInput" class="search-input" placeholder="Szukaj..." onkeyup="renderAll()">
        </div>

        <div class="filter-section">
            <div class="filter-row">
                <select id="provinceSelect" class="filter-select" onchange="renderAll()">
                    <option value="ALL" id="t-prov-all">Wszystkie Prowincje</option>
                    <option value="Noord-Brabant" selected>Noord-Brabant</option>
                    <option value="Limburg">Limburg</option>
                    <option value="Gelderland">Gelderland</option>
                    <option value="Zuid-Holland">Zuid-Holland</option>
                    <option value="Noord-Holland">Noord-Holland</option>
                    <option value="Utrecht">Utrecht</option>
                </select>
                <input type="text" id="cityInput" class="filter-input" value="Helmond" placeholder="Miasto" onkeyup="renderAll()">
            </div>

            <div class="time-filter-container">
                <label id="t-lbl-filter-shift">Data i godzina zmiany:</label>
                <div class="time-filter-controls">
                    <input type="date" id="filterDate" onchange="renderAll()">
                    <button class="shift-mode-btn active" id="btn-time-all" onclick="toggleTimeFilter(false)">Wszystkie</button>
                    <button class="shift-mode-btn" id="btn-time-exact" onclick="toggleTimeFilter(true)">Szukaj godziny</button>
                    <input type="time" id="exactFilterTime" value="06:00" style="display:none;" onchange="renderAll()">
                </div>
            </div>
        </div>
    </div>

    <!-- 5 ZAKŁADEK -->
    <div class="nav-tabs">
        <div class="tab active" id="t-tab-carpool" onclick="switchView('carpool', this)">🚗 Dojazdy</div>
        <div class="tab" id="t-tab-food" onclick="switchView('food', this)">🍱 Kantyna</div>
        <div class="tab" id="t-tab-orders" onclick="switchView('orders', this)">
            📋 Rezerw.
            <span id="ordersBadge" class="badge-counter">0</span>
        </div>
        <div class="tab" id="t-tab-messages" onclick="switchView('messages', this)">
            📬 Wiadom.
            <span id="msgsBadge" class="badge-counter">0</span>
        </div>
        <div class="tab" id="t-tab-legal" onclick="switchView('legal', this)">ℹ️ Regulamin</div>
    </div>

    <div class="content">
        <!-- DOJAZDY -->
        <div id="view-carpool" class="view-section active">
            <div class="add-form">
                <h4 id="t-add-carpool">➕ Szukasz lub oferujesz transport?</h4>
                <input type="text" id="carName" placeholder="Początek trasy">
                <input type="text" id="carDest" placeholder="Cel (np. Van Loon, Son)">
                <select id="carRole" onchange="toggleSeatsInput()">
                    <option value="driver" id="t-opt-driver">Kierowca (Mam wolne miejsca)</option>
                    <option value="passenger" id="t-opt-passenger">Pasażer (Szukam transportu)</option>
                </select>

                <div id="driverDetails">
                    <div class="form-row-2">
                        <input type="text" id="carModel" placeholder="Marka i model (np. Honda Civic)">
                        <input type="text" id="carColor" placeholder="Kolor auta (np. Srebrny)">
                    </div>
                    <div class="form-row-2">
                        <input type="text" id="carPlate" placeholder="Numer rejestracyjny (np. 12-AB-34)">
                        <select id="carSmoking">
                            <option value="no" id="t-smoke-no">🚭 Autko niepalące</option>
                            <option value="yes" id="t-smoke-yes">🚬 Można palić</option>
                        </select>
                    </div>
                    
                    <div id="seatsContainer" style="margin-bottom: 6px;">
                        <label style="font-size:0.73rem; color:var(--muted);" id="t-lbl-seats">Liczba wolnych miejsc:</label>
                        <input type="number" id="carSeats" value="3" min="1" max="8">
                    </div>

                    <div style="background:#fff; border:1px solid var(--border); padding:8px; border-radius:8px; margin-bottom:6px;">
                        <div class="form-row-2">
                            <div>
                                <label style="font-size:0.73rem; color:var(--muted);" id="t-lbl-price">Koszt przejazdu (€):</label>
                                <input type="text" id="carPrice" placeholder="np. 3.00" value="3.00">
                            </div>
                            <div>
                                <label style="font-size:0.73rem; color:var(--muted);" id="t-lbl-distance">Dystans (km):</label>
                                <input type="number" id="carDistance" placeholder="np. 25" value="25" oninput="calcFuel()">
                            </div>
                        </div>
                        <div class="form-row-2">
                            <div>
                                <label style="font-size:0.73rem; color:var(--muted);" id="t-lbl-consumption">Spalanie (L/100km):</label>
                                <input type="number" step="0.1" id="carConsumption" placeholder="np. 6.5" value="6.5" oninput="calcFuel()">
                            </div>
                            <div style="display:flex; flex-direction:column; justify-content:center; gap:2px;">
                                <button type="button" class="shift-mode-btn" onclick="setNegotiablePrice()" id="t-btn-tbd">🤝 Do ustalenia</button>
                                <button type="button" class="shift-mode-btn" onclick="setFreePrice()" id="t-btn-free">🎁 Za darmo</button>
                            </div>
                        </div>
                        <div class="calc-preview" id="fuelPreview">Orientacyjne zużycie paliwa: ~1.63 L</div>
                    </div>
                </div>

                <div class="form-row-2">
                    <div>
                        <label style="font-size:0.73rem; color:var(--muted);" id="t-lbl-shift-date">Data zmiany:</label>
                        <input type="date" id="carShiftDate">
                    </div>
                    <div>
                        <label style="font-size:0.73rem; color:var(--muted);" id="t-lbl-shift-start">Godzina pracy:</label>
                        <input type="time" id="carShiftTime" value="06:00">
                    </div>
                </div>
                <div class="form-row-2" style="margin-top:6px;">
                    <div>
                        <label style="font-size:0.73rem; color:var(--muted);" id="t-lbl-depart-time">Godzina wyjazdu:</label>
                        <input type="time" id="carDepartTime" value="05:20">
                    </div>
                    <div>
                        <label style="font-size:0.73rem; color:var(--muted);">&nbsp;</label>
                        <input type="text" id="carMeetingPoint" placeholder="Miejsce zbiórki / Dworzec NS">
                    </div>
                </div>
                <input type="tel" id="carPhone" placeholder="Numer WhatsApp (opcjonalnie)" style="margin-top:6px;">

                <div class="form-row-2" style="margin-top:6px;">
                    <select id="carCountry"></select>
                    <select id="carLang"></select>
                </div>
                <button class="btn-action" style="width:100%; margin-top:6px;" id="t-btn-add-carpool" onclick="addCarpoolOffer()">Dodaj Ogłoszenie</button>
            </div>
            <div id="carpoolList"></div>
        </div>

        <!-- KANTYNA -->
        <div id="view-food" class="view-section">
            <div class="add-form">
                <h4 id="t-add-food">🍱 Masz gotowy posiłek na zmianę?</h4>
                <input type="text" id="foodTitle" placeholder="Nazwa potrawy">
                <input type="text" id="foodLoc" placeholder="Odbiór (np. Szatnia Son / Helmond)">
                
                <div class="form-row-2">
                    <div>
                        <label style="font-size:0.73rem; color:var(--muted);" id="t-lbl-food-date">Data zmiany:</label>
                        <input type="date" id="foodShiftDate">
                    </div>
                    <div>
                        <label style="font-size:0.73rem; color:var(--muted);" id="t-lbl-food-shift">Na zmianę o godz.:</label>
                        <input type="time" id="foodShiftTime" value="06:00">
                    </div>
                </div>
                <div class="form-row-2" style="margin-top:6px;">
                    <div>
                        <label style="font-size:0.73rem; color:var(--muted);" id="t-lbl-food-price">Cena za porcję (€):</label>
                        <input type="number" id="foodPrice" placeholder="€">
                    </div>
                    <div>
                        <label style="font-size:0.73rem; color:var(--muted);">&nbsp;</label>
                        <input type="tel" id="foodPhone" placeholder="WhatsApp (opcjonalnie)">
                    </div>
                </div>
                
                <div class="form-row-2" style="margin-top:6px;">
                    <select id="foodCountry"></select>
                    <select id="foodLang"></select>
                </div>
                <button class="btn-action secondary" style="width:100%; margin-top:6px;" id="t-btn-add-food" onclick="addFoodOffer()">Wystaw Posiłek</button>
            </div>
            <div id="foodList"></div>
        </div>

        <!-- REZERWACJE -->
        <div id="view-orders" class="view-section">
            <h4 style="margin-bottom:10px;" id="t-orders-title">Twoje Rezerwacje & Prośby</h4>
            <div id="ordersList"></div>
        </div>

        <!-- WIADOMOŚCI -->
        <div id="view-messages" class="view-section">
            <div id="chatList"></div>
            <div id="activeChat" class="chat-window">
                <div class="chat-header">
                    <span id="chatPartnerName">Rozmowa</span>
                    <button class="btn-link" style="color:#fff;" onclick="closeChat()">✖</button>
                </div>
                <div class="chat-messages" id="chatMessages"></div>
                <div class="chat-input-box">
                    <input type="text" id="chatMsgInput" placeholder="Napisz wiadomość...">
                    <button class="btn-action" id="t-btn-send" onclick="sendMessage()">Wyślij</button>
                </div>
            </div>
        </div>

        <!-- REGULAMIN -->
        <div id="view-legal" class="view-section">
            <div class="legal-content">
                <h3 id="t-leg-h1">ℹ️ O aplikacji ShiftShare</h3>
                <p id="t-leg-p1">ShiftShare to darmowa platforma społecznościowa dla pracowników zmianowych w Holandii.</p>

                <h3 id="t-leg-h2">🚗 Zasady Carpoolingu</h3>
                <p id="t-leg-p2">Przejazdy mają charakter koleżeński i niekomercyjny (podział kosztów paliwa).</p>

                <h3 id="t-leg-h3">🛡️ Bezpieczeństwo i Odpowiedzialność</h3>
                <p id="t-leg-p3">Aplikacja jest wyłącznie tablicą ogłoszeń. Każdy podróżuje na własną odpowiedzialność.</p>

                <h3 id="t-leg-h4">🔒 Polityka Prywatności</h3>
                <p id="t-leg-p4">Nie gromadzimy poufnych danych osobowych. Podanie nicku i WhatsApp jest dobrowolne.</p>
            </div>
        </div>

        <div class="support-footer-box">
            <h4 id="t-sup-title" style="font-size: 0.85rem; margin-bottom: 2px; color: #60a5fa;">☕ Wesprzyj rozwój aplikacji</h4>
            <p id="t-sup-desc" style="font-size: 0.72rem; color: #94a3b8; margin-bottom: 8px;">Dziękujemy za wsparcie!</p>
            <div style="display: flex; gap: 6px; justify-content: center;">
                <a href="https://revolut.me/ksiedlik" target="_blank" class="btn-action" style="background: #0075ff; font-size: 0.7rem; text-decoration: none; padding: 5px 10px;">💙 Revolut</a>
                <a href="https://wise.com/pay/me/krzysztofs1240" target="_blank" class="btn-action" style="background: #9fe870; color: #0f172a; font-size: 0.7rem; text-decoration: none; padding: 5px 10px; font-weight: 700;">💚 Wise</a>
            </div>
        </div>
    </div>
</div>

<!-- Modal Oceny -->
<div id="rateModal" class="modal">
    <div class="modal-body">
        <h3 id="t-rate-title" style="margin-bottom: 6px;">Oceń użytkownika</h3>
        <p id="t-rate-sub" style="font-size:0.8rem; color:var(--muted); margin-bottom: 10px;">Jak przebiegła transakcja?</p>
        <div class="stars-interactive" id="starContainer" style="margin-bottom: 12px;">
            <span onclick="setRating(1)">★</span><span onclick="setRating(2)">★</span><span onclick="setRating(3)">★</span><span onclick="setRating(4)">★</span><span onclick="setRating(5)">★</span>
        </div>
        <button class="btn-action" style="width:100%; margin-bottom:6px;" onclick="submitRating()" id="t-btn-send-rate">Oceń</button>
        <button class="btn-link" style="width:100%; text-align:center;" onclick="closeRateModal()" id="t-btn-cancel-rate">Anuluj</button>
    </div>
</div>

<div id="authModal" class="modal">
    <div class="modal-body">
        <h3 id="t-modal-title">Twój Profil</h3>
        <p style="font-size:0.78rem; color:var(--muted); margin-bottom:10px;" id="t-modal-sub">Wpisz swój pseudonim / nick.</p>
        <input type="text" id="authUser" placeholder="Pseudonim / Nick" style="width:100%; padding:8px; margin-bottom:6px; border:1px solid var(--border); border-radius:6px;">
        <input type="text" id="authCity" placeholder="Twoje Miasto" style="width:100%; padding:8px; margin-bottom:6px; border:1px solid var(--border); border-radius:6px;">
        <input type="tel" id="authPhone" placeholder="Numer WhatsApp (opcjonalnie)" style="width:100%; padding:8px; margin-bottom:10px; border:1px solid var(--border); border-radius:6px;">
        <button class="btn-action" style="width:100%; margin-bottom:6px;" id="t-btn-save-profile" onclick="login()">Zapisz i kontynuuj</button>
        <button class="btn-link" style="width:100%; text-align:center;" id="t-btn-cancel-profile" onclick="closeAuthModal()">Anuluj</button>
    </div>
</div>

<script>
    const translations = {
        pl: {
            subtitle: "Logistyka i żywienie dla pracowników zmianowych",
            warnTitle: "⚠️ Ważna informacja o bezpieczeństwie:",
            warnDesc: "Korzystasz z aplikacji na własną odpowiedzialność. Twórca nie odpowiada za bezpieczeństwo przejazdów ani zachowanie użytkowników. Miej włączony GPS i w razie obaw udostępnij swoją lokalizację komuś zaufanemu.",
            supTitle: "☕ Wesprzyj rozwój aplikacji", supDesc: "Dziękujemy za wsparcie!",
            unlogged: "Niezalogowany", loginBtn: "Zaloguj", logoutBtn: "Wyloguj",
            searchPlaceholder: "Szukaj...", cityPlaceholder: "Miasto", provAll: "Wszystkie Prowincje",
            lblFilterShift: "Data i godzina zmiany:", btnTimeAll: "Wszystkie", btnTimeExact: "Szukaj godziny",
            carpool: "🚗 Dojazdy", food: "🍱 Kantyna", orders: "📋 Rezerw.", messages: "📬 Wiadom.", legal: "ℹ️ Regulamin",
            addCarpoolTitle: "➕ Szukasz lub oferujesz transport?", routeStartPH: "Początek trasy", routeEndPH: "Cel (np. Van Loon)",
            optDriver: "Kierowca (Mam wolne miejsca)", optPassenger: "Pasażer (Szukam transportu)", lblShiftDate: "Data zmiany:", lblShiftStart: "Godzina pracy:", lblDepartTime: "Wyjazd:",
            lblSeats: "Miejsca:", meetingPointPH: "Miejsce zbiórki / Dworzec NS", phonePH: "WhatsApp (opcjonalnie)", btnAddCarpool: "Dodaj Ogłoszenie",
            lblPrice: "Koszt (€):", lblDistance: "Dystans (km):", lblConsumption: "Spalanie (L/100km):",
            btnTbd: "🤝 Do ustalenia", btnFree: "🎁 Za darmo", fuelCalcLabel: "Zużycie paliwa:",
            carModelPH: "Marka i model", carColorPH: "Kolor", carPlatePH: "Rejestracja",
            smokeNo: "🚭 Autko niepalące", smokeYes: "🚬 Można palić",
            smokeNoLabel: "Niepalące", smokeYesLabel: "Palenie dozwolone",
            addFoodTitle: "🍱 Masz gotowy posiłek?", foodTitlePH: "Nazwa potrawy", foodLocPH: "Odbiór (np. Szatnia)",
            lblFoodDate: "Data:", lblFoodShift: "Na zmianę o:", lblFoodPrice: "Cena (€):", btnAddFood: "Wystaw Posiłek", ordersTitle: "Twoje Rezerwacje", joinBtn: "Rezerwuj", orderBtn: "Zamów",
            writeBtn: "Napisz", sendBtn: "Wyślij", chatPH: "Napisz wiadomość...", modalTitle: "Twój Profil",
            modalSub: "Wpisz pseudonim / nick.", nickPH: "Pseudonim / Nick",
            myCityPH: "Miasto", btnSaveProfile: "Zapisz", btnCancel: "Anuluj", emptyCarpool: "Brak ogłoszeń.",
            emptyFood: "Brak posiłków.", emptyOrders: "Brak rezerwacji.", emptyMessages: "Brak wiadomości.",
            shiftLabel: "Zmiana", departLabel: "Wyjazd", meetingLabel: "Zbiórka", routeLabel: "Trasa", pickupLabel: "Odbiór", priceLabel: "Cena", seatsLabel: "Miejsca", countryLabel: "Kraj", langLabel: "Język",
            carInfoLabel: "Auto", tbdText: "Do ustalenia 🤝", freeText: "Gratis 🎁", newTag: "Nowe 🔥",
            legH1: "ℹ️ O aplikacji", legP1: "ShiftShare to platforma dla pracowników zmianowych w Holandii.",
            legH2: "🚗 Carpooling", legP2: "Przejazdy mają charakter koleżeński (podział kosztów paliwa).",
            legH3: "🛡️ Bezpieczeństwo", legP3: "Każdy podróżuje na własną odpowiedzialność.",
            legH4: "🔒 Prywatność", legP4: "Nie gromadzimy poufnych danych osobowych.",
            btnAccept: "Akceptuj ✅", btnReject: "Odrzuć ❌", btnComplete: "Zakończ ⭐",
            statusPending: "Oczekuje ⏳", statusAccepted: "Zaakceptowano ✅", statusRejected: "Odrzucono ❌", statusCompleted: "Zakończone ⭐",
            rateTitle: "Oceń użytkownika", rateSub: "Jak przebiegł przejazd?", btnRate: "Oceń"
        },
        nl: {
            subtitle: "Logistiek en maaltijden voor dienstmedewerkers",
            warnTitle: "⚠️ Belangrijke veiligheidsinformatie:",
            warnDesc: "Je reist op eigen risico. De maker is niet verantwoordelijk voor de ritten of het gedrag van gebruikers. Houd je GPS aan en deel je locatie met een vertrouwd persoon als je je onveilig voelt.",
            supTitle: "☕ Steun de app", supDesc: "Bedankt!",
            unlogged: "Niet ingelogd", loginBtn: "Inloggen", logoutBtn: "Uitloggen",
            searchPlaceholder: "Zoeken...", cityPlaceholder: "Stad", provAll: "Alle Provincies",
            lblFilterShift: "Datum en tijd:", btnTimeAll: "Alle", btnTimeExact: "Op tijd",
            carpool: "🚗 Carpool", food: "🍱 Kantine", orders: "📋 Reserv.", messages: "📬 Berichten", legal: "ℹ️ Reglement",
            addCarpoolTitle: "➕ Bied je een rit aan?", routeStartPH: "Vertrekpunt", routeEndPH: "Bestemming",
            optDriver: "Bestuurder", optPassenger: "Passagier", lblShiftDate: "Datum:", lblShiftStart: "Werktijd:", lblDepartTime: "Vertrek:",
            lblSeats: "Plaatsen:", meetingPointPH: "Ophaalpunt", phonePH: "WhatsApp", btnAddCarpool: "Plaats",
            lblPrice: "Prijs (€):", lblDistance: "Afstand (km):", lblConsumption: "Verbruik (L/100km):",
            btnTbd: "🤝 N.t.b.", btnFree: "🎁 Gratis", fuelCalcLabel: "Brandstof:",
            carModelPH: "Merk & model", carColorPH: "Kleur", carPlatePH: "Kenteken",
            smokeNo: "🚭 Rookvrij", smokeYes: "🚬 Roken mag",
            smokeNoLabel: "Rookvrij", smokeYesLabel: "Roken toegestaan",
            addFoodTitle: "🍱 Maaltijd aanbieden?", foodTitlePH: "Gerecht", foodLocPH: "Ophalen",
            lblFoodDate: "Datum:", lblFoodShift: "Voor dienst:", lblFoodPrice: "Prijs (€):", btnAddFood: "Plaats", ordersTitle: "Reserveringen", joinBtn: "Reserveren", orderBtn: "Bestel",
            writeBtn: "Bericht", sendBtn: "Verstuur", chatPH: "Schrijf...", modalTitle: "Profiel",
            modalSub: "Vul je nick in.", nickPH: "Bijnaam",
            myCityPH: "Stad", btnSaveProfile: "Opslaan", btnCancel: "Annuleren", emptyCarpool: "Geen ritten.",
            emptyFood: "Geen maaltijden.", emptyOrders: "Geen reserveringen.", emptyMessages: "Geen berichten.",
            shiftLabel: "Dienst", departLabel: "Vertrek", meetingLabel: "Ophaalpunt", routeLabel: "Route", pickupLabel: "Ophalen", priceLabel: "Prijs", seatsLabel: "Plaatsen", countryLabel: "Land", langLabel: "Taal",
            carInfoLabel: "Auto", tbdText: "N.t.b. 🤝", freeText: "Gratis 🎁", newTag: "Nieuw 🔥",
            legH1: "ℹ️ Over ShiftShare", legP1: "ShiftShare is een platform voor dienstmedewerkers in Nederland.",
            legH2: "🚗 Carpool", legP2: "Ritten zijn collegiaal en niet-commercieel.",
            legH3: "🛡️ Veiligheid", legP3: "Elke gebruiker reist op eigen risico.",
            legH4: "🔒 Privacy", legP4: "We slaan geen gevoelige gegevens op.",
            btnAccept: "Accepteren ✅", btnReject: "Weigeren ❌", btnComplete: "Voltooien ⭐",
            statusPending: "Wacht ⏳", statusAccepted: "Goedgekeurd ✅", statusRejected: "Geweigerd ❌", statusCompleted: "Voltooid ⭐",
            rateTitle: "Beoordelen", rateSub: "Hoe verliep de rit?", btnRate: "Beoordeel"
        },
        en: {
            subtitle: "Logistics and meal sharing for shift workers",
            warnTitle: "⚠️ Important safety information:",
            warnDesc: "You travel at your own risk. The creator is not responsible for travel safety or user behavior. Keep your GPS on and share your location with a trusted person if needed.",
            supTitle: "☕ Support the App", supDesc: "Thank you for your support!",
            unlogged: "Not logged in", loginBtn: "Log in", logoutBtn: "Log out",
            searchPlaceholder: "Search...", cityPlaceholder: "City", provAll: "All Provinces",
            lblFilterShift: "Shift date & time:", btnTimeAll: "All", btnTimeExact: "Search time",
            carpool: "🚗 Carpool", food: "🍱 Canteen", orders: "📋 Bookings", messages: "📬 Messages", legal: "ℹ️ Terms",
            addCarpoolTitle: "➕ Offering or looking for a ride?", routeStartPH: "Starting point", routeEndPH: "Destination",
            optDriver: "Driver (Seats available)", optPassenger: "Passenger (Looking for ride)", lblShiftDate: "Shift date:", lblShiftStart: "Work time:", lblDepartTime: "Departure:",
            lblSeats: "Seats:", meetingPointPH: "Pickup location", phonePH: "WhatsApp (optional)", btnAddCarpool: "Post Ride",
            lblPrice: "Cost (€):", lblDistance: "Distance (km):", lblConsumption: "Consumption (L/100km):",
            btnTbd: "🤝 To be agreed", btnFree: "🎁 Free", fuelCalcLabel: "Fuel consumption:",
            carModelPH: "Car brand & model", carColorPH: "Color", carPlatePH: "License plate",
            smokeNo: "🚭 Non-smoking car", smokeYes: "🚬 Smoking allowed",
            smokeNoLabel: "Non-smoking", smokeYesLabel: "Smoking allowed",
            addFoodTitle: "🍱 Offering a meal?", foodTitlePH: "Dish name", foodLocPH: "Pick up location",
            lblFoodDate: "Date:", lblFoodShift: "For shift at:", lblFoodPrice: "Price (€):", btnAddFood: "Post Meal", ordersTitle: "Your Bookings", joinBtn: "Book", orderBtn: "Order",
            writeBtn: "Message", sendBtn: "Send", chatPH: "Type a message...", modalTitle: "Your Profile",
            modalSub: "Enter a nickname.", nickPH: "Nickname",
            myCityPH: "City", btnSaveProfile: "Save", btnCancel: "Cancel", emptyCarpool: "No offers.",
            emptyFood: "No meals.", emptyOrders: "No bookings.", emptyMessages: "No messages.",
            shiftLabel: "Shift", departLabel: "Departure", meetingLabel: "Meeting Point", routeLabel: "Route", pickupLabel: "Pickup", priceLabel: "Price", seatsLabel: "Seats", countryLabel: "Country", langLabel: "Language",
            carInfoLabel: "Car", tbdText: "To be agreed 🤝", freeText: "Free 🎁", newTag: "New 🔥",
            legH1: "ℹ️ About ShiftShare", legP1: "ShiftShare is a platform for shift workers in the Netherlands.",
            legH2: "🚗 Carpool Rules", legP2: "Rides are non-commercial (fuel cost sharing).",
            legH3: "🛡️ Safety", legP3: "Every user travels at their own risk.",
            legH4: "🔒 Privacy", legP4: "We do not store sensitive personal data.",
            btnAccept: "Accept ✅", btnReject: "Reject ❌", btnComplete: "Complete ⭐",
            statusPending: "Pending ⏳", statusAccepted: "Accepted ✅", statusRejected: "Rejected ❌", statusCompleted: "Completed ⭐",
            rateTitle: "Rate user", rateSub: "How was the trip?", btnRate: "Rate"
        },
        uk: {
            subtitle: "Логістика та харчування для працівників змін",
            warnTitle: "⚠️ Важлива інформація про безпеку:",
            warnDesc: "Ви подорожуєте на власний ризик. Творець не несе відповідальності за безпеку поїздок чи поведінку користувачів. Тримайте GPS увімкненим та діліться геолокацією з довіреними особами.",
            supTitle: "☕ Підтримати додаток", supDesc: "Дякуємо за підтримку!",
            unlogged: "Неавторизований", loginBtn: "Увійти", logoutBtn: "Вийти",
            searchPlaceholder: "Пошук...", cityPlaceholder: "Місто", provAll: "Усі Провінції",
            lblFilterShift: "Дата та час зміни:", btnTimeAll: "Усі", btnTimeExact: "Шукати час",
            carpool: "🚗 Поїздки", food: "🍱 Їдальня", orders: "📋 Брон.", messages: "📬 Повідом.", legal: "ℹ️ Правила",
            addCarpoolTitle: "➕ Пропонуєте або шукаєте транспорт?", routeStartPH: "Початок маршруту", routeEndPH: "Пункт призначення",
            optDriver: "Водій (Є вільні місця)", optPassenger: "Пасажир (Шукаю транспорт)", lblShiftDate: "Дата зміни:", lblShiftStart: "Початок роботи:", lblDepartTime: "Виїзд:",
            lblSeats: "Місця:", meetingPointPH: "Місце збору", phonePH: "WhatsApp (опціонально)", btnAddCarpool: "Опублікувати",
            lblPrice: "Вартість (€):", lblDistance: "Дистанція (км):", lblConsumption: "Витрата (Л/100км):",
            btnTbd: "🤝 За домовленістю", btnFree: "🎁 Безкоштовно", fuelCalcLabel: "Витрата палива:",
            carModelPH: "Марка та модель", carColorPH: "Колір", carPlatePH: "Номер",
            smokeNo: "🚭 Непатяще авто", smokeYes: "🚬 Можна палити",
            smokeNoLabel: "Непатяще", smokeYesLabel: "Можна палити",
            addFoodTitle: "🍱 Маєте готовий обід?", foodTitlePH: "Назва страви", foodLocPH: "Де забрати",
            lblFoodDate: "Дата:", lblFoodShift: "На зміну о:", lblFoodPrice: "Ціна (€):", btnAddFood: "Опублікувати Обід", ordersTitle: "Ваші Бронювання", joinBtn: "Забронювати", orderBtn: "Замовити",
            writeBtn: "Написати", sendBtn: "Надіслати", chatPH: "Напишіть...", modalTitle: "Ваш Профіль",
            modalSub: "Вкажіть нікнейм.", nickPH: "Нікнейм",
            myCityPH: "Місто", btnSaveProfile: "Зберегти", btnCancel: "Скасувати", emptyCarpool: "Немає оголошень.",
            emptyFood: "Немає обідів.", emptyOrders: "Немає бронювань.", emptyMessages: "Немає повідомлень.",
            shiftLabel: "Зміна", departLabel: "Виїзд", meetingLabel: "Місце збору", routeLabel: "Маршрут", pickupLabel: "Забрати", priceLabel: "Ціна", seatsLabel: "Місця", countryLabel: "Країна", langLabel: "Мова",
            carInfoLabel: "Авто", tbdText: "За домовленістю 🤝", freeText: "Безкоштовно 🎁", newTag: "Нове 🔥",
            legH1: "ℹ️ Про ShiftShare", legP1: "ShiftShare — платформа для працівників змін у Нідерландах.",
            legH2: "🚗 Правила поїздок", legP2: "Поїздки мають некомерційний характер (розподіл палива).",
            legH3: "🛡️ Безпека", legP3: "Кожен користувач подорожує на власний ризик.",
            legH4: "🔒 Конфіденційність", legP4: "Ми не зберігаємо чутливих даних.",
            btnAccept: "Прийняти ✅", btnReject: "Відхилити ❌", btnComplete: "Завершити ⭐",
            statusPending: "Очікує ⏳", statusAccepted: "Прийнято ✅", statusRejected: "Відхилено ❌", statusCompleted: "Завершено ⭐",
            rateTitle: "Оцінити", rateSub: "Як пройшла поїздка?", btnRate: "Оцінити"
        },
        ro: {
            subtitle: "Logistică și masă pentru lucrătorii în schimburi",
            warnTitle: "⚠️ Informație importantă de siguranță:",
            warnDesc: "Călătorești pe propria răspundere. Creatorul nu este responsabil pentru siguranța curselor sau comportamentul utilizatorilor. Ține GPS-ul pornit și distribuie locația unei persoane de încredere.",
            supTitle: "☕ Susține aplicația", supDesc: "Mulțumim pentru sprijin!",
            unlogged: "Neautentificat", loginBtn: "Autentificare", logoutBtn: "Deconectare",
            searchPlaceholder: "Caută...", cityPlaceholder: "Oraș", provAll: "Toate Provinciile",
            lblFilterShift: "Data și ora schimbului:", btnTimeAll: "Toate", btnTimeExact: "După oră",
            carpool: "🚗 Transport", food: "🍱 Cantină", orders: "📋 Rezerv.", messages: "📬 Mesaje", legal: "ℹ️ Termeni",
            addCarpoolTitle: "➕ Oferi sau cauți transport?", routeStartPH: "Punct plecare", routeEndPH: "Destinație",
            optDriver: "Șofer (Locuri libere)", optPassenger: "Pasager (Caut transport)", lblShiftDate: "Data schimbului:", lblShiftStart: "Ora începerii:", lblDepartTime: "Plecare:",
            lblSeats: "Locuri:", meetingPointPH: "Punct de întâlnire", phonePH: "WhatsApp (opțional)", btnAddCarpool: "Adaugă Anunț",
            lblPrice: "Cost (€):", lblDistance: "Distanță (km):", lblConsumption: "Consum (L/100km):",
            btnTbd: "🤝 De convenit", btnFree: "🎁 Gratuit", fuelCalcLabel: "Consum combustibil:",
            carModelPH: "Marcă și model", carColorPH: "Culoare", carPlatePH: "Număr",
            smokeNo: "🚭 Mașină nefumători", smokeYes: "🚬 Fumatul permis",
            smokeNoLabel: "Nefumători", smokeYesLabel: "Fumat permis",
            addFoodTitle: "🍱 Ai o masă pregătită?", foodTitlePH: "Nume mâncare", foodLocPH: "Loc ridicare",
            lblFoodDate: "Data:", lblFoodShift: "Pentru schimbul:", lblFoodPrice: "Preț (€):", btnAddFood: "Adaugă Mâncare", ordersTitle: "Rezervările Tale", joinBtn: "Rezervă", orderBtn: "Comandă",
            writeBtn: "Scrie", sendBtn: "Trimite", chatPH: "Scrie un mesaj...", modalTitle: "Profilul Tău",
            modalSub: "Introdu un pseudonim.", nickPH: "Pseudonim",
            myCityPH: "Oraș", btnSaveProfile: "Salvează", btnCancel: "Anulează", emptyCarpool: "Fără anunțuri.",
            emptyFood: "Fără mâncare.", emptyOrders: "Fără rezervări.", emptyMessages: "Fără mesaje.",
            shiftLabel: "Schimb", departLabel: "Plecare", meetingLabel: "Întâlnire", routeLabel: "Traseu", pickupLabel: "Ridicare", priceLabel: "Preț", seatsLabel: "Locuri", countryLabel: "Țară", langLabel: "Limbă",
            carInfoLabel: "Mașină", tbdText: "De convenit 🤝", freeText: "Gratis 🎁", newTag: "Nou 🔥",
            legH1: "ℹ️ Despre ShiftShare", legP1: "ShiftShare este o platformă pentru lucrătorii în schimburi din Olanda.",
            legH2: "🚗 Regulile transportului", legP2: "Cursele au caracter colegial (împărțirea costului de combustibil).",
            legH3: "🛡️ Securitate", legP3: "Fiecare călătorește pe propriul risc.",
            legH4: "🔒 Confidențialitate", legP4: "Nu stocăm date personale sensibile.",
            btnAccept: "Acceptă ✅", btnReject: "Respinge ❌", btnComplete: "Finalizează ⭐",
            statusPending: "În așteptare ⏳", statusAccepted: "Acceptat ✅", statusRejected: "Respins ❌", statusCompleted: "Finalizat ⭐",
            rateTitle: "Evaluează", rateSub: "Cum a fost cursa?", btnRate: "Evaluează"
        },
        es: {
            subtitle: "Logística y comida para trabajadores por turnos",
            warnTitle: "⚠️ Información importante de seguridad:",
            warnDesc: "Viajas bajo tu propio riesgo. El creador no es responsable de la seguridad de los viajes ni del comportamiento de los usuarios. Mantén tu GPS encendido y comparte tu ubicación con alguien de confianza.",
            supTitle: "☕ Apoya la aplicación", supDesc: "¡Gracias por el apoyo!",
            unlogged: "No conectado", loginBtn: "Iniciar sesión", logoutBtn: "Cerrar sesión",
            searchPlaceholder: "Buscar...", cityPlaceholder: "Ciudad", provAll: "Todas las Provincias",
            lblFilterShift: "Fecha y hora del turno:", btnTimeAll: "Todas", btnTimeExact: "Buscar hora",
            carpool: "🚗 Carpool", food: "🍱 Cantina", orders: "📋 Reservas", messages: "📬 Mensajes", legal: "ℹ️ Normas",
            addCarpoolTitle: "➕ ¿Ofreces o buscas transporte?", routeStartPH: "Origen", routeEndPH: "Destino",
            optDriver: "Conductor (Asientos libres)", optPassenger: "Pasajero (Busca viaje)", lblShiftDate: "Fecha del turno:", lblShiftStart: "Hora de trabajo:", lblDepartTime: "Salida:",
            lblSeats: "Asientos:", meetingPointPH: "Punto de encuentro", phonePH: "WhatsApp (opcional)", btnAddCarpool: "Publicar Viaje",
            lblPrice: "Coste (€):", lblDistance: "Distancia (km):", lblConsumption: "Consumo (L/100km):",
            btnTbd: "🤝 A convenir", btnFree: "🎁 Gratis", fuelCalcLabel: "Consumo de combustible:",
            carModelPH: "Marca y modelo", carColorPH: "Color", carPlatePH: "Matrícula",
            smokeNo: "🚭 Coche libre de humo", smokeYes: "🚬 Se permite fumar",
            smokeNoLabel: "Libre de humo", smokeYesLabel: "Fumar permitido",
            addFoodTitle: "🍱 ¿Tienes comida preparada?", foodTitlePH: "Nombre del plato", foodLocPH: "Lugar de recogida",
            lblFoodDate: "Fecha:", lblFoodShift: "Para el turno:", lblFoodPrice: "Precio (€):", btnAddFood: "Publicar Comida", ordersTitle: "Tus Reservas", joinBtn: "Reservar", orderBtn: "Pedir",
            writeBtn: "Escribir", sendBtn: "Enviar", chatPH: "Escribe un mensaje...", modalTitle: "Tu Perfil",
            modalSub: "Introduce un apodo.", nickPH: "Apodo",
            myCityPH: "Ciudad", btnSaveProfile: "Guardar", btnCancel: "Cancelar", emptyCarpool: "No hay anuncios.",
            emptyFood: "No hay comida.", emptyOrders: "No hay reservas.", emptyMessages: "No hay mensajes.",
            shiftLabel: "Turno", departLabel: "Salida", meetingLabel: "Punto de encuentro", routeLabel: "Ruta", pickupLabel: "Recogida", priceLabel: "Precio", seatsLabel: "Asientos", countryLabel: "País", langLabel: "Idioma",
            carInfoLabel: "Coche", tbdText: "A convenir 🤝", freeText: "Gratis 🎁", newTag: "Nuevo 🔥",
            legH1: "ℹ️ Acerca de ShiftShare", legP1: "ShiftShare es una plataforma para trabajadores por turnos en los Países Bajos.",
            legH2: "🚗 Normas de Carpool", legP2: "Los viajes no son comerciales (compartir combustible).",
            legH3: "🛡️ Seguridad", legP3: "Cada usuario viaja bajo su propio riesgo.",
            legH4: "🔒 Privacidad", legP4: "No almacenamos datos personales sensibles.",
            btnAccept: "Aceptar ✅", btnReject: "Rechazar ❌", btnComplete: "Completar ⭐",
            statusPending: "Pendiente ⏳", statusAccepted: "Aceptado ✅", statusRejected: "Rechazado ❌", statusCompleted: "Completado ⭐",
            rateTitle: "Calificar", rateSub: "¿Cómo fue el viaje?", btnRate: "Calificar"
        },
        pt: {
            subtitle: "Logística e alimentação para trabalhadores por turnos",
            warnTitle: "⚠️ Informação importante de segurança:",
            warnDesc: "Você viaja por sua conta e risco. O criador não é responsável pela segurança das viagens ou comportamento dos usuários. Mantenha o GPS ligado e compartilhe sua localização com alguém de confiança.",
            supTitle: "☕ Apoie o aplicativo", supDesc: "Obrigado pelo apoio!",
            unlogged: "Não conectado", loginBtn: "Entrar", logoutBtn: "Sair",
            searchPlaceholder: "Pesquisar...", cityPlaceholder: "Cidade", provAll: "Todas as Províncias",
            lblFilterShift: "Data e hora do turno:", btnTimeAll: "Todas", btnTimeExact: "Buscar hora",
            carpool: "🚗 Carona", food: "🍱 Kantina", orders: "📋 Reservas", messages: "📬 Mensagens", legal: "ℹ️ Termos",
            addCarpoolTitle: "➕ Oferece ou procura carona?", routeStartPH: "Origem", routeEndPH: "Destino",
            optDriver: "Motorista (Vagas disponíveis)", optPassenger: "Passageiro (Procura carona)", lblShiftDate: "Data do turno:", lblShiftStart: "Hora do trabalho:", lblDepartTime: "Saída:",
            lblSeats: "Vagas:", meetingPointPH: "Ponto de encontro", phonePH: "WhatsApp (opcional)", btnAddCarpool: "Publicar Carona",
            lblPrice: "Custo (€):", lblDistance: "Distância (km):", lblConsumption: "Consumo (L/100km):",
            btnTbd: "🤝 A combinar", btnFree: "🎁 Grátis", fuelCalcLabel: "Consumo de combustível:",
            carModelPH: "Marca e modelo", carColorPH: "Cor", carPlatePH: "Placa",
            smokeNo: "🚭 Carro não fumante", smokeYes: "🚬 Permitido fumar",
            smokeNoLabel: "Não fumante", smokeYesLabel: "Fumar permitido",
            addFoodTitle: "🍱 Tem uma refeição preparada?", foodTitlePH: "Nome do prato", foodLocPH: "Local de retirada",
            lblFoodDate: "Data:", lblFoodShift: "Para o turno:", lblFoodPrice: "Preço (€):", btnAddFood: "Publicar Refeição", ordersTitle: "Suas Reservas", joinBtn: "Reservar", orderBtn: "Pedir",
            writeBtn: "Mensagem", sendBtn: "Enviar", chatPH: "Escreva uma mensagem...", modalTitle: "Seu Perfil",
            modalSub: "Digite um apelido.", nickPH: "Apelido",
            myCityPH: "Cidade", btnSaveProfile: "Salvar", btnCancel: "Cancelar", emptyCarpool: "Sem anúncios.",
            emptyFood: "Sem refeições.", emptyOrders: "Sem reservas.", emptyMessages: "Sem mensagens.",
            shiftLabel: "Turno", departLabel: "Saída", meetingLabel: "Punto de encontro", routeLabel: "Rota", pickupLabel: "Retirada", priceLabel: "Preço", seatsLabel: "Vagas", countryLabel: "País", langLabel: "Idioma",
            carInfoLabel: "Carro", tbdText: "A combinar 🤝", freeText: "Grátis 🎁", newTag: "Novo 🔥",
            legH1: "ℹ️ Sobre o ShiftShare", legP1: "ShiftShare é uma plataforma para trabalhadores por turnos na Holanda.",
            legH2: "🚗 Regras de Carona", legP2: "As viagens não são comerciais (divisão de combustível).",
            legH3: "🛡️ Segurança", legP3: "Cada utilizador viaja por sua conta e risco.",
            legH4: "🔒 Privacidade", legP4: "Não armazenamos dados pessoais sensíveis.",
            btnAccept: "Aceitar ✅", btnReject: "Rejeitar ❌", btnComplete: "Concluir ⭐",
            statusPending: "Pendente ⏳", statusAccepted: "Aceito ✅", statusRejected: "Rejeitado ❌", statusCompleted: "Concluído ⭐",
            rateTitle: "Avaliar", rateSub: "Como foi a viagem?", btnRate: "Avaliar"
        }
    };

    const countryNames = {
        pl: { pl: "🇵🇱 Polska", uk: "🇺🇦 Україна", nl: "🇳🇱 Nederland", ro: "🇷🇴 România", es: "🇪🇸 España", pt: "🇵🇹 Portugal", other: "🇬🇧 Inny" },
        nl: { pl: "🇵🇱 Polen", uk: "🇺🇦 Oekraïne", nl: "🇳🇱 Nederland", ro: "🇷🇴 Roemenië", es: "🇪🇸 Spanje", pt: "🇵🇹 Portugal", other: "🇬🇧 Overig" },
        en: { pl: "🇵🇱 Poland", uk: "🇺🇦 Ukraine", nl: "🇳🇱 Netherlands", ro: "🇷🇴 Romania", es: "🇪🇸 Spain", pt: "🇵🇹 Portugal", other: "🇬🇧 Other" },
        uk: { pl: "🇵🇱 Польща", uk: "🇺🇦 Україна", nl: "🇳🇱 Нідерланди", ro: "🇷🇴 Румунія", es: "🇪🇸 Іспанія", pt: "🇵🇹 Португалія", other: "🇬🇧 Інша" },
        ro: { pl: "🇵🇱 Polonia", uk: "🇺🇦 Ucraina", nl: "🇳🇱 Olanda", ro: "🇷🇴 România", es: "🇪🇸 Spania", pt: "🇵🇹 Portugalia", other: "🇬🇧 Alta" },
        es: { pl: "🇵🇱 Polonia", uk: "🇺🇦 Ucrania", nl: "🇳🇱 Países Bajos", ro: "🇷🇴 Rumanía", es: "🇪🇸 España", pt: "🇵🇹 Portugal", other: "🇬🇧 Otro" },
        pt: { pl: "🇵🇱 Polônia", uk: "🇺🇦 Ucrânia", nl: "🇳🇱 Países Baixos", ro: "🇷🇴 Romênia", es: "🇪🇸 Espanha", pt: "🇵🇹 Portugal", other: "🇬🇧 Outro" }
    };

    const languageNames = {
        pl: { pl: "Język: Polski", nl: "Język: Nederlands", en: "Język: English", uk: "Język: Українська", ro: "Język: Română", es: "Język: Español", pt: "Język: Português" },
        nl: { pl: "Taal: Pools", nl: "Taal: Nederlands", en: "Taal: Engels", uk: "Taal: Oekraïens", ro: "Taal: Roemeens", es: "Taal: Spaans", pt: "Taal: Portugees" },
        en: { pl: "Language: Polish", nl: "Language: Dutch", en: "Language: English", uk: "Language: Ukrainian", ro: "Language: Romanian", es: "Language: Spanish", pt: "Language: Portuguese" },
        uk: { pl: "Мова: Польська", nl: "Мова: Нідерландська", en: "Мова: Англійська", uk: "Мова: Українська", ro: "Мова: Румунська", es: "Мова: Іспанська", pt: "Мова: Португальська" },
        ro: { pl: "Limbă: Poloneză", nl: "Limbă: Olandeză", en: "Limbă: Engleză", uk: "Limbă: Ucraineană", ro: "Limbă: Română", es: "Limbă: Spaniolă", pt: "Limbă: Portugheză" },
        es: { pl: "Idioma: Polaco", nl: "Idioma: Holandés", en: "Idioma: Inglés", uk: "Idioma: Ucraniano", ro: "Idioma: Rumano", es: "Idioma: Español", pt: "Idioma: Portugués" },
        pt: { pl: "Idioma: Polonês", nl: "Idioma: Holandês", en: "Idioma: Inglês", uk: "Idioma: Ucraniano", ro: "Idioma: Romeno", es: "Idioma: Espanha", pt: "Idioma: Português" }
    };

    let currentLang = 'pl';
    let currentUser = JSON.parse(localStorage.getItem('ss_user')) || null;
    let filterExactShift = false;
    let activeChatPartner = null;
    let ratingTargetUser = null;
    let selectedStars = 5;

    let carpoolOffers = JSON.parse(localStorage.getItem('ss_carpool')) || [];
    let foodOffers = JSON.parse(localStorage.getItem('ss_food')) || [];
    let bookings = JSON.parse(localStorage.getItem('ss_bookings')) || [];
    let chats = JSON.parse(localStorage.getItem('ss_chats')) || [];
    let ratings = JSON.parse(localStorage.getItem('ss_ratings')) || {};

    const todayStr = new Date().toISOString().split('T')[0];
    window.addEventListener('DOMContentLoaded', () => {
        document.getElementById('filterDate').value = todayStr;
        document.getElementById('carShiftDate').value = todayStr;
        document.getElementById('foodShiftDate').value = todayStr;
        cleanExpiredOffers();
    });

    function cleanExpiredOffers() {
        const now = new Date();
        now.setHours(0,0,0,0);
        carpoolOffers = carpoolOffers.filter(item => !item.shiftDate || new Date(item.shiftDate) >= now);
        foodOffers = foodOffers.filter(item => !item.shiftDate || new Date(item.shiftDate) >= now);
        localStorage.setItem('ss_carpool', JSON.stringify(carpoolOffers));
        localStorage.setItem('ss_food', JSON.stringify(foodOffers));
    }

    function calcFuel() {
        const dist = parseFloat(document.getElementById('carDistance').value) || 0;
        const cons = parseFloat(document.getElementById('carConsumption').value) || 0;
        const totalLiters = (dist * cons / 100).toFixed(2);
        const t = translations[currentLang] || translations.pl;
        document.getElementById('fuelPreview').innerText = `${t.fuelCalcLabel} ~${totalLiters} L`;
    }

    function setNegotiablePrice() {
        const t = translations[currentLang] || translations.pl;
        document.getElementById('carPrice').value = t.tbdText;
    }

    function setFreePrice() {
        const t = translations[currentLang] || translations.pl;
        document.getElementById('carPrice').value = t.freeText;
    }

    function populateDropdowns() {
        const cNames = countryNames[currentLang] || countryNames.pl;
        const lNames = languageNames[currentLang] || languageNames.pl;

        [document.getElementById('carCountry'), document.getElementById('foodCountry')].forEach(s => {
            s.innerHTML = Object.entries(cNames).map(([k, v]) => `<option value="${v}">${v}</option>`).join('');
        });
        [document.getElementById('carLang'), document.getElementById('foodLang')].forEach(s => {
            s.innerHTML = Object.entries(lNames).map(([k, v]) => `<option value="${v}">${v}</option>`).join('');
        });
    }

    function toggleSeatsInput() {
        const role = document.getElementById('carRole').value;
        document.getElementById('driverDetails').style.display = role === 'passenger' ? 'none' : 'block';
    }

    function toggleTimeFilter(isExact) {
        filterExactShift = isExact;
        document.getElementById('btn-time-all').classList.toggle('active', !isExact);
        document.getElementById('btn-time-exact').classList.toggle('active', isExact);
        document.getElementById('exactFilterTime').style.display = isExact ? 'inline-block' : 'none';
        renderAll();
    }

    function changeLanguage(lang) {
        currentLang = lang;
        const t = translations[lang] || translations.pl;

        document.getElementById('t-subtitle').innerText = t.subtitle;
        document.getElementById('t-warn-title').innerText = t.warnTitle;
        document.getElementById('t-warn-desc').innerText = t.warnDesc;
        document.getElementById('t-sup-title').innerText = t.supTitle;
        document.getElementById('t-sup-desc').innerText = t.supDesc;

        document.getElementById('searchInput').placeholder = t.searchPlaceholder;
        document.getElementById('cityInput').placeholder = t.cityPlaceholder;
        document.getElementById('t-prov-all').innerText = t.provAll;

        document.getElementById('t-lbl-filter-shift').innerText = t.lblFilterShift;
        document.getElementById('btn-time-all').innerText = t.btnTimeAll;
        document.getElementById('btn-time-exact').innerText = t.btnTimeExact;

        document.getElementById('t-tab-carpool').innerText = t.carpool;
        document.getElementById('t-tab-food').innerText = t.food;
        document.getElementById('t-tab-orders').firstChild.textContent = t.orders + " ";
        document.getElementById('t-tab-messages').firstChild.textContent = t.messages + " ";
        document.getElementById('t-tab-legal').innerText = t.legal;

        document.getElementById('t-add-carpool').innerText = t.addCarpoolTitle;
        document.getElementById('carName').placeholder = t.routeStartPH;
        document.getElementById('carDest').placeholder = t.routeEndPH;
        document.getElementById('t-opt-driver').innerText = t.optDriver;
        document.getElementById('t-opt-passenger').innerText = t.optPassenger;
        document.getElementById('carModel').placeholder = t.carModelPH;
        document.getElementById('carColor').placeholder = t.carColorPH;
        document.getElementById('carPlate').placeholder = t.carPlatePH;
        document.getElementById('t-smoke-no').innerText = t.smokeNo;
        document.getElementById('t-smoke-yes').innerText = t.smokeYes;

        document.getElementById('t-lbl-shift-date').innerText = t.lblShiftDate;
        document.getElementById('t-lbl-shift-start').innerText = t.lblShiftStart;
        document.getElementById('t-lbl-depart-time').innerText = t.lblDepartTime;
        document.getElementById('t-lbl-seats').innerText = t.lblSeats;
        document.getElementById('t-lbl-price').innerText = t.lblPrice;
        document.getElementById('t-lbl-distance').innerText = t.lblDistance;
        document.getElementById('t-lbl-consumption').innerText = t.lblConsumption;
        document.getElementById('t-btn-tbd').innerText = t.btnTbd;
        document.getElementById('t-btn-free').innerText = t.btnFree;
        document.getElementById('carMeetingPoint').placeholder = t.meetingPointPH;
        document.getElementById('carPhone').placeholder = t.phonePH;
        document.getElementById('t-btn-add-carpool').innerText = t.btnAddCarpool;

        document.getElementById('t-add-food').innerText = t.addFoodTitle;
        document.getElementById('foodTitle').placeholder = t.foodTitlePH;
        document.getElementById('foodLoc').placeholder = t.foodLocPH;
        document.getElementById('t-lbl-food-date').innerText = t.lblFoodDate;
        document.getElementById('t-lbl-food-shift').innerText = t.lblFoodShift;
        document.getElementById('t-lbl-food-price').innerText = t.lblFoodPrice;
        document.getElementById('foodPhone').placeholder = t.phonePH;
        document.getElementById('t-btn-add-food').innerText = t.btnAddFood;

        document.getElementById('t-orders-title').innerText = t.ordersTitle;
        document.getElementById('chatMsgInput').placeholder = t.chatPH;
        document.getElementById('t-btn-send').innerText = t.sendBtn;

        document.getElementById('t-leg-h1').innerText = t.legH1;
        document.getElementById('t-leg-p1').innerText = t.legP1;
        document.getElementById('t-leg-h2').innerText = t.legH2;
        document.getElementById('t-leg-p2').innerText = t.legP2;
        document.getElementById('t-leg-h3').innerText = t.legH3;
        document.getElementById('t-leg-p3').innerText = t.legP3;
        document.getElementById('t-leg-h4').innerText = t.legH4;
        document.getElementById('t-leg-p4').innerText = t.legP4;

        document.getElementById('t-modal-title').innerText = t.modalTitle;
        document.getElementById('t-modal-sub').innerText = t.modalSub;
        document.getElementById('authUser').placeholder = t.nickPH;
        document.getElementById('authCity').placeholder = t.myCityPH;
        document.getElementById('authPhone').placeholder = t.phonePH;
        document.getElementById('t-btn-save-profile').innerText = t.btnSaveProfile;
        document.getElementById('t-btn-cancel-profile').innerText = t.btnCancel;

        document.getElementById('t-rate-title').innerText = t.rateTitle;
        document.getElementById('t-rate-sub').innerText = t.rateSub;
        document.getElementById('t-btn-send-rate').innerText = t.btnRate;
        document.getElementById('t-btn-cancel-rate').innerText = t.btnCancel;

        calcFuel();
        populateDropdowns();
        updateUserUI();
        renderAll();
    }

    function updateUserUI() {
        const t = translations[currentLang] || translations.pl;
        if (currentUser) {
            document.getElementById('userNameDisplay').innerText = currentUser.name;
            document.getElementById('userAvatar').innerText = currentUser.name.substring(0, 2).toUpperCase();
            document.getElementById('authBtn').innerText = t.logoutBtn;
        } else {
            document.getElementById('userNameDisplay').innerText = t.unlogged;
            document.getElementById('userAvatar').innerText = '?';
            document.getElementById('authBtn').innerText = t.loginBtn;
        }
    }

    function openAuthModal() {
        if (currentUser) {
            currentUser = null;
            localStorage.removeItem('ss_user');
            updateUserUI();
        } else {
            document.getElementById('authModal').style.display = 'flex';
        }
    }
    function closeAuthModal() { document.getElementById('authModal').style.display = 'none'; }
    function login() {
        const nick = document.getElementById('authUser').value;
        const city = document.getElementById('authCity').value || 'Helmond';
        const phone = document.getElementById('authPhone').value || '';
        if (!nick) return alert('Wpisz swój nick!');
        currentUser = { name: nick, city, phone };
        localStorage.setItem('ss_user', JSON.stringify(currentUser));
        updateUserUI();
        closeAuthModal();
    }

    function switchView(viewName, el) {
        document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
        document.querySelectorAll('.view-section').forEach(v => v.classList.remove('active'));
        el.classList.add('active');
        document.getElementById('view-' + viewName).classList.add('active');
    }

    function getAverageRating(username) {
        const userRatings = ratings[username];
        if (!userRatings || userRatings.length === 0) return 'Brak ocen';
        const sum = userRatings.reduce((a, b) => a + b, 0);
        return (sum / userRatings.length).toFixed(1) + ' ★ (' + userRatings.length + ')';
    }

    function renderAll() {
        cleanExpiredOffers();
        const t = translations[currentLang] || translations.pl;
        const search = document.getElementById('searchInput').value.toLowerCase();
        const province = document.getElementById('provinceSelect').value;
        const selectedDate = document.getElementById('filterDate').value;
        const targetShiftTime = document.getElementById('exactFilterTime').value;

        let pendingOrdersCount = 0;
        let unreadMsgsCount = 0;

        if (currentUser) {
            pendingOrdersCount = bookings.filter(b => b.provider === currentUser.name && b.status === 'pending').length;
            chats.forEach(c => {
                if (c.partner !== activeChatPartner && c.messages.length > 0) {
                    unreadMsgsCount += c.messages.filter(m => m.sender !== currentUser.name).length > 0 ? 1 : 0;
                }
            });
        }

        const oBadge = document.getElementById('ordersBadge');
        if (pendingOrdersCount > 0) {
            oBadge.innerText = pendingOrdersCount;
            oBadge.style.display = 'inline-block';
        } else {
            oBadge.style.display = 'none';
        }

        const mBadge = document.getElementById('msgsBadge');
        if (unreadMsgsCount > 0) {
            mBadge.innerText = unreadMsgsCount;
            mBadge.style.display = 'inline-block';
        } else {
            mBadge.style.display = 'none';
        }

        // Render Carpool
        const carpoolList = document.getElementById('carpoolList');
        carpoolList.innerHTML = '';
        const filteredCarpool = carpoolOffers.filter(item => {
            const matchesSearch = item.from.toLowerCase().includes(search) || item.to.toLowerCase().includes(search) || item.name.toLowerCase().includes(search);
            const matchesProvince = province === 'ALL' || item.province === province;
            const matchesDate = !selectedDate || item.shiftDate === selectedDate;
            const matchesShift = !filterExactShift || item.shift === targetShiftTime;
            return matchesSearch && matchesProvince && matchesDate && matchesShift;
        });

        if (filteredCarpool.length === 0) {
            carpoolList.innerHTML = `<div class="empty-state">${t.emptyCarpool}</div>`;
        } else {
            filteredCarpool.forEach(item => {
                const roleTxt = item.role === 'driver' ? `${t.optDriver} (${item.seats} ${t.seatsLabel})` : t.optPassenger;
                const waBtn = item.phone ? `<a href="https://wa.me/${item.phone.replace(/[^0-9]/g, '')}" target="_blank" class="btn-wa">📱 WhatsApp</a>` : '';
                const priceFormatted = isNaN(item.price) ? item.price : `€${item.price}`;
                const userRatingStr = getAverageRating(item.name);
                
                let carDetailsStr = item.carModel || '';
                if (item.carColor) carDetailsStr += `, ${item.carColor}`;
                if (item.carPlate) carDetailsStr += ` [${item.carPlate}]`;
                const carInfoHtml = item.role === 'driver' && carDetailsStr ? `<div class="meta-info">🚘 ${t.carInfoLabel}: <strong>${carDetailsStr}</strong></div>` : '';
                const smokingText = item.smoking === 'yes' ? t.smokeYesLabel : t.smokeNoLabel;

                const destEncoded = encodeURIComponent(item.to + ", " + item.city + ", Netherlands");
                const navLinksHtml = `
                    <a href="https://www.google.com/maps/search/?api=1&query=${destEncoded}" target="_blank" class="nav-link-btn btn-gmaps">📍 Maps</a>
                    <a href="https://waze.com/ul?q=${destEncoded}" target="_blank" class="nav-link-btn btn-waze">🚗 Waze</a>
                `;

                carpoolList.innerHTML += `
                    <div class="card">
                        <div class="avatar">${item.name.substring(0, 2).toUpperCase()}</div>
                        <div class="info">
                            <div>
                                <span class="new-badge">${t.newTag}</span>
                                <span class="date-badge">📅 ${item.shiftDate || ''}</span>
                            </div>
                            <h4>${item.name} <span class="rating-stars">⭐ ${userRatingStr}</span></h4>
                            <p>${t.routeLabel}: ${item.from} ➔ ${item.to} ${navLinksHtml}</p>
                            ${carInfoHtml}
                            <div class="meta-info">📍 ${item.province} (${item.city}) | ⏰ ${t.shiftLabel}: ${item.shift}</div>
                            <div class="meta-info">🚗 ${t.departLabel}: ${item.departTime} | 📍 ${t.meetingLabel}: ${item.meetingPoint || 'Do ustalenia'}</div>
                            <div class="meta-info">🚬 ${smokingText}</div>
                            ${item.role === 'driver' ? `<div class="fuel-box">💳 ${t.priceLabel}: <strong>${priceFormatted}</strong> | 🛣️ ${item.distance || 0} km (~${item.fuelLiters || 0} L)</div>` : ''}
                            <span class="lang-badge">${item.country} | 🗣 ${item.lang}</span><br>
                            <span class="tag tag-car">${roleTxt}</span>
                        </div>
                        <div class="btn-group">
                            <button class="btn-action" onclick="bookItem('Carpool', '${item.name}', '${item.from} ➔ ${item.to}')">${t.joinBtn}</button>
                            ${waBtn}
                        </div>
                    </div>`;
            });
        }

        // Render Food
        const foodList = document.getElementById('foodList');
        foodList.innerHTML = '';
        const filteredFood = foodOffers.filter(item => {
            const matchesSearch = item.title.toLowerCase().includes(search) || item.loc.toLowerCase().includes(search);
            const matchesProvince = province === 'ALL' || item.province === province;
            const matchesDate = !selectedDate || item.shiftDate === selectedDate;
            const matchesShift = !filterExactShift || item.shift === targetShiftTime;
            return matchesSearch && matchesProvince && matchesDate && matchesShift;
        });

        if (filteredFood.length === 0) {
            foodList.innerHTML = `<div class="empty-state">${t.emptyFood}</div>`;
        } else {
            filteredFood.forEach(item => {
                const waBtn = item.phone ? `<a href="https://wa.me/${item.phone.replace(/[^0-9]/g, '')}" target="_blank" class="btn-wa">📱 WhatsApp</a>` : '';
                const userRatingStr = getAverageRating(item.name);
                const locEncoded = encodeURIComponent(item.loc + ", " + item.city + ", Netherlands");
                const foodNavHtml = `<a href="https://www.google.com/maps/search/?api=1&query=${locEncoded}" target="_blank" class="nav-link-btn btn-gmaps">📍 Maps</a>`;

                foodList.innerHTML += `
                    <div class="card">
                        <div class="avatar" style="color:var(--secondary); background:var(--secondary-light);">${item.name.substring(0, 2).toUpperCase()}</div>
                        <div class="info">
                            <div>
                                <span class="new-badge">${t.newTag}</span>
                                <span class="date-badge">📅 ${item.shiftDate || ''}</span>
                            </div>
                            <h4>${item.name} <span class="rating-stars">⭐ ${userRatingStr}</span></h4>
                            <p>${item.title}</p>
                            <div class="meta-info">📍 ${item.province} (${item.city}) | ⏰ ${t.shiftLabel}: ${item.shift}</div>
                            <div class="meta-info">🥡 ${t.pickupLabel}: ${item.loc} ${foodNavHtml}</div>
                            <span class="lang-badge">${item.country} | 🗣 ${item.lang}</span><br>
                            <span class="tag tag-food">${t.priceLabel}: €${item.price}</span>
                        </div>
                        <div class="btn-group">
                            <button class="btn-action secondary" onclick="bookItem('Posiłek', '${item.name}', '${item.title}')">${t.orderBtn}</button>
                            ${waBtn}
                        </div>
                    </div>`;
            });
        }

        // Render Orders
        const ordersList = document.getElementById('ordersList');
        ordersList.innerHTML = bookings.length ? '' : `<div class="empty-state">${t.emptyOrders}</div>`;
        bookings.forEach(b => {
            const isMyOffer = currentUser && b.provider === currentUser.name;
            const isMyBooking = currentUser && b.requester === currentUser.name;

            let statusClass = 'status-pending';
            let statusText = t.statusPending;
            if (b.status === 'accepted') { statusClass = 'status-accepted'; statusText = t.statusAccepted; }
            if (b.status === 'rejected') { statusClass = 'status-rejected'; statusText = t.statusRejected; }
            if (b.status === 'completed') { statusClass = 'status-completed'; statusText = t.statusCompleted; }

            let actionsHtml = '';
            if (isMyOffer && b.status === 'pending') {
                actionsHtml = `
                    <button class="btn-action" style="background:var(--secondary); padding:4px 8px;" onclick="updateBookingStatus(${b.id}, 'accepted')">${t.btnAccept}</button>
                    <button class="btn-action danger" style="padding:4px 8px;" onclick="updateBookingStatus(${b.id}, 'rejected')">${t.btnReject}</button>
                `;
            } else if (b.status === 'accepted' && (isMyOffer || isMyBooking)) {
                actionsHtml = `<button class="btn-action secondary" style="padding:4px 8px;" onclick="completeBooking(${b.id})">${t.btnComplete}</button>`;
            }

            ordersList.innerHTML += `
                <div class="card">
                    <div class="info">
                        <h4>[${b.type}] ${b.title}</h4>
                        <p>${isMyOffer ? `Prośba od: <strong>${b.requester}</strong>` : `Kierowca / Dostawca: <strong>${b.provider}</strong>`}</p>
                        <span class="status-badge ${statusClass}">${statusText}</span>
                    </div>
                    <div class="btn-group">
                        ${actionsHtml}
                        <button class="btn-action" onclick="openChat('${isMyOffer ? b.requester : b.provider}')">${t.writeBtn}</button>
                    </div>
                </div>`;
        });

        renderChats();
    }

    function addCarpoolOffer() {
        if (!currentUser) return openAuthModal();
        const from = document.getElementById('carName').value;
        const to = document.getElementById('carDest').value;
        const role = document.getElementById('carRole').value;
        const carModel = document.getElementById('carModel').value || '';
        const carColor = document.getElementById('carColor').value || '';
        const carPlate = document.getElementById('carPlate').value || '';
        const smoking = document.getElementById('carSmoking').value || 'no';
        const seats = document.getElementById('carSeats').value || 3;
        const price = document.getElementById('carPrice').value || 'Do ustalenia';
        const distance = parseFloat(document.getElementById('carDistance').value) || 0;
        const consumption = parseFloat(document.getElementById('carConsumption').value) || 0;
        const fuelLiters = (distance * consumption / 100).toFixed(2);

        const shiftDate = document.getElementById('carShiftDate').value || todayStr;
        const shift = document.getElementById('carShiftTime').value;
        const departTime = document.getElementById('carDepartTime').value;
        const meetingPoint = document.getElementById('carMeetingPoint').value || 'Do ustalenia';
        const phone = document.getElementById('carPhone').value || currentUser.phone || '';
        const country = document.getElementById('carCountry').value;
        const lang = document.getElementById('carLang').value;

        if (!from || !to) return alert('Wypełnij trasę!');

        carpoolOffers.unshift({
            id: Date.now(), name: currentUser.name, from, to, role, carModel, carColor, carPlate, smoking, seats, price, distance, fuelLiters, shiftDate, shift, departTime, meetingPoint, phone, country, lang,
            province: document.getElementById('provinceSelect').value === 'ALL' ? 'Noord-Brabant' : document.getElementById('provinceSelect').value,
            city: currentUser.city
        });
        localStorage.setItem('ss_carpool', JSON.stringify(carpoolOffers));
        renderAll();
    }

    function addFoodOffer() {
        if (!currentUser) return openAuthModal();
        const title = document.getElementById('foodTitle').value;
        const loc = document.getElementById('foodLoc').value;
        const shiftDate = document.getElementById('foodShiftDate').value || todayStr;
        const shift = document.getElementById('foodShiftTime').value;
        const price = document.getElementById('foodPrice').value;
        const phone = document.getElementById('foodPhone').value || currentUser.phone || '';
        const country = document.getElementById('foodCountry').value;
        const lang = document.getElementById('foodLang').value;

        if (!title || !price) return alert('Wypełnij dane posiłku!');

        foodOffers.unshift({
            id: Date.now(), name: currentUser.name, title, loc, shiftDate, shift, price, phone, country, lang,
            province: document.getElementById('provinceSelect').value === 'ALL' ? 'Noord-Brabant' : document.getElementById('provinceSelect').value,
            city: currentUser.city
        });
        localStorage.setItem('ss_food', JSON.stringify(foodOffers));
        renderAll();
    }

    function bookItem(type, provider, title) {
        if (!currentUser) return openAuthModal();
        if (currentUser.name === provider) return alert('To Twoje własne ogłoszenie!');

        bookings.unshift({ id: Date.now(), type, provider, requester: currentUser.name, title, status: 'pending' });
        localStorage.setItem('ss_bookings', JSON.stringify(bookings));
        
        let chat = chats.find(c => c.partner === provider);
        if (!chat) { chat = { partner: provider, messages: [] }; chats.push(chat); }
        chat.messages.push({ sender: currentUser.name, text: `Cześć! Zgłaszam prośbę o rezerwację: ${title}.` });
        localStorage.setItem('ss_chats', JSON.stringify(chats));

        alert(`Wysłano prośbę do: ${provider}`);
        renderAll();
    }

    function updateBookingStatus(bookingId, status) {
        const b = bookings.find(item => item.id === bookingId);
        if (b) {
            b.status = status;
            localStorage.setItem('ss_bookings', JSON.stringify(bookings));
            renderAll();
        }
    }

    function completeBooking(bookingId) {
        const b = bookings.find(item => item.id === bookingId);
        if (b) {
            b.status = 'completed';
            localStorage.setItem('ss_bookings', JSON.stringify(bookings));
            openRateModal(currentUser.name === b.provider ? b.requester : b.provider);
            renderAll();
        }
    }

    function openRateModal(targetUser) {
        ratingTargetUser = targetUser;
        selectedStars = 5;
        document.getElementById('rateModal').style.display = 'flex';
    }
    function closeRateModal() { document.getElementById('rateModal').style.display = 'none'; ratingTargetUser = null; }
    function setRating(stars) {
        selectedStars = stars;
        document.querySelectorAll('#starContainer span').forEach((s, i) => s.classList.toggle('active', i < stars));
    }
    function submitRating() {
        if (!ratingTargetUser) return closeRateModal();
        if (!ratings[ratingTargetUser]) ratings[ratingTargetUser] = [];
        ratings[ratingTargetUser].push(selectedStars);
        localStorage.setItem('ss_ratings', JSON.stringify(ratings));
        closeRateModal();
        renderAll();
    }

    function openChat(partner) {
        activeChatPartner = partner;
        document.getElementById('chatList').style.display = 'none';
        document.getElementById('activeChat').style.display = 'flex';
        document.getElementById('chatPartnerName').innerText = partner;
        renderChatMessages();
    }
    function closeChat() {
        activeChatPartner = null;
        document.getElementById('chatList').style.display = 'block';
        document.getElementById('activeChat').style.display = 'none';
        renderAll();
    }
    function renderChats() {
        const t = translations[currentLang] || translations.pl;
        const chatList = document.getElementById('chatList');
        chatList.innerHTML = chats.length ? '' : `<div class="empty-state">${t.emptyMessages}</div>`;
        chats.forEach(c => {
            const lastMsg = c.messages[c.messages.length - 1]?.text || '';
            chatList.innerHTML += `
                <div class="chat-list-item" onclick="openChat('${c.partner}')">
                    <div class="avatar">${c.partner.substring(0,2).toUpperCase()}</div>
                    <div class="info"><h4>${c.partner}</h4><p>${lastMsg}</p></div>
                </div>`;
        });
    }
    function renderChatMessages() {
        if (!activeChatPartner) return;
        const chat = chats.find(c => c.partner === activeChatPartner);
        const box = document.getElementById('chatMessages');
        box.innerHTML = '';
        if (chat) {
            chat.messages.forEach(m => {
                const isMe = currentUser && m.sender === currentUser.name;
                box.innerHTML += `<div class="msg-bubble ${isMe ? 'msg-sent' : 'msg-received'}">${m.text}</div>`;
            });
        }
        box.scrollTop = box.scrollHeight;
    }
    function sendMessage() {
        const input = document.getElementById('chatMsgInput');
        const text = input.value;
        if (!text || !activeChatPartner) return;
        let chat = chats.find(c => c.partner === activeChatPartner);
        if (!chat) { chat = { partner: activeChatPartner, messages: [] }; chats.push(chat); }
        chat.messages.push({ sender: currentUser ? currentUser.name : 'Ty', text });
        localStorage.setItem('ss_chats', JSON.stringify(chats));
        input.value = '';
        renderChatMessages();
        renderChats();
    }

    const userLang = navigator.language.slice(0, 2);
    if (translations[userLang]) currentLang = userLang;
    document.getElementById('langSelect').value = currentLang;
    changeLanguage(currentLang);
</script>
</body>
</html>
