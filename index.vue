<template>
    <div class="organization-chart">
        <OrgChartFixedCard command-label="발주처" detail-label="SK Specialty" />
        <div class="line" style="width: 1px; height: 32px; background-color: #ccc"></div>
        <OrgChartFixedCard command-label="안전" :detail-label="children[0].position" :button="true" @create-card="create" @remove-card="remove" />
        <!-- 최상위 Node -->
        <OrgChartParentNode ref="parentNodeRef" />
    </div>
</template>

<script>
import OrgChartFixedCard from "@components/OrgChartFixedCard.vue"
import OrgChartParentNode from "@components/OrgChartParentNode.vue"

export default {
    components: { OrgChartFixedCard, OrgChartParentNode },
    data() {
        return {
            isActive: false,
            orgChartData: [],
            parentLength: 0,
            rowData: [],
            childLength: 0,
            children: [
                {
                    node_id: 1,
                    depth: 2, // 열
                    order: 1, // 행
                    position: "안전보건조정자",
                    name: "GSIL",
                    parent_node_id: 1,
                    child_nodes: [],
                },
            ],
        }
    },
    methods: {
        create() {
            this.$refs.parentNodeRef.create()
        },
        remove() {
            this.$refs.parentNodeRef.remove()
        },
    },
}
</script>

<style lang="scss" scoped>
.organization-chart {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;

    height: 100vh;

    /* Hide scrollbar for IE, Edge and Firefox */
    -ms-overflow-style: none; /* IE and Edge */
    scrollbar-width: none; /* Firefox */

    /* Hide scrollbar for Chrome, Safari and Opera */
    &::-webkit-scrollbar {
        display: none;
    }
}
</style>
