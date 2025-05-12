<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Planner Hebdomadaire Optimisé</title>
  <style>
    :root {
      --bg-color: #f5f5f5;
      --text-color: #333;
      --table-bg: white;
      --table-border: #ddd;
      --th-bg: #007BFF;
      --th-color: white;
      --editable-bg: #fdfdfd;
      --editable-hover: #f0f0f0;
      --btn-primary: #28a745;
      --btn-primary-hover: #218838;
      --btn-danger: #dc3545;
      --btn-danger-hover: #c82333;
      --btn-info: #17a2b8;
      --btn-info-hover: #138496;
      --btn-warning: #ff851b;
      --btn-warning-hover: #e57300;
      --shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    .dark-mode {
      --bg-color: #1a1a1a;
      --text-color: #f0f0f0;
      --table-bg: #2d2d2d;
      --table-border: #444;
      --th-bg: #0056b3;
      --editable-bg: #3a3a3a;
      --editable-hover: #4a4a4a;
      --shadow: 0 2px 5px rgba(0,0,0,0.3);
    }

    body {
      font-family: 'Arial', sans-serif;
      margin: 0;
      padding: 20px;
      background: var(--bg-color);
      color: var(--text-color);
      transition: background 0.3s, color 0.3s;
    }

    h2 {
      text-align: center;
      margin-bottom: 20px;
    }

    .header-controls {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      flex-wrap: wrap;
      gap: 10px;
    }

    .controls {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }

    select, button {
      padding: 10px;
      font-size: 16px;
      border: 1px solid var(--table-border);
      border-radius: 5px;
      cursor: pointer;
      background: var(--table-bg);
      color: var(--text-color);
    }

    button {
      background: var(--btn-primary);
      color: white;
      border: none;
      transition: background 0.2s;
    }

    button:hover {
      opacity: 0.9;
    }

    #dark-mode-btn {
      background: #6c757d;
    }

    #save-btn {
      background: var(--btn-primary);
    }

    #save-btn:hover {
      background: var(--btn-primary-hover);
    }

    #reset-btn {
      background: var(--btn-danger);
    }

    #reset-btn:hover {
      background: var(--btn-danger-hover);
    }

    #print-btn {
      background: var(--btn-info);
    }

    #print-btn:hover {
      background: var(--btn-info-hover);
    }

    #shopping-list-btn {
      background: var(--btn-warning);
    }

    #shopping-list-btn:hover {
      background: var(--btn-warning-hover);
    }

    table {
      width: 100%;
      border-collapse: collapse;
      background: var(--table-bg);
      box-shadow: var(--shadow);
      margin-bottom: 20px;
    }

    th, td {
      border: 1px solid var(--table-border);
      padding: 12px;
      text-align: left;
      min-width: 100px;
    }

    th {
      background: var(--th-bg);
      color: var(--th-color);
      font-weight: bold;
    }

    td[contenteditable] {
      background: var(--editable-bg);
      cursor: pointer;
      transition: background 0.2s;
    }

    td[contenteditable]:hover {
      background: var(--editable-hover);
    }

    .extra-hours {
      background: #e9ecef;
      font-style: italic;
    }

    .dark-mode .extra-hours {
      background: #3a3a3a;
    }

    /* Catégories de couleur */
    .urgent-red {
      background: #cc0000 !important;
      color: white;
    }
    .medium-yellow {
      background: #fff4cc !important;
    }
    .dark-mode .medium-yellow {
      color: #333;
    }
    .free-green {
      background: #ccffcc !important;
    }
    .school-appointment-purple {
      background: #d9b3ff !important;
    }
    .vacation-lightgreen {
      background: #b3ffb3 !important;
    }
    .kids-show-orange {
      background: #ffdab3 !important;
    }
    .doctor-red {
      background: #ff3333 !important;
      color: white;
    }
    .birthday-rainbow {
      background: linear-gradient(to right, red, orange, yellow, green, blue, indigo, violet) !important;
      color: white;
    }

    #shopping-list-container {
      display: none;
      background: var(--table-bg);
      padding: 20px;
      border-radius: 5px;
      box-shadow: var(--shadow);
      margin-bottom: 20px;
    }

    .shopping-category {
      margin-bottom: 20px;
    }

    .shopping-category h3 {
      margin: 0 0 10px;
      color: var(--th-bg);
    }

    .shopping-item {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 5px;
    }

    .shopping-item input[type="checkbox"] {
      width: 20px;
      height: 20px;
    }

    .shopping-item input[type="text"] {
      flex: 1;
      padding: 5px;
      border: 1px solid var(--table-border);
      border-radius: 3px;
      background: var(--editable-bg);
      color: var(--text-color);
    }

    .shopping-item button {
      background: var(--btn-danger);
      padding: 5px 10px;
    }

    .add-item-btn {
      background: var(--btn-primary);
      margin-top: 10px;
    }

    .category-reset {
      font-size: 12px;
      margin-left: 5px;
      color: var(--btn-danger);
      cursor: pointer;
      text-decoration: underline;
    }

    @media (max-width: 768px) {
      .header-controls {
        flex-direction: column;
        align-items: stretch;
      }
      
      .controls {
        flex-direction: column;
      }
      
      th, td {
        padding: 8px;
        min-width: 80px;
        font-size: 14px;
      }
      
      select, button {
        width: 100%;
      }
    }

    @media print {
      .header-controls, #shopping-list-container {
        display: none;
      }
      
      h2 {
        font-size: 18px;
        margin: 10px 0;
      }
      
      table {
        box-shadow: none;
        margin: 0;
      }
      
      th, td {
        font-size: 12px;
        padding: 8px;
      }
    }
  </style>
</head>
<body>
  <div class="header-controls">
    <h2>🗓️ Mon Planner Hebdomadaire</h2>
    <div>
      <button id="dark-mode-btn">🌙 Mode Sombre</button>
    </div>
  </div>
  
  <div class="controls">
    <select id="week-select"></select>
    <select id="month-select">
      <option value="0">Janvier</option>
      <option value="1">Février</option>
      <option value="2">Mars</option>
      <option value="3">Avril</option>
      <option value="4">Mai</option>
      <option value="5">Juin</option>
      <option value="6">Juillet</option>
      <option value="7">Août</option>
      <option value="8">Septembre</option>
      <option value="9">Octobre</option>
      <option value="10">Novembre</option>
      <option value="11">Décembre</option>
    </select>
    <select id="year-select">
      <option value="2025">2025</option>
      <option value="2026">2026</option>
      <option value="2027">2027</option>
    </select>
    <select id="category-select">
      <option value="">Aucune catégorie</option>
      <option value="work-dad">Travail Papa (TP)</option>
      <option value="work-mom">Travail Maman (TM)</option>
      <option value="urgent-red">Urgent (Rouge)</option>
      <option value="medium-yellow">Moyen (Jaune)</option>
      <option value="free-green">Libre (Vert)</option>
      <option value="school-appointment-purple">Rendez-vous École (Violet)</option>
      <option value="vacation-lightgreen">Vacances (Vert Clair)</option>
      <option value="kids-show-orange">Spectacle Enfant (Orange)</option>
      <option value="doctor-red">Médecin (Rouge)</option>
      <option value="birthday-rainbow">Anniversaires (Arc-en-ciel)</option>
    </select>
    <button id="save-btn">💾 Sauvegarder</button>
    <button id="reset-btn">🔄 Reset</button>
    <button id="print-btn">🖨️ Imprimer</button>
    <button id="shopping-list-btn">🛒 Liste de Courses</button>
  </div>
  
  <div id="shopping-list-container">
    <h3>Liste de Courses</h3>
    <div id="shopping-list"></div>
    <button class="add-item-btn" onclick="addShoppingItem('Autres')">+ Ajouter un article</button>
  </div>
  
  <table id="planner">
    <thead>
      <tr>
        <th>Heure</th>
        <th>Lundi</th>
        <th>Mardi</th>
        <th>Mercredi</th>
        <th>Jeudi</th>
        <th>Vendredi</th>
        <th>Samedi</th>
        <th>Dimanche</th>
      </tr>
    </thead>
    <tbody id="planner-body"></tbody>
  </table>

  <script>
    // Éléments DOM
    const plannerBody = document.getElementById("planner-body");
    const weekSelect = document.getElementById("week-select");
    const monthSelect = document.getElementById("month-select");
    const yearSelect = document.getElementById("year-select");
    const categorySelect = document.getElementById("category-select");
    const saveBtn = document.getElementById("save-btn");
    const resetBtn = document.getElementById("reset-btn");
    const printBtn = document.getElementById("print-btn");
    const shoppingListBtn = document.getElementById("shopping-list-btn");
    const shoppingListContainer = document.getElementById("shopping-list-container");
    const shoppingList = document.getElementById("shopping-list");
    const darkModeBtn = document.getElementById("dark-mode-btn");

    // Constantes
    const startHour = 8;
    const endHour = 18;
    const extraHours = ["Matin (00h-07h)", "Soir (19h-00h)"];
    const shoppingCategories = ["Fruits", "Légumes", "Viandes", "Produits Laitiers", "Autres"];
    const categoryClasses = [
      'urgent-red', 'medium-yellow', 'free-green', 
      'school-appointment-purple', 'vacation-lightgreen', 
      'kids-show-orange', 'doctor-red', 'birthday-rainbow'
    ];

    // Initialisation
    function init() {
      generateWeekOptions();
      generatePlanner();
      loadPlanner();
      generateShoppingList();
      setupEventListeners();
    }

    // Générer les options de semaine
    function generateWeekOptions() {
      for (let i = 1; i <= 52; i++) {
        const option = document.createElement("option");
        option.value = i;
        option.text = `Semaine ${i}`;
        weekSelect.appendChild(option);
      }
    }

    // Générer le planner
    function generatePlanner() {
      plannerBody.innerHTML = "";
      
      // Heures normales
      for (let h = startHour; h <= endHour; h++) {
        const row = document.createElement("tr");
        row.innerHTML = `<td>${h}h</td>`;
        for (let d = 1; d <= 7; d++) {
          row.innerHTML += '<td contenteditable="true"></td>';
        }
        plannerBody.appendChild(row);
      }
      
      // Heures supplémentaires
      extraHours.forEach(hour => {
        const row = document.createElement("tr");
        row.innerHTML = `<td class="extra-hours">${hour}</td>`;
        for (let d = 1; d <= 7; d++) {
          row.innerHTML += '<td contenteditable="true" class="extra-hours"></td>';
        }
        plannerBody.appendChild(row);
      });
      
      setupCellEventListeners();
    }

    // Configurer les événements sur les cellules
    function setupCellEventListeners() {
      const cells = plannerBody.querySelectorAll('td[contenteditable]');
      cells.forEach(cell => {
        cell.addEventListener('click', handleCellClick);
        cell.addEventListener('blur', savePlanner);
      });
    }

    // Gérer le clic sur une cellule
    function handleCellClick() {
      const selectedCategory = categorySelect.value;
      if (!selectedCategory) {
        alert("Veuillez sélectionner une catégorie avant de cliquer sur une case.");
        return;
      }
      applyCategoryToCell(this);
    }

    // Appliquer la catégorie à une cellule
    function applyCategoryToCell(cell) {
      const selectedCategory = categorySelect.value;
      
      // Reset des classes de catégorie
      cell.classList.remove(...categoryClasses);
      
      // Appliquer la nouvelle catégorie si nécessaire
      if (selectedCategory && !['work-dad', 'work-mom'].includes(selectedCategory)) {
        cell.classList.add(selectedCategory);
      }
      
      // Gérer les préfixes de texte
      let prefix = '';
      if (selectedCategory === 'work-dad') prefix = 'TP: ';
      if (selectedCategory === 'work-mom') prefix = 'TM: ';
      if (selectedCategory === 'doctor-red') prefix = 'M: ';
      
      const currentText = cell.innerText.trim();
      if (prefix) {
        if (!currentText.startsWith(prefix) && currentText !== '') {
          cell.innerText = prefix + currentText;
        } else if (currentText === '') {
          cell.innerText = prefix;
        }
      }
      
      // Ajouter le bouton de reset si une catégorie est appliquée
      addResetButtonToCell(cell);
    }

    // Ajouter un bouton de reset à une cellule
    function addResetButtonToCell(cell) {
      // Supprimer d'abord tout bouton existant
      const existingReset = cell.querySelector('.category-reset');
      if (existingReset) existingReset.remove();
      
      // Vérifier si la cellule a une catégorie
      const hasCategory = categoryClasses.some(cls => cell.classList.contains(cls));
      
      if (hasCategory) {
        const resetBtn = document.createElement('span');
        resetBtn.className = 'category-reset';
        resetBtn.textContent = 'reset';
        resetBtn.title = 'Supprimer la catégorie';
        resetBtn.addEventListener('click', (e) => {
          e.stopPropagation();
          resetCellCategory(cell);
        });
        cell.appendChild(resetBtn);
      }
    }

    // Réinitialiser la catégorie d'une cellule
    function resetCellCategory(cell) {
      cell.classList.remove(...categoryClasses);
      const resetBtn = cell.querySelector('.category-reset');
      if (resetBtn) resetBtn.remove();
      
      // Supprimer les préfixes de texte
      const prefixes = ['TP: ', 'TM: ', 'M: '];
      prefixes.forEach(prefix => {
        if (cell.innerText.startsWith(prefix)) {
          cell.innerText = cell.innerText.replace(prefix, '');
        }
      });
      
      savePlanner();
    }

    // Sauvegarder le planner
    function savePlanner() {
      const key = getStorageKey();
      localStorage.setItem(key, plannerBody.innerHTML);
    }

    // Charger le planner
    function loadPlanner() {
      const key = getStorageKey();
      const saved = localStorage.getItem(key);
      
      if (saved) {
        plannerBody.innerHTML = saved;
        setupCellEventListeners();
        
        // Re-attacher les boutons de reset
        const cells = plannerBody.querySelectorAll('td[contenteditable]');
        cells.forEach(cell => {
          if (categoryClasses.some(cls => cell.classList.contains(cls))) {
            addResetButtonToCell(cell);
          }
        });
      } else {
        generatePlanner();
      }
    }

    // Générer la liste de courses
    function generateShoppingList() {
      shoppingList.innerHTML = "";
      shoppingCategories.forEach(category => {
        const categoryDiv = document.createElement("div");
        categoryDiv.className = "shopping-category";
        categoryDiv.innerHTML = `<h3>${category}</h3>`;
        categoryDiv.innerHTML += `<div id="items-${category.replace(/\s+/g, '-')}" class="items"></div>`;
        categoryDiv.innerHTML += `<button class="add-item-btn" onclick="addShoppingItem('${category}')">+ Ajouter un article</button>`;
        shoppingList.appendChild(categoryDiv);
      });
    }

    // Ajouter un article à la liste de courses
    function addShoppingItem(category) {
      const itemsContainer = document.getElementById(`items-${category.replace(/\s+/g, '-')}`);
      const itemDiv = document.createElement("div");
      itemDiv.className = "shopping-item";
      itemDiv.innerHTML = `
        <input type="checkbox" onchange="saveShoppingList()">
        <span>- </span>
        <input type="text" placeholder="Quantité et ingrédient (ex: 2 Pommes)" oninput="saveShoppingList()">
        <button onclick="this.parentElement.remove(); saveShoppingList()">Supprimer</button>
      `;
      itemsContainer.appendChild(itemDiv);
      saveShoppingList();
    }

    // Sauvegarder la liste de courses
    function saveShoppingList() {
      const key = `shopping_${getStorageKey()}`;
      const items = {};
      
      shoppingCategories.forEach(category => {
        const itemsContainer = document.getElementById(`items-${category.replace(/\s+/g, '-')}`);
        const itemElements = itemsContainer.querySelectorAll(".shopping-item");
        items[category] = Array.from(itemElements).map(item => ({
          checked: item.querySelector("input[type='checkbox']").checked,
          text: item.querySelector("input[type='text']").value
        }));
      });
      
      localStorage.setItem(key, JSON.stringify(items));
    }

    // Charger la liste de courses
    function loadShoppingList() {
      const key = `shopping_${getStorageKey()}`;
      const saved = localStorage.getItem(key);
      
      generateShoppingList();
      
      if (saved) {
        const items = JSON.parse(saved);
        shoppingCategories.forEach(category => {
          const itemsContainer = document.getElementById(`items-${category.replace(/\s+/g, '-')}`);
          if (items[category]) {
            items[category].forEach(item => {
              const itemDiv = document.createElement("div");
              itemDiv.className = "shopping-item";
              itemDiv.innerHTML = `
                <input type="checkbox" ${item.checked ? 'checked' : ''} onchange="saveShoppingList()">
                <span>- </span>
                <input type="text" value="${item.text}" placeholder="Quantité et ingrédient (ex: 2 Pommes)" oninput="saveShoppingList()">
                <button onclick="this.parentElement.remove(); saveShoppingList()">Supprimer</button>
              `;
              itemsContainer.appendChild(itemDiv);
            });
          }
        });
      }
    }

    // Obtenir la clé de stockage
    function getStorageKey() {
      return `planner_${yearSelect.value}_${monthSelect.value}_${weekSelect.value}`;
    }

    // Configurer les événements
    function setupEventListeners() {
      // Boutons principaux
      saveBtn.addEventListener('click', () => {
        savePlanner();
        alert("Planner sauvegardé !");
      });
      
      resetBtn.addEventListener('click', () => {
        if (confirm("Voulez-vous vraiment réinitialiser le planner ? Tout le contenu sera supprimé.")) {
          generatePlanner();
          categorySelect.value = "";
          alert("Planner réinitialisé !");
        }
      });
      
      printBtn.addEventListener('click', () => window.print());
      
      shoppingListBtn.addEventListener('click', () => {
        shoppingListContainer.style.display = shoppingListContainer.style.display === 'block' ? 'none' : 'block';
        if (shoppingListContainer.style.display === 'block') {
          loadShoppingList();
        }
      });
      
      // Mode sombre
      darkModeBtn.addEventListener('click', toggleDarkMode);
      
      // Changements de sélection
      weekSelect.addEventListener('change', () => {
        loadPlanner();
        loadShoppingList();
      });
      
      monthSelect.addEventListener('change', () => {
        loadPlanner();
        loadShoppingList();
      });
      
      yearSelect.addEventListener('change', () => {
        loadPlanner();
        loadShoppingList();
      });
    }

    // Basculer le mode sombre
    function toggleDarkMode() {
      document.body.classList.toggle('dark-mode');
      const isDark = document.body.classList.contains('dark-mode');
      darkModeBtn.textContent = isDark ? '☀️ Mode Clair' : '🌙 Mode Sombre';
      localStorage.setItem('darkMode', isDark ? 'enabled' : 'disabled');
    }

    // Vérifier le mode sombre au chargement
    function checkDarkMode() {
      if (localStorage.getItem('darkMode') === 'enabled') {
        document.body.classList.add('dark-mode');
        darkModeBtn.textContent = '☀️ Mode Clair';
      }
    }

    // Exposer les fonctions globales
    window.addShoppingItem = addShoppingItem;
    window.saveShoppingList = saveShoppingList;

    // Démarrer l'application
    checkDarkMode();
    init();
  </script>
</body>
</html>
