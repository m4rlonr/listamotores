<template>
  <v-container>
    <v-row>
      <v-col>
        <q-card class="my-card text-white" style="background: radial-gradient(circle, #35a2ff 0%, #014a88 100%)">
          <q-card-actions align="around">
            <q-btn flat @click="download">GERAR PDF</q-btn>
            <q-btn flat @click="Router">VOLTAR</q-btn>
          </q-card-actions>
        </q-card>
      </v-col>
    </v-row>
  </v-container>
  <v-container id="PDFpage">
    <v-row>
      <v-col>
        <Lista v-if="tipo === 0" :headers="headers" :table="table" :title="title" pesquisa="true" :pagination="false" />
        <Lista v-if="tipo === 1" :headers="headers1" :table="table" :title="title" pesquisa="true"
          :pagination="false" />
      </v-col>
    </v-row>
    <v-row class="ma-5">
      <v-col class="CampoAssinatura">
        <span style="text-decoration: underline;">{{ elaborador }}</span>
        <span id="linhaAssinatura">Elaborador</span>
      </v-col>
      <v-col>
        <v-row class="ma-0">
          <v-col>
            <div class="textInfo d-flex justify-space-around">
              <div>
                <strong>Emitido em:</strong>
                <p>{{ emitidoEm }}</p>
              </div>
            </div>
          </v-col>
        </v-row>
      </v-col>
      <v-col class="CampoAssinatura">
        <span id="linhaAssinatura">Tecnologia Mr'Tecnol</span>
        <p>Todos os direitos reservados.</p>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import hmtl2pdf from "html2pdf.js";
import store from "@/store";
import Lista from "../components/Lista.vue";

export default {
  name: "PDFpage",

  components: {
    Lista,
  },
  computed: {
    table() {
      return store.state.listaImpressao;
    },
    title() {
      return store.state.titulo;
    },
    elaborador() {
      return store.state.elaborador;
    },
    tipoImpressao() {
      return store.state.tipoImpressao;
    }
  },
  data() {
    return {
      tipo: null,
      observacoes:
        "A responsabilidade das especificações abaixo é da ELETROTÉCNICA ARAÚJO. Quaisquer alterações ou substituição dos itens listados deverá ser aprovado pela equipe da Engenharia",
      headers: [
        {
          align: 'left',
          sortable: true,
          name: "id",
          label: "#",
          field: "id",
        },
        {
          align: 'left',
          sortable: true,
          name: "descricao",
          label: "Descrição",
          field: "descricao",
        },
        {
          align: "center",
          name: "cv",
          label: "CV",
          // field: (row) => this.convertProduto(row.produto),
          field: "cv",
        },
        {
          align: "center",
          name: "cv",
          label: "kW",
          field: "kw",
        },
        {
          align: "center",
          name: "cv",
          label: "HP",
          field: "hp",
        },
        {
          align: "center",
          name: "tensao",
          label: "Tensão(V)",
          field: "tensao",
        },
        {
          align: "center",
          name: "Corrente ",
          label: "Corrente(A)",
          field: "corrente",
          // field: (row) => this.convertProduto(row.cv, row.tensao),
        },
      ],
      headers1: [
        {
          align: 'left',
          sortable: true,
          name: "id",
          label: "#",
          field: "id",
        },
        {
          align: 'left',
          sortable: true,
          name: "descricao",
          label: "Descrição",
          field: "descricao",
        },
        {
          align: "center",
          name: "quantidade",
          label: "Quantidade",
          field: "quantidade",
        },
        {
          align: "center",
          name: "medida",
          label: "Unidade de medida",
          field: "medida",
        },

      ],
      emitidoEm: null
    };
  },
  methods: {
    download() {
      let config = {
        // margin: 0.2,
        pagespilt: true,
        filename: this.title,
        image: { type: "png" },
        html2canvas: { scale: 2 },
        jsPDF: { unit: "in", format: "letter", orientation: "portrait", page_size: "A4" },
        pagebreak: { mode: 'avoid-all', before: '#page2el' }
      };
      let template = document.getElementById("PDFpage");
      hmtl2pdf().set(config).from(template).save();
    },
    // convertProduto(cv, tensao) {
    //   console.log(cv, tensao)
    //   if (tensao === "220") {
    //     return cv * 2.5
    //   } if (tensao === "380") {
    //     return cv * 1.5
    //   } if (tensao === "440") {
    //     return cv * 1.75
    //   }
    // },
    Router() {
      if (this.tipo === 0) {
        this.$router.push('/carga')
      }
      if (this.tipo === 1) {
        this.$router.push('/material')
      }
    }
  },
  mounted() {
    const today = new Date(Date.now());
    this.emitidoEm = today.toLocaleDateString()
    this.tipo = this.tipoImpressao
  }
};
</script>

<style>
.textAlerts {
  font-size: 10px;
  text-transform: uppercase;
}

.textInfo {
  font-size: 12px;
  text-transform: uppercase;
}

.title_table {
  font-size: 24px;
  text-align: center;
  text-transform: uppercase;
}

#linhaAssinatura {
  font-size: 14px;
  font-weight: bold;
}

.CampoAssinatura {
  margin-top: 1rem;
  padding: 0px;
  display: flex;
  place-content: center;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>