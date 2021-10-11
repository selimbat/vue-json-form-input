<template>
  <div>
    <JsonFormNode
      v-for="(value, propName) in template"
      :key="propName"
      :propName="propName"
      :value="value"
      :labels="labels"
      @value-changed="(newValue) => registerChange(propName, newValue)"
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
    data() {
      return {
        model: {},
      };
    },
    methods: {
      registerChange(propName, newValue) {
        this.model[propName] = newValue;
        this.bubble();
      },
      bubble() {
        this.$emit("value-changed", this.model);
      },
    },
  };
</script>

<style scoped></style>
