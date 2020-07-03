<template>
    <div class="container">

        <h1>Captura de requisiciones</h1>
    <form  @submit.prevent="adquisicionALote(adquicicion)">
    <div class="form-group">
        <div class="row">
            <div class="col">
                <label for="subsec">Subsecretaría</label> :
                <select name="subsec" id = "subsec" class="form-control"
                v-model="adquicicion.ct_subsec">
                    <option  v-for="subs in subsecretariaArr">
                        {{subs.subsecretaria}}
                    </option>
                </select>
            </div>
            <div class="col">
                <label for="fechahoy">Fecha de elaboracion: </label>
                <input type="date" id="fechahoy" name="fechahoy" class="form-control"
                v-model="adquicicion.fechae" />
            </div> 
        </div>

        <div class="row">
            <div class="col">
                <label for="subsec">Área</label>
                <input type="text" name="cct" id="cct" class="form-control" size="10" maxlength="10" readonly value="Área específica"
                placeholder="funciona"
                v-model="adquicicion.ctdestino"/>
            </div>
            <div class="col">
                <label for="fechareq">Fecha que se requiere: </label>
                <input type="date" id="fechareq" name="fechareq" class="form-control"
                v-model="adquicicion.fechar" />
            </div> 
        </div>

        <div class="row">
            <div class="col">
                <label for="subsec">Origen del recurso</label> :
                <select name="subsec" id = "subsec" class="form-control"
                v-model="adquicicion.fuente"
                @blur="captarFuente(adquicicion.fuente)">
                    <option value="">-- Recurso --</option>
                    <option value="2013302">ESTATAL</option>
                    <option value="2013301">FEDERAL</option>
                </select>
            </div>
        </div>

        <div class="row">
            <div class="col">
                <label for="cctdestino">Destino del bien: </label>
                <input type="text" name="cctdestino" id="cctdestino" class="form-control" size="10" maxlength="10" placeholder="Clave CT"
                v-model="adquicicion.destinoCT"/>
            </div>      
            <div class="col">
                <label for="buscact">Nombre CT: </label>
                <select name="buscact" id = "buscact" class="form-control"
                v-model="adquicicion.ctdestino">
		            <option v-for="prod in arrCT">
                        {{prod.nombre}}
                    </option>
                </select>
            </div>        
        </div>
        <div class="row">
            <div class="col">
                <label for="domicilioct">Domicilio de entrega: </label>
                <input type="text" name="domicilioct" id="domicilioct" class="form-control" placeholder="Específique domicilio de entrega" 
                v-model="adquicicion.domicilioEntrega"/>
            </div>      
        </div>

        <div class="row">
            <div class="col">
                <label for="accion">Acción </label>
                <select name="accion" id = "accion" class="form-control"
                v-model="adquicicion.accion"
                @blur="descripcionAccion(adquicicion.accion)">
                    <option v-for="part in accionArr">
                        {{part}}
                    </option>
                </select>
            </div>      
            <div class="col">
                <label for="nomaccion">Nombre de acción </label>
                <input type="text" name="nomaccion" id="nomaccion" class="form-control" 
                readonly/>
                
            </div>      
        </div>

        <div class="row">
            <div class="col">
                <label for="partida">Partida</label> :
                <select name="partida" id = "partida" class="form-control"
                v-model="adquicicion.partida"
                @blur="descripcionPartida(adquicicion.partida)">
                    <option v-for="part in partidaArr">
                        {{part}}
                    </option>
                </select>
            </div>
            <div class="col">
                <label for="nompartida">Nombre partida:</label>
                <input type="textarea" name="nompartida" id="nompartida" class="form-control" 
                    readonly />
            </div>    
        </div>

        <div class="row">
            <div class="col">
                <label for="buscaprod">Buscar producto:</label>
                <input type="text" name="buscaprod" id="buscaprod" class="form-control" size="50" maxlength="50" value="MICA"/>
            </div>
            <div class="col">
                <label for="producto">Producto</label>
                <select name="producto" id = "producto" class="form-control"
                v-model="adquicicion.articulo"
                @blur="this.productoArr(adquicicion.articulo)">
		        <option v-for="prod in productoArr">
			            {{prod.descripcion}}
		        </option>
</select>
            </div>
            <div class="col">
                <label for="unidad">Unidad</label> :
                <select name="Unidad" id = "Unidad" class="form-control"
                v-model="adquicicion.unidad">
                    <option value='PAQUETE'>Paquete</option>
                    <option value='PIEZA'>Pieza</option>
                </select>
            </div>
            <div class="col">
                <label for="cantidad">Cantidad </label>
                <input type="number" name="cantidad" id="cantidad" class="form-control" min="1" max="9999999" step="1"
                v-model="adquicicion.cantidad"/>
            </div>   
        </div>

         <div class="row">
            <div class="col">
                <div class="form-group">
                    <label for="descripcion">Descripción detallada del bien o servicio:</label>
                    <textarea class="form-control" rows="5" id="descripcion" name="descripcion"
                    v-model="adquicicion.descripcion">
                        MICA TERMICA DE 15X11.5 CM C/100
                    </textarea>
                </div>
            </div>
        </div>

        <div>
            <div class="col">
                <label for="anexos">Anexos:</label>
                <input type="file" class="form-control" id="anexos" name="anexos" />            
            </div>
        </div>

        <div class="row">
            <button type="submit" class="btn btn-success btn-block">Agregar lote</button>
        </div>

        <div class="panel border p-4">
            <table class="table table-striped">
                <thead>
                    <tr>
                        <th>Lote</th>
                        <th>Cantidad</th>
                        <th>Unidad</th>
                        <th>Descripción</th>
                        <th colspan="6">Clave presupuestal</th>
                        <th>Acción</th>
                        <th>Fuente</th>
                        <th>Partida</th>        
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item,i) in loteArr" :key="i">
                        <th scope="row">{{item.length}}</th>
                        <td>{{item.cantidad}}</td>
                        <td>{{item.unidad}}</td>
                        <td>{{item.descripcion}}</td>
                        <td></td>
                        <td>1006</td>
                        <td>31</td>
                        <td>2</td>
                        <td>4</td>
                        <td>1</td>
                        <td>{{item.accion}}</td>
                        <td>{{item.fuente}}</td>
                        <td>{{item.partida}}</td>
                    </tr>
                </tbody>
            </table>        
        </div>
        </div>
        </form>
         <div class="row">
            <button type="button" class="btn btn-success btn-block">Enviar</button>
        </div>
        
    </div>
</template>
<script>
export default {
    data(){
        return{
            subsecretariaArr:[],
            accionArr:[],
            partidaArr:[],
            productoArr:[],
            fuentesArr:[],
            arrCT:[],
            datosAdquisicion:[],
            loteArr:[],
            adquicicion:[{
                    ct_subsec:'',fechae:'',fechar:'',
                    fuente:'',ctdestino:'',
                    accion:'',status:'',partida:'',
                    articulo:'',unidad:'',descripcion:'',
                    cantidad:'',
            }],
            funciona:'a nu ma si era así',
            otroTexti:'si cambio :o',
            nombreAccion:'',
            nombrePartida:'',
            domicilioEntrega:'',
            usuario:'',
            nombreAccion:'',
            nombrePartida:'',
        }
    },
    created(){
        this.obtenerSubsecretaria();
        this.obtenerCT();
    },
    computed:{
        options: () => this.productoArr,
    },
    methods:{
        registrarAdquisicion(){
            this.axios.post('nueva-adquisicion', this.adquicicion)
            .then(res => {
                // Alerta de mensaje
                this.showAlert();
                this.mensaje.texto = "registro correcto";
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
        obtenerSesion(){
            this.axios.get('session-check')
            .then((response) => {
                // console.log(response.data)
                this.usuario = response.data;
                console.log(this.usuario);
                document.getElementById("cct").value=this.usuario.cv_cct;
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        obtenerSubsecretaria(){
            this.axios.get('subsecretarias')
            .then((response) => {
                // console.log(response.data)
                this.subsecretariaArr = response.data;
                console.log(response.data);
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        obtenerFuente(){
            this.axios.get(`presupuestal`)
            .then((response) => {
                // console.log(response.data)
                this.fuentesArr = response.data;
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        captarFuente(fuente){
            if(this.adquicicion.fuente=='FEDERAL'){
                fuente=2023301;
            }else if(this.adquicicion.fuente=='ESTATAL'){
                fuente=2013302;
            }
            this.axios.get(`adquisiciones/${fuente}`)
            .then((response) => {
                console.log('response ',response.data)
                this.accionArr = response.data.acciones;
                this.partidaArr=response.data.partidas;
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        descripcionAccion(accion){
            this.axios.get(`accion-desc/${accion}`)
            .then((response) => {
                console.log('response ',response);
                this.nombreAccion = response.data;
                console.log('descripcion ',this.nombreAccion);
                this.colocaNombreAccion(this.nombreAccion);

        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
         descripcionPartida(partida){
            this.axios.get(`partida-desc/${partida}`)
            .then((response) => {
                console.log('response ',response);
                this.nombrePartida = response.data;
                console.log('descripcion ',this.nombrePartida);
                this.colocaNombrePartida(this.nombrePartida);
                this.obtenerProducto(partida);
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },

        obtenerAccion(fuente){
            this.axios.get(`partida-verificada${fuente}`)
            .then((response) => {
                // console.log(response.data)
                this.accionArr = response.data;
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        obtenerCT(){
            this.axios.get('centro-trabajo')
            .then((response) => {
                console.log(response.data);
                this.arrCT = response.data;
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        obtenerProducto(partida){
            this.axios.get(`productos/${partida}`)
            .then((response) => {
                console.log(response.data)
                this.productoArr = response.data;
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        verificaArea(idSubsec){
            this.axios.get('subsecretaria-verificada')
            .then((response) => {
                this.adquicicion.ct_subsec=response.data;
                this.colocaNombreArea(this.adquicicion.ct_subsec);
        })
            .catch((e)=>{
                console.log('error' + e);
                this.showAlert();
                this.mensaje.texto = "No existe esta Area";
                this.mensaje.color = 'danger';
            })
        },
        MuestraDireccion(nomCT){
           
        },
        adquisicionALote(nuevaAdquisicion){
            this.nuevoLote = new Object({
                ...nuevaAdquisicion
            });
            this.loteArr.push(this.nuevoLote);
            console.log(this.nuevoLote);
            console.log("extension ",this.loteArr.length);
            console.log("objetos ",this.loteArr);
        },
        colocaNombreAccion(nombAccion){
            console.log('nombre accion ',nombAccion)
            document.getElementById("nomaccion").value=nombAccion;
        },
        colocaNombrePartida(nombPartida){
            document.getElementById("nompartida").value=nombPartida;
        },
         colocaDescripcion(descProd){
            this.adquicicion.descripcion=descProd;
            document.getElementById("descripcion").value=this.adquicicion.descripcion;
        },
        limpiarFormulario(){
            
        },

    }
}
</script>