<template>
    <div>
        <b-table class="flight-table" small :items="flights" :fields="fields" fixed>
            <template slot="companyLogos" slot-scope="data">
                <b-img v-if="hasCompany(data.item.flights, 'Air Astana')" src="/images/airastana-logo.png" fluid-grow alt="AirAstana Logo"></b-img>
                <b-img v-if="hasCompany(data.item.flights, 'Scat')" src="/images/scat-logo.png" fluid-grow alt="Scat Logo"></b-img>
            </template>
            <template slot="type" slot-scope="data">
                <span v-if="data.item.flights.length === 1">прямой</span>
                <span v-if="data.item.flights.length === 2">через {{data.item.flights[0].cities.arrivalCity}}</span>
                <span v-if="data.item.flights.length > 2">пересадки ({{data.item.flights.length-1}})</span>
            </template>
            <template slot="timeChoose">
                <div>
                    <b-link href="#" class="time-link">Выбрать другое время</b-link>
                </div>
                <b-link href="#" class="time-link">Поделиться</b-link>
            </template>
            <template slot="time" slot-scope="data">
                <span class="departure-time">{{data.item.flights[0].departureTime}} - </span>{{data.item.flights[data.item.flights.length-1].arrivalTime}}
            </template>
            <template slot="flightDuration" slot-scope="data">
                {{flightDuration(data.item.flightDuration)}}
            </template>
            <template slot="buy" slot-scope="data">
                <button class="btn btn-buy">Купить за {{data.item.price}} kzt</button>
            </template>
        </b-table>
    </div>
</template>
<script>
    import Vue from 'vue';
    import { MultiSelectPlugin } from "@syncfusion/ej2-vue-dropdowns";
    import { MultiSelect, CheckBoxSelection } from '@syncfusion/ej2-dropdowns';
    MultiSelect.Inject(CheckBoxSelection);
    Vue.use(MultiSelectPlugin);
    export default {
        name: 'flights-table',
        props: {
            flights: {
                type: [Array, Function],
                default: () => []
            }
        },
        data: () => {
            return {
                fields: [
                    {"key": "companyLogos", "label": "Авиакомпания"}, {"key": "type", "label": "Рейс"},
                    {"key": "timeChoose", "label": "Выбор времени"}, {"key": "time", "label": "Вылет - посадка"},
                    {"key": "flightDuration", "label": "Время в пути"}, {"key": "buy", "label": ""}
                ]
            }
        },
        methods: {
            flightDuration(minute) {
                let result = '';
                if (minute >= 24*60) {
                    result += Math.floor(minute/24/60) + 'д ';
                    minute %= 24*60;
                }
                if (minute >= 60) {
                    result += Math.floor(minute/60) + 'ч ';
                    minute %= 60;
                }
                return result + minute + 'м';
            },
            hasCompany(flights, company) {
                for (let i = 0; i < flights.length; i++) {
                    if (flights[i].airline.name === company) {
                        return true
                    }
                }
                return false;
            }
        },
    }
</script>
<style>
    .flight-table table {
        color: #212C5B !important;
    }
    .flight-table thead {
        background: #397FBF;
        color: #FFFFFF;
    }
    .flight-table thead th {
        padding: 5px 10px;
        font-weight: normal;
        font-size: 13px;
    }
    .flight-table thead th:first-child {
        padding-left: 20px;
    }
    .flight-table tbody td {
        background: #FFFFFF;
        padding: 15px 10px;
        font-size: 12px;
    }
    .flight-table tbody tr td:first-child {
        padding-left: 20px;
        padding-right: 20px;
    }
    .flight-table tbody tr td:last-child {
        padding-right: 20px;
    }
    .btn.btn-buy {
        font-size: 14px;
        background: #FEA53C;
        color: #FFFFFF;
    }
    .btn.btn-buy:hover {
        background: #FE9922;
        color: #FFFFFF;
    }
    .departure-time {
        font-weight: bold;
    }
    .time-link {
        text-decoration: underline dotted;
    }

</style>
