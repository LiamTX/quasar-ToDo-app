<template>
  <div class="">
    <q-btn label="Editar" color="dark" @click="small = true" />
    <!-- <q-btn label="Medium" color="primary" @click="medium = true" /> -->

    <q-dialog v-model="small">
      <q-card style="width: 300px">
        <q-card-section>
          <div class="text-h6">Suas Informações.</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-form class="q-gutter-md">
            <q-avatar size="56px" class="q-mb-sm">
              <img v-if="user.avatar != ''" :src="user.avatar" />
              <img v-else src="https://cdn.quasar.dev/img/boy-avatar.png" />
            </q-avatar>
            <q-input
              filled
              v-model="user.name"
              label="Qual o seu nome?"
              lazy-rules
              :rules="[
                (val) => (val && val.length > 0) || 'Please type something',
              ]"
            />

            <div>
              <q-btn label="Salvar" color="dark" @click="salvar" />
            </div>
          </q-form>
        </q-card-section>
      </q-card>
    </q-dialog>

    <!-- <q-dialog v-model="medium">
      <q-card style="width: 700px; max-width: 80vw">
        <q-card-section>
          <div class="text-h6">Medium</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          Click/Tap on the backdrop.
        </q-card-section>

        <q-card-actions align="right" class="bg-white text-teal">
          <q-btn flat label="OK" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: {
        avatar: "",
        name: "",
      },
      file: "",
      small: false,
      medium: false,
    };
  },
  methods: {
    filePicker(){
      console.log(this.file)
    },
    // onFilePicked(event) {
    //   const files = event.target.files;
    //   let fileName = files[0].filename;
    //   this.imageData = event.target.files[0];

    //   const fileReader = new FileReader();
    //   fileReader.addEventListener("load", () => {
    //     this.user.avatar = fileReader.result;
    //   });
    //   fileReader.readAsDataURL(files[0]);
    // },
    salvar() {
      if (this.user.name === "") {
        this.$q.notify({
          message: "Preencha o campo nome.",
          color: "black",
        });
        return;
      }
      console.log(this.file)
      this.$q.localStorage.set("user", this.user);
      this.small = false;
      this.$emit("userChange", this.user);
    },
  },
};
</script>