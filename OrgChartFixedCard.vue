<template>
    <div class="card">
        <div class="card__command">
            <span class="card__command__label">{{ commandLabel }}</span>
        </div>
        <div class="card__detail">
            <span class="card__detail__label">{{ detailLabel }}</span>
        </div>
        <div v-if="button" class="card__button-box">
            <button @click="create" class="card__button-box__button">
                <el-icon><Plus /></el-icon>
            </button>
            <button @click="remove" class="card__button-box__button">
                <el-icon><Minus /></el-icon>
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        commandLabel: {
            type: String,
            required: false,
        },
        detailLabel: {
            type: String,
            required: false,
        },
        button: {
            type: Boolean,
            required: false,
        },
    },
    date() {
        return {}
    },
    methods: {
        // 새로운 조직도 카드 생성 함수
        create() {
            this.$emit("create-card", true)
        },
        remove() {
            this.$emit("remove-card", true)
        },
    },
}
</script>

<style lang="scss" scoped>
.card {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    gap: 4px;

    cursor: pointer;

    &__command {
        display: flex;
        align-items: center;
        justify-content: center;

        width: 180px;
        height: 32px;

        border-radius: 4px;
        background-color: #fef0e8;

        &__label {
            font-weight: 700;
        }
    }
    &__detail {
        display: flex;
        align-items: center;
        justify-content: center;

        width: 180px;
        height: 32px;
        border-radius: 4px;
        background-color: #f1f1f3;

        &__label {
            color: rgb(80, 80, 80);
        }
    }
    &__button-box {
        display: flex;
        align-items: center;
        justify-content: center;

        gap: 4px;

        cursor: pointer;

        &__button {
            display: flex;
            align-items: center;
            justify-content: center;

            width: 28px;
            height: 28px;

            margin: 2px 0;
        }
    }
}
</style>
