<script setup lang="ts">
import { ref } from 'vue'

useHead({
    title: 'Каталог'
})

interface Product {
    id: number
    name: string
    price: number
    image: string
}

interface ApiResponse {
    total: number
    totalPages: number
    currentPage: number
    limit: number
    products: Product[]
}

const products = ref<Product[]>([])
const currentPage = ref(1)
const totalPages = ref(1)
const isLoading = ref(false)
const hasError = ref(false)
const limit = 12

const fetchProducts = async (page: number): Promise<ApiResponse | null> => {
    try {
        if (import.meta.server) {
            const response = await $fetch<ApiResponse>(`https://test-task-api.tapir.ws/products?page=${page}&limit=${limit}`)
            return response
        } else {
            const response = await fetch(`https://test-task-api.tapir.ws/products?page=${page}&limit=${limit}`)
            if (!response.ok) throw new Error('Network error')
            return await response.json()
        }
    } catch (error) {
        console.error('Error fetching products:', error)
        return null
    }
}

const { data: initialData } = await useAsyncData<ApiResponse | null>('products-page-1',
    () => fetchProducts(1)
)

if (initialData.value) {
    products.value = initialData.value.products
    totalPages.value = initialData.value.totalPages
    currentPage.value = initialData.value.currentPage
}

const loadMore = async () => {
    if (currentPage.value >= totalPages.value) return

    isLoading.value = true
    hasError.value = false

    const nextPage = currentPage.value + 1
    const data = await fetchProducts(nextPage)

    isLoading.value = false

    if (data) {
        products.value = [...products.value, ...data.products]
        currentPage.value = data.currentPage
        totalPages.value = data.totalPages
    } else {
        hasError.value = true
    }
}

const formatPrice = (price: number): string => {
    return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ') + ' ₽'
}

const calculateHalfPrice = (price: number): number => {
    return Math.ceil(price / 2)
}
</script>

<template>
    <div class="catalog-page">
        <div class="catalog-page__container">
            <h1 class="catalog-page__title">КАТАЛОГ</h1>
            <div class="catalog-page__grid">
                <a v-for="product in products" :key="product.id" href="#" class="product-card">
                    <div class="product-card__image-wrapper">
                        <img :src="product.image" :alt="product.name" class="product-card__image" loading="lazy">

                        <button class="product-card__favorite" aria-label="Добавить в избранное">
                            <svg class="product-card__favorite-icon" width="20" height="19" viewBox="0 0 20 19"
                                fill="none" xmlns="http://www.w3.org/2000/svg">
                                <path
                                    d="M10 18.35L8.55 17.03C6.86667 15.5133 5.475 14.205 4.375 13.105C3.275 12.005 2.4 11.0175 1.75 10.1425C1.1 9.2675 0.645833 8.46333 0.3875 7.73C0.129167 6.99667 0 6.24667 0 5.48C0 3.91333 0.525 2.605 1.575 1.555C2.625 0.505 3.93333 -0.02 5.5 -0.02C6.36667 -0.02 7.19167 0.163333 7.975 0.53C8.75833 0.896667 9.43333 1.41333 10 2.08C10.5667 1.41333 11.2417 0.896667 12.025 0.53C12.8083 0.163333 13.6333 -0.02 14.5 -0.02C16.0667 -0.02 17.375 0.505 18.425 1.555C19.475 2.605 20 3.91333 20 5.48C20 6.24667 19.8708 6.99667 19.6125 7.73C19.3542 8.46333 18.9 9.2675 18.25 10.1425C17.6 11.0175 16.725 12.005 15.625 13.105C14.525 14.205 13.1333 15.5133 11.45 17.03L10 18.35ZM10 15.63C11.6 14.1967 12.9167 12.9675 13.95 11.9425C14.9833 10.9175 15.8 10.0258 16.4 9.2675C17 8.50917 17.4167 7.83417 17.65 7.2425C17.8833 6.65083 18 6.06333 18 5.48C18 4.48 17.6667 3.64667 17 3C16.3333 2.35333 15.5 2.02 14.5 2.02C13.7167 2.02 12.9917 2.24083 12.325 2.6825C11.6583 3.12417 11.2 3.68667 10.95 4.37H9.05C8.8 3.68667 8.34167 3.12417 7.675 2.6825C7.00833 2.24083 6.28333 2.02 5.5 2.02C4.5 2.02 3.66667 2.35333 3 3C2.33333 3.64667 2 4.48 2 5.48C2 6.06333 2.11667 6.65083 2.35 7.2425C2.58333 7.83417 3 8.50917 3.6 9.2675C4.2 10.0258 5.01667 10.9175 6.05 11.9425C7.08333 12.9675 8.4 14.1967 10 15.63Z"
                                    fill="black" />
                            </svg>
                        </button>

                    </div>

                    <div class="product-card__content">
                        <div class="product-card__price-row">
                            <span class="product-card__price">{{ formatPrice(product.price) }}</span>
                            <span class="product-card__half-price">{{ formatPrice(calculateHalfPrice(product.price)) }}
                                x 2</span>
                        </div>
                        <h3 class="product-card__title">{{ product.name }}</h3>
                    </div>
                </a>
            </div>

            <div v-if="currentPage < totalPages" class="catalog-page__actions">
                <button v-if="!isLoading && !hasError" @click="loadMore" class="catalog-page__load-more">
                    Показать ещё
                </button>

                <div v-if="isLoading" class="catalog-page__loading">
                    Загрузка...
                </div>

                <div v-if="hasError" class="catalog-page__error">
                    <p class="catalog-page__error-message">
                        Произошла ошибка, попробуйте позже
                    </p>
                    <button @click="loadMore" class="catalog-page__retry-button">
                        Повторить
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.catalog-page {
    flex: 1;
    padding: $spacing-xxxxl 0 $spacing-xl 0;
    background-color: $color-background;

    &__container {
        max-width: $container-width-md;
        margin: 0 auto;
        padding: 0 $spacing-xxxl;

        @media (max-width: $breakpoint-sm) {
            padding: 0 $spacing-lg;
        }
    }

    @media (max-width: $breakpoint-sm) {
        padding: $spacing-lg 0 $spacing-xl 0;
    }

    &__title {
        font-size: $font-size-title;
        font-weight: 500;
        text-align: center;
        color: $color-secondary;
        margin: 0 0 $spacing-xxxxxl 0;

        @media (max-width: $breakpoint-sm) {
            font-size: $font-size-title-sm;
            margin-bottom: $spacing-xl;
        }
    }

    &__grid {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        gap: $spacing-xl;
        margin-bottom: $spacing-xxl;

        @media (max-width: $breakpoint-sm) {
            grid-template-columns: repeat(2, 1fr);
            gap: $spacing-lg;
        }
    }

    &__actions {
        display: flex;
        justify-content: center;
        margin-top: $spacing-xxxxxxl;

        @media (max-width: $breakpoint-sm) {
            margin-top: $spacing-xl;
        }
    }

    &__load-more {
        padding: $spacing-md $spacing-xxl;
        background-color: $color-background;
        border: 1px solid;
        line-height: $spacing-lg;
        font-size: $font-size-sm;
        font-weight: 400;
        cursor: pointer;
    }

    &__loading {
        padding: $spacing-md $spacing-xxl;
        background-color: $color-background;
        font-size: $font-size-sm;
        line-height: $spacing-lg;
        font-weight: 400;
    }

    &__error {
        text-align: center;
    }

    &__error-message {
        margin-bottom: $spacing-lg;
        line-height: $spacing-lg;
        font-size: $font-size-sm;
    }

    &__retry-button {
        padding: $spacing-sm $spacing-xl;
        background-color: $color-background;
        border: 1px solid;
        font-size: $font-size-sm;
        line-height: $spacing-lg;
        font-weight: 400;
        cursor: pointer;
    }
}

.product-card {
    text-decoration: none;
    color: inherit;
    display: block;


    &__image-wrapper {
        position: relative;
        aspect-ratio: 1 / 1;
        overflow: hidden;
        display: flex;
        align-items: center;
        justify-content: center;
    }



    &__image {
        max-width: 100%;
        max-height: 100%;
        width: auto;
        height: auto;
        object-fit: contain;
    }

    &__favorite {
        position: absolute;
        top: $spacing-md;
        right: $spacing-md;
        width: 40px;
        height: 40px;
        background-color: transparent;
        border: none;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        z-index: 2;
    }

    &__favorite-icon {
        width: 20px;
        height: 19px;
    }

    &__content {
        padding: 0 $spacing-xs;
    }

    &__price-row {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        gap: $spacing-sm;
        margin-bottom: $spacing-sm;

        @media (max-width: $breakpoint-sm) {
            flex-direction: column;
            align-items: baseline;
            gap: 0;
        }
    }

    &__price {
        font-size: $font-size-xl;
        line-height: 140%;
        font-style: normal;
        font-weight: 500;
        letter-spacing: 0%;
        color: $color-secondary;
    }

    &__half-price {
        font-size: $font-size-sm;
        color: $color-text-light;
        background-color: $color-secondary;
        line-height: 100%;
        letter-spacing: -5%;
        font-weight: 500;
        padding: $spacing-xs;

    }

    &__title {
        font-size: $font-size-md;
        font-weight: 400;
        color: $color-secondary;
        line-height: 140%;

        @media (max-width: $breakpoint-sm) {
            font-size: $font-size-xs;
        }
    }
}
</style>