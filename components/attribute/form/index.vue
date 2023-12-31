<template>
  <Form
    v-slot="{ resetForm, setFieldValue, errors, meta }"
    :validation-schema="validationSchema"
    class="flex flex-col gap-4"
    @submit="(values) => emit('submit', values)"
  >
    <Field
      type="text"
      label="Nazwa"
      name="name"
      v-bind="initialValues?.name && { defaultValue: initialValues.name }"
    />

    <Field
      is="select"
      label="Typ"
      name="type"
      v-bind="initialValues?.type && { defaultValue: initialValues.type }"
      disabled
    >
      <option value="select" selected>Select</option>
    </Field>
    <AttributeValuesInput
      :default-values="initialValues.values"
      v-bind="
        meta.touched && errors.values ? { errorMessage: errors.values } : {}
      "
      :reset="reset"
      @update-values="(values) => setFieldValue('values', values)"
    />

    <ButtonPrimary type="submit">{{ submitButtonLabel }}</ButtonPrimary>
    <ButtonSecondary
      type="button"
      @click="
        initialValues
          ? (resetForm({ values: { ...initialValues } }), handleReset())
          : resetForm()
      "
    >
      Resetuj
    </ButtonSecondary>
  </Form>
</template>

<script setup lang="ts">
import { Form } from "vee-validate";

const validationSchema = useAttributeValidationSchema();

export interface Props {
  submitButtonLabel: string;
  initialValues?: {
    name?: string;
    type?: ProductAttributeType;
    values?: string[];
  };
}
const props = withDefaults(defineProps<Props>(), {
  initialValues: {
    type: "select",
    values: [],
  },
});

const initialValuesCopy = ref({ ...props.initialValues });

for (let i = 0; i < initialValuesCopy.value.values.length; i++) {
  const key = `value-${i}`;
  const value = initialValuesCopy.value.values[i];
  props.initialValues[key] = value;
}

const emit = defineEmits<{ submit: [any] }>();

const reset = ref(false);

const handleReset = () => {
  reset.value = !reset.value;
};
</script>
