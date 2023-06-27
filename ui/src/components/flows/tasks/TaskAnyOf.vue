<template>
    <el-input
        :model-value="JSON.stringify(values)"
        :disabled="true"
    >
        <template #append>
            <el-button :icon="Eye" @click="isOpen = true" />
        </template>
    </el-input>


    <el-drawer
        v-if="isOpen"
        v-model="isOpen"
        destroy-on-close
        size=""
        :append-to-body="true"
    >
        <template #header>
            <code>{{ root }}</code>
        </template>
        <el-select
            :model-value="selectedSchema"
            @update:model-value="onSelect"
        >
            <el-option
                v-for="schema in schemas"
                :key="schema.$ref"
                :label="schema.$ref.split('.').pop()"
                :value="schema.$ref"
            />
        </el-select>
        <el-form label-position="top" v-if="selectedSchema">
            <task-object
                v-if="currentSchema"
                :model-value="modelValue"
                @update:model-value="onInput"
                :schema="currentSchema"
                :definitions="definitions"
            />
        </el-form>
        <template #footer>
            <el-button :icon="ContentSave" @click="isOpen = false" type="primary">
                {{ $t('save') }}
            </el-button>
        </template>
    </el-drawer>
</template>

<script setup>
    import Eye from "vue-material-design-icons/Eye.vue";
    import ContentSave from "vue-material-design-icons/ContentSave.vue";
</script>

<script>
    import Task from "./Task"
    export default {
        mixins: [Task],
        data() {
            return {
                isOpen: false,
                schemas: [],
                selectedSchema: undefined
            };
        },
        created() {
            this.schemas = this.schema?.anyOf ?? []
        },
        methods: {
            onSelect(value) {
                this.selectedSchema = value.split("/").pop();
            },
        },
        computed: {
            currentSchema() {
                return this.definitions[this.selectedSchema]
            }
        },
    };
</script>
