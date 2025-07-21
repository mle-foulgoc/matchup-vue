<template>
  <div class="items-page">
    <h1>Objets disponibles sur la Faille de l'invocateur</h1>
    <div class="items-list">
      <div
        v-for="(item, id) in filteredItems"
        :key="id"
        class="item-card"
        @mouseenter="showTooltip($event, item, id)"
        @mouseleave="hideTooltip"
      >
        <img
          class="item-img"
          :src="`/img/item/${item.image.full}`"
          :alt="item.name"
        />
        <div class="item-info">
          <strong>{{ item.name }}</strong>
          <span class="item-price">{{ item.gold.total }}g</span>
          <span class="item-efficiency"
            >{{ getEfficiency(item) }}% efficient</span
          >
        </div>
      </div>
    </div>
    <div
      v-if="tooltip.visible"
      class="custom-tooltip"
      :style="{ top: tooltip.y + 'px', left: tooltip.x + 'px' }"
    >
      <div class="tooltip-header">
        <img
          :src="`/img/item/${tooltip.item?.image.full}`"
          class="tooltip-img"
          v-if="tooltip.item"
        />
        <div>
          <div class="tooltip-title">{{ tooltip.item?.name }}</div>
          <div class="tooltip-gold">{{ tooltip.item?.gold.total }}g</div>
        </div>
      </div>
      <div class="tooltip-description" v-html="tooltip.item?.description"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import itemJson from "@/assets/data/15.13.1/data/fr_FR/item.json";

const itemsData = itemJson.data;

// Table de conversion des stats en valeur d'or
const statGoldValues = {
  FlatPhysicalDamageMod: 35, // AD pour 1
  FlatMagicDamageMod: 20, // AP pour 1
  FlatArmorMod: 20, // Armor pour 1
  FlatSpellBlockMod: 20, // MR pour 1
  FlatHPPoolMod: 2.67, // HP pour 1
  FlatMPPoolMod: 1, // Mana pour 1
  AbilityHaste: 10, // Ability Haste pour 1 (5 AH = 50g donc 1 AH = 10g)
  FlatHPRegenMod: 3, // HP5 pour 1
  FlatMPRegenMod: 4, // MP5 pour 1
  FlatCritChanceMod: 2.67 * 15, // 15% Crit = 40g donc 1% = 2.67g
  PercentAttackSpeedMod: 2.5 * 10, // 10% AS = 25g donc 1% = 2.5g
  FlatMovementSpeedMod: 0.48 * 25, // 25 MS = 12g donc 1 MS = 0.48g
};

function getItemGoldValue(stats) {
  let total = 0;
  if (!stats) return 0;
  for (const [key, value] of Object.entries(stats)) {
    if (value && statGoldValues[key]) {
      // Pour FlatCritChanceMod et PercentAttackSpeedMod, la valeur est en décimal (ex: 0.15 pour 15%)
      if (key === "FlatCritChanceMod") {
        total += value * 100 * 2.67; // 1% = 2.67g
      } else if (key === "PercentAttackSpeedMod") {
        total += value * 100 * 2.5; // 1% = 2.5g
      } else if (key === "FlatMovementSpeedMod") {
        total += value * 0.48; // 1 MS = 0.48g
      } else if (key === "AbilityHaste") {
        total += value * 10; // 1 AH = 10g
      } else {
        total += value * statGoldValues[key];
      }
    }
  }
  return total;
}

const filteredItems = computed(() => {
  // On ne garde que les items disponibles sur la map 11 (Faille de l'invocateur) ET dont le prix total est supérieur à 0g
  return Object.entries(itemsData)
    .filter(([id, item]) => item.maps["11"] && item.gold && item.gold.total > 0)
    .reduce((acc, [id, item]) => {
      acc[id] = item;
      return acc;
    }, {});
});

function getEfficiency(item) {
  const goldValue = getItemGoldValue(item.stats);
  if (!item.gold || !item.gold.total) return 0;
  return Math.round((goldValue / item.gold.total) * 100);
}

const tooltip = ref({ visible: false, x: 0, y: 0, item: null });

function showTooltip(event, item, id) {
  const rect = event.currentTarget.getBoundingClientRect();
  tooltip.value = {
    visible: true,
    x: rect.right + 12 + window.scrollX,
    y: rect.top + window.scrollY,
    item,
  };
}
function hideTooltip() {
  tooltip.value.visible = false;
}
</script>

<style scoped>
.items-page {
  max-width: 1100px;
  margin: 0 auto;
  padding: 2rem;
}
.items-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 1.2rem;
}
.item-card {
  background: #212121;
  border-radius: 8px;
  padding: 1rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  cursor: pointer;
}
.item-img {
  width: 64px;
  height: 64px;
  object-fit: contain;
  margin-bottom: 0.7rem;
}
.item-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
}
.item-price {
  color: #e6c06b;
  font-size: 1em;
  font-weight: bold;
}
.item-efficiency {
  color: #b0e57c;
  font-size: 0.95em;
  font-weight: bold;
  margin-top: 0.1em;
}

.custom-tooltip {
  position: absolute;
  z-index: 1000;
  min-width: 320px;
  max-width: 350px;
  background: #10131a;
  color: #fff;
  border: 1px solid #23272e;
  border-radius: 8px;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.5);
  padding: 1.1rem 1.2rem;
  font-size: 1.05em;
  pointer-events: none;
  transition: opacity 0.15s;
}
.tooltip-header {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 0.7rem;
}
.tooltip-img {
  width: 48px;
  height: 48px;
  border-radius: 6px;
  border: 1px solid #333;
}
.tooltip-title {
  font-size: 1.25em;
  font-weight: bold;
  color: #fff;
}
.tooltip-gold {
  color: #e6c06b;
  font-size: 1.1em;
  font-weight: bold;
}
.tooltip-description {
  margin-top: 0.5rem;
  color: #e0e0e0;
  font-size: 1em;
}
</style>
