<template>
    <div class="container">

        <h1>Captura de requisiciones</h1>
    <form  @submit.prevent="adquisicionALote()">
    <div class="form-group">
        <div class="row">
            <div class="col">
                <label for="subsec">Subsecretaría</label> :
                <select name="subsec" id = "subsec" class="form-control">
                    <option  v-for="subs in productoArr">
                        {{subs.nombreProducto}}
                    </option>
                </select>
            </div>
            <div class="col">
                <label for="fechahoy">Fecha de elaboracion: </label>
                <input type="date" id="fechahoy" name="fechahoy" class="form-control"
                v-model="adquicicion.fechaElaboracion" />
            </div> 
        </div>

        <div class="row">
            <div class="col">
                <label for="subsec">Área</label>
                <input type="text" name="cct" id="cct" class="form-control" size="10" maxlength="10" readonly value="Área específica"
                placeholder="funciona"
                v-model="adquicicion.area" @focus="agregaArea()" @blur="cambiaEnDesenfoque()"/>
            </div>
            <div class="col">
                <label for="fechareq">Fecha que se requiere: </label>
                <input type="date" id="fechareq" name="fechareq" class="form-control"
                v-model="adquicicion.fechaRequerida" />
            </div> 
        </div>

        <div class="row">
            <div class="col">
                <label for="subsec">Origen del recurso</label> :
                <select name="subsec" id = "subsec" class="form-control"
                v-model="adquicicion.origenRecurso">
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
                <input type="text" name="buscact" id="buscact" class="form-control" size="50" maxlength="50" placeholder="Nombre del CT a buscar"
                v-model="adquicicion.nombreCT"/>
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
                <input type="number" name="accion" id="accion" class="form-control" min="1" max="999" step="1" value="100"
                v-model="adquicicion.accion"/>
            </div>      
            <div class="col">
                <label for="nomaccion">Nombre de acción </label>
                <input type="text" name="nomaccion" id="nomaccion" class="form-control" 
                    value="Administración de recursos materiales y financieros para la adecuada prestación de los servicios educativos por medio de la gestión central" 
                    readonly />
            </div>      
        </div>

        <div class="row">
            <div class="col">
                <label for="partida">Partida</label> :
                <select name="partida" id = "partida" class="form-control"
                v-model="adquicicion.partida">
                    <option v-for="part in partidaArr">
                        {{part.nomPartida}}
                    </option>
                </select>
            </div>
            <div class="col">
                <label for="nompartida">Nombre partida:</label>
                <input type="text" name="nompartida" id="nompartida" class="form-control" 
                    value="Asignaciones destinadas a la adquisición de materiales y artículos diversos, propios para el uso de las oficinas, tales como: papelería, formas, libretas, carpetas, y cualquier tipo de papel, vasos y servilletas desechables, limpia tipos, rollos fotográficos; útiles de escritorio como engrapadoras, perforadoras manuales, sacapuntas; artículos de dibujo, correspondencia y archivo; cestos de basura, y otros productos similares. Incluye la adquisición de artículos de envoltura, sacos y valijas, entre otros" 
                    readonly />
            </div>    
        </div>

        <div class="row">
            <div class="col">
                <label for="buscaprod">Buscar producto:</label>
                <input type="text" name="buscaprod" id="buscaprod" class="form-control" size="50" maxlength="50" value="MICA"/>
            </div>
            <div class="col">
                <label for="producto">Producto</label> :
                <select name="producto" id = "producto" class="form-control"
                v-model="adquicicion.producto">
                    <option v-for="prod in productoArr">
                        {{prod.nomProducto}}
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
                    <tr v-for="item in loteArr">
                        <td>1</td>
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
            partidaArr:[],
            productoArr:[],
            loteArr:[],
            adquicicion:[{
                    subsecretaria:'',fechaElaboracion:'',area:'',
                    fechaRequerida:'',origenRecurso:'', destinoCT:'',
                    nombreCT:'',domicilioEntrega:'',accion:'',
                    nombreAccion:'',partida:'',nombrePartida:'',
                    producto:'',unidad:'',cantidad:'',descripcion:''
            }],
            funciona:'a nu ma si era así',
            otroTexti:'si cambio :o',
        }
    },
    created(){



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
        obtenerSubsecretaria(){
            this.axios.get('subsecretaria')
            .then((response) => {
                // console.log(response.data)
                this.subsecretariaArr = response.data;
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        obtenerPartida(){
            this.axios.get('partida')
            .then((response) => {
                // console.log(response.data)
                this.partidaArr = response.data;
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        obtenerProducto(){
            this.axios.get('producto-verificado')
            .then((response) => {
                // console.log(response.data)
                this.productoArr = response.data;
        })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        adquisicionALote(){
            this.loteArr.push(this.adquicicion);
            console.log(this.adquicicion);
        },
        agregaArea(){
            document.getElementById("cct").value=this.funciona;
        },
        cambiaEnDesenfoque(){
            document.getElementById("cct").value=this.otroTexti;
        },
        colocaNombreAccion(){
            document.getElementById("cct").value=this.otroTexti;
        },
        colocaNombrePartida(){
            document.getElementById("cct").value=this.otroTexti;
        },

    }
}
</script>