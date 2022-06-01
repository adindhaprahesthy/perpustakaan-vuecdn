<template>
    <div>
       <div class="container-fluid px-4">
            <h1 class="mt-4">Siswa</h1>
            <ol class="breadcrumb mb-4">
                <li class="breadcrumb-item"><a href="#">Dashboard</a></li>
                <li class="breadcrumb-item active">Siswa</li>
            </ol>

            <div class="card mb-4">
                <div class="card-header">
                    <i class="fas fa-table me-1"></i>
                    List Siswa

                    <button @click="Add()" data-bs-toggle="modal" data-bs-target="#Siswamodal" class="btn btn-primary btn-sm float-end"><i class="fas fa-plus fa-fw"></i> Tambah Siswa</button>
                </div>
                <div class="card-body">
                    <table id="datatablesSimple" class="table table-hover table-striped">
                        <thead>
                            <tr>
                                <th>Id Siswa</th>
                                <!-- <th>Foto Siswa</th> -->
                                <th>Nama Siswa</th>
                                <th>Tanggal Lahir</th>
                                <th>Gender</th>
                                <th>Alamat</th>
                                <th>Id Kelas</th>
                                <th>Aksi</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="lb in list_siswa" :key="lb">
                                <td>{{ lb.id_siswa }}</td>
                                <!-- <td><img :src="api_url2 + '/images/' + lb.image" width="150"></td> -->
                                <td>{{ lb.nama_siswa }}</td>
                                <td>{{ lb.tanggal_lahir }}</td>
                                <td><span class="badge bg-info" v-if="lb.gender === 'L'">Laki-Laki</span>
                                    <span class="badge bg-warning" v-if="lb.gender === 'P'">Perempuan</span>
                                    </td>
                                <td>{{ lb.alamat }}</td>
                                <td><span class="badge bg-dark" >{{ lb.nama_kelas + ' '}}</span></td>
                                <td>
                                    <button class="btn btn-warning" @click="Edit(lb)" data-bs-toggle="modal" data-bs-target="#CoverSiswamodal" ><i class="fas fa-image fa-fw"></i></button>
                                    <button class="btn btn-info" @click="Edit(lb)" data-bs-toggle="modal" data-bs-target="#Siswamodal" ><i class="fas fa-pencil-alt fa-fw"></i></button>
                                    <button class="btn btn-danger" @click="Delete(lb.id_siswa)"><i class="fas fa-trash-alt fa-fw"></i></button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="Siswamodal" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">Data Siswa</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="mb-3">
                            <label for="nama_siswa" class="form-label">Nama Siswa</label>
                            <input type="text" class="form-control" id="nama_siswa" v-model="nama_siswa" placeholder="Nama Siswa">
                        </div>

                        <div class="mb-3">
                            <label for="tanggal_lahir" class="form-label">Tanggal Lahir</label>
                            <input type="date" class="form-control" id="tanggal_lahir" v-model="tanggal_lahir" placeholder="Tanggal Lahir">
                        </div>

                        <div class="mb-3">
                            <label for="gender" class="form-label">Gender</label>
                            <input type="text" class="form-control" id="gender" v-model="gender" placeholder="Gender">
                        </div>

                        <div class="mb-3">
                            <label for="alamat" class="form-label">Alamat</label>
                            <textarea class="form-control" id="alamat" v-model="alamat" rows="3"></textarea>
                        </div>

                         <div class="mb-3">
                            <label for="id_kelas" class="form-label">Id Kelas</label>
                            <input type="text" class="form-control" id="id_kelas" v-model="id_kelas" placeholder="Id Kelas">
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button type="button" class="btn btn-primary" @click="Save()" data-bs-dismiss="modal">Submit</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="CoverSiswamodal" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <!-- <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="staticBackdropLabel">Foto Siswa</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">

                        <div class="mb-3">
                            <label for="cover_siswa" class="form-label">Foto Siswa</label>
                            <input type="file" class="form-control" id="cover_siswa" @change="uploadCover($event)">
                        </div>

                    </div> -->
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button type="button" class="btn btn-primary" @click="Upload(id_siswa)" data-bs-dismiss="modal">Submit</button>
                    </div>
                </div>
            </div>
        </div>
    <!-- </div> -->
</template>
<script>
module.exports = {
     //state
     data : function(){
         return {
            id_siswa:"",
            nama_siswa:"",
            tanggal_lahir:"",
            gender: "",
            alamat:"",
            id_kelas:"",
            aksi:"",
            list_siswa: [],
         }
     },
     methods: {
        getData: function(){
            //mapping header token
            let config = {
            headers : {
              "Authorization" : "Bearer " + this.$cookies.get('Authorization')}
            }
            axios.get(api_url + "/Siswamodel" , config)
            .then( response => {
                this.list_siswa = response.data;
            })
        },
        Add: function() {
           this.id_siswa = ""
           this.nama_siswa = ""
           this.tanggal_lahir = ""
           this.gender = ""
           this.alamat = ""
           this.id_kelas= ""
           this.aksi = "insert" 
        },
        Edit: function(lb){
            this.id_siswa=lb.id_siswa
            this.nama_siswa=lb.nama_siswa
            this.tanggal_lahir=lb.tanggal_lahir
            this.gender=lb.gender
            this.alamat=lb.alamat
            this.id_kelas=lb.id_kelas
            this.aksi="update"
        },
        uploadCover: function(e){
            this.cover_siswa = e.target.files[0]
        },
    Upload: function(id){
            let config = {
                headers : { 
                    "Authorization" : "Bearer " + this.$cookies.get("Authorization"), 
                    'Content-Type' : 'multipart/form-data',
                }
            }

            let form  = new FormData()
            form.append("cover_siswa", this.cover_siswa)

            axios.post(api_url + '/Siswamodel/UploadCoverSiswa/'+ id_siswa, form, config)
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
                headers : { 
                    "Authorization" : "Bearer " + this.$cookies.get("Authorization")
                }
            }

            //mapping data
            let form  = {
                //backend       //state
                'nama_siswa': this.nama_siswa,
                'tanggal_lahir': this.tanggal_lahir,
                'gender': this.gender,
                'alamat' : this.alamat,
                'id_kelas'  :this.id_kelas,
                'cover_siswa' : this.cover_siswa
            }

            if(this.aksi === 'insert'){ //POST

                axios.post(api_url + '/Siswamodel', form, config)
                .then( response => {
                   Swal.fire({
                        title: 'Success!',
                        text: response.data.message,
                        icon: 'success',
                        confirmButtonText: 'ok'
                    })
                })

            } else { //PUT
                axios.put(api_url + '/Siswamodel/' + this.id_siswa, form, config)
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
        Delete: function(id_siswa){
            //mapping header token
            let config = {
                headers : { "Authorization" : "Bearer " + this.$cookies.get("Authorization")}
            }

            Swal.fire({
                title: 'Hapus Data Siswa',
                text: 'Apakah anda yakin ingin menghapus data ?',
                icon: 'warning',
                showDenyButton: true,
                showCancelButton: false,
                confirmButtonText: 'Yes',
                denyButtonText: `Cancel`,
            }).then((result) => {
                if (result.isConfirmed) {
                     axios.delete(api_url + '/Siswamodel/' + id_siswa, config)
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