<script>
  import Icon from 'fa-svelte'
  import { faArrowAltCircleLeft } from '@fortawesome/free-solid-svg-icons/faArrowAltCircleLeft';
  import { faArrowAltCircleRight } from '@fortawesome/free-solid-svg-icons/faArrowAltCircleRight';
  import Producto from "./lib/Producto.svelte";
  
  let icon = faArrowAltCircleLeft;
  let icon2 = faArrowAltCircleRight;
  let catalogo = [];
  let totalProductos = 0;
  let cantidadProductos = 20;
  let datoIniCarga = 0;
  let datoFinCarga = 0;
  let pagina = 1;
  let totalPaginas = 0;
  const infoRecibida = async () => {
    const respuesta = await fetch(
      `https://rickandmortyapi.com/api/character?page=${pagina}`
    );
    const info = await respuesta.json();
    totalProductos = info.info.count;
    totalPaginas = Math.ceil(totalProductos / cantidadProductos);
    catalogo = info.results;
    datoIniCarga = catalogo[0].id;
    datoFinCarga = catalogo[catalogo.length - 1].id;
  };
  infoRecibida();
  const paginaSiguiente = () => {
    pagina++;
    if (pagina <= totalPaginas) {
      infoRecibida();
    } else {
      pagina = totalPaginas;
    }
  };
  const paginaAnterior = () => {
    pagina--;
    if (pagina >= 1) {
      infoRecibida();
    } else {
      document.getElementById('anterior').disabled = true;
      pagina = 1;
    }
  };
</script>
<div>
  <Icon icon={icon}>
  </Icon></div>
<h1 class="title">Tu catalogo</h1>
<div class="container">
  <div class="paginador">
    <div id="anterior"
      on:click={() => {
        paginaAnterior();
      }}
    disabled>
    <Icon icon={icon}>
    </Icon>
      Previos
    </div>
    <div>{datoIniCarga}-{datoFinCarga} de {totalProductos}</div>
    <div id="siguiente"
      on:click={() => {
        paginaSiguiente();
      }}
      disabled={pagina == totalPaginas}
    >
      Siguientes
      <Icon icon={icon2}>
      </Icon>
    </div></div>
</div>
<div class="container">
  <div class="grid">
    {#each catalogo as producto}
      <Producto {producto} />
    {/each}
  </div>
</div>
