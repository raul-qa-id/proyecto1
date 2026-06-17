# proyecto1
prueba

\```javascript

<!doctype html>
<html lang="es">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Catálogo de productos</title>
</head>

<body>
  <form id="formulario">
    <input type="text" id="nombreProducto" value="ProductoRandom">
    <input type="text" id="marcaProducto" value="MarcaRandom">
    <input type="number" id="precioProducto" value="33" step="0.01">
    <select id="categoria">
      <option value="informatica">Informática</option>
      <option value="alimentacion">Alimentación</option>
      <option value="hogar">Hogar</option>
      <option value="electronica">Electrónica</option>
      <option value="Otros">Otros</option>
    </select>
    <select id="disponible">
      <option value="true">Disponible</option>
      <option value="false">No disponible</option>
    </select>

    <button type="submit">Agregar productos</button>
  </form>

  <div id="listaProductos"></div>

  <script type="module" src="/src/main.js"></script>
</body>

</html>



import './style.css'

const formulario = document.getElementById("formulario");
const nombreProducto = document.getElementById("nombreProducto");
const marcaProducto = document.getElementById("marcaProducto");
const precioProducto = document.getElementById("precioProducto");
const categoria = document.getElementById("categoria");
const disponible = document.getElementById("disponible");
const listaProductos = document.getElementById("listaProductos");

let productos = JSON.parse(localStorage.getItem("productos")) || [];

formulario.addEventListener("submit", (e) => {
  e.preventDefault();

  const yaExiste = productos.some(p => p.nombre.toLowerCase() === nombreProducto.value.toLowerCase());
  if (yaExiste) {
    alert("Ya existe un producto con ese nombre");
    return;
  }

  let productoNuevo = {
    id: crypto.randomUUID(),
    nombre: nombreProducto.value,
    marca: marcaProducto.value,
    precio: parseInt(precioProducto.value),
    categoria: categoria.value,
    disponible: disponible.value === "true",
  }

  productos.push(productoNuevo);
  guardarProductos();
  renderizarProductos();
});

function pintarProducto(producto) {
  const divProducto = document.createElement("div");
  divProducto.dataset.id = producto.id;
  const infoProducto = document.createElement("ul");

  const tipoEstado = producto.disponible ? "Disponible" : "No disponible";

  infoProducto.innerHTML = `
  <li><b>Nombre de producto:</b> ${producto.nombre}</li>
  <li>Marca de producto: ${producto.marca}</li>
  <li>Precio: ${producto.precio} €</li>
  <li>Categoría: ${producto.categoria}</li>
  <li>Estado: ${tipoEstado}</li>
  `;

  const btnEliminar = document.createElement("button");
  btnEliminar.textContent = "Eliminar";
  btnEliminar.addEventListener("click", () => eliminarProducto(producto.id));

  const btnEstado = document.createElement("button");
  btnEstado.textContent = producto.disponible ? "Marcar no disponible" : "Marcar disponible";
  btnEstado.addEventListener("click", () => toggleEstado(producto.id));

  divProducto.appendChild(infoProducto);
  divProducto.appendChild(btnEliminar);
  divProducto.appendChild(btnEstado);
  listaProductos.append(divProducto);
}

function renderizarProductos() {
  listaProductos.innerHTML = "";
  productos.forEach(p => pintarProducto(p));
}

function eliminarProducto(id) {
  productos = productos.filter(p => p.id !== id);
  guardarProductos();
  renderizarProductos();
}

function toggleEstado(id) {
  const producto = productos.find(p => p.id === id);
  if (producto) {
    producto.disponible = !producto.disponible;
    guardarProductos();
    renderizarProductos();
  }
}

function guardarProductos() {
  localStorage.setItem("productos", JSON.stringify(productos));
}

renderizarProductos();
\```
