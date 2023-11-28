<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
      bg-color="white"
      class="col"
      filled
      v-model="newTask"
      placeholder="Add task"
      dense
      @keyup.enter="addTask"
      >

        <template v-slot:append>
          <q-btn
          @click="addTask"
          round
          dense
          flat
          icon="add" />
        </template>
      </q-input>
    </div>
    <q-list
    separator
    bordered
    class="bg-white">
      <q-item
      :class="{'done bg-blue-1': task.done }"
      v-for="(task, index) in tasks"
      :key="task.title"
      clickable
      @click="task.done = !task.done"
      v-ripple>
        <q-item-section avatar>
          <q-checkbox
           v-model="task.done"
           color="primary"
           class="no-pointer-events"
           />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section
        v-if="task.done"
        side
        >
         <q-btn
         @click="deleteTask(index)"
         dense
         flat
         round
         color="primary"
         icon="delete" />
        </q-item-section>

      </q-item>
    </q-list>
    <div
    class="no-tasks absolute-center"
    v-if="!tasks.length"
    >
      <q-icon
      name="check"
      size="100px"
      color="primary"
      />
      <div class="text-h5 text-primary text-center">
        No tasks
      </div>

    </div>

  </q-page>
</template>

<script>

import { useQuasar, Dialog, Notify } from 'quasar'

export default {
  data () {
    return {
      newTask: '',
      tasks: [
        // {
        //   title: 'Get bananas',
        //   done: false
        // },

        // {
        //   title: 'Eat bananas',
        //   done: true
        // },

        // {
        //   title: 'Poo bananas',
        //   done: false
        // },

      ]
    }
  },

  methods: {
    deleteTask(index){
        this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1);
        this.$q.notify('Task deleted')
      })

    },

    addTask(){
      //console.log('addTask');
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = '';
    }
  }

};
</script>

<style>
  .done {
    color: #bbb;
  }

  .no-tasks {
    opacity:0.5;
  }

</style>
