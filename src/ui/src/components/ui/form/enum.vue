<template>
    <bk-select class="form-enum-selector"
        v-model="selected"
        :clearable="allowClear"
        :searchable="searchable"
        :disabled="disabled"
        :multiple="multiple"
        :placeholder="placeholder"
        :font-size="fontSize"
        :popover-options="{
            boundary: 'window'
        }"
        ref="selector">
        <bk-option
            v-for="(option, index) in options"
            :key="index"
            :id="option.id"
            :name="option.name">
        </bk-option>
    </bk-select>
</template>

<script>
    export default {
        name: 'cmdb-form-enum',
        props: {
            value: {
                type: [Array, String, Number],
                default: ''
            },
            disabled: {
                type: Boolean,
                default: false
            },
            multiple: {
                type: Boolean,
                default: false
            },
            allowClear: {
                type: Boolean,
                default: false
            },
            autoSelect: {
                type: Boolean,
                default: true
            },
            options: {
                type: Array,
                default () {
                    return []
                }
            },
            placeholder: {
                type: String,
                default: ''
            },
            fontSize: {
                type: [String, Number],
                default: 'medium'
            }
        },
        data () {
            return {
                selected: this.multiple ? [] : ''
            }
        },
        computed: {
            searchable () {
                return this.options.length > 7
            }
        },
        watch: {
            value (value) {
                if (value !== null) {
                    this.selected = value
                }
            },
            selected (selected) {
                this.$emit('input', selected)
                this.$emit('on-selected', selected)
            },
            disabled (disabled) {
                this.setInitData()
            }
        },
        created () {
            this.setInitData()
        },
        methods: {
            setInitData () {
                if (this.autoSelect) {
                    if (this.value === '') {
                        const defaultOption = this.options.find(option => option['is_default'])
                        if (defaultOption) {
                            this.selected = this.multiple ? [defaultOption.id] : defaultOption.id
                        } else {
                            this.$emit('input', null)
                        }
                    } else {
                        this.selected = this.value
                    }
                } else {
                    this.selected = this.value
                }
            },
            focus () {
                this.$refs.selector.show()
            }
        }
    }
</script>

<style lang="scss" scoped>
    .form-enum-selector{
        width: 100%;
    }
</style>
