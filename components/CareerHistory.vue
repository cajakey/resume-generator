<template>
    <div>
        <a-row :gutter="[0, 20]">
            <a-col :span="24" align="right">
                <a-button icon="plus" @click="add" />
            </a-col>
            <a-col :span="24" v-for="(item, index) in form.careerHistory" :key="index">
                <a-card>
                    <a-form-model>
                        <a-form-model-item label="Job title">
                            <a-input v-model="item.jobTitle" />
                        </a-form-model-item>
                        <a-form-model-item label="Company name">
                            <a-input v-model="item.companyName" />
                        </a-form-model-item>
                        <a-row :gutter="[20, 0]">
                            <a-col :span="12">
                                <a-form-model-item label="Started">
                                    <a-month-picker style="width: 100%;" v-model="item.started" format="MMM YYYY" />
                                </a-form-model-item>
                            </a-col>
                            <a-col :span="12">
                                <a-form-model-item label="Ended">
                                    <a-month-picker style="width: 100%;" v-model="item.ended" format="MMM YYYY" />
                                </a-form-model-item>
                            </a-col>
                        </a-row>
                        <a-form-model-item label="Description">
                            <a-textarea v-model="item.description" />
                        </a-form-model-item>
                    </a-form-model>
                    <a-button type="primary" block @click="remove(index)">Delete</a-button>
                </a-card>
            </a-col>
            <a-col :span="24" v-if="form.careerHistory.length == 0">
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
                this.form.careerHistory.push({});
            },
            remove(index) {
                this.form.careerHistory.splice(index, 1);
            }
        }
    }
</script>