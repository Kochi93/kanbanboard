<template>
  <div class="container mt-5">
    <div class="row">
      <div
        class="col-4"
        v-for="statusCard in statusCards"
        :key="statusCard.status"
      >
        <StatusCard
          :title="statusCard.title"
          :titleClasses="statusCard.titleClasses"
          :newTasks="statusCard.newTasks"
          :status="statusCard.status"
          :tasks="filteredTasks(statusCard.status)"
          @new-task="addTask"
          @status-updated="updateStatus"
        />
      </div>
    </div>
  </div>
</template>

<script>
import StatusCard from "./components/StatusCard";
import logger from "./mixins/logger";

export default {
  name: "App",
  mixins: [logger],
  components: {
    StatusCard,
  },
  provide() {
    return {
      maxNumberOfChars: 255,
    };
  },
  data() {
    return {
      number: 255,
      tasks: [
        {
          id: 1,
          content: "Dashboard überarbeiten.",
          status: 1,
        },
        {
          id: 2,
          content: "Anwendung auf Vue.js umstellen.",
          status: 1,
        },
        {
          id: 3,
          content: "Kryptowährungsmaschine zum Laufen bringen.",
          status: 2,
        },
      ],
      statusCards: [
        {
          title: "Neue Aufgaben",
          titleClasses: "bg-secondary text-white",
          newTasks: true,
          status: 0,
        },
        {
          title: "In Bearbeitung",
          titleClasses: "bg-primary text-white",
          newTasks: false,
          status: 1,
        },
        {
          title: "Erledigt",
          titleClasses: "bg-success text-white",
          newTasks: false,
          status: 2,
        },
      ],
    };
  },
  /*   computed: {
    newTasks() {
      return this.tasks.filter((task) => task.status === 0);
    },
  }, */
  /*   mounted() {
    console.log("App-Component ist vollständig bereit.");
  }, */
  methods: {
    filteredTasks(status) {
      return this.tasks.filter((task) => task.status === status);
    },
    addTask(task) {
      let randomNumber = Math.random();
      let idTaken = this.getIdTaken(randomNumber);
      do {
        if (!this.getIdTaken(randomNumber)) idTaken = false;
        else {
          console.log("jumped in");
          randomNumber = this.getRandomNumber();
        }
      } while (idTaken);
      task.id = randomNumber;
      this.tasks.push(task);
      this.writeLogEntry("Neue Aufgabe hinzugefügt.");
    },
    getRandomNumber() {
      return Math.random();
    },
    getIdTaken(number) {
      return this.tasks.find((task) => task.id === number) ? true : false;
    },
    updateStatus(statusDO) {
      const task = this.tasks.find(
        (task) => task.id === Number(statusDO.taskId)
      );
      task.status = statusDO.newStatus;
    },
  },
};
</script>

<style>
@import "~bootstrap/dist/css/bootstrap.min.css";
</style>
