<template>
    <div> 
        <div class="row">
            <div v-for="t in tables" :key="t.index" class="col-12">
                 <table class="table table-sm table-dark">
                    <thead>
                        <tr>
                        <th v-for="h in t" :key="h.name" scope="col"> {{ h.name }}</th>  
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                        <td v-for="v in t" :key="v.name">{{ v.value }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Tables',
    data (){
        return {
            tableCount: 1,
            tables: []
        }
    },
    props: {
        col: Number,
        hours: Array
    },
    created() {
        /*define table count for responsability*/
        this.tableCount = Math.ceil(this.hours.length/this.col)
       
         for  (let c = 0; c < this.tableCount; c++) {
                 let h_table = []
                for (let i = c*this.col+1; i <= Math.min(this.col*(c+1),this.hours.length); i++) {
                    h_table.push(this.hours[i-1])
                }
                this.tables.push(h_table) 
            }
        console.log(this.tables)
    },
}
</script>