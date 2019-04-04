<template>
  <div>{{ test }}</div>
</template>

<script lang="ts">
import {Component, Vue} from 'vue-property-decorator';

@Component({})
export default class SomeComponent extends Vue {
  test = "World";
}
</script>
