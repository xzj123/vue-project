<template>
    <modal :show.sync="show" :day="day">
        <div slot="modal-dialog" class="modal-dialog" ref="dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" aria-label="Close" @click="cancel">
                        <span aria-hidden="true">&times;</span>
                    </button>
                    <h4 class="modal-title">{{ $t('generic.add_event')}}</h4>
                </div>
                <div class="modal-body">
                    <form>
                        <div class="form-group">
                            <label for="event-title">{{ $t('generic.event_title')}}:</label>
                            <input type="text" class="form-control" id="event-title" v-model="eventTitle"/>
                        </div>
                        <div class="form-group">
                            <label for="event-color">{{ $t('generic.event_color') }}:</label>
                            <select v-model="eventColor" class="form-control" id="event-color">
                                <option v-for="color in colors"
                                        :value="color.value">
                                    {{color.text}}
                                </option>
                            </select>
                        </div>
                        <div class="form-group">
                            <label for="event-desc">{{ $t('generic.event_description')}}:</label>
                            <textarea v-model="eventDesc" class="form-control" id="event-desc"></textarea>
                        </div>
                    </form>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-danger" @click="cancel">{{ $t('generic.cancel')}}</button>
                    <button type="button" class="btn btn-primary" @click="saveEvent">{{ $t('generic.add_event')}}</button>
                </div>
            </div>
        </div>
    </modal>
</template>

<script>
    import {EVENT_ADDED, SHOW_ADD_EVENT_FORM, CANCEL_ADD_EVENT_FORM} from '../actions';

    export default {
        data () {
            return {
                eventTitle: "",
                eventColor: "",
                eventDesc: "",
                colors: [
                    { text: this.$t('color.grey'), value: 'panel-default' },
                    { text: this.$t('color.blue'), value: 'panel-primary' },
                    { text: this.$t('color.lightblue'), value: 'panel-info' },
                    { text: this.$t('color.green'), value: 'panel-success' },
                    { text: this.$t('color.orange'), value: 'panel-warning' },
                    { text: this.$t('color.red'), value: 'panel-danger' },
                ]
            };
        },
        components: {
            'Modal': require('./parts/Modal.vue')
        },
        props: {
            day: {
                type: Object,
            },
            show: {
                type: Boolean,
                default: false
            },
        },
        methods: {
            saveEvent () {
                this.$root.$emit(EVENT_ADDED, {
                    title: this.eventTitle,
                    color: this.eventColor,
                    description: this.eventDesc,
                    date: this.day.date._d
                });

                this.eventTitle = "";
                this.eventDesc = "";
                this.$emit(SHOW_ADD_EVENT_FORM, false);
            },
            cancel () {
                this.eventTitle = "";
                this.eventDesc = "";
                this.$emit(CANCEL_ADD_EVENT_FORM);
                this.$emit(SHOW_ADD_EVENT_FORM, false)
            },
            clickBackdrop () {
                this.cancel();
            }
        }
    };
</script>