<template>
  <div>
    <JsonFormNode
      v-for="(value, propName) in template"
      :key="propName"
      :propName="propName"
      :value="value"
      :labels="labels"
    ></JsonFormNode>
  </div>
</template>

<script>
  import JsonFormNode from "@/components/JsonFormNode";

  export default {
    name: "JsonFormInput",
    components: {
      JsonFormNode,
    },
    props: {
      template: {
        type: Object,
        required: true,
      },
      labels: {
        type: Object,
      },
    },
  };
</script>

<style scoped></style>
