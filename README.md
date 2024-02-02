# Payload Live Preview for Vue

This is a port of the Payload CMS Live Preview hook for React into Vue 3. It allows implementing Live Preview functionality to your Vue apps

## Installation

Add the plugin to your Vue 3 or Nuxt 3 app

```bash
pnpm i -D @cgvweb/payload-live-preview-vue
```

## How to use

Inside your Vue 3 or Nuxt 3 app, you can use the `useLivePreview` composable that this package provides inside of your page `setup` function:

```vue
<script setup lang="ts">
import type { PageData } from '~/types';
import { defineProps } from 'vue';
import { useLivePreview } from '@cgvweb/payload-live-preview-vue';

// Fetch the initial data on the parent component or using async state
const props = defineProps<{ initialData: PageData }>();

const { data } = useLivePreview<PageData>({
  initialData: props.initialData, // This is to prevent UI flickering while Live Preview data loads the first time
  serverURL: 'http://localhost:3000', // Recommended: Use environment variables for the server URL
  depth: 2, // Make sure this matches your Payload configuration
});
</script>

<template>
  <h1>{{ data.title }}</h1>
</template>
```

Another example using Nuxt 3 `useAsyncData` or `useFetch` for your page SFC:

```vue
<script setup lang="ts">
import type { PageData } from '~/types';
import { useLivePreview } from '@cgvweb/payload-live-preview-vue';

// Fetch the initial data from your Payload CMS API
const { data: initialData } = await useFetch('http://localhost:3000/api/globals/homepage');

// This data will react to Live Preview changes only from the Payload Admin UI. It doesn't update your live website
const { data } = useLivePreview<PageData>({
  initialData: initialData.value,
  serverURL: 'http://localhost:3000',
  depth: 2,
});
</script>

<template>
  <h1>{{ data.title }}</h1>
</template>
```
