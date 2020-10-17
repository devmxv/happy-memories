<template>
    <div class="w-25">
        <form @submit.prevent="saveData">
            <div class="input-group mb-3 w-100">
                <input
                    v-model="form.title"
                    :class="{'is-invalid' : form.errors.has('title')}"
                    type="text"
                    class="form-control form-control-lg"
                    placeholder="Escribe un momento feliz"
                    aria-label="username"
                    aria-describedby="button-addon2"
                    @keydown="form.errors.clear('title')"
                    />
                <div class="input-group-append">
                    <button
                        class="btn btn-success"
                        type="submit"
                        id="button-addon2"
                        >Agregar
                    </button>
                </div>
                <span class="text-danger pt-3 pb-3" style="font-size:20px;" v-if="form.errors.has('title')" v-text="form.errors.get('title')"></span>
            </div>
        </form>
        <div class="w-20 memory-list">
            <div v-for="memory in memories" :key="memory.id" class="w-100">
                {{memory.title}}
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                editmode: false,
                memories: '',
                form: new Form({
                    title: '',
                })
            }
        },
        methods:{
            getMemories(){
                axios.get('/api/memory').then((res) => {
                    this.memories = res.data
                }).catch((error) => {
                    console.log(error)
                })
            },
            saveData(){
                let data = new FormData();
                data.append('title',this.form.title)
                axios.post('/api/memory', data).then((res) => {
                    this.form.reset()
                    this.getMemories()
                }).catch((error) => {
                    this.form.errors.record(error.response.data.errors)
                })
            }
        },
        mounted() {
            //console.log('Component mounted.')
            this.getMemories()
        }
    }
</script>
