<template>
<div class="content">
    <div class="container-fluid">
      <card>
        <template slot="header">
          <h4 class="card-title">Open binary file</h4>
        </template>

        <div class="row">
          <div class="col-md-8">
              <b-form-file v-model="file"
                  :state="Boolean(file)"
                  placeholder="Choose a file...">
              </b-form-file>
          <br>
          <br>
          <b-button @click="open">Abrir</b-button>
          <br>
          <br>          
          <div v-if="is_open" class="alert alert-info">
              <span><b>El archivo se cargó correctamente</b></span>
          </div>
          <div v-if="is_file" class="alert alert-danger">
              <span><b>No hay archivo seleccionado</b></span>
          </div>

          </div>
        </div>
      </card>
    </div>
</div>
</template>

<script>
import Card from 'src/components/UIComponents/Cards/Card.vue'

export default {
  name: 'openbin',
  components: {
      Card
    },
  data () {
    return {
      items: [],
      perPage: 25,
      currentPage: 1,
      tableType: true,

      reader: null,
      file: null,

      response: null,
      parsed: null,
      is_open: false,
      is_file: false,
    }
  },

  methods: {
    table_type: function () {
      this.tableType = ! this.tableType
    },

    open: function (event) {
      if(!this.file){
        this.is_file = true
      }
      this.reader.readAsBinaryString(this.file)
      setTimeout(this.show, 1000)
    },



    show: function () {
      if (this.reader.readyState === 2) {
        // console.log('reader result: ' + this.reader.result)
        this.is_open = !this.is_open
        this.$router.push({
          name: 'analizebin',
          params: {
            binary: this.reader.result
          }
        })
      }
    }
  },

  
  created: function () {
    this.reader = new FileReader()
    this.reader.onload = function (event) {
      // console.log('onLoad: ' + event.target.result + '\ntype: ' + event.type)
    }
    this.reader.onloadend = function (event) {
      console.log('type: ' + event.type + '\nResult onloadend: ' + event.target.result)
      // alert('Se termino de leer')
      // this.is_open = !this.is_open
    }
  },

  watch: {
    file: function (val) {
      this.is_file = false
    }
  }
}
</script>