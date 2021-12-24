<template>
    <div class="map">
        <h3>Карта офиса</h3>
        <div
            class="map-root"
        >
            <!-- map -->
            <MapSVG 
                ref="svg"
                v-on="$listeners"
            />
            <TableSVG 
                v-show="false"
                ref="table"
            />
        </div>
    </div>
</template>

<script>
import MapSVG from '@/assets/images/map.svg';
import TableSVG from '@/assets/images/workPlace.svg';
import * as d3 from 'd3';
import tables from '@/assets/data/tables.json';
import legend from '@/assets/data/legend.json';
export default {
    components:{
        MapSVG,
        TableSVG,
    },
    data() {
        return {
            svg: null,
            g: null,
            tables: [],
            tableSvg: null,
        };
    },
    mounted(){
        this.svg = d3.select(this.$refs.svg);
        this.g = this.svg.select('g');
        this.tables = tables;
        this.tableSvg = d3.select(this.$refs.table);

        if (this.g){
            this.drawTables();
        }else{
            console.error('Error');
        }
    },
    methods:{
        drawTables(){
            //создаем группу рабочих мест
            const svgTablesGroup = this.g.append('g').classed('groupPlaces',true);
            this.tables.forEach( (table) => {
                //создать группу рабочего стола
                const svgTable = svgTablesGroup.append('g')
                    .attr('transform',`translate(${table.x},${table.y}) scale(.5)`)
                    .attr('id', table._id)
                    .classed('employer-place', true);

                svgTable
                    .append('g')
                    .attr('transform', `rotate(${table.rotate || 0})`)
                    .attr('group_id', table.group_id)
                    .classed('even-table', true)
                    .html(this.tableSvg.html())
                    .attr(
                        'fill', legend.find( it => it.group_id === table.group_id)?.color ?? 'transparent'
                    );
            });
        },
    }
};
</script>

<style scoped>
.map {
    height: 100%;
    width: 100%;
    padding: 24px;
    overflow: hidden;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
}

.map-root {
    height: 100%;
    width: 100%;
    overflow: hidden;
    box-sizing: border-box;
}

h3 {
    margin-top: 0px;
}

::v-deep svg {
    height: 100%;
    width: 100%;
}

::v-deep .table {
    cursor: pointer;
    transition: transform .5s;
}
::v-deep .table:hover{
    transform: scale(1.15);
}
.selected::after{
    content: '';
    display: block;
    width: 10px;
    height: 10px;
    background: bisque;
    border: 1px solid red;
}
</style>
