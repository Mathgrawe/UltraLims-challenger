<template>
  <div class="flex-1 bg-neutral-200 h-screen gap-5">
    <div class="absolute p-2 bg-white rounded-2xl font-semibold shadow-lg">
      <RouterLink to="/desafio2">Desafio2</RouterLink>
    </div>
    <div class="flex flex-col items-center justify-center">
      <div class="py-8">
        <p class="font-semibold text-2xl text-black">Consulta de CEP</p>
      </div>
      <div class="flex items-center bg-white p-5 rounded-2xl shadow-lg mb-5">
        <div class="flex flex-row border rounded-md p-1">
          <input
            v-model="cep"
            placeholder="Digite um CEP"
            class="border-none active:no-underline outline-none flex-grow"
          />
          <button
            @click="buscarEndereco"
            class="flex flex-row items-center px-4 py-2 rounded-lg"
          >
            <SearchIcon class="ml-2" />
          </button>
        </div>
      </div>

      <div
        class="flex flex-col bg-white p-5 shadow-xl rounded-2xl text-lg"
        v-if="apiData"
      >
        <p>CEP: {{ apiData.cep }}</p>
        <p>Logradouro: {{ apiData.logradouro }}</p>
        <p>Bairro: {{ apiData.bairro }}</p>
        <p>Cidade: {{ apiData.localidade }}</p>
        <p>Estado: {{ apiData.uf }}</p>
        <p>DD: {{ apiData.ddd }}</p>
      </div>
    </div>
    <div class="flex justify-center mx-20">
      <table class="mt-10 bg-white table-auto w-full rounded-lg">
        <thead>
          <tr>
            <th class="border-">CEP</th>
            <th class="border">Logradouro</th>
            <th class="border">
              Bairro
              <button @click="ordenarRegistros('bairro')">
                <ArrowIcon class="w-5 h-5" />
              </button>
            </th>
            <th class="border">
              Cidade
              <button @click="ordenarRegistros('uf')">
                <ArrowIcon class="w-5 h-5" />
              </button>
            </th>
            <th class="border">
              Estado
              <button @click="ordenarRegistros('localidade')">
                <ArrowIcon class="w-5 h-5" />
              </button>
            </th>
            <th class="border">DD</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="registro in consulta" :key="registro.cep">
            <td class="border text-center">{{ registro.cep }}</td>
            <td class="border text-center">{{ registro.logradouro }}</td>
            <td class="border text-center">{{ registro.bairro }}</td>
            <td class="border text-center">{{ registro.localidade }}</td>
            <td class="border text-center">{{ registro.uf }}</td>
            <td class="border text-center">{{ registro.ddd }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import SearchIcon from "../../assets/icons/search.svg";
import ArrowIcon from "../../assets/icons/arrow.svg";

export default {
  name: "Home",
  components: { SearchIcon, ArrowIcon },
  data() {
    return {
      cep: "",
      apiData: null,
      consulta: [],
      sortField: "",
      sortOrder: "asc",
    };
  },
  methods: {
    buscarEndereco() {
      axios
        .get(`https://viacep.com.br/ws/${this.cep}/json/`)
        .then((response) => {
          this.apiData = response.data;

          if (this.apiData) {
            this.consulta.unshift(this.apiData);
          }
        })
        .catch((error) => {
          console.error("Ocorreu um erro ao buscar os dados da API: " + error);
        });
    },
    ordenarRegistros(field) {
      if (field === this.sortField) {
        this.sortOrder = this.sortOrder === "asc" ? "desc" : "asc";
      } else {
        this.sortField = field;
        this.sortOrder = "asc";
      }

      this.consulta.sort((a, b) => {
        const fieldA = a[field];
        const fieldB = b[field];

        if (this.sortOrder === "asc") {
          return fieldA.localeCompare(fieldB);
        } else {
          return fieldB.localeCompare(fieldA);
        }
      });
    },
  },
};
</script>
