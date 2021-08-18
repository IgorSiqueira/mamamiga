<template>
    <jet-action-section>
        <template #title> Deletar uma equipe </template>

        <template #description> Exclua esta equipe permanentemente. </template>

        <template #content>
            <div class="max-w-xl text-sm text-gray-600">
                Depois que uma equipe é excluída, todos os seus recursos e dados
                serão excluído permanentemente. Antes de excluir esta equipe,
                faça o download quaisquer dados ou informações sobre esta equipe
                que você deseja reter.
            </div>

            <div class="mt-5">
                <jet-danger-button @click="confirmTeamDeletion">
                    Deletar Equipe
                </jet-danger-button>
            </div>

            <!-- Delete Team Confirmation Modal -->
            <jet-confirmation-modal
                :show="confirmingTeamDeletion"
                @close="confirmingTeamDeletion = false"
            >
                <template #title> Delete Team </template>

                <template #content>
                    Are you sure you want to delete this team? Once a team is
                    deleted, all of its resources and data will be permanently
                    deleted.
                </template>

                <template #footer>
                    <jet-secondary-button
                        @click="confirmingTeamDeletion = false"
                    >
                        Cancel
                    </jet-secondary-button>

                    <jet-danger-button
                        class="ml-2"
                        @click="deleteTeam"
                        :class="{ 'opacity-25': form.processing }"
                        :disabled="form.processing"
                    >
                        Delete Team
                    </jet-danger-button>
                </template>
            </jet-confirmation-modal>
        </template>
    </jet-action-section>
</template>

<script>
import JetActionSection from "@/Jetstream/ActionSection.vue";
import JetConfirmationModal from "@/Jetstream/ConfirmationModal.vue";
import JetDangerButton from "@/Jetstream/DangerButton.vue";
import JetSecondaryButton from "@/Jetstream/SecondaryButton.vue";

export default {
    props: ["team"],

    components: {
        JetActionSection,
        JetConfirmationModal,
        JetDangerButton,
        JetSecondaryButton,
    },

    data() {
        return {
            confirmingTeamDeletion: false,
            deleting: false,

            form: this.$inertia.form(),
        };
    },

    methods: {
        confirmTeamDeletion() {
            this.confirmingTeamDeletion = true;
        },

        deleteTeam() {
            this.form.delete(route("teams.destroy", this.team), {
                errorBag: "deleteTeam",
            });
        },
    },
};
</script>
