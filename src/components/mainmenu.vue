<script setup>
import { useCustomizeThemeStore } from "@/stores/Theme.js";
const customizeTheme = useCustomizeThemeStore();
const props = defineProps({
  // Data
  menu: {
    type: Array,
    default: () => [],
  },
});

</script>
<template>
   <v-list class="menu-list" nav dense color="primary">
    <template v-for="menuArea in props.menu" :key="menuArea.key">
      <div
        v-if="!customizeTheme.miniSidebar && (menuArea.key || menuArea.text)"
        class="pa-1 mt-2 text-overline"
      >
        {{ menuArea.key ? $t(menuArea.key) : menuArea.text }}
      </div>
      <template v-if="menuArea.items">
        <template v-for="menuItem in menuArea.items" :key="menuItem.key">
          <!-- menu level 1 -->
          <v-list-item
            v-if="!menuItem.items"
            :to="menuItem.link"
            :prepend-icon="menuItem.icon || 'mdi-circle-medium'"
            :active-class="`active-nav-${customizeTheme.primaryColor.colorName}`"
            density="compact"
          >
            <v-list-item-title
              v-text="menuItem.key ? $t(menuItem.key) : menuItem.text"
            ></v-list-item-title>
          </v-list-item>
          <v-list-group v-else :value="menuItem.items">
            <!-- subMenu activator -->
            <template v-slot:activator="{ props }">
              <v-list-item
                v-bind="props"
                :prepend-icon="menuItem.icon || 'mdi-circle-medium'"
                :title="menuItem.key ? $t(menuItem.key) : menuItem.text"
              >
              </v-list-item>
            </template>
            <!-- menu level 2 -->
            <v-list-item
              v-for="subMenuItem in menuItem.items"
              :key="subMenuItem.key"
              :prepend-icon="subMenuItem.icon || 'mdi-circle-medium'"
              :title="subMenuItem.key ? $t(subMenuItem.key) : subMenuItem.text"
              :to="subMenuItem.link"
              density="compact"
            ></v-list-item>
          </v-list-group>
        </template>
      </template>
    </template>
  </v-list>
</template>
<script>

</script>
<style scoped>
.v-list-group .v-list-item {
  padding-left: 8px !important;
}
</style>