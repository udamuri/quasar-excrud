<template>
  <q-page>
    <div class="q-pa-md">
      <q-table 
        title="Posts" 
        :rows="datas"
        :columns="columns"
        row-key="title" 
        :rows-per-page-options="[0]" >
        <template v-slot:top>
          <q-btn dense color="secondary" label="Add Posts" @click="add" no-caps></q-btn>
          <div class="q-pa-sm q-gutter-sm">
            <q-dialog 
              v-model="formDialog"  >
              <q-card style="min-width: 350px;max-width:100%">
                <q-bar>
                  <q-space></q-space>

                  <q-btn dense flat icon="close" v-close-popup>
                    <q-tooltip>Close</q-tooltip>
                  </q-btn>
                </q-bar>

                <q-card-section>
                  <div class="text-h6">Add new posts!</div>
                </q-card-section>

                <q-card-section>
                  <div class="row">
                    <div class="col-12">
                      <q-input
                        filled
                        v-model="form.title"
                        class="q-mt-md"
                        label="Title *"
                      />
                    </div>
                    <div class="col-12">
                      <q-editor v-model="form.body" class="q-mt-md" min-height="5rem"></q-editor>
                    </div>
                    <div class="col-12">
                      <q-btn color="secondary" @click="saveData" class="justify-end q-mt-md" label="Simpan" />
                    </div>
                  </div>
                </q-card-section>
              </q-card>
            </q-dialog>
          </div>
        </template>

        <template v-slot:body-cell-actions="props">
          <q-td :props="props">
            <q-btn color="blue" label="Update" @click="edit(props.row)" size=sm no-caps></q-btn>
            <q-btn color="red" label="Delete"  @click="deleteData(props.row.id)" size=sm no-caps></q-btn>
          </q-td>
        </template>
      </q-table>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import { api } from 'boot/axios'
import { Notify } from 'quasar'

export default defineComponent({
  name: 'PageIndex',
   data() {
    return {
      columns: [
        { name: 'title', align: 'left', label: 'Title', field: 'title' },
        //{ name: 'body', align: 'left', label: 'Body', field: 'body'},
        { name: 'actions', align: 'center', label: 'Action'},
      ],
      datas: [],
      formDialog: false,
      confirm: false,
      iId: null,
      maximizedToggle: true,
      form: {
        title: null,
        body: null,
      }
    };
  },
  methods: {
    formatPrice(value) {
      const price = value / 100;
      return price.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
    },
    add() {
      this.formDialog =  true
      this.iId = null
      this.form.title = null
      this.form.body = null
    },
    edit(item) {
      this.formDialog =  true
      this.iId = item.id
      this.form.title = item.title
      this.form.body = item.body
      console.log(item)
    }, 
    async loadData() {
      await api.get('posts')
        .then((response) => {
          console.log(response.data.data)
          this.datas = response.data.data;
        })
        .catch(() => {
      });
    }, 
    async saveData() {
      if(this.iId) {
        let res = await api.put('posts/' + this.iId, this.form)
        .then(function (response) {
          if(response.data.success) {
            return response.data.success;
          }
        })
        .catch(function (error) {
          return error;
        });
        if(res) {
          this.loadData();
          this.formDialog = false
          Notify.create({
            message: 'Update Post'
          })
        }
      } else {
        let res = await api.post('posts', this.form)
        .then(function (response) {
          if(response.data.success) {
            return response.data.success;
          }
        })
        .catch(function (error) {
          return error;
        });
        if(res) {
          this.loadData();
          this.formDialog = false
          Notify.create({
            message: 'Create New Posts'
          })
        }
      }
    },
    async deleteData(id) {
      await api.delete('posts/' + id)
        .then((response) => {
          this.loadData();
          Notify.create({
            message: 'Delete Posts'
          })
        })
        .catch(() => {
      });
    }, 
  },
  watch() {
    
  },
  mounted() {
    this.loadData()
  },
})
</script>
