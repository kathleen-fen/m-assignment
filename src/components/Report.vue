<template>
    <div class="report">
        <div class="header" :class="className">
            <div class="row" >
                <div class="col-md-3">Machine:</div>
                <div class="col-md-3">{{ machine.MACHINE }}</div>
            </div>
            <div class="row">
                <div class="col-md-3">Production:</div>
                <div class="col-md-3">{{ machine.PRODUCTION }}</div>
            </div>
            <div class="row">
                <div class="col-md-3">Scrap percentage:</div>
                <div class="col-md-3">{{ machine.SCRAP_PERCENTAGE }}</div>
            </div>
            <div class="row">
                <div class="col-md-3">Downtime percentage:</div>
                <div class="col-md-3">{{ machine.DOWNTIME_PERCENTAGE }}</div>
            </div>
            <div class="row">
                <div class="col-md-3">Availability:</div>
                <div class="col-md-3">{{ ooe.AVAILABILITY }}</div>
            </div>
            <div class="row">
                <div class="col-md-3">OEE:</div>
                <div class="col-md-3">{{ ooe.OEE }}</div>
            </div>
            <div class="row">
                <div class="col-md-3">Performance:</div>
                <div class="col-md-3">{{ ooe.PERFORMANCE }}</div>
            </div>
            <div class="row">
                <div class="col-md-3">Quality:</div>
                <div class="col-md-3">{{ ooe.QUALITY }}</div>
            </div>
            <div class="row">
                <div class="col-md-3">Date:</div>
                <div class="col-md-3">{{ new Date(machine.DATETIME_FROM).toLocaleDateString() }}</div>
            </div>
        
        </div>
        
        <table class="table table-sm table-dark">
        <thead>
            <tr>
            <th v-for="h in this.hours" :key="h.name" scope="col"> {{ h.name }}</th>  
            </tr>
        </thead>
        <tbody>
            <tr>
            <td v-for="v in this.hours" :key="v.name">{{ v.value }}</td>
            </tr>
        </tbody>
        </table>
        <div class="row">
            <div class="col-md-12">
                <Graph
                    :hoursKeys="hoursKeys"
                    :hours="hours"
                 />
            </div>
            
        </div>
        <hr>
        
    </div>
</template>
<script>
import Graph from './Graph'
export default {
    name: 'Report',
    data () {
        return {
            hours: [],
            hoursKeys: [],
            hoursValues: [],
            className: 'alert-success'

        }
    },
    components: {
       Graph
    },
    props: {
        machine: {
            type: Object,
            required:true
        },
        color: {
            type: String,
            required: true
        },
        ooe: {
            type: Object,
            required: true
        }
    },
    created () {
        for (let i=0; i<=23; i++) {
            this.hours.push({
                name: `${i+1}h`,
                value: this.machine[`H${i}`],
                perc: 'p'+ Math.round(this.machine[`H${i}`]/50000*100)
            })
            this.hoursKeys.push(`${i+1}h`)
            this.hoursValues.push(this.machine[`H${i}`])
        }
        if (this.color.includes('warning'))
            this.className = 'alert-warning'
        if (this.color.includes('fatal'))
            this.className = 'alert-danger' 
        if (this.color.includes('good'))
            this.className = 'alert-success'       
        console.log(this.hours)

        
    }

}
</script>
<style lang="scss" scoped>
.report {
    margin: 20px 0;
}
table {
    margin-top: 10px;
}
.warning {
    background-color: #fff3cd;
}
.fatal {
     background-color: #f8d7da;
}
.good {
     background-color: green;
}
</style>