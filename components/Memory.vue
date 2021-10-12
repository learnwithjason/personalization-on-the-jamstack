<template>
  <div>
    <h2>{{ uniformData.parameters.contentfulItem.value.fields.title }}</h2>
    <iframe
      width="560"
      height="315"
      :src="uniformData.parameters.contentfulItem.value.fields.youTubeEmbedUrl"
      title="YouTube video player"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen
    ></iframe>
  </div>
</template>
<script>
export default {
  props: {
    uniformData: {
      type: Object,
      required: true,
    },
  },
}
</script>
