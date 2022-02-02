<template>
    <div class="item">
        <div class="item--quantity">
            <button class="button" @click="decreaseQuantity(item.id)" :disabled="item.quantity <= 0">-</button>
            <span class="number">{{item.quantity}}</span>
            <button class="button" @click="increaseQuantity(item.id)">+</button>
        </div>
        <div class="item--img-container">
            <img class="item--img" :src="imagePath">
        </div>
        <div class="content">
            <h3 class="item--name">{{item.name}}</h3>
            <p class="item--observation">Adicionar observação</p>
        </div>
        <p class="item--price">{{item.price | currency}}</p>
    </div>
</template>

<script>
import { mapActions } from 'vuex'
export default {
    name: 'CartItem',
    props: {
        item: {}
    },
    filters: {
        currency(value) {
            return `R$ ${value.toLocaleString('pt-br', {minimumFractionDigits: 2})}`
        }
    },
    computed: {
        selectedCategory() {
            return this.$store.state.selectedCategory;
        },
        imagePath() {
            return require(`../assets/images/${this.selectedCategory}/${this.item.id}.png`)
        }
    },
    methods: {
        ...mapActions({
            increaseQuantity: 'increaseQuantity',
            decreaseQuantity: 'decreaseQuantity'
        })
    }
}
</script>

<style lang="less" scoped>
.item {
    display: flex;
    padding: 20px 0;
    border-bottom: 1px solid @light-grey;

    &--quantity {
        display: flex;
        align-items: center;
        padding-right: 40px;

        .number {
            font-weight: 500;
            font-size: 18px;
            color: @yellow;
            width: 28px;
            text-align: center;
        }

        .button {
            font-weight: 600;
            font-size: 18px;
            cursor: pointer;
            background: none;
            border: none;

            &:focus {
                outline: 0;
            }
        }
    }

    &--img-container {
        border-radius: 8px;
        background: @light-yellow;
        width: 86px;
        height: 81px;
        display: flex;
        align-items: center;
    }

    &--img {
        width: 65px;
        display: block;
        margin: auto;
    }

    &--name {
        font-weight: 600;
        font-size: 18px;
        margin: 0;
    }

    &--observation {
        font-weight: 500;
        font-size: 12px;
        color: @dark-grey;
        text-decoration: underline;
    }

    .content {
        flex-grow: 1;
        padding: 0 20px;
    }

    &--price {
        font-weight: 600;
        font-size: 18px;
        line-height: 27px;
        color: @yellow;
    }

    @media @tablets {

        flex-wrap: wrap;

        &--img-container {
            order: 1;
        }

        .content {
            order: 2;
        }

        &--quantity {
            order: 3;
            padding: 0;
            width: 81px;
            justify-content: center;
        }

        &--price {
            order: 4;
            padding: 0;
            margin: 5px 0;
        }
    }
}
</style>