# proyecto1
prueba
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


const productos = [];

formulario.addEventListener("submit", (e) => {
  e.preventDefault();
  let productoNuevo = {
    nombre: nombreProducto.value,
    marca: marcaProducto.value,
    precio: parseInt(precioProducto.value),
    categoria: categoria.value,
    disponible: disponible.value === "true",
  }

  productos.push(productoNuevo);

  pintarProductos(productoNuevo)

  guardarProductos();

});

function pintarProductos(productoNuevo) {
  const divProducto = document.createElement("div");
  const infoProducto = document.createElement("ul");

  const tipoEstado = productoNuevo.disponible ? "Disponible" : "No disponible";

  infoProducto.innerHTML = `
  <li><b>Nombre de producto:</b> ${productoNuevo.nombre}</li>
  <li>Marca de producto: ${productoNuevo.marca}</li>
  <li>Precio: ${productoNuevo.precio} € </li>
  <li>Categoría: ${productoNuevo.categoria}</li>
  <li>Disponible: ${tipoEstado}</li>
  `;

  divProducto.appendChild(infoProducto);
  listaProductos.append(divProducto);
};

function guardarProductos() {
localStorage.setItem("productos", JSON.stringify(productos));
};
