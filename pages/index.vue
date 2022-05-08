<template>
  <div>
    <h2>Cloud Functions</h2>
    <h3>
      アイドル状態1秒当たりの料金
      <span class="text-subtitle-1"
        ><a
          href="https://cloud.google.com/skus/?hl=ja&currency=JPY&filter=Cloud+Functions"
          target="_blank"
          rel="noopener noreferrer"
          >こちら</a
        ></span
      >
    </h3>
    <div class="d-flex mt-4">
      <v-text-field
        v-model="memoryPrice"
        label="Memory"
        suffix="円"
        type="number"
        density="compact"
        variant="outlined"
        hide-details
        min="0.000100000"
        max="0.005000000"
        step="0.000000100"
      />
      <v-text-field
        v-model="cpuPrice"
        label="CPU"
        suffix="円"
        type="number"
        density="compact"
        variant="outlined"
        hide-details
        min="0.000100000"
        max="0.005000000"
        step="0.000000100"
      />
      <v-select
        v-model="days"
        label="日数"
        :items="['1', '2', '3', '5', '10', '15', '20', '30']"
        density="compact"
        variant="outlined"
        hide-details
      />
    </div>
    <v-table density="compact" class="table mt-4">
      <thead>
        <tr>
          <th v-for="header in firstHeaders" :class="header.class">
            {{ header.text }}
          </th>
        </tr>
      </thead>
      <tbody>
        <CloudFunctionsTableItem
          v-for="item in items"
          class="table__item"
          :memorySpec="item.memorySpec"
          :cpuSpec="item.cpuSpec"
          :memoryPrice="memoryPrice"
          :cpuPrice="cpuPrice"
          :days="Number(days)"
        />
      </tbody>
    </v-table>
  </div>
</template>

<script setup lang="ts">
  import CloudFunctionsTableItem from "~/components/molecules/CloudFunctionsTableItem.vue";

  const memoryPrice = ref(0.000320337);
  const cpuPrice = ref(0.000133516);
  const days = ref("30");

  const firstHeaders = computed(() => [
    { text: "Memory(MB)", class: "text-center" },
    { text: "CPU(MHz)", class: "text-center" },
    { text: "Memory(円)", class: "text-center" },
    { text: "CPU(円)", class: "text-center" },
    { text: "合計(日/円)", class: "text-center" },
  ]);

  const items = computed(() => {
    return memorySpecs.value.map((_, index) => {
      return {
        memorySpec: memorySpecs.value[index],
        cpuSpec: cpuSpecs.value[index],
      };
    });
  });

  const memorySpecs = computed(() => {
    return [
      { text: "128MB", value: 128 },
      { text: "256MB", value: 256 },
      { text: "512MB", value: 512 },
      { text: "1GB", value: 1024 },
      { text: "2GB", value: 2048 },
      { text: "4GB", value: 4096 },
      { text: "8GB", value: 8192 },
    ];
  });

  const cpuSpecs = computed(() => {
    return [
      { text: "200MHz", value: 200 },
      { text: "400MHz", value: 400 },
      { text: "800MHz", value: 800 },
      { text: "1.4GHz", value: 1400 },
      { text: "2.4GHz", value: 2400 },
      { text: "4.8GHz", value: 4800 },
      { text: "4.8GHz", value: 4800 },
    ];
  });
</script>

<style lang="scss" scoped>
  .table {
    width: 600px;
    border: 1px solid black;

    &__item {
      &:nth-child(odd) {
        background-color: lightgray;
      }
    }
  }
  .d-flex {
    width: 600px;
    gap: 16px;
  }
</style>
