<template>
    <div class="animated fadeIn">
        <div class="row">
            <div class="col-6">
            <div class="col">
                <h1>Nama Supplier</h1>
                <table class="table table-bordered table-sm">
                    <thead>
                        <tr>
                        <th scope="col">ID</th>
                        <th scope="col">Nama</th>
                        <th scope="col">Alamat</th>
                        <th class="text-center">
                          <b-button @click="tambah()" variant="primary" class="btn-pill" size="sm">
                            <i class="fa fa-plus-o"></i>&nbsp;Tambah
                          </b-button>
                          </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(i, index) in dataPage.content" :key="index">
                            <th scope="row">{{i.id}}</th>
                            <td>{{i.nama}}</td>
                            <td>{{i.alamat.kota}}</td>
                            <td>
                                <b-button variant="default" @click="ubah(i)">Ubah</b-button> 
                                <b-button variant="danger" @click="hapus(i)">Hapus</b-button>
                            </td>
                        </tr>
                    </tbody>
                    </table>
                    </div>
            </div>
            <div class="col-6">
                <div class="col">
                    <h1>Masukkan Nama</h1>
                    <b-form-group id="fieldsetHorizontal"
                                    horizontal
                                    :label-cols="4"
                                    breakpoint="md"
                                    label="Nama"
                                    label-for="nama">
                        <b-form-input id="nama" v-model="vm.nama"></b-form-input>
                    </b-form-group>
                    <b-form-group id="fieldsetHorizontal"
                                    horizontal
                                    :label-cols="4"
                                    breakpoint="md"
                                    label="Alamat"
                                    label-for="alamat">
                        <v-select v-model="vm.alamat" label="kota" :options="listAlamat"></v-select>
                    </b-form-group>
                    <b-button variant="default" @click="bersihkan">Bersihkan</b-button>
                    <b-button variant="success" @click="simpan">Simpan</b-button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    const axios = require("axios");
    export default {
  data() {
    return {
      dataPage: {},
      vm : {},
      listAlamat: [],
      judul: "Ini halaman Supplier ya"
    };
  },
  methods : {
    bersihkan(){
      this.vm = {}
    },
    ubah(supplier){
      var r = confirm("Anda Yakin Ingin Ubah");
      if (r == true) {
        this.modalTitle = "Ubah nama";
        this.vm = JSON.parse(JSON.stringify(supplier));
      }
    },
    simpan(){
      var r = confirm("Simpan Success");
      if (r == true) {
        axios.post("/api/master/supplier", this.vm).then(() => this.refresh());
      }
    },
    hapus(supplier) {
      var r = confirm("Anda yakin untuk menghapus '" + supplier.nama + "'?");
      if (r == true) {
        axios.delete("/api/master/supplier/" + supplier.id).then(() => this.refresh());
      }
    },
    refresh(){
      axios.get("/api/master/supplier").then(response => (this.dataPage = response.data));
    }
  },
  mounted() {
    this.refresh();
    axios.get("/api/master/alamat-supplier/all").then(response => (this.listAlamat = response.data));
  }
};
</script>