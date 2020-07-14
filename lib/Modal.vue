<template>
    <div
        :id="$parent.$options.name"
        :aria-labelledby="`${ $parent.$options.name } Label`"
        :class="{ 'fade': animate, 'has-image': !!image }"
        :data-backdrop="showModalBackdrop"
        class="modal"
        tabindex="-1"
        role="dialog"
    >
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div
                    v-if="!!image"
                    :style="{ backgroundImage: `url(${image})` }"
                    class="image"
                />
                <div class="modal-inner">
                    <div v-if="headerShouldShow" class="modal-header" >
                        <slot name="header">
                            <button
                                type="button"
                                class="close"
                                data-dismiss="modal"
                                aria-label="Close"
                            ><span aria-hidden="true">&times;</span></button>
                            <h4
                                v-if="title"
                                :id="`${ $parent.$options.name } Label`"
                                :class="{ 'has-title-icon': showTitleIcon }"
                                class="modal-title"
                            >
                                <span v-if="showTitleIcon"><img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABYAAAAWCAYAAADEtGw7AAACMklEQVRIS7WVPUwTYRjH//+3NvSMH0QcSIwsagShDphYdFY38GNggsUCLobBDXaMk4nGpVhwUBcGsWVTZ7WJYfBsoVEXGpMOYgom9mq99zFXvYr1elw1vuP7/O93z/t8Ej7n5sJzY1dkT98Osk9r3elIlVJF2MiVvm68vjZ8utzsc3oZ7jzKdBhhY5zCUYDdIFSDTiCyogUPLPvL7NVLsfVGzh/gubR5jsAtkN1+r6nbhHmBnowPRZ9s1f8GTqbNUQWVAMUIBP0lsrTIxNhQ9L57VQfPP86dQUgvAYi0CHXlFmw1ePnCsWfORQ2cXMjuUxGdAXm4CdQC8PKnbaDpz0XeaUvFxoZ7P/0Ap8wppXi9qacia2vLHw859q7+/e9BdjXTCvRUfPD4DSaWXu0Mi7EMylGfEBRtqVx07CG2LQKolZ7nEearLPczufjmpArhhUdJ/V2oBVrbOMW5tBknmfSlCG0QxZpG0AlKyE+vtcR5L21OCznj7x4/VMrVmKNpM8IZQA5sA54OBhYprJd0jwPraFcrIA9uCw4WCimIbPTW6pN7swHA8WDJkxbAbvIClVtLYFmtsnIiaIMUStbnWp23R3bn/UJRb5B6SxvitOyRf2pp4K0uc6De0g7svwwh18v5lDkCqtmWx6awrKGveI5NF343ZZ4NEbeDD3pZtQWT4+ejT7eG0XM1OWOUhj1BwQigerxXk85p4UNUVMKJaWNuPMGuqNky1d8ku1ndNP2W6XeLxBJK+3VZxQAAAABJRU5ErkJggg=="></span>
                                <span>{{ title }}</span>
                            </h4>
                        </slot>
                    </div>
                    <div
                        v-if="$slots.default || $slots.body"
                        :class="{
                            'has-header': headerShouldShow,
                            'has-footer': footerShouldShow,
                        }"
                        class="modal-body"
                    >
                        <slot :name="($slots.default) ? 'default' : 'body'" />
                    </div>
                    <div v-if="footerShouldShow" class="modal-footer">
                        <slot name="footer">
                            <button type="button" class="btn btn-green" data-dismiss="modal">{{ closeButtonText }}</button>
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'modal',

        props: {
            title: {
                type: String,
                default() {
                    return '';
                },
            },

            closeButtonText: {
                type: String,
                default() {
                    return '';
                },
            },

            image: {
                type: String,
                default() {
                    return '';
                },
            },

            showCloseButton: {
                type: Boolean,
                default() {
                    return false;
                },
            },

            showTitleIcon: {
                type: Boolean,
                default() {
                    return false;
                },
            },


            // If you need to prevent the modal closing when you click off it:
            // Set this to 'static'
            showModalBackdrop: {
                type: String,
                default() {
                    return '';
                }
            },

            animate: {
                type: Boolean,
                default() {
                    return true;

                },
            },
        },

        computed: {
            headerShouldShow() {
                return this.$slots.header || this.title || this.showCloseButton;
            },

            footerShouldShow() {
                return this.$slots.footer || this.closeButtonText;
            },
        },

        mounted() {
            if (this.$root.viewport === 'xs') {
                $(this.$el).on('show.bs.modal', () => {
                    $('header').css({
                        zIndex: 2,
                    });
                });
                $(this.$el).on('hidden.bs.modal', () => {
                    $('header').css({
                        zIndex: '',
                    });
                });
            }
        },
    }
</script>

<style lang="less">
    @import (reference) "~assets/less/variables.less";

    .modal-body {
        p,
        li {
            margin-bottom: 1em;
            @media (min-width: @screen-sm-min) {
                margin-bottom: 1.5em;
            }
        }
    }
</style>

<style scoped lang="less">
    @import (reference) "~assets/less/variables.less";
    @import (reference) "~assets/less/components/mixins/helpers.less";
    @import (reference) "~bootstrap/less/mixins/clearfix.less";
    @import (reference) "~bootstrap/less/responsive-utilities.less";
    @import (reference) "~bootstrap/less/mixins/responsive-visibility.less";

    @modal-padding-vertical: 30px;
    @modal-padding-horizontal: 45px;

    @modal-padding-vertical-mobile: 30px;
    @modal-padding-horizontal-mobile: 40px;

    .modal-body {
        font-size: 16px;
        font-weight: 300;
    }
    .modal-header {
        border-bottom: none;
    }
    .modal {
        &.has-image {
            .modal-inner {
                @media (min-width: @screen-sm-min) {
                    padding-left: 40%;
                }
            }
        }
        .image {
            .bg-cover;
            border-radius: 7px 0 0 7px;
            padding-bottom: 0;
            position: absolute;
            top: 0;
            left: 0;
            bottom: 0;
            width: 40%;
            .hidden-xs;
        }
        .modal-dialog {
            @media (min-width: @screen-sm-min) {
                width: 1045px;
                max-width: 90%;
            }
            .modal-content {
                border-radius: 4px;
                .modal-header {
                    background: transparent;
                    padding: @modal-padding-vertical-mobile @modal-padding-horizontal-mobile (@modal-padding-vertical-mobile / 2);
                    @media (min-width: @screen-sm-min) {
                        padding: @modal-padding-vertical @modal-padding-horizontal (@modal-padding-vertical / 2);
                    }
                    button {
                        background: transparent url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAPCAYAAAA71pVKAAAAqElEQVQ4T63TvQ4BURCG4WcL8dNouQEqiUSJC3DHenQ6HRdAqUKikUmsbE7WbsKe9uR95zszZzJ0MMYJN/WnixGOGaaY4IINHhV8FFpgiEPAPcwxqBEUwSi0CzhOG8u3sSxB6X0OVwm+iotwCNJoe8y+JUrhVPBEC2ds02aWwSHoY/UGQ7DGNZ1Co5Xr3hzR73mCRrr985z/+mF//e18q47FZlQsx2erXu55RZYWlA5BAAAAAElFTkSuQmCC) no-repeat center;
                        width: 23px;
                        height: 23px;
                        transition: 0.15s;
                        top: 30px;
                        right: 25px;
                        padding: 0;
                        z-index: 5;
                        &:after {
                            content: '';
                        }
                        &:hover {
                            opacity: 0.5;
                        }
                        span {
                            display: none;
                        }
                    }
                    h4 {
                        font-family: @headings-font-family;
                        font-size: 23px;
                        font-weight: 700;
                        padding-right: 15px;
                        &.has-title-icon {
                            display: table;
                            width: 100%;
                            > * {
                                display: table-cell;
                                vertical-align: top;
                                &:nth-child(1) {
                                    padding-right: 10px;
                                }
                            }
                        }
                    }
                }
                .modal-body {
                    padding: @modal-padding-vertical-mobile @modal-padding-horizontal-mobile;
                    @media (min-width: @screen-sm-min) {
                        padding: @modal-padding-vertical @modal-padding-horizontal;

                    }
                    &.has-header {
                        padding-top: 0;
                    }
                    &.has-footer {
                        padding-bottom: 0;
                    }
                }
                .modal-footer {
                    border: 0;
                    padding: (@modal-padding-vertical-mobile / 2) @modal-padding-horizontal-mobile @modal-padding-vertical-mobile;
                    text-align: left;
                    @media (min-width: @screen-sm-min) {
                        padding: (@modal-padding-vertical / 2) @modal-padding-horizontal @modal-padding-vertical;
                    }
                    .btn {
                        font-family: @headings-font-family;
                        font-size: 14px;
                        font-weight: 700;
                        padding: 7px 30px;
                        border-radius: 2px;
                    }
                }
            }
        }
    }
</style>
