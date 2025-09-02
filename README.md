# Plan_Vial_Suarez_C
GESTION DE PLAN VIAL
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Gestión de Maquinaria Municipal</title>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&family=Montserrat:wght@900&display=swap" rel="stylesheet">
    <!-- FontAwesome -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/js/all.min.js"></script>
    <style>
        :root {
            --main1: #23293a;
            --main2: #2a384d;
            --main3: #5b6b82;
            --main4: #d1d5db;
            --main5: #8999b3;
            --accent: #fff;
            --radius: 18px;
            --header-bg: #f8fafc;
            --shadow: 0 8px 32px rgba(0,0,0,0.08);
            --primary-btn: linear-gradient(135deg, #23293a 60%, #5b6b82 100%);
            --success-btn: linear-gradient(135deg, #2a384d 60%, #5b6b82 100%);
            --danger-btn: linear-gradient(135deg, #723232 60%, #b35454 100%);
            --warning-btn: linear-gradient(135deg, #dbb600 60%, #ffe066 100%);
        }
        html, body {
            font-family: 'Roboto', Arial, sans-serif;
            background: linear-gradient(135deg, #23293a 0%, #5b6b82 100%);
            margin: 0; padding: 0; min-height: 100vh; color: var(--main2);
        }
        .header {
            background: var(--header-bg);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            padding: 2rem 1rem 1.5rem 1rem;
            margin: 2rem auto 1rem auto;
            max-width: 900px;
            display: flex;
            align-items: center;
            gap: 2rem;
        }
        .logo-inst {
            width: 95px;
            height: 95px;
            border-radius: 16px;
            object-fit: cover;
            box-shadow: 0 2px 12px #0002;
            border: 4px solid var(--main3);
            background: #fff;
        }
        .title-group {
            flex: 1;
        }
        .main-title {
            font-family: 'Montserrat', Arial, sans-serif;
            font-weight: 900;
            font-size: 2.1rem;
            color: var(--main1);
            letter-spacing: 1px;
            margin-bottom: 0.35rem;
        }
        .subtitle {
            font-size: 1.1rem;
            color: var(--main3);
            margin-bottom: 0.5rem;
            font-weight: 600;
        }
        .inst-name {
            color: var(--main5);
            font-weight: bold;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            font-size: 1rem;
        }
        nav {
            margin: 0 auto 2rem auto;
            max-width: 900px;
            background: var(--header-bg);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            padding: 0.7rem 0.7rem 0.1rem 0.7rem;
            display: flex;
            gap: 0.5rem;
        }
        nav a {
            text-decoration: none;
            color: var(--main3);
            font-weight: 700;
            font-size: 1.05rem;
            padding: 0.7rem 1.4rem;
            border-radius: 10px 10px 0 0;
            background: transparent;
            transition: background 0.18s, color 0.18s;
            display: flex; align-items: center; gap: 8px;
        }
        nav a.active, nav a:hover {
            background: var(--main1);
            color: #fff;
        }
        .page {
            display: none;
            max-width: 900px;
            margin: 0 auto 2rem auto;
            background: var(--header-bg);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            padding: 2rem 1.5rem 2rem 1.5rem;
        }
        .page.active { display: block; }
        /* Adaptar la tabla y estilos de controles */
        .controls, .stats-bar, .table-container {
            margin-bottom: 2rem;
        }
        .controls, .stats-bar {
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
            align-items: center;
        }
        .controls input, .controls select {
            min-width: 140px;
        }
        .btn {
            padding: 12px 20px;
            border: none;
            border-radius: 12px;
            font-size: 1rem;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s;
            letter-spacing: 0.5px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .btn-primary { background: var(--primary-btn); color: #fff; }
        .btn-success { background: var(--success-btn); color: #fff; }
        .btn-danger { background: var(--danger-btn); color: #fff; }
        .btn-warning { background: var(--warning-btn); color: #23293a; }
        .btn-small { padding: 6px 12px; font-size: 0.82rem; border-radius: 5px;}
        table {
            width: 100%;
            border-collapse: collapse;
            background: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 16px rgba(0,0,0,0.06);
        }
        th {
            background: var(--main1);
            color: #fff;
            padding: 13px 8px;
            text-align: center;
            font-weight: 700;
            letter-spacing: 0.5px;
        }
        td {
            padding: 12px 8px;
            text-align: center;
            border-bottom: 1px solid #e0e4e9;
            font-size: 1rem;
        }
        tr:hover td { background: #f3f5fa;}
        .estado-ok {
            background: #2a384d;
            color: #fff;
            padding: 7px 15px;
            border-radius: 18px;
            font-weight: bold;
            font-size: 0.9rem;
            display: inline-block;
            min-width: 90px;
        }
        .estado-proximo {
            background: #dbb600;
            color: #23293a;
            padding: 7px 15px;
            border-radius: 18px;
            font-weight: bold;
            font-size: 0.9rem;
            display: inline-block;
            min-width: 90px;
            animation: pulse 2s infinite;
        }
        .estado-vencido {
            background: #b35454;
            color: #fff;
            padding: 7px 15px;
            border-radius: 18px;
            font-weight: bold;
            font-size: 0.9rem;
            display: inline-block;
            min-width: 90px;
            animation: blink 1.2s infinite;
        }
        @keyframes pulse { 0%,100%{opacity:1;}50%{opacity:0.7;}}
        @keyframes blink { 0%,100%{opacity:1;}50%{opacity:0.5;} }
        .page-title {
            font-family: 'Montserrat', sans-serif;
            color: var(--main2);
            font-size: 1.3rem;
            margin-bottom: 1.3rem;
            font-weight: 900;
            letter-spacing: 1px;
        }
        .apartados-list {
            list-style: none;
            padding: 0;
            display: flex;
            flex-wrap: wrap;
            gap: 1.5rem;
        }
        .apartados-list li {
            background: var(--main1);
            color: #fff;
            padding: 1.7rem 2.2rem;
            border-radius: 14px;
            font-size: 1.2rem;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 1rem;
            box-shadow: 0 2px 12px #23293a12;
        }
        .apartados-list li i { font-size: 2rem;}
        .no-results {
            text-align: center;
            color: #7f8c8d;
            font-size: 1.05rem;
            padding: 2rem 0;
        }
        @media (max-width: 900px) {
            .header, nav, .page { max-width: 99vw; }
        }
        @media (max-width: 650px) {
            .header { flex-direction: column; text-align: center;}
            .logo-inst { margin-bottom: 1rem;}
            .page { padding: 1.5rem 0.5rem;}
        }
    </style>
</head>
<body>
    <div class="header">
        <img src="https://raw.githubusercontent.com/campo0099/control-maquinaria-alcaldia_SC/main/logo.jpg" alt="Logo institucional" class="logo-inst" />
        <div class="title-group">
            <div class="main-title">
                <i class="fas fa-cogs" style="color:var(--main3);"></i>
                Sistema de Control de Maquinaria
            </div>
            <div class="subtitle">
                Plataforma Municipal Formal de Gestión y Mantenimiento
            </div>
            <div class="inst-name">
                Alcaldía Municipal de Ejemplo
            </div>
        </div>
    </div>
    <nav>
        <a href="#" class="active" onclick="showPage('dashboard', event)"><i class="fas fa-chart-bar"></i> Dashboard</a>
        <a href="#" onclick="showPage('apartados', event)"><i class="fas fa-bars"></i> Apartados</a>
        <a href="#" onclick="showPage('inventario', event)"><i class="fas fa-warehouse"></i> Inventario</a>
        <a href="#" onclick="showPage('hojasvida', event)"><i class="fas fa-id-card"></i> Hojas de Vida</a>
    </nav>

    <!-- Dashboard -->
    <div class="page active" id="dashboard">
        <div class="page-title"><i class="fas fa-chart-bar"></i> Resumen General</div>
        <div class="stats-bar" id="statsBar">
            <div class="stat-item">
                <div class="stat-number" id="totalMaquinas">0</div>
                <div class="stat-label">Total Máquinas</div>
            </div>
            <div class="stat-item">
                <div class="stat-number" id="alTiempo">0</div>
                <div class="stat-label">A Tiempo</div>
            </div>
            <div class="stat-item">
                <div class="stat-number" id="proximas">0</div>
                <div class="stat-label">Próximas</div>
            </div>
            <div class="stat-item">
                <div class="stat-number" id="vencidas">0</div>
                <div class="stat-label">Vencidas</div>
            </div>
        </div>
        <div id="alertBanner" class="alert-banner" style="display: none;">
            ⚠️ <span id="alertText"></span>
        </div>
        <div style="margin-top:2rem;text-align:center;color:var(--main5);">
            <i class="fas fa-info-circle"></i> Use el menú superior para navegar entre apartados, inventario y hojas de vida de máquinas.
        </div>
    </div>
    <!-- Apartados -->
    <div class="page" id="apartados">
        <div class="page-title"><i class="fas fa-bars"></i> Apartados del Sistema</div>
        <ul class="apartados-list">
            <li><i class="fas fa-warehouse"></i> Inventario</li>
            <li><i class="fas fa-file-alt"></i> Hojas de Vida</li>
            <li><i class="fas fa-calendar-check"></i> Mantenimientos</li>
            <li><i class="fas fa-chart-line"></i> Reportes</li>
            <li><i class="fas fa-users-cog"></i> Administración</li>
        </ul>
    </div>
    <!-- Inventario -->
    <div class="page" id="inventario">
        <div class="page-title"><i class="fas fa-warehouse"></i> Inventario de Maquinaria</div>
        <div class="controls">
            <input type="text" id="searchInput" class="search-box" placeholder="🔍 Buscar por ID, tipo o marca...">
            <select id="filterEstado" class="filter-select">
                <option value="">Todos los estados</option>
                <option value="A tiempo">A tiempo</option>
                <option value="Próximo">Próximo</option>
                <option value="Vencido">Vencido</option>
            </select>
            <select id="filterTipo" class="filter-select">
                <option value="">Todos los tipos</option>
                <option value="Volqueta">Volqueta</option>
                <option value="Retroexcavadora">Retroexcavadora</option>
                <option value="Motoniveladora">Motoniveladora</option>
                <option value="Compactadora">Compactadora</option>
                <option value="Camión">Camión</option>
            </select>
            <button class="btn btn-primary" onclick="openModal()"><i class="fas fa-plus"></i> Agregar Máquina</button>
        </div>
        <div class="table-container">
            <table id="maquinariaTable">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Tipo</th>
                        <th>Marca</th>
                        <th>Modelo</th>
                        <th>Último Mant.</th>
                        <th>Próximo Mant.</th>
                        <th>Días Restantes</th>
                        <th>Estado</th>
                        <th>Acciones</th>
                    </tr>
                </thead>
                <tbody id="maquinariaTableBody"></tbody>
            </table>
            <div id="noResults" class="no-results" style="display: none;">
                <i class="fas fa-search"></i> No se encontraron resultados con los filtros aplicados
            </div>
        </div>
    </div>
    <!-- Hojas de Vida -->
    <div class="page" id="hojasvida">
        <div class="page-title"><i class="fas fa-id-card"></i> Hojas de Vida de Máquinas</div>
        <div id="hojasVidaList"></div>
    </div>

    <!-- Modal para agregar/editar máquina -->
    <div id="maquinariaModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeModal()">&times;</span>
            <h2 id="modalTitle" style="font-family:'Montserrat',sans-serif;color:var(--main2);margin-bottom:1rem;">
                <i class="fas fa-tractor"></i> <span id="modalTitleText">Agregar Nueva Máquina</span>
            </h2>
            <form id="maquinariaForm">
                <div class="form-group">
                    <label for="tipo">Tipo de Máquina:</label>
                    <select id="tipo" required>
                        <option value="">Seleccionar tipo...</option>
                        <option value="Volqueta">🚛 Volqueta</option>
                        <option value="Retroexcavadora">🚜 Retroexcavadora</option>
                        <option value="Motoniveladora">🏗️ Motoniveladora</option>
                        <option value="Compactadora">🗜️ Compactadora</option>
                        <option value="Camión">🚚 Camión</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="marca">Marca:</label>
                    <input type="text" id="marca" required>
                </div>
                <div class="form-group">
                    <label for="modelo">Modelo:</label>
                    <input type="text" id="modelo" required>
                </div>
                <div class="form-group">
                    <label for="ultimoMantenimiento">Último Mantenimiento:</label>
                    <input type="date" id="ultimoMantenimiento" required>
                </div>
                <div class="form-group">
                    <label for="intervaloMantenimiento">Intervalo de Mantenimiento (días):</label>
                    <input type="number" id="intervaloMantenimiento" value="180" min="30" max="365" required>
                </div>
                <div style="display: flex; gap: 1rem; margin-top: 1.5rem;">
                    <button type="submit" class="btn btn-primary" style="flex: 1;">
                        <i class="fas fa-save"></i> Guardar
                    </button>
                    <button type="button" class="btn btn-warning" onclick="closeModal()" style="flex: 1;">
                        Cancelar
                    </button>
                </div>
            </form>
        </div>
    </div>
    <script>
    // Páginas y navegación
    function showPage(pageId, e) {
        if (e) e.preventDefault();
        document.querySelectorAll('nav a').forEach(a=>a.classList.remove('active'));
        document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
        document.querySelector('nav a[onclick*="'+pageId+'"]').classList.add('active');
        document.getElementById(pageId).classList.add('active');
        if(pageId === "hojasvida") renderHojasVida();
        if(pageId === "dashboard") updateStatsBar();
        if(pageId === "inventario") filtrarDatos();
    }

    // ----- Datos -----
    let maquinaria = [
        { id: 1, tipo: "Volqueta", marca: "Marca A", modelo: "V-2024", ultimoMantenimiento: "2024-01-01", intervaloMantenimiento: 150 },
        { id: 2, tipo: "Retroexcavadora", marca: "Marca B", modelo: "RX-300", ultimoMantenimiento: "2024-03-15", intervaloMantenimiento: 180 },
        { id: 3, tipo: "Motoniveladora", marca: "Caterpillar", modelo: "120M", ultimoMantenimiento: "2024-05-20", intervaloMantenimiento: 120 },
        { id: 4, tipo: "Compactadora", marca: "BOMAG", modelo: "BW213", ultimoMantenimiento: "2024-02-10", intervaloMantenimiento: 200 },
        { id: 5, tipo: "Camión", marca: "Mercedes-Benz", modelo: "Actros", ultimoMantenimiento: "2024-06-01", intervaloMantenimiento: 90 }
    ];
    let editingId = null;
    const tipoIconos = {
        "Volqueta": "🚛",
        "Retroexcavadora": "🚜",
        "Motoniveladora": "🏗️",
        "Compactadora": "🗜️",
        "Camión": "🚚"
    };
    function calcularEstado(maquina) {
        const ultimoMantenimiento = new Date(maquina.ultimoMantenimiento);
        const hoy = new Date();
        const proximoMantenimiento = new Date(ultimoMantenimiento.getTime() + (maquina.intervaloMantenimiento * 24 * 60 * 60 * 1000));
        const diasRestantes = Math.ceil((proximoMantenimiento - hoy) / (1000 * 60 * 60 * 24));
        let estado;
        if (diasRestantes < 0) estado = "Vencido";
        else if (diasRestantes <= 30) estado = "Próximo";
        else estado = "A tiempo";
        return {
            proximoMantenimiento: proximoMantenimiento.toISOString().split('T')[0],
            diasRestantes,
            estado
        };
    }
    function renderizarTabla(datos = maquinaria) {
        const tbody = document.getElementById('maquinariaTableBody');
        const noResults = document.getElementById('noResults');
        if (!tbody) return;
        if (datos.length === 0) {
            tbody.innerHTML = '';
            noResults.style.display = 'block';
            updateStatsBar([]);
            verificarAlertas([]);
            return;
        }
        noResults.style.display = 'none';
        tbody.innerHTML = datos.map(maquina => {
            const info = calcularEstado(maquina);
            const estadoClass = `estado-${info.estado.toLowerCase().replace(' ', '-')}`;
            const icono = tipoIconos[maquina.tipo] || "🚧";
            return `
                <tr>
                    <td><strong>${maquina.id}</strong></td>
                    <td><span style="font-size:1.25em">${icono}</span> ${maquina.tipo}</td>
                    <td>${maquina.marca}</td>
                    <td>${maquina.modelo}</td>
                    <td>${formatearFecha(maquina.ultimoMantenimiento)}</td>
                    <td>${formatearFecha(info.proximoMantenimiento)}</td>
                    <td><strong>${info.diasRestantes > 0 ? info.diasRestantes : 'Vencido'}</strong></td>
                    <td><span class="${estadoClass}">${info.estado}</span></td>
                    <td>
                        <div class="actions">
                            <button class="btn btn-primary btn-small" onclick="editarMaquina(${maquina.id})"><i class="fas fa-edit"></i></button>
                            <button class="btn btn-success btn-small" onclick="realizarMantenimiento(${maquina.id})"><i class="fas fa-wrench"></i></button>
                            <button class="btn btn-warning btn-small" onclick="eliminarMaquina(${maquina.id})"><i class="fas fa-trash"></i></button>
                        </div>
                    </td>
                </tr>
            `;
        }).join('');
        updateStatsBar(datos);
        verificarAlertas(datos);
    }
    function formatearFecha(fecha) {
        return new Date(fecha).toLocaleDateString('es-ES', { day: '2-digit', month: '2-digit', year: 'numeric' });
    }
    function updateStatsBar(datos = maquinaria) {
        const stats = datos.reduce((acc, maquina) => {
            const info = calcularEstado(maquina);
            acc.total++;
            if (info.estado === "A tiempo") acc.alTiempo++;
            if (info.estado === "Próximo") acc.proximas++;
            if (info.estado === "Vencido") acc.vencidas++;
            return acc;
        }, { total: 0, alTiempo: 0, proximas: 0, vencidas: 0 });
        document.getElementById('totalMaquinas').textContent = stats.total;
        document.getElementById('alTiempo').textContent = stats.alTiempo;
        document.getElementById('proximas').textContent = stats.proximas;
        document.getElementById('vencidas').textContent = stats.vencidas;
    }
    function verificarAlertas(datos = maquinaria) {
        const vencidas = datos.filter(m => calcularEstado(m).estado === "Vencido");
        const banner = document.getElementById('alertBanner');
        const alertText = document.getElementById('alertText');
        if (banner && alertText) {
            if (vencidas.length > 0) {
                banner.style.display = 'block';
                alertText.textContent = `${vencidas.length} máquina(s) tienen mantenimiento vencido. ¡Se requiere atención inmediata!`;
            } else {
                banner.style.display = 'none';
            }
        }
    }
    function filtrarDatos() {
        const searchTerm = document.getElementById('searchInput')?.value.toLowerCase() || "";
        const estadoFilter = document.getElementById('filterEstado')?.value || "";
        const tipoFilter = document.getElementById('filterTipo')?.value || "";
        const datosFiltrados = maquinaria.filter(maquina => {
            const info = calcularEstado(maquina);
            const matchSearch =
                !searchTerm ||
                maquina.id.toString().includes(searchTerm) ||
                maquina.tipo.toLowerCase().includes(searchTerm) ||
                maquina.marca.toLowerCase().includes(searchTerm) ||
                maquina.modelo.toLowerCase().includes(searchTerm);
            const matchEstado = !estadoFilter || info.estado === estadoFilter;
            const matchTipo = !tipoFilter || maquina.tipo === tipoFilter;
            return matchSearch && matchEstado && matchTipo;
        });
        renderizarTabla(datosFiltrados);
    }
    document.getElementById('searchInput').addEventListener('input', filtrarDatos);
    document.getElementById('filterEstado').addEventListener('change', filtrarDatos);
    document.getElementById('filterTipo').addEventListener('change', filtrarDatos);

    function openModal() {
        document.getElementById('maquinariaModal').style.display = 'block';
        document.getElementById('modalTitleText').textContent = 'Agregar Nueva Máquina';
        document.getElementById('maquinariaForm').reset();
        editingId = null;
    }
    function closeModal() {
        document.getElementById('maquinariaModal').style.display = 'none';
        editingId = null;
    }
    document.getElementById('maquinariaForm').addEventListener('submit', function(e) {
        e.preventDefault();
        const formData = {
            tipo: document.getElementById('tipo').value,
            marca: document.getElementById('marca').value,
            modelo: document.getElementById('modelo').value,
            ultimoMantenimiento: document.getElementById('ultimoMantenimiento').value,
            intervaloMantenimiento: parseInt(document.getElementById('intervaloMantenimiento').value)
        };
        if (editingId) {
            const index = maquinaria.findIndex(m => m.id === editingId);
            maquinaria[index] = { ...maquinaria[index], ...formData };
        } else {
            const newId = Math.max(...maquinaria.map(m => m.id), 0) + 1;
            maquinaria.push({ id: newId, ...formData });
        }
        guardarMaquinaria();
        filtrarDatos();
        closeModal();
    });
    function editarMaquina(id) {
        const maquina = maquinaria.find(m => m.id === id);
        if (!maquina) return;
        document.getElementById('tipo').value = maquina.tipo;
        document.getElementById('marca').value = maquina.marca;
        document.getElementById('modelo').value = maquina.modelo;
        document.getElementById('ultimoMantenimiento').value = maquina.ultimoMantenimiento;
        document.getElementById('intervaloMantenimiento').value = maquina.intervaloMantenimiento;
        document.getElementById('modalTitleText').textContent = 'Editar Máquina';
        document.getElementById('maquinariaModal').style.display = 'block';
        editingId = id;
    }
    function realizarMantenimiento(id) {
        if (confirm('¿Confirmar que se realizó el mantenimiento hoy?')) {
            const maquina = maquinaria.find(m => m.id === id);
            if (maquina) {
                maquina.ultimoMantenimiento = new Date().toISOString().split('T')[0];
                guardarMaquinaria();
                filtrarDatos();
                alert('Mantenimiento registrado exitosamente');
            }
        }
    }
    function eliminarMaquina(id) {
        if (confirm('¿Está seguro de eliminar esta máquina del registro?')) {
            maquinaria = maquinaria.filter(m => m.id !== id);
            guardarMaquinaria();
            filtrarDatos();
        }
    }
    function guardarMaquinaria() {
        try {
            localStorage.setItem('maquinaria', JSON.stringify(maquinaria));
        } catch (e) { alert('No se pudo guardar los datos localmente.'); }
    }
    // Hojas de Vida por máquina
    function renderHojasVida() {
        const cont = document.getElementById('hojasVidaList');
        if (!cont) return;
        if (maquinaria.length === 0) {
            cont.innerHTML = '<div class="no-results"><i class="fas fa-search"></i> No hay máquinas registradas.</div>';
            return;
        }
        cont.innerHTML = maquinaria.map(maquina => {
            const info = calcularEstado(maquina);
            return `
                <div style="background:#fff;border-radius:12px;box-shadow:0 2px 12px #0001;padding:1.5rem 1rem;margin-bottom:1.5rem;">
                    <div style="font-family:Montserrat;font-weight:900;font-size:1.1rem;color:var(--main2);margin-bottom:0.3rem;">
                        ${tipoIconos[maquina.tipo]||'🚧'} ${maquina.tipo} <span style="color:var(--main5);font-weight:400;font-size:0.94rem;">(${maquina.modelo})</span>
                    </div>
                    <div><b>ID:</b> ${maquina.id} &nbsp; <b>Marca:</b> ${maquina.marca}</div>
                    <div><b>Último Mantenimiento:</b> ${formatearFecha(maquina.ultimoMantenimiento)}</div>
                    <div><b>Próximo Mantenimiento:</b> ${formatearFecha(calcularEstado(maquina).proximoMantenimiento)}</div>
                    <div><b>Días restantes:</b> ${info.diasRestantes > 0 ? info.diasRestantes : 'Vencido'} &nbsp; <span class="estado-${info.estado.toLowerCase().replace(' ', '-')}">${info.estado}</span></div>
                    <div style="margin-top:0.7rem;color:var(--main3);"><i class="fas fa-file-alt"></i> Aquí puede añadir historial, adjuntos o detalles adicionales.</div>
                </div>
            `;
        }).join('');
    }
    window.onclick = function(event) {
        const modal = document.getElementById('maquinariaModal');
        if (event.target === modal) closeModal();
    }
    document.addEventListener('DOMContentLoaded', function() {
        try {
            const savedData = localStorage.getItem('maquinaria');
            if (savedData) maquinaria = JSON.parse(savedData);
        } catch (e) {
            localStorage.removeItem('maquinaria');
            alert('Error al cargar los datos almacenados. Se reiniciará la base de datos.');
        }
        filtrarDatos();
        updateStatsBar();
    });
    </script>
</body>
</html>
