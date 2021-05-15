<template>
    <section>
        <h3 class="title">{{ document.title }}</h3>

        <article class="sheet">
            <small>
                <i>YOU GUYS BETTER GIVE ME CREDIT ON THE TEST</i> ❤️
                <br />
                <b>GNU LGPL v3</b>
            </small>

            <nuxt-content :document="document" />
        </article>
    </section>
</template>

<script lang="ts">
import { IContentDocument } from "@nuxt/content/types/content";
import Vue from "vue";

export default Vue.extend({
    data() {
        return {
            document: null as IContentDocument | null,
        };
    },

    async fetch() {
        const content = await this.$content(
            `notes/${this.$route.params.slug}`
        ).fetch();

        this.document = Array.isArray(content) ? content[0] : content;
    },
});
</script>

<style scoped>
section {
    padding: 2rem;
}

.sheet {
    max-width: 800px;
    background: rgb(54, 63, 75);
    border-radius: 10px;
    box-shadow: rgb(60 64 67 / 15%) 0px 3px 3px 1px !important;
    margin: 1rem auto;
    padding: 2rem;
}
</style>
