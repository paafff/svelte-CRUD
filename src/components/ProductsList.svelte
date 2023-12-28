<script lang="ts">
	import axios from 'axios';
	import { onMount } from 'svelte';

	import { Modal, getModalStore } from '@skeletonlabs/skeleton';
	import type { ModalSettings, ModalComponent, ModalStore } from '@skeletonlabs/skeleton';

	import { initializeStores } from '@skeletonlabs/skeleton';

	initializeStores();

	const modalStore = getModalStore();

	interface Product {
		id: number;
		name: string;
		price: number;
		imageName: string;
		imageURL: string;
		uuid: string;
	}

// Di dalam komponen ProductsList
export let dataProducts: Product[]; // Pastikan dataProducts diterima di sini
// Terima fungsi getProducts sebagai properti
export let getProducts: () => Promise<void>;

	// export const getProducts = async () => {
	// 	try {
	// 		// <Product[]> setelah pemanggilan axios.get adalah salah satu cara untuk memberi tahu TypeScript tentang jenis data yang diharapkan yang akan diterima dari respons API.
	// 		const response = await axios.get<Product[]>(`http://localhost:5000/products`);

	// 		dataProducts = response.data;
	// 	} catch (error: any) {
	// 		if (error.response) {
	// 			alert(error.response.data.msg);
	// 		} else {
	// 			console.log(error);
	// 		}
	// 	}
	// };

	// onMount(async () => {
	// 	await getProducts();
	// 	console.log('🚀 ~ file: ProductsList.svelte:11 ~ getProducts ~ response:', dataProducts);
	// });

	//deleteproduct

	let productToDeleteId: string = '';

	const deleteProduct = async () => {
		try {
			await axios.delete(`http://localhost:5000/products/${productToDeleteId}`);

			alert('product deleted successfully');
			await getProducts();
		} catch (error: any) {
			if (error.response) {
				alert(error.response.data.msg);
			} else {
				console.log(error);
			}
		}
	};

	const modalDeleteProduct: ModalSettings = {
		type: 'confirm',
		// Data
		title: 'Please Confirm',
		body: 'Are you sure you wish to proceed?',
		// TRUE if confirm pressed, FALSE if cancel pressed
		// response: (r: boolean) => console.log('response:', r)
		response: (r: boolean) => {
			console.log('response:', r);
			if (r) {
				console.log('ini true');
				deleteProduct();
			} else {
				console.log('ini false');
			}
		}
	};
	// modalStore.trigger(modalDeleteProduct);

	function showModalDeleteProduct(uuid: string, name: string) {
		//set nilai/value modal
		productToDeleteId = uuid;
		modalDeleteProduct.body = `Are you sure you wish to delete product with ID ${uuid} and name ${name}?`;
		// Panggil modal saat tombol diklik
		// modalStore.trigger(modalSettings);
		modalStore.trigger(modalDeleteProduct);
		console.log(uuid);
		console.log(name);
		// console.log('lalalalalaaa')
	}
</script>

<Modal />

<div class="flex flex-wrap w-2/3 gap-5 justify-center items-center">
	{#each dataProducts as product}
		<div class="card bg-[#2b2e40] text-gray-100 card-hover w-[350px]">
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
				<div>
					{product.uuid}
				</div>
			</section>
			<footer class="card-footer flex justify-end space-x-5">
				<button type="button" class="btn rounded-xl variant-filled-secondary">update</button>
				<button
					type="button"
					class="btn rounded-xl variant-filled-primary"
					on:click={()=>showModalDeleteProduct(product.uuid, product.name)}>delete</button
				>
			</footer>
		</div>
	{/each}
</div>
