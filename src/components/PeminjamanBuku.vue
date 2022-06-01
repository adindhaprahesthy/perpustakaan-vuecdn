<template>
    <main>
        <div class="container-fluid px-4">
            <h2 class="mt-4">Peminjaman Buku</h2>
            <ol class="breadcrumb mb-4">
                <li class="breadcrumb-item"><router-link to="/">Dashboard</router-link></li>
                <li class="breadcrumb-item active">Peminjaman Buku</li>
            </ol>

            <div class="card mb-4">
                <div class="card-header">
                    <i class="fas fa-table me-1"></i>
                    List Peminjaman Buku

                    <button @click="Add()" data-bs-toggle="modal" data-bs-target="#PeminjamanBukumodal" class="btn btn-primary btn-sm float-end"><i class="fas fa-plus fa-fw"></i> Tambah Peminjaman Buku</button>
                </div>
                <div class="card-body">
                    <table id="siswa" class="table table-responsive table-striped table-hover">
                        <thead>
                            <tr>
                                <th>NO</th>
                                <th>NAMA SISWA</th>
                                <th>TANGGAL PEMINJAMAN</th>
                                <th>TANGGAL PENGEMBALIAN</th>
                                <th>AKSI</th>
                            </tr>
                        </thead>
                       
                        <tbody>
                            <tr v-for="(lb, index) in list_transaksi" :key="lb">
                                <td>{{ index+1 }}</td>
                                <td>{{ lb.siswa.nama_siswa }}</td>
                                <td><span class="badge bg-dark" >{{ lb.siswa.kelas.nama_kelas+ ' '}}</span></td>
                                <td>{{ lb.tanggal_pinjam }}</td>
                                <td>{{ lb.tanggal_kembali }}</td>
                               
                                <td>
                                    <button class="btn btn-sm btn-info" @click="Detail(lb)" data-bs-toggle="modal" data-bs-target="#DetailPeminjamanBukumodal" ><i class="fas fa-list fa-fw"></i></button>&nbsp;
                                    <button class="btn btn-sm btn-success" @click="Return(lb.id_siswa)" data-bs-toggle="modal" data-bs-target="#PeminjamanBukumodal"><i class="fas fa-check fa-fw"></i></button>
                                </td>
                            </tr>
                            
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="DetailPeminjamanBukumodal" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg bg-primary">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">Data Peminjaman Buku</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <table class="table table-responsive table-stripped">
                            <thead>
                                <tr>
                                    <th>NO</th>
                                    <th>COVER</th>
                                    <th>NAMA BUKU</th>
                                    <th>PENGARANG</th>
                                </tr>
                            </thead>
 
                            <tbody>
                                <tr v-for="(detail, index) in list_detail_transaksi" :key="detail">
                                    <td>{{index+1}}</td>
                                    <td>
                                        <img v-if="detail.image !== null" :src="api_url2 + '/images/' + detail.image" width="150">
                                        <span v-else>No IMAGE</span>
                                    </td>
                                    <td>{{ detail.nama_buku }}</td>
                                    <td>{{ detail.pengarang }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <div class="modal fade" id="PeminjamanBukumodal" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-xl bg-primary">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">Data Peminjaman Buku</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="row">
                            <div class="col-md-3">
                                <div class="card">
                                    <div class="card-body">
                                         <div class="mb-3">
                                            <div class="form-group">
                                                <label for="id_student" class="form-label">Nama Siswa</label>
                                                <select class="form-control" v-model="id_siswa">
                                                    <option value="" disabled>-- Pilih Siswa --</option>
                                                    <option v-for="lb in list_siswa" :key="lb.id_siswa" :value="lb.id_siswa">{{ lb.nama_siswa }}</option>
                                                </select>
                                            </div>
                                        </div>
                                        <div class="mb-3">
                                            <label for="tanggal_pinjam" class="form-label">Tanggal Peminjaman Buku</label>
                                            <input type="date" class="form-control" id="tanggal_pinjam" v-model="tanggal_pinjam" placeholder="Tanggal pinjam">
                                        </div>
                                        <div class="mb-3">
                                            <label for="tanggal_kembali" class="form-label">Tanggal Pengembalian Buku</label>
                                            <input type="date" class="form-control" id="tanggal_kembali" v-model="tanggal_kembali" placeholder="Tanggal Kembali">
                                        </div>
                                    </div>
                                </div>
                               
                            </div>
                            <div class="col-md-9">
                                <div class="card">
                                    <div class="card-body">
                                        <button @click="addItem" class="btn btn-sm btn-primary text-white"><i class="mdi mdi-plus btn-icon-prepend"></i> Tambah Item</button>
                                        <br><br>
                                        <div class="row" v-for="(detail, counter) in detail_transaksi" :key="counter">
                                            <br><br>
                                            <div class="col-md-8">
                                                <div class="form-group">
                                                    <select class="form-control" v-model="detail.id_buku">
                                                        <option value="" disabled>-- Pilih Buku --</option>
                                                        <option v-for="lb in list_buku" :key="lb.id_buku" :value="lb.id_buku">{{ lb.nama_buku }} - {{ lb.pengarang }}</option>
                                                    </select>
                                                </div>
                                            </div>
                                            <div class="col-md-3">
                                                <div class="form-group">
                                                    <input class="form-control" placeholder="Qty" type="number" v-model="detail.qty"> 
                                                </div>
                                            </div>
                                            <div class="col-md-1">
                                                <button class="btn btn-danger btn-sm" @click="deleteItem(counter)"> - </button>
                                            </div>
                                        </div>
                                    </div>
                                    
                                </div>
                                
                            </div>
                        </div>
                        
                    </div>
                    <div class="modal-footer">
                        <button @click="Save()" class="btn btn-block btn-lg btn-success">SUBMIT</button>
                    </div>
                </div>
            </div>
        </div>

    </main>
</template>
<script>
module.exports = {
    data : function(){
        return {
            id_siswa: "",
            tanggal_pinjam: "",
            tanggal_kembali: "",
            list_transaksi: [],
            list_detail_transaksi: [],
            //for transaction form
            list_buku: [],
            list_siswa: [],
            detail_transaksi: [],
        }
    },
    methods: {
        addItem(){
            this.detail_transaksi.push({
                id_buku: '',
                qty: '',
            })
        },
        deleteItem(counter){
            this.detail_transaksi.splice(counter,1);
        },
        getBook: function(){
            //ambil data buku untuk dropdown
            let token = {
                headers : { 
                    "Authorization" : "Bearer " + this.$cookies.get("Authorization")
                }
            }
            axios.get(base_url + "/Bookmodel", token)
            .then(response => {
                this.list_buku = response.data;
            })
        },
        getStudent: function(){
            //ambil data student untuk dropdown
            let token = {
                headers : { 
                    "Authorization" : "Bearer " + this.$cookies.get("Authorization")
                }
            }
            axios.get(base_url + "/Siswamodel", token)
            .then(response => {
                this.list_siswa = response.data;
            })
        },
        getData: function(){
            let token = {
                headers : { 
                    "Authorization" : "Bearer " + this.$cookies.get("Authorization")
                }
            }
            axios.get(base_url + "/PeminjamanBukumodel", token)
            .then( response => {
                console.log(response.data)
                this.list_transaksi = response.data;
            })
        },
        // statusCheck: function(date_of_returning){
        //     const status = moment().isBefore(moment(date_of_returning))
        //     if(status){
        //         return true
        //     } else {
        //         return false
        //     }
        // },
        Add: function() {
            this.id_siswa = ""
            this.tanggal_pinjam = ""
            this.tanggal_kembali = ""

            this.getBook()
            this.getSiswa()
        },
        Detail: function(data) {
            this.id_siswa = data.id_siswa
            this.tanggal_pinjam = data.tanggal_pinjam
            this.tanggal_kembali = data.tanggal_kembali
            //get detail
            let token = {
                headers : { "Authorization" : "Bearer " + this.$cookies.get("Authorization")}
            }
            axios.get("/DetailPeminjamanBukumodel/" + data.id_peminjaman_buku, token)
            .then( response => {
                this.list_detail_transaksi = response.data
            })
        },
        Save: function() {
            let token = {
                headers : { 
                    "Authorization" : "Bearer " + this.$cookies.get("Authorization")
                }
            }
            
            let form = {
                "id_siswa": this.id_siswa,
                "tanggal_pinjam": this.tanggal_pinjam,
                "tanggal_kembali": this.tanggal_kembali,
                "detail": this.detail_transaksi
            }
            axios.post(base_url + '/PeminjamanBukumodel', form, token)
            .then( response => {
                Swal.fire({
                    title: 'Success!',
                    text: response.data.message,
                    icon: 'success',
                    confirmButtonText: 'OK'
                })
            })
            this.getData()
        },
    },
    mounted(){
        this.getData()
    },
}
</script>