<template>
    <div class="validate-input-container pb-3">
        <input v-bind="$attrs"
               class="form-control"
               :value="inputRef.val"
               @input="updateValue"
               @blur="validateInput" :class="{'is-invalid': inputRef.error}">
        <span v-if="inputRef.error" class="invalid-feedback">{{inputRef.message}}</span>
    </div>

</template>

<script lang="ts">
    import {defineComponent, PropType, reactive} from 'vue';
    const emailReg = /^([a-zA-Z0-9._%-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4})*$/;
    interface RuleProp {
        type: 'required' | 'email' | 'minLength';
        message: string;
    }
    export type RulesProp = RuleProp[];
    export default defineComponent({
        props: {
            rules: Array as PropType<RulesProp>,
            minLength: Number,
            modelValue: String
        },
        setup(props, context) {
            const inputRef = reactive({
                val: props.modelValue || '',
                error: false,
                message: ''
            });
            const updateValue = (e: KeyboardEvent) => {
                const targetValue = (e.target as HTMLInputElement).value;
                inputRef.val = targetValue;
                context.emit('update:modelValue', targetValue)
            }
            const validateInput = () => {
                if (props.rules) {
                    const allPassed = props.rules.every(rule => {
                        let passed = true;
                        let minlength = 0;
                        inputRef.message = rule.message;
                        if (props.minLength) {
                            minlength = props.minLength;
                        }
                        switch (rule.type) {
                            case 'required':
                                passed = (inputRef.val.trim() !== '');
                                break;
                            case 'email':
                                passed = (emailReg.test(inputRef.val));
                                break;
                            case 'minLength':
                                passed = (inputRef.val.trim().length > minlength);
                                inputRef.message = `输入内容长度不得少于${minlength}`;
                                break;
                            default:
                                break;
                        }
                        return passed
                    })
                    inputRef.error = !allPassed;
                }
            }
            return {
                inputRef,
                validateInput,
                updateValue
            }
        }
    })
</script>

<style scoped>
.validate-input-container {
    text-align: left;
}
</style>
