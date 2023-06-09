<template>
  <div>
    <SignIn @enter="step = 'Mode-Prescription'" v-if="step === 'Mode-SignIn'" />
    <SignUp @login="step = 'Mode-SignIn'" v-if="step === 'Mode-SignUp'" />
    <Prescription @model="step= 'Mode-Model'" v-if="step === 'Mode-Prescription'" @view-lesson="showLesson" />
    <Lesson @main="step = 'Mode-Prescription'" v-if="step == 'Mode-View-Lesson'" :lesson="lesson" />
    <!-- <Model  v-if="step === 'Mode-Model'" :model="model" /> -->
    <Model @main="step = 'Mode-Prescription'" v-if="step === 'Mode-Model'" />
    <!-- <Model @model="step = 'Mode-Model'" v-if="step === 'Mode-SignUp'" /> -->
  </div>
</template>

<script>
import SignUp from "./signup.vue";
import SignIn from "./signin.vue";
import Prescription from "./prescription.vue";
import Lesson from './lesson.vue';
import Model from './model.vue';


export default {
  components: { SignIn, SignUp, Prescription, Lesson, Model },
  data() {
    return {
      step: "Mode-SignUp",
      lesson: 0
    };
  },
  beforeMount() {
        const parts = location.pathname.split("/");
        if (parts[1] == "lesson") {
            this.lesson = parts[2];
            this.step = "Mode-View-Lesson";
        } else {
            this.step = "Mode-SignUp";
        }
    },
  methods: {
    handleEnter() {
      this.step = "Mode-Prescription";
    }
  },
    showLesson(id) {
    this.lesson = id;
    this.step = "Mode-View-Lesson";
  },
  goToModel(){
    this.step = "Mode-Model";
  }



};
</script>

<style src="./styles.css" scoped></style>
