<template>
  <b-container fluid>
    <b-row>
        <b-col md="12">
          <iq-card>
            <template v-slot:headerTitle>
              <h4 class="card-title mt-3">Control de placas</h4>
              <div class="iq-search-bar mt-2">
                <div class="row">
                  <div class="col-sm">
                    <b-form action="#" class="searchbox">
                    </b-form>
                  </div>
                  <div class="col-sm">
                  </div>
                </div>
              </div>
            </template>
            <template v-slot:body>
              <b-tabs>
                <b-tab title="Tipos de placa" active>
                  <div class="d-flex justify-content-end mb-3">
                    <b-button variant="primary" v-b-modal.modal-1>AGREGAR NUEVO</b-button>
                  </div>
                  <table id="miTabla" class="display">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Nombre</th>
                            <th>Descripcion</th>
                            <th>Estado</th>
                            <th>Acciones</th>
                            <th>Desactivar o Activar</th>
                        </tr>
                    </thead>
                    <tbody>
                      <tr v-for="datos in datosPost" :key="datos.id">
                          <td v-text="datos.id"></td>
                          <td v-text="datos.NombreTipodePlaca"></td>
                          <td v-text="datos.Descripcion"></td>
                          <td v-text="datos.Estado"></td>
                          <td>
                            <b-button variant="primary" @click="setData(datos)">Editar</b-button>
                            <b-button variant="danger" @click="modalEliminar(datos)">Eliminar</b-button>
                          </td>
                          <td>
                            <template>
                              <b-button v-if="datos.Estado === 1" @click="postEstado(1, datos.id)">Desactivar</b-button>
                              <b-button v-else variant="danger" @click="postEstado(2, datos.id)">Activar</b-button>
                            </template>
                          </td>
                      </tr>
                    </tbody>
                </table>
                </b-tab>
              </b-tabs>
            </template>
          </iq-card>
        </b-col>
    </b-row>
    <b-modal id="modal-1" ref="modal-1" size="xl" title="Bienvenido al modal de ingreso de equipos">
        <form>
          <div class="row">
            <div class="col-md-6 mb-3">
              <label for="NombreTipodePlaca">Nombre de tipo placa</label>
              <input
                required
                type="text"
                class="form-control"
                id="NombreTipodePlaca"
                v-model.trim="$v.form.NombreTipodePlaca.$model"
                :state="!$v.form.NombreTipodePlaca.$error"
                placeholder="Ingresar tipo de placa"
              >
            </div>
            <div class="col-md-6 mb-3">
              <label for="Descripcion">Descripcion</label>
              <input
                required
                class="form-control"
                id="Descripcion"
                type="text"
                v-model.trim="$v.form.Descripcion.$model"
                :state="!$v.form.Descripcion.$error"
                placeholder="Ingrese descripcion"
              >
            </div>
          </div>
          <div class="row">
            <div class="col-md-12 mb-3">
              <label for="Estado">Estado</label>
              <input
                required
                class="form-control"
                id="Estado"
                type="number"
                v-model.trim="$v.form.Estado.$model"
                :state="!$v.form.Estado.$error"
                placeholder="Ingresar estado"
              >
            </div>
          </div>
        </form>
        <template #modal-footer="{}">
          <b-button  variant="primary" @click="onSave(), $bvModal.hide('modal-1')"
            >Guardar</b-button
          >
          <b-button variant="danger" @click="$bvModal.hide('modal-1')"
            >Cancelar</b-button
          >
        </template>
    </b-modal>
    <b-modal id="modal-2" ref="modal-2" size="xl" title="Bienvenido al modal de edicion de equipo">
        <form>
          <div class="row">
            <div class="col-md-6 mb-3">
              <label for="NombreTipodePlaca">Nombre de tipo placa</label>
              <input
                required
                type="text"
                class="form-control"
                id="NombreTipodePlaca"
                v-model.trim="$v.form.NombreTipodePlaca.$model"
                :state="!$v.form.NombreTipodePlaca.$error"
                placeholder="Ingresar tipo de placa"
              >
            </div>
            <div class="col-md-6 mb-3">
              <label for="Descripcion">Descripcion</label>
              <input
                required
                class="form-control"
                id="Descripcion"
                type="text"
                v-model.trim="$v.form.Descripcion.$model"
                :state="!$v.form.Descripcion.$error"
                placeholder="Ingrese descripcion"
              >
            </div>
          </div>
          <div class="row">
            <div class="col-md-12 mb-3">
              <label for="Estado">Estado</label>
              <input
                required
                class="form-control"
                id="Estado"
                type="number"
                v-model.trim="$v.form.Estado.$model"
                :state="!$v.form.Estado.$error"
                placeholder="Ingresar estado"
              >
            </div>
          </div>
        </form>
        <template #modal-footer="{}">
          <b-button  variant="primary" @click="onUpdate(), $bvModal.hide('modal-2')"
            >Editar</b-button
          >
          <b-button variant="danger" @click="$bvModal.hide('modal-2')"
            >Cancelar</b-button
          >
        </template>
    </b-modal>
    <b-modal id="modal-3" ref="modal-3" title="Eliminar equipo">
        <h6 class="my-4">
          ¿Desea eliminar el equipo?
        </h6>
        <template #modal-footer="{}">
          <b-button
            type="submit"
            variant="primary"
            @click="eliminarRegistro(datos), $bvModal.hide('modal-3')">Eliminar</b-button>
          <b-button variant="danger" @click="$bvModal.hide('modal-3')"
            >Cancelar</b-button
          >
        </template>
    </b-modal>
  </b-container>
</template>
<script>
import { xray } from '../../../config/pluginInit'
import { laravelUrl } from '../../../config/constant'
import axios from 'axios'
import useVuelidate from '@vuelidate/core'
import { required } from '@vuelidate/validators'

export default {
  name: 'Ejercicio',
  setup () {
    return { $v: useVuelidate() }
  },
  components: {
  },
  data () {
    return {
      search: '',
      datosPost: [],
      form: {
        NombreTipodePlaca: '',
        Descripcion: '',
        Estado: 1
      },
      apiBase: laravelUrl + '/tiposdeplaca/getTodos'
    }
  },
  mounted () {
    xray.index()
    axios.get(laravelUrl + '/tiposdeplaca/getTodos').then((response) => {
      this.datosPost = response.data
    })
  },
  beforeMount () {
  },
  validations () {
    return {
      form: {
        NombreTipodePlaca: { required },
        Descripcion: { required },
        Estado: { required }
      }
    }
  },
  methods: {
    modalEliminar (datos) {
      this.datos = datos
      this.$refs['modal-3'].show()
    },
    setData (datos) {
      this.datos = datos
      this.form.id = datos.id
      this.form.NombreTipodePlaca = datos.NombreTipodePlaca
      this.form.Descripcion = datos.Descripcion
      this.form.Estado = datos.Estado
      this.$refs['modal-2'].show()
    },
    modalActualizar (datos) {
      this.form = datos
      this.$bvModal.show('modal-2')
    },
    onUpdate () {
      const me = this
      axios.put(laravelUrl + '/tiposdeplaca/' + parseInt(me.form.id), {
        NombreTipodePlaca: me.form.NombreTipodePlaca,
        Descripcion: me.form.Descripcion,
        Estado: me.form.Estado })
        .then((response) => {
          me.refresh()
          me.getDatos()
        })
        .catch((error) => {
          console.error('Error!', error)
        })
    },
    refresh () {
      axios.get(laravelUrl + '/tiposdeplaca/getTodos').then((response) => {
        this.datosPost = response.data
      })
    },
    postEstado (accion, datosid) {
      const me = this
      if (accion === 1) {
        axios.put(laravelUrl + '/tiposdeplaca/state/' + datosid, {
          Estado: 0 })
          .then((response) => {
            me.refresh()
          })
          .catch((error) => {
            console.error('Error!', error)
          })
      } else if (accion === 2) {
        axios.put(laravelUrl + '/tiposdeplaca/state/' + datosid, {
          Estado: 1 })
          .then((response) => {
            me.refresh()
          })
          .catch((error) => {
            console.error('Error!', error)
          })
      }
    },
    eliminarRegistro (datos) {
      axios.delete(laravelUrl + '/tiposdeplaca/' + datos.id).then((response) => {
        console.log(datos.id)
        console.log('Eliminado')
        this.refresh()
        this.getDatos()
      })
    },
    getDatos () {
      axios.get(laravelUrl + '/tiposdeplaca/getTodos').then((response) => {
        this.datosPosts = response.data
      })
    },
    /* Guardar */
    onSave () {
      const me = this
      axios.post(laravelUrl + '/tiposdeplaca/', {
        NombreTipodePlaca: me.form.NombreTipodePlaca,
        Descripcion: me.form.Descripcion,
        Estado: me.form.Estado })
        .then((response) => {
          me.getDatos()
          me.refresh()
        })
        .catch((error) => {
          console.error('Error!', error)
        })
    }
  }
}
</script>
  <style>
  table {
      width: 100%;
      border-collapse: collapse;
  }

  /* Estilo para las celdas de encabezado */
  th {
      background-color: #f2f2f2;
  }

  /* Estilo para las celdas de datos */
td, th {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
}
</style>
