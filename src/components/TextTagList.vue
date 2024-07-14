<script>
import TextTag from "@/components/TextTag.vue";

export default {
    name: 'TextTags',
    components: {
        TextTag
    },

    props: {
        tags: {
            type: Array,
            required: true
        },
        alignment: {
            type: String,
            default: 'left',
            validator: value => ['left', 'justify'].includes(value)
        },
        fontSize: {
            type: String,
            default: 'initial'
        }
    },

    data() {
        return {
            isEmpty: false,
            visibleTags: this.tags
        };
    },

    mounted() {
        this.updateVisibleTags()
        window.addEventListener('resize', this.updateVisibleTags)
    },

    beforeDestroy() {
        window.removeEventListener('resize', this.updateVisibleTags)
    },

    computed: {
        stylesHidden() {
            const styles = {
                height: 0,
                minHeight: 0,
                borderTop: 0,
                borderBottom: 0,
                marginTop: 0,
                marginBottom: 0,
                paddingTop: 0,
                paddingBottom: 0,
            }
            return this.isEmpty ? styles : {}
        }
    },

    methods: {
        updateVisibleTags() {
            let widthTagsElements = 0
            let widthTagsElementsVisible = 0
            const containerWidth = this.$refs.container?.getBoundingClientRect().width ?? 0
            const separatorWidth = this.$refs.separators?.[0].getBoundingClientRect().width ?? 0

            this.visibleTags = this.tags.map((tag, i) => {
                let visible = false
                const widthTagElement = this.$refs.tags?.[i].getBoundingClientRect().width ?? 0
                const widthElements = widthTagElement + (i ? separatorWidth : 0)
                widthTagsElements += widthElements
                if (widthTagsElements < containerWidth) {
                    visible = true
                    widthTagsElementsVisible += widthElements
                }
                return { ...tag, visible }
            })

            this.isEmpty = !widthTagsElementsVisible
        }
    },

    watch: {
        tags() {
            this.visibleTags = this.tags
            this.$nextTick(this.updateVisibleTags)
        }
    }
}
</script>

<template>
    <div :style="stylesHidden">
        <div
            class="text-tags"
            :class="{ 'text-tags--justify': alignment === 'justify' }"
            ref="container"
        >
            <template v-for="(tag, index) in visibleTags">
                <div
                    class="text-tags__item text-tags__separator"
                    :class="{ 'text-tags__item--hidden': !(tag.visible ?? true) }"
                    :style="{ fontSize }"
                    :key="`separator-${ tag.text }`"
                    v-if="index"
                    ref="separators"
                >
                    <v-icon size="1.75em">
                        mdi-circle-small
                    </v-icon>
                </div>
                <div
                    class="text-tags__item"
                    :class="{ 'text-tags__item--hidden': !(tag.visible ?? true) }"
                    :key="`tag-${ tag.text }`"
                    ref="tags"
                >
                    <text-tag
                        :text="tag.text"
                        :icon="tag.icon"
                        :font-size="fontSize"
                    />
                </div>
            </template>
        </div>
    </div>
</template>

<style lang="scss">
.text-tags {
    display: flex;
    align-items: center;
    width: 100%;
    overflow: hidden;

    &--justify {
        justify-content: space-between;
    }

    &__item {
        &--hidden {
            position: absolute;
            opacity: 0;
            z-index: -999999;
        }
    }

    &__separator {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 2.2em;
        min-width: 2.2em;
        height: 1em;
        font-size: initial;
        margin-top: 0.1em;
        overflow: hidden;
    }
}
</style>