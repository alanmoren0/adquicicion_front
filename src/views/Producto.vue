<template>
    <div class="container">

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
                        <input type="text" name="descripcion" id="descripcion" class="form-control" size="30" maxlength="80" />
                    </div>
                </div>
            </div>

            <div class="panel border p-4" v-if="verificadoSelect==0">
                <label>TODOS</label>
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
                    <tr v-for="item in arrProductos">
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

        
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            arrProductos:[],
            arrVerProductos:[],
            arrNoVerProductos:[],
            producto:{articulo:'',unidad:'',
                    descripcion:'',partida:'',
                    des_partida:'',verificado:''
            },
            verificadoSelect:0,

        }
    },
    methods:{
         obtenerTodosProductos(){
            this.axios.get('productos')
            .then((response) => {
                // console.log(response.data)
                this.arrProductos = response.data;
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
    }
}
</script>