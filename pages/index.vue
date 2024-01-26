<template>
	<MainLayout>
		<div id="IndexPage" class="mt-4 max-w-[1200px] mx-auto px-2">
			<div
				class="grid xl:grid-cols-6 lg:grid-cols-5 md:grid-cols-4 sm:grid-cols-3 grid-cols-2 gap-4"
			>
				<div v-if="products" v-for="product in products" :key="product">
					<ProductComponent :product="product" />
				</div>
			</div>
		</div>
	</MainLayout>
</template>

<script setup>
	import MainLayout from '~/layouts/MainLayout.vue';
	import ProductComponent from '~/components/ProductComponent.vue';

	const products = [
		{
			id: 1,
			title: 'Título 1',
			description: 'Essa é uma descrição',
			url: 'https://picsum.photos/id/7/800/800',
			price: 9999,
		},
		{
			id: 2,
			title: 'Título 2',
			description: 'Essa é uma descrição',
			url: 'https://picsum.photos/id/7/800/800',
			price: 9699,
		},
		{
			id: 3,
			title: 'Título 3',
			description: 'Essa é uma descrição',
			url: 'https://picsum.photos/id/7/800/800',
			price: 9999,
		},
		{
			id: 4,
			title: 'Título 4',
			description: 'Essa é uma descrição',
			url: 'https://picsum.photos/id/7/800/800',
			price: 29999,
		},
		{
			id: 5,
			title: 'Título 5',
			description: 'Essa é uma descrição',
			url: 'https://picsum.photos/id/7/800/800',
			price: 49999,
		},
		{
			id: 6,
			title: 'Título 6',
			description: 'Essa é uma descrição',
			url: 'https://picsum.photos/id/7/800/800',
			price: 94999,
		},
		{
			id: 7,
			title: 'Título 7',
			description: 'Essa é uma descrição',
			url: 'https://picsum.photos/id/7/800/800',
			price: 39999,
		},
		{
			id: 8,
			title: 'Título 8',
			description: 'Essa é uma descrição',
			url: 'https://picsum.photos/id/7/800/800',
			price: 93999,
		},
		{
			id: 9,
			title: 'Título 9',
			description: 'Essa é uma descrição',
			url: 'https://picsum.photos/id/7/800/800',
			price: 99599,
		},
		{
			id: 10,
			title: 'Título 10',
			description: 'Essa é uma descrição',
			url: 'https://picsum.photos/id/7/800/800',
			price: 69999,
		},
	];
</script>
