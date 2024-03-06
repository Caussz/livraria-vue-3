<script setup>
import { ref, onMounted } from "vue";
import CategoriasApi from "@/api/categorias";

const categoriasApi = new CategoriasApi();

const categorias = ref([]);
const categoria = ref({});

async function salvar() {
  if (categoria.value.id) {
    await categoriasApi.atualizarCategoria(categoria.value);
  } else {
    await categoriasApi.adicionarCategoria(categoria.value);
  }
  categoria.value = {};
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
}
function editar(categ) {
  Object.assign(categoria.value, categ);
}

async function excluir(categoria) {
  await categoriasApi.excluirCategoria(categoria.id);
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
}

onMounted(async () => {
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
});
</script>

<template>
  <h1>Categoria</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="categoria.descricao" placeholder="Descrição" />
    <button @click="salvar">Salvar</button>
  </div>
  <hr />
  <ul>
    <li v-for="categoria in categorias" :key="categoria.id">
      <span @click="editar(categoria)">
        ({{ categoria.id }}) - {{ categoria.descricao }} -
      </span>
      <button @click="excluir(categoria)">X</button>
    </li>
  </ul>
</template>
