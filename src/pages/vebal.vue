<script setup lang="ts">
import Hero from '@/components/contextual/pages/vebal/Hero.vue';

import useGauges from '@/composables/vebal/useGauges';
import useWeb3 from '@/services/web3/useWeb3';
import usePoolFilters from '@/composables/pools/usePoolFilters';

import { isVeBalSupported } from '@/composables/useVeBAL';
import GaugesTable from '@/components/tables/GaugesTables/GaugesTable.vue';

const { isWalletReady } = useWeb3();

const {
  selectedTokens,
  addSelectedToken,
  removeSelectedToken
} = usePoolFilters();

const {
  gauges,
  isLoadingGauges,
} = useGauges();
</script>

<template>
  <div v-if="isVeBalSupported" class="lg:container lg:mx-auto pt-10 md:pt-12">
    <Hero />
    <template v-if="isWalletReady">
      <div class="px-4 lg:px-0">
        <BalStack horizontal justify="between" align="center">
          <h3>Pools eligible for liquidity mining</h3>
        </BalStack>
      </div>
      <GaugesTable
        :isLoading="isLoadingGauges"
        :data="gauges"
        :noPoolsLabel="$t('noInvestments')"
        showPoolShares
        :selectedTokens="selectedTokens"
        class="mb-8"
      />
    </template>
  </div>
  <div v-else class="text-center">
    <div class="font-semibold text-lg">
      {{ $t('veBAL.notSupported.title') }}
    </div>
    <div>{{ $t('veBAL.notSupported.description') }}</div>
  </div>
</template>
