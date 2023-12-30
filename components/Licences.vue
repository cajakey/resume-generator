<template>
    <div>
        <a-row :gutter="[0, 20]">
            <a-col :span="24" align="right">
                <a-button icon="plus" @click="add" />
            </a-col>
            <a-col :span="24" v-for="(item, index) in form.licenses" :key="index">
                <a-card>
                    <a-form-model>
                        <a-form-model-item label="License name">
                            <a-input v-model="item.licenseName" />
                        </a-form-model-item>
                        <a-form-model-item label="Issuing organization">
                            <a-input v-model="item.issuingOrganization" />
                        </a-form-model-item>
                        <a-row :gutter="[20, 0]">
                            <a-col :span="12">
                                <a-form-model-item label="Issue date">
                                    <a-month-picker style="width: 100%;" v-model="item.issueDate" format="MMM YYYY" />
                                </a-form-model-item>
                            </a-col>
                            <a-col :span="12">
                                <a-form-model-item label="Expiry date">
                                    <a-month-picker style="width: 100%;" v-model="item.expiryDate" format="MMM YYYY" />
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
            <a-col :span="24" v-if="form.licenses.length == 0">
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
                this.form.licenses.push({});
            },
            remove(index) {
                this.form.licenses.splice(index, 1);
            }
        }
    }
</script>