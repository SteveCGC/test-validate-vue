<template>
  <el-form-item :error="errorMessage">
    <el-input :placeholder="label" v-model="value" v-bind="$attrs" />
  </el-form-item>
</template>

<script>
import { useField } from "vee-validate";

export default {
  name: "InputWithValidation",
  inheritAttrs: false,
  props: {
    name: {
      type: String,
      required: true,
    },
    label: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    const { value, errorMessage } = useField(props.name);

    return {
      value,
      errorMessage,
    };
  },
};
</script>