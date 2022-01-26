<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h3>Selecciona Rango De Fechas</h3>
    <div class="d-flex justify-content-center">
      <div class="col-4 mx-5">
        <div>
          <label for="example-datepicker">Fecha Inicio</label>
          <b-form-datepicker id="example-datepicker" v-model="value1" :state=state1 class="mb-2"></b-form-datepicker>
        </div>
      </div>  

      <div class="col-4 mx-5">
        <div>
          <label for="example-datepicker">Fecha Fin</label>
          <b-form-datepicker id="example-datepicker2" v-model="value2" :state=state2 class="mb-2"></b-form-datepicker>
        </div>
      </div>
    </div>

    <div class="d-flex justify-content-center">
      <div class="col-3 mt-4">
        <b-button variant="success" 
        @click="getData"
        >
        Buscar
        </b-button>
        <p v-if="errorMessage" class="mt-2">
          Por favor selecciona un rango de fechas valido
        </p>
      </div>
    </div>
    <div class="d-flex justify-content-center">
      <div class="col-10">
        <b-table
          id="employee-table"
          ref="employee-table"
          :items="dataFinal"
          small
        ></b-table>

      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data(){
    return{
      value1: "",
      value2: "",
      state1: false,
      state2: false,
      errorMessage: false,
      tableShow: false,
      dataTables: [],
      dataFinal: [],
      
    }
  },
  watch: {
    value1: function () {
      this.state1 = true;
    },
    value2: function () {
      this.state2 = true;
    },
  },

  methods: {
    async getData(){
      if(this.state1 == true && this.state2 == true){
        this.errorMessage = false;

        const url = "https://localhost:44387/Reports";
        const fechaIni = this.value1;
        const fechaFin = this.value2;
        const dataFetch = async (url,fechaIni,fechaFin) => {
          return await fetch(url, {headers: {'sFechaIni': fechaIni , 'sFechaFin': fechaFin}})
          .then(res => res.json()).catch(err => alert('err' + err.errorMessage));
        }
        const tempData = await dataFetch(url,fechaIni,fechaFin);

        this.dataTables = await tempData;

        console.log(this.dataTables);
        console.log(this.dataTables[0]);


        let count = 0;
        this.dataTables[0].forEach(() => {
          this.dataFinal.push({
            nrreporte: this.dataTables[0][count], 
            svindice: this.dataTables[1][count], 
            svfhreporte: this.dataTables[2][count], 
            prnombre: this.dataTables[3][count], 
            prrfc: this.dataTables[4][count], 
            clnombre: this.dataTables[5][count],
            clestatus: this.dataTables[6][count],
            sunombre: this.dataTables[7][count],
            susupervisoria: this.dataTables[8][count],
          });
          count += 1;
        });

        
        
      }else{
        this.errorMessage = true;
      }
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
