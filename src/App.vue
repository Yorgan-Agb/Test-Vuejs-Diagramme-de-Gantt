<template>
  <div id="app">
    <h1>Test Technique edTake</h1>
    <div class="viewModeContainer">
      <button class="viewMode" @click="demoViewMode('day')">
        Affichage jour
      </button>
      <button class="viewMode" @click="demoViewMode('week')">
        Affichage semaine
      </button>
      <button class="viewMode" @click="demoViewMode('month')">
        Affichage mois
      </button>
    </div>
    <div class="sectionTop">
      <frappe-gantt
        :view-mode="mode"
        :tasks="tasks"
        @task-updated="debugEventLog.push($event)"
        @task-date-updated="debugEventLog.push($event)"
        @task-progress-change="debugEventLog.push($event)"
      />

      <div class="table">
        <table class="tableGantt">
          <thead>
            <tr>
              <th>Nom</th>
              <th>Date de début</th>
              <th>Date de fin</th>
            </tr>
          </thead>
          <tbody>
            <tr
              class="secondTableGantt"
              v-for="event in debugEventLog"
              :key="event.id"
            >
              <th scope="row">{{ event.task.name }}</th>
              <td>{{ event.start }}</td>
              <td>{{ event.end }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <h3>Ajouter une tâche</h3>
    <div class="pascal">
      <input class="pascalContainer" v-model="name" />
      <input class="pascalContainer" v-model="start" placeholder="2022-04-06" />
      <input class="pascalContainer" v-model="end" placeholder="2022-04-28" />
      <button class="addButton" @click="addTask(name, start, end)">Add</button>
    </div>
  </div>
</template>

<script>
import FrappeGantt from './components/frappe-gantt.vue'

import { v4 as uuidv4 } from 'uuid'
uuidv4()

export default {
  name: 'App',
  components: {
    FrappeGantt,
  },
  data() {
    return {
      mode: 'week',
      tasks: [
        {
          id: '1',
          name: 'Finalisation projet client',
          start: '2022-04-06',
          end: '2022-04-15',
          progress: 10,
        },
        {
          id: '2',
          name: 'Présentation projet au client',
          start: '2022-04-15',
          end: '2022-04-24',
          progress: 20,
          dependencies: '1',
        },
        {
          id: '3',
          name: 'Derniers ajustements',
          start: '2022-04-24',
          end: '2022-05-01',
          progress: 10,
          dependencies: '2',
        },
        {
          id: '4',
          name: 'Lancement du site',
          start: '2022-05-01',
          end: '2022-05-02',
          progress: 10,
          dependencies: '3',
        },
      ],
      debugEventLog: [],
    }
  },
  methods: {
    addTask(name, start, end) {
      const id = uuidv4()
      this.tasks.push({
        id: id,
        name: name,
        start: start,
        end: end,
        progress: Math.random() * 100,
      })
    },
    demoViewMode(viewMode) {
      this.mode = viewMode
    },
  },
}
</script>

<style src="./style.css"></style>
