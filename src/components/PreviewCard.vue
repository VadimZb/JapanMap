<template>
    <transition name="fade">
        <v-card
            :key="prefecture.name"
            elevation="6"
            :class="sm ? 'mx-auto' : 'desktopCard'"
            :max-width="sm ? 700 : 300"
        >
            <v-img
                v-if="!sm"
                :src="prefecture.preview.image.large"
                :lazy-src="prefecture.preview.image.small"
            >
                <template v-slot:placeholder>
                    <v-row
                        class="fill-height ma-0"
                        align="center"
                        justify="center"
                    >
                        <v-progress-circular
                            indeterminate
                            color="primary"
                        ></v-progress-circular>
                    </v-row>
                </template>
            </v-img>
            <v-card-text class="text-center">
                <p class="text-h4 text--primary mb-1">{{ prefecture.name }}</p>
                <p class="text-subtitle-1 mt-0 mb-1">
                    {{ prefecture.preview.center }}
                </p>
                <p>{{ prefecture.preview.intro }}</p>
                <div :class="sm && 'd-flex justify-space-around'">
                    <div v-for="(field, i) in filterFields(fields)" :key="i">
                        <p
                            v-if="field.value"
                            class="text-left d-flex align-center"
                            :class="
                                sm &&
                                    'd-flex flex-column align-center text-center'
                            "
                        >
                            <v-icon :small="!sm" left>{{ field.icon }}</v-icon>
                            <span v-if="!sm" class="font-weight-bold"
                                >{{ field.name }}:</span
                            >
                            <span
                                style="max-width: 150px;"
                                :style="sm && 'max-width: 100px;'"
                                class="d-inline-block text-truncate ml-0 ml-md-1"
                                >{{ field.value }}</span
                            >
                        </p>
                    </div>
                </div>
                <v-btn @click="open" color="primary" block v-if="sm">
                    <v-icon left>mdi-book-open-variant</v-icon>
                    Открыть
                </v-btn>
            </v-card-text>
        </v-card>
    </transition>
</template>

<script>
export default {
    props: {
        prefecture: {
            type: Object,
        },
        sm: {
            type: Boolean,
            required: true,
        },
    },
    methods: {
        open() {
            this.$emit("open");
        },
        filterFields(fields) {
            return fields.filter((field) => field.value.length);
        },
    },
    computed: {
        fields() {
            return [
                {
                    name: "Блюдо",
                    icon: "$food",
                    value: this.prefecture.preview.food,
                },
                {
                    name: "Культура",
                    icon: "$culture",
                    value: this.prefecture.list[0].title,
                },
                {
                    name: "Диалект",
                    icon: "$language",
                    value: this.prefecture.preview.dialect,
                },
            ];
        },
    },
};
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
    opacity: 0;
}

.desktopCard {
    position: absolute;
    right: 100px;
    top: 50%;
    transform: translateY(-50%);
}

@media screen and (max-width: 1500px) {
    .desktopCard {
        right: 10px;
    }
}
</style>
