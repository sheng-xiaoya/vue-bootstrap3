<template>
    <li :class="[prefixCls + '-wrap']" v-show="!hidden">
        <div :class="[prefixCls + '-title']">{{ label }}</div>
        <ul :class="[prefixCls+'-option-wrap']">
            <li :class="[prefixCls]" ref="options"><slot></slot></li>
        </ul>
    </li>
</template>
<script>
    const prefixCls = 'i-select-group';

    export default {
        name: 'OptionGroup',
        props: {
            label: {
                type: String,
                default: ''
            }
        },
        data () {
            return {
                prefixCls: prefixCls,
                hidden: false    // for search
            };
        },
        methods: {
            queryChange () {
                this.$nextTick(() => {
                    const options = this.$refs.options.querySelectorAll('.select-item');
                    let hasVisibleOption = false;
                    for (let i = 0; i < options.length; i++) {
                        if (options[i].style.display !== 'none') {
                            hasVisibleOption = true;
                            break;
                        }
                    }
                    this.hidden = !hasVisibleOption;
                });
            }
        },
        mounted () {
            this.$on('on-query-change', () => {
                this.queryChange();
                return true;
            });
        }
    };
</script>
