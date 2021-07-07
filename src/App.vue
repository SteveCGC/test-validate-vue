<template>
  <h1>Element Plus + vee-validate</h1>

  <Form ref="formref" :validation-schema="schema">
    <!-- You can use the field component to wrap a 'el-form-item' component -->
    <!-- Do this if you have only one or a few places that need validation -->
    <Field name="email1" v-slot="{ field, errorMessage }">
      <el-form-item :error="errorMessage">
        <el-input
          v-model="modelEmail"
          placeholder="Email Address"
          v-bind="field"
        />
      </el-form-item>
    </Field>

    <!-- Or compose vee-validate's validation into your element form components -->
    <!-- Do this if you plan to have validation frequently throughout your app -->
    <InputWithValidation label="Full name" name="fullName" />
    <InputWithValidation label="Password" name="password" type="password" />

    <!-- The same wrapping technique can be applied to special form elements like checkboxs but with some variations -->
    <div>
      <Field
        name="terms"
        type="checkbox"
        :value="true"
        :unchecked-value="false"
        v-slot="{ value, errorMessage, handleChange }"
      >
        <el-form-item :error="errorMessage">
          <el-checkbox :model-value="value" @update:model-value="handleChange">
            Agree to terms and conditions
          </el-checkbox>
        </el-form-item>
      </Field>
    </div>

    <div>
      <!-- Even better with the composition API -->
      <CheckboxWithValidation
        name="subscribed"
        label="Subscribe to latest news"
      />
    </div>

    <div>
      <el-button type="primary" onClick="onSubmit">Submit</el-button>
    </div>
  </Form>
</template>

<script>
import { Field, Form } from "vee-validate";
import { nextTick, ref } from "vue";
import * as yup from "yup";
import InputWithValidation from "./components/InputWithValidation.vue";
import CheckboxWithValidation from "./components/CheckboxWithValidation.vue";

export default {
  name: "App",
  components: {
    Form,
    Field,
    InputWithValidation,
    CheckboxWithValidation,
  },
  setup() {
    const modelEmail = ref("");
    const formref = ref(null);
    const schema = yup.object({
      email: yup.string().required().email().label("Email address"),
      fullName: yup.string().required().label("Full name"),
      password: yup.string().required().min(6).label("Password"),
      terms: yup
        .boolean()
        .required()
        .isTrue("You must agree to terms and conditions")
        .label("terms agreement"),
      subscribed: yup
        .boolean()
        .required()
        .isTrue("This is unusal but you have to subscribe")
        .label("subscription agreement"),
    });

    function onSubmit(values, actions) {
      // console.log(JSON.stringify(values, null, 2));
      // actions.resetForm();
      // nextTick(() => {
      //   console.log(formref);
      //   // formref.value.validate().then((flag) => {
      //   //   console.log(flag);
      //   // });
      // });
    }

    return {
      onSubmit,
      schema,
      modelEmail,
    };
  },
};
</script>

<style>
body {
  font-family: Helvetica Neue, Helvetica, PingFang SC, Hiragino Sans GB,
    Microsoft YaHei, SimSun, sans-serif;
}
</style>