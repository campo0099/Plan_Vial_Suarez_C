<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1"/>
  <title>Sistema de Control de Maquinaria - Alcaldía de Suárez (Con Hojas de Vida y Mantenimiento)</title>

  <!-- Fonts & icons -->
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&family=Montserrat:wght@700;900&display=swap" rel="stylesheet">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/js/all.min.js"></script>
  <!-- SheetJS (Excel) -->
  <script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
  <!-- jsPDF -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <style>
    :root{
      --bg: #f7f8fb;
      --card: #ffffff;
      --accent: #23293a;
      --muted: #5b6b82;
      --radius: 14px;
      --shadow: 0 12px 30px rgba(0,0,0,0.12);
    }
    html,body{margin:0;padding:0;font-family:Roboto,Arial,Helvetica,sans-serif;background:linear-gradient(135deg,#23293a 0%, #5b6b82 100%);min-height:100vh;color:var(--muted)}
    .container{max-width:1200px;margin:20px auto;padding:0 16px;}
    .header{display:flex;gap:16px;background:var(--card);padding:18px;border-radius:var(--radius);align-items:center;box-shadow:var(--shadow)}
    .logo{width:84px;height:84px;border-radius:12px;object-fit:cover;border:3px solid #dfe6ed}
    .title{flex:1}
    .title h1{margin:0;font-family:Montserrat,Arial;font-size:20px;color:var(--accent);letter-spacing:1px}
    .title p{margin:4px 0 0 0;color:var(--muted)}
    nav{margin:16px 0;display:flex;gap:6px;flex-wrap:wrap}
    nav a{background:var(--card);padding:10px 14px;border-radius:12px;text-decoration:none;color:var(--muted);font-weight:700;box-shadow:0 6px 20px rgba(0,0,0,0.06)}
    nav a.active{background:var(--accent);color:#fff}
    .user-bar{display:flex;gap:8px;justify-content:flex-end;align-items:center;margin-bottom:14px;}
    .btn{border:none;padding:8px 12px;border-radius:10px;font-weight:700;cursor:pointer}
    .btn-primary{background:linear-gradient(135deg,#23293a,#5b6b82);color:#fff}
    .btn-success{background:linear-gradient(135deg,#2a8f4f,#4cc57a);color:#fff}
    .btn-warning{background:#f7b500;color:#222}
    .btn-ghost{background:transparent;border:1px solid #e6e9ee;color:#fff}
    .page{display:none;background:var(--card);padding:18px;border-radius:var(--radius);box-shadow:var(--shadow);margin-bottom:16px}
    .page.active{display:block}
    .page-title{font-family:Montserrat;font-weight:900;margin:0 0 12px 0;color:var(--accent)}
    .controls{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:12px;align-items:center}
    .controls input,.controls select{padding:8px;border-radius:8px;border:1px solid #d7dee8}
    table{width:100%;border-collapse:collapse;background:transparent}
    th,td{padding:8px;border-bottom:1px solid #eef3f7;text-align:left;font-size:13px;color:var(--muted)}
    th{background:var(--accent);color:#fff;position:sticky;top:0}
    .table-scroll{overflow:auto;max-height:460px;border-radius:10px;padding:6px;background:#fff}
    .form-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:10px}
    .form-group{display:flex;flex-direction:column;gap:6px}
    label{font-weight:700;font-size:13px;color:var(--accent)}
    input[type="text"],input[type="number"],input[type="date"],select,textarea,input[type="password"]{padding:8px;border-radius:8px;border:1px solid #d7dee8}
    .small{font-size:12px;color:#7d8897}
    .badge-ok{background:#1d6b4f;color:#fff;padding:6px 10px;border-radius:20px;font-weight:800}
    .badge-warning{background:#dbb600;color:#222;padding:6px 10px;border-radius:20px;font-weight:800}
    .badge-danger{background:#b35454;color:#fff;padding:6px 10px;border-radius:20px;font-weight:800}
    .modal{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.5);align-items:center;justify-content:center;z-index:9999}
    .modal-content{background:#fff;padding:18px;border-radius:12px;max-width:1000px;width:95%;max-height:90vh;overflow:auto}
    .close{float:right;cursor:pointer;font-size:20px}
    .note{background:#fff3cd;border:1px solid #ffeeba;padding:10px;border-radius:8px;color:#856404;margin-bottom:12px}
    .flex-row{display:flex;gap:8px;align-items:center}
    .list-card{background:#fff;padding:12px;border-radius:10px;margin-bottom:10px;box-shadow:0 6px 18px rgba(0,0,0,0.04)}
    @media (max-width:760px){ .form-grid{grid-template-columns:1fr} }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <img class="logo" src="https://raw.githubusercontent.com/campo0099/control-maquinaria-alcaldia_SC/main/logo.jpg" alt="logo">
      <div class="title">
        <h1><i class="fas fa-cogs" style="color:#5b6b82"></i> Sistema de Control de Maquinaria - Alcaldía de Suárez</h1>
        <p>Inventario extendido · Hojas de Vida · Mantenimientos · Observaciones · Seguimiento</p>
      </div>
      <div style="text-align:right">
        <div id="userInfo" class="small">Vista previa - No autenticado</div>
      </div>
    </div>

    <nav id="mainMenu">
      <a href="#" class="active" onclick="showPage('dashboard',event)"><i class="fas fa-chart-bar"></i> Dashboard</a>
      <a href="#" onclick="showPage('inventario',event)"><i class="fas fa-warehouse"></i> Inventario</a>
      <a href="#" onclick="showPage('preventivo',event)"><i class="fas fa-tools"></i> Mantenimientos</a>
      <a href="#" onclick="showPage('hojasvida',event)"><i class="fas fa-file-alt"></i> Hojas de Vida</a>
      <a href="#" onclick="showPage('observaciones',event)"><i class="fas fa-comments"></i> Observaciones</a>
      <a href="#" onclick="showPage('seguimiento',event)"><i class="fas fa-user-cog"></i> Seguimiento</a>
      <a href="#" onclick="showPage('usuarios',event)"><i class="fas fa-users"></i> Usuarios</a>
      <a href="#" onclick="showPage('reportes',event)"><i class="fas fa-file-excel"></i> Reportes</a>
    </nav>

    <div class="user-bar">
      <button class="btn btn-primary" id="btnLogin" onclick="openLoginModal()">Iniciar sesión</button>
      <button class="btn btn-success" id="btnRegister" onclick="openRegisterModal()">Registrarse</button>
      <button class="btn btn-warning" id="btnLogout" onclick="logout()" style="display:none">Cerrar sesión</button>
      <button class="btn" id="btnEditProfile" onclick="openEditProfile()" style="display:none">Editar perfil</button>
    </div>

    <!-- DASHBOARD -->
    <div class="page active" id="dashboard">
      <div class="page-title"><i class="fas fa-chart-bar"></i> Resumen</div>
      <div style="display:flex;gap:12px;flex-wrap:wrap">
        <div style="background:#fff;padding:12px;border-radius:12px;min-width:160px;flex:1">
          <div class="small">Total máquinas</div>
          <div id="totalMaquinas" style="font-weight:900;font-size:20px">0</div>
        </div>
        <div style="background:#fff;padding:12px;border-radius:12px;min-width:160px;flex:1">
          <div class="small">A tiempo</div>
          <div id="alTiempo" style="font-weight:900;font-size:20px">0</div>
        </div>
        <div style="background:#fff;padding:12px;border-radius:12px;min-width:160px;flex:1">
          <div class="small">Próximas (30 días)</div>
          <div id="proximas" style="font-weight:900;font-size:20px">0</div>
        </div>
        <div style="background:#fff;padding:12px;border-radius:12px;min-width:160px;flex:1">
          <div class="small">Vencidas</div>
          <div id="vencidas" style="font-weight:900;font-size:20px">0</div>
        </div>
      </div>

      <div id="dashboardAlerts" style="margin-top:14px"></div>
      <div id="dashboardInfo" style="margin-top:14px">
        <div class="note"><i class="fas fa-info-circle"></i> Vista previa: inicie sesión para ver o editar datos confidenciales o usar importación/exportación.</div>
      </div>
    </div>

    <!-- INVENTARIO -->
    <div class="page" id="inventario">
      <div class="page-title"><i class="fas fa-warehouse"></i> Inventario de Maquinaria y Automotores</div>

      <div class="controls">
        <input id="searchInput" placeholder="Buscar por ID, placa, marca, modelo..." style="min-width:260px">
        <select id="filterTipo" onchange="filtrarDatos()">
          <option value="">Todos los tipos</option>
          <option>Motocicleta</option>
          <option>Motocarro</option>
          <option>Campero</option>
          <option>Camioneta</option>
          <option>Camión</option>
          <option>Volqueta</option>
          <option>Tractor</option>
          <option>Retro Excavadora</option>
          <option>Motoniveladora</option>
        </select>
        <button class="btn btn-primary" id="btnAddMaquina" onclick="openMaquinaModal()">Agregar máquina</button>
        <button class="btn btn-success" id="btnExportExcel" onclick="exportInventario('excel')">Exportar Excel</button>
        <button class="btn btn-primary" id="btnExportPDF" onclick="exportInventario('pdf')">Exportar PDF</button>
        <button class="btn" id="btnImportJSON" onclick="openImportModal()">Importar JSON</button>
        <button class="btn" id="btnDownloadJSON" onclick="downloadJSON()">Descargar JSON</button>
      </div>

      <div class="table-scroll" style="margin-top:8px">
        <table>
          <thead>
            <tr>
              <th>ID</th><th>TIPO</th><th>CLASE</th><th>MARCA</th><th>LINEA</th><th>MODELO</th><th>PLACA</th><th>N° MOTOR</th>
              <th>COLOR</th><th>CILINDRAJE</th><th>COMBUSTIBLE</th><th>RODAJE</th><th>CAPACIDAD</th><th>PESO</th><th>ALTO</th><th>ANCHO</th><th>LARGO</th>
              <th>Últ. Mant.</th><th>Próx. Mant.</th><th>Días</th><th>Estado</th><th>Acciones</th>
            </tr>
          </thead>
          <tbody id="maquinariaTableBody"></tbody>
        </table>
      </div>
      <div id="noResults" class="small" style="text-align:center;margin-top:8px;display:none;color:#666"><i class="fas fa-search"></i> No se encontraron resultados.</div>
    </div>

    <!-- MANTENIMIENTOS PREVENTIVOS -->
    <div class="page" id="preventivo">
      <div class="page-title"><i class="fas fa-tools"></i> Mantenimientos Programados y Historial</div>
      <div style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:12px">
        <button class="btn btn-primary" onclick="openProgramarPreventivo()">Programar mantenimiento</button>
        <select id="filterPreventivoEstado" onchange="renderPreventivos()">
          <option value="">Todos</option>
          <option value="Programado">Programado</option>
          <option value="Realizado">Realizado</option>
        </select>
        <input id="filterPreventivoMaquina" placeholder="Filtrar por ID máquina" oninput="renderPreventivos()">
      </div>
      <div id="preventivosList" class="table-scroll"></div>
    </div>

    <!-- HOJAS DE VIDA -->
    <div class="page" id="hojasvida">
      <div class="page-title"><i class="fas fa-file-alt"></i> Hojas de Vida</div>
      <div style="margin-bottom:12px">
        <input id="hvFilter" placeholder="Filtrar por ID máquina o tipo..." oninput="renderHojasVida()">
      </div>
      <div id="hojasVidaList"></div>
    </div>

    <!-- OBSERVACIONES -->
    <div class="page" id="observaciones">
      <div class="page-title"><i class="fas fa-comments"></i> Observaciones</div>
      <div style="margin-bottom:8px" class="small">Registro de observaciones generales. Se guarda en localStorage.</div>
      <textarea id="observacionesText" style="width:100%;height:140px;padding:8px;border-radius:8px;border:1px solid #ddd"></textarea>
      <div style="margin-top:8px;display:flex;gap:8px;justify-content:flex-end">
        <button class="btn btn-success" onclick="guardarObservaciones()">Guardar observaciones</button>
        <button class="btn" onclick="limpiarObservaciones()">Limpiar</button>
      </div>
      <div style="margin-top:16px" id="observacionesList"></div>
    </div>

    <!-- SEGUIMIENTO OPERARIO -->
    <div class="page" id="seguimiento">
      <div class="page-title"><i class="fas fa-user-cog"></i> Seguimiento de Operarios</div>
      <div style="margin-bottom:12px" class="flex-row">
        <input id="nuevoOperario" placeholder="Nombre operario">
        <input id="nuevoIdMaquina" placeholder="ID Máquina">
        <button class="btn btn-primary" onclick="asignarMaquina()">Asignar</button>
      </div>
      <div id="tablaSeguimiento"></div>
    </div>

    <!-- USUARIOS -->
    <div class="page" id="usuarios">
      <div class="page-title"><i class="fas fa-users"></i> Usuarios</div>
      <div style="margin-bottom:10px">
        <button class="btn btn-success" onclick="openRegisterModal()">Registrar nuevo usuario</button>
      </div>
      <div id="warningUsuarios" class="note" style="display:none"></div>
      <table>
        <thead><tr><th>Nombre</th><th>Correo</th><th>Teléfono</th><th>Rol</th><th>AdminPrincipal</th><th>Acciones</th></tr></thead>
        <tbody id="tablaUsuarios"></tbody>
      </table>
    </div>

    <!-- REPORTES -->
    <div class="page" id="reportes">
      <div class="page-title"><i class="fas fa-file-excel"></i> Reportes</div>
      <div style="margin-bottom:12px" class="small">Exporta inventario, mantenimientos o hojas de vida filtradas.</div>
      <div style="display:flex;gap:8px;margin-bottom:12px">
        <button class="btn btn-success" onclick="exportInventario('excel')">Exportar inventario (Excel)</button>
        <button class="btn btn-primary" onclick="exportInventario('pdf')">Exportar inventario (PDF)</button>
        <button class="btn btn-success" onclick="exportPreventivos('excel')">Exportar mantenimientos (Excel)</button>
        <button class="btn btn-primary" onclick="exportPreventivos('pdf')">Exportar mantenimientos (PDF)</button>
      </div>
      <div style="margin-top:12px" class="table-scroll">
        <table>
          <thead><tr><th>ID</th><th>TIPO</th><th>MARCA</th><th>MODELO</th><th>PLACA</th><th>Últ. Mant.</th><th>Próx. Mant.</th><th>Estado</th></tr></thead>
          <tbody id="tablaReporteInventario"></tbody>
        </table>
      </div>
    </div>

    <!-- MODALES -->
    <!-- Modal: Agregar / Editar Máquina -->
    <div id="maquinaModal" class="modal">
      <div class="modal-content">
        <span class="close" onclick="closeMaquinaModal()">&times;</span>
        <h3 id="maquinaModalTitle">Agregar Máquina</h3>
        <form id="maquinaForm">
          <div class="form-grid">
            <div class="form-group"><label>TIPO DE MAQUINARIA</label><input id="m_tipo" type="text" placeholder="ej. Volqueta"></div>
            <div class="form-group"><label>CLASE</label><input id="m_clase" type="text" placeholder="ej. Pesada"></div>
            <div class="form-group"><label>MARCA</label><input id="m_marca" type="text"></div>
            <div class="form-group"><label>LINEA</label><input id="m_linea" type="text"></div>
            <div class="form-group"><label>MODELO</label><input id="m_modelo" type="text"></div>
            <div class="form-group"><label>PLACA</label><input id="m_placa" type="text"></div>
            <div class="form-group"><label>N° MOTOR</label><input id="m_nromotor" type="text"></div>
            <div class="form-group"><label>COLOR</label><input id="m_color" type="text"></div>
            <div class="form-group"><label>CILINDRAJE</label><input id="m_cilindraje" type="text" placeholder="ej. 4500 CC"></div>
            <div class="form-group"><label>TIPO DE COMBUSTIBLE</label><input id="m_combustible" type="text" placeholder="Diesel / Gasolina"></div>
            <div class="form-group"><label>RODAJE</label><input id="m_rodaje" type="text"></div>
            <div class="form-group"><label>CAPACIDAD</label><input id="m_capacidad" type="text"></div>
            <div class="form-group"><label>PESO</label><input id="m_peso" type="text"></div>
            <div class="form-group"><label>ALTO (mm)</label><input id="m_alto" type="text"></div>
            <div class="form-group"><label>ANCHO (mm)</label><input id="m_ancho" type="text"></div>
            <div class="form-group"><label>LARGO (mm)</label><input id="m_largo" type="text"></div>
            <div class="form-group"><label>Último Mantenimiento</label><input id="m_ultimo" type="date"></div>
            <div class="form-group"><label>Intervalo (días)</label><input id="m_intervalo" type="number" min="1" value="180"></div>
          </div>
          <div style="margin-top:12px;display:flex;gap:8px;justify-content:flex-end">
            <button class="btn btn-primary" type="submit">Guardar</button>
            <button class="btn" type="button" onclick="closeMaquinaModal()">Cancelar</button>
          </div>
        </form>
      </div>
    </div>

    <!-- Modal: Programar Preventivo -->
    <div id="programarPreventivoModal" class="modal">
      <div class="modal-content">
        <span class="close" onclick="closeProgramarPreventivo()">&times;</span>
        <h3>Programar Mantenimiento Preventivo</h3>
        <form id="programarPreventivoForm">
          <div class="form-grid">
            <div class="form-group"><label>ID Máquina</label><input id="p_idMaquina" required></div>
            <div class="form-group"><label>Tipo operación</label><input id="p_operacion" required></div>
            <div class="form-group"><label>Fecha programada</label><input id="p_fecha" type="date" required></div>
            <div class="form-group"><label>Responsable</label><input id="p_responsable"></div>
            <div class="form-group" style="grid-column:1/-1"><label>Observaciones</label><textarea id="p_observaciones"></textarea></div>
          </div>
          <div style="margin-top:12px;display:flex;gap:8px;justify-content:flex-end">
            <button class="btn btn-success" type="submit">Programar</button>
            <button class="btn" type="button" onclick="closeProgramarPreventivo()">Cancelar</button>
          </div>
        </form>
      </div>
    </div>

    <!-- Modal: Hojas de Vida - Agregar entrada -->
    <div id="hvModal" class="modal">
      <div class="modal-content">
        <span class="close" onclick="closeHvModal()">&times;</span>
        <h3>Agregar entrada a Hoja de Vida</h3>
        <form id="hvForm">
          <div class="form-grid">
            <div class="form-group"><label>ID Máquina</label><input id="hv_idMaquina" required></div>
            <div class="form-group"><label>Fecha</label><input id="hv_fecha" type="date" required></div>
            <div class="form-group"><label>Tipo</label><select id="hv_tipo"><option>Preventivo</option><option>Correctivo</option><option>Inspección</option></select></div>
            <div class="form-group"><label>Operación</label><input id="hv_operacion"></div>
            <div class="form-group"><label>Responsable</label><input id="hv_responsable"></div>
            <div class="form-group" style="grid-column:1/-1"><label>Observaciones</label><textarea id="hv_observaciones"></textarea></div>
            <div class="form-group" style="grid-column:1/-1"><label>Adjuntar imagen (opcional)</label><input id="hv_adjuntos" type="file" accept="image/*"></div>
          </div>
          <div style="margin-top:12px;display:flex;gap:8px;justify-content:flex-end">
            <button class="btn btn-primary" type="submit">Agregar</button>
            <button class="btn" type="button" onclick="closeHvModal()">Cancelar</button>
          </div>
        </form>
      </div>
    </div>

    <!-- Modal: Login -->
    <div id="loginModal" class="modal">
      <div class="modal-content">
        <span class="close" onclick="closeLoginModal()">&times;</span>
        <h3>Iniciar sesión</h3>
        <form id="loginForm">
          <div class="form-group"><label>Correo</label><input id="loginCorreo" type="email" required></div>
          <div class="form-group"><label>Contraseña</label><input id="loginPassword" type="password" required></div>
          <div style="margin-top:10px"><button class="btn btn-primary" type="submit">Ingresar</button></div>
        </form>
        <div style="margin-top:8px" class="small">¿No tienes cuenta? <button class="btn btn-success btn-small" onclick="openRegisterModal(); closeLoginModal()">Regístrate</button></div>
      </div>
    </div>

    <!-- Modal: Registro -->
    <div id="registerModal" class="modal">
      <div class="modal-content">
        <span class="close" onclick="closeRegisterModal()">&times;</span>
        <h3>Registro</h3>
        <form id="registerForm">
          <div class="form-grid">
            <div class="form-group"><label>Nombre</label><input id="regNombre" required></div>
            <div class="form-group"><label>Correo</label><input id="regCorreo" type="email" required></div>
            <div class="form-group"><label>Teléfono</label><input id="regTelefono" required></div>
            <div class="form-group"><label>Contraseña</label><input id="regPassword" type="password" required></div>
            <div class="form-group"><label>Rol</label>
              <select id="regRol" required>
                <option value="">Seleccione...</option>
                <option>Operario</option>
                <option>Consulta</option>
              </select>
            </div>
          </div>
          <div style="margin-top:12px"><button class="btn btn-success" type="submit">Registrarse</button></div>
        </form>
      </div>
    </div>

    <!-- Modal: Edit profile -->
    <div id="editProfileModal" class="modal">
      <div class="modal-content">
        <span class="close" onclick="closeEditProfile()">&times;</span>
        <h3>Editar perfil</h3>
        <form id="editProfileForm">
          <div class="form-grid">
            <div class="form-group"><label>Nombre</label><input id="editNombre"></div>
            <div class="form-group"><label>Correo</label><input id="editCorreo" type="email"></div>
            <div class="form-group"><label>Teléfono</label><input id="editTelefono"></div>
            <div class="form-group"><label>Contraseña</label><input id="editPassword" type="password" placeholder="Dejar vacío para no cambiar"></div>
            <div class="form-group"><label>Rol</label>
              <select id="editRol"><option>Consulta</option><option>Operario</option><option>Administrador</option></select>
            </div>
          </div>
          <div style="margin-top:12px"><button class="btn btn-primary" type="submit">Guardar cambios</button></div>
        </form>
      </div>
    </div>

    <!-- Modal: Import JSON -->
    <div id="importModal" class="modal">
      <div class="modal-content">
        <span class="close" onclick="closeImportModal()">&times;</span>
        <h3>Importar JSON - Pega aquí un array de máquinas</h3>
        <div class="small" style="margin-bottom:8px">Cada objeto puede contener: id,tipo,clase,marca,linea,modelo,placa,nromotor,color,cilindraje,combustible,rodaje,capacidad,peso,alto,ancho,largo,ultimoMantenimiento,intervaloMantenimiento</div>
        <textarea id="importJsonText" style="width:100%;height:260px;padding:8px;border-radius:8px;border:1px solid #ddd"></textarea>
        <div style="margin-top:8px;display:flex;gap:8px;justify-content:flex-end">
          <button class="btn btn-success" onclick="confirmImport()">Importar</button>
          <button class="btn" onclick="closeImportModal()">Cancelar</button>
        </div>
      </div>
    </div>

  </div>

  <script>
    /**********************
       CONFIGURACIÓN PRINCIPAL
    **********************/
    const PRIMARY_ADMIN_EMAIL = 'admin@suarez.local';
    const PRIMARY_ADMIN_PASSWORD = 'AdminPass123';
    const MAX_ADMINS = 3;

    /**********************
       Datos y storage
    **********************/
    let usuarios = [];
    let usuarioActivo = null;
    let maquinaria = [];
    let preventivos = [];
    let hojaVida = [];
    let observaciones = [];
    let operarios = [];
    let editingId = null;

    // Storage helpers
    function saveUsers(){ localStorage.setItem('usuarios', JSON.stringify(usuarios)); }
    function loadUsers(){ usuarios = JSON.parse(localStorage.getItem('usuarios')||'[]'); }
    function saveActive(){ localStorage.setItem('usuarioActivo', JSON.stringify(usuarioActivo)); }
    function loadActive(){ usuarioActivo = JSON.parse(localStorage.getItem('usuarioActivo')||'null'); }
    function saveMaquinaria(){ localStorage.setItem('maquinaria', JSON.stringify(maquinaria)); }
    function loadMaquinaria(){ maquinaria = JSON.parse(localStorage.getItem('maquinaria')||'[]'); }
    function savePreventivos(){ localStorage.setItem('preventivos', JSON.stringify(preventivos)); }
    function loadPreventivos(){ preventivos = JSON.parse(localStorage.getItem('preventivos')||'[]'); }
    function saveHojaVida(){ localStorage.setItem('hojaVida', JSON.stringify(hojaVida)); }
    function loadHojaVida(){ hojaVida = JSON.parse(localStorage.getItem('hojaVida')||'[]'); }
    function saveObservaciones(){ localStorage.setItem('observacionesList', JSON.stringify(observaciones)); }
    function loadObservaciones(){ observaciones = JSON.parse(localStorage.getItem('observacionesList')||'[]'); }
    function saveOperarios(){ localStorage.setItem('operarios', JSON.stringify(operarios)); }
    function loadOperarios(){ operarios = JSON.parse(localStorage.getItem('operarios')||'[]'); }

    /**********************
       Utilitarios
    **********************/
    function formatearFecha(fecha){
      if(!fecha) return '';
      try { return new Date(fecha).toLocaleDateString('es-ES'); } catch(e){ return fecha; }
    }

    function calcularEstado(m){
      const ultimo = m.ultimoMantenimiento ? new Date(m.ultimoMantenimiento) : new Date();
      const hoy = new Date();
      const intervalo = parseInt(m.intervaloMantenimiento || 180, 10);
      const proximo = new Date(ultimo.getTime() + (intervalo * 24*60*60*1000));
      const dias = Math.ceil((proximo - hoy)/(1000*60*60*24));
      let estado = 'A tiempo';
      if(dias < 0) estado = 'Vencido';
      else if(dias <= 30) estado = 'Próximo';
      return { proximoMantenimiento: proximo.toISOString().split('T')[0], diasRestantes: dias, estado };
    }

    /**********************
       Inicialización admin principal
    **********************/
    function ensurePrimaryAdminExists(){
      loadUsers();
      const existingPrimary = usuarios.find(u => u.isPrimary && (u.correo === PRIMARY_ADMIN_EMAIL || u.email === PRIMARY_ADMIN_EMAIL));
      if(!existingPrimary){
        const primary = {
          nombre: 'Admin Principal',
          correo: PRIMARY_ADMIN_EMAIL,
          email: PRIMARY_ADMIN_EMAIL,
          telefono: '0000',
          password: PRIMARY_ADMIN_PASSWORD,
          rol: 'Administrador',
          isPrimary: true
        };
        const already = usuarios.find(u => u.correo === PRIMARY_ADMIN_EMAIL || u.email === PRIMARY_ADMIN_EMAIL);
        if(!already){
          usuarios.unshift(primary);
        } else {
          already.isPrimary = true;
          if(!already.password) already.password = PRIMARY_ADMIN_PASSWORD;
        }
        saveUsers();
      }
    }

    function countAdmins(){
      return usuarios.filter(u => u.rol === 'Administrador' || u.isPrimary).length;
    }
    function isPrimaryAdmin(user){ return !!user && !!user.isPrimary; }

    /**********************
       Renderizado Inventario / Stats
    **********************/
    function renderizarTabla(datos = maquinaria){
      const tbody = document.getElementById('maquinariaTableBody');
      const noResults = document.getElementById('noResults');
      if(!tbody) return;
      if(datos.length === 0){
        tbody.innerHTML = '';
        noResults.style.display = 'block';
        actualizarEstadisticas([]);
        return;
      }
      noResults.style.display = 'none';
      tbody.innerHTML = datos.map(m => {
        const info = calcularEstado(m);
        const estadoBadge = info.estado==='A tiempo' ? '<span class="badge-ok">A tiempo</span>' : (info.estado==='Próximo'?'<span class="badge-warning">Próximo</span>':'<span class="badge-danger">Vencido</span>');
        let acciones = '';
        if(usuarioActivo && (usuarioActivo.rol==='Administrador' || usuarioActivo.rol==='Operario')){
          acciones = `<button class="btn btn-primary btn-small" onclick="editarMaquina('${m.id}')" title="Editar"><i class="fas fa-edit"></i></button>
                      <button class="btn btn-success btn-small" onclick="registrarMantenimiento('${m.id}')" title="Registrar mantenimiento"><i class="fas fa-wrench"></i></button>`;
          if(usuarioActivo.rol==='Administrador'){
            acciones += ` <button class="btn btn-warning btn-small" onclick="eliminarMaquina('${m.id}')" title="Eliminar"><i class="fas fa-trash"></i></button>`;
          }
        }
        acciones += ` <button class="btn btn-ghost btn-small" onclick="verHojaVida('${m.id}')">H. Vida</button>`;
        return `
          <tr>
            <td>${m.id}</td>
            <td>${m.tipo||''}</td>
            <td>${m.clase||''}</td>
            <td>${m.marca||''}</td>
            <td>${m.linea||''}</td>
            <td>${m.modelo||''}</td>
            <td>${m.placa||''}</td>
            <td>${m.nromotor||''}</td>
            <td>${m.color||''}</td>
            <td>${m.cilindraje||''}</td>
            <td>${m.combustible||''}</td>
            <td>${m.rodaje||''}</td>
            <td>${m.capacidad||''}</td>
            <td>${m.peso||''}</td>
            <td>${m.alto||''}</td>
            <td>${m.ancho||''}</td>
            <td>${m.largo||''}</td>
            <td>${formatearFecha(m.ultimoMantenimiento)}</td>
            <td>${formatearFecha(info.proximoMantenimiento)}</td>
            <td>${info.diasRestantes>0?info.diasRestantes:'Vencido'}</td>
            <td>${estadoBadge}</td>
            <td>${acciones}</td>
          </tr>
        `;
      }).join('');
      actualizarEstadisticas(datos);
    }

    function actualizarEstadisticas(datos = maquinaria){
      const stats = datos.reduce((acc,m)=>{
        const info = calcularEstado(m);
        acc.total++;
        if(info.estado==='A tiempo') acc.at++;
        if(info.estado==='Próximo') acc.prox++;
        if(info.estado==='Vencido') acc.ven++;
        return acc;
      },{total:0,at:0,prox:0,ven:0});
      document.getElementById('totalMaquinas').textContent = stats.total;
      document.getElementById('alTiempo').textContent = stats.at;
      document.getElementById('proximas').textContent = stats.prox;
      document.getElementById('vencidas').textContent = stats.ven;
      renderDashboardAlerts();
    }

    function renderDashboardAlerts(){
      // Mantenimientos próximos en 30 días y vencidos
      const proximos = maquinaria.filter(m => calcularEstado(m).estado === 'Próximo');
      const vencidos = maquinaria.filter(m => calcularEstado(m).estado === 'Vencido');
      const container = document.getElementById('dashboardAlerts');
      let html = '';
      if(vencidos.length){
        html += `<div class="note" style="background:#f8d7da;color:#842029">⚠️ ${vencidos.length} máquina(s) con mantenimiento vencido. Revise Hojas de Vida y programe mantenimiento.</div>`;
      }
      if(proximos.length){
        html += `<div class="note" style="background:#fff3cd;color:#856404">🔔 ${proximos.length} máquina(s) con mantenimiento próximo en 30 días.</div>`;
      }
      container.innerHTML = html;
    }

    /**********************
       Filtrado
    **********************/
    document.getElementById('searchInput').addEventListener('input', filtrarDatos);
    document.getElementById('filterTipo').addEventListener('change', filtrarDatos);
    function filtrarDatos(){
      const term = (document.getElementById('searchInput').value||'').toLowerCase();
      const tipo = document.getElementById('filterTipo').value;
      const datos = maquinaria.filter(m=>{
        const matchTipo = !tipo || (m.tipo||'').toLowerCase().includes(tipo.toLowerCase());
        const matchTerm = !term || Object.values(m).some(v => String(v||'').toLowerCase().includes(term));
        return matchTipo && matchTerm;
      });
      renderizarTabla(datos);
    }

    /**********************
       CRUD Maquinaria
    **********************/
    function openMaquinaModal(){ if(!usuarioActivo || (usuarioActivo.rol!=='Administrador' && usuarioActivo.rol!=='Operario')){ alert('No tiene permisos para agregar máquinas.'); return; } editingId = null; document.getElementById('maquinaModalTitle').textContent = 'Agregar Máquina'; document.getElementById('maquinaForm').reset(); document.getElementById('maquinaModal').style.display = 'flex'; }
    function closeMaquinaModal(){ document.getElementById('maquinaModal').style.display = 'none'; editingId = null; }

    document.getElementById('maquinaForm').addEventListener('submit', function(e){
      e.preventDefault();
      const idVal = editingId || generarId();
      const m = {
        id: idVal,
        tipo: document.getElementById('m_tipo').value.trim(),
        clase: document.getElementById('m_clase').value.trim(),
        marca: document.getElementById('m_marca').value.trim(),
        linea: document.getElementById('m_linea').value.trim(),
        modelo: document.getElementById('m_modelo').value.trim(),
        placa: document.getElementById('m_placa').value.trim(),
        nromotor: document.getElementById('m_nromotor').value.trim(),
        color: document.getElementById('m_color').value.trim(),
        cilindraje: document.getElementById('m_cilindraje').value.trim(),
        combustible: document.getElementById('m_combustible').value.trim(),
        rodaje: document.getElementById('m_rodaje').value.trim(),
        capacidad: document.getElementById('m_capacidad').value.trim(),
        peso: document.getElementById('m_peso').value.trim(),
        alto: document.getElementById('m_alto').value.trim(),
        ancho: document.getElementById('m_ancho').value.trim(),
        largo: document.getElementById('m_largo').value.trim(),
        ultimoMantenimiento: document.getElementById('m_ultimo').value || "",
        intervaloMantenimiento: parseInt(document.getElementById('m_intervalo').value || 180, 10)
      };
      if(editingId){
        const idx = maquinaria.findIndex(x=>String(x.id)===String(editingId));
        if(idx!==-1) { maquinaria[idx] = m; }
      } else {
        maquinaria.push(m);
      }
      saveMaquinaria();
      renderizarTabla();
      closeMaquinaModal();
    });

    function editarMaquina(id){
      if(!usuarioActivo || (usuarioActivo.rol!=='Administrador' && usuarioActivo.rol!=='Operario')){ alert('No tiene permisos para editar máquinas.'); return; }
      const m = maquinaria.find(x=>String(x.id)===String(id)); if(!m) return;
      editingId = id;
      document.getElementById('maquinaModalTitle').textContent = 'Editar Máquina (ID '+id+')';
      document.getElementById('m_tipo').value = m.tipo||'';
      document.getElementById('m_clase').value = m.clase||'';
      document.getElementById('m_marca').value = m.marca||'';
      document.getElementById('m_linea').value = m.linea||'';
      document.getElementById('m_modelo').value = m.modelo||'';
      document.getElementById('m_placa').value = m.placa||'';
      document.getElementById('m_nromotor').value = m.nromotor||'';
      document.getElementById('m_color').value = m.color||'';
      document.getElementById('m_cilindraje').value = m.cilindraje||'';
      document.getElementById('m_combustible').value = m.combustible||'';
      document.getElementById('m_rodaje').value = m.rodaje||'';
      document.getElementById('m_capacidad').value = m.capacidad||'';
      document.getElementById('m_peso').value = m.peso||'';
      document.getElementById('m_alto').value = m.alto||'';
      document.getElementById('m_ancho').value = m.ancho||'';
      document.getElementById('m_largo').value = m.largo||'';
      document.getElementById('m_ultimo').value = m.ultimoMantenimiento||'';
      document.getElementById('m_intervalo').value = m.intervaloMantenimiento||180;
      document.getElementById('maquinaModal').style.display = 'flex';
    }

    function eliminarMaquina(id){
      if(!usuarioActivo || usuarioActivo.rol!=='Administrador'){ alert('Solo administrador puede eliminar máquinas.'); return; }
      if(!confirm('Eliminar máquina ID '+id+'?')) return;
      maquinaria = maquinaria.filter(m=>String(m.id)!==String(id));
      saveMaquinaria();
      renderizarTabla();
    }

    function registrarMantenimiento(id){
      if(!usuarioActivo || (usuarioActivo.rol!=='Administrador' && usuarioActivo.rol!=='Operario')){ alert('No tiene permisos para registrar mantenimiento.'); return; }
      if(!confirm('Registrar mantenimiento realizado hoy para ID '+id+'?')) return;
      const m = maquinaria.find(x=>String(x.id)===String(id));
      if(!m) return;
      m.ultimoMantenimiento = new Date().toISOString().split('T')[0];
      saveMaquinaria();
      // Agrega entrada en hoja de vida automáticamente
      const entry = {
        id: 'HV' + (hojaVida.length + 1),
        idMaquina: m.id,
        fecha: m.ultimoMantenimiento,
        tipo: 'Preventivo',
        operacion: 'Mantenimiento registrado (acciones rápidas)',
        responsable: usuarioActivo ? usuarioActivo.nombre : '',
        observaciones: '',
        adjuntos: []
      };
      hojaVida.push(entry);
      saveHojaVida();
      renderizarTabla();
      alert('Mantenimiento registrado y agregado a Hoja de Vida.');
    }

    function generarId(){
      const numericIds = maquinaria.map(m => {
        const n = parseInt(String(m.id).replace(/[^0-9]/g,''),10);
        return isNaN(n)?0:n;
      });
      const max = numericIds.length?Math.max(...numericIds):0;
      return 'X' + (max + 1);
    }

    /**********************
       Import / Export
    **********************/
    function exportInventario(tipo){
      if(!usuarioActivo || (usuarioActivo.rol!=='Administrador' && usuarioActivo.rol!=='Operario')){ alert('No tiene permisos para exportar datos.'); return; }
      const term = (document.getElementById('searchInput').value||'').toLowerCase();
      const tipoFiltro = document.getElementById('filterTipo').value;
      const datos = maquinaria.filter(m=>{
        const matchTipo = !tipoFiltro || (m.tipo||'').toLowerCase().includes(tipoFiltro.toLowerCase());
        const matchTerm = !term || Object.values(m).some(v=>String(v||'').toLowerCase().includes(term));
        return matchTipo && matchTerm;
      });
      const rows = datos.map(m=>({
        ID: m.id, TIPO_MAQUINARIA: m.tipo, CLASE: m.clase, MARCA: m.marca, LINEA: m.linea,
        MODELO: m.modelo, PLACA: m.placa, N_MOTOR: m.nromotor, COLOR: m.color, CILINDRAJE: m.cilindraje,
        COMBUSTIBLE: m.combustible, RODAJE: m.rodaje, CAPACIDAD: m.capacidad, PESO: m.peso,
        ALTO: m.alto, ANCHO: m.ancho, LARGO: m.largo, ULTIMO_MANT: m.ultimoMantenimiento, INTERVALO_DIAS: m.intervaloMantenimiento
      }));
      if(tipo==='excel'){
        const ws = XLSX.utils.json_to_sheet(rows);
        const wb = XLSX.utils.book_new();
        XLSX.utils.book_append_sheet(wb, ws, 'Inventario');
        XLSX.writeFile(wb, 'Inventario_Extendido.xlsx');
      } else if(tipo==='pdf'){
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF('landscape');
        doc.setFontSize(10);
        let y = 12;
        doc.text('Inventario de Maquinaria - Alcaldía de Suárez', 12, 10);
        rows.forEach((r, i)=>{
          const line = `${i+1}. ID:${r.ID} | ${r.TIPO_MAQUINARIA} | ${r.MARCA} ${r.MODELO} | PLACA:${r.PLACA} | Último Mant:${r.ULTIMO_MANT}`;
          doc.text(line, 12, y);
          y += 6;
          if(y > 275){ doc.addPage(); y = 12; }
        });
        doc.save('Inventario_Extendido.pdf');
      }
    }

    function downloadJSON(){
      const data = JSON.stringify(maquinaria, null, 2);
      const blob = new Blob([data], {type: 'application/json'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'maquinaria.json';
      a.click();
      URL.revokeObjectURL(url);
    }

    function openImportModal(){ document.getElementById('importJsonText').value=''; document.getElementById('importModal').style.display='flex'; }
    function closeImportModal(){ document.getElementById('importModal').style.display='none'; }

    function confirmImport(){
      if(!usuarioActivo || (usuarioActivo.rol!=='Administrador' && usuarioActivo.rol!=='Operario')){ alert('No tiene permisos para importar.'); return; }
      const txt = document.getElementById('importJsonText').value;
      if(!txt){ alert('Pegue el JSON primero'); return; }
      try {
        const arr = JSON.parse(txt);
        if(!Array.isArray(arr)){ alert('El JSON debe ser un array de objetos'); return; }
        arr.forEach(item=>{
          const obj = {
            id: item.id || item.CODIGO || item.COD || generarId(),
            tipo: item.tipo || item['TIPO'] || item['TIPO DE VEHICULO'] || item['TIPO DE MAQUINARIA'] || '',
            clase: item.clase || item.CLASE || '',
            marca: item.marca || item.MARCA || '',
            linea: item.linea || item.LINEA || '',
            modelo: item.modelo || item.MODELO || '',
            placa: item.placa || item.PLACA || '',
            nromotor: item.nromotor || item['N° MOTOR'] || item['N MOTOR'] || item.NUM_MOTOR || '',
            color: item.color || item.COLOR || '',
            cilindraje: normalizeCilindraje(item.cilindraje || item.CILINDRAJE || ''),
            combustible: item.combustible || item['TIPO DE COMBUSTIBLE'] || item.COMBUSTIBLE || '',
            rodaje: item.rodaje || item.RODAJE || '',
            capacidad: item.capacidad || item.CAPACIDAD || item['CAPACIDAD'] || '',
            peso: item.peso || item.PESO || '',
            alto: item.alto || item.ALTO || '',
            ancho: item.ancho || item.ANCHO || '',
            largo: item.largo || item.LARGO || '',
            ultimoMantenimiento: item.ultimoMantenimiento || item['Último Mantenimiento'] || item.ULTIMO_MANT || '',
            intervaloMantenimiento: parseInt(item.intervaloMantenimiento || item.INTERVALO_DIAS || item['Intervalo'] || 180, 10)
          };
          const existing = maquinaria.find(x=>String(x.id) === String(obj.id));
          if(existing){ Object.assign(existing, obj); } else { maquinaria.push(obj); }
        });
        saveMaquinaria();
        renderizarTabla();
        closeImportModal();
        alert('Importación finalizada correctamente.');
      } catch(e){
        alert('JSON inválido: ' + e.message);
      }
    }

    function normalizeCilindraje(text){
      if(!text) return '';
      const t = String(text).toUpperCase().replace(/\s+/g,' ').trim();
      let num = t.replace(/\./g,'').replace(/,/g,'').match(/(\d+)/);
      if(num) {
        const unidadMatch = t.match(/[A-Za-z]+$/);
        const unidad = unidadMatch ? unidadMatch[0] : '';
        return num[1] + (unidad ? ' ' + unidad : '');
      }
      return t;
    }

    /**********************
       Hojas de Vida
    **********************/
    function openHvModal(prefillId){
      document.getElementById('hvForm').reset();
      if(prefillId) document.getElementById('hv_idMaquina').value = prefillId;
      document.getElementById('hvModal').style.display = 'flex';
    }
    function closeHvModal(){ document.getElementById('hvModal').style.display = 'none'; }
    document.getElementById('hvForm').addEventListener('submit', function(e){
      e.preventDefault();
      const idMaquina = document.getElementById('hv_idMaquina').value.trim();
      const fecha = document.getElementById('hv_fecha').value || new Date().toISOString().split('T')[0];
      const tipo = document.getElementById('hv_tipo').value;
      const operacion = document.getElementById('hv_operacion').value.trim();
      const responsable = document.getElementById('hv_responsable').value.trim();
      const observ = document.getElementById('hv_observaciones').value.trim();
      const file = document.getElementById('hv_adjuntos').files[0];
      const entry = {
        id: 'HV' + (hojaVida.length + 1),
        idMaquina,
        fecha,
        tipo,
        operacion,
        responsable,
        observaciones: observ,
        adjuntos: []
      };
      if(file){
        const reader = new FileReader();
        reader.onload = function(ev){
          entry.adjuntos.push(ev.target.result);
          hojaVida.push(entry);
          saveHojaVida();
          renderHojasVida();
          closeHvModal();
          alert('Entrada agregada con adjunto.');
        };
        reader.readAsDataURL(file);
      } else {
        hojaVida.push(entry);
        saveHojaVida();
        renderHojasVida();
        closeHvModal();
        alert('Entrada agregada.');
      }
    });

    function renderHojasVida(){
      const cont = document.getElementById('hojasVidaList');
      const filter = (document.getElementById('hvFilter').value||'').toLowerCase();
      if(!cont) return;
      if(hojaVida.length === 0){
        cont.innerHTML = '<div class="no-results small">No hay entradas en Hojas de Vida.</div>';
        return;
      }
      // Agrupar por máquina
      const byMachine = {};
      hojaVida.forEach(h => {
        if(filter && !(String(h.idMaquina||'').toLowerCase().includes(filter) || String(h.tipo||'').toLowerCase().includes(filter))) return;
        if(!byMachine[h.idMaquina]) byMachine[h.idMaquina] = [];
        byMachine[h.idMaquina].push(h);
      });
      let html = '';
      for(const id in byMachine){
        html += `<div class="list-card"><div style="display:flex;justify-content:space-between;align-items:center"><strong>Máquina ${id}</strong>
                 <div><button class="btn btn-primary btn-small" onclick="openHvModal('${id}')"><i class="fas fa-plus"></i> Añadir entrada</button></div></div>`;
        byMachine[id].sort((a,b)=> new Date(b.fecha) - new Date(a.fecha));
        byMachine[id].forEach(h => {
          html += `<div style="margin-top:8px;border-top:1px solid #eef3f7;padding-top:8px">
                    <div><b>${formatearFecha(h.fecha)} · ${h.tipo} · ${h.operacion||''}</b></div>
                    <div class="small">Responsable: ${h.responsable||''}</div>
                    <div style="margin-top:6px">${h.observaciones||''}</div>`;
          if(h.adjuntos && h.adjuntos.length){
            h.adjuntos.forEach((a,i)=> {
              html += `<div style="margin-top:6px"><a href="${a}" target="_blank">Adjunto ${i+1}</a></div>`;
            });
          }
          html += `</div>`;
        });
        html += `</div>`;
      }
      cont.innerHTML = html || '<div class="small">No hay coincidencias con el filtro.</div>';
    }

    function verHojaVida(id){
      showPage('hojasvida');
      document.getElementById('hvFilter').value = id;
      renderHojasVida();
    }

    /**********************
       Preventivos (programaciones)
    **********************/
    function openProgramarPreventivo(){ if(!usuarioActivo || (usuarioActivo.rol!=='Administrador' && usuarioActivo.rol!=='Operario')){ alert('No tiene permisos.'); return; } document.getElementById('programarPreventivoForm').reset(); document.getElementById('programarPreventivoModal').style.display = 'flex'; }
    function closeProgramarPreventivo(){ document.getElementById('programarPreventivoModal').style.display = 'none'; }
    document.getElementById('programarPreventivoForm').addEventListener('submit', function(e){
      e.preventDefault();
      const id = 'P' + (preventivos.length + 1);
      const idMaquina = document.getElementById('p_idMaquina').value.trim();
      const operacion = document.getElementById('p_operacion').value.trim();
      const fechaProgramada = document.getElementById('p_fecha').value;
      const responsable = document.getElementById('p_responsable').value.trim();
      const observ = document.getElementById('p_observaciones').value.trim();
      preventivos.push({ id, idMaquina, operacion, fechaProgramada, responsable, observaciones: observ, estado: 'Programado' });
      savePreventivos();
      renderPreventivos();
      closeProgramarPreventivo();
      alert('Mantenimiento programado.');
    });

    function renderPreventivos(){
      loadPreventivos();
      const estadoFiltro = document.getElementById('filterPreventivoEstado').value;
      const idFiltro = (document.getElementById('filterPreventivoMaquina').value||'').trim();
      const list = preventivos.filter(p => ( !estadoFiltro || p.estado === estadoFiltro ) && ( !idFiltro || String(p.idMaquina).includes(idFiltro) ));
      const cont = document.getElementById('preventivosList');
      if(list.length === 0){ cont.innerHTML = '<div class="small">No hay mantenimientos programados.</div>'; return; }
      let html = '<table><thead><tr><th>ID</th><th>ID Máquina</th><th>Operación</th><th>Fecha</th><th>Responsable</th><th>Estado</th><th>Acciones</th></tr></thead><tbody>';
      list.forEach(p => {
        html += `<tr><td>${p.id}</td><td>${p.idMaquina}</td><td>${p.operacion}</td><td>${formatearFecha(p.fechaProgramada)}</td><td>${p.responsable||''}</td><td>${p.estado}</td>
                 <td>${ p.estado === 'Programado' ? `<button class="btn btn-success btn-small" onclick="marcarRealizado('${p.id}')">Marcar realizado</button>` : '' }
                 <button class="btn btn-primary btn-small" onclick="verPreventivoDetalle('${p.id}')">Ver</button></td></tr>`;
      });
      html += '</tbody></table>';
      cont.innerHTML = html;
    }

    function verPreventivoDetalle(id){
      const p = preventivos.find(x=>x.id===id);
      if(!p) return alert('No encontrado');
      const text = `ID: ${p.id}\nMáquina: ${p.idMaquina}\nOperación: ${p.operacion}\nFecha: ${p.fechaProgramada}\nResponsable: ${p.responsable}\nEstado: ${p.estado}\nObservaciones: ${p.observaciones}`;
      alert(text);
    }

    function marcarRealizado(id){
      if(!usuarioActivo || (usuarioActivo.rol!=='Administrador' && usuarioActivo.rol!=='Operario')){ alert('No tiene permisos.'); return; }
      if(!confirm('Marcar como realizado?')) return;
      const p = preventivos.find(x=>x.id===id);
      if(!p) return;
      p.estado = 'Realizado';
      // actualizar ultimoMantenimiento de la máquina y agregar a hoja de vida
      const m = maquinaria.find(x=>String(x.id) === String(p.idMaquina));
      const fechaHoy = new Date().toISOString().split('T')[0];
      if(m){
        m.ultimoMantenimiento = fechaHoy;
        saveMaquinaria();
      }
      hojaVida.push({
        id: 'HV' + (hojaVida.length + 1),
        idMaquina: p.idMaquina,
        fecha: fechaHoy,
        tipo: 'Preventivo',
        operacion: p.operacion,
        responsable: p.responsable,
        observaciones: p.observaciones,
        adjuntos: []
      });
      saveHojaVida();
      savePreventivos();
      renderPreventivos();
      renderizarTabla();
      alert('Mantenimiento marcado como realizado y registrado en Hoja de Vida.');
    }

    /**********************
       Observaciones globales
    **********************/
    function guardarObservaciones(){
      const val = document.getElementById('observacionesText').value.trim();
      const item = { id: 'O' + (observaciones.length+1), fecha: new Date().toISOString(), usuario: usuarioActivo ? usuarioActivo.nombre : 'Anónimo', texto: val };
      observaciones.unshift(item);
      saveObservaciones();
      renderObservacionesList();
      alert('Observación guardada.');
    }
    function cargarObservacionesText(){
      document.getElementById('observacionesText').value = '';
    }
    function renderObservacionesList(){
      const cont = document.getElementById('observacionesList');
      if(!observaciones || observaciones.length === 0){ cont.innerHTML = '<div class="small">No hay observaciones.</div>'; return; }
      cont.innerHTML = observaciones.map(o => `<div class="list-card"><div style="display:flex;justify-content:space-between"><strong>${o.usuario}</strong><small>${formatearFecha(o.fecha)}</small></div><div style="margin-top:6px">${o.texto}</div></div>`).join('');
    }
    function limpiarObservaciones(){
      if(!confirm('Limpiar todas las observaciones?')) return;
      observaciones = [];
      saveObservaciones();
      renderObservacionesList();
    }

    /**********************
       Seguimiento operario
    **********************/
    function loadDefaultOperariosIfEmpty(){
      loadOperarios();
      if(operarios.length === 0){
        operarios = [{ nombre: "Juan Pérez", maquinas: [] }, { nombre: "Ana Gómez", maquinas: [] }];
        saveOperarios();
      }
    }
    function renderSeguimiento(){
      loadOperarios();
      const cont = document.getElementById('tablaSeguimiento');
      if(!cont) return;
      let html = '<table><thead><tr><th>Operario</th><th>ID Máquina</th><th>Tipo/Modelo</th><th>Acciones</th></tr></thead><tbody>';
      operarios.forEach((op, i) => {
        if(op.maquinas.length === 0){
          html += `<tr><td>${op.nombre}</td><td colspan="3" class="small">Sin máquinas asignadas</td></tr>`;
        } else {
          op.maquinas.forEach(mid => {
            const maq = maquinaria.find(m=>String(m.id)===String(mid)) || {};
            html += `<tr><td>${op.nombre}</td><td>${mid}</td><td>${maq.tipo||''} ${maq.modelo||''}</td><td><button class="btn btn-warning btn-small" onclick="quitarAsignacion('${op.nombre}','${mid}')">Quitar</button></td></tr>`;
          });
        }
      });
      html += '</tbody></table>';
      cont.innerHTML = html;
    }
    function asignarMaquina(){
      const nombre = document.getElementById('nuevoOperario').value.trim();
      const id = document.getElementById('nuevoIdMaquina').value.trim();
      if(!nombre || !id) return alert('Complete los datos');
      let op = operarios.find(o=>o.nombre === nombre);
      if(!op){ operarios.push({ nombre, maquinas: [id] }); }
      else { if(!op.maquinas.includes(id)) op.maquinas.push(id); }
      saveOperarios();
      renderSeguimiento();
      alert('Asignación guardada.');
    }
    function quitarAsignacion(nombre, id){
      const op = operarios.find(o=>o.nombre===nombre);
      if(!op) return;
      op.maquinas = op.maquinas.filter(m=>String(m)!==String(id));
      if(op.maquinas.length === 0){ operarios = operarios.filter(o=>o.nombre !== nombre); }
      saveOperarios();
      renderSeguimiento();
    }

    /**********************
       Usuarios (login/register / rol)
    **********************/
    function openLoginModal(){ document.getElementById('loginModal').style.display='flex'; }
    function closeLoginModal(){ document.getElementById('loginModal').style.display='none'; document.getElementById('loginForm').reset(); }
    function openRegisterModal(){ document.getElementById('registerModal').style.display='flex'; }
    function closeRegisterModal(){ document.getElementById('registerModal').style.display='none'; document.getElementById('registerForm').reset(); }
    function openEditProfile(){ if(!usuarioActivo) return; document.getElementById('editProfileModal').style.display='flex'; document.getElementById('editNombre').value = usuarioActivo.nombre; document.getElementById('editCorreo').value = usuarioActivo.correo; document.getElementById('editTelefono').value = usuarioActivo.telefono; document.getElementById('editPassword').value = ''; document.getElementById('editRol').value = usuarioActivo.rol; }
    function closeEditProfile(){ document.getElementById('editProfileModal').style.display='none'; }

    document.getElementById('loginForm').addEventListener('submit', function(e){
      e.preventDefault();
      const correo = document.getElementById('loginCorreo').value.trim();
      const pass = document.getElementById('loginPassword').value;
      loadUsers();
      if(correo === PRIMARY_ADMIN_EMAIL && pass === PRIMARY_ADMIN_PASSWORD){
        ensurePrimaryAdminExists();
        const prim = usuarios.find(u => u.correo === PRIMARY_ADMIN_EMAIL || u.email === PRIMARY_ADMIN_EMAIL);
        if(prim){ usuarioActivo = prim; saveActive(); actualizarVistaUsuario(); closeLoginModal(); showPage('dashboard'); alert('Bienvenido Admin Principal'); return; }
      }
      const u = usuarios.find(x=> (x.correo === correo || x.email === correo) && x.password === pass );
      if(u){ usuarioActivo = u; saveActive(); actualizarVistaUsuario(); closeLoginModal(); showPage('dashboard'); alert('Bienvenido ' + u.nombre); }
      else alert('Usuario no encontrado o contraseña incorrecta.');
    });

    document.getElementById('registerForm').addEventListener('submit', function(e){
      e.preventDefault();
      const nombre = document.getElementById('regNombre').value.trim();
      const correo = document.getElementById('regCorreo').value.trim();
      const telefono = document.getElementById('regTelefono').value.trim();
      const password = document.getElementById('regPassword').value;
      let rol = document.getElementById('regRol').value;
      loadUsers();
      if(usuarios.find(x=>x.correo === correo || x.email === correo)){ alert('Ya existe usuario con ese correo'); return; }
      if(rol === 'Administrador'){ alert('No puede crear cuenta Administrador desde aquí. Solicite al administrador principal la promoción.'); rol = 'Consulta'; }
      const nuevo = { nombre, correo, telefono, password, rol, isPrimary: false };
      usuarios.push(nuevo);
      saveUsers();
      usuarioActivo = nuevo;
      saveActive();
      actualizarVistaUsuario();
      closeRegisterModal();
      showPage('dashboard');
      alert('Registro exitoso. Bienvenido ' + nombre);
    });

    document.getElementById('editProfileForm').addEventListener('submit', function(e){
      e.preventDefault();
      if(!usuarioActivo) return;
      const isEditorPrimary = isPrimaryAdmin(usuarioActivo);
      const newName = document.getElementById('editNombre').value.trim();
      const newCorreo = document.getElementById('editCorreo').value.trim();
      const newTelefono = document.getElementById('editTelefono').value.trim();
      const newPass = document.getElementById('editPassword').value;
      const newRol = document.getElementById('editRol').value;
      usuarioActivo.nombre = newName; usuarioActivo.correo = newCorreo; usuarioActivo.telefono = newTelefono;
      if(newPass) usuarioActivo.password = newPass;
      if(newRol === 'Administrador' && !isEditorPrimary){ alert('Solo el administrador principal puede asignar el rol Administrador.'); }
      else if(newRol === 'Administrador' && isEditorPrimary){
        if(countAdmins() >= MAX_ADMINS && usuarioActivo.rol !== 'Administrador'){ alert('Ya se alcanzó el máximo de administradores ('+MAX_ADMINS+').'); }
        else usuarioActivo.rol = newRol;
      } else usuarioActivo.rol = newRol;
      const idx = usuarios.findIndex(u=>u.correo === usuarioActivo.correo || u.email === usuarioActivo.correo);
      if(idx !== -1) usersSafeReplace(idx, usuarioActivo); else usuarios.push(usuarioActivo);
      saveUsers(); saveActive(); closeEditProfile(); actualizarVistaUsuario(); renderUsuarios();
    });

    function usersSafeReplace(idx, userObj){
      usuarios[idx] = userObj;
    }

    function logout(){ usuarioActivo = null; saveActive(); actualizarVistaUsuario(); alert('Sesión cerrada'); showPage('dashboard'); }

    function renderUsuarios(){
      const tbody = document.getElementById('tablaUsuarios');
      const warning = document.getElementById('warningUsuarios');
      loadUsers();
      if(!usuarioActivo || usuarioActivo.rol !== 'Administrador'){ warning.style.display = 'block'; warning.textContent = 'No tiene permisos para ver o editar usuarios.'; tbody.innerHTML = ''; return; }
      warning.style.display = 'none';
      tbody.innerHTML = usuarios.map((u,i)=>`
        <tr>
          <td>${u.nombre}</td>
          <td>${u.correo || u.email || ''}</td>
          <td>${u.telefono||''}</td>
          <td>${u.rol||''}</td>
          <td>${u.isPrimary? 'Sí' : ''}</td>
          <td>
            <button class="btn btn-primary btn-small" onclick="editarUsuario(${i})"><i class="fas fa-edit"></i></button>
            ${ renderAdminControls(i) }
            <button class="btn btn-warning btn-small" onclick="eliminarUsuario(${i})"><i class="fas fa-trash"></i></button>
          </td>
        </tr>
      `).join('');
    }

    function renderAdminControls(index){
      if(!isPrimaryAdmin(usuarioActivo)) return '';
      const target = usuarios[index];
      if(!target) return '';
      if(target.isPrimary) return '';
      if(target.rol === 'Administrador'){
        return `<button class="btn btn-ghost btn-small" onclick="demoteAdmin(${index})" title="Quitar rol Administrador">Quitar Admin</button>`;
      } else {
        if(countAdmins() >= MAX_ADMINS){
          return `<button class="btn btn-ghost btn-small" disabled title="Límite de admins alcanzado (${MAX_ADMINS})">Promover (límite)</button>`;
        }
        return `<button class="btn btn-success btn-small" onclick="promoteAdmin(${index})" title="Promover a Administrador">Promover Admin</button>`;
      }
    }

    function editarUsuario(i){
      const u = usuarios[i];
      if(!usuarioActivo) return;
      document.getElementById('editNombre').value = u.nombre;
      document.getElementById('editCorreo').value = u.correo || u.email || '';
      document.getElementById('editTelefono').value = u.telefono || '';
      document.getElementById('editPassword').value = '';
      document.getElementById('editRol').value = u.rol || 'Consulta';
      document.getElementById('editProfileModal').style.display = 'flex';
      document.getElementById('editProfileForm').onsubmit = function(ev){
        ev.preventDefault();
        const newName = document.getElementById('editNombre').value.trim();
        const newCorreo = document.getElementById('editCorreo').value.trim();
        const newTelefono = document.getElementById('editTelefono').value.trim();
        const newPass = document.getElementById('editPassword').value;
        const newRol = document.getElementById('editRol').value;
        if(newRol === 'Administrador' && !isPrimaryAdmin(usuarioActivo)){ alert('Solo el administrador principal puede asignar el rol Administrador.'); return; }
        if(newRol === 'Administrador' && isPrimaryAdmin(usuarioActivo) && usuarios[i].rol !== 'Administrador'){ if(countAdmins() >= MAX_ADMINS){ alert('No se puede promover: límite alcanzado.'); return; } }
        usuarios[i].nombre = newName; usuarios[i].correo = newCorreo; usuarios[i].email = newCorreo; if(newPass) usuarios[i].password = newPass;
        if(!usuarios[i].isPrimary) usuarios[i].rol = newRol;
        saveUsers();
        document.getElementById('editProfileModal').style.display = 'none';
        renderUsuarios();
      };
    }

    function eliminarUsuario(i){
      if(usuarios[i].isPrimary){ alert('No se puede eliminar al administrador principal desde aquí.'); return; }
      if(!confirm('Eliminar usuario?')) return;
      usuarios.splice(i,1);
      saveUsers();
      renderUsuarios();
    }

    function promoteAdmin(index){
      if(!isPrimaryAdmin(usuarioActivo)){ alert('Solo el administrador principal puede promover.'); return; }
      if(countAdmins() >= MAX_ADMINS){ alert('Límite de administradores alcanzado.'); return; }
      usuarios[index].rol = 'Administrador';
      usuarios[index].isPrimary = false;
      saveUsers();
      renderUsuarios();
      alert('Usuario promovido a Administrador.');
    }
    function demoteAdmin(index){
      if(!isPrimaryAdmin(usuarioActivo)){ alert('Solo el administrador principal puede despromover.'); return; }
      if(usuarios[index].isPrimary){ alert('No puede despromover al administrador principal.'); return; }
      usuarios[index].rol = 'Consulta';
      usuarios[index].isPrimary = false;
      saveUsers();
      renderUsuarios();
      alert('Rol Administrador removido.');
    }

    /**********************
       Navegación y vista
    **********************/
    function showPage(id,e){
      if(e) e.preventDefault();
      document.querySelectorAll('nav a').forEach(a=>a.classList.remove('active'));
      const link = Array.from(document.querySelectorAll('nav a')).find(a=> (a.getAttribute('onclick')||'').includes("'" + id + "'") );
      if(link) link.classList.add('active');
      document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
      const page = document.getElementById(id);
      if(page) page.classList.add('active');
      if(id==='inventario') filtrarDatos();
      if(id==='usuarios') renderUsuarios();
      if(id==='preventivo') renderPreventivos();
      if(id==='hojasvida') renderHojasVida();
      if(id==='observaciones') { renderObservacionesList(); cargarObservacionesText(); }
      if(id==='seguimiento') renderSeguimiento();
      if(id==='reportes') renderReportes();
    }

    function actualizarVistaUsuario(){
      loadActive();
      const info = document.getElementById('userInfo');
      const btnLogin = document.getElementById('btnLogin');
      const btnRegister = document.getElementById('btnRegister');
      const btnLogout = document.getElementById('btnLogout');
      const btnEditProfile = document.getElementById('btnEditProfile');
      if(usuarioActivo){
        info.textContent = usuarioActivo.nombre + ' ('+usuarioActivo.rol+')' + (usuarioActivo.isPrimary ? ' • Admin Principal' : '');
        btnLogin.style.display='none'; btnRegister.style.display='none'; btnLogout.style.display=''; btnEditProfile.style.display='';
        const canEdit = (usuarioActivo.rol==='Administrador' || usuarioActivo.rol==='Operario');
        document.getElementById('btnAddMaquina').style.display = canEdit? 'inline-block' : 'none';
        document.getElementById('btnExportExcel').style.display = canEdit? 'inline-block' : 'none';
        document.getElementById('btnExportPDF').style.display = canEdit? 'inline-block' : 'none';
        document.getElementById('btnImportJSON').style.display = canEdit? 'inline-block' : 'none';
        document.getElementById('btnDownloadJSON').style.display = 'inline-block';
      } else {
        info.textContent = 'Vista previa - No autenticado';
        btnLogin.style.display=''; btnRegister.style.display=''; btnLogout.style.display='none'; btnEditProfile.style.display='none';
        document.getElementById('btnAddMaquina').style.display = 'none';
        document.getElementById('btnExportExcel').style.display = 'none';
        document.getElementById('btnExportPDF').style.display = 'none';
        document.getElementById('btnImportJSON').style.display = 'none';
        document.getElementById('btnDownloadJSON').style.display = 'none';
      }
    }

    /**********************
       Reportes
    **********************/
    function renderReportes(){
      const tbody = document.getElementById('tablaReporteInventario');
      tbody.innerHTML = maquinaria.map(m=>{
        const info = calcularEstado(m);
        return `<tr><td>${m.id}</td><td>${m.tipo}</td><td>${m.marca}</td><td>${m.modelo}</td><td>${m.placa}</td><td>${formatearFecha(m.ultimoMantenimiento)}</td><td>${formatearFecha(info.proximoMantenimiento)}</td><td>${info.estado}</td></tr>`;
      }).join('');
    }
    function exportPreventivos(tipo){
      if(!usuarioActivo || (usuarioActivo.rol!=='Administrador' && usuarioActivo.rol!=='Operario')){ alert('No tiene permisos para exportar.'); return; }
      const rows = preventivos.map(p=>({
        ID: p.id, ID_MAQUINA: p.idMaquina, OPERACION: p.operacion, FECHA_PROGRAMADA: p.fechaProgramada, RESPONSABLE: p.responsable, OBSERVACIONES: p.observaciones, ESTADO: p.estado
      }));
      if(tipo==='excel'){ const ws = XLSX.utils.json_to_sheet(rows); const wb = XLSX.utils.book_new(); XLSX.utils.book_append_sheet(wb, ws, 'Preventivos'); XLSX.writeFile(wb, 'Preventivos.xlsx'); }
      else if(tipo==='pdf'){ const { jsPDF } = window.jspdf; const doc = new jsPDF('landscape'); doc.setFontSize(10); let y=12; doc.text('Mantenimientos Preventivos',12,10); rows.forEach((r,i)=>{ doc.text(`${i+1}. ID:${r.ID} | Máquina:${r.ID_MAQUINA} | Fecha:${r.FECHA_PROGRAMADA} | Estado:${r.ESTADO}`,12,y); y+=6; if(y>275){doc.addPage(); y=12;} }); doc.save('Preventivos.pdf'); }
    }

    /**********************
       Inicio / carga
    **********************/
    document.addEventListener('DOMContentLoaded', function(){
      loadUsers(); ensurePrimaryAdminExists(); loadActive();
      loadMaquinaria(); loadPreventivos(); loadHojaVida(); loadObservaciones(); loadOperarios(); loadDefaultOperariosIfEmpty();
      actualizarVistaUsuario(); renderizarTabla(); renderReportes(); renderPreventivos(); renderHojasVida(); renderObservacionesList(); renderSeguimiento();
    });

    /**********************
       util: cerrar modales al click fuera
    **********************/
    window.onclick = function(ev){
      ['maquinaModal','loginModal','registerModal','editProfileModal','importModal','programarPreventivoModal','hvModal'].forEach(id=>{
        const el = document.getElementById(id);
        if(el && ev.target === el) el.style.display = 'none';
      });
    };
  </script>
</body>
</html>
