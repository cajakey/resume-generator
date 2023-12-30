<template>
    <div>
        <div v-show="false">
            <div ref="content">
                <a-row :gutter="[10, 20]">
                    <a-col :span="24">
                        <a-descriptions :title="form.name" :column="2" bordered>
                            <a-descriptions-item label="Phone number">
                                +63{{ form.phoneNumber }}
                            </a-descriptions-item>
                            <a-descriptions-item label="Email">
                                {{ form.email }}
                            </a-descriptions-item>
                            <a-descriptions-item label="Address">
                                {{ form.address }}
                            </a-descriptions-item>
                        </a-descriptions>
                    </a-col>
                    <a-col :span="12">
                        <a-card>
                            <div style="font-size: 14px; font-weight: 400;" slot="title">Career history</div>
                            <a-card-grid style="width: 100%;" v-for="(item, i) in form.careerHistory" :key="i" :hoverable="false">
                                <div>{{ item.jobTitle }}</div>
                                <div>{{ item.companyName }}</div>
                                <div>{{ formatDate(item.started) }} - {{ formatDate(item.ended) || "Present" }}</div>
                                <div>{{ item.description }}</div>
                            </a-card-grid>
                        </a-card>
                    </a-col>
                    <a-col :span="12">
                        <a-card>
                            <div style="font-size: 14px; font-weight: 400;" slot="title">Education</div>
                            <a-card-grid style="width: 100%;" v-for="(item, i) in form.education" :key="i" :hoverable="false">
                                <div>{{ item.institution }}</div>
                                <div>{{ item.course }}</div>
                                <div>{{ formatDate(item.finished) || "Currently enrolled" }}</div>
                                <div>{{ item.courseHighlights }}</div>
                            </a-card-grid>
                        </a-card>
                    </a-col>
                    <a-col :span="24">
                        <a-descriptions :column="1" bordered>
                            <a-descriptions-item label="Skills">
                                {{ joinByComma("skills") }}
                            </a-descriptions-item>
                            <a-descriptions-item label="Languages">
                                {{ joinByComma("languages") }}
                            </a-descriptions-item>
                            <a-descriptions-item label="Personal summary">
                                {{ form.personalSummary }}
                            </a-descriptions-item>
                        </a-descriptions>
                    </a-col>
                </a-row>
            </div>
        </div>
        <a-row :gutter="[0, 10]">
            <a-col :span="15">
                <VueFileToolbarMenu :content="menu" />
            </a-col>
            <a-col :span="9" align="right">
                <a-select style="width: 100px;" :default-value="['Letter', 216, 279]" @change="onFormatChange">
                    <a-select-option v-for="format in formats" :key="format">
                        {{ format[0] }}
                    </a-select-option>
                </a-select>
            </a-col>
        </a-row>
        <VueDocumentEditor :content.sync="content" :page_format_mm="page_format_mm" ref="editor" />
    </div>
</template>

<script>
    export default {
        props: ["form"],
        methods: {
            joinByComma(key) {
                if (this.form[key] && this.form[key].length) {
                    return this.form[key].join(", ");
                }
                return "";
            },
            formatDate(date) {
                if (date) {
                    return this.$moment(date).format("MMM YYYY");
                }
                return "";
            },
            onFormatChange(format) {
                this.page_format_mm = [format[1], format[2]];
            }
        },
        data() {
            return {
                content: [],
                page_format_mm: [210, 297],
                mounted: false
            };
        },
        mounted() {
            this.mounted = true;
            this.content = [this.$refs.content.outerHTML];
        },
        computed: {
            menu() {
                return [
                    { text: "Print", title: "Print", icon: "print", click: () => window.print() },
                    { is: "spacer" },
                    { icon: "format_align_left", title: "Align left", active: this.isLeftAligned, disabled: !this.current_text_style, hotkey: this.isMacLike ? "shift+command+l" : "ctrl+shift+l", click: () => document.execCommand("justifyLeft") },
                    { icon: "format_align_center", title: "Align center", active: this.isCentered, disabled: !this.current_text_style, hotkey: this.isMacLike ? "shift+command+e" : "ctrl+shift+e", click: () => document.execCommand("justifyCenter") },
                    { icon: "format_align_right", title: "Align right", active: this.isRightAligned, disabled: !this.current_text_style, hotkey: this.isMacLike ? "shift+command+r" : "ctrl+shift+r", click: () => document.execCommand("justifyRight") },
                    { icon: "format_align_justify", title: "Justify content", active: this.isJustified, disabled: !this.current_text_style, hotkey: this.isMacLike ? "shift+command+j" : "ctrl+shift+j", click: () => document.execCommand("justifyFull") },
                    { is: "separator" },
                    { icon: "format_bold", title: "Bold", active: this.isBold, disabled: !this.current_text_style, hotkey: this.isMacLike ? "command+b" : "ctrl+b", click: () => document.execCommand("bold") },
                    { icon: "format_italic", title: "Italic", active: this.isItalic, disabled: !this.current_text_style, hotkey: this.isMacLike ? "command+i" : "ctrl+i", click: () => document.execCommand("italic") },
                    { icon: "format_underline", title: "Underline", active: this.isUnderline, disabled: !this.current_text_style, hotkey: this.isMacLike ? "command+u" : "ctrl+u", click: () => document.execCommand("underline") },
                    { icon: "format_strikethrough", title: "Strike through", active: this.isStrikeThrough, disabled: !this.current_text_style, click: () => document.execCommand("strikethrough") }
                ];
            },
            formats: () => [
                ["Letter", 216, 279],
                ["Legal", 216, 356],
                ["A3", 297, 420],
                ["A4", 210, 297]
            ],
            current_text_style () { return this.mounted ? this.$refs.editor.current_text_style : false; },
            isLeftAligned () { return ["start", "left", "-moz-left"].includes(this.current_text_style.textAlign); },
            isRightAligned () { return ["end", "right", "-moz-right"].includes(this.current_text_style.textAlign); },
            isCentered () { return ["center", "-moz-center"].includes(this.current_text_style.textAlign); },
            isJustified () { return ["justify", "justify-all"].includes(this.current_text_style.textAlign); }
        }
    }
</script>