<template>
  <q-page >
    <div class="q-pa-md q-gutter-md">
       <q-btn color="white" text-color="black" label="Nueva" @click="nuevo=true" />
      
      <q-list bordered class="rounded-borders" >
      <q-item-label header>Tareas</q-item-label>
      <template v-for="(task,index) in tasks">
        <q-item :key="task.id">
          <q-item-section avatar top>
            {{index+1}}
          </q-item-section>
          <q-item-section top>
            <q-item-label lines="1">            
              <span class="text-grey-8">{{task.taskname}}</span>
            </q-item-label>
            <q-item-label caption lines="1">
              <q-chip size="sm" v-if="task.state=='hecho'" color="teal" text-color="white" icon="bookmark">
                  Hecho
              </q-chip>
              <div v-else>Pendiente</div>
            </q-item-label>          
          </q-item-section>
          <q-item-section top side>
            <div class="text-grey-8 q-gutter-xs">
              <q-btn flat dense round icon="delete" @click="confirm(task, index)" />
              <EditTask :task="task" :index="index" @taskUpdatedd="update"/>
            </div>
          </q-item-section>
        </q-item>
        <q-separator spaced />
      </template>
    </q-list>

    <q-dialog v-model="nuevo" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Nueva Tarea</div>
        </q-card-section>
        <q-card-section class="q-pt-none">
          <q-form
              class="q-gutter-md"
            >
              <q-input
                filled
                v-model="taskname"
                label="Tarea *"
                hint="ingresa tu tarea"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Campo necesario']"
              />
          </q-form>
        </q-card-section>
        <q-card-actions align="right" class="text-primary">
          <q-btn label="Registrar" color="primary" @click="submit"/>
          <q-btn label="Cerrar" color="primary" flat class="q-ml-sm" @click="onReset" />
        </q-card-actions>
      </q-card>
    </q-dialog>


    </div>
  </q-page>
</template>
<script>
import EditTask from '../components/EditTask.vue';

export default {
  name: 'PageIndex',
  components: {
    EditTask,
  },
   data(){
    return {
      taskname: null,
      tasks: [],
      nuevo: false,
      edit: false,
    }
  },
  methods: {
    submit() {
      this.tasks.push({
          id: Math.floor((Math.random() * 100) + 1),
          taskname: this.taskname,
          freg: "2020-11-14",
          state: "pendiente"
      });
            
        this.$q.notify({
          color: 'green-4',
          textColor: 'white',
          icon: 'cloud_done',
          message: 'Tarea agregada'
        });
      this.taskname = null
      this.nuevo = false
    },

    update(tarea, index){
      //console.log(index)
      const tsk = this.tasks[index]      
      tsk.taskname = tarea.taskname
      tsk.state = tarea.state
      this.tasks[index] = tsk
        this.$q.notify({
          color: 'green-4',
          textColor: 'white',
          icon: 'cloud_done',
          message: 'Tarea actualizada'
        });
    },

    onReset () {
      this.taskname = null
      this.nuevo = false
    },

    confirm(task, index) {
      this.$q.dialog({
        title: '¿Esta Seguro?',
        message: 'borrar "'+task.taskname+'"',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index,1)
      }).onOk(() => {
        // console.log('>>>> second OK catcher')
      }).onCancel(() => {
        // console.log('>>>> Cancel')
      }).onDismiss(() => {
        // console.log('I am triggered on both OK and Cancel')
      })
    },

  }

}
</script>
