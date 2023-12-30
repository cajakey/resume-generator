<template>
    <div>
        <a-row :gutter="[0, 20]">
            <a-col :span="24" align="right">
                <a-button icon="plus" @click="add" />
            </a-col>
            <a-col :span="24" v-for="(item, index) in form.education" :key="index">
                <a-card>
                    <a-form-model>
                        <a-form-model-item label="Institution">
                            <a-input v-model="item.institution" />
                        </a-form-model-item>
                        <a-form-model-item label="Course or qualification">
                            <a-input v-model="item.course" />
                        </a-form-model-item>
                        <a-form-model-item label="Finished">
                            <a-month-picker style="width: 100%;" v-model="item.finished" format="MMM YYYY" />
                        </a-form-model-item>
                        <a-form-model-item label="Course highlights">
                            <a-textarea v-model="item.courseHighlights" />
                        </a-form-model-item>
                    </a-form-model>
                    <a-button type="primary" block @click="remove(index)">Delete</a-button>
                </a-card>
            </a-col>
            <a-col :span="24" v-if="form.education.length == 0">
                <a-empty />
            </a-col>
        </a-row>
    </div>
</template>

<script>
    export default {
        props: ["form"],
        methods: {
            add() {
                this.form.education.push({});
            },
            remove(index) {
                this.form.education.splice(index, 1);
            }
        }
    }
</script>