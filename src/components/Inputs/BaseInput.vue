<template>
  <div class="form-group">
    <slot name="label">
      <label v-if="label" :class="labelClasses">
        {{label}}
      </label>
    </slot>
    <div :class="[
       {'input-group': hasIcon},
       {'focused': focused},
       {'input-group-alternative': alternative},
       {'has-label': label || $slots.label},
       inputGroupClasses
       ]">
      <div v-if="prependIcon || $slots.prepend" class="input-group-prepend">
        <span class="input-group-text">
          <slot name="prepend">
            <i :class="prependIcon"></i>
          </slot>
        </span>
      </div>
      <slot v-bind="slotData">
        <input
          :value="value"
          :type="type"
          :min="min"
          v-on="listeners"
          v-bind="$attrs"
          :valid="!error"
          :required="required"
          class="form-control"
          :class="[{'is-valid': valid === true}, {'is-invalid': error}, inputClasses]">
      </slot>
      <div v-if="appendIcon || $slots.append"
           @click="appendIconClickEventHandler ? appendIconClickEventHandler() : () => {}"
           class="input-group-append">
          <span class="input-group-text">
              <slot name="append">
                  <i :class="appendIcon"></i>
              </slot>
          </span>
      </div>
      <slot name="infoBlock"></slot>
      <slot name="error">
        <div v-if="error" class="invalid-feedback" style="display: block;">
          {{ error }}
        </div>
      </slot>
      <slot name="success">
        <div class="valid-feedback" v-if="!error && valid">
          {{successMessage}}
        </div>
      </slot>
    </div>
  </div>
</template>
<script>
  export default {
    inheritAttrs: false,
    name: "base-input",
    props: {
      required: {
        type: Boolean,
        description: "Whether input is required (adds an asterix *)"
      },
      group: {
        type: Boolean,
        description: "Whether input is an input group (manual override in special cases)"
      },
      valid: {
        type: Boolean,
        description: "Whether is valid",
        default: undefined
      },
      alternative: {
        type: Boolean,
        description: "Whether input is of alternative layout"
      },
      label: {
        type: String,
        description: "Input label (text before input)"
      },
      error: {
        type: String,
        description: "Input error (below input)"
      },
      successMessage: {
        type: String,
        description: "Input success message",
        default: 'Looks good!'
      },
      labelClasses: {
        type: String,
        description: "Input label css classes",
        default: "form-control-label"
      },
      inputClasses: {
        type: String,
        description: "Input css classes"
      },
      inputGroupClasses: {
        type: String,
        description: "Input group css classes"
      },
      value: {
        type: [String, Number],
        description: "Input value"
      },
      min: {
        type: String,
        description: "Min value (Hypefury uses it for datetime-local)"
      },
      type: {
        type: String,
        description: "Input type",
        default: "text"
      },
      appendIcon: {
        type: String,
        description: "Append icon (right)"
      },
      appendIconClickEventHandler: Function,
      prependIcon: {
        type: String,
        description: "Prepend icon (left)"
      }
    },
    data() {
      return {
        focused: false
      };
    },
    computed: {
      listeners() {
        return {
          ...this.$listeners,
          input: this.updateValue,
          focus: this.onFocus,
          blur: this.onBlur
        };
      },
      slotData() {
        return {
          focused: this.focused,
          error: this.error,
          ...this.listeners
        };
      },
      hasIcon() {
        const { append, prepend } = this.$slots;
        return (
          append !== undefined ||
          prepend !== undefined ||
          this.appendIcon !== undefined ||
          this.prependIcon !== undefined ||
          this.group
        );
      }
    },
    methods: {
      updateValue(evt) {
        let value = evt.target.value;
        this.$emit("input", value);
      },
      onFocus(evt) {
        this.focused = true;
        this.$emit("focus", evt);
      },
      onBlur(evt) {
        this.focused = false;
        this.$emit("blur", evt);
      }
    }
  };
</script>
<style>
</style>
