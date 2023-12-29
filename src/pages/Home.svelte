<script lang="ts">
	import { Modal, getModalStore } from '@skeletonlabs/skeleton';
	import type { ModalSettings, ModalComponent, ModalStore } from '@skeletonlabs/skeleton';
	import CreateProductForm from '../components/CreateProductForm.svelte';

	import { initializeStores } from '@skeletonlabs/skeleton';
	import ProductsList from '../components/ProductsList.svelte';
	import { onMount } from 'svelte';
	import axios from 'axios';

	initializeStores();

	const modalStore = getModalStore();

	const modal: ModalSettings = {
		type: 'component',
		component: 'modalCreateForm'
	};

	function showModal() {
		// Panggil modal saat tombol diklik
		// modalStore.trigger(modalSettings);
		modalStore.trigger(modal);
		// console.log('lalalalalaaa')
	}

	interface Product {
		id: number;
		name: string;
		price: number;
		imageName: string;
		imageURL: string;
		uuid: string;
	}

	let dataProducts: Product[] = [];

	export const getProducts = async () => {
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

	const modalRegistry: Record<string, ModalComponent> = {
		// Set a unique modal ID, then pass the component reference
		modalCreateForm: {
			ref: CreateProductForm,
			props: {
				// dataProducts, // Meneruskan properti dataProducts
				getProducts // Meneruskan properti getProducts
			}
		}
		// modalComponentTwo: { ref: ModalComponentTwo },
		// ...
	};
</script>

<div class="flex flex-col justify-center items-center">
	<Modal components={modalRegistry} />
	<h1>Input Your Product</h1>

	<button type="button" class="btn variant-filled" on:click={showModal}>Create Product</button>
	<!-- <button type="button" class="btn variant-filled" on:click={()=> {modalStore.trigger(modal)}}>Create Product</button> -->
</div>

<div class="flex w-full justify-center items-center">
	<ProductsList {dataProducts} {getProducts} />
</div>
