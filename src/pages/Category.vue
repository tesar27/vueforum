<template>
  <h1>{{ category.name }}</h1>
  <ForumList :title="category.name" :forums="getForumsForCategory(category)" />
</template>
<script>
import ForumList from "@/components/ForumList";
import { findById } from "@/helpers";
import { mapActions } from "vuex";

export default {
  name: "Category",
  components: {
    ForumList,
  },
  props: {
    id: {
      type: String,
      required: true,
    },
  },
  computed: {
    category() {
      return findById(this.$store.state.categories, this.id) || {};
    },
  },
  methods: {
    ...mapActions(["fetchCategory", "fetchForums"]),
    getForumsForCategory(category) {
      return this.$store.state.forums.filter(
        (forum) => forum.categoryId === category.id
      );
    },
  },
  async created() {
    // if (this.category === undefined) {}
    const category = await this.fetchCategory({ id: this.id });
    this.fetchForums({ ids: category.forums });
  },
};
</script>