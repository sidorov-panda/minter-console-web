<script>
    import checkEmpty from '~/assets/v-check-empty';
    import QrScan from '~/components/common/QrScan';
    import InputMaskedInteger from '~/components/common/InputMaskedInteger';

    export default {
        inheritAttrs: false,
        components: {
            QrScan,
            InputMaskedInteger,
        },
        directives: {
            checkEmpty,
        },
        props: {
            value: {
                type: [String, Number],
                required: true,
            },
            $value: {
                type: Object,
                require: true,
            },
            label: {
                type: String,
                required: true,
            },
            isInteger: {
                type: Boolean,
                default: false,
            },
        },
        data() {
            return {
                hasCamera: false,
            };
        },
        methods: {
            handleQrScanned(result) {
                this.$emit('input', result);
                this.$value.$touch();
            },
        },
    };
</script>

<template>
    <label class="form-field" :class="{'is-error': $value.$error, 'form-field--with-icon': hasCamera}">
        <InputMaskedInteger
            class="form-field__input" v-check-empty
            v-bind="$attrs"
            :value="value"
            @input="$emit('input', $event)"
            @blur="$value.$touch()"
            v-if="isInteger"
        />
        <input class="form-field__input" type="text" autocapitalize="off" spellcheck="false" v-check-empty
               v-bind="$attrs"
               :value="value"
               @input="$emit('input', $event.target.value)"
               @blur="$value.$touch()"
               v-else
        >
        <QrScan @qrScanned="handleQrScanned" :qrVisible.sync="hasCamera"/>
        <span class="form-field__label">{{ label }}</span>
    </label>
</template>

