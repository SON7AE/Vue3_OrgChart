<template>
    <ul class="parent-node">
        <OrgChartCard
            v-for="(item, index) in children[0].child_nodes"
            :key="index"
            command-label="건축담당"
            detail-label="관리감독자"
            detail-name="GSIL MANAGER"
            :button="true"
            class="parent-node__item"
            @create-card="createChild(item, index)"
            @remove-card="removeChild"
        >
            <template #child>
                <ul class="child-node">
                    <OrgChartCard v-for="subItem in item.child_nodes" :key="subItem" :command-label="item.position" detail-label="관리감독자" class="child-node__item"> </OrgChartCard>
                </ul>
            </template>
        </OrgChartCard>
    </ul>
</template>

<script>
import OrgChartCard from "@components/OrgChartCard.vue"

export default {
    name: "OrgChartParentNode",
    components: { OrgChartCard },
    data() {
        return {
            isActive: false,
            rowLength: 0,
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
            // 안전 = node_id: 1 / parent_node_id: 1
            // 클릭했을 때, 새로운 레이아웃 ul 태그 생성 및 li 태그 생성 그리고 라인 그리기
            // 재귀적인 사고를 통한 방법 : 작업을 단순화하고 자기 자신을 호출함
            let item = {
                node_id: 2,
                depth: 3, // 열
                order: 1, // 행
                position: "",
                name: "",
                parent_node_id: 1,
                child_nodes: [],
                check: false,
            }

            // depth 3
            if (this.children[0].child_nodes.length === 0) {
                this.children[0].child_nodes.push(item)
            } else {
                for (let i = 0; i < this.children[0].child_nodes.length; i++) {
                    item.node_id++
                    item.order++
                }
                this.children[0].child_nodes.push(item)
            }
            // 클릭했을 때, 새로운 레이아웃 ul 태그 생성 및 li 태그 생성 그리고 라인 그리기
            // Parent Node 추가되었을 때 width UI 설정
            this.isActive = true
            let length = this.children[0].child_nodes.length

            if (length === 1) {
                this.rowLength = 0
            }
            if (length >= 2) {
                for (let i = 2; i <= length; i++) {
                    this.rowLength = (length - 1) * 204
                }
            }
        },
        remove() {
            this.$confirm("하위 모든 조직도 목록을 삭제하시겠습니까?").then(() => {
                this.isActive = false
                this.children[0].child_nodes = []
            })
        },
        createChild(parent, index) {
            parent.check = true
            // parent를 column 값으로 가져간다.
            // depth 4
            this.children[0].child_nodes.forEach((item) => {
                const idx = 0
                if (item.check === true && item.order === index + 1) {
                    return item.child_nodes.push({
                        node_id: null,
                        depth: item.depth + 1,
                        order: idx + 1,
                        position: "직책 테스트",
                        name: "이름 테스트",
                        parent_node_id: item.node_id,
                        child_nodes: [],
                    })
                }
            })
            const arrLength = this.children[0].child_nodes.map((item) => {
                return (item = item.child_nodes.length)
            })
            const result = arrLength.reduce(function add(sum, currValue) {
                return sum + currValue
            }, 0)
            console.log(result)
        },
    },
}
</script>

<style lang="scss" scoped>
* {
    margin: 0;
    padding: 0;
}
.parent-node {
    padding-top: 20px;
    position: relative;

    transition: all 0.5s;
    -webkit-transition: all 0.5s;
    -moz-transition: all 0.5s;

    &__item {
        float: left;
        text-align: center;
        list-style-type: none;
        position: relative;
        padding: 20px 5px 0 5px;

        transition: all 0.5s;
        -webkit-transition: all 0.5s;
        -moz-transition: all 0.5s;

        /* We will use ::before and ::after to draw the connectors */
        &::before,
        &::after {
            content: "";
            position: absolute;
            top: 0;
            right: 50%;
            border-top: 1px solid #ccc;
            width: 50%;
            height: 20px;
        }
        &::after {
            right: auto;
            left: 50%;
            border-left: 1px solid #ccc;
        }
        /* We need to remove left-right connectors from elements without any siblings */
        &:only-child::after,
        &:only-child::before {
            display: none;
        }
        /*Remove space from the top of single children*/
        &:only-child {
            padding-top: 0;
        }
        /*Remove left connector from first child and right connector from last child*/
        &:first-child::before,
        &:last-child::after {
            border: 0 none;
        }
        /*Adding back the vertical connector to the last nodes*/
        &:last-child::before {
            border-right: 1px solid #ccc;
            border-radius: 0 5px 0 0;
            -webkit-border-radius: 0 5px 0 0;
            -moz-border-radius: 0 5px 0 0;
        }
        &:first-child::after {
            border-radius: 5px 0 0 0;
            -webkit-border-radius: 5px 0 0 0;
            -moz-border-radius: 5px 0 0 0;
        }
    }
}
// .parent-node {
//     display: none;
//     align-items: flex-start;
//     justify-content: center;

//     width: 100%;

//     margin: 4px 0 0 -40px;

//     position: relative;

//     &.active {
//         display: flex;
//         gap: 24px;

//         &::after {
//             content: "";
//             position: absolute;

//             top: -40px;
//             width: 1px;
//             height: 40px;

//             background-color: #ccc;
//         }
//         &::before {
//             content: "";
//             position: absolute;
//             top: 0;
//             // right: 50%;
//             border-top: 1px solid #ccc;
//             width: var(--width); // 가로 길이
//             height: 1px;
//         }
//     }
//     &__item {
//         &.active {
//             margin-top: 8px;

//             &::before {
//                 content: "";
//                 position: absolute;

//                 top: -8px;
//                 width: 1px;
//                 height: 9px;

//                 background-color: #ccc;
//             }
//         }
//     }
// }

.child-node {
    position: relative;

    transition: all 0.5s;
    -webkit-transition: all 0.5s;
    -moz-transition: all 0.5s;

    &__item {
        float: left;
        text-align: center;
        list-style-type: none;
        position: relative;
        padding: 20px 5px 0 5px;

        transition: all 0.5s;
        -webkit-transition: all 0.5s;
        -moz-transition: all 0.5s;

        /* We will use ::before and ::after to draw the connectors */
        &::before,
        &::after {
            content: "";
            position: absolute;
            top: 10px;
            right: 50%;
            border-top: 1px solid #ccc;
            width: 60%;
            height: 10px;
        }
        &::after {
            right: auto;
            left: 50%;
            border-left: 1px solid #ccc;
        }
        /* We need to remove left-right connectors from elements without any siblings */
        &:only-child::after,
        &:only-child::before {
            display: none;
        }
        /*Remove space from the top of single children*/
        &:only-child {
            padding-top: 20px;
        }

        /*Remove left connector from first child and
right connector from last child*/
        &:first-child::before,
        &:last-child::after {
            border: 0 none;
        }
        /*Adding back the vertical connector to the last nodes*/
        &:last-child::before {
            border-right: 1px solid #ccc;
            border-radius: 0 5px 0 0;
            -webkit-border-radius: 0 5px 0 0;
            -moz-border-radius: 0 5px 0 0;
        }
        &:first-child::after {
            border-radius: 5px 0 0 0;
            -webkit-border-radius: 5px 0 0 0;
            -moz-border-radius: 5px 0 0 0;
        }
    }

    /*Time to add downward connectors from parents*/

    // &.active {
    //     display: flex;

    //     gap: 24px;

    //     &::before {
    //         content: "";
    //         position: absolute;
    //         top: -8px;
    //         // right: 50%;
    //         border-top: 1px solid #ccc;
    //         width: var(--width); // 가로 길이
    //         height: 1px;
    //     }
    //     &::after {
    //         content: "";
    //         position: absolute;

    //         top: -24px;
    //         width: 1px;
    //         height: 16px;

    //         background-color: #ccc;
    //     }
    // }
    // &__item {
    //     &.active {
    //         margin-top: 8px;

    //         &::before {
    //             content: "";
    //             position: absolute;

    //             top: -16px;
    //             width: 1px;
    //             height: 16px;

    //             background-color: #ccc;
    //         }
    //     }
    // }
}
// .child-node::before {
//     content: "";
//     position: absolute;
//     top: -5px;
//     left: 50%;
//     border-left: 1px solid #ccc;
//     width: 0;
//     height: 15px;
// }
</style>
