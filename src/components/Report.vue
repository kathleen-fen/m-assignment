<template>
    <div class="report">
        <div class="row">
            <div class="col-md-3">Machine:</div>
            <div class="col-md-9">{{ machine.MACHINE }}</div>
        </div>
        <div class="row">
            <div class="col-md-3">Production:</div>
            <div class="col-md-9">{{ machine.PRODUCTION }}</div>
        </div>
        <div class="row">
            <div class="col-md-3">Scrap percentage:</div>
            <div class="col-md-9">{{ machine.SCRAP_PERCENTAGE }}</div>
        </div>
         <div class="row">
            <div class="col-md-3">Downtime percentage:</div>
            <div class="col-md-9">{{ machine.DOWNTIME_PERCENTAGE }}</div>
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
        
    </div>
</template>
<script>

export default {
    name: 'Report',
    data () {
        return {
            hours: [],
            hoursKeys: [],
            hoursValues: []

        }
    },
    components: {
       
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
                value: this.machine[`H${i}`]
            })
            this.hoursKeys.push(`${i+1}h`)
            this.hoursValues.push(this.machine[`H${i}`])
        }
        console.log(this.hours)

        
    }

}
</script>
<style lang="sass" scoped>

</style>