<template lang='pug'>
  fieldset.form-group(:class='classes')
    legend.form-group-label(v-if='label' @click='toggleCollapsed')
      .collapse-toggle(v-if='collapsible' )
        icon(:name='`${collapsed ? "plus" : "minus"}-square-o`')
      .label-text
        icon.form-group-locked-icon(v-if='disabled' name='lock')
        icon.form-group-locked-icon(v-if='icon' :name='icon')
        | {{ label }}
    .form-group-content(@click='toggleCollapsedIfCollapsed')
      slot(v-if='!collapsible || (collapsible && !collapsed)')
      slot(v-if='collapsible && collapsed' name='summary')
</template>

<script>
import _ from 'lodash';

const components = {
};


export default {
  components,
  props: {
    label: String,
    collapsible: Boolean,
    startCollapsed: Boolean,
    disabled: Boolean,
    borderless: Boolean,
    icon: String,
  },
  data() {
    return {
      collapsed: this.startCollapsed,
    };
  },
  computed: {
    computedDisabled() {
      if (this.disabled) return true;
      return _.isFunction(this.formParentDisabled) && this.formParentDisabled();
    },
    classes() {
      return {
        collapsible: this.collapsible,
        collapsed: this.collapsible && this.collapsed,
        borderless: this.borderless,
      };
    },
  },
  provide() {
    return {
      formParentDisabled: () => this.computedDisabled,
    };
  },
  inject: { formParentDisabled: { default: false } },
  methods: {
    toggleCollapsed() {
      if (this.collapsible) this.collapsed = !this.collapsed;
    },
    toggleCollapsedIfCollapsed() {
      if (this.collapsible && this.collapsed) this.collapsed = !this.collapsed;
    },
  },
};
</script>

<style lang='less'>


.form-group {
  // TO FIX TEXT OVERFLOW ELLIPSIS ISSUE
  // https://stackoverflow.com/questions/7434756/overflow-and-text-overflow-within-fieldsets
  min-width: 0;

  display: block;
  position: relative;

  margin: 15px 0;
  padding: 0;
  // background: #F0F0F0;
  border: 1px solid #AAA;
  border-radius: 3px;
  // border: 3px solid #d8f8ff;
  padding: 0 5px 5px;

  > .form-group-content {
    > .form-row:first-of-type {
      border-top: none;
      margin-top: -2px;
    }
    + .form-row {
      border-top: none;
    }

    // Nested form groups look more like a section within the containing group
    > .form-group {
      margin: 0px 0px;
      border-radius: 0;
      border-width: 1px;
      // border-color: rgba(0,0,0,.2);
      border-left: none;
      border-right: none;
      border-bottom: none;

      .form-group-label {
        color: #777;
        cursor: default !important;
      }
      + .form-row {
        border-top-color: rgba(0,0,0,.3);
      }
    }
  }

  &.collapsed {
    > .form-group-content {
      cursor: pointer;
    }
  }

  &.borderless {
    border: none;
  }

  .form-group-locked-icon {
    margin-right: 4px;
    width: 13px;
    height: 13px;
    vertical-align: middle;
  }
}
.form-group-label {
  font-weight: bold;
  text-transform: uppercase;
  font-size: 11px;
  line-height: 1em;
  margin-left: 7px;
  padding: 0 5px;
  color: white;

  .collapse-toggle {
    display: inline-block;
    cursor: pointer;
    margin-right: 10px;
    height: 16px;
    // margin-top: 4px;
    // margin-bottom: -4px;
    vertical-align: middle;
  }

  .label-text {
    display: inline-block;
    white-space: nowrap;
    vertical-align: middle;
  }

  .form-group.collapsible & {
    cursor: pointer;
    &:hover {
      .collapse-toggle {
        transform: scale(1.1);
      }
    }
  }
}
.popup, .todo-section {
  .form-group {
    margin: 0px 0px;
    border-radius: 0;
    border-left: none;
    border-right: none;
    border-bottom: none;
  }
  .form-group + .form-group {
    margin-top: 10px;
  }
  .form-group + .form-row {
    border-top: 1px solid rgba(0,0,0,.3);
  }

}
.form-group ~ .form-row {
  border-top: none;
}

</style>
