<template>
  <div class="table-pessoas">
    <table class=" table table-striped tb-rendered col-8" id="tb-pessoas">
      <thead class="thead-dark">
        <tr>
          <th scope="col">--</th>
          <th scope="col">Nome</th>
          <th scope="col">Email</th>
          <th scope="col">Ativo</th>
          <th scope="col" style="width: 200px;">Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="pessoa in displayedPessoas" :key="pessoa.id">
          <td>
            <!-- checkbox se limita a uma seleção apenas -->
            <!-- quando clicada emite uma função com dado pessoa a ser usado na importação do componente -->
            <input
              type="checkbox"
              name="sel-Pessoa"
              @click="$emit('pessoa-sel', pessoa)"
              v-model="pSel"
              :value="pessoa"
              :disabled="pSel.length == 1"
            />
          </td>
          <td>{{ pessoa.no_nome }}</td>
          <td>{{ pessoa.no_email }}</td>
          <td>{{ pessoa.ativo }}</td>
          <td class="button-gp">
            <button
              class="btn btn-sm btn-dark mr-10"
              @click="showModal(pessoa)"
            >
              <b-icon icon="search"></b-icon>
            </button>
            <button
              @click="findPessoa(pessoa.id)"
              class="btn btn-sm btn-warning mr-10"
            >
              <b-icon icon="pencil-square"></b-icon>
            </button>
            <button
              @click="deletePessoa(pessoa.id)"
              class=" btn btn-sm btn-danger"
            >
              <b-icon icon="trash"></b-icon>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="offset">
      <nav class="col-12" aria-label="Page navigation example">
        <ul class="pagination">
          <li class="page-item">
            <button
              type="button"
              class="page-link"
              v-if="page != 1"
              @click="page--"
            >
              <b-icon icon="caret-left"></b-icon>
            </button>
          </li>
          <li class="page-item">
            <button
              type="button"
              class="page-link"
              v-for="pageNumber in pages.slice(page - 1, page + 5)"
              v-bind:key="pageNumber"
              @click="page = pageNumber"
              displayedPessoas
            >
              {{ pageNumber }}
            </button>
          </li>
          <li class="page-item">
            <button
              type="button"
              @click="page++"
              v-if="page < pages.length"
              class="page-link"
            >
              <b-icon icon="caret-right"></b-icon>
            </button>
          </li>
        </ul>
      </nav>
    </div>
    <b-modal ref="my-modal" hide-header hide-footer title="Detalhe Pessoa">
      <div class="d-block text-center">
        <Detail name="detalhe" :dados="elementoDetalhe" />
      </div>
      <button
        class="btn btn-dark bt-modal"
        variant="outline-danger"
        block
        @click="hideModal"
      >
        Fechar
      </button>
    </b-modal>
  </div>
</template>

<script>
import Detail from "../details/Detail";

export default {
  name: "TablePessoas",
  components: {
    Detail,
  },

  data() {
    return {
      // usado para limitar seleções na checkBox
      pSel: [],
      pessoas: [
        {
          id: "b73db850-179b-4694-bb28-25b032378aa6",
          no_nome: "Charlie Cassam",
          no_email: "ccassam0@dedecms.com",
          endereco: "7 Roth Center",
          sexo: "M",
          ativo: false,
          created_at: "2019-12-02",
          updated_at: "2020-08-31",
        },
        {
          id: "8e116509-10a6-49ae-9d66-3223e143d198",
          no_nome: "Martelle Skehan",
          no_email: "mskehan1@mayoclinic.com",
          endereco: "81 Utah Drive",
          sexo: "F",
          ativo: false,
          created_at: "2020-04-08",
          updated_at: "2020-07-14",
        },
        {
          id: "b416027e-9127-49aa-9867-18cc4b263a59",
          no_nome: "Florentia Royle",
          no_email: "froyle2@forbes.com",
          endereco: "9950 Merchant Trail",
          sexo: "F",
          ativo: false,
          created_at: "2019-12-19",
          updated_at: "2020-06-07",
        },
        {
          id: "9dfbca35-bdd8-4e4e-9267-baef30f4d07b",
          no_nome: "Beverie Tavner",
          no_email: "btavner3@cbslocal.com",
          endereco: "21680 Hermina Pass",
          sexo: "F",
          ativo: true,
          created_at: "2020-01-27",
          updated_at: "2020-09-10",
        },
        {
          id: "e1ce8659-81d3-410d-aa94-d21d71dc12b9",
          no_nome: "Denys Coney",
          no_email: "dconey4@github.io",
          endereco: "03 Sunnyside Circle",
          sexo: "F",
          ativo: false,
          created_at: "2019-11-23",
          updated_at: "2020-08-31",
        },
        {
          id: "1f85bf07-687d-4b5f-9eda-9a9975e0a464",
          no_nome: "Alfi Donhardt",
          no_email: "adonhardt5@nbcnews.com",
          endereco: "40 Eagle Crest Avenue",
          sexo: "F",
          ativo: true,
          created_at: "2019-11-13",
          updated_at: "2020-09-17",
        },
        {
          id: "f33d215e-35d9-49dc-ba62-a06fbbc90068",
          no_nome: "Marty Dibnah",
          no_email: "mdibnah6@whitehouse.gov",
          endereco: "26005 Randy Terrace",
          sexo: "F",
          ativo: true,
          created_at: "2020-01-05",
          updated_at: "2020-06-04",
        },
        {
          id: "ea8a95ea-b4a6-460c-9a11-9f1fc7ef295c",
          no_nome: "Curt Katte",
          no_email: "ckatte7@vimeo.com",
          endereco: "17 Michigan Park",
          sexo: "M",
          ativo: false,
          created_at: "2020-05-15",
          updated_at: "2020-08-01",
        },
        {
          id: "31ad1eb9-67aa-4890-9790-cb96b0fb5fbd",
          no_nome: "Shell Hise",
          no_email: "shise8@answers.com",
          endereco: "1 Jenna Center",
          sexo: "F",
          ativo: true,
          created_at: "2019-11-03",
          updated_at: "2020-05-28",
        },
        {
          id: "81232d97-668a-476b-afc1-dcc5638ab673",
          no_nome: "Lisbeth Spadotto",
          no_email: "lspadotto9@hugedomains.com",
          endereco: "49992 Kipling Alley",
          sexo: "F",
          ativo: false,
          created_at: "2020-01-22",
          updated_at: "2020-08-06",
        },
      ],
      pessoa: {},
      page: 1,
      perPage: 5,
      pages: [],
      pessoasPag: [],
      elementoDetalhe: {},
    };
  },
  methods: {
    showModal(pessoa) {
      this.elementoDetalhe = pessoa;
      this.$refs["my-modal"].show();
      console.log(pessoa);
    },
    hideModal() {
      this.$refs["my-modal"].hide();
    },
    setPages() {
      let numberOfPages = Math.ceil(this.pessoas.length / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
    },
    paginate(pessoas) {
      let page = this.page;
      let perPage = this.perPage;
      let from = page * perPage - perPage;
      let to = page * perPage;
      return pessoas.slice(from, to);
    },

    findPessoa(id) {
      console.log(id);
    },
    deletePessoa(id) {
      console.log(id);
    },
  },
  computed: {
    displayedPessoas() {
      return this.paginate(this.pessoas);
    },
  },
  created() {
    this.setPages();
  },
};
</script>

<style scoped>
button.page-link {
  display: inline-block;
}
button.page-link {
  font-size: 12px;
  color: #222;
  font-weight: bold;
  width: 40px;
}
button.bt-modal {
  width: 200px;
  margin: 0 120px;
}
.offset {
  width: 500px !important;
  margin: 20px auto;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
.tb-rendered {
  margin: auto;
  width: 700px;
  background: #d070462d;
}

.btn {
  width: 40px;
}
.button-gp {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: flex-start;
}
button {
  margin: 0 10px;
}
</style>
