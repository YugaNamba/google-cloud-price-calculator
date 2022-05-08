<template>
  <tr>
    <td v-for="text in texts" class="text-right">
      {{ text }}
    </td>
  </tr>
</template>

<script setup lang="ts">
  type Spec = {
    text: string;
    value: number;
  };

  type Props = {
    memorySpec: Spec;
    cpuSpec: Spec;
    memoryPrice: number;
    cpuPrice: number;
    days: number;
  };

  const props = defineProps<Props>();

  const secondPerDay = computed(() => 60 * 60 * 24);

  const memoryCost = computed(() => {
    const cost =
      (props.memorySpec.value / 1024) *
      secondPerDay.value *
      props.days *
      props.memoryPrice;
    return ceil.value(cost, 7);
  });
  const cpuCost = computed(() => {
    const cost =
      (props.cpuSpec.value / 1000) *
      secondPerDay.value *
      props.days *
      props.cpuPrice;
    return ceil.value(cost, 7);
  });

  const total = computed(() => ceil.value(memoryCost.value + cpuCost.value, 2));

  const texts = computed(() => {
    return [
      props.memorySpec.text,
      props.cpuSpec.text,
      memoryCost.value,
      cpuCost.value,
      total.value,
    ];
  });

  const ceil = computed(
    () =>
      (target: number, digits: number = 1) =>
        Math.ceil(target * Math.pow(10, Math.round(digits))) /
        Math.pow(10, Math.round(digits))
  );
</script>

<style lang="scss" scoped></style>
