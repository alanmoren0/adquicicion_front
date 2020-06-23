<template>
    <div class="container">

        <b-alert
            :show="dismissCountDown"
            dismissible
            :variant="mensaje.color"
            @dismissed="dismissCountDown=0"
            @dismiss-count-down="countDownChanged"
            >
            {{mensaje.texto}}
        </b-alert>

        <div class="tittle-log">
            <h2>Sistema de adquisiciones</h2>
            <h4>Entrada al sistema</h4>
        </div>

        <form action="#" v-on:submit.prevent="login()">
            <div class="form-group">
                <label for="email">Usuario</label>
                <input type="text" class="form-control" id="email" v-model="emailUser"
                placeholder="Ingrese usuario">
            </div>
            <div class="form-group">
                <label for="pwd">Password:</label>
                <input type="password" class="form-control" id="pwd" v-model="contrasena"
                placeholder="Contraseña">
            </div>
            <div class="btn-acces">
                <button type="submit" class="btn btn-success">Ingresar</button>
            </div>
            <div>
                <a class="btn btn-danger" href="recupera" role="button">Recuperar contraseña</a>
                <a class="btn btn-primary" href="registro" role="button">Registrarme</a>
            </div>

        </form>
    </div>
</template>
<script>
export default {
    data(){
        return{
            dismissSecs: 5,
            dismissCountDown: 0,
            mensaje: {color: 'success', texto: ''},
            emailUser:'',
            contrasena:'',
        }
    },

    methods:{
        login(){
            this.axios.post('login', {
            emailUser: this.emailUser,
            contrasena: this.contrasena
            }).then(res => {
                this.showAlert();
                this.mensaje.texto = this.emailUser ;
                this.mensaje.color = 'success';
            }).catch(err => {
                console.log(err)
                this.showAlert();
                this.mensaje.texto = err
                this.mensaje.color = 'danger';
            })
        }, countDownChanged(dismissCountDown) {
            this.dismissCountDown = dismissCountDown
        },
        showAlert() {
            this.dismissCountDown = this.dismissSecs
        },
    }
}
</script>