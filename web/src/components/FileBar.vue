<template>
  <div class="q-pb-sm">
    <q-bar>
      <q-btn-group flat>
        <q-btn size="md" dense no-caps flat label="File">
          <q-menu>
            <q-list dense style="min-width: 100px">
              <q-item clickable>
                <q-item-section>Add</q-item-section>
                <q-item-section side>
                  <q-icon name="keyboard_arrow_right" />
                </q-item-section>
                <q-menu anchor="top right" self="top left">
                  <q-list dense style="min-width: 100px">
                    <q-item clickable v-close-popup @click="showAddClientModal">
                      <q-item-section>Client</q-item-section>
                    </q-item>
                    <q-item clickable v-close-popup @click="showAddSiteModal">
                      <q-item-section>Site</q-item-section>
                    </q-item>
                  </q-list>
                </q-menu>
              </q-item>

              <q-item clickable v-close-popup @click="showAuditManager">
                <q-item-section>Audit Log</q-item-section>
              </q-item>
              <q-item clickable v-close-popup @click="showDebugLog">
                <q-item-section>Debug Log</q-item-section>
              </q-item>
            </q-list>
          </q-menu>
        </q-btn>
        <!-- view -->
        <q-btn size="md" dense no-caps flat label="View">
          <q-menu auto-close>
            <q-list dense style="min-width: 100px">
              <q-item clickable v-close-popup @click="showPendingActions">
                <q-item-section>Pending Actions</q-item-section>
              </q-item>
            </q-list>
          </q-menu>
        </q-btn>
        <!-- agents -->
        <q-btn size="md" dense no-caps flat label="Agents">
          <q-menu auto-close>
            <q-list dense style="min-width: 100px">
              <q-item clickable v-close-popup @click="showInstallAgent = true">
                <q-item-section>Install Agent</q-item-section>
              </q-item>
              <q-item clickable v-close-popup @click="showDeployments">
                <q-item-section>Manage Deployments</q-item-section>
              </q-item>
              <q-item clickable v-close-popup @click="showUpdateAgentsModal = true">
                <q-item-section>Update Agents</q-item-section>
              </q-item>
            </q-list>
          </q-menu>
        </q-btn>

        <!-- settings -->
        <q-btn size="md" dense no-caps flat label="Settings">
          <q-menu auto-close>
            <q-list dense style="min-width: 100px">
              <!-- clients manager -->
              <q-item clickable v-close-popup @click="showClientsManager">
                <q-item-section>Clients Manager</q-item-section>
              </q-item>
              <!-- script manager -->
              <q-item clickable v-close-popup @click="showScriptManager">
                <q-item-section>Script Manager</q-item-section>
              </q-item>
              <!-- automation manager -->
              <q-item clickable v-close-popup @click="showAutomationManager">
                <q-item-section>Automation Manager</q-item-section>
              </q-item>
              <!-- alerts manager -->
              <q-item clickable v-close-popup @click="showAlertsManager">
                <q-item-section>Alerts Manager</q-item-section>
              </q-item>
              <!-- permissions manager -->
              <q-item clickable v-close-popup @click="showPermissionsManager">
                <q-item-section>Permissions Manager</q-item-section>
              </q-item>
              <!-- admin manager -->
              <q-item clickable v-close-popup @click="showAdminManager = true">
                <q-item-section>User Administration</q-item-section>
              </q-item>
              <!-- core settings -->
              <q-item clickable v-close-popup @click="showEditCoreSettingsModal = true">
                <q-item-section>Global Settings</q-item-section>
              </q-item>
              <!-- code sign -->
              <q-item v-if="!hosted" clickable v-close-popup @click="showCodeSign = true">
                <q-item-section>Code Signing</q-item-section>
              </q-item>
            </q-list>
          </q-menu>
        </q-btn>
        <!-- tools -->
        <q-btn size="md" dense no-caps flat label="Tools">
          <q-menu auto-close>
            <q-list dense style="min-width: 100px">
              <!-- bulk command -->
              <q-item clickable v-close-popup @click="showBulkAction('command')">
                <q-item-section>Bulk Command</q-item-section>
              </q-item>
              <!-- bulk script -->
              <q-item clickable v-close-popup @click="showBulkAction('script')">
                <q-item-section>Bulk Script</q-item-section>
              </q-item>
              <!-- bulk patch management -->
              <q-item clickable v-close-popup @click="showBulkAction('patch')">
                <q-item-section>Bulk Patch Management</q-item-section>
              </q-item>
              <!-- server maintenance -->
              <q-item clickable v-close-popup @click="showServerMaintenance = true">
                <q-item-section>Server Maintenance</q-item-section>
              </q-item>
            </q-list>
          </q-menu>
        </q-btn>
        <!-- help -->
        <q-btn v-if="!hosted" size="md" dense no-caps flat label="Help">
          <q-menu auto-close>
            <q-list dense style="min-width: 100px">
              <q-item clickable v-close-popup @click="openHelp('docs')">
                <q-item-section>Documentation</q-item-section>
              </q-item>
              <q-item clickable v-close-popup @click="openHelp('github')">
                <q-item-section>GitHub Repo</q-item-section>
              </q-item>
              <q-item clickable v-close-popup @click="openHelp('bug')">
                <q-item-section>Bug Report</q-item-section>
              </q-item>
              <q-item clickable v-close-popup @click="openHelp('feature')">
                <q-item-section>Feature Request</q-item-section>
              </q-item>
              <q-item clickable v-close-popup @click="openHelp('discord')">
                <q-item-section>Join Discord</q-item-section>
              </q-item>
            </q-list>
          </q-menu>
        </q-btn>
      </q-btn-group>
      <q-space />
      <!-- edit core settings modal -->
      <q-dialog v-model="showEditCoreSettingsModal">
        <EditCoreSettings @close="showEditCoreSettingsModal = false" />
      </q-dialog>
      <!-- Install Agents -->
      <div class="q-pa-md q-gutter-sm">
        <q-dialog v-model="showInstallAgent">
          <InstallAgent @close="showInstallAgent = false" />
        </q-dialog>
      </div>
      <!-- Update Agents Modal -->
      <div class="q-pa-md q-gutter-sm">
        <q-dialog v-model="showUpdateAgentsModal" maximized transition-show="slide-up" transition-hide="slide-down">
          <UpdateAgents @close="showUpdateAgentsModal = false" />
        </q-dialog>
      </div>
      <!-- Admin Manager -->
      <div class="q-pa-md q-gutter-sm">
        <q-dialog v-model="showAdminManager">
          <AdminManager @close="showAdminManager = false" />
        </q-dialog>
      </div>
      <!-- Server Maintenance -->
      <q-dialog v-model="showServerMaintenance">
        <ServerMaintenance @close="showMaintenance = false" />
      </q-dialog>
      <!-- Code Sign -->
      <q-dialog v-model="showCodeSign">
        <CodeSign @close="showCodeSign = false" />
      </q-dialog>
    </q-bar>
  </div>
</template>

<script>
import DialogWrapper from "@/components/ui/DialogWrapper";
import DebugLog from "@/components/logs/DebugLog";
import PendingActions from "@/components/logs/PendingActions";
import ClientsManager from "@/components/clients/ClientsManager";
import ClientsForm from "@/components/clients/ClientsForm";
import SitesForm from "@/components/clients/SitesForm";
import UpdateAgents from "@/components/modals/agents/UpdateAgents";
import ScriptManager from "@/components/scripts/ScriptManager";
import EditCoreSettings from "@/components/modals/coresettings/EditCoreSettings";
import AlertsManager from "@/components/AlertsManager";
import AutomationManager from "@/components/automation/AutomationManager";
import AdminManager from "@/components/AdminManager";
import InstallAgent from "@/components/modals/agents/InstallAgent";
import AuditManager from "@/components/logs/AuditManager";
import BulkAction from "@/components/modals/agents/BulkAction";
import Deployment from "@/components/clients/Deployment";
import ServerMaintenance from "@/components/modals/core/ServerMaintenance";
import CodeSign from "@/components/modals/coresettings/CodeSign";
import PermissionsManager from "@/components/accounts/PermissionsManager";

export default {
  name: "FileBar",
  inject: ["refreshDashboard"],
  components: {
    UpdateAgents,
    EditCoreSettings,
    InstallAgent,
    AdminManager,
    ServerMaintenance,
    CodeSign,
    PermissionsManager,
  },
  data() {
    return {
      showServerMaintenance: false,
      showUpdateAgentsModal: false,
      showEditCoreSettingsModal: false,
      showAdminManager: false,
      showInstallAgent: false,
      showCodeSign: false,
    };
  },
  computed: {
    hosted() {
      return this.$store.state.hosted;
    },
  },
  methods: {
    openHelp(mode) {
      let url;
      switch (mode) {
        case "github":
          url = "https://github.com/wh1te909/tacticalrmm/";
          break;
        case "docs":
          url = "https://wh1te909.github.io/tacticalrmm/";
          break;
        case "bug":
          url = "https://github.com/wh1te909/tacticalrmm/issues/new?template=bug_report.md";
          break;
        case "feature":
          url = "https://github.com/wh1te909/tacticalrmm/issues/new?template=feature_request.md";
          break;
        case "discord":
          url = "https://discord.gg/upGTkWp";
          break;
      }
      window.open(url, "_blank");
    },
    showAutomationManager() {
      this.$q.dialog({
        component: AutomationManager,
      });
    },
    showAlertsManager() {
      this.$q
        .dialog({
          component: AlertsManager,
        })
        .onDismiss(this.refreshDashboard);
    },
    showClientsManager() {
      this.$q
        .dialog({
          component: ClientsManager,
        })
        .onDismiss(() => this.refreshDashboard(false));
    },
    showAddClientModal() {
      this.$q
        .dialog({
          component: ClientsForm,
        })
        .onOk(this.refreshDashboard);
    },
    showAddSiteModal() {
      this.$q
        .dialog({
          component: SitesForm,
        })
        .onOk(this.refreshDashboard);
    },
    showPermissionsManager() {
      this.$q.dialog({
        component: PermissionsManager,
      });
    },
    showAuditManager() {
      this.$q.dialog({
        component: DialogWrapper,
        componentProps: {
          vuecomponent: AuditManager,
          noCard: true,
          componentProps: {
            modal: true,
          },
          dialogProps: {
            maximized: true,
            ["transition-show"]: "slide-up",
            ["transition-hide"]: "slide-down",
          },
        },
      });
    },
    showScriptManager() {
      this.$q.dialog({
        component: ScriptManager,
      });
    },
    showBulkAction(mode) {
      this.$q.dialog({
        component: BulkAction,
        componentProps: {
          mode: mode,
        },
      });
    },
    showDebugLog() {
      this.$q.dialog({
        component: DialogWrapper,
        componentProps: {
          vuecomponent: DebugLog,
          noCard: true,
          componentProps: {
            modal: true,
          },
          dialogProps: {
            maximized: true,
            ["transition-show"]: "slide-up",
            ["transition-hide"]: "slide-down",
          },
        },
      });
    },
    showPendingActions() {
      this.$q
        .dialog({
          component: PendingActions,
        })
        .onDismiss(this.refreshDashboard);
    },
    showDeployments() {
      this.$q.dialog({
        component: Deployment,
      });
    },
  },
};
</script>
