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

        <div class="panel border p-4">
            <div class="form-group"> 
                <div class="row" >
                    <div class="col-small">
                        <div class="form-check">
                            <label class="form-check-label">
                                <input type="radio" class="form-check-input" name="veri" 
                                value="verificado" @click="verificadoSelect=1">Verificados
                            </label>
                        </div>
                    </div>
                    <div class="col-small" >
                        <div class="form-check">
                            <label class="form-check-label">
                                <input type="radio" class="form-check-input" name="noveri" 
                                value="noverificado" @click="verificadoSelect=2">No verificados
                            </label>
                        </div>
                    </div>
                    <div class="col" >
                        <label for="subsec">Filtro</label>
                        <input type="text" name="descripcion" id="descripcion" class="form-control" size="30" maxlength="80" 
                        v-model="searchText"/>
                    </div>
                </div>
            </div>

            <div class="panel border p-4" v-if="verificadoSelect==0">
                <label>TODOS</label>
                <paginate ref="paginator" name = "arrProductos" 
                :list = "arrProductos" :per = "20">
                <table class="table table-striped">
                    <thead>
                        <tr>
                            <th>Clave</th>
                            <th>Unidad</th>
                            <th>Descripción</th>
                            <th>Partida</th>
                            <th>Descrip Partida</th>
                            <th>Verificado</th>
                        </tr>
                    </thead>
                    <tbody>
                        
                        <tr v-for="index in paginated('arrProductos')" :key="index.id">
                            <td>
                                {{index.articulo}}
                            </td>
                            <td>
                                {{index.unidad}}
                            </td>
                            <td>
                                {{index.descripcion}}
                            </td>
                            <td>
                                <input type='number' size='4' 
                                v-model="index.partida1"
                                @blur="obtenerArticulo(index.articulo)"/>
                            </td>
                            <td>
                                <input type='text' readonly size='20' 
                                v-model="arrDesc[encontrarIndice(arrProductos,index)]"/>
                            </td>
                            <td>
                                <input type='checkbox' class='form-check-input' 
                                v-model="index.status"
                                @click="obtenerArticuloStatus(index.articulo)">
                            </td>
                        </tr>
                    </tbody>
                </table>
                </paginate>
                <paginate-links
                    for="arrProductos"
                        :show-step-links="true"
                        :simple="{
                                prev: 'Anterior',
                                next: 'Siguiente'  
                            }"
                ></paginate-links>
        </div>

        <div class="panel border p-4" v-if="verificadoSelect==1">
            <label>Verificado</label>
                <table class="table table-striped">
                <thead>
                    <tr>
                        <th>Clave</th>
                        <th>Unidad</th>
                        <th>Descripción</th>
                        <th>Partida</th>
                        <th>Descrip Partida</th>
                        <th>Verificado</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in arrVerProductos">
                        <td>
                            {{item.articulo}}
                        </td>
                        <td>
                            {{item.unidad}}
                        </td>
                        <td>
                            {{item.descripcion}}
                        </td>
                        <td>
                            <input type='number' size='4' 
                            v-model="item.partida"/>
                        </td>
                        <td>
                            <input type='text' readonly size='20' 
                            v-model="item.des_partida"/>
                        </td>
                        <td>
                            <input type='checkbox' class='form-check-input' 
                            value='verificado'>
                        </td>
                    </tr>
                </tbody>
            </table>  
        </div>

        <div class="panel border p-4" v-if="verificadoSelect==2">
            <label>no verificado</label>
                <table class="table table-striped">
                <thead>
                    <tr>
                        <th>Clave</th>
                        <th>Unidad</th>
                        <th>Descripción</th>
                        <th>Partida</th>
                        <th>Descrip Partida</th>
                        <th>Verificado</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in arrNoVerProductos">
                        <td>
                            {{item[0].articulo}}
                        </td>
                        <td>
                            {{item.unidad}}
                        </td>
                        <td>
                            {{item.descripcion}}
                        </td>
                        <td>
                            <input type='number' size='4' 
                            v-model="item.partida"/>
                        </td>
                        <td>
                            <input type='text' readonly size='20' 
                            v-model="item.des_partida"/>
                        </td>
                        <td>
                            <input type='checkbox' class='form-check-input' 
                            value='verificado'>
                        </td>
                    </tr>
                </tbody>
            </table>  
        </div>

        
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            dismissSecs: 5,
            dismissCountDown: 0,
            mensaje: {color: 'success', texto: ''},
            paginate:['arrProductos'],
            arrProductos:[],
            arrVerProductos:[],
            arrNoVerProductos:[],
            articuloEditar:[],
            arrPartidas:[],
            arrDesc:[],
            descripcionPartida:'',
            verificadoSelect:0,
            indice:-1,
            indiceArt:'',
            indiceStat:'',
            searchText:'',                    
}
    },created(){
        this.obtenerTodosProductos();
        console.log('arreglos con datos',this.arrProductos);
        //<tr v-for="(item, index) in arrProductos" :key="index">
    },
    methods:{
         obtenerTodosProductos(){
            this.axios.get('productos')
            .then((response) => {
                //console.log(response)
                console.log('response',response.data);
                this.arrProductos=response.data;
                console.log('arreglo ',this.arrProductos);
                this.obtenerDesPartida();
           })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        obtenerDesPartida(idPartida){
            this.axios.get('partidas')
            .then((response) => {
                this.arrPartidas=response.data;
                this.cantidadPartidas=this.arrProductos.length;
                for (this.i=0;this.i<this.arrProductos.length;this.i++){ 
   	                for (this.j=0;this.j<this.arrPartidas.length;this.j++) {
   	                    if(this.arrProductos[this.i].partida1==this.arrPartidas[this.j].partida){
                            this.arrDesc[this.i]=this.arrPartidas[this.j].descripcion;
                            break;
                        }else if(this.arrProductos[this.i].partida1!=this.arrPartidas[this.j].partida){
                            this.arrDesc[this.i]='No existe esta partida';
                        }
   	                } 
                }
           })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
         obtenerVerProductos(){
            this.axios.get('productos')
            .then((response) => {
                // console.log(response.data)
                this.arrVerProductos = response.data;
           })
            .catch((e)=>{
                console.log('error' + e);
            })
        }, obtenerNoVerProductos(){
            this.axios.get('productos')
            .then((response) => {
                // console.log(response.data)
                this.arrNoVerProductos = response.data;
           })
            .catch((e)=>{
                console.log('error' + e);
            })
        },
        obtenerArticulo(idArticulo){
            console.log("este es el articulo ",idArticulo);
            this.axios.get(`buscar-articulo/${idArticulo}`)
            .then(res => {
                this.articuloEditar = res.data;
                this.indiceArt=this.encontrarIndice(this.arrProductos,this.articuloEditar);
                this.articuloEditar.partida1=this.arrProductos[this.indiceArt].partida1;
                this.editarPartida(this.articuloEditar);
            })
            .catch(e => {
                console.log(e.response);
            })
        },
        editarPartida(item){
            console.log("este es el objeto ",item);
           this.axios.put(`editar-producto/${item._id}`, item)
            .then(res => {
                let index = this.arrProductos.findIndex( ip => ip._id === this.articuloEditar._id);
                this.arrProductos[index].partida1 = this.articuloEditar.partida1;
                this.obtenerDesPartida();

                this.showAlert();
                this.mensaje.texto = 'Partida Actualizada'
                this.mensaje.color = 'success'
            })
            .catch(e => {
                this.showAlert();
                this.mensaje.texto = 'Error al actualizar partida'
                this.mensaje.color = 'danger'
                console.log(e);
             })
        },
        obtenerArticuloStatus(idArticulo){
            this.axios.get(`buscar-articulo/${idArticulo}`)
            .then(res => {
                this.statusEditar = res.data;
                this.indiceStat=this.encontrarIndice(this.arrProductos,this.statusEditar);
                this.statusEditar.status=this.arrProductos[this.indiceStat].status;
                this.cambiaStatus(this.statusEditar);
            })
            .catch(e => {
                console.log(e.response);
            })
        },
        cambiaStatus(item){
            this.axios.put(`editar-producto/${item._id}`, item)
            .then(res => {
                let index = this.arrProductos.findIndex( ip => ip._id === this.statusEditar._id);
                this.arrProductos[index].status = this.statusEditar.status;

                this.showAlert();
                this.mensaje.texto = 'Status Actualizado'
                this.mensaje.color = 'success'
            })
            .catch(e => {
                console.log(e);
             })
        },
        countDownChanged(dismissCountDown) {
            this.dismissCountDown = dismissCountDown
        },
        showAlert() {
            this.dismissCountDown = this.dismissSecs
        },
        encontrarIndice(a,b){
            console.log("a ",a);
            console.log("b ",b);
             for (this.i=0;this.i<a.length;this.i++){ 
                 if(a[this.i].articulo==b.articulo){
                     return this.i;
                 }
            }
        },
    }
}
</script>