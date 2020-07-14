<template>
    <modal close-button-text="Ok, got it">
        <div slot="header">
            <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
            <i class="icons8-info"/>
        </div>
        <div slot="body">
            <div class="form-box">
                <div class="text-block">
                    <div class="logo-image">
                        <img src="~/images/nhs-images/NHS-logo.png">
                    </div>
                    <div class="text-block-title">
                        <slot name="title"></slot>
                    </div>
                    <div class="text-block-body">
                        <slot name="body-text"></slot>
                    </div>
                </div>
                <div class="email-form">
                    <slot name="form"></slot>
                </div>
            </div>
        </div>
    </modal>
</template>

<script>
    import Modal from 'Modal';
    import ModalBase from 'ModalBase';

    export default {
        name: 'nhs-modal',

        components: { Modal },

        mixins: [ModalBase],

        mounted() {
            window.bus.$on('showNhsModal', this.showModal);
        },
    }
</script>

<style lang="less">
    #nhs-guaranteed-modal {
        .icons8-info {
            display: none !important;
        }

        .modal-dialog {
            text-align: center;
            font-family: Montserrat, sans-serif;
            font-size: 16px;
            font-weight: 400;
        }

        .modal-content {
            max-width: 620px;
            box-shadow: 0 0 16px rgba(0, 0, 0, 0.28);
            border-radius: 12px;
            border: 1px solid #979797;
            background-color: #ffffff;
            margin: 0 auto;
        }
        .modal-header {
            color: #92c62e;
            font-size: 35px;
            @media (min-width: @screen-sm-min) {
                padding-top: 42px;
            }
        }
        .modal-body {
            padding: 0;

            @media (min-width: @screen-sm-min) {
                padding: 6px 45px;
            }
            p {
                margin: 0;
            }
        }

        .modal-footer {
            display: none;
        }

        .form-box {
            max-width: 620px;
            margin: 0 auto;
            width: calc(100% - 30px);
            background-color: #fff;
            padding: 5px;

            @media (min-width: @screen-sm-min) {
                padding: 25px;
            }

            .logo-image {
                display: flex;
                justify-content: center;
                margin-bottom: 20px;
            }
        }

        .text-block {
            text-align: center;
            max-width: 510px;
            margin: 0 auto;

            &-title {
                color: #777777;
                font-family: Montserrat, sans-serif;
                font-size: 28px;
                font-weight: 600;
                margin-bottom: 20px;

                @media (min-width: @screen-sm-min) {
                    font-size: 36px;
                }
            }

            p {
                color: #777777;
                font-family: Montserrat, sans-serif;
                font-size: 13px;
                font-weight: 500;
                line-height: 26px;
                margin-bottom: 20px;

                @media (min-width: @screen-sm-min) {
                    font-size: 18px;
                }
            }
        }

        .email-form {
            max-width: 300px;
            margin: 40px auto 20px;

            h4 {
                color: #2f3f46;
                font-family: Montserrat, sans-serif;
                font-size: 16px;
                font-weight: 700;
            }
        }
    }
</style>
