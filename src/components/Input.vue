<template>
  <div class="form-group">
    <label>{{ name }}</label>
    <span class="fa" v-if="activated" :class="validClass"></span>
    <input type="text" class="form-control" @input="onInput">
  </div>
</template>

<script>
    export default {
        props: {
            name: {
                type: String,
                required: true
            },
            value: {
                type: null,
                required: true
            },
            pattern: {
                type: RegExp
            }
        },
        data() {
            return {
                activated: this.value != ''
            }
        },
        methods: {
            onInput(e) {
                this.activated = true;

                this.$emit('onchange', {
                    value: e.target.value,
                    valid: this.pattern.test(e.target.value)
                });
            }
        },
        computed: {
            validClass() {
                return this.pattern.test(this.value)
                    ? "fa-check-circle text-success"
                    : "fa-exclamation-circle text-danger";
            },
        }
    }
</script>

<style scoped>

</style>
