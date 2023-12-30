<template>
    <div>
        <a-form-model>
            <a-form-model-item label="Personal summary">
                <a-textarea style="width: 100%;" v-model="form.personalSummary" />
            </a-form-model-item>
        </a-form-model>
    </div>
</template>

<script>
    export default {
        props: ["form"]
    }
</script>