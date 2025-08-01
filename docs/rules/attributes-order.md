---
pageClass: rule-details
sidebarDepth: 0
title: vue/attributes-order
description: enforce order of attributes
since: v4.3.0
---

# vue/attributes-order

> enforce order of attributes

- :gear: This rule is included in all of `"plugin:vue/recommended"`, `*.configs["flat/recommended"]`, `"plugin:vue/vue2-recommended"` and `*.configs["flat/vue2-recommended"]`.
- :wrench: The `--fix` option on the [command line](https://eslint.org/docs/user-guide/command-line-interface#fix-problems) can automatically fix some of the problems reported by this rule.

## :book: Rule Details

This rule aims to enforce ordering of component attributes. The default order is specified in the [Vue.js Style Guide](https://vuejs.org/style-guide/rules-recommended.html#element-attribute-order) and is:

- `DEFINITION`
  e.g. 'is', 'v-is'
- `LIST_RENDERING`
  e.g. 'v-for item in items'
- `CONDITIONALS`
  e.g. 'v-if', 'v-else-if', 'v-else', 'v-show', 'v-cloak'
- `RENDER_MODIFIERS`
  e.g. 'v-once', 'v-pre'
- `GLOBAL`
  e.g. 'id'
- `UNIQUE`
  e.g. 'ref', 'key'
- `SLOT`
  e.g. 'v-slot', 'slot'.
- `TWO_WAY_BINDING`
  e.g. 'v-model'
- `OTHER_DIRECTIVES`
  e.g. 'v-custom-directive'
- `OTHER_ATTR`
  alias for `[ATTR_DYNAMIC, ATTR_STATIC, ATTR_SHORTHAND_BOOL]`:
  - `ATTR_DYNAMIC`
    e.g. 'v-bind:prop="foo"', ':prop="foo"'
  - `ATTR_STATIC`
    e.g. 'prop="foo"', 'custom-prop="foo"'
  - `ATTR_SHORTHAND_BOOL`
    e.g. 'boolean-prop'
- `EVENTS`
  e.g. '@click="functionCall"', 'v-on="event"'
- `CONTENT`
  e.g. 'v-text', 'v-html'

### the default order

<eslint-code-block fix :rules="{'vue/attributes-order': ['error']}">

```vue
<template>
  <!-- ✓ GOOD -->
  <div
    is="header"
    v-for="item in items"
    v-if="!visible"
    v-once
    id="uniqueID"
    ref="header"
    v-model="headerData"
    my-prop="prop"
    @click="functionCall"
    v-text="textContent">
  </div>
  <div
    v-for="item in items"
    v-if="!visible"
    prop-one="prop"
    :prop-two="prop"
    prop-three="prop"
    @click="functionCall"
    v-text="textContent">
  </div>
  <div
    prop-one="prop"
    :prop-two="prop"
    prop-three="prop">
  </div>

  <!-- ✗ BAD -->
  <div
    ref="header"
    v-for="item in items"
    v-once
    id="uniqueID"
    v-model="headerData"
    my-prop="prop"
    v-if="!visible"
    is="header"
    @click="functionCall"
    v-text="textContent">
  </div>
</template>
```

</eslint-code-block>

Note that `v-bind="object"` syntax is considered to be the same as the next or previous attribute categories.

<eslint-code-block fix :rules="{'vue/attributes-order': ['error']}">

```vue
<template>
  <!-- ✓ GOOD (`v-bind="object"` is considered GLOBAL category) -->
  <MyComponent
    v-bind="object"
    id="x"
    v-model="x"
    v-bind:foo="x">
  </MyComponent>

  <!-- ✗ BAD (`v-bind="object"` is considered UNIQUE category) -->
  <MyComponent
    key="x"
    v-model="x"
    v-bind="object">
  </MyComponent>
</template>
```

</eslint-code-block>

## :wrench: Options

```json
{
  "vue/attributes-order": ["error", {
    "order": [
      "DEFINITION",
      "LIST_RENDERING",
      "CONDITIONALS",
      "RENDER_MODIFIERS",
      "GLOBAL",
      ["UNIQUE", "SLOT"],
      "TWO_WAY_BINDING",
      "OTHER_DIRECTIVES",
      "OTHER_ATTR",
      "EVENTS",
      "CONTENT"
    ],
    "alphabetical": false,
    "sortLineLength": false
  }]
}
```

### `"alphabetical": true`

<eslint-code-block fix :rules="{'vue/attributes-order': ['error', {alphabetical: true}]}">

```vue
<template>
  <!-- ✓ GOOD -->
    <div
      a-custom-prop="value"
      :another-custom-prop="value"
      :blue-color="false"
      boolean-prop
      class="foo"
      :class="bar"
      z-prop="Z"
      v-on:[c]="functionCall"
      @change="functionCall"
      v-on:click="functionCall"
      @input="functionCall"
      v-text="textContent">
    </div>

  <!-- ✗ BAD -->
    <div
      z-prop="Z"
      a-prop="A">
    </div>

    <div
      @input="bar"
      @change="foo">
    </div>

    <div
      v-on:click="functionCall"
      v-on:[c]="functionCall">
    </div>

    <div
      :z-prop="Z"
      :a-prop="A">
    </div>

    <div
      :class="foo"
      class="bar">
    </div>

</template>
```

</eslint-code-block>

### `"sortLineLength": true`

<eslint-code-block fix :rules="{'vue/attributes-order': ['error', {sortLineLength: true}]}">

```vue
<template>
  <!-- ✓ GOOD -->
    <div
      a="short"
      abc="value"
      a-prop="longer"
      boolean-prop
      :my-prop="value"
      very-long-prop="value"
      @blur="functionCall"
      @change="functionCall"
      @input="handleInput">
    </div>

  <!-- ✗ BAD -->
    <div
      very-long-prop="value"
      a="short"
      a-prop="longer">
    </div>

    <div
      @input="handleInput"
      @blur="short">
    </div>

    <div
      :my-prop="value"
      :a="short">
    </div>

</template>
```

</eslint-code-block>

### `"alphabetical": true` with `"sortLineLength": true`

When `alphabetical` and `sortLineLength` are both set to `true`, attributes within the same group are sorted primarily by their line length, and then alphabetically as a tie-breaker for attributes with the same length. This provides a clean, predictable attribute order that enhances readability.

<eslint-code-block fix :rules="{'vue/attributes-order': ['error', {alphabetical: true, sortLineLength: true}]}">

```vue
<template>
  <!-- ✓ GOOD -->
  <div
    a="1"
    b="2"
    cc="3"
    dd="4"
    @keyup="fn"
    @submit="fn"
  ></div>

  <!-- ✗ BAD -->
  <div
    b="2"
    a="1"
    @submit="fn"
    @keyup="fn"
    dd="4"
    cc="3"
  ></div>
</template>
```

</eslint-code-block>

### Custom orders

#### `['LIST_RENDERING', 'CONDITIONALS', 'RENDER_MODIFIERS', 'GLOBAL', 'UNIQUE', 'TWO_WAY_BINDING', 'DEFINITION', 'OTHER_DIRECTIVES', 'OTHER_ATTR', 'EVENTS', 'CONTENT']`

<eslint-code-block fix :rules="{'vue/attributes-order': ['error', {order: ['LIST_RENDERING', 'CONDITIONALS', 'RENDER_MODIFIERS', 'GLOBAL', 'UNIQUE', 'TWO_WAY_BINDING', 'DEFINITION', 'OTHER_DIRECTIVES', 'OTHER_ATTR', 'EVENTS', 'CONTENT']}]}">

```vue
<template>
  <!-- ✓ GOOD -->
  <div
    ref="header"
    is="header"
    prop-one="prop"
    prop-two="prop">
  </div>

  <!-- ✗ BAD -->
  <div
    ref="header"
    prop-one="prop"
    is="header">
  </div>
</template>
```

</eslint-code-block>

#### `[['LIST_RENDERING', 'CONDITIONALS', 'RENDER_MODIFIERS'], ['DEFINITION', 'GLOBAL', 'UNIQUE'], 'TWO_WAY_BINDING', 'OTHER_DIRECTIVES', 'OTHER_ATTR', 'EVENTS', 'CONTENT']`

<eslint-code-block fix :rules="{'vue/attributes-order': ['error', {order: [['LIST_RENDERING', 'CONDITIONALS', 'RENDER_MODIFIERS'], ['DEFINITION', 'GLOBAL', 'UNIQUE'], 'TWO_WAY_BINDING', 'OTHER_DIRECTIVES', 'OTHER_ATTR', 'EVENTS', 'CONTENT']}]}">

```vue
<template>
  <!-- ✓ GOOD -->
  <div
    ref="header"
    is="header"
    prop-one="prop"
    prop-two="prop">
  </div>
  <div
    is="header"
    ref="header"
    prop-one="prop"
    prop-two="prop">
  </div>
</template>
```

</eslint-code-block>

## :books: Further Reading

- [Style guide - Element attribute order](https://vuejs.org/style-guide/rules-recommended.html#element-attribute-order)
- [Style guide (for v2) - Element attribute order](https://v2.vuejs.org/v2/style-guide/#Element-attribute-order-recommended)

## :rocket: Version

This rule was introduced in eslint-plugin-vue v4.3.0

## :mag: Implementation

- [Rule source](https://github.com/vuejs/eslint-plugin-vue/blob/master/lib/rules/attributes-order.js)
- [Test source](https://github.com/vuejs/eslint-plugin-vue/blob/master/tests/lib/rules/attributes-order.js)
