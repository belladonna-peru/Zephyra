// script.js mejorado con persistencia por sesión y sistema de puntos/pedidos

document.addEventListener("DOMContentLoaded", function () {
  // -------------------- CERRAR MODAL LOGIN/REGISTRO --------------------
  const cerrarModal = document.getElementById("cerrarModal");
  if (cerrarModal) {
    cerrarModal.addEventListener("click", function () {
      const modal = cerrarModal.closest(".modal");
      if (modal) modal.style.display = "none";
    });
  }
  function mostrarDatosGuardados() {
  const usuarioLogueado = JSON.parse(localStorage.getItem('usuarioLogueado'));
  formLogin.addEventListener('submit', function (e) {
  e.preventDefault();

  const email = document.getElementById('email-login').value;
  const password = document.getElementById('password-login').value;

  const usuarios = JSON.parse(localStorage.getItem('usuarios')) || [];
  const usuario = usuarios.find(user => user.email === email && user.password === password);

  if (usuario) {
    localStorage.setItem('usuarioLogueado', JSON.stringify(usuario));
    alert('¡Inicio de sesión exitoso!');
    window.location.href = 'panel.html';
  } else {
    alert('Correo o contraseña incorrectos');
  }
});


  // Verifica si existe un usuario logueado y tiene estructura válida
  if (!usuarioLogueado || !usuarioLogueado.nombre) {
    console.warn('No hay un usuario logueado o los datos son inválidos.');
    return;
  }

  // Mostrar los datos del perfil
  document.getElementById('nombrePerfil').value = usuarioLogueado.nombre || '';
  document.getElementById('emailPerfil').value = usuarioLogueado.email || '';
  document.getElementById('telefonoPerfil').value = usuarioLogueado.telefono || '';

  // Métodos de pago
  document.getElementById('numeroYape').value = usuarioLogueado.numeroYape || '';
  document.getElementById('numeroPlin').value = usuarioLogueado.numeroPlin || '';
  document.getElementById('tarjetaCredito').value = usuarioLogueado.tarjetaCredito || '';
  document.getElementById('tarjetaDebito').value = usuarioLogueado.tarjetaDebito || '';

  // Direcciones
  document.getElementById('direccion1').value = usuarioLogueado.direccion1 || '';
  document.getElementById('direccion2').value = usuarioLogueado.direccion2 || '';
  document.getElementById('referencia').value = usuarioLogueado.referencia || '';

  // Puntos y pedidos
  document.getElementById('puntosUsuario').textContent = usuarioLogueado.puntos || 0;
  mostrarPedidos(usuarioLogueado.pedidos || []);
}


  // -------------------- REGISTRO --------------------
  const formRegistro = document.getElementById("form-registro");
  if (formRegistro) {
    formRegistro.addEventListener("submit", function (e) {
      e.preventDefault();
      const nombre = document.getElementById("nombre-registro").value.trim();
      const email = document.getElementById("email-registro").value.trim();
      const password = document.getElementById("password-registro").value;

      if (!nombre || !email || !password) {
        alert("Por favor, completa todos los campos.");
        return;
      }

      let usuarios = JSON.parse(localStorage.getItem("usuarios")) || [];
      const existe = usuarios.find(user => user.email === email);
      if (existe) {
        alert("Este correo ya está registrado.");
        return;
      }

      const nuevoUsuario = {
        nombre,
        email,
        password,
        perfil: {},
        direccion: {},
        metodoPago: '',
        puntos: 0,
        pedidos: []
      };

      usuarios.push(nuevoUsuario);
      localStorage.setItem("usuarios", JSON.stringify(usuarios));

      alert("Registro exitoso. ¡Ahora puedes iniciar sesión!");
      window.location.href = "login.html";
    });
  }

  // -------------------- LOGIN --------------------
  const formLogin = document.getElementById("form-login");
  if (formLogin) {
    formLogin.addEventListener("submit", function (e) {
      e.preventDefault();
      const email = document.getElementById("email-login").value.trim();
      const password = document.getElementById("password-login").value;

      let usuarios = JSON.parse(localStorage.getItem("usuarios")) || [];
      const usuario = usuarios.find(user => user.email === email && user.password === password);

      if (!usuario) {
        alert("Correo o contraseña incorrectos.");
        return;
      }

      // Validación de sesión: guardar token de sesión (simulado)
      sessionStorage.setItem("sesionIniciada", "true");

      localStorage.setItem("usuarioActivo", JSON.stringify(usuario));
      alert("Bienvenida " + usuario.nombre);
      window.location.href = "panel.html";
    });
  }

  // -------------------- MOSTRAR DATOS GUARDADOS --------------------
  function mostrarDatosGuardados() {
    const usuario = JSON.parse(localStorage.getItem("usuarioActivo"));
    if (!usuario) return;

    const { perfil, direccion, metodoPago } = usuario;

    const map = [
      ["datoNombre", perfil.nombre],
      ["datoApellido", perfil.apellido],
      ["datoDNI", perfil.dni],
      ["datoTelefono", perfil.telefono],
      ["datoCorreo", perfil.correo],
      ["datoDep", direccion.dep],
      ["datoProv", direccion.prov],
      ["datoDist", direccion.dist],
      ["datoDireccion", direccion.direccionExacta],
      ["datoReferencia", direccion.referencia],
      ["datoMetodoPago", metodoPago || 'No seleccionado'],
    ];

    map.forEach(([id, val]) => {
      const el = document.getElementById(id);
      if (el) el.textContent = val || '';
    });

    const puntosEl = document.getElementById("puntosUsuario");
    if (puntosEl) puntosEl.textContent = usuario.puntos || 0;
  }

  mostrarDatosGuardados();

  // -------------------- PANEL USUARIO --------------------
  const panel = document.getElementById("panel-usuario");
  if (panel) {
    const usuario = JSON.parse(localStorage.getItem("usuarioActivo"));
    if (!usuario || sessionStorage.getItem("sesionIniciada") !== "true") {
      alert("Debes iniciar sesión primero.");
      window.location.href = "login.html";
    } else {
      panel.innerHTML = `
        <h2>Hola, ${usuario.nombre}</h2>
        <p>Correo: ${usuario.email}</p>
        <p>Puntos: <span id="puntosPanel">${usuario.puntos}</span></p>
        <button id="cerrar-sesion">Cerrar Sesión</button>
        <button id="canjear-puntos">Canjear Puntos</button>
      `;
      document.getElementById("cerrar-sesion").addEventListener("click", () => {
        localStorage.removeItem("usuarioActivo");
        sessionStorage.removeItem("sesionIniciada");
        alert("Sesión cerrada correctamente.");
        window.location.href = "index.html";
      });

      document.getElementById("canjear-puntos").addEventListener("click", () => {
        if (usuario.puntos >= 100) {
          usuario.puntos -= 100;
          alert("¡Has canjeado 100 puntos!");
          actualizarUsuarioEnStorage(usuario);
          document.getElementById("puntosPanel").textContent = usuario.puntos;
          mostrarDatosGuardados();
        } else {
          alert("Necesitas al menos 100 puntos para canjear.");
        }
      });
    }
  }

  // -------------------- FORMULARIOS --------------------
  const usuario = JSON.parse(localStorage.getItem("usuarioActivo"));
  const usuarios = JSON.parse(localStorage.getItem("usuarios")) || [];

  function actualizarUsuarioEnStorage(usuarioActualizado) {
    const index = usuarios.findIndex(u => u.email === usuarioActualizado.email);
    if (index !== -1) {
      usuarios[index] = usuarioActualizado;
      localStorage.setItem("usuarios", JSON.stringify(usuarios));
      localStorage.setItem("usuarioActivo", JSON.stringify(usuarioActualizado));
    }
  }

  // PERFIL
  const formPerfil = document.getElementById("formPerfil");
  if (formPerfil && usuario) {
    const campos = ["perfilNombre", "perfilApellido", "perfilDNI", "perfilTelefono", "perfilCorreo"];
    campos.forEach(id => {
      const el = document.getElementById(id);
      if (el && usuario.perfil) el.value = usuario.perfil[id.replace("perfil", "").toLowerCase()] || '';
    });

    formPerfil.addEventListener("submit", e => {
      e.preventDefault();
      campos.forEach(id => {
        usuario.perfil[id.replace("perfil", "").toLowerCase()] = document.getElementById(id).value;
      });
      actualizarUsuarioEnStorage(usuario);
      alert("Perfil guardado correctamente");
      mostrarDatosGuardados();
    });
  }

  // DIRECCIÓN
  const formDireccion = document.getElementById("formDireccion");
  if (formDireccion && usuario) {
    const campos = ["dep", "prov", "dist", "direccionExacta", "referencia"];
    campos.forEach(id => {
      const el = document.getElementById(id);
      if (el && usuario.direccion) el.value = usuario.direccion[id] || '';
    });

    formDireccion.addEventListener("submit", e => {
      e.preventDefault();
      campos.forEach(id => {
        usuario.direccion[id] = document.getElementById(id).value;
      });
      actualizarUsuarioEnStorage(usuario);
      alert("Dirección guardada correctamente");
      mostrarDatosGuardados();
    });
  }

  // MÉTODO DE PAGO
  const formPago = document.getElementById("formPago");
  if (formPago && usuario) {
    if (usuario.metodoPago) {
      const radio = formPago.querySelector(`input[value="${usuario.metodoPago}"]`);
      if (radio) radio.checked = true;
    }
    formPago.addEventListener("submit", e => {
      e.preventDefault();
      const metodo = formPago.metodoPago.value;
      usuario.metodoPago = metodo;
      actualizarUsuarioEnStorage(usuario);
      alert("Método de pago guardado");
      mostrarDatosGuardados();
    });
  }

  // PEDIDOS
  const pedidosContainer = document.getElementById("pedidosContainer");
  if (usuario && usuario.pedidos && pedidosContainer) {
    pedidosContainer.innerHTML = "";
    usuario.pedidos.forEach(pedido => {
      const div = document.createElement("div");
      div.classList.add("pedido");
      div.innerHTML = `
        <p>Pedido #${pedido.id}</p>
        <span class="estado ${pedido.estado === 'En camino' ? 'en-camino' : 'pendiente'}">${pedido.estado}</span>
      `;
      if (pedido.estado === "Pendiente") {
        const btn = document.createElement("button");
        btn.classList.add("btn-pagar");
        btn.textContent = "Pagar";
        btn.addEventListener("click", () => {
          alert(`Redirigiendo al pago de pedido #${pedido.id}...`);
        });
        div.appendChild(btn);
      }
      pedidosContainer.appendChild(div);
    });
  }
});
