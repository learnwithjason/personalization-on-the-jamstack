<template>
  <div class="container">
    <composition :composition="composition">
      <template #default="{ parameters }">
        <h1>{{ parameters.title.value }}</h1>
        <slot-content slot-name="memories" key="memories" />
      </template>
    </composition>
  </div>
</template>

<script>
import {
  createContentfulEnhancer,
  CANVAS_CONTENTFUL_PARAMETER_TYPES,
} from '@uniformdev/canvas-contentful'
import { enhance, EnhancerBuilder } from '@uniformdev/canvas'
import { createClient } from 'contentful'

export default {
  async asyncData({ $uniformCanvasNuxt }) {
    const { composition } = await $uniformCanvasNuxt.getCompositionBySlug({
      slug: '/memories',
    })

    const client = createClient({
      // NOTE: for production code ensure you use environment variables to configure Contentful, not hard-coded values.
      space: 'f4xt4xefz3kw',
      // only needed if not the default 'master' environment
      environment: 'master',
      accessToken: 'RorpOqT6gqHJppa0mptQGOEdtO3HBgkivY2f5YAeGp4',
    })

    const contentfulEnhancer = createContentfulEnhancer({ client })

    // apply the enhancers to the composition data, enhancing it with external data
    // in this case, the _value_ of the Contentful parameter you created is enhanced
    // from the entry ID it is linked to, to the Contentful entry REST API response
    // for that entry. You can create your own enhancers easily; they are a simple function.
    await enhance({
      composition,
      enhancers: new EnhancerBuilder().parameterType(
        CANVAS_CONTENTFUL_PARAMETER_TYPES,
        contentfulEnhancer
      ),
      context: {},
    })

    return { composition }
  },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
