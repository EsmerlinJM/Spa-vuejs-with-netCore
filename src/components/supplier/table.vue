<template>
<div>
        <v-btn
        fab
        dark
        small
        color="indigo"
        class="btn-icons"
        @click="dialogFormVisible1 = true"
         >
        <v-icon>assignment_ind</v-icon>
      </v-btn>
<el-dialog title="Suplidores" :visible.sync="dialogFormVisible1">
  <el-form :model="form1"  :rules="rules1" ref="form1">
    <el-form-item  label="Nuevo" :label-width="formLabelWidth"  prop="name">
      <el-input v-model="form1.name" placeholder="Nombre del suplidor" auto-complete="off"></el-input>
    </el-form-item>
    <el-form-item :label-width="formLabelWidth">
     <el-button @click="dialogFormVisible1 = false; msg('canselar')">Cancelar</el-button>
    <el-button v-show="form1.name" type="primary" @click="dialogFormVisible1 = false; msg('ok')" >Agregar</el-button>
    </el-form-item>
     </el-form> 
<v-card  v-loading="loading">
  <!-- red darken-2 color para contrastar -->
    <h1 class="text-center mt-2">Suplidor
    </h1>
    <v-card-title>
      <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="search"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="data"
      :search="search"
    >
      <template slot="items" slot-scope="props">
        <td>{{props.item.name}}</td>
        <td>
          <div class="row">
          <button @click="remove(props.item.id)" ><v-icon class="btn btn-danger ">delete</v-icon></button>
          </div>
         </td>
      </template>
      <v-alert slot="no-results" :value="true" color="error" icon="warning">
        Your search for "{{ search }}" found no results.
      </v-alert>
    </v-data-table>
  </v-card>       
    </el-dialog>
</div>
</template>
<script>
/* eslint-disable */
export default {
  data() {
    return {
      data: [],
      name: "table",
      dialogFormVisible1: false,
      formLabelWidth: "120px",
      loading: false,
      search: "",
      form1: {
        name: ""
      },
      headers: [{ text: "Nombre", value: "name" }],
      rules1: {
        name: [
          {
            required: true,
            message: "Ingrese el nombre de la marca",
            trigger: "blur"
          }
        ]
      }
    };
  },
  created() {
    let supplier = this;
    supplier.get();
  },
  methods: {
    msg(value) {
      let supplier = this;
      supplier.loading = true;
      switch (value) {
        case "ok":
          supplier.$store.state.services.supplierService
            .add(supplier.form1)
            .then(b => {
              supplier.loading = false;
              this.$message({
                message: "Suplidor Añadido",
                type: "success"
              });
              supplier.get();
            })
            .catch(b => {
              supplier.$message({
                message: "ha ocurrido un error",
                type: "error"
              });
              supplier.loading = false;
            });
          break;
        case "canselar":
          this.$message("accion cancelada");
          supplier.loading = false;
          break;
      }
    },
    get() {
      let supplier = this;
      supplier.loading = true;
      supplier.$store.state.services.supplierService
        .getAllSupplier()
        .then(b => {
          supplier.data = b.data;
          supplier.loading = false;
        })
        .catch(b => {
          supplier.$message({
            message: "ha ocurrido un error",
            type: "error"
          });
        });
    },
    remove(id) {
      let supplier = this;
      supplier.loading = true;
      supplier.$store.state.services.supplierService.remove(id).then(b => {
        supplier.$message({
          message: "Suplidor eliminado",
          type: "success"
        });
        supplier.loading = false;
        supplier.get()
      });
    }
  }
};
</script>
<style>
.add {
  color: white;
}
</style>
