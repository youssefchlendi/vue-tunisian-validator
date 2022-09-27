<template>
  <div :class="valid ? '' : 'vtv-invalid'">
    <slot></slot>
  </div>
  <div class="vtv-error-message" v-if="!valid">
    {{ validationMessage }}
  </div>
  {{ valid }}
</template>
<script setup>
import { ref, toRefs, defineProps, watch } from "vue";
const props = defineProps({
  type: {
    type: String,
    default: "text",
  },
  value: {
    default: "",
  },
  message: {
    type: String,
    default: "",
  },
});
const { type, value, message } = toRefs(props);
const inputType = ref();
const types = {
  cin: "number",
  phone: "tel",
  email: "email",
  password: "password",
};
const valid = ref(true);
const validationMessage = ref("");
inputType.value = types[type.value];
const validateCin = () => {
  const cin = value.value;
  if (isNaN(cin)) {
    valid.value = false;
    validationMessage.value = message.value || "CIN must be a number";
  }
  //   check if the cin is 8 digits and the first digit is 1 or 0, if not check if the cin is between 1 and 7 digits
  let regex = /^$/;
  if ((cin + "").length == 8) regex = /^(1|0)\d{7}$/;
  else if ((cin + "").length < 8) regex = /^\d{1,7}$/;
  valid.value = regex.test(cin);
  validationMessage.value = message.value || "CIN is not valid";
};
const validatePhone = () => {
  const phone = value.value;
  if (isNaN(phone)) {
    valid.value = false;
    validationMessage.value = message.value || "Phone must be a number";
  }
  //   check if the phone is 8 digits and the first digit is 2 or 5 or 9
  let regex = /^[2,5,9]\d{7}$/;
  valid.value = regex.test(phone);
  validationMessage.value = message.value || "Phone is not valid";
};
const validate = () => {
  if (type.value === "cin") {
    validateCin();
  }
  if (type.value === "phone") {
    validatePhone();
  }
};
watch(value, validate);
</script>
<style lang="scss">
.vtv-invalid {
  width: fit-content;
  border: 2px solid red;
}
.vtv-error-message {
  display: block;
  color: red;
}
</style>
