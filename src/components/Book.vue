<template>
<div>
    <div class="container-fluid px-4">
                        <h1 class="mt-4">Buku</h1>
                        <ol class="breadcrumb mb-4">
                            <li class="breadcrumb-item"><a href="#">Dashboard</a></li>
                            <li class="breadcrumb-item active">Buku</li>
                        </ol>
                        
                        <div class="card mb-4">
                            <div class="card-header">
                                <i class="fas fa-table me-1"></i>
                                List Buku
                                <button @click="Add()" data-bs-toggle="modal" data-bs-target="#Bukumodal" class="btn btn-primary btn-sm float-end"><i class="fas fa-plus fa-fw"></i>Tambah Buku</button>
                            </div> 
                            <div class="card-body">
                                <table id="datatablesSimple" class="table table-hover table-striped">
                                    <thead>
                                        <tr>
                                            <th>Id Buku </th>
                                            <th>Cover Buku </th>
                                            <th>Nama Buku</th>
                                            <th>Pengarang</th>
                                            <th>Deskripsi</th>
                                            <th>Aksi</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="lb in list_buku" :key="lb">
                                            <td>{{ lb.id_buku }}</td>
                                            <td><img :src="api_url2 + '/images/' + lb.image" width="150"></td>
                                            <td>{{ lb.nama_buku }}</td>
                                            <td>{{ lb.pengarang }}</td>
                                            <td>{{ lb.deskripsi }}</td>
                                            <td>
                                                <button class="btn btn-warning" @click="Edit(lb)" data-bs-toggle="modal" data-bs-target="#CoverBukumodal" ><i class="fas fa-image fa-fw"></i></button>
                                                <button class="btn btn-info" @click="Edit(lb)" data-bs-toggle="modal" data-bs-target="#Bukumodal"><i class="fas fa-pencil-alt fa-fw"></i></button>
                                                <button class="btn btn-danger" @click="Delete(lb.id_buku)"><i class="fas fa-trash fa-fw"></i></button>

                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                   <!-- Modal -->
                    <div class="modal fade" id="Bukumodal" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
                    <div class="modal-dialog modal-lg">
                        <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="staticBackdropLabel">Data Buku</h5>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body">
                            <div class="mb-3">
                            <label for="nama_buku" class="form-label">Nama Buku</label>
                            <input type="text" class="form-control" id="nama_buku" v-model="nama_buku" placeholder="Nama Buku">
                            </div>

                            <div class="mb-3">
                            <label for="pengarang" class="form-label">Pengarang</label>
                            <input type="text" class="form-control" id="pengarang" v-model="pengarang" placeholder="Pengarang">
                            </div>

                            <div class="mb-3">
                            <label for="deskripsi" class="form-label">Deskripsi</label>
                            <textarea class="form-control" id="deskripsi" v-model="deskripsi" rows="3"></textarea>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" class="btn btn-primary" @click="Save()">Save</button>
                        
    </div>
  </div>
</div>
</div>
<!-- Modal -->
 <div class="modal fade" id="CoverBukumodal" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">Cover Buku</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">

                        <div class="mb-3">
                            <label for="cover_buku" class="form-label">Cover Buku</label>
                            <input type="file" class="form-control" id="cover_buku" @change="uploadCover($event)">
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button type="button" class="btn btn-primary" @click="Upload(id_buku)" data-bs-dismiss="modal">Submit</button>
                    </div>
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
            id_buku:"",
            nama_buku:"",
            pengarang:"",
            deskripsi:"",
            aksi:"",
            list_buku: [],
         }
     },
     methods: {
        getData: function(){
            //mapping header token
            let config = {
            headers : {
              "Authorization" : "Bearer " + this.$cookies.get('Authorization')}
            }
            axios.get(api_url + "/Bukumodel" , config)
            .then( response => {
                this.list_buku = response.data;
            })
        },
        Add: function() {
           this.id_buku = ""
           this.nama_buku = ""
           this.pengarang = ""
           this.deskripsi = ""
           this.aksi = "insert" 
        },
        Edit: function(lb){
            this.id_buku=lb.id_buku
            this.nama_buku=lb.nama_buku
            this.pengarang=lb.pengarang
            this.deskripsi=lb.deskripsi
            this.aksi="update"
        },
        uploadCover: function(e){
            this.cover_buku = e.target.files[0]
        },
        Upload: function(id_buku){
            let config = {
                headers : { 
                    "Authorization" : "Bearer " + this.$cookies.get("Authorization"), 
                    'Content-Type' : 'multipart/form-data',
                }
            }

            let form  = new FormData()
            form.append("cover_buku", this.cover_buku)

            axios.post(api_url + '/Bukumodel/UploadCoverBuku/'+ id_buku, form, config)
            .then( response => {
                Swal.fire({
                    title: 'Success!',
                    text: response.data.message,
                    icon: 'success',
                    confirmButtonText: 'ok'
                })

                this.getData()
            })

        },
    Save: function() {
            //mapping header token
            let config = {
                headers : { "Authorization" : "Bearer " + this.$cookies.get('Authorization')}
            }

            //mapping data
            let form = {
                //backend    //state
                'nama_buku' : this.nama_buku,
                'pengarang' : this.pengarang,
                'deskrispi' : this.deskripsi,
                'cover_buku' : this.cover_buku
            }
            if(this.aksi === 'insert'){ //post data
                axios.post(api_url + '/Bukumodel', form, config)
                .then( response => {
                    
                   Swal.fire({
                        title: 'Success!',
                        text: response.data.message,
                        icon: 'success',
                        confirmButtonText: 'ok'
                    })
                })

            }else{ //put
                axios.post(api_url + '/Bukumodel/'+ this.id_buku, form, config)
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
                    
        Delete: function(id_buku){
            //mapping header token
            let config = {
                headers : { "Authorization" : "Bearer " + this.$cookies.get('Authorization')}
            }

            
            Swal.fire({
                title: 'Hapus Data Buku',
                text: 'Aapakah anda yakin ingin menghapus data ?',
                icon: 'warning',
                showDenyButton: true,
                showCancelButton: false,
                confirmButtonText: 'Yes',
                denyButtonText: `Cancel`,
            }).then((result) => {
                if (result.isConfirmed) {
                     axios.delete(api_url + '/Bukumodel/' + id_buku, config)
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
                        text: 'Data tidak terhapus',
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