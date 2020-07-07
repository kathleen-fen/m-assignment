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
        <Tables 
        class="extra-large"
        :col="24"
        :hours="hours"
        />
        <Tables 
        class="large"
        :col="12"
        :hours="hours"
        />
         <Tables
         class="medium" 
        :col="8"
        :hours="hours"
        />
        <Tables
         class="small" 
        :col="5"
        :hours="hours"
        />
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
import Tables from './Tables'

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
       Graph,
       Tables
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
    //    console.log(this.hours)

        
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

//media requests
.extra-large {
    display: none;
}
.large {
    display: none;
}
.medium {
    display: none;
}
.small {
    display: block;
}

@media(min-width: 460px)  {
    .extra-large {
        display: none;
    }
    .large {
        display: none;
    }
    .medium {
        display: block;
    }
    .small {
        display: none;
    }
}

@media(min-width: 660px)  {
    .extra-large {
        display: none;
    }
    .large {
        display: block;
    }
    .medium {
        display: none;
    }
    .small {
        display: none;
    }
}

@media(min-width: 1300px)  {
    .extra-large {
        display: block;
    }
    .large {
        display: none;
    }
    .medium {
        display: none;
    }
    .small {
        display: none;
    }
}


</style>