<template>
  <AttributeForm
    :initial-values="initialValues"
    submit-button-label="Edytuj"
    @submit="onSubmit"
  />
</template>

<script setup lang="ts">
const attributeStorage = useAttributeStorage();
const modal = useAttributeModalEdit();

export interface Props {
  attributeId: string;
  initialValues: {
    name: string;
    type: string;
    values: string[];
  };
}

const props = defineProps<Props>();

const onSubmit = (values) => {
  attributeStorage.updateEntity({
    id: props.attributeId,
    name: values.name,
    type: values.type,
    values: values.values,
  });
  modal.hide();
};
</script>
