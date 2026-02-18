<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Sistema de Gestión de Óptica</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: Arial, sans-serif; }
    body { background-color: #f4f6f9; color: #333; }
    header { background-color: #2c3e50; color: white; padding: 15px; text-align: center; }

    nav { background-color: #34495e; display: flex; justify-content: space-between; padding: 10px 20px; }
    nav button { background-color: #1abc9c; border: none; padding: 10px 15px; color: white; cursor: pointer; border-radius: 6px; }

    .container { padding: 20px; max-width: 1000px; margin: auto; }

    .card { background: white; padding: 20px; border-radius: 10px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); margin-bottom: 20px; display: none; }
    .card.active { display: block; }

    h2 { margin-bottom: 15px; color: #2c3e50; }

    input { width: 100%; padding: 10px; margin: 8px 0 15px 0; border: 1px solid #ccc; border-radius: 6px; }

    button.primary { background-color: #3498db; border: none; padding: 10px 15px; color: white; border-radius: 6px; cursor: pointer; }

    table { width: 100%; border-collapse: collapse; margin-top: 15px; }
    table, th, td { border: 1px solid #ddd; }
    th, td { padding: 10px; text-align: center; }
    th { background-color: #2c3e50; color: white; }
  </style>
</head>
<body>

  <!-- LOGIN / REGISTRO -->
  <div id="menuInicio" class="container" style="margin-top:40px;">

    <div id="login" class="card active" style="display:block; text-align:center;">
      <img src="https://images.unsplash.com/photo-1579684385127-1ef15d508118" alt="Lentes" style="width:120px; margin-bottom:15px;">
      <h2>Iniciar sesión</h2>
      <input type="text" id="loginUsuario" placeholder="Usuario" />
      <input type="password" id="loginPassword" placeholder="Contraseña" />
      <button class="primary" onclick="iniciarSesion()">Entrar</button>
      <p style="margin-top:10px;">¿No tienes cuenta? <a href="#" onclick="mostrarRegistro()">Regístrate aquí</a></p>
    </div>

    <div id="registro" class="card" style="text-align:center;">
      <img src="https://images.unsplash.com/photo-1582719478250-c89cae4dc85b" alt="Registro" style="width:120px; margin-bottom:15px;">
      <h2>Registro de usuario</h2>
      <input type="text" id="regUsuario" placeholder="Nuevo usuario" />
      <input type="password" id="regPassword" placeholder="Nueva contraseña" />
      <button class="primary" onclick="registrarUsuario()">Crear cuenta</button>
      <p style="margin-top:10px;">¿Ya tienes cuenta? <a href="#" onclick="mostrarLogin()">Inicia sesión</a></p>
    </div>

  </div>

  <!-- SISTEMA -->
  <div id="sistema" style="display:none;">

    <header>
      <h1>Sistemas de Gestión "IVISION"</h1>
      <img src="https://www.facebook.com/photo/?fbid=122719944083596&set=a.122720007416923" alt="Óptica" style="width:100%; max-height:200px; object-fit:cover; margin-top:10px; border-radius:8px;" />
    </header>

    <nav>
      <div>
        <button onclick="mostrarSeccion('expedientes')">Expedientes</button>
        <button onclick="mostrarSeccion('ventas')">Ventas</button>
        <button onclick="mostrarSeccion('reportes')">Reportes</button>
      </div>
      <button onclick="cerrarSesion()" style="background-color:#e74c3c;">Cerrar sesión</button>
    </nav>

    <div class="container">

      <div id="expedientes" class="card active">
        <h2>Gestión de Expedientes</h2>
        <input type="text" id="nombrePaciente" placeholder="Nombre del paciente" />
        <input type="text" id="telefonoPaciente" placeholder="Teléfono" />
        <input type="text" id="diagnosticoPaciente" placeholder="Diagnóstico" />
        <button class="primary" onclick="agregarExpediente()">Guardar expediente</button>
        <table>
          <thead><tr><th>Nombre</th><th>Teléfono</th><th>Diagnóstico</th></tr></thead>
          <tbody id="tablaExpedientes"></tbody>
        </table>
      </div>

      <div id="ventas" class="card">
        <h2>Ventas de Lentes</h2>
        <input type="text" id="clienteVenta" placeholder="Cliente" />
        <input type="text" id="productoVenta" placeholder="Tipo de lente" />
        <input type="number" id="precioVenta" placeholder="Precio" />
        <button class="primary" onclick="registrarVenta()">Registrar venta</button>
        <table>
          <thead><tr><th>Cliente</th><th>Producto</th><th>Precio</th></tr></thead>
          <tbody id="tablaVentas"></tbody>
        </table>
      </div>

      <div id="reportes" class="card">
        <h2>Reportes</h2>
        <p>Total de ventas: <strong id="totalVentas">$0</strong></p>
      </div>

    </div>
  </div>

  <script>
    const expedientes = [];
    const ventas = [];

    function mostrarRegistro() {
      document.getElementById('login').classList.remove('active');
      document.getElementById('registro').classList.add('active');
    }

    function mostrarLogin() {
      document.getElementById('registro').classList.remove('active');
      document.getElementById('login').classList.add('active');
    }

    function registrarUsuario() {
      const user = document.getElementById('regUsuario').value;
      const pass = document.getElementById('regPassword').value;

      if (!user || !pass) return alert('Completa los datos');

      localStorage.setItem('usuario', user);
      localStorage.setItem('password', pass);
      alert('Usuario registrado correctamente');
      mostrarLogin();
    }

    function iniciarSesion() {
      const user = document.getElementById('loginUsuario').value;
      const pass = document.getElementById('loginPassword').value;

      const userGuardado = localStorage.getItem('usuario');
      const passGuardado = localStorage.getItem('password');

      if (user === userGuardado && pass === passGuardado) {
        document.getElementById('menuInicio').style.display = 'none';
        document.getElementById('sistema').style.display = 'block';
      } else {
        alert('Usuario o contraseña incorrectos');
      }
    }

    function cerrarSesion() {
      document.getElementById('sistema').style.display = 'none';
      document.getElementById('menuInicio').style.display = 'block';
    }

    function mostrarSeccion(id) {
      document.querySelectorAll('#sistema .card').forEach(c => c.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    function agregarExpediente() {
      const nombre = nombrePaciente.value;
      const telefono = telefonoPaciente.value;
      const diagnostico = diagnosticoPaciente.value;
      if (!nombre || !telefono || !diagnostico) return alert('Completa todos los campos');
      expedientes.push({ nombre, telefono, diagnostico });
      renderExpedientes();
      nombrePaciente.value = telefonoPaciente.value = diagnosticoPaciente.value = '';
    }

    function renderExpedientes() {
      tablaExpedientes.innerHTML = '';
      expedientes.forEach(e => {
        tablaExpedientes.innerHTML += `<tr><td>${e.nombre}</td><td>${e.telefono}</td><td>${e.diagnostico}</td></tr>`;
      });
    }

    function registrarVenta() {
      const cliente = clienteVenta.value;
      const producto = productoVenta.value;
      const precio = parseFloat(precioVenta.value);
      if (!cliente || !producto || !precio) return alert('Completa todos los campos');
      ventas.push({ cliente, producto, precio });
      renderVentas(); actualizarTotal();
      clienteVenta.value = productoVenta.value = precioVenta.value = '';
    }

    function renderVentas() {
      tablaVentas.innerHTML = '';
      ventas.forEach(v => {
        tablaVentas.innerHTML += `<tr><td>${v.cliente}</td><td>${v.producto}</td><td>$${v.precio.toFixed(2)}</td></tr>`;
      });
    }

    function actualizarTotal() {
      const total = ventas.reduce((s, v) => s + v.precio, 0);
      totalVentas.textContent = '$' + total.toFixed(2);
    }
  </script>

</body>
</html>
