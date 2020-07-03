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

         <form action="reg_user.html" @submit.prevent="checkFormProveedor()" 
        v-if="tipoRegistro==false">
            <div class="form-group">
                
                <div class="form-check">
                    <label class="form-check-label">
                        <input type="radio" class="form-check-input" name="tipouser" value="Proveedor"
                        v-model="proveedor.typeUser"
                        @click="tipoRegistro=false">Proveedor
                    </label>
                </div>
                <div class="form-check">
                    <label class="form-check-label">
                        <input type="radio" class="form-check-input" name="tipouser"  value="area" 
                        v-model="proveedor.typeUser"
                        @click="tipoRegistro=true">Área o departamento
                    </label>
                </div>
                <div class="form-group">
                    <label for="cedula">Número de Cédula</label>
                    <input type="text" class="form-control" id="cedula"
                    v-model="proveedor.numCedula">
                </div>
                <div class="form-group">
                    <label for="rfc">RFC</label>
                    <input type="text" class="form-control" id="rfc"
                    v-model="proveedor.rfc" >
                </div>
                <div class="form-group">
                    <label for="email">Correo electrónico</label>
                    <input type="email" class="form-control" id="email" 
                    v-model="proveedor.email">
                </div>
                <div class="form-group">
                    <label for="pass">Contraseña</label>
                    <input type="password" class="form-control" id="pass" 
                    v-model="proveedor.password">
                </div>
                <div class="form-group">
                    <label for="passc">Confirma contraseña</label>
                    <input type="password" class="form-control" id="passc"
                    v-model="confirmaPass">
                </div>
                <div class="form-group">
                    <label for="nombre">Nombre del usuario</label>
                    <input type="text" class="form-control" id="nombre" 
                    v-model="proveedor.nombreUser">
                </div>
                <button type="submit" class="btn btn-success">Enviar</button>
                <button type="cancel" class="btn btn-danger">Cancelar</button>
            </div>
        </form>

        <form action="reg_user.html" @submit.prevent="checkFormArea()"
        v-if="tipoRegistro==true">
            <div class="form-group">
                
                <div class="form-check">
                    <label class="form-check-label">
                        <input type="radio" class="form-check-input" name="tipouser" value="Proveedor"
                        v-model="area.typeUser"
                        @click="tipoRegistro=false">Proveedor
                    </label>
                </div>
                <div class="form-check">
                    <label class="form-check-label">
                        <input type="radio" class="form-check-input" name="tipouser"  value="Area" 
                        v-model="area.typeUser"
                        @click="tipoRegistro=true">Área o departamento
                    </label>
                </div>
                <div class="form-group">
                    <label for="cct">Clave de centro de trabajo</label>
                    <input type="text" class="form-control" id="cct" 
                    v-model="area.cv_cct">
                </div>
                <div class="row">
                    <div class="col">
                        <label for="municipio">Municipio</label> :
                        <select name="municipio" id ="municipio" class="form-control" 
                        v-model="area.municipio"
                        @blur="obtenerLocalidades(area.municipio)">
                            <option value="">-- Municipio --</option>
                            <option v-for="mun in arrMunicipio">
                                {{mun.nombreMunicipio}}</option>
                        </select>
                        
                    </div>
                    <div class="col">
                        <label for="loca">Localidad</label> :
                        <select name="loca" id = "loca" class="form-control" 
                        v-model="area.localidad">
                            <option value="">-- LOCALIDAD --</option>
                            <option v-for="loc in arrColonia">
                                {{loc.nombreLocalidad}}</option>
                        </select>
                    </div> 
                </div>
                <div class="form-group">
                    <label for="email">Correo electrónico</label>
                    <input type="email" class="form-control" id="email" 
                    v-model="area.email">
                </div>
                <div class="form-group">
                    <label for="pass">Contraseña</label>
                    <input type="password" class="form-control" id="pass" 
                    v-model="area.password">
                </div>
                <div class="form-group">
                    <label for="passc">Confirma contraseña</label>
                    <input type="password" class="form-control" id="passc"
                    v-model="confirmaPassArea">
                </div>
                <div class="form-group">
                    <label for="nombre">Nombre del usuario</label>
                    <input type="text" class="form-control" id="nombre" 
                    v-model="area.nombreUser">
                </div>
                <button type="submit" class="btn btn-success">Enviar</button>
                <button type="cancel" class="btn btn-danger">Cancelar</button>
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
            arrLocalidadMunicipio:{},
            arrMunicipio:{},
            arrColonia:{},
            errors:[],
            col:[],
            mensaje: {color: 'success', texto: ''},
            area:{
                cv_cct:'',municipio:'',localidad:'',
                email:'',
                typeUser:'',nombreUser:'',password:''
            },
            proveedor:{
                numCedula:'',rfc:'',
                email:'',
                typeUser:'',nombreUser:'',password:''
            },
            usuario:{email:'',
                tipo:'',nombreUser:'',contrasena:''},
            proovedores:[{
                numeroCedula:'123456',rfc:'ABCD1234'
            }
            ],
            departamento:[{
                numeroCedula:'654321',rfc:'DCBA4321'
            }],
            tipoRegistro:false,
            actualPass:'',
            confirmaPass:'',
            confirmaPassArea:'',
        }
    },created(){
        this.obtenerMunicipio();
        console.log(this.tipoRegistro);
    },
    methods:{
        registrarUsuario(){
            console.log(this.proveedor.typeUser);

            this.axios.post('signup', this.proveedor)
            .then(res => {
                // Alerta de mensaje
                console.log(res.data);
                this.showAlert();
                this.mensaje.texto = this.proveedor.typeUser;
                this.mensaje.color = 'success';
            })
            .catch( e => {
                console.log(e.response);
                if(e.response.data.error.errors.nombre.message){
                    this.mensaje.texto = e.response.data.error.errors.nombre.message;
                }else{
                    this.mensaje.texto = 'Error del sistema';
                }

                  // Alerta de mensaje
                this.mensaje.color = 'danger';
                this.showAlert();
            })
        },
        registrarUsuarioArea(){
            console.log(this.area.typeUser);

            this.axios.post('signup-area', this.area)
            .then(res => {
                // Alerta de mensaje
                console.log(res.data);
                this.showAlert();
                this.mensaje.texto = this.area.typeUser;
                this.mensaje.color = 'success';
            })
            .catch( e => {
                console.log(e.response);
                if(e.response.data.error.errors.nombre.message){
                    this.mensaje.texto = e.response.data.error.errors.nombre.message;
                }else{
                    this.mensaje.texto = 'Error del sistema';
                }

                  // Alerta de mensaje
                this.mensaje.color = 'danger';
                this.showAlert();
            })
        },
        countDownChanged(dismissCountDown) {
            this.dismissCountDown = dismissCountDown
        },
        showAlert() {
            this.dismissCountDown = this.dismissSecs
        },
        obtenerMunicipio(){
            this.showAlert();
            this.mensaje.texto = 'Ejecutado!'
            this.mensaje.color = 'danger';
            this.axios.get('localidadmunicipio')
            .then((response) => {
                // console.log(response.data)
                this.arrLocalidadMunicipio = jsonMun;
                this.arrMunicipio = this.removeDuplicates(this.arrLocalidadMunicipio,'nombreMunicipio');
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        obtenerLocalidades(slcMun){
            this.arrColonia=this.arrLocalidadMunicipio.filter(item=>{
                return item.nombreMunicipio===slcMun;
            });
            console.log(this.arrColonia);
        },
        checkFormProveedor(){
            this.errors = [];
            console.log("entro");
            if(!this.proveedor.typeUser){
                console.log("user nop");
                this.errors.push('El tipo de usuario es obligatorio.');
            }
            if(!this.proveedor.rfc){
                console.log("rfc nop");
                this.errors.push('El RFC es obligatorio.');
            }
            if(!this.proveedor.email){
                console.log("email nop");
                this.errors.push('La correo es obligatorio.');
            }
            if(!this.proveedor.password){
                console.log("pass nop");
                this.errors.push('La contraseña es obligatorio.');
            }else if(!this.verificaLargoPass(this.proveedor.password)){
                console.log("pass cortita");
                this.errors.push('La contraseña debe ser superios a seis carácteres.');
            }else if(!this.comprobarContrasena(this.proveedor.password,this.confirmaPass)){
                console.log("pass no coincide");
                this.errors.push('Las contraseñas no coinciden.');
            }//else if(!this.comprobarNumeroPass(this.proovedor.password)){
                //console.log("pass no que no");
                //this.errors.push('Las contraseña debe contener al menos un número.');
            //}
            if(!this.proveedor.nombreUser){
                console.log("email nop");
                this.errors.push('La correo es obligatorio.');
            }

            if (this.errors.length) {
                console.log("me alcanzo");
                this.showAlert();
                this.mensaje.texto = this.errors;
                this.mensaje.color = 'danger';
            }else{
                this.registrarUsuario();
            }

        },
        checkFormArea(){
            this.errors = [];
            console.log("entro");
            if(!this.area.typeUser){
                console.log("user nop");
                this.errors.push('El tipo de usuario es obligatorio.');
            }
            if(!this.area.cv_cct){
                console.log("cct nop");
                this.errors.push('La clave del centro de trabajo es obligatoria.');
            }
            //if(!this.area.municipio){
                //console.log("mun nop");
              //  this.errors.push('Municipio es obligatorio.');
            //}
            //if(!this.area.localidad){
              //  console.log("loc nop");
                //this.errors.push('La localidad es obligatoria.');
            //}
            if(!this.area.email){
                console.log("email nop");
                this.errors.push('El correo electrónico es obligatorio.');
            }
            if(!this.area.password){
                console.log("pass nop");
                this.errors.push('La contraseña es obligatorio.');
            }else if(!this.verificaLargoPass(this.area.password)){
                console.log("pass cortita");
                this.errors.push('La contraseña debe ser superios a seis carácteres.');
            }else if(!this.comprobarContrasena(this.area.password,this.confirmaPassArea)){
                console.log("pass no coincide");
                this.errors.push('Las contraseñas no coinciden.');
            }else if(!this.comprobarNumeroPass(this.area.password)){
                console.log("pass no que no");
                this.errors.push('Las contraseña debe contener al menos un número.');
            }
            if(!this.area.nombreUser){
                console.log("nombre nop");
                this.errors.push('Nombre de usuario obligatorio.');
            }

            if (this.errors.length) {
                console.log("me alcanzo");
                this.showAlert();
                this.mensaje.texto = this.errors;
                this.mensaje.color = 'danger';
            }else{
                this.registrarUsuarioArea();
            }

        },
        verificaLargoPass(emailUs){
            this.largo=emailUs.length;
            console.log(this.largo+" este es el largo")
            if(this.largo<6){
                return false;
            }else{
                return true;
            }
        },
        comprobarContrasena(contraAct,contraVer){
            this.clave1=contraAct;
            console.log(this.clave1);
            this.clave2=contraVer;
            console.log(this.clave2);
            if (this.clave1 == this.clave2){
                return true;
            }
            else{
                return false;
            }
        },
        comprobarNumeroPass(contraAct){
           //var re= "^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{6,}$"
           return contraAct.replace("^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{6,}$");
            //return re.test(contraAct);
        },
        removeDuplicates(originalArray, prop) {
            var newArray = [];
            var lookupObject  = {};

            for(var i in originalArray) {
                lookupObject[originalArray[i][prop]] = originalArray[i];
            }

            for(i in lookupObject) {
                newArray.push(lookupObject[i]);
            }
            return newArray;
        },
    },
}
</script>