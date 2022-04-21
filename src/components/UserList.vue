<template>
  <div class="row">
    <div class="col s4" v-for="user in users.data" :key="user.id">
      <ul class="collection">
        <li class="collection-item avatar">
          <img :src="user.avatar" alt="" class="circle" />
          <span class="title">ID - {{ user.id }}</span>
          <p>
            {{ user.first_name }} - {{ user.last_name }} <br />
            {{ user.email }}
          </p>
          <a
            href="#modal1"
            class="secondary-content waves-effect waves-light modal-trigger"
            @click="$emit('modal', user)"
            ><i class="material-icons">person</i></a
          >
          <div id="modal1" class="modal">
            <div class="modal-content">
              <h4>{{ modalData.modalName }} {{ modalData.modalSurname }}</h4>
            </div>
            <form class="col s10">
              <div class="row">
                <div class="input-field col s2">
                  <input
                    id="first_name"
                    type="text"
                    class="validate"
                    v-bind:value="modalData.modalName"
                    @input="$emit('input', $event.target.value)"
                  />
                  <label for="first_name" class="active">First Name</label>
                </div>
                <div class="input-field col s2">
                  <input
                    id="last_name"
                    type="text"
                    v-bind:value="modalData.modalSurname"
                    v-on:input="$emit('input', $event.target.value)"
                  />
                  <label for="last_name" class="active">Last Name</label>
                </div>
                <div class="input-field col s4">
                  <input
                    id="email"
                    type="text"
                    class="validate"
                    v-bind:value="modalData.modalEmail"
                    v-on:input="$emit('input', $event.target.value)"
                  />
                  <label for="email" class="active">Email</label>
                </div>
              </div>
            </form>
            <div class="modal-footer">
              <a href="#!" class="modal-close waves-effect waves-green btn-flat"
                >Save</a
              >
            </div>
          </div>
        </li>
      </ul>
    </div>

    <!-- Modal Structure -->
  </div>
</template>

<script>
export default {
  props: {
    users: {
      type: Object,
      required: true,
    },
    modalData: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped></style>
