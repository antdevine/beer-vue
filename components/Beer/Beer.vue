<template>
    <div class="Card h-full p-4">
      <div class="Card-image">
        <img 
        :src="imageUrl" 
        :alt="title" 
        class="w-full" />
      </div>
      <h3 class="font-bold text-lg mt-0" v-if="title" v-html="title" />
      <h4 class="font-bold text-lg mt-0" v-if="tagline" v-html="tagline" />
      <p v-if="description" v-html="description" />
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

