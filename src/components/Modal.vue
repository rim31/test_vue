<template>
  <div class="bloc-modale" v-if="reveal">
    <div class="overlay" v-on:click="toggleModal"></div>
    <div class="modale card">
      <div v-on:click="toggleModal" class="btn-modale btn btn-danger">X</div>
      <h1>Details : #{{ object }}</h1>
      <h1>Details : #{{ object.id }}</h1>
      <div>first name : {{ object.first_name }}</div>
      <div>last name : {{ object.last_name }}</div>
      <div>email : {{ object.email }}</div>
      <div>gender : {{ object.gender }}</div>
      <div>country code : {{ object.country_code }}</div>
      <div>phone : {{ object.phone }}</div>
      <div>birthday : {{ object.birthday }}</div>
      <div>contact channel : {{ object.contact_channel }}</div>
      <div>status : {{ object.status }}</div>
      <div>interaction creation date : {{ object.interaction_creation_date }}</div>
      <div>due date : {{ object.due_date }}</div>
      <div>attachments : {{ object.attachments }}</div>
      <div>last comment : {{ object.last_comment }}</div>
      <div>assignedTO : {{ object.assignedTO }}</div>
    </div>
  </div>
</template>


<script>
export default {
  name: "Modal",
  props: ["reveal", "toggleModal", "object"],
};
</script>


<style scoped>
.bloc-modale {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.overlay {
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.modale {
  background: #f1f1f1;
  color: #333;
  padding: 50px;
  position: fixed;
  top: 30%;
}

.btn-modale {
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>