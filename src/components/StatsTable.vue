<template>
  <v-table>
    <tbody>
      <tr>
        <td>
          <img
            src="@/assets/Icones/Attack_damage_icon.png"
            alt="AD"
            width="24"
            style="vertical-align: middle; margin-right: 6px"
          />
          {{ stats.attackdamage }}
        </td>
        <td>
          <img
            src="@/assets/Icones/Health_icon.png"
            alt="HP"
            width="24"
            style="vertical-align: middle; margin-right: 6px"
          />
          {{ stats.hp }}
        </td>
      </tr>
      <tr>
        <td>
          <img
            src="@/assets/Icones/Armor_icon.png"
            alt="Armor"
            width="24"
            style="vertical-align: middle; margin-right: 6px"
          />
          {{ stats.armor }}
        </td>
        <td>
          <img
            src="@/assets/Icones/Magic_resistance_icon.png"
            alt="Magic Resist"
            width="24"
            style="vertical-align: middle; margin-right: 6px"
          />
          {{ stats.spellblock }}
        </td>
      </tr>
      <tr>
        <td>
          <img
            src="@/assets/Icones/Critical_strike_chance_icon.png"
            alt="Crit"
            width="24"
            style="vertical-align: middle; margin-right: 6px"
          />
          {{ stats.crit }}
        </td>
        <td>
          <img
            src="@/assets/Icones/Movement_speed_icon.png"
            alt="Move Speed"
            width="24"
            style="vertical-align: middle; margin-right: 6px"
          />
          {{ stats.movespeed }}
        </td>
      </tr>
    </tbody>
  </v-table>
</template>

<script setup>
const props = defineProps({
  stats: {
    type: Object,
    required: true,
  },
});
</script>
