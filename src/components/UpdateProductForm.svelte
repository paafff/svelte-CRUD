<script lang="ts">
	import axios from 'axios';
	import { onMount } from 'svelte';
	import Product from '../pages/Product.svelte';
	import { Modal, getModalStore } from '@skeletonlabs/skeleton';

	const modalStore = getModalStore();

	// export let dataProducts;
	export let getProducts: () => Promise<void>;
	export let productSelectedUUID: string;

	const handleFileInputChange = (event: Event) => {
		const input = event.target as HTMLInputElement;
		if (input.files && input.files[0]) {
			imageProduct = input.files[0];
		}
	};

	interface Product {
		name: string;
		uuid: string;
		price: number;
		imageURL: string;
	}

	let productData: Product = {
		name: '',
		uuid: '',
		price: 0,
		imageURL: ''
	};

	let name = '';
	let price = '';
	let imageProduct: File | null = null;

	const getProductsByUUID = async () => {
		try {
			const response = await axios.get(`http://localhost:5000/products/${productSelectedUUID}`);

			// productSelectedUUID
			productData = response.data;
			console.log(
				'🚀 ~ file: UpdateProductForm.svelte:38 ~ getProductsByUUID ~ productData:',
				productData
			);
			// console.log(productData);
		} catch (error: any) {
			if (error.response) {
				alert(error.response.data.msg);
			} else {
				console.log(error);
			}
		}
		console.log(
			'🚀 ~ file: UpdateProductForm.svelte:49 ~ getProductsByUUID ~ productSelectedUUID:',
			productSelectedUUID
		);
	};

	const updateProduct = async () => {
		try {
			const formUpdateProduct = new FormData();

			formUpdateProduct.append('name', name);
			formUpdateProduct.append('price', price);

			if (imageProduct !== null) {
				formUpdateProduct.append('imageProduct', imageProduct);
			} else {
				imageProduct = null;
			}

			await axios.patch(
				`http://localhost:5000/products/${productSelectedUUID}`,
				formUpdateProduct,
				{
					headers: { 'Content-Type': 'multipart/form-data' }
				}
			);
			alert('product update successfully');

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

	onMount(async () => {
		await getProductsByUUID();
		console.log('🚀 ~ file: UpdateProductForm.svelte:11 ~ productData:', productData);
	});
</script>

<div class="bg-[#2b2e40] rounded-lg p-6 flex flex-col space-y-5 text-gray-200">
	<label class="label">
		<span>Product Name : {productData.name}</span>
		<input
			class="input rounded-md p-2 selection:bg-gray-400"
			type="text"
			placeholder="Product Name"
			bind:value={name}
		/>
	</label>

	<label class="label">
		<span>Product Price : {productData.price}</span>
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

	<button type="button" class="btn variant-filled-primary" on:click={updateProduct}
		>Update Product</button
	>
</div>
