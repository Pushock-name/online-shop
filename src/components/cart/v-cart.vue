<template lang="">
    <div class="v-cart">
        <router-link :to="{name: 'catalog'}">
            <div class="v-catalog__link_to_cart btn">Back to Catalog</div>
        </router-link>
        <h1>Cart</h1>
        <p v-if="!cart_data.length">There are no products in cart...</p>
        <v-cart-item 
            v-for="(item, index) in cart_data"
            :key="item.article"
            :cart_item_data="item"
            @deleteFromCart="deleteFromCart(index)"
            @decrementItem="decrementItem(index)"
            @incrementItem="incrementItem(index)"
        />
        <div class="v_cart__total">
            <p class="total__name">Total:</p>
            <p>{{cartTotalCost}} р.</p>
        </div>
        <div class="v-cart__total__block"> 
        </div>
    </div>
</template>
<script>
import vCartItem from '../cart/v-cart-item.vue';
import { mapActions } from 'vuex';

export default {
    name: 'v-cart',
    components: {
        vCartItem
    },
    props: {
        cart_data: {
            type: Array,
            default() {
                return []
            }
        }
    },
    computed: {
        cartTotalCost() {
            let result = []

            if (this.cart_data.length) {

            for (let item of this.cart_data) {
                result.push(item.price * item.quantity)
            }

            result = result.reduce(function (sum, el) {
                return sum + el;
            })
            return result;
            } else {
                return 0
            }
        }
    },
    methods: {
        decrementItem(index) {
            this.DECREMENT_CART_ITEM(index)
        },
        incrementItem(index) {
            this.INCREMENT_CART_ITEM(index)
        },
        deleteFromCart(index) {
            this.DELETE_FROM_CART(index)
        },
        ...mapActions([
            'DELETE_FROM_CART',
            'DECREMENT_CART_ITEM',
            'INCREMENT_CART_ITEM'
        ])
    }
}
</script>
<style lang="scss">
    .v_cart {    
        &__total {
            position: fixed;
            bottom: 0px;
            right: 0px;
            left: 0px;
            padding: $padding*2 $padding*3;
            display: flex;
            justify-content: center;
            background-color: #03bb85;
            color: white;
            font-size: 20px;
                .total__name {
                margin-right: $margin*2;
                }
        }
    }

    .v-cart__total__block {
            height: 96px;
    }

</style>