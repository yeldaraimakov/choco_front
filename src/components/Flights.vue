<template>
    <div class="container">
        <div class="filters">
            <ejs-multiselect id='multiselect' :dataSource='companies' placeholder="Авиакомпании"
                             mode="CheckBox" :fields='companyFields' :showSelectAll='showSelectAll'
                             selectAllText="Все" unSelectAllText="Все" v-model="selectedCompanies" style="color: #ff0000;">
            </ejs-multiselect>
        </div>
        <b-form-checkbox id="onlyDirect" name="onlyDirect" v-model="onlyDirect" class="only-direct">
            Только прямые рейсы
        </b-form-checkbox>
        <b-button v-on:click="filter" class="btn btn-search">Найти</b-button>
        <flight-table :flights="filteredFlights"></flight-table>
    </div>
</template>

<script>
    import flights from '../data/data.json';
    import flightTable from './FlightTable.vue';

    export default {
        name: "Flights",
        data: () => {
            return {
                filteredFlights: flights,
                onlyDirect: false,
                companies: [
                    { Company: 'Air Astana' },
                    { Company: 'Bek Air' },
                    { Company: 'Scat' }
                ],
                companyFields : { text: 'Company', value: 'Company' },
                showSelectAll : true,
                selectedCompanies: []
            }
        },
        mounted() {
            this.selectedCompanies = this.companies.map(company => company.Company);
            console.log(this.selectedCompanies);
        },
        components: {flightTable},
        methods: {
            filter() {
                this.filteredFlights = flights;
                if (this.onlyDirect) {
                    this.filteredFlights = this.filteredFlights.filter(flight => {
                        return flight.flights.length === 1
                    })
                }
                this.filteredFlights = this.filteredFlights.filter(flight => {
                    for (var i = 0; i < flight.flights.length; i++) {
                        if (this.selectedCompanies.indexOf(flight.flights[i].airline.name) > -1 ) {
                            return true;
                        }
                    }
                    return false;
                })
            }
        }
    }
</script>

<style scoped>
    @import "../../node_modules/@syncfusion/ej2-base/styles/material.css";
    @import "../../node_modules/@syncfusion/ej2-inputs/styles/material.css";
    @import "../../node_modules/@syncfusion/ej2-vue-dropdowns/styles/material.css";
    @import "../../node_modules/@syncfusion/ej2-buttons/styles/material.css";

    .filters {
        width: 200px;
        float: left;
        margin-right: 50px;
    }

    .only-direct {
        float: left;
        margin-right: 20px;
    }

    .btn.btn-search {
        background: #FEA53C;
        border-color: unset;
        border-radius: 0px;
    }
    .btn.btn-search:active {
        background: #FEA53C !important;
        border-color: unset !important;
    }
    .btn.btn-search:focus {
        background: #FEA53C !important;
        border-color: unset !important;
    }

</style>

