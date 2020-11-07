<template>
  <div>
    <form @submit.prevent="agregar">
      <h3>Agregar nota</h3>
      <input type="text" class="form-control mb-2" 
        placeholder="Nombre de la nota" v-model="nota.name">
        <input type="text" class="form-control mb-2" 
        placeholder="Telefono" v-model="nota.telephone">
        <input type="text" class="form-control mb-2" 
        placeholder="Email" v-model="nota.email">
      <input type="text" class="form-control mb-2" 
        placeholder="Issue" v-model="nota.issue">
      <button class="btn btn-primary" type="submit">Agregar</button>
    </form>
    <hr>
    <h3>Lista de notas:</h3>
    <ul class="list-group">
         <li class="list-group-item" 
            v-for="(item, index) in notas" :key="index" >
          <span class="badge badge-primary float-right">
            {{item.updated_at}}
          </span>
          <p>{{item.name}}</p>
          <p>{{item.telephone}}</p>
          <p>{{item.email}}</p>
          <p>{{item.issue}}</p>
          <p>
            <button class="btn btn-warning btn-sm" 
                @click="editarFormulario(item)">Editar</button>
          </p>
        </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      notas: [],
      modoEditar: false,
      nota: {name: '', telephone:'', email:'', issue: ''}
    }
  },
  created(){
    axios.get('/notas').then(res=>{
      this.notas = res.data;
    })
  },
  methods:{
    agregar(){
      if(this.nota.name.trim() === '' || this.nota.telephone.trim() === '' || this.nota.email.trim() === '' || this.nota.issue.trim() === ''){
        alert('Debes completar todos los campos antes de guardar');
        return;
      }
      const notaNueva = this.nota;
      this.nota = {name: '', telephone:'', email:'', issue:''};    
      axios.post('/notas', notaNueva)
        .then((res) =>{
          const notaServidor = res.data;
          alert('Nota agregada');
          this.notas.push(notaServidor);
        })
    },
    editarFormulario(item){
      this.nota.name = item.name;
      this.nota.telephone = item.telephone;
      this.nota.email = item.email;
      this.nota.issue = item.issue;
      this.nota.id = item.id;
      this.modoEditar = true;
    }
  }
}
</script>