<template>
    <jet-action-section>
        <template #title> Deletar conta </template>

        <template #description>Exclua sua conta permanentemente. </template>

        <template #content>
            <div class="max-w-xl text-sm text-gray-600">
                Depois que sua conta for excluída, todos os seus recursos e
                dados serão ser excluído permanentemente. Antes de deletar sua
                conta, por favor baixe quaisquer dados ou informações que você
                deseja reter.
            </div>

            <div class="mt-5">
                <jet-danger-button @click="confirmUserDeletion">
                    Deletar Conta
                </jet-danger-button>
            </div>

            <!-- Delete Account Confirmation Modal -->
            <jet-dialog-modal
                :show="confirmingUserDeletion"
                @close="closeModal"
            >
                <template #title> Deletar conta</template>

                <template #content>
                    Tem certeza que deseja deletar sua conta? Uma vez que o seu
                    conta for excluída, todos os seus recursos e dados serão
                    excluído permanentemente. Por favor digite sua senha para
                    confirmar você gostaria de excluir permanentemente sua
                    conta.

                    <div class="mt-4">
                        <jet-input
                            type="password"
                            class="mt-1 block w-3/4"
                            placeholder="Password"
                            ref="password"
                            v-model="form.password"
                            @keyup.enter="deleteUser"
                        />

                        <jet-input-error
                            :message="form.errors.password"
                            class="mt-2"
                        />
                    </div>
                </template>

                <template #footer>
                    <jet-secondary-button @click="closeModal">
                        Cancelar
                    </jet-secondary-button>

                    <jet-danger-button
                        class="ml-2"
                        @click="deleteUser"
                        :class="{ 'opacity-25': form.processing }"
                        :disabled="form.processing"
                    >
                        Deletar
                    </jet-danger-button>
                </template>
            </jet-dialog-modal>
        </template>
    </jet-action-section>
</template>

<script>
import JetActionSection from "@/Jetstream/ActionSection.vue";
import JetDialogModal from "@/Jetstream/DialogModal.vue";
import JetDangerButton from "@/Jetstream/DangerButton.vue";
import JetInput from "@/Jetstream/Input.vue";
import JetInputError from "@/Jetstream/InputError.vue";
import JetSecondaryButton from "@/Jetstream/SecondaryButton.vue";

export default {
    components: {
        JetActionSection,
        JetDangerButton,
        JetDialogModal,
        JetInput,
        JetInputError,
        JetSecondaryButton,
    },

    data() {
        return {
            confirmingUserDeletion: false,

            form: this.$inertia.form({
                password: "",
            }),
        };
    },

    methods: {
        confirmUserDeletion() {
            this.confirmingUserDeletion = true;

            setTimeout(() => this.$refs.password.focus(), 250);
        },

        deleteUser() {
            this.form.delete(route("current-user.destroy"), {
                preserveScroll: true,
                onSuccess: () => this.closeModal(),
                onError: () => this.$refs.password.focus(),
                onFinish: () => this.form.reset(),
            });
        },

        closeModal() {
            this.confirmingUserDeletion = false;

            this.form.reset();
        },
    },
};
</script>
