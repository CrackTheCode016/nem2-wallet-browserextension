// Copyright (C) 2019 Contributors as noted in the AUTHORS file
// 
// This file is part of nem2-wallet-browserextension.
// 
// nem2-wallet-browserextension is free software: you can redistribute it and/or modify
// it under the terms of the GNU General Public License as published by
// the Free Software Foundation, either version 3 of the License, or
// (at your option) any later version.
// 
// nem2-wallet-browserextension is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.
// 
// You should have received a copy of the GNU General Public License
// along with nem2-wallet-browserextension.  If not, see <http://www.gnu.org/licenses/>.

<template>
  <div>
    <div v-if="assets.length === 0">
      <v-flex xs12>
        <v-alert
          :value="true"
          type="info"
        >
          There is nothing to show here!
        </v-alert>
      </v-flex>
    </div>
    <div v-if="assets.length > 0">
      <AssetModification
        v-show="modifyAsset"
        :active-asset="activeAsset"
        @closeComponent="modifyAsset = false"
      />
      <v-list
        three-line
        class="py-0 pa-0"
      >
        <v-list-group
          v-for="(a, i) in assets"
          :key="i"
          :prepend-icon="a.action"
          no-action
        >
          <template v-slot:activator>
            <v-list-tile
              ripple
              color="a.active?green:blue"
            >
              <v-list-tile-content class="my-2">
                <div class="asset-list-header asset-list-header-container">
                  <div class="asset-list-header asset-list-header-left">
                    <v-list-tile-title>{{ a.title }}</v-list-tile-title>
                    <v-list-tile-sub-title
                      class="text--primary"
                    >
                      Balance:&nbsp;
                      {{ parseInt(a.balance).toLocaleString() }}&nbsp;
                      [{{ (a.amount/Math.pow(10, a.divisibility)).toLocaleString() }}]
                    </v-list-tile-sub-title>
                    <v-list-tile-sub-title>{{ a.expirationText }}</v-list-tile-sub-title>
                  </div>
                  <div class="asset-list-header asset-list-header-right">
                    <div v-if="ownedAssets">
                      <v-btn
                        small
                        color="primary"
                        :disabled="!a.active"
                        @click.stop
                      >
                        Link a namespace
                      </v-btn>
                      <v-btn
                        small
                        color="primary"
                        :disabled="
                          !(a.active
                            && a.supplyMutable)
                        "
                        @click.stop="
                          modifyAsset = !modifyAsset;
                          activeAsset = a.id;
                        "
                      >
                        Modify supply
                      </v-btn>
                    </div>
                  </div>
                </div>
              </v-list-tile-content>
            </v-list-tile>
          </template>
          <v-list-tile-content>
            <div class="asset-detail">
              <v-list-tile-title>Meta ID: {{ a.metaId }}</v-list-tile-title>
              <v-list-tile-sub-title>Owner: {{ a.owner }}</v-list-tile-sub-title>
              <v-list-tile-sub-title>
                supply: {{ a.supply.toLocaleString() }} | divisibility: {{ a.divisibility }} |&nbsp;
                supplyMutable: {{ a.supplyMutable }} | transferable: {{ a.transferable }}
              </v-list-tile-sub-title>
            </div>
          </v-list-tile-content>
        </v-list-group>
        <v-divider />
      </v-list>
    </div>
  </div>
</template>

<script>
import AssetModification from './AssetModification.vue';

export default {
  name: 'AssetTab',
  components: {
    AssetModification,
  },
  props: {
    // eslint-disable-next-line vue/require-default-prop
    assets: {
      type: Array,
    },
    ownedAssets: {
      type: Boolean,
      default() { return false; },
    },
    activeAsset: {
      type: String,
      default() { return ''; },
    },
  },
  data() {
    return {
      index: 0,
      modifyAsset: false,
    };
  },
};
</script>

<style scoped>
</style>
