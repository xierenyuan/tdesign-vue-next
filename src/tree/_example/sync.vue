<template>
  <t-space direction="vertical">
    <t-space direction="vertical">
      <t-input-adornment prepend="checked:">
        <t-input :value="allChecked" />
      </t-input-adornment>
      <t-input-adornment prepend="expanded:">
        <t-input :value="allExpanded" />
      </t-input-adornment>
      <t-input-adornment prepend="activated:">
        <t-input :value="allActived" />
      </t-input-adornment>
    </t-space>
  </t-space>
  <t-tree
    v-model="checked"
    v-model:expanded="expanded"
    v-model:actived="actived"
    :data="items"
    checkable
    activable
    :expand-on-click-node="false"
    :active-multiple="false"
    :value-mode="valueMode"
  />
</template>

<script setup>
import { ref, computed } from 'vue';

const items = [
  {
    value: '1',
    label: '1',
    children: [
      {
        value: '1.1',
        label: '1.1',
        children: [
          {
            value: '1.1.1',
            label: '1.1.1',
            children: [
              {
                value: '1.1.1.1',
                label: '1.1.1.1',
              },
              {
                value: '1.1.1.2',
                label: '1.1.1.2',
              },
            ],
          },
          {
            value: '1.1.2',
            label: '1.1.2',
            children: [
              {
                value: '1.1.2.1',
                label: '1.1.2.1',
              },
              {
                value: '1.1.2.2',
                label: '1.1.2.2',
              },
            ],
          },
        ],
      },
      {
        value: '1.2',
        label: '1.2',
        children: [
          {
            value: '1.2.1',
            label: '1.2.1',
            children: [
              {
                value: '1.2.1.1',
                label: '1.2.1.1',
              },
              {
                value: '1.2.1.2',
                label: '1.2.1.2',
              },
            ],
          },
          {
            value: '1.2.2',
            label: '1.2.2',
            children: [
              {
                value: '1.2.2.1',
                label: '1.2.2.1',
              },
              {
                value: '1.2.2.2',
                label: '1.2.2.2',
              },
            ],
          },
        ],
      },
    ],
  },
  {
    value: '2',
    label: '2',
    checkable: false,
    children: [
      {
        value: '2.1',
        label: '2.1',
        checkable: false,
      },
      {
        value: '2.2',
        label: '2.2',
        checkable: false,
      },
    ],
  },
];

const checked = ref(['1.1.1.1', '1.1.1.2']);
const expanded = ref(['1', '1.1', '1.1.1', '2']);
const actived = ref(['2']);
const valueMode = 'onlyLeaf';

const allChecked = computed(() => {
  let arr = [];
  if (Array.isArray(checked.value)) {
    arr = checked.value;
  }
  return arr.join(', ');
});

const allExpanded = computed(() => {
  let arr = [];
  if (Array.isArray(expanded.value)) {
    arr = expanded.value;
  }
  return arr.join(', ');
});

const allActived = computed(() => {
  let arr = [];
  if (Array.isArray(actived.value)) {
    arr = actived.value;
  }
  return arr.join(', ');
});

const getValueFromString = (val) => {
  const arr = val.split(',');
  const vals = [];
  arr
    .map((str) => str.trim())
    .forEach((tag) => {
      const match = /^\{([^{}]+)\}$/.exec(tag);
      if (match && match[1]) {
        vals.push(match[1]);
      }
    });
  return vals;
};

const onAllCheckedInput = (val) => {
  console.log('checked input on change', val);
  const vals = getValueFromString(val);
  console.log(vals);
  checked.value = vals;
};

const onAllExpandedInput = (val) => {
  console.log('expanded input on change', val);
  const vals = getValueFromString(val);
  expanded.value = vals;
};

const onAllActivedInput = (val) => {
  console.log('actived input on change', val);
  const vals = getValueFromString(val);
  actived.value = vals;
};
</script>
<style scoped>
.demo-tree-base {
  display: block;
}
</style>
