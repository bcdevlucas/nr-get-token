<template>
  <v-container>
    <h2>My Applications</h2>
    <p>*Submit the Request Account form to add a new application</p>
    <br />
    <v-container fluid class="px-0">
      <v-row dense>
        <v-col cols="12" md="4" xl="2" class="my-app-card">
          <BaseActionCard linkName="RequestAccount">
            <div class="new-app text-center">
              <h3>Add a New Application</h3>
              <v-icon large color="blue darken-4">mdi-plus-circle</v-icon>
            </div>
          </BaseActionCard>
        </v-col>
        <v-col
          v-for="acr in acronyms"
          :key="acr.acronym"
          cols="12"
          md="4"
          xl="2"
          class="my-app-card"
        >
          <BaseActionCard linkName="Application" :linkParams="acr">
            <div class="app-link">
              <h3>{{acr.acronym}}</h3>
              <div class="env-statuses">
                <v-skeleton-loader
                  type="list-item-three-line"
                  :loading="!moduleLoaded"
                  transition="scale-transition"
                >
                  <p>
                    <ClientStatus :env="envs.DEV" :clientStatuses="acr.clientStatus" />
                    <ClientStatus :env="envs.TEST" :clientStatuses="acr.clientStatus" />
                    <ClientStatus :env="envs.PROD" :clientStatuses="acr.clientStatus" />
                  </p>
                </v-skeleton-loader>
              </div>
            </div>
          </BaseActionCard>
        </v-col>
      </v-row>
    </v-container>
  </v-container>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';

import { KcEnv } from '@/utils/constants';
import ClientStatus from '@/components/apiAccess/ClientStatus.vue';

export default {
  name: 'ApplicationList',
  components: {
    ClientStatus
  },
  created() {
    this.loadModule();
  },
  computed: {
    ...mapGetters('user', ['acronyms', 'moduleLoaded']),
    envs: () => KcEnv
  },
  methods: {
    ...mapActions('user', ['loadModule'])
  }
};
</script>

<style scoped>
.my-app-card {
  padding-right: 2rem;
  margin-bottom: 2rem;
  min-height: 9rem;
}

.new-app h3 {
  color: grey;
  margin-bottom: 2.5rem;
  font-weight: 400;
}

.app-link h3 {
  color: #38598a;
  margin-bottom: 2rem;
  font-weight: 400;
}
</style>
