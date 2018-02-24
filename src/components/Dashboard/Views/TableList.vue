<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-12">
          <card>
            <template slot="header">
              <h4 class="card-title">{{title}}</h4>
              <p class="card-category">{{subtitle}}</p>
            </template>
            <div class="table-responsive">
              <l-table class="table-hover table-striped"
                       :columns="myColumns"
                       :data="myValues">
              </l-table>
            </div>
          </card>

        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import LTable from 'src/components/UIComponents/Table.vue'
  import Card from 'src/components/UIComponents/Cards/Card.vue'

  export default {
    components: {
      LTable,
      Card
    },
    data () {
      return {
        myColumns: this.columns,
        myValues: this.values
      }
    },
    props: {
      columns: null,
      values: null,
      title: null,
      subtitle: null
    }
  }
</script>
<style>
</style>
