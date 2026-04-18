<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Paris - Hanoi en Train | Mon projet d'aventure</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            padding: 20px;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.1);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(135deg, #2c3e50 0%, #1a252f 100%);
            color: white;
            padding: 40px;
            text-align: center;
        }
        
        .stats {
            background: #34495e;
            color: white;
            padding: 20px;
            display: flex;
            justify-content: space-around;
            flex-wrap: wrap;
            gap: 20px;
        }
        
        .stat-card { text-align: center; flex: 1; min-width: 150px; }
        .stat-number { font-size: 2em; font-weight: bold; color: #f39c12; }
        
        .content { padding: 30px; }
        .section { margin-bottom: 40px; background: #f8f9fa; padding: 25px; border-radius: 10px; }
        
        .section h2 {
            color: #2c3e50;
            margin-bottom: 20px;
            border-bottom: 3px solid #f39c12;
            padding-bottom: 10px;
        }
        
        /* Styles des Pays */
        .pays-card {
            background: white;
            margin-bottom: 20px;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .pays-header {
            background: #f0f2f5;
            padding: 15px 20px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px solid #f39c12;
        }
        
        .pays-header h3 { color: black; font-size: 1.2em; border: none; outline: none; }
        
        .pays-content { padding: 20px; display: none; }
        .pays-content.active { display: block; }
        
        .info-row {
            margin-bottom: 15px;
            padding: 12px;
            background: #f0f2f5;
            border-radius: 8px;
            border-left: 3px solid #f39c12;
            position: relative;
        }

        .info-header-flex {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }
        
        .info-label { font-weight: bold; color: #e74c3c; font-size: 0.9em; outline: none; }
        
        /* Checklist */
        .checklist-item {
            display: flex;
            align-items: center;
            gap: 10px;
            background: white;
            padding: 10px;
            margin-bottom: 5px;
            border-radius: 5px;
            border: 1px solid #eee;
        }

        .checklist-item input[type="text"] {
            flex: 1;
            border: none;
            outline: none;
            background: transparent;
        }
        
        /* Global UI */
        [contenteditable="true"]:hover { background: rgba(243, 156, 18, 0.05); }
        [contenteditable="true"]:focus { background: white; border: 1px solid #f39c12; border-radius: 4px; }
        
        button {
            background: #f39c12; color: white; border: none;
            padding: 8px 15px; border-radius: 5px; cursor: pointer;
            transition: 0.3s;
        }
        button:hover { background: #e67e22; }
        
        .delete-btn { background: #e74c3c; padding: 4px 8px; font-size: 0.8em; }
        .delete-btn:hover { background: #c0392b; }

        .btn-add-section {
            background: #3498db;
            width: 100%;
            margin-top: 10px;
        }

        textarea { width: 100%; padding: 10px; border-radius: 5px; border: 1px solid #ddd; }
        
        .action-buttons { position: fixed; bottom: 20px; right: 20px; z-index: 1000; display: flex; gap: 10px; }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1 contenteditable="true">🚂 Paris → Hanoi en Train</h1>
            <p contenteditable="true">Mon aventure à travers l'Eurasie | Projet personnel</p>
        </header>
        
        <div class="stats">
            <div class="stat-card">
                <div class="stat-number" contenteditable="true">~15,000 km</div>
                <div contenteditable="true">Kilomètres</div>
            </div>
            <div class="stat-card">
                <div class="stat-number" contenteditable="true">11</div>
                <div contenteditable="true">Pays traversés</div>
            </div>
            <div class="stat-card">
                <div class="stat-number" contenteditable="true">2-3 mois</div>
                <div contenteditable="true">Durée estimée</div>
            </div>
        </div>
        
        <div class="content">
            <div class="section">
                <h2>🗺️ Mon itinéraire détaillé</h2>
                <div id="paysContainer"></div>
                <button class="btn-add-section" style="background: #27ae60; padding: 15px; font-size: 1.1em;" onclick="ajouterPays()">➕ Ajouter un nouveau pays</button>
            </div>
            
            <div class="section">
                <h2>✅ Checklist de préparation</h2>
                <div id="checklistContainer"></div>
                <div style="margin-top: 15px; display: flex; gap: 10px;">
                    <input type="text" id="newChecklistItem" placeholder="Nouvel élément..." style="flex:1; padding: 10px; border-radius: 5px; border: 1px solid #ddd;">
                    <button onclick="addChecklistItem()" style="background: #27ae60;">Ajouter</button>
                </div>
            </div>
            
            <div class="section">
                <h2>📝 Notes générales</h2>
                <textarea id="generalNotes" rows="6" placeholder="Notes globales..."></textarea>
            </div>
        </div>
    </div>

    <div class="action-buttons">
        <button onclick="saveAll()" style="background:#27ae60; font-weight: bold;">💾 TOUT SAUVEGARDER</button>
        <button onclick="location.reload()" style="background:#3498db">🔄 Actualiser</button>
    </div>

    <script>
        // --- DONNÉES INITIALES ---
        let paysData = [
            { id: 1, nom: "🇩🇪 Allemagne - Berlin", details: [
                { label: "📅 Dates", value: "À définir" },
                { label: "💰 Budget", value: "À définir" },
                { label: "📍 À voir", value: "Porte de Brandebourg" }
            ]},
            { id: 2, nom: "🇻🇳 Vietnam - Hanoi", details: [
                { label: "📅 Dates", value: "À définir" },
                { label: "🍜 À manger", value: "Phở, Bún chả" },
                { label: "🎉 Note", value: "Destination finale !" }
            ]}
        ];

        let checklistData = [
            { text: "Passeport (valide + 6 mois)", checked: false },
            { text: "Visa Chine", checked: false },
            { text: "Assurance voyage", checked: false }
        ];

        // --- FONCTIONS PAYS & SECTIONS ---

        function afficherPays() {
            const container = document.getElementById('paysContainer');
            container.innerHTML = '';
            
            paysData.forEach(pays => {
                const card = document.createElement('div');
                card.className = 'pays-card';
                
                let sectionsHtml = pays.details.map((d, index) => `
                    <div class="info-row">
                        <div class="info-header-flex">
                            <div class="info-label" contenteditable="true" onblur="updateSectionLabel(${pays.id}, ${index}, this.innerText)">${d.label}</div>
                            <button class="delete-btn" onclick="supprimerSection(${pays.id}, ${index})">✕</button>
                        </div>
                        <div contenteditable="true" onblur="updateSectionValue(${pays.id}, ${index}, this.innerText)">${d.value}</div>
                    </div>
                `).join('');

                card.innerHTML = `
                    <div class="pays-header" onclick="togglePays(this)">
                        <h3 contenteditable="true" onclick="event.stopPropagation()" onblur="updatePaysNom(${pays.id}, this.innerText)">${pays.nom}</h3>
                        <div>
                            <span class="toggle-icon"> ▶</span>
                        </div>
                    </div>
                    <div class="pays-content">
                        ${sectionsHtml}
                        <button class="btn-add-section" onclick="ajouterSection(${pays.id})">➕ Ajouter une section</button>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function togglePays(header) {
            const content = header.nextElementSibling;
            const icon = header.querySelector('.toggle-icon');
            const isActive = content.style.display === 'block';
            content.style.display = isActive ? 'none' : 'block';
            icon.textContent = isActive ? ' ▶' : ' ▼';
        }

        function ajouterPays() {
            const newId = Date.now();
            paysData.push({ id: newId, nom: "Nouveau Pays", details: [{ label: "📅 Dates", value: "À définir" }] });
            afficherPays();
        }

        function ajouterSection(paysId) {
            const pays = paysData.find(p => p.id === paysId);
            pays.details.push({ label: "Titre section", value: "Contenu..." });
            afficherPays();
        }

        function supprimerSection(paysId, sectionIndex) {
            const pays = paysData.find(p => p.id === paysId);
            pays.details.splice(sectionIndex, 1);
            afficherPays();
        }

        function updatePaysNom(id, val) { paysData.find(p => p.id === id).nom = val; }
        function updateSectionLabel(pid, sidx, val) { paysData.find(p => p.id === pid).details[sidx].label = val; }
        function updateSectionValue(pid, sidx, val) { paysData.find(p => p.id === pid).details[sidx].value = val; }

        // --- FONCTIONS CHECKLIST ---

        function afficherChecklist() {
            const container = document.getElementById('checklistContainer');
            container.innerHTML = '';
            checklistData.forEach((item, index) => {
                const div = document.createElement('div');
                div.className = 'checklist-item';
                div.innerHTML = `
                    <input type="checkbox" ${item.checked ? 'checked' : ''} onchange="checklistData[${index}].checked = this.checked">
                    <input type="text" value="${item.text}" onblur="checklistData[${index}].text = this.value">
                    <button class="delete-btn" onclick="removeChecklistItem(${index})">✕</button>
                `;
                container.appendChild(div);
            });
        }

        function addChecklistItem() {
            const input = document.getElementById('newChecklistItem');
            if(input.value.trim()) {
                checklistData.push({ text: input.value, checked: false });
                input.value = '';
                afficherChecklist();
            }
        }

        function removeChecklistItem(index) {
            checklistData.splice(index, 1);
            afficherChecklist();
        }

        // --- SAUVEGARDE LOCALE ---

        function saveAll() {
            localStorage.setItem('paysData_v2', JSON.stringify(paysData));
            localStorage.setItem('checklistData_v2', JSON.stringify(checklistData));
            localStorage.setItem('generalNotes_v2', document.getElementById('generalNotes').value);
            alert("✅ Tout est sauvegardé localement sur ce navigateur !");
        }

        function loadAll() {
            const savedPays = localStorage.getItem('paysData_v2');
            const savedCheck = localStorage.getItem('checklistData_v2');
            const savedNotes = localStorage.getItem('generalNotes_v2');

            if(savedPays) paysData = JSON.parse(savedPays);
            if(savedCheck) checklistData = JSON.parse(savedCheck);
            if(savedNotes) document.getElementById('generalNotes').value = savedNotes;

            afficherPays();
            afficherChecklist();
        }

        // Init
        window.onload = loadAll;
    </script>
</body>
</html>
