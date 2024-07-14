<script>
import TextTagList from './components/TextTagList.vue';

export default {
    name: 'App',
    components: {
        TextTagList
    },
    data() {
        return {
            tags: [
                { text: 'первый' },
                { text: 'второй', icon: 'mdi-home' },
                { text: 'третий' },
                { text: 'четвертый', icon: 'mdi-account' },
                { text: 'пятый' }
            ]
        }
    }
};
</script>

<template>
    <v-app>
        <div class="tags">
            <div class="tags__items">
                <text-tag-list
                    class="tags__item tags__item--column"
                    :tags="tags"
                />
                <text-tag-list
                    class="tags__item tags__item--column tags__item--max-w-50"
                    :tags="tags"
                />
                <text-tag-list
                    class="tags__item tags__item--column"
                    alignment="justify"
                    font-size="24px"
                    :tags="tags"
                />
                <text-tag-list
                    class="tags__item tags__item--column tags__item--max-w-50"
                    alignment="justify"
                    :tags="tags"
                />
            </div>
            <div class="tags__items tags__items--row">
                <text-tag-list
                    class="tags__item tags__item--row tags__item--max-w-50"
                    :tags="tags"
                />
                <text-tag-list
                    class="tags__item tags__item--row tags__item--max-w-50"
                    alignment="justify"
                    :tags="tags"
                />
            </div>
        </div>
    </v-app>
</template>

<style lang="scss">
.tags {
    $itemsGap: 40px;

    width: 100%;
    max-width: 1200px;
    padding: 40px;
    margin: 0 auto;

    &__items {
        & + & {
            margin-top: $itemsGap;
        }

        &--row {
            display: flex;
            width: 100%;
        }
    }

    &__item {
        width: 100%;
        border: 1px solid #333;
        padding: 10px 20px;
        overflow: hidden;

        &--row + &--row {
            margin-left: $itemsGap;
        }

        &--column + &--column {
            margin-top: $itemsGap;
        }

        &--max-w-50 {
            max-width: 50%;
        }
    }
}
</style>