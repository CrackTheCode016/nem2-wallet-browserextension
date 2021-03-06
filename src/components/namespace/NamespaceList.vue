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
  <v-layout column>
    <div v-if="sharedState.loading_getNamespacesByAddress">
      <v-progress-linear :indeterminate="true" />
    </div>
    <div
      v-if="!sharedState.loading_getNamespacesByAddress
        && sharedState.namespaces.length === 0"
    >
      <v-flex xs12>
        <v-alert
          :value="true"
          type="info"
        >
          This account does not own any namespace.
        </v-alert>
      </v-flex>
    </div>
    <div
      v-if="!sharedState.loading_getNamespacesByAddress
        && sharedState.namespaces.length > 0"
    >
      <v-layout
        row
        justify-start
        pl-3
      >
        <h6 class="headline pt-3">
          List of owned namespaces
        </h6>
      </v-layout>
      <v-list
        three-line
        class="py-0"
      >
        <v-slide-y-transition
          group
          tag="v-list"
          class="py-0"
        >
          <template v-for="(ns, index) in sharedState.namespaces">
            <v-layout
              :key="ns.name"
              column
            >
              <v-list-tile
                avatar
                ripple
              >
                <v-list-tile-content class="my-2">
                  <v-list-tile-title>
                    {{ ns.name }} - <span class="">{{ ns.hexId }}</span>
                  </v-list-tile-title>
                  <v-list-tile-sub-title>
                    <div class="monospaced">
                      {{ ns.expire }}
                    </div>
                  </v-list-tile-sub-title>
                  <v-list-tile-sub-title>
                    <div class="monospaced">
                      {{ ns.type }} {{ ns.alias }}
                    </div>
                  </v-list-tile-sub-title>
                </v-list-tile-content>
                <v-list-tile-action>
                  <v-btn
                    icon
                    @click="ns.expand.isExpandMore = !ns.expand.isExpandMore"
                  >
                    <v-icon> {{ ns.expand.isExpandMore ? "expand_more" : "expand_less" }} </v-icon>
                  </v-btn>
                </v-list-tile-action>
              </v-list-tile>
              <v-layout>
                <AliasTransaction
                  v-show="ns.expand.isExpandMore"
                  class="pa-3"
                  :alias-action-type="ns.expand.aliasActionType"
                  :namespace-name="ns.expand.namespaceName"
                  :current-alias="ns.expand.currentAlias"
                  :current-alias-type="ns.expand.currentAliasType"
                />
              </v-layout>
            </v-layout>
            <v-divider
              v-if="index + 1 < sharedState.namespaces.length"
              :key="index"
            />
          </template>
        </v-slide-y-transition>
      </v-list>
    </div>
  </v-layout>
  </div>
</template>
<script>
import StateRepository from '../../infrastructure/StateRepository';
import AliasTransaction from './AliasTransaction.vue';

export default {
  components: { AliasTransaction },
  props: {
    reloadNamespaceNotifier: {
      type: Number,
      default() {
        return 0;
      },
    },
  },
  data() {
    return {
      sharedState: StateRepository.state,
    };
  },
};

</script>
<style scoped>
</style>
