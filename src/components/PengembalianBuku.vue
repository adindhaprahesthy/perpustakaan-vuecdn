<template>
    <div>
       <div class="container-fluid px-4">
            <h1 class="mt-4">Pengembalian Buku</h1>
            <ol class="breadcrumb mb-4">
                <li class="breadcrumb-item"><a href="#">Dashboard</a></li>
                <li class="breadcrumb-item active">Pengembalian Buku</li>
            </ol>

            <div class="card mb-4">
                <div class="card-header">
                    <i class="fas fa-table me-1"></i>
                    List Pengembalian Buku

                    <button @click="Add()" data-bs-toggle="modal" data-bs-target="#PengembalianBukumodal" class="btn btn-primary btn-sm float-end"><i class="fas fa-plus fa-fw"></i> Tambah Pengembalian Buku</button>
                </div>
                <div class="card-body">
                    <table id="datatablesSimple" class="table table-hover table-striped">
                        <thead>
                            <tr>
                                <th>Id Pengembalian Buku</th>
                                <th>Id Peminjaman Buku</th>
                                <th>Tanggal Pengembalian</th>
                                <th>Denda</th>
                                <th>Aksi</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="lb in list_pengembalian_buku" :key="lb">
                                <td>{{ lb.id_pengembalian_buku }}</td>
                                <td>{{ lb.id_peminjaman_buku }}</td>
                                <td>{{ lb.tanggal_pengembalian }}</td>
                                <td>{{ lb.denda }}</td>
                                <td>
                                    <button class="btn btn-info" @click="Edit(lb)" data-bs-toggle="modal" data-bs-target="#PengembalianBukumodal" ><i class="fas fa-pencil-alt fa-fw"></i></button>
                                    <button class="btn btn-danger" @click="Delete(lb.id_pengembalian_buku)"><i class="fas fa-trash-alt fa-fw"></i></button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="PengembalianBukumodal" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">Data Pengembalian Buku</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="mb-3">
                            <label for="id_peminjaman_buku" class="form-label">Id Peminjaman Buku</label>
                            <input type="text" class="form-control" id="id_peminjaman_buku" v-model="id_peminjaman_buku" placeholder="Id Peminjaman Buku">
                        </div>

                        <div class="mb-3">
                            <label for="tanggal_pengembalian" class="form-label">Tanggal Pengembalian</label>
                            <input type="date" class="form-control" id="tanggal_pengembalian" v-model="tanggal_pengembalian" placeholder="Tanggal Pengembalian">
                        </div>

                        <div class="mb-3">
                            <label for="denda" class="form-label">Denda</label>
                            <input type="text" class="form-control" id="denda" v-model="denda" placeholder="Denda">
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button type="button" class="btn btn-primary" @click="Save()" data-bs-dismiss="modal">Submit</button>
                    </div>
                </div>
            </div>
        </div>
</template>
<script>
module.exports = {
     //state
     data : function(){
         return {
            id_pengembalian_buku:"",
            id_peminjaman_buku:"",
            tanggal_pengembalian:"",
            denda:"",
            aksi:"",
            list_pengembalian_buku: [],
         }
     },
     methods: {
        getData: function(){
            //mapping header token
            let config = {
            headers : {
              "Authorization" : "Bearer " + this.$cookies.get('Authorization')}
            }
            axios.get(api_url + "/PengembalianBukumodel" , config)
            .then( response => {
                this.list_pengembalian_buku = response.data;
            })
        },
        Add: function() {
            this.id_pengembalian_buku = ""
           this.id_peminjaman_buku = ""
           this.tanggal_pengembalian = ""
           this.denda = ""
           this.aksi = "insert" 
        },
        Edit: function(lb){
            this.id_pengembalian_buku=lb.id_pengembalian_buku
            this.id_peminjaman_buku=lb.id_peminjaman_buku
            this.tanggal_pengembalian=lb.tanggal_pengembalian
            this.denda=lb.denda
            this.aksi ="update"
    },
        Save: function() {
            //mapping header token
            let config = {
                headers : { 
                    "Authorization" : "Bearer " + this.$cookies.get("Authorization")
                }
            }

            //mapping data
            let form  = {
                //backend       //state
                'id_peminjaman_buku': this.id_peminjaman_buku,
                'tanggal_pengembalian': this.tanggal_pengembalian,
                'denda' : this.denda
            }

            if(this.action === 'insert'){ //POST

                axios.post(api_url + '/PengembalianBukumodel', form, config)
                .then( response => {
                   Swal.fire({
                        title: 'Success!',
                        text: response.data.message,
                        icon: 'success',
                        confirmButtonText: 'ok'
                    })
                })

            } else { //PUT
                axios.put(api_url + '/PengembalianBukumodel/' + this.id_pengembalian_buku, form, config)
                .then( response => {
                    Swal.fire({
                        title: 'Success!',
                        text: response.data.message,
                        icon: 'success',
                        confirmButtonText: 'ok'
                    })
                })
            }

            this.getData()
        },
        Delete: function(id_book_return){
            //mapping header token
            let config = {
                headers : { "Authorization" : "Bearer " + this.$cookies.get("Authorization")}
            }

            Swal.fire({
                title: 'Hapus Data Pengembalian Buku',
                text: 'Apakah anda yakin ingin menghapus data ?',
                icon: 'warning',
                showDenyButton: true,
                showCancelButton: false,
                confirmButtonText: 'Yes',
                denyButtonText: `Cancel`,
            }).then((result) => {
                if (result.isConfirmed) {
                     axios.delete(api_url + '/PengembalianBukumodel/' + id_pengembalian_buku, config)
                    .then( response => {

                        Swal.fire({
                            title: 'Success !',
                            text: response.data.message,
                            icon: 'success',
                            confirmButtonText: 'ok'
                        })

                        this.getData()
                    })

                    //Swal.fire('Saved!', '', 'success')
                } else if (result.isDenied) {
                    Swal.fire({
                        title: 'Cancel !',
                        text: 'Data is not deleted',
                        icon: 'error',
                        confirmButtonText: 'ok'
                    })
                }
            })

        }
    },
    mounted() {
        this.getData()
    },
}
</script>