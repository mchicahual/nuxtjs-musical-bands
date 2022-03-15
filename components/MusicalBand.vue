<template>
  <div>
    <b-container class="mt-5">
      <b-card>
        <b-card-header>
          <h3>Musical Bands</h3>
          <div>
            <b-row class="my-1">
              <b-col sm="10">
                 <b-form-input v-model="searchName" placeholder="Enter name musical bands"></b-form-input>
              </b-col>
              <b-col sm="2">
                <b-button variant="primary" @click="toggleBusy">Search Bands</b-button> 
              </b-col>
            </b-row>
          </div>
        </b-card-header>
        <b-card-body>
          <b-table striped hover responsive :items="musicalBands.canciones" :fields="fields" :busy="isBusy" show-empty>
            <template #table-busy>
                <div class="text-center text-danger my-2">
                  <b-spinner class="align-middle"></b-spinner>
                  <strong>Loading...</strong>
                </div>
            </template>
            <template #empty="scope">
              <h4>{{ scope.emptyText }}</h4>
            </template>
          </b-table>
        </b-card-body>
      </b-card>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      searchName: '',
      isBusy: false,
      musicalBands: [],
      fields: [
          {
            key: 'nombre_tema',
            label: 'Nombre Cancion',
            sortable: true
          },
          {
            key: 'nombre_album',
            label: 'Nombre Album',
            sortable: true
          },
          {
            key: 'preview_url',
            label: 'Url Preview',
            sortable: false
          },
          {
            key: 'precio.valor',
            label: 'Precio',
            sortable: true
          },
          {
            key: 'fecha_lanzamiento',
            label: 'Fecha de lanzamiento',
            sortable: false
          },
        ],
    }
  },
  methods: {
      toggleBusy() {
        this.myProvider();
      },
      async myProvider() {
        this.isBusy = true
        try {
          const res = await axios.get(`http://localhost:3001/api/v1/track/search_tracks?name=${this.searchName}`);
          this.isBusy = false;
          this.musicalBands = res.data;
        } catch (error) {
          this.isBusy = false;
          return [];
        }
    }
  },
  async created(){
    await this.myProvider();
  }
}
</script>