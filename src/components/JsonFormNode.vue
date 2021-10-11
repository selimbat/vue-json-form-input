<template>
  <div>
    <label v-if="propName" :for="`${$options._scopeId}:${propName}`">
      {{ labels[propName] != null ? labels[propName] : propName }}
    </label>
    <input
      :id="`${$options._scopeId}:${propName}`"
      v-if="isScalar(value)"
      :type="inputType(value)"
      :placeholder="value"
    />
    <JsonFormInput
      class="pad"
      :id="`${$options._scopeId}:${propName}`"
      v-else-if="isObject(value)"
      :template="value"
      :labels="labels"
    ></JsonFormInput>
    <ul v-else :id="`${$options._scopeId}:${propName}`">
      <li v-for="(item, index) in value" :key="index">
        <JsonFormNode :value="item" :labels="labels"></JsonFormNode>
      </li>
    </ul>
  </div>
</template>

<script>
  import JsonFormInput from "@/components/JsonFormInput";

  export default {
    name: "JsonFormNode",
    components: {
      JsonFormInput: () => Promise.resolve(JsonFormInput),
    },
    props: {
      propName: {
        type: String,
      },
      value: {
        required: true,
      },
      labels: {
        type: Object,
      },
    },
    methods: {
      inputType(value) {
        if (this.isScalar(value)) {
          return this.isString(value) ? "text" : "number";
        }
        return "object";
      },
      isScalar(value) {
        return this.isString(value) || this.isNumber(value);
      },
      isString(value) {
        return typeof value === "string";
      },
      isNumber(value) {
        return typeof value === "number";
      },
      isArray(value) {
        return Array.isArray(value);
      },
      isObject(value) {
        return typeof value === "object" && !this.isArray(value);
      },
    },
  };
</script>

<style scoped>
  .pad {
    padding-left: 2em;
    border-left: 2px solid coral;
  }
  ul {
    /*style reset*/
    list-style: none;
    padding: 0;
  }
</style>
