<template>
    <div>
        <a-form-model>
            <a-form-model-item label="Skills">
                <a-select style="width: 100%;" v-model="form.skills" mode="tags" />
            </a-form-model-item>
        </a-form-model>
    </div>
</template>

<script>
    export default {
        props: ["form"]
    }
</script>