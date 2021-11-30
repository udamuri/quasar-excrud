<template>
  <q-page>
    <div class="q-pa-md">
      <div class="q-table__container q-table--horizontal-separator column no-wrap q-table__card q-table--no-wrap" style="height: 400px;">
        <div class="q-table__top relative-position row items-center">
            <div class="q-table__control">
              <div class="q-table__title">
                <q-btn color="secondary" @click="formDialog = true" class="justify-end" label="Tambah" />
              </div>
            </div>
        </div>
        <div class="q-table__middle ">
            <table class="q-table">
              <thead>
                <tr class="q-tr">
                    <th class="text-left sortable">Title</th>
                    <th class="text-left sortable" width="10%">Action</th>
                </tr>
              </thead>
              <tbody class="q-virtual-scroll__content" id="qvs_1" tabindex="-1">
                <tr class="q-tr " v-for="(item, key) in datas" :key="key">
                  <td class="q-td ">{{item.title}}</td>
                  <td class="q-td text-left">
                    <q-btn color="red" label="Hapus" /> &nbsp;
                    <q-btn color="primary" label="Edit" />
                  </td>
                </tr>
              </tbody>
            </table>
        </div>
        <div class="q-table__bottom row items-center justify-end">
            <div class="q-table__separator col"></div>
            <div class="q-table__control"><span></span></div>
        </div>
      </div>

      <q-dialog 
        v-model="formDialog" 
        :maximized="maximizedToggle" >
        <q-card style="min-width: 350px;max-width:100%">
          <q-bar>
            <q-icon name="network_wifi"></q-icon>
            <q-space></q-space>

            <q-btn dense flat icon="close" v-close-popup>
              <q-tooltip>Close</q-tooltip>
            </q-btn>
          </q-bar>

          <q-card-section>
            <q-input
              filled
              v-model="form.title"
              class="q-mt-md"
              label="Title *"
            />
            <q-editor v-model="form.body" class="q-mt-md" min-height="5rem"></q-editor>
            <q-btn color="secondary" @click="postData" class="justify-end q-mt-md" label="Simpan" />
          </q-card-section>
        </q-card>
      </q-dialog>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import { api } from 'boot/axios'

export default defineComponent({
  name: 'PageIndex',
   data() {
    return {
      datas: [],
      formDialog: false,
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
    loadData() {
      api.get('posts')
        .then((response) => {
          this.datas = response.data.data;
          console.log(this.datas)
        })
        .catch(() => {
      });
    }, 
    postData() {
      let resp = api.post('/posts', this.form)
      .then(function (response) {
        if(response.data.status) {
          return response.data.status;
        }
      })
      .catch(function (error) {
        return error;
      });

      if(resp) {
        this.loadData()
        this.formDialog = false
        this.form.title = null
        this.form.body = null
      }
    }
  },
  mounted() {
    this.loadData()
  },
})
</script>
