<template>
  <Box class="w-full h-min">
    <BoxTitle>produkty</BoxTitle>
    <p
      v-if="search.productResults.value.length === 0"
      class="text-l mt-4 normalcase text-gray-400"
    >
      Brak wyników wyszukiwania
    </p>
    <div v-else class="mt-4 flex flex-col gap-y-4">
      <SearchResultItem
        v-for="product in search.productResults.value"
        :id="product.id"
        :key="product.id"
        :name="product.name"
        @click="editProduct(product.id)"
      />
    </div>
  </Box>
  <Box class="w-full h-min">
    <BoxTitle>atrybuty</BoxTitle>
    <p
      v-if="search.attributeResults.value.length === 0"
      class="text-l mt-4 normalcase text-gray-400"
    >
      Brak wyników wyszukiwania
    </p>
    <div v-else class="mt-4 flex flex-col gap-y-4">
      <SearchResultItem
        v-for="attribute in search.attributeResults.value"
        :id="attribute.id"
        :key="attribute.id"
        :name="attribute.name"
        @click="editAttribute(attribute.id)"
      />
    </div>
  </Box>
</template>

<script setup lang="ts">
const search = useSearch();
const router = useRouter();
const attributeModalEdit = useAttributeModalEdit();
const attributeStorage = useAttributeStorage();
const productModalEdit = useProductModalEdit();
const productStorage = useProductStorage();

const editAttribute = (attributeId: string) => {
  attributeModalEdit.show({
    attributeId,
    initialValues: attributeStorage.getEntity(attributeId),
  });
  router.push({ path: "/attributes" });
  search.reset();
};

const editProduct = (productId: string) => {
  productModalEdit.show({
    productId,
    initialValues: productStorage.getEntity(productId),
  });
  router.push({ path: "/products" });
  search.reset();
};
</script>
