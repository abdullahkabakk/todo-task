<script setup>
import axios from "axios"
import { onMounted, reactive } from "vue"

let state = reactive({
  data: [], isActive: false,
  taskTitle: "", loading: true,
})

const getUrl = "https://test-api.elselab.io/api/todo/list"

onMounted(async () => {
  await axios.get(getUrl).then(res => {
    state.data = res.data.content
    state.loading = false
  }).catch(() => {
    state.loading = true
  })
})

function saveForm() {
  state.data.push({
    title: state.taskTitle,
    value: state.data.length + 1,
    props: {
      prependIcon: "tabler-check",
      rounded: "xl",
    },
    icon: "check",
  })

  state.taskTitle = ""
  state.isActive = false
}

/**
 *
 * Hints
 * Template Doc: https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/documentation/
 * Dialog: https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/components/dialog
 * List: https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/components/list
 * Axios: https://axios-http.com/docs/intro
 * Demo: https://task1.elselab.io/
 * Use vue.js's ref or reactive feature.
 *
 */
</script>

<template>
  <div>
    <VProgressLinear
      v-if="state.loading"
      indeterminate
      color="primary"
    />
    <VCard
      v-else
      title="Task List"
    >
      <template #text>
        <VList :items="state.data" />
      </template>

      <VCardActions>
        <VDialog
          v-model="state.isActive"
          class="v-dialog-sm"
        >
          <!-- Dialog Activator -->
          <template #activator="{ props }">
            <VBtn
              v-bind="props"
              :disabled="state.isActive"
            >
              ADD NEW TASK
            </VBtn>
          </template>

          <!-- Dialog close btn -->
          <DialogCloseBtn @click="state.isActive = !state.isActive" />

          <!-- Dialog Content -->
          <VCard title="Add New Task">
            <VCardText>
              <VRow>
                <VCol
                  cols="12"
                  sm="12"
                  md="12"
                >
                  <VTextField
                    v-model="state.taskTitle"
                    label="Task Title"
                  />
                </VCol>
              </VRow>
            </VCardText>

            <!-- Form Button -->
            <VCardText class="d-flex justify-end flex-wrap gap-3">
              <VBtn @click="saveForm">
                ADD TASK
              </VBtn>
            </VCardText>
          </VCard>
        </VDialog>
      </VCardActions>
    </VCard>
  </div>
</template>
