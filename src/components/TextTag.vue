<script>
export default {
    name: 'TextTag',

    props: {
        icon: {
            type: String,
            required: false
        },
        text: {
            type: String,
            required: true
        },
        fontSize: {
            type: String,
            default: 'initial'
        }
    },

    computed: {
        tagStyle() {
            return {
                fontSize: this.fontSize
            }
        }
    }
}
</script>

<template>
    <div class="text-tag" :style="tagStyle">
        <div class="text-tag__icon" v-if="icon">
            <v-icon size="1.5em">{{ icon }}</v-icon>
        </div>
        <div class="text-tag__text">{{ text }}</div>
    </div>
</template>

<style lang="scss">
.text-tag {
    display: flex;
    font-size: initial;
    overflow: hidden;

    &__icon {
        width: 1.5em;
        font-size: inherit;
        margin-top: -0.125em;
        margin-right: 0.25em;
        overflow: hidden;
    }

    &__text {
        font-size: inherit;
        white-space: nowrap;
    }
}
</style>