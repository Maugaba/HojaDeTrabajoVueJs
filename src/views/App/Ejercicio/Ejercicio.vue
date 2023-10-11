<template>
  <b-container fluid>
    <b-row>
      <b-col md="12">
        <iq-card>
          <template v-slot:headerTitle>
            <h4 class="card-title mt-3">Ejercicios</h4>
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
              <b-tab title="Ejercicio1" active>
                <div class="d-flex justify-content-end mb-3">
                  <b-button variant="primary" v-b-modal.modal-1>AGREGAR NUEVO</b-button>
                </div>
                <table id="miTabla" class="display">
                  <thead>
                      <tr>
                          <th>Nombre</th>
                          <th>Cantidad de Jugadores</th>
                          <th>Direccion de las oficinas del equipo</th>
                          <th>Estadio</th>
                          <th>Id que relaciona team con jugadores</th>
                          <th>Estado</th>
                          <th>Acciones</th>
                      </tr>
                  </thead>
                  <tbody>
                    <tr v-for="team in teams" :key="team.id">
                        <td v-text="team.Name"></td>
                        <td v-text="team.PlayersCount"></td>
                        <td v-text="team.DirectionTeam"></td>
                        <td v-text="team.Stadium"></td>
                        <td v-text="team.idPlayersTeam"></td>
                        <td>
                            <template v-if="team.state === 1">
                              ACTIVO
                            </template>
                            <template v-else>
                              INACTIVO
                            </template>
                        </td>
                        <td>
                          <template>
                            <b-button @click="editar(team.id)" variant="primary">Editar</b-button>
                            &nbsp;
                          </template>
                          <template v-if="team.state === 1">
                                <button class="btn btn-success" @click="postEstado(1, team.id)">
                                    DESACTIVAR
                                </button>
                          </template>
                          <template v-else>
                              <button class="btn btn-danger" @click="postEstado(2, team.id)">
                                  ACTIVAR
                              </button>
                          </template>
                          <template>
                            &nbsp;
                            <button class="btn btn-danger" @click="eliminar(team.id)">
                                ELIMINAR
                            </button>
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
            <label for="nombre">Nombre del equipo</label>
            <input
              required
              type="text"
              class="form-control"
              id="nombre"
              v-model.trim="$v.form.nombre.$model"
              :state="!$v.form.nombre.$error"
              placeholder="Ingresar nombre"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="quantity">Cantidad de Jugadores del equipo</label>
            <input
              required
              class="form-control"
              id="quantity"
              type="number"
              v-model.trim="$v.form.quantity.$model"
              :state="!$v.form.quantity.$error"
              placeholder="Ingresar cantidad"
            >
          </div>
        </div>
        <div class="row">
          <div class="col-md-12 mb-3">
            <label for="direction">Dirección de las oficinas del equipo</label>
            <input
              required
              class="form-control"
              id="direction"
              v-model.trim="$v.form.direction.$model"
              :state="!$v.form.direction.$error"
              placeholder="Ingresar dirección"
            >
          </div>
        </div>
        <div class="row">
          <div class="col-md-6 mb-3">
            <label for="stadium">Nombre del estadio</label>
            <input
              required
              type="text"
              class="form-control"
              id="stadium"
              v-model.trim="$v.form.stadium.$model"
              :state="!$v.form.stadium.$error"
              placeholder="Ingresar estadio"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="idteam">Id que relaciona team con jugadores</label>
            <input
              required
              class="form-control"
              id="idteam"
              type="number"
              v-model.trim="$v.form.idteam.$model"
              :state="!$v.form.idteam.$error"
              placeholder="Ingresar Id de la relacion con jugadores"
            >
          </div>
        </div>
      </form>
      <template #modal-footer="{}">
        <b-button  variant="primary" @click="onValidate('save')"
          >Guardar</b-button
        >
        <b-button variant="danger" @click="closeModal('save')"
          >Cancelar</b-button
        >
      </template>
    </b-modal>
    <b-modal id="modal-2" ref="modal-2" size="xl" title="Bienvenido al modal de edicion de equipo">
      <form>
        <div class="row">
          <div class="col-md-6 mb-3">
            <label for="nombre">Nombre del equipo</label>
            <input
              required
              type="text"
              class="form-control"
              id="nombre"
              v-model.trim="$v.form.nombre.$model"
              :state="!$v.form.nombre.$error"
              placeholder="Ingresar nombre"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="quantity">Cantidad de Jugadores del equipo</label>
            <input
              required
              class="form-control"
              id="quantity"
              type="number"
              v-model.trim="$v.form.quantity.$model"
              :state="!$v.form.quantity.$error"
              placeholder="Ingresar cantidad"
            >
          </div>
        </div>
        <div class="row">
          <div class="col-md-12 mb-3">
            <label for="direction">Dirección de las oficinas del equipo</label>
            <input
              required
              class="form-control"
              id="direction"
              v-model.trim="$v.form.direction.$model"
              :state="!$v.form.direction.$error"
              placeholder="Ingresar dirección"
            >
          </div>
        </div>
        <div class="row">
          <div class="col-md-6 mb-3">
            <label for="stadium">Nombre del estadio</label>
            <input
              required
              type="text"
              class="form-control"
              id="stadium"
              v-model.trim="$v.form.stadium.$model"
              :state="!$v.form.stadium.$error"
              placeholder="Ingresar estadio"
            >
          </div>
          <div class="col-md-6 mb-3">
            <label for="idteam">Id que relaciona team con jugadores</label>
            <input
              required
              class="form-control"
              id="idteam"
              type="number"
              v-model.trim="$v.form.idteam.$model"
              :state="!$v.form.idteam.$error"
              placeholder="Ingresar Id de la relacion con jugadores"
            >
          </div>
        </div>
      </form>
      <template #modal-footer="{}">
        <b-button  variant="primary" @click="onValidate('update')"
          >Editar</b-button
        >
        <b-button variant="danger" @click="closeModal('update')"
          >Cancelar</b-button
        >
      </template>
    </b-modal>
    <b-modal id="modal-3" ref="modal-3" title="Eliminar equipo">
      <h6 class="my-4">
        ¿Desea eliminar el equipo: {{ nameteam }} ?
      </h6>
      <template #modal-footer="{}">
        <b-button
          type="submit"
          variant="primary"
          @click="Eliminar(form.id)
                  $bvModal.hide('modal-3')"
          >Desactivar</b-button
        >
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
      form: {
        id: '',
        nombre: '',
        quantity: '',
        direction: '',
        stadium: '',
        idteam: '',
        state: 1
      },
      teams: [],
      teamindividual: [],
      nameteam: '',
      apiBase: laravelUrl + '/team',
      typeOptions: []
    }
  },
  mounted () {
    xray.index()
    axios.get(laravelUrl + '/team/getTodos').then((response) => {
      this.teams = response.data
    })
  },
  beforeMount () {
  },
  validations () {
    return {
      form: {
        nombre: { required },
        quantity: { required },
        direction: { required },
        stadium: { required },
        idteam: { required }
      }
    }
  },
  methods: {
    postEstado (accion, teamid) {
      const me = this
      if (accion === 1) {
        axios.put(laravelUrl + '/team/state/' + teamid, {
          state: 0 })
          .then((response) => {
            me.refresh()
          })
          .catch((error) => {
            // this.errorMessage = error.message;
            console.error('Error!', error)
          })
      } else if (accion === 2) {
        axios.put(laravelUrl + '/team/state/' + teamid, {
          state: 1 })
          .then((response) => {
            me.refresh()
          })
          .catch((error) => {
            // this.errorMessage = error.message;
            console.error('Error!', error)
          })
      }
    },
    openModal (modal, action) {
      switch (modal) {
        case 'save': {
          this.$v.$reset()
          this.form.id = ''
          this.form.nombre = ''
          this.form.quantity = ''
          this.form.direction = ''
          this.form.stadium = ''
          this.form.idteam = ''
          this.form.state = 1
          break
        }
      }
    },
    closeModal (action) {
      switch (action) {
        case 'save': {
          this.$v.$reset()
          this.$refs['modal-1'].hide()
          this.form.id = ''
          this.form.nombre = ''
          this.form.quantity = ''
          this.form.direction = ''
          this.form.stadium = ''
          this.form.idteam = ''
          this.form.state = 1
          break
        }
        case 'update': {
          this.$v.$reset()
          this.$refs['modal-2'].hide()
          this.form.id = ''
          this.form.nombre = ''
          this.form.quantity = ''
          this.form.direction = ''
          this.form.stadium = ''
          this.form.idteam = ''
          this.form.state = 1
          break
        }
      }
    },
    onValidate (action) {
      this.$v.$touch()
      if (this.$v.$error !== true) {
        if (action === 'save') {
          this.onSave()
        } else if (action === 'update') {
          this.onUpdate()
        }
      }
    },
    setData (data) {
      this.form.user = data.user
      this.form.state = data.estado
      this.form.id = data.id
    },
    /* Guardar */
    onSave () {
      const me = this
      axios.post(laravelUrl + '/team/', {
        Name: me.form.nombre,
        PlayersCount: parseInt(me.form.quantity),
        DirectionTeam: me.form.direction,
        Stadium: me.form.stadium,
        idPlayersTeam: parseInt(me.form.idteam),
        state: me.form.state
      })
        .then((response) => {
          me.refresh()
          me.closeModal('save')
        })
        .catch((error) => {
          console.log(error)
        })
    },
    /* Guardar */
    onUpdate () {
      const me = this
      axios.put(laravelUrl + '/team/' + me.form.id, {
        Name: me.form.nombre,
        PlayersCount: parseInt(me.form.quantity),
        DirectionTeam: me.form.direction,
        Stadium: me.form.stadium,
        idPlayersTeam: parseInt(me.form.idteam),
        state: me.form.state })
        .then((response) => {
          me.refresh()
          me.closeModal('update')
        })
        .catch((error) => {
          // this.errorMessage = error.message;
          console.error('Error!', error)
        })
    },
    makeQueryParams (sortOrder, currentPage, perPage) {
      return sortOrder[0]
        ? {
          criterio: sortOrder[0] ? sortOrder[0].sortField : 'createdAt',
          order: sortOrder[0] ? sortOrder[0].direction : 'desc',
          page: currentPage,
          limit: this.perPage,
          search: this.search,
          columna: this.columna.value
        }
        : {
          criterio: sortOrder[0] ? sortOrder[0].sortField : 'createdAt',
          order: sortOrder[0] ? sortOrder[0].direction : 'desc',
          page: currentPage,
          limit: this.perPage,
          search: this.search,
          columna: this.columna.value
        }
    },
    changePageSizes (perPage) {
      this.perPage = perPage
      this.$refs.vuetable.refresh()
    },
    searchChange (val) {
      this.search = val.toLowerCase()
      this.$refs.vuetable.refresh()
    },
    onPaginationData (paginationData) {
      this.from = paginationData.from
      this.to = paginationData.to
      this.total = paginationData.total
      this.lastPage = paginationData.last_page
      this.items = paginationData.data
      this.$refs.pagination.setPaginationData(paginationData)
    },
    onChangePage (page) {
      this.$refs.vuetable.changePage(page)
    },
    changeTypeSearch (columna) {
      this.columna = columna
    },
    mensaje (mensaje) {
      this.$refs['modal-1'].show()
      this.form.nombre = mensaje
    },
    mensajeDespedida () {
      console.log('Antes de irnos del componente manda este mensaje')
    },
    refresh () {
      axios.get(laravelUrl + '/team/getTodos').then((response) => {
        this.teams = response.data
      })
    },
    editar (teamid) {
      axios.get(laravelUrl + '/team/' + teamid).then((response) => {
        this.teamindividual = response.data
        this.$v.$reset()
        this.$refs['modal-2'].show()
        this.form.id = this.teamindividual.id
        this.form.nombre = this.teamindividual.Name
        this.form.quantity = this.teamindividual.PlayersCount
        this.form.direction = this.teamindividual.DirectionTeam
        this.form.stadium = this.teamindividual.Stadium
        this.form.idteam = this.teamindividual.idPlayersTeam
        this.form.state = this.teamindividual.state
      })
    },
    eliminar (teamid) {
      axios.get(laravelUrl + '/team/' + teamid).then((response) => {
        this.teamindividual = response.data
        this.$v.$reset()
        this.$refs['modal-3'].show()
        this.form.id = this.teamindividual.id
        this.nameteam = this.teamindividual.Name
      })
    },
    Eliminar (teamid) {
      axios.delete(laravelUrl + '/team/' + teamid).then((response) => {
        this.refresh()
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
