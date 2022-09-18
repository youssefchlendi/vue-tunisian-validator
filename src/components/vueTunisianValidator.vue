<template>
  <div :class="valid ? '' : 'vtv-invalid'">
    <slot></slot>
    {{ valid }}
  </div>
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
});
const { type, value } = toRefs(props);
const inputType = ref();
const types = {
  cin: "number",
  phone: "tel",
  email: "email",
  password: "password",
};
const valid = ref(true);

inputType.value = types[type.value];
const validateCin = () => {
  const cin = value.value;
  if (isNaN(cin)) {
    return false;
  }
  //   check if the cin is 8 digits and the first digit is 1 or 0, if not check if the cin is between 1 and 7 digits
  let regex = /^$/;
  if ((cin + "").length == 8) regex = /^(1|0)\d{7}$/;
  else if ((cin + "").length < 8) regex = /^\d{1,7}$/;
  return regex.test(cin);
};
const validate = () => {
  if (type.value === "cin") {
    valid.value = validateCin();
  }
};
watch(value, validate);
</script>
<style lang="scss">
.vtv-invalid {
  input {
    border-color: red;
  }
}
</style>
