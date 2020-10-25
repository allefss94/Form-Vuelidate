<template>
  <div class="container">
    <h1>Trabalhando com Formulários</h1>
    <form
      @submit.prevent="submit"
      @reset.prevent="reset"
      class="form-container"
    >
      <div class="input-group">
        <label for="phone">Telefone:</label>
        <input
          type="tel"
          placeholder="seu numero de celular"
          v-mask="'(##)#####-####'"
          v-model.trim.lazy="$v.phone.$model"
        />
      </div>
      <div class="error" v-if="!$v.phone.required && $v.phone.$dirty">
        Campo obrigatório.
      </div>
      <div class="error" v-if="!$v.phone.minLength && $v.phone.$dirty">
        O telefone deve ter 9 digitos.
      </div>

      <div class="input-group">
        <label for="phone">CPF:</label>
        <input
          type="num"
          placeholder="CPF"
          v-mask="'###.###.###-##'"
          v-model.trim.lazy="$v.cpf.$model"
        />
      </div>
      <div class="error" v-if="!$v.cpf.required && $v.cpf.$dirty">
        Campo obrigatório.
      </div>
      <div class="error" v-if="!$v.cpf.minLength">
        O CPF deve ter 11 numeros.
      </div>
      <div class="buttons">
        <button type="submit">enviar</button>
        <button type="reset">limpar</button>
      </div>
      <transition name="fade" mode="out-in">
        <p class="badge success" v-if="stateForm === 'validRequest'">
          Tudo certo
        </p>
        <p class="badge warning" v-if="stateForm === 'invalidRequest'">
          Preencha os campos corretamente.
        </p>
      </transition>
    </form>
  </div>
</template>

<script>
import { required, minLength } from "vuelidate/lib/validators";
import { mask } from "vue-the-mask";
export default {
  directives: { mask },
  name: "vue",
  data() {
    return {
      phone: "",
      cpf: "",
      stateForm: null,
    };
  },
  methods: {
    reset() {
      (this.phone = ""), (this.cpf = ""), (this.stateForm = null);
    },
    submit() {
      this.$v.$touch();
      if (this.$v.$invalid) {
        setTimeout(() => {
          this.stateForm = "invalidRequest";
        }, 1000);
      } else {
        setTimeout(() => {
          this.stateForm = "validRequest";
          (this.phone = ""), (this.cpf = "");
        }, 1000);
      }
    },
  },
  validations: {
    phone: {
      required,
      minLength: minLength(13),
    },
    cpf: {
      required,
      minLength: minLength(14),
    },
  },
};
</script>

<style scoped>
.container {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  background: #eee;
}

.form-container {
  border: 1px solid black;
  border-radius: 4px;
  padding: 20px;
  width: 40%;
  display: grid;
  grid-template-columns: 1fr;
}

.input-group {
  display: flex;
  flex-direction: column;
  align-content: flex-start;
}
.input-group label {
  margin: 5px 0;
}

.input-group input {
  padding: 0.6rem;
  border-radius: 4px;
  color: #333;
  border: 1px solid #333;
}

.error {
  font-size: 11px;
  font-style: italic;
  padding: 5px;
}
button:nth-child(1) {
  margin-right: 15px;
}
button {
  padding: 0.6rem;
  color: #eee;
  background: #432d92;
  border: 1px solid #342374;
  border-radius: 4px;
  font-size: 16px;
  margin: 20px 0 0;
  cursor: pointer;
  transition: all 0.2s;
  text-transform: uppercase;
}
button:hover {
  background: #342374;
  border: 1px solid #432d92;
}

.badge {
  border: 1px solid;
  border-radius: 4px;
  padding: 5px;
  text-align: center;
  color: #eee;
}
.success {
  background: #13972985;
}
.warning {
  background: #b62d288a;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
