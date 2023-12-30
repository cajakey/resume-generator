<template>
    <div>
        <a-form-model>
            <a-form-model-item label="Name">
                <a-input v-model="form.name" />
            </a-form-model-item>
            <a-form-model-item label="Phone number">
                <a-input-group style="display: flex;" compact>
                    <a-select default-value="PH">
                        <a-select-option value="PH">
                            +63
                        </a-select-option>
                    </a-select>
                    <a-input v-model="form.phoneNumber" />
                </a-input-group>
            </a-form-model-item>
            <a-form-model-item label="Lives in">
                <a-textarea v-model="form.address" />
            </a-form-model-item>
            <a-form-model-item label="Email">
                <a-input v-model="form.email" />
            </a-form-model-item>
        </a-form-model>
    </div>
</template>

<script>
    export default {
        props: ["form"]
    }
</script>