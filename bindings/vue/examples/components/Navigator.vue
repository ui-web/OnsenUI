<template>
  <v-ons-page>
    <v-ons-navigator swipeable
      :page-stack="pageStack"
      :options="{animation: 'slide'}"
      @postpush="log('postpush!')"
      @show="log('show from navigator!')"

      @push="pageStack = [...pageStack, $event]"
      @reset="pageStack = [pageStack[0]]"
      @pop="pageStack.pop()"
    >
    </v-ons-navigator>
  </v-ons-page>
</template>

<script>
  const log = (...args) => console.log(...args);

  const myToolbar = {
    template: `
    <v-ons-toolbar>
      <div class="left"><v-ons-back-button>Back</v-ons-back-button></div>,
      <div class="center"><slot></slot></div>
    </v-ons-toolbar>
    `,
    methods: { log }
  };

  const page3 = {
    name: 'page3',
    template: `
      <v-ons-page p3>
        <my-toolbar>Page 3</my-toolbar>
        Page 3
        <v-ons-button @click="replace">Replace with first page</v-ons-button>
        <v-ons-button @click="reset">Reset to first page</v-ons-button>
      </v-ons-page>
    `,
    methods: {
      log,
      replace() {
        this.$emit('pop');
        this.$emit('push', page1);
      },
      reset() {
        this.$emit('reset')
      }
    },
    components: { myToolbar },
  };

  const page2 = {
    name: 'page2',
    template: `
      <v-ons-page p2 @init="log('init!')">
        <my-toolbar>Page 2</my-toolbar>
        Page 2
        <v-ons-button @click="push">Push 3 pages</v-ons-button>
      </v-ons-page>
    `,
    methods: {
      log,
      push() {
        this.$emit('push', page3);
        this.$emit('push', page3);
        this.$emit('push', page3);
      }
    },
    components: { myToolbar },
    mounted() {
    }
  };

  const page1 = {
    name: 'page1',
    template: `
      <v-ons-page p1>
        <my-toolbar>Page 1</my-toolbar>
        Page 1
        <v-ons-button @click="push">Push</v-ons-button>
      </v-ons-page>
    `,
    methods: {
      log,
      push() {
        this.$emit('push', page2);
      }
    },
    components: { myToolbar },
    mounted() {
    }
  };

	export default {
    data() {
      return {
        pageStack: [page1]
      };
    },
    methods: {
      log
    },
    components: {
      page1,
      page2,
      page3
    }
	};
</script>

<style>
</style>
