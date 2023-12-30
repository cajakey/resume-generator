<template>
  <div>
    <a-page-header title="Resumé Generator" />
    <a-row style="padding: 20px;" :gutter="[0, 20]">
      <a-col :span="24">
        <a-steps v-model="current">
          <a-step v-for="(step, index) in steps" :key="index" :title="step.title">
            <a-icon slot="icon" :type="step.icon" />
          </a-step>
        </a-steps>
      </a-col>
      <a-col :span="24">
        <component style="margin: 40px;" :is="content" :form="form" />
      </a-col>
      <a-col :span="24" align="right">
        <a-button v-if="current > 0" @click="current--">
          Previous
        </a-button>
        <a-button v-if="current < steps.length - 1" type="primary" @click="current++">
          Next
        </a-button>
        <a-button v-if="current == steps.length - 1" type="primary" @click="save">
          Save
        </a-button>
      </a-col>
    </a-row>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        current: 0,
        steps: [
          { title: "Personal details", icon: "user", component: "PersonalDetails" },
          { title: "Career history", icon: "profile", component: "CareerHistory" },
          { title: "Education", icon: "read", component: "Education" },
          // { title: "Licences", icon: "idcard", component: "Licences" },
          { title: "Skills", icon: "star", component: "Skills" },
          { title: "Languages", icon: "global", component: "Languages" },
          { title: "Personal Summary", icon: "solution", component: "PersonalSummary" },
          { title: "Done", icon: "check-circle", component: "Done" }
        ],
        form: {
          careerHistory: [],
          education: [],
          licenses: []
        }
      };
    },
    computed: {
      content() {
        if (this.current < this.steps.length) {
          return this.steps[this.current].component;
        }
        return "";
      }
    },
    methods: {
      save() {
        localStorage.setItem("form", JSON.stringify(this.form));
        this.$message.success("Resumé saved");
      }
    },
    created() {
      if (localStorage.getItem("form")) {
        this.form = JSON.parse(localStorage.getItem("form"));
      }
    }
  }
</script>
