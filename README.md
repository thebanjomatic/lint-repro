# lint-repro

This reproducer demonstrates a bug that occurs when using `eslint-plugin-vue` combined with `@typescript-eslint/parser` versions `1.5.0` and later.

It appears to have been introduced by the following pull request https://github.com/typescript-eslint/typescript-eslint/pull/367.

From what I can tell, it looks like the `calculateProjectParserOptions` function gets called multiple times for the same file when the `eslint-vue-plugin` is involved for different portions of the file. For example, if your SFC has an interpolation / mustache block in the template:
```vue
<template>
  <div>{{ test }}</div>
</template>
<script>
<script lang="ts">
import {Component, Vue} from 'vue-property-decorator';

@Component({})
export default class SomeComponent extends Vue {
  test = "World";
}
</script>
```
then calculateProjectParserOptions winds up getting called twice on this file with the same `filePath` given, but the `code` provided contains first the contents of the interpolation from the template, and then a second time with contents from the script tag.

This seems to break the assumption made in https://github.com/typescript-eslint/typescript-eslint/pull/367 which stated:
> Here we cut down the number of times this happens by ensuring we only tell the TS API that a file has changed when we lint a file for a second time (thereby implying a --fix mode or "watch" mode).


# Steps to reproduce

```bash
yarn
yarn lint
```

```
2:28  error  Parsing error: Unexpected end of expression  vue/no-parsing-error
✖ 1 problem (1 error, 0 warnings)
```

# To see expected behavior

set `"@typescript-eslint": "1.4.2"` in package.json and use the same steps as above. Should provide no linter errors.
