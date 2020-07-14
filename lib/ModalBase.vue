<script>
    export default {

        data () {
            return {
                autoshow: false,
            }
        },

        mounted() {
            this.$nextTick(() => {
                this.registerModalEvents()
            });
            if (this.autoshow) {
                this.showModal();
            }
        },

        methods: {
            registerModalEvents() {
                ['shown', 'show', 'hide', 'hidden'].forEach((event) => {
                    const eventName = `${event}.bs.modal`;
                    $(this.$el).on(eventName, (e) => {
                        this.$emit(event, e);
                    });
                });
            },

            showModal() {
                $(this.$el).modal('show')
            },

            hideModal() {
                $(this.$el).modal('hide')
            },
        },
    }
</script>
