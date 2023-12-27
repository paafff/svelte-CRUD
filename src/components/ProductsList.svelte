<script lang="ts">
	import axios from 'axios';
	import { onMount } from 'svelte';

	interface Product {
		id: number;
		name: string;
		price: number;
		imageName: string;
		imageURL: string;
		uuid: string;
	}

	let dataProducts: Product[] = [];

	const getProducts = async () => {
		try {
			// <Product[]> setelah pemanggilan axios.get adalah salah satu cara untuk memberi tahu TypeScript tentang jenis data yang diharapkan yang akan diterima dari respons API.
			const response = await axios.get<Product[]>(`http://localhost:5000/products`);

			dataProducts = response.data;
		} catch (error: any) {
			if (error.response) {
				alert(error.response.data.msg);
			} else {
				console.log(error);
			}
		}
	};

	onMount(async () => {
		await getProducts();
		console.log('🚀 ~ file: ProductsList.svelte:11 ~ getProducts ~ response:', dataProducts);
	});
</script>

<div class="flex flex-wrap w-2/3 gap-5 justify-center items-center">
	{#each dataProducts as product}
		<div class="card card-hover w-[350px]">
			<header
				class="card-header aspect-video w-full bg-cover bg-center"
				style="background-image: url('{product.imageURL}')"
			>
				<!-- <img class=" aspect-video w-full" src={product.imageURL} /> -->
			</header>
			<section class="p-4">
				<div>
					{product.name}
				</div>
				<div>
					{product.price}
				</div>
			</section>
			<footer class="card-footer text-sm">{product.uuid}</footer>
		</div>
	{/each}
</div>
