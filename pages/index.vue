<template>
    <section>
        <h1 class="title">Raghav's Epic Notes</h1>
        <div class="sheet">
            <div
                v-for="(subjectNotes, subjectName, i) in notesBySubject"
                :key="i"
            >
                <h1 class="subject-name">{{ subjectName }}</h1>
                <nuxt-link
                    v-for="(note, i2) in subjectNotes"
                    :key="i2"
                    class="notes-entry"
                    :to="`/view/${note.slug}`"
                >
                    {{ note.title }}
                </nuxt-link>
            </div>
        </div>
    </section>
</template>

<script lang="ts">
import Vue from "vue";
import { IContentDocument } from "@nuxt/content/types/content";
export default Vue.extend({
    data() {
        return { notes: [] as IContentDocument[] };
    },

    async fetch() {
        const content = await this.$content("notes")
            .only(["slug", "title", "subject"])
            .fetch();

        this.notes = content as IContentDocument[];
    },

    computed: {
        notesBySubject() {
            const notesBySubjectDict: Record<string, IContentDocument[]> = {};

            for (const note of this.notes) {
                if (!notesBySubjectDict[note.subject]) {
                    notesBySubjectDict[note.subject] = [];
                }

                notesBySubjectDict[note.subject].push(note);
            }

            return notesBySubjectDict;
        },
    },
});
</script>

<style scoped>
.subject-name {
    font-size: 2rem;
}

.notes-entry {
    font-size: 1.5rem;
}
</style>
