<script lang="ts">
	import { FileDropzone } from '@skeletonlabs/skeleton';
	import { FileButton } from '@skeletonlabs/skeleton';
	import { Modal, getModalStore } from '@skeletonlabs/skeleton';
	import type { ModalSettings, ModalComponent, ModalStore } from '@skeletonlabs/skeleton';
	import axios from 'axios';
	// import {getProducts} from './ProductsList.svelte';

	let name = '';
	let price = '';
	let imageProduct: File | null = null;

	const handleFileInputChange = (event: Event) => {
		const input = event.target as HTMLInputElement;
		if (input.files && input.files[0]) {
			imageProduct = input.files[0];
		}
	};

	const modalStore = getModalStore();

	// export let dataProducts;
	export let getProducts: () => Promise<void>;

	const createProduct = async () => {
		try {
			const formProduct = new FormData();

			formProduct.append('name', name);
			formProduct.append('price', String(price));
			// formProduct.append('imageProduct', imageProduct);

			if (imageProduct !== null) {
				formProduct.append('imageProduct', imageProduct);
			} else {
				throw new Error('Image is required');
			}

			await axios.post(`http://localhost:5000/products`, formProduct, {
				headers: { 'Content-Type': 'multipart/form-data' }
			});

			alert('product create successfully');

			modalStore.close();

			getProducts();
		} catch (error: any) {
			if (error.response) {
				alert(error.response.data.msg);
			} else {
				console.log(error);
			}
		}
	};

	// const handleCreateProduct = async () => {
	// try {
	//   // Lakukan logika untuk membuat produk
	//   const newProduct = await createProduct();

	//   // Dapatkan produk terbaru dari getProducts
	//   const updatedProducts = await getProducts();
	//   // Lakukan sesuatu dengan produk terbaru...
	// } catch (error) {
	//   console.error('Error:', error);
	// }
	// };
</script>

<div class="bg-[#2b2e40] rounded-lg p-6 flex flex-col space-y-5 text-gray-200">
	<label class="label">
		<span>Product Name</span>
		<input
			class="input rounded-md p-2 selection:bg-gray-400"
			type="text"
			placeholder="Product Name"
			bind:value={name}
		/>
	</label>

	<label class="label">
		<span>Product Price</span>
		<input
			class="input rounded-md p-2 selection:bg-gray-400"
			type="number"
			placeholder="Product Price"
			bind:value={price}
		/>
	</label>

	<!-- Field input untuk gambar -->
	<label class="label">
		<span>Product Image</span>
		<input
			class="input rounded-md p-2 border-0"
			type="file"
			accept="image/*"
			on:change={handleFileInputChange}
		/>
	</label>

	<!-- <FileButton name="files" /> -->

	<button type="button" class="btn variant-filled-primary" on:click={createProduct}
		>Save Product</button
	>
</div>
