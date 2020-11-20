<template>
    <section>
        <q-btn size="12px" flat dense round icon="edit" @click="edit = true"/>
        <q-dialog v-model="edit" persistent>
            <q-card style="min-width: 350px">
                <q-card-section>
                <div class="text-h6">Editar Tarea</div>
                </q-card-section>
                <q-card-section class="q-pt-none">
                <q-form
                    class="q-gutter-md"
                    >
                    <q-input
                        filled
                        v-model="taskname"
                        label="Tarea"
                        lazy-rules
                        :rules="[ val => val && val.length > 0 || 'Campo necesario']"
                    />
                    <q-checkbox label="Hecho" v-model="state" />
                </q-form>
                <!-- <pre>{{task}}</pre> -->
                </q-card-section>
                <q-card-actions align="right" class="text-primary">
                <q-btn label="Actualizar" color="primary" @click="update"/>
                <q-btn label="Cerrar" color="primary" flat class="q-ml-sm" @click="edit = false" />
                </q-card-actions>
            </q-card>
        </q-dialog>
    </section>
</template>

<script>
    export default {
        name: 'EditTask',
        props: {
            task: Object,
            index: Number
        },
        data() {
            return {
                taskname: '',
                state: false,
                edit: false
            }
        },
        
        methods: {
            update() {
                let estado = 'pendiente'
                if(this.state){
                    estado = 'hecho'
                }
                this.$emit('taskUpdatedd',{
                    id: this.task.id,
                    taskname: this.taskname,
                    state: estado,
                }, this.index)
                this.edit = false
            },
        },

        created () {
            this.taskname = this.task.taskname
            if(this.task.state=='pendiente'){
                this.state = false
            }else {
                this.state = true
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>