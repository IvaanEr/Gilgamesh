<template>
<div class="content">
    <div class="container-fluid">
      <card>
        <template slot="header">
          <h4 class="card-title">Analize binary</h4>
        </template>
        <div class="row">
          <div class="col-md-8">
            <table-list :columns="tableColumns"
                        :values="tableValues">
            </table-list>
          </div>
          <div class="col-md-4">
            
          </div>
        </div>
      </card>
    </div>
</div>
</template>

<script>
import Card from 'src/components/UIComponents/Cards/Card.vue'
import TableList from 'src/components/Dashboard/Views/TableList.vue'

export default {
  name: 'analizebin',
  components: {
    Card,
    TableList
  },

  data () {
    return {
      hexa: '',
      ascii: '',
      tableColumns: ['Indice', 'Hexadecimal', 'Ascii'],
      tableValues: []
    }
  },

  props: {
    binary: null
  },

  methods: {
    hexdump: function (buffer, blockSize) {
      blockSize = blockSize || 16
      var lines = []
      var hex = '0123456789ABCDEF'
      for (var b = 0; b < buffer.length; b += blockSize) {
        var block = buffer.slice(b, Math.min(b + blockSize, buffer.length))
        var addr = ('0000' + b.toString(16)).slice(-4)
        var codes = block.split('').map(function (ch) {
          var code = ch.charCodeAt(0)
          return ' ' + hex[(0xF0 & code) >> 4] + hex[0x0F & code]
        }).join('')
        codes += '   '.repeat(blockSize - block.length)
        var chars = block.replace(/[\x00-\x1F\x20]/g, '.')
        chars += ' '.repeat(blockSize - block.length)
        lines.push(addr + ' ' + codes + '  ' + chars)
      }
      return lines.join('\n')
    },

    parse: function () {
      this.parsed = this.hexdump(this.binary, 16)
    // let's split this string
      // 4 chars from the "id" + space = 5
      // 16 pairs of 2 (hex) with a space betweenthem = 16*2 + 16
      var extract = 5 + (16*2) + 16
      var i = 0
      while(i < this.parsed.length) {
        var index = parseInt(this.parsed.substr(i, 5),16)
        var hex = this.parsed.substr(i+5, extract-5)
        var dec = this.parsed.substr(i+extract, 18)
        var raw = {indice: index, hexadecimal: hex, ascii: dec}
        this.tableValues.push(raw)
        this.hexa = this.hexa.concat(hex)
        this.ascii = this.ascii.concat(dec)
        var auxStr = this.parsed.substr(i,extract) + ' -> ' + this.parsed.substr(i+extract, 18)
        // this.result = this.result + auxStr + '\n'
        // console.log(this.parsed.substr(i,extract) + ' -> ' + this.parsed.substr(i+extract, 18))
        i = i + extract + 19 // 19 because of a space
      }
    },

},

  beforeMount: function () {
    if (!this.binary) {
      this.$router.push({
        name: 'openbin'
      })
    }
  },

  created: function () {
    this.parse()
  }
}
</script>

<style>

</style>

