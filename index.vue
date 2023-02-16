<template>
    <div class="organization-chart">
        <OrgChartFixedCard command-label="발주처" detail-label="SK Specialty" />
        <div class="line" style="width: 1px; height: 32px; background-color: #ccc"></div>
        <OrgChartFixedCard command-label="안전" detail-label="안전보건조정자" :button="true" @create-card="create" @remove-card="remove" />
        <!-- 최상위 Node -->
        <ul class="organization-chart__parent" :class="{ active: isActive }" :style="{ '--width': parentLength + 'px' }">
            <OrgChartCard
                v-for="item in parent"
                :key="item.id"
                command-label="건축담당"
                detail-label="관리감독자"
                detail-name="GSIL MANAGER"
                class="organization-chart__parent__item"
                :class="{ active: isActive }"
            />
        </ul>
    </div>
</template>

<script>
import OrgChartFixedCard from "@components/OrgChartFixedCard.vue"
import OrgChartCard from "@components/OrgChartCard.vue"

export default {
    components: { OrgChartFixedCard, OrgChartCard },
    data() {
        return {
            isActive: false,
            parent: [],
            parentLength: 0,
        }
    },
    methods: {
        create() {
            // 클릭했을 때, 새로운 레이아웃 ul 태그 생성 및 li 태그 생성 그리고 라인 그리기
            this.isActive = true
            this.parent.push({ id: 1 })

            // Parent Node 추가되었을 때 width UI 설정
            let length = this.parent.length

            if (length === 1) {
                this.parentLength = length * 180
            }
            if (length >= 2) {
                for (let i = 2; i <= length; i++) {
                    this.parentLength = (length - 1) * 204
                }
            }
        },
        remove() {
            this.$confirm("하위 모든 조직도 목록을 삭제하시겠습니까?").then(() => {
                this.isActive = false
                this.parent = []
            })
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

    width: 100%;
    height: 100vh;

    &__parent {
        display: none;
        align-items: center;
        justify-content: center;

        width: 100%;

        margin: 4px 0 0 -40px;

        position: relative;

        &.active {
            display: flex;
            gap: 24px;

            &::after {
                content: "";
                position: absolute;

                top: -40px;
                width: 1px;
                height: 40px;

                background-color: #ccc;
            }
            &::before {
                content: "";
                position: absolute;
                top: 0;
                // right: 50%;
                border-top: 1px solid #ccc;
                width: var(--width); // 가로 길이
                height: 1px;
            }
        }
        &__item {
            &.active {
                margin-top: 8px;

                &::before {
                    content: "";
                    position: absolute;

                    top: -8px;
                    width: 1px;
                    height: 9px;

                    background-color: #ccc;
                }
            }
        }
    }
}
</style>
