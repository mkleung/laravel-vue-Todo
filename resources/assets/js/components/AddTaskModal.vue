<template>
    <section>
        <button class="button is-primary is-rounded "
            @click="isComponentModalActive = true">
            <span class="icon">
                    <i class="fa fa-location-arrow"></i>
                </span>
            <span>Add a task</span>
        </button>

        <b-modal :active.sync="isComponentModalActive" has-modal-card>
           
                <div class="modal-card" style="width: 600px">
                    <header class="modal-card-head">
                        <p class="modal-card-title">Add Task</p>
                    </header>
                    
                    <section class="modal-card-body">
                       <div class="field">
                            <p class="control has-icons-left has-icons-right">
                                <b-input 
                                maxlength="100" 
                                type="textarea" 
                                v-model="task.title"
                                @keydown.native.enter="addTask()"  @focus.native="$event.target.select()" ></b-input>
                            </p>
                        </div>
                    </section>

                    <footer class="modal-card-foot">
                        <button class="button" type="button" @click="isComponentModalActive = false">Close</button>
                        <button class="button is-primary" @click="addTask()">Add</button>
                    </footer>
                </div>
           

        </b-modal>
    </section>
</template>

<script>

    export default {
        data() {
            return {
                isComponentModalActive: false,
                 task: {
                    title: ''
                },
            }
        },
  
        
        methods: {
            addTask() {
                this.isComponentModalActive = false;
                
                if (this.$data.task.title !== "") {
                    axios.post('task', this.$data.task)
                    .then((response)=>{
                        this.$parent.searchList.unshift(response.data)
                        this.$parent.titleList.push(response.data.title);
                        this.$data.task.title = "";
                    })
                    .catch((error) => this.errors = error.response.data);
                }


            }
        }
    }
</script>