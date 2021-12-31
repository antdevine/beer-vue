<template>
    <div class="Card h-full flex p-14 max-w-5xl">
      <div class="Card-image mr-16">
        <img 
        :src="imageUrl" 
        :alt="title" 
        class="w-full" />
      </div>
      <div>
        <h3 class="font-bold text-6xl mt-0 uppercase" v-if="title" v-html="title" />
        <h4 class="font-bold text-3xl mt-0 text-gray-400" v-if="tagline" v-html="tagline" />
        <p v-if="description" v-html="description" />
      </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
    };
  },
  props: {
    imageUrl: {
        type: String,
        required: true
    },
    title: {
        type: String,
        required: true
    },
    tagline: {
      type: String,
      required: true
    },
    description: {
      type: String,
      required: true
    }
  },
};
</script>

