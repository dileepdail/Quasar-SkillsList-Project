<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newSkill"
        @keyup.enter="addSkills()"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add Skills"
        dense>
        <template v-slot:before>
          <q-icon name="event" />
        </template>
        <template v-slot:append>
          <q-btn
            @click="addSkills()"
            round
            dense
            flat
            icon="add" />
        </template>
      </q-input>
    </div>
    <q-list
      class="bg-white"
      separator
      bordered>
      <q-item
        v-for="(skill, index) in skills"
        :key="index"
        @click="skill.done = !skill.done"
        :class="{ 'done bg-blue-1' : skill.done }"
        clickable
        v-ripple>
        <q-item-section avatar>
          <q-checkbox
            v-model="skill.done"
            color="teal"
            class="no-pointer-events" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ skill.title }}</q-item-label>
        </q-item-section>

        <q-item-section
          v-if="skill.done"
          side>
          <q-btn
            @click.stop="deleteSkill(index)"
            push
            color="primary"
            round
            dense
            icon="delete" />
        </q-item-section>

      </q-item>
    </q-list>
    <div
      v-if="!skills.length"
      class="no-skills absolute-center">
      <q-icon
        name="check"
        size="100px"
        color="primary"
      />
      <div class="text-h5 text-primary text-center">
        No Skills
      </div>
    </div>
  </q-page>
</template>

<script>
export default {

  data() {

    return {
      newSkill: '',
      skills: [
        /*{
          title: 'Task1',
          done: false
        },
        {
          title: 'Task2',
          done: false
        },
        {
          title: 'Task3',
          done: false
        }*/
      ]
    }
  },
  methods : {
    deleteSkill(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Do you want to delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.skills.splice(index,1);
        this.$q.notify('Skill deleted')
      })
    },
    addSkills() {
      //console.log("In add Task");
      this.skills.push({
        title: this.newSkill,
        done: false
      })
      this.newSkill = '';
    }
  }

}
</script>

<style lang="scss">
  .done {
    .q-item__label {
      text-decoration: line-through;
      color: #bbb;

    }
  }
  .no-skills {
    opacity: 0.5;
  }
</style>
