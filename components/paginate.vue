<template>
    <div class="cb--paginate">
        <slot name="first" :css="firstCss" :select="select" />
        <div
            :class="['cb--paginate-prev', firstCss]"
            v-if="skip"
            @click="select(-skip + modelValue)"
        ></div>
        <div
            v-for="num in mapping"
            :key="num"
            :class="active(num)"
            @click="select(num)"
        >
            {{ num }}
        </div>
        <div
            :class="['cb--paginate-next', lastCss]"
            v-if="skip"
            @click="select(skip + modelValue)"
        ></div>
        <slot name="last" :css="lastCss" :select="select" />
    </div>
</template>

<script>
import { reactive, computed, watch } from "vue";

export default {
    props: {
        map: {
            type: Number,
            default: 5
        },
        modelValue: Number,
        skip: {
            type: [Number],
            default: 1
        },
        total: Number
    },
    setup(props, { emit }) {
        const state = reactive({
            mapping: []
        });

        const start = computed(() => state.mapping[0]);

        const end = computed(() => state.mapping[state.mapping.length - 1]);

        const setMap = (page, forse) => {
            if (page <= start.value) {
                page = page - 1;
            } else if (page >= end.value) {
                page = page + 2 - props.map;
            } else {
                if (!forse) return;
            }

            state.mapping = [];
            if (page < 1) {
                page = 1;
            } else if (page - 1 + props.map > props.total) {
                page = props.total - props.map + 1;
            }
            for (let num = page; num <= page + props.map - 1; num++) {
                if (num < 1) continue;
                if (num > props.total) break;
                state.mapping.push(num);
            }
        };
        setMap(1, true);

        const mapping = computed(() => state.mapping);

        watch(
            () => props.total,
            total => {
                if (props.modelValue > total) {
                    emit("update:modelValue", total);
                } else {
                    setMap(props.modelValue);
                }
            }
        );

        watch(
            () => props.modelValue,
            page => {
                setMap(page);
            }
        );

        const firstCss = computed(() =>
            props.modelValue <= 1 ? "disabled" : ""
        );

        const active = num => (num == props.modelValue ? "active" : "");

        const lastCss = computed(() =>
            props.modelValue >= props.total ? "disabled" : ""
        );

        const select = page => {
            if (page < 1) {
                page = 1;
            } else if (page > props.total) {
                page = props.total;
            }
            emit("update:modelValue", page);
        };

        return { mapping, firstCss, active, lastCss, select };
    }
};
</script>
