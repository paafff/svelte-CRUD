<script lang="ts">
	import { Modal, getModalStore } from '@skeletonlabs/skeleton';
	import type { ModalSettings, ModalComponent, ModalStore } from '@skeletonlabs/skeleton';
	import CreateProductForm from '../components/CreateProductForm.svelte';

	import { initializeStores } from '@skeletonlabs/skeleton';
	import ProductsList from '../components/ProductsList.svelte';

	initializeStores();

	const modalStore = getModalStore();

	// const modal: ModalSettings = {
	// 	type: 'prompt',
	// 	// Data
	// 	title: 'Enter Name',
	// 	body: 'Provide your first name in the field below.',
	// 	// Populates the input value and attributes
	// 	value: 'Skeleton',
	// 	valueAttr: { type: 'text', minlength: 3, maxlength: 10, required: true },
	// 	// Returns the updated response value
	// 	response: (r: string) => console.log('response:', r)
	// };

	// function showModal() {
	// 	// Panggil modal saat tombol diklik
	// 	// modalStore.trigger(modalSettings);
	// 	modalStore.trigger(modal);
	// 	// console.log('lalalalalaaa')
	// }

	const modal: ModalSettings = {
		type: 'component',
		component: 'modalCreateForm'
	};

	const modalRegistry: Record<string, ModalComponent> = {
		// Set a unique modal ID, then pass the component reference
		modalCreateForm: { ref: CreateProductForm }
		// modalComponentTwo: { ref: ModalComponentTwo },
		// ...
	};

	function showModal() {
		// Panggil modal saat tombol diklik
		// modalStore.trigger(modalSettings);
		modalStore.trigger(modal);
		// console.log('lalalalalaaa')
	}
</script>

<div class="flex flex-col justify-center items-center">
	<Modal components={modalRegistry} />
	<h1>Input Your Product</h1>

	<button type="button" class="btn variant-filled" on:click={showModal}>Create Product</button>
	<!-- <button type="button" class="btn variant-filled" on:click={()=> {modalStore.trigger(modal)}}>Create Product</button> -->
</div>

<div class="flex w-full justify-center items-center">
	<ProductsList />
</div>
