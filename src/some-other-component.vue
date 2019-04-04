<template>
  <div>{{ test }}</div>
</template>

<script lang="ts">
import {Component, Vue} from 'vue-property-decorator';

@Component({})
export default class SomeOtherComponent extends Vue {
  test = "Hello"
}
</script>
