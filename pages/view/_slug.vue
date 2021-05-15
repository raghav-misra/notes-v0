<template>
    <section>
        <h1 class="title">{{ title }}</h1>

        <details class="toc">
            <summary><h2>Contents:</h2></summary>

            <div class="headings">
                <a
                    v-for="(section, i) in toc"
                    :key="i"
                    :href="`#${section.id}`"
                >
                    {{ section.textContent }}
                </a>
            </div>
        </details>

        <article ref="document" class="sheet">
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
            toc: [] as HTMLHeadingElement[],
        };
    },

    async fetch() {
        const content = await this.$content(
            `notes/${this.$route.params.slug}`
        ).fetch();

        this.document = Array.isArray(content) ? content[0] : content;
    },

    computed: {
        title(): string {
            return this.document ? this.document.title : "";
        },
    },

    watch: {
        title() {
            document.title = this.title === "" ? "Notes" : this.title;
        },
        document() {
            const article = this.$refs.document as HTMLElement;
            this.toc = Array.from(article.querySelectorAll("h1"));
        },
    },
});
</script>

<style scoped>
section {
    padding: 2rem;
}

section > * {
    max-width: 850px;
    margin: 0 auto;
}

.sheet {
    background: rgb(54, 63, 75);
    border-radius: 0 0 10px 10px;
    box-shadow: rgb(60 64 67 / 15%) 0px 3px 3px 1px !important;
    padding: 2rem;
}

.toc {
    background: white;
    padding: 1rem 2rem;
}

.toc * {
    color: var(--back) !important;
}

.toc summary {
    display: flex;
    cursor: pointer;
}

.toc summary h2::before {
    content: "+";
    margin-right: 0.5rem;
}

.toc[open] summary h2::before {
    content: "-";
}

.toc .headings {
    display: flex;
    margin-top: 0.5rem;
    flex-wrap: wrap;
}

.toc a {
    display: inline-block;
    width: 33%;
    text-decoration: none;
    color: cornflowerblue !important;
}
</style>
