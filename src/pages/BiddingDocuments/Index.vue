<template>
    <q-page class="flex flex-center">
      <q-table color="secondary" 
        :loading="loading"
        :rows="docs"
        :columns="columns">
        <template #loading>
          <q-inner-loading showing color="secondary" />
        </template>
  
        <!-- <template v-slot:body="props">
          <q-tr :props="props" @click="onRowClick(props.row.link)">
            <q-td
              v-for="col in props.cols"
              :key="col.name"
              :props="props"
            >
              {{ col.value }}
            </q-td>
          </q-tr>
        </template> -->
        <template v-slot:body="props">
          <q-tr :props="props">
            <q-td
              v-for="col in props.cols"
              :key="col.name"
              :props="props"
            >
            <a :href="props.row.link" target="_blank">{{ col.value }}</a>
            </q-td>
          </q-tr>
        </template>
        
      </q-table>
    </q-page>
  </template>
  
  <script>
  import { defineComponent, ref } from 'vue'
  import axios from 'axios'
  
  export default defineComponent({
    name: 'IndexPage',
    setup () {
      
      const loading = ref(true)
      const docs = ref([])
  
      const columns = [
        {name: 'code', label: 'Code', field: 'code', align: 'left'},
        {name: 'description', label: 'Decription', field: 'description', align: 'left'}
      ]
  
      const config = {
        headers: {
          'X-API-KEY': 'eyJpdiI6IjYwRVBuRFRHN3Yydjg4VU0rV0tWUkE9PSIsInZhbHVlIjoiVG1FV0pmUFQvMkRBRkNaMHBBVjNwMXBWZXNZNHVybUYza1VQNkpUOHFSWW5MeGFlR0lyQXlrcmZBQ0VFeVlCMHBJc0ZrR2lWcjdCT21UVXI2enF0MGRKcW5HcGxXQ2NFWmRrc1QzVFNpeE09IiwibWFjIjoiYzIyYzUzY2YwOTA1ODU2ODAzNzQxZWY5MDE5OTgxMWUwYTQxYTA3MTZiMDc1NGM2NmM3MTkwNzRlM2I2YjI1ZiIsInRhZyI6IiJ9'
        }
      }
  
      axios.get('https://telemed.dohcsmc.site/nims/api/bidding-documents', config)
        .then(response => {
          docs.value = response.data.data.data
        })
        .finally(() => {
          loading.value = false
        })
  
      return {
        onRowClick: (col) => alert(`${col} clicked`),
        loading,
        docs,
        columns
      }
    }
  })
  </script>  