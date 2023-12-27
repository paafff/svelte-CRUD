<script lang="ts">
	import { FileDropzone } from '@skeletonlabs/skeleton';
	import axios from 'axios';

	let name = '';
	let price = 0;
	let imageProduct: File | null = null;

	const handleFileInputChange = (event: Event) => {
		const input = event.target as HTMLInputElement;
		if (input.files && input.files[0]) {
			imageProduct = input.files[0];
		}
	};

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
		} catch (error: any) {
			if (error.response) {
				alert(error.response.data.msg);
			} else {
				console.log(error);
			}
		}
	};
</script>

<div class="bg-gray-700 rounded-lg p-6 flex flex-col space-y-5 text-gray-200">
	<label class="label">
		<span>Product Name</span>
		<input
			class="input rounded-md p-2 bg-gray-100 text-gray-800 selection:bg-gray-400"
			type="text"
			placeholder="Product Name"
			bind:value={name}
		/>
	</label>

	<label class="label">
		<span>Product Price</span>
		<input
			class="input rounded-md p-2 bg-gray-100 text-gray-800 selection:bg-gray-400"
			type="number"
			placeholder="Product Price"
			bind:value={price}
		/>
	</label>

	<!-- Field input untuk gambar -->
	<label class="label">
		<span>Product Image</span>
		<input
			class="input rounded-md p-2 bg-gray-700 hover:bg-gray-700 border-0"
			type="file"
			accept="image/*"
			on:change={handleFileInputChange}
		/>
	</label>

	<button type="button" class="btn variant-filled-primary" on:click={createProduct}>Save</button>
</div>
