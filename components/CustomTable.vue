<template>
  <div>
    <v-data-table
      v-model="selected"
      :headers="tableHeaders"
      :items="tableData"
      :single-select="singleSelect"
      :single-expand="singleExpand"
      :expanded.sync="expanded"
      item-key="id"
      show-expand
      show-select
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Banks Table(Task)</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-switch
            v-model="singleSelect"
            label="Single select"
            class="pa-3"
          ></v-switch>
          <v-switch
            v-model="singleExpand"
            label="Single expand"
            class="pa-3"
          ></v-switch>
        </v-toolbar>
      </template>
      <template v-slot:expanded-item="{ headers, item }">
        <td :colspan="headers.length">
          UID is {{ item.uid }}
        </td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import axios from 'axios'
  export default {
    name: 'CustomTable',
    
    data () {
      return {
          expanded: [],
          selected: [],
          singleExpand: false,
          singleSelect: false,
          tableHeaders: [],
          tableData: [],
      } 
    },
    mounted() {
      axios.get('https://random-data-api.com/api/bank/random_bank?size=10',).then(response => {
        this.tableData = response.data;
        this.getTableHeaders()
      })           
    },
    
    methods: {
      getTableHeaders() {
        //Чтобы получить динамические заголовки сначала получим массив ключей одного из элементов таблицы, по которым будет заполняться таблица
        let headersValues = Object.keys(this.tableData[0]).filter(key => key!=="uid")

        //Копируем массив ключей, чтобы убрать подчёркивания и сделать первую букву большой для заполнения заголовков.
        let clone = headersValues.slice(0)
        let headersText = []
        clone.map(el => {
          el = el[0].toUpperCase() + el.slice(1)
          el = el.replace("_", " ")
          headersText.push(el)
        })

        //Заполняем массив headersText объектами
        for (let i = 0; i<headersValues.length; i+=1) {
          let obj = {}
          obj.text = headersText[i]
          obj.value = headersValues[i]
          this.tableHeaders.push(obj)
        }
      }
    }, 
  }
</script>