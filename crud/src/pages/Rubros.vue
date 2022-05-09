<template>
  <form action="">
    <div class="q-pa-md">
      <h5>Mantenimiento de Rubros</h5>
    </div>

    <div class="row">
      <div class="col-md-6">
        <div class="q-pa-md">
          <q-input
            dense
            filled
            label="Nombre de Rubro"
            v-model="nombre"
          ></q-input>
        </div>
      </div>
      <div class="col-md-6">
        <div class="q-pa-md">
          <q-input dense filled label="Estado" v-model="estado"></q-input>
        </div>
      </div>
    </div>
    <div class="q-pa-md">
        <q-btn label="enviar" @click="enviarDatos" color="primary"></q-btn>
    </div>
    <div class="q-pa-md">
      <q-table
        flat
        bordered
        no-data-label="No hay datos"
        color="blue"
        :columns="data_columns"
        :rows="data_rubro"
      >
      <template  #body-cell-estado="props">
            <q-td :props="props">
                <q-chip
                  :color="props.row.estado == 'true' ? 'green': props.row.estado == 'false' ? 'red': 'grey'"
                  text-color="white"
                  dense
                  class="text-weight-bolder"
                  square
                >{{props.row[props.col.name]}}
                </q-chip>
            </q-td>
          </template>
           <template v-slot:body-cell-acciones="props">
            <q-td :props="props">
              <q-btn dense round flat color="grey" @click="editRow(props)" icon="edit"></q-btn>
              <q-btn dense round flat color="grey" @click="deleteRow(props)" icon="delete"></q-btn>
            </q-td>          
          </template>
      </q-table>
    </div>
  </form>
</template>
<script>
module.exports = {
  chainWebpack: (config) => {
    config.module.rules.delete("eslint");
  },
  data() {
    return {
      nombre: "",
      estado: "",
      data_columns: [
        {name:"acciones",label:"acciones",align:"center",field:"acciones"},
        {
          name: "nombre",
          label: "Nombre de Rubro",
          align: "center",
          field: "nombre",
        },
        {
          name: "estado",
          label: "Estado",
          align: "center",
          field: "estado",
        },
      ],
      data_rubro: [],
    };
  },
  methods: {
    async datosRubros() {
      await this.$axios.get("http://localhost:3000/rubros").then((res) => {
        this.data_rubro = res.data;
      }).catch(error=>{
             console.error(error.message);
      });
    },
    
    async enviarDatos(){
        var datos = {
            nombre:this.nombre,
            estado:this.estado
        }
        await this.$axios.post("http://localhost:3000/rubros",datos).
        then(res=> {
            console.log(res);
            datos = this.data_rubro.push({
                nombre:this.nombre,
                estado:this.estado
            });
        });
    },
      
  },
  async mounted() {
    this.datosRubros();
    
  },
};
</script>
