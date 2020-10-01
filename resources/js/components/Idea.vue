<template>
    <div>
    <h2 class="text-center">Captura tus ideas</h2>
        <div class="card">
            <div class="card-body">
                <h4>¿En que estas pensando?</h4>
                <form v-on:submit.prevent="createIdea">
                    <div class="input-group">
                        <input type="text" class="form-control input-sm" v-model="newIdea" maxlength="256">
                        <span class="input-group-btn">
                            <button class="btn btn-primary btn-sm">Agregar</button>
                        </span>
                    </div>
                </form>
                <hr>
                <ul class="list-unstyled">
                    <li v-for="idea in ideas">
                        <p>
                            {{ idea.description }}
                            <small class="text-muted">
                                <em>{{ since(idea.created_at) }}</em>
                            </small>
                        </p>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios' //Ajax
    import toastr from 'toastr' //Mejor diseño
    import moment from 'moment' //Formateo de la hora
    //import css

    moment.lang('es');

    export default {
       data () {
           return {
               ideas: [],
               newIdea: '',
           }
       },
       created: function(){
           this.getIdeas();
       },
       methods:{

           since: function(d){
               return moment(d).fromNow();
           },
           getIdeas: function(){
               var urlIdeas = 'myIdea';
               axios.get(urlIdeas).then(response => {
                   this.ideas = response.data;
               });
           },
           createIdea: function(){
               var url = 'saveIdea';
               axios.post(url, {
                   description: this.newIdea
               }).then(response => {
                   this.getIdeas();
                   this.newIdea = '';
                   toastr.success('Nueva idea registrada');
               }).catch(error =>{
                   toastr.error('Error');
               });
           }
       }
    }
</script>

