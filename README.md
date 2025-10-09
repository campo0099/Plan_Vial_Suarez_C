# Plan_Vial_Suarez_C
GESTION DE PLAN VIAL
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Gestión de Maquinaria - Alcaldía de Suárez</title>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&family=Montserrat:wght@900&display=swap" rel="stylesheet">
    <!-- FontAwesome -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/js/all.min.js"></script>
    <!-- SheetJS (Excel Export) -->
    <script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
    <!-- jsPDF (PDF Export) -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
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
            font-size: 2.2rem;
            color: var(--main1);
            letter-spacing: 2px;
            margin-bottom: 0.35rem;
            text-shadow: 1px 2px 7px #d1d5db, 0px 1px 2px #5b6b8277;
            border-bottom: 4px solid var(--main3);
            padding-bottom: 0.3rem;
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
            flex-wrap: wrap;
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
        .page-title {
            font-family: 'Montserrat', sans-serif;
            color: var(--main2);
            font-size: 1.3rem;
            margin-bottom: 1.3rem;
            font-weight: 900;
            letter-spacing: 1px;
        }
        .stat-bar {
            display: flex;
            gap: 2rem;
            margin-bottom: 2.5rem;
            flex-wrap: wrap;
        }
        .stat-item {
            background: var(--main1);
            color: #fff;
            padding: 1rem 1.5rem;
            border-radius: 12px;
            text-align: center;
            min-width: 120px;
            box-shadow: 0 2px 12px #23293a12;
        }
        .stat-number {
            font-size: 1.7rem;
            font-weight: 900;
        }
        .stat-label {
            font-size: 1rem;
            font-weight: 500;
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
        .controls, .table-container, .stat-bar {
            margin-bottom: 2rem;
        }
        .controls {
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
            align-items: center;
        }
        .controls input, .controls select {
            min-width: 140px;
        }
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
        textarea {
            font-family: 'Roboto', Arial, sans-serif;
            border: 1px solid #5b6b82;
            width: 100%;
            min-height: 120px;
            border-radius: 10px;
            padding: 1rem;
            font-size: 1rem;
        }
        .user-bar {
            display: flex;
            align-items: center;
            gap: 1rem;
            justify-content: flex-end;
            margin-bottom: 1rem;
            color: var(--main3);
            font-size: 1rem;
        }
        .user-bar .user-name {
            font-weight: 700;
            color: var(--main2);
        }
        .user-bar .btn {
            font-size: 0.92rem;
            padding: 6px 12px;
        }
        .form-group {
            margin-bottom: 1rem;
        }
        .form-group label {
            font-weight: 700;
            display: block;
            margin-bottom: 0.3rem;
        }
        .form-group input, .form-group select {
            width: 100%;
            padding: 0.5rem;
            border-radius: 7px;
            border: 1px solid var(--main3);
            font-size: 1rem;
        }
        .modal {
            display: none;
            position: fixed; z-index: 1000; top: 0; left: 0;
            width: 100vw; height: 100vh;
            background: #0006;
            align-items: center; justify-content: center;
        }
        .modal-content {
            background: #fff; border-radius: 14px; max-width: 400px;
            margin: 80px auto; padding: 2rem; position: relative;
        }
        .close {
            position: absolute; top: 12px; right: 22px;
            font-size: 2rem; cursor: pointer;
        }
        .alert-banner {
            background: #ffe066;
            color: #23293a;
            border-radius: 12px;
            padding: 1rem 1.5rem;
            font-weight: 700;
            font-size: 1rem;
            margin-bottom: 1rem;
            box-shadow: 0 2px 12px #0001;
        }
        .warning-message {
            background: #f8d7da;
            color: #842029;
            border-radius: 11px;
            padding: 1rem 1.5rem;
            font-weight: 700;
            font-size: 1rem;
            margin-bottom: 1rem;
        }
        @media (max-width: 900px) {
            .header, nav, .page { max-width: 99vw; }
        }
        @media (max-width: 650px) {
            .header { flex-direction: column; text-align: center;}
            .logo-inst { margin-bottom: 1rem;}
            .page { padding: 1.5rem 0.5rem;}
            .stat-bar { flex-direction: column; gap: 1rem;}
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
                Alcaldía Municipal de Suárez
            </div>
        </div>
    </div>
    <nav id="mainMenu">
        <a href="#" class="active" onclick="showPage('dashboard', event)"><i class="fas fa-chart-bar"></i> Dashboard</a>
        <a href="#" onclick="showPage('apartados', event)"><i class="fas fa-bars"></i> Apartados</a>
        <a href="#" onclick="showPage('inventario', event)"><i class="fas fa-warehouse"></i> Inventario</a>
        <a href="#" onclick="showPage('usuarios', event)"><i class="fas fa-users"></i> Usuarios</a>
        <a href="#" onclick="showPage('hojasvida', event)"><i class="fas fa-id-card"></i> Hojas de Vida</a>
        <a href="#" onclick="showPage('preventivo', event)"><i class="fas fa-tools"></i> Mantenimiento Preventivo</a>
        <a href="#" onclick="showPage('observaciones', event)"><i class="fas fa-comments"></i> Observaciones</a>
        <a href="#" onclick="showPage('seguimiento', event)"><i class="fas fa-user-cog"></i> Seguimiento Operario</a>
        <a href="#" onclick="showPage('reportes', event)"><i class="fas fa-file-excel"></i> Reportes</a>
    </nav>
    <div class="user-bar" id="userBar">
        <span id="userInfo"></span>
        <button class="btn btn-primary" id="btnLogin" onclick="openLoginModal()">Iniciar sesión</button>
        <button class="btn btn-success" id="btnRegister" onclick="openRegisterModal()">Registrarse</button>
        <button class="btn btn-warning" id="btnLogout" onclick="logout()" style="display:none;">Cerrar sesión</button>
        <button class="btn btn-primary" id="btnEditProfile" onclick="openEditProfile()" style="display:none;">Editar perfil</button>
    </div>
    <!-- Dashboard -->
    <div class="page active" id="dashboard">
        <div class="page-title"><i class="fas fa-chart-bar"></i> Resumen General</div>
        <div class="stat-bar" id="statsBar">
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
        <div id="dashboardInfo"></div>
    </div>
    <!-- Apartados -->
    <div class="page" id="apartados">
        <div class="page-title"><i class="fas fa-bars"></i> Apartados del Sistema</div>
        <ul class="apartados-list">
            <li><i class="fas fa-warehouse"></i> Inventario</li>
            <li><i class="fas fa-users"></i> Usuarios</li>
            <li><i class="fas fa-file-alt"></i> Hojas de Vida</li>
            <li><i class="fas fa-calendar-check"></i> Mantenimientos</li>
            <li><i class="fas fa-chart-line"></i> Reportes</li>
            <li><i class="fas fa-users-cog"></i> Administración</li>
            <li><i class="fas fa-tools"></i> Mantenimiento Preventivo</li>
            <li><i class="fas fa-comments"></i> Observaciones</li>
            <li><i class="fas fa-user-cog"></i> Seguimiento Operario</li>
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
            <button class="btn btn-primary" id="btnAddMaquina" onclick="openModal()"><i class="fas fa-plus"></i> Agregar Máquina</button>
            <button class="btn btn-success" id="btnExportExcel" onclick="exportInventario('excel')"><i class="fas fa-file-excel"></i> Exportar Excel</button>
            <button class="btn btn-danger" id="btnExportPDF" onclick="exportInventario('pdf')"><i class="fas fa-file-pdf"></i> Exportar PDF</button>
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
        <div id="warningInventario" class="warning-message" style="display:none;"></div>
    </div>
    <!-- Usuarios -->
    <div class="page" id="usuarios">
        <div class="page-title"><i class="fas fa-users"></i> Gestión de Usuarios</div>
        <div id="warningUsuarios" class="warning-message" style="display:none;"></div>
        <div style="margin-bottom:1rem;">
            <button class="btn btn-primary" onclick="openRegisterModal()"><i class="fas fa-user-plus"></i> Registrar usuario</button>
        </div>
        <table style="width:100%;">
            <thead>
                <tr>
                    <th>Nombre</th>
                    <th>Correo</th>
                    <th>Teléfono</th>
                    <th>Rol</th>
                    <th>Acciones</th>
                </tr>
            </thead>
            <tbody id="tablaUsuarios"></tbody>
        </table>
    </div>
    <!-- Hojas de Vida -->
    <div class="page" id="hojasvida">
        <div class="page-title"><i class="fas fa-id-card"></i> Hojas de Vida de Máquinas</div>
        <div id="hojasVidaList"></div>
    </div>
    <!-- Mantenimiento Preventivo -->
    <div class="page" id="preventivo">
        <div class="page-title"><i class="fas fa-tools"></i> Mantenimiento Preventivo</div>
        <div style="color:var(--main3);margin-bottom:1rem;">Gestione y registre mantenimientos preventivos aquí.</div>
        <table style="width:100%;margin-bottom:1rem;">
            <thead>
                <tr>
                    <th>ID Máquina</th>
                    <th>Tipo</th>
                    <th>Operación</th>
                    <th>Fecha</th>
                    <th>Responsable</th>
                    <th>Observaciones</th>
                    <th>Acción</th>
                </tr>
            </thead>
            <tbody id="tablaPreventivo">
                <!-- Dinámico -->
            </tbody>
        </table>
        <button class="btn btn-primary" id="btnAddPreventivo" onclick="agregarPreventivo()">Agregar Registro</button>
    </div>
    <!-- Observaciones -->
    <div class="page" id="observaciones">
        <div class="page-title"><i class="fas fa-comments"></i> Observaciones</div>
        <div style="color:var(--main3);margin-bottom:1rem;">Bitácora y comentarios generales sobre el parque automotor.</div>
        <textarea id="observacionesText"></textarea>
        <br>
        <button class="btn btn-success" onclick="guardarObservaciones()">Guardar Observaciones</button>
    </div>
    <!-- Seguimiento de Operario -->
    <div class="page" id="seguimiento">
        <div class="page-title"><i class="fas fa-user-cog"></i> Seguimiento de Operario y Máquinas</div>
        <div style="color:var(--main3);margin-bottom:1rem;">Consulte qué operario está asignado a cada máquina.</div>
        <table style="width:100%;">
            <thead>
                <tr>
                    <th>Operario</th>
                    <th>ID Máquina</th>
                    <th>Tipo</th>
                    <th>Modelo</th>
                    <th>Asignar/Quitar</th>
                </tr>
            </thead>
            <tbody id="tablaSeguimiento">
                <!-- Dinámico -->
            </tbody>
        </table>
    </div>
    <!-- Reportes -->
    <div class="page" id="reportes">
        <div class="page-title"><i class="fas fa-file-excel"></i> Reportes Generales</div>
        <div id="warningReportes" class="warning-message" style="display:none;"></div>
        <div>
            <h4>Inventario completo</h4>
            <button class="btn btn-success" onclick="exportReporte('inventario','excel')"><i class="fas fa-file-excel"></i> Exportar Excel</button>
            <button class="btn btn-danger" onclick="exportReporte('inventario','pdf')"><i class="fas fa-file-pdf"></i> Exportar PDF</button>
            <table style="width:100%;margin-top:1rem;">
                <thead>
                    <tr>
                        <th>ID</th><th>Tipo</th><th>Marca</th><th>Modelo</th>
                        <th>Último Mant.</th><th>Próximo Mant.</th><th>Estado</th>
                    </tr>
                </thead>
                <tbody id="tablaReporteInventario"></tbody>
            </table>
        </div>
        <div style="margin-top:2rem;">
            <h4>Mantenimientos preventivos</h4>
            <button class="btn btn-success" onclick="exportReporte('preventivo','excel')"><i class="fas fa-file-excel"></i> Exportar Excel</button>
            <button class="btn btn-danger" onclick="exportReporte('preventivo','pdf')"><i class="fas fa-file-pdf"></i> Exportar PDF</button>
            <table style="width:100%;margin-top:1rem;">
                <thead>
                    <tr>
                        <th>ID Máquina</th><th>Tipo</th><th>Operación</th><th>Fecha</th><th>Responsable</th><th>Observaciones</th>
                    </tr>
                </thead>
                <tbody id="tablaReportePreventivo"></tbody>
            </table>
        </div>
        <div style="margin-top:2rem;">
            <h4>Usuarios</h4>
            <button class="btn btn-success" onclick="exportReporte('usuarios','excel')"><i class="fas fa-file-excel"></i> Exportar Excel</button>
            <button class="btn btn-danger" onclick="exportReporte('usuarios','pdf')"><i class="fas fa-file-pdf"></i> Exportar PDF</button>
            <table style="width:100%;margin-top:1rem;">
                <thead>
                    <tr>
                        <th>Nombre</th><th>Correo</th><th>Teléfono</th><th>Rol</th>
                    </tr>
                </thead>
                <tbody id="tablaReporteUsuarios"></tbody>
            </table>
        </div>
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
    <!-- Modal de login -->
    <div id="loginModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeLoginModal()">&times;</span>
            <h2><i class="fas fa-sign-in-alt"></i> Iniciar sesión</h2>
            <form id="loginForm">
                <div class="form-group">
                    <label for="loginCorreo">Correo:</label>
                    <input type="email" id="loginCorreo" required>
                </div>
                <div class="form-group">
                    <label for="loginTelefono">Teléfono:</label>
                    <input type="text" id="loginTelefono" required>
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%;margin-top:1rem;">Ingresar</button>
            </form>
            <div style="margin-top:1rem;text-align:center;">
                ¿No tienes cuenta? <button class="btn btn-success btn-small" onclick="openRegisterModal();closeLoginModal();">Regístrate</button>
            </div>
        </div>
    </div>
    <!-- Modal de registro -->
    <div id="registerModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeRegisterModal()">&times;</span>
            <h2><i class="fas fa-user-plus"></i> Registro de usuario</h2>
            <form id="registerForm">
                <div class="form-group">
                    <label for="regNombre">Nombre:</label>
                    <input type="text" id="regNombre" required>
                </div>
                <div class="form-group">
                    <label for="regCorreo">Correo:</label>
                    <input type="email" id="regCorreo" required>
                </div>
                <div class="form-group">
                    <label for="regTelefono">Teléfono:</label>
                    <input type="text" id="regTelefono" required>
                </div>
                <div class="form-group">
                    <label for="regRol">Rol:</label>
                    <select id="regRol" required>
                        <option value="">Seleccione rol...</option>
                        <option value="Administrador">Administrador</option>
                        <option value="Operario">Operario</option>
                        <option value="Consulta">Consulta</option>
                    </select>
                </div>
                <button type="submit" class="btn btn-success" style="width:100%;margin-top:1rem;">Registrarse</button>
            </form>
        </div>
    </div>
    <!-- Modal edición perfil -->
    <div id="editProfileModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeEditProfile()">&times;</span>
            <h2><i class="fas fa-user-edit"></i> Editar perfil</h2>
            <form id="editProfileForm">
                <div class="form-group">
                    <label for="editNombre">Nombre:</label>
                    <input type="text" id="editNombre" required>
                </div>
                <div class="form-group">
                    <label for="editCorreo">Correo:</label>
                    <input type="email" id="editCorreo" required>
                </div>
                <div class="form-group">
                    <label for="editTelefono">Teléfono:</label>
                    <input type="text" id="editTelefono" required>
                </div>
                <div class="form-group">
                    <label for="editRol">Rol:</label>
                    <select id="editRol" required>
                        <option value="Administrador">Administrador</option>
                        <option value="Operario">Operario</option>
                        <option value="Consulta">Consulta</option>
                    </select>
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%;margin-top:1rem;">Guardar cambios</button>
            </form>
        </div>
    </div>
    <script>
    // --- Gestión de usuarios ---
    let usuarios = [];
    let usuarioActivo = null;

    function saveUsuarios() {
        localStorage.setItem('usuarios', JSON.stringify(usuarios));
    }
    function loadUsuarios() {
        const data = localStorage.getItem('usuarios');
        usuarios = data ? JSON.parse(data) : [];
    }
    function saveUsuarioActivo() {
        localStorage.setItem('usuarioActivo', JSON.stringify(usuarioActivo));
    }
    function loadUsuarioActivo() {
        const data = localStorage.getItem('usuarioActivo');
        usuarioActivo = data ? JSON.parse(data) : null;
    }
    function logout() {
        usuarioActivo = null;
        saveUsuarioActivo();
        actualizarVistaUsuario();
        showPage('dashboard');
        mostrarAdvertencia('dashboard', 'Sesión cerrada correctamente.');
    }
    function openLoginModal() {
        document.getElementById('loginModal').style.display = 'flex';
    }
    function closeLoginModal() {
        document.getElementById('loginModal').style.display = 'none';
        document.getElementById('loginForm').reset();
    }
    function openRegisterModal() {
        document.getElementById('registerModal').style.display = 'flex';
    }
    function closeRegisterModal() {
        document.getElementById('registerModal').style.display = 'none';
        document.getElementById('registerForm').reset();
    }
    function openEditProfile() {
        if (!usuarioActivo) return;
        document.getElementById('editProfileModal').style.display = 'flex';
        document.getElementById('editNombre').value = usuarioActivo.nombre;
        document.getElementById('editCorreo').value = usuarioActivo.correo;
        document.getElementById('editTelefono').value = usuarioActivo.telefono;
        document.getElementById('editRol').value = usuarioActivo.rol;
    }
    function closeEditProfile() {
        document.getElementById('editProfileModal').style.display = 'none';
        document.getElementById('editProfileForm').reset();
    }
    document.getElementById('loginForm').addEventListener('submit', function(e) {
        e.preventDefault();
        const correo = document.getElementById('loginCorreo').value.trim();
        const telefono = document.getElementById('loginTelefono').value.trim();
        const usuario = usuarios.find(u => u.correo === correo && u.telefono === telefono);
        if (usuario) {
            usuarioActivo = usuario;
            saveUsuarioActivo();
            actualizarVistaUsuario();
            closeLoginModal();
            showPage('dashboard');
            mostrarAdvertencia('dashboard', `¡Bienvenido, ${usuario.nombre}!`);
        } else {
            alert('Usuario no encontrado o datos incorrectos.');
        }
    });
    document.getElementById('registerForm').addEventListener('submit', function(e) {
        e.preventDefault();
        const nombre = document.getElementById('regNombre').value.trim();
        const correo = document.getElementById('regCorreo').value.trim();
        const telefono = document.getElementById('regTelefono').value.trim();
        const rol = document.getElementById('regRol').value;
        if (usuarios.find(u => u.correo === correo)) {
            alert('Ya existe un usuario con ese correo.');
            return;
        }
        const nuevo = { nombre, correo, telefono, rol };
        usuarios.push(nuevo);
        saveUsuarios();
        usuarioActivo = nuevo;
        saveUsuarioActivo();
        actualizarVistaUsuario();
        closeRegisterModal();
        showPage('dashboard');
        mostrarAdvertencia('dashboard', `Registro exitoso, ¡bienvenido ${nombre}!`);
    });
    document.getElementById('editProfileForm').addEventListener('submit', function(e) {
        e.preventDefault();
        if (!usuarioActivo) return;
        usuarioActivo.nombre = document.getElementById('editNombre').value.trim();
        usuarioActivo.correo = document.getElementById('editCorreo').value.trim();
        usuarioActivo.telefono = document.getElementById('editTelefono').value.trim();
        usuarioActivo.rol = document.getElementById('editRol').value;
        // Actualizar en la lista de usuarios
        const idx = usuarios.findIndex(u => u.correo === usuarioActivo.correo);
        if (idx !== -1) usuarios[idx] = usuarioActivo;
        saveUsuarios();
        saveUsuarioActivo();
        actualizarVistaUsuario();
        closeEditProfile();
        mostrarAdvertencia('dashboard', 'Perfil actualizado.');
        showPage('dashboard');
    });

    // Gestión de usuarios solo por Administrador
    function renderUsuarios() {
        const tbody = document.getElementById('tablaUsuarios');
        const warning = document.getElementById('warningUsuarios');
        if (!usuarioActivo || usuarioActivo.rol !== 'Administrador') {
            warning.style.display = 'block';
            warning.textContent = 'No tiene permisos para ver o editar usuarios.';
            tbody.innerHTML = '';
            return;
        }
        warning.style.display = 'none';
        tbody.innerHTML = usuarios.map((u, i) => `
            <tr>
                <td>${u.nombre}</td>
                <td>${u.correo}</td>
                <td>${u.telefono}</td>
                <td>${u.rol}</td>
                <td>
                    <button class="btn btn-primary btn-small" onclick="editarUsuario(${i})"><i class="fas fa-edit"></i></button>
                    <button class="btn btn-danger btn-small" onclick="eliminarUsuario(${i})"><i class="fas fa-trash"></i></button>
                </td>
            </tr>
        `).join('');
    }
    function editarUsuario(idx) {
        const u = usuarios[idx];
        document.getElementById('editNombre').value = u.nombre;
        document.getElementById('editCorreo').value = u.correo;
        document.getElementById('editTelefono').value = u.telefono;
        document.getElementById('editRol').value = u.rol;
        document.getElementById('editProfileModal').style.display = 'flex';
        // Al guardar, se sobrescribe el usuario en la lista
        document.getElementById('editProfileForm').onsubmit = function(e) {
            e.preventDefault();
            const nombre = document.getElementById('editNombre').value.trim();
            const correo = document.getElementById('editCorreo').value.trim();
            const telefono = document.getElementById('editTelefono').value.trim();
            const rol = document.getElementById('editRol').value;
            usuarios[idx] = { nombre, correo, telefono, rol };
            saveUsuarios();
            document.getElementById('editProfileModal').style.display = 'none';
            renderUsuarios();
        };
    }
    function eliminarUsuario(idx) {
        if (confirm('¿Está seguro de eliminar este usuario?')) {
            usuarios.splice(idx, 1);
            saveUsuarios();
            renderUsuarios();
        }
    }

    // --- Maquinaria ---
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
    function saveMaquinaria() {
        localStorage.setItem('maquinaria', JSON.stringify(maquinaria));
    }
    function loadMaquinaria() {
        const data = localStorage.getItem('maquinaria');
        maquinaria = data ? JSON.parse(data) : maquinaria;
    }
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
            let acciones = '';
            if (usuarioActivo && (usuarioActivo.rol === 'Administrador' || usuarioActivo.rol === 'Operario')) {
                acciones = `
                    <button class="btn btn-primary btn-small" onclick="editarMaquina(${maquina.id})"><i class="fas fa-edit"></i></button>
                    <button class="btn btn-success btn-small" onclick="realizarMantenimiento(${maquina.id})"><i class="fas fa-wrench"></i></button>
                    <button class="btn btn-warning btn-small" onclick="eliminarMaquina(${maquina.id})"><i class="fas fa-trash"></i></button>
                `;
            }
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
                    <td>${acciones}</td>
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
        if (!usuarioActivo || (usuarioActivo.rol !== 'Administrador' && usuarioActivo.rol !== 'Operario')) {
            mostrarAdvertencia('inventario', 'No tiene permisos para agregar máquinas.');
            return;
        }
        document.getElementById('maquinariaModal').style.display = 'flex';
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
        saveMaquinaria();
        filtrarDatos();
        closeModal();
    });
    function editarMaquina(id) {
        if (!usuarioActivo || (usuarioActivo.rol !== 'Administrador' && usuarioActivo.rol !== 'Operario')) {
            mostrarAdvertencia('inventario', 'No tiene permisos para editar máquinas.');
            return;
        }
        const maquina = maquinaria.find(m => m.id === id);
        if (!maquina) return;
        document.getElementById('tipo').value = maquina.tipo;
        document.getElementById('marca').value = maquina.marca;
        document.getElementById('modelo').value = maquina.modelo;
        document.getElementById('ultimoMantenimiento').value = maquina.ultimoMantenimiento;
        document.getElementById('intervaloMantenimiento').value = maquina.intervaloMantenimiento;
        document.getElementById('modalTitleText').textContent = 'Editar Máquina';
        document.getElementById('maquinariaModal').style.display = 'flex';
        editingId = id;
    }
    function realizarMantenimiento(id) {
        if (!usuarioActivo || (usuarioActivo.rol !== 'Administrador' && usuarioActivo.rol !== 'Operario')) {
            mostrarAdvertencia('inventario', 'No tiene permisos para registrar mantenimientos.');
            return;
        }
        if (confirm('¿Confirmar que se realizó el mantenimiento hoy?')) {
            const maquina = maquinaria.find(m => m.id === id);
            if (maquina) {
                maquina.ultimoMantenimiento = new Date().toISOString().split('T')[0];
                saveMaquinaria();
                filtrarDatos();
                alert('Mantenimiento registrado exitosamente');
            }
        }
    }
    function eliminarMaquina(id) {
        if (!usuarioActivo || usuarioActivo.rol !== 'Administrador') {
            mostrarAdvertencia('inventario', 'Solo el administrador puede eliminar máquinas.');
            return;
        }
        if (confirm('¿Está seguro de eliminar esta máquina del registro?')) {
            maquinaria = maquinaria.filter(m => m.id !== id);
            saveMaquinaria();
            filtrarDatos();
        }
    }

    // --- Exportación de Inventario ---
    function exportInventario(tipo) {
        if (!usuarioActivo || (usuarioActivo.rol !== 'Administrador' && usuarioActivo.rol !== 'Operario')) {
            mostrarAdvertencia('inventario', 'No tiene permisos para exportar datos.');
            return;
        }
        // Filtra la tabla actual
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
        exportarDatosTabulares(datosFiltrados, tipo, 'Inventario_Maquinaria');
    }
    function exportarDatosTabulares(datos, tipo, nombre) {
        if (tipo === 'excel') {
            const ws = XLSX.utils.json_to_sheet(datos.map(m => ({
                ID: m.id,
                Tipo: m.tipo,
                Marca: m.marca,
                Modelo: m.modelo,
                Ultimo_Mantenimiento: m.ultimoMantenimiento,
                Intervalo_Mantenimiento: m.intervaloMantenimiento
            })));
            const wb = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(wb, ws, 'Inventario');
            XLSX.writeFile(wb, nombre+'.xlsx');
        } else if (tipo === 'pdf') {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            doc.setFontSize(12);
            doc.text('Inventario de Maquinaria', 14, 14);
            let y = 25;
            datos.forEach((m, i) => {
                doc.text(`${i+1}. ID: ${m.id} | Tipo: ${m.tipo} | Marca: ${m.marca} | Modelo: ${m.modelo} | Último Mant.: ${m.ultimoMantenimiento}`, 14, y);
                y += 8;
                if (y > 270) { doc.addPage(); y = 20; }
            });
            doc.save(nombre+'.pdf');
        }
    }

    // --- Apartado de Reportes ---
    function renderReportes() {
        // Inventario
        const tbodyInv = document.getElementById('tablaReporteInventario');
        tbodyInv.innerHTML = maquinaria.map(m => {
            const info = calcularEstado(m);
            const estadoClass = `estado-${info.estado.toLowerCase().replace(' ', '-')}`;
            return `
                <tr>
                    <td>${m.id}</td>
                    <td>${m.tipo}</td>
                    <td>${m.marca}</td>
                    <td>${m.modelo}</td>
                    <td>${formatearFecha(m.ultimoMantenimiento)}</td>
                    <td>${formatearFecha(info.proximoMantenimiento)}</td>
                    <td><span class="${estadoClass}">${info.estado}</span></td>
                </tr>
            `;
        }).join('');
        // Preventivo
        const tbodyPrev = document.getElementById('tablaReportePreventivo');
        tbodyPrev.innerHTML = preventivos.map(p => `
            <tr>
                <td>${p.idMaquina}</td>
                <td>${p.tipo}</td>
                <td>${p.operacion}</td>
                <td>${formatearFecha(p.fecha)}</td>
                <td>${p.responsable}</td>
                <td>${p.observaciones||''}</td>
            </tr>
        `).join('');
        // Usuarios
        const tbodyUsu = document.getElementById('tablaReporteUsuarios');
        tbodyUsu.innerHTML = usuarios.map(u => `
            <tr>
                <td>${u.nombre}</td>
                <td>${u.correo}</td>
                <td>${u.telefono}</td>
                <td>${u.rol}</td>
            </tr>
        `).join('');
    }
    function exportReporte(tabla, tipo) {
        let datos = [];
        let nombre = '';
        if (tabla === 'inventario') {
            datos = maquinaria.map(m => ({
                ID: m.id,
                Tipo: m.tipo,
                Marca: m.marca,
                Modelo: m.modelo,
                Ultimo_Mantenimiento: m.ultimoMantenimiento,
                Intervalo_Mantenimiento: m.intervaloMantenimiento
            }));
            nombre = 'Reporte_Inventario';
        }
        if (tabla === 'preventivo') {
            datos = preventivos.map(p => ({
                ID_Maquina: p.idMaquina,
                Tipo: p.tipo,
                Operacion: p.operacion,
                Fecha: p.fecha,
                Responsable: p.responsable,
                Observaciones: p.observaciones
            }));
            nombre = 'Reporte_Preventivo';
        }
        if (tabla === 'usuarios') {
            datos = usuarios.map(u => ({
                Nombre: u.nombre,
                Correo: u.correo,
                Telefono: u.telefono,
                Rol: u.rol
            }));
            nombre = 'Reporte_Usuarios';
        }
        exportarDatosTabulares(datos, tipo, nombre);
    }

    // --- Hojas de Vida por máquina ---
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
    // --- Mantenimiento Preventivo ---
    let preventivos = [];
    function savePreventivos() {
        localStorage.setItem('preventivos', JSON.stringify(preventivos));
    }
    function loadPreventivos() {
        const data = localStorage.getItem('preventivos');
        preventivos = data ? JSON.parse(data) : [];
    }
    function renderPreventivo() {
        const tbody = document.getElementById('tablaPreventivo');
        if (!tbody) return;
        tbody.innerHTML = preventivos.length === 0 ?
            `<tr><td colspan="7" style="color:var(--main5);text-align:center;">Sin registros</td></tr>`
            : preventivos.map((p, i) => `
                <tr>
                    <td>${p.idMaquina}</td>
                    <td>${p.tipo}</td>
                    <td>${p.operacion}</td>
                    <td>${formatearFecha(p.fecha)}</td>
                    <td>${p.responsable}</td>
                    <td>${p.observaciones||''}</td>
                    <td><button class="btn btn-warning btn-small" onclick="eliminarPreventivo(${i})">Eliminar</button></td>
                </tr>
            `).join('');
    }
    function agregarPreventivo() {
        if (!usuarioActivo || (usuarioActivo.rol !== 'Administrador' && usuarioActivo.rol !== 'Operario')) {
            mostrarAdvertencia('preventivo', 'No tiene permisos para agregar registros.');
            return;
        }
        const idMaquina = prompt('ID de máquina:');
        const tipo = prompt('Tipo de máquina:');
        const operacion = prompt('Operación realizada:');
        const fecha = prompt('Fecha (AAAA-MM-DD):', new Date().toISOString().split('T')[0]);
        const responsable = prompt('Responsable:');
        const observaciones = prompt('Observaciones:');
        if(idMaquina && tipo && operacion && fecha && responsable) {
            preventivos.push({idMaquina, tipo, operacion, fecha, responsable, observaciones});
            savePreventivos();
            renderPreventivo();
            renderReportes();
        }
    }
    function eliminarPreventivo(idx) {
        if (!usuarioActivo || usuarioActivo.rol !== 'Administrador') {
            mostrarAdvertencia('preventivo', 'Solo el administrador puede eliminar registros.');
            return;
        }
        if(confirm('¿Eliminar este registro?')) {
            preventivos.splice(idx, 1);
            savePreventivos();
            renderPreventivo();
            renderReportes();
        }
    }

    // ---- Observaciones ----
    function guardarObservaciones() {
        const val = document.getElementById('observacionesText').value;
        localStorage.setItem('observaciones', val);
        alert('Observaciones guardadas');
    }
    function cargarObservaciones() {
        document.getElementById('observacionesText').value = localStorage.getItem('observaciones') || '';
    }

    // ---- Seguimiento Operario ----
    let operarios = [
        { nombre: "Juan Pérez", maquinas: [1, 3] },
        { nombre: "Ana Gómez", maquinas: [2] }
    ];
    function saveOperarios() {
        localStorage.setItem('operarios', JSON.stringify(operarios));
    }
    function loadOperarios() {
        const data = localStorage.getItem('operarios');
        operarios = data ? JSON.parse(data) : operarios;
    }
    function renderSeguimiento() {
        const tbody = document.getElementById('tablaSeguimiento');
        if (!tbody) return;
        let rows = '';
        operarios.forEach(op => {
            op.maquinas.forEach(id => {
                const maq = maquinaria.find(m=>m.id===id);
                if (maq) {
                    rows += `<tr>
                        <td>${op.nombre}</td>
                        <td>${maq.id}</td>
                        <td>${maq.tipo}</td>
                        <td>${maq.modelo}</td>
                        <td>
                            <button class="btn btn-danger btn-small" onclick="quitarAsignacion('${op.nombre}',${maq.id})">Quitar</button>
                        </td>
                    </tr>`;
                }
            });
        });
        // Agregar opción de asignar
        rows += `<tr>
            <td><input id="nuevoOperario" placeholder="Nuevo Operario"></td>
            <td><input id="nuevoIdMaquina" type="number" placeholder="ID Máquina"></td>
            <td colspan="2"></td>
            <td><button class="btn btn-primary btn-small" onclick="asignarMaquina()">Asignar</button></td>
        </tr>`;
        tbody.innerHTML = rows;
    }
    function quitarAsignacion(nombre, id) {
        const op = operarios.find(o=>o.nombre===nombre);
        if (op) {
            op.maquinas = op.maquinas.filter(mid=>mid!==id);
            if(op.maquinas.length === 0) {
                operarios = operarios.filter(o=>o.nombre!==nombre);
            }
            saveOperarios();
            renderSeguimiento();
        }
    }
    function asignarMaquina() {
        const nombre = document.getElementById('nuevoOperario').value.trim();
        const id = parseInt(document.getElementById('nuevoIdMaquina').value);
        if(!nombre || isNaN(id)) return alert('Complete los datos');
        let op = operarios.find(o=>o.nombre===nombre);
        if(!op) {
            operarios.push({nombre, maquinas: [id]});
        } else {
            if(!op.maquinas.includes(id)) op.maquinas.push(id);
        }
        saveOperarios();
        renderSeguimiento();
    }

    // --- Vista y navegación ---
    function showPage(pageId, e) {
        if (e) e.preventDefault();
        document.querySelectorAll('nav a').forEach(a=>a.classList.remove('active'));
        document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
        document.querySelector('nav a[onclick*="'+pageId+'"]').classList.add('active');
        document.getElementById(pageId).classList.add('active');
        // Mostrar advertencias si no tiene permisos
        if (pageId === "usuarios") renderUsuarios();
        if (pageId === "dashboard") updateStatsBar();
        if (pageId === "inventario") filtrarDatos();
        if (pageId === "preventivo") renderPreventivo();
        if (pageId === "seguimiento") renderSeguimiento();
        if (pageId === "observaciones") cargarObservaciones();
        if (pageId === "hojasvida") renderHojasVida();
        if (pageId === "reportes") renderReportes();
        ocultarAdvertencias();
    }
    function actualizarVistaUsuario() {
        loadUsuarioActivo();
        // Actualizar barra de usuario
        const bar = document.getElementById('userBar');
        const info = document.getElementById('userInfo');
        const btnLogin = document.getElementById('btnLogin');
        const btnRegister = document.getElementById('btnRegister');
        const btnLogout = document.getElementById('btnLogout');
        const btnEditProfile = document.getElementById('btnEditProfile');
        if (usuarioActivo) {
            info.textContent = `Bienvenido, ${usuarioActivo.nombre} (${usuarioActivo.rol})`;
            btnLogin.style.display = 'none';
            btnRegister.style.display = 'none';
            btnLogout.style.display = '';
            btnEditProfile.style.display = '';
            // Menú adaptado por rol
            document.getElementById('mainMenu').style.display = '';
            document.getElementById('btnAddMaquina').style.display = (usuarioActivo.rol==='Administrador' || usuarioActivo.rol==='Operario') ? '' : 'none';
            document.getElementById('btnExportExcel').style.display = (usuarioActivo.rol==='Administrador' || usuarioActivo.rol==='Operario') ? '' : 'none';
            document.getElementById('btnExportPDF').style.display = (usuarioActivo.rol==='Administrador' || usuarioActivo.rol==='Operario') ? '' : 'none';
            document.getElementById('btnAddPreventivo').style.display = (usuarioActivo.rol==='Administrador' || usuarioActivo.rol==='Operario') ? '' : 'none';
        } else {
            info.textContent = 'Vista previa - No autenticado';
            btnLogin.style.display = '';
            btnRegister.style.display = '';
            btnLogout.style.display = 'none';
            btnEditProfile.style.display = 'none';
            // Menú y botones limitados
            document.getElementById('mainMenu').style.display = '';
            document.getElementById('btnAddMaquina').style.display = 'none';
            document.getElementById('btnExportExcel').style.display = 'none';
            document.getElementById('btnExportPDF').style.display = 'none';
            document.getElementById('btnAddPreventivo').style.display = 'none';
        }
        // Dashboard info limitada si no está autenticado
        const dashInfo = document.getElementById('dashboardInfo');
        if (!usuarioActivo) {
            dashInfo.innerHTML = `<div style="margin-top:2rem;text-align:center;color:var(--main5);">
                <i class="fas fa-info-circle"></i> Inicie sesión para acceder a toda la funcionalidad y los datos confidenciales.
            </div>`;
        } else {
            dashInfo.innerHTML = '';
        }
    }
    function ocultarAdvertencias() {
        document.querySelectorAll('.warning-message').forEach(el=>el.style.display='none');
    }
    function mostrarAdvertencia(page, msg) {
        const el = document.getElementById('warning'+capitalize(page));
        if (el) {
            el.style.display = 'block';
            el.textContent = msg;
        }
    }
    function capitalize(t) { return t.charAt(0).toUpperCase()+t.slice(1);}
    window.onclick = function(event) {
        ['maquinariaModal','loginModal','registerModal','editProfileModal'].forEach(modalId=>{
            const modal = document.getElementById(modalId);
            if (event.target === modal) modal.style.display = 'none';
        });
    }
    document.addEventListener('DOMContentLoaded', function() {
        loadUsuarios();
        loadUsuarioActivo();
        loadMaquinaria();
        loadPreventivos();
        loadOperarios();
        actualizarVistaUsuario();
        filtrarDatos();
        updateStatsBar();
        renderReportes();
    });
    </script>
</body>
</html>
