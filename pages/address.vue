<template>
	<MainLayout>
		<div id="AddressPage" class="mt-4 max-w-[500px] mx-auto px-2">
			<div class="mx-auto bg-white rounded-lg p-3">
				<div class="text-xl font-bold mb-2">Detalhes do Endereço</div>

				<form @submit.prevent="submit()">
					<TextInput
						class="w-full"
						placeholder="Nome do Contato"
						v-model:input="contactName"
						inputType="text"
						:error="error && error.type == 'contactName' ? error.message : ''"
					/>

					<TextInput
						class="w-full mt-2"
						placeholder="CEP"
						v-model:input="zipCode"
						inputType="text"
						:error="error && error.type == 'zipCode' ? error.message : ''"
					/>

					<TextInput
						class="w-full mt-2"
						placeholder="Endereço"
						v-model:input="address"
						inputType="text"
						:error="error && error.type == 'address' ? error.message : ''"
					/>

					<TextInput
						class="w-full mt-2"
						placeholder="Cidade"
						v-model:input="city"
						inputType="text"
						:error="error && error.type == 'city' ? error.message : ''"
					/>

					<TextInput
						class="w-full mt-2"
						placeholder="Estado"
						v-model:input="country"
						inputType="text"
						:error="error && error.type == 'country' ? error.message : ''"
					/>

					<button
						:disabled="isWorking"
						type="submit"
						class="mt-6 bg-gradient-to-r from-[#FE630C] to-[#FF3200] w-full text-white text-[21px] font-semibold p-1.5 rounded-full"
					>
						<div v-if="!isWorking">Atualizar Endereço</div>
						<Icon v-else name="eos-icons:loading" size="25" class="mr-2" />
					</button>
				</form>
			</div>
		</div>
	</MainLayout>
</template>

<script setup>
	import MainLayout from '~/layouts/MainLayout.vue';
	import TextInput from '~/components/TextInput.vue';
	import { useUserStore } from '~~/stores/user';
	const userStore = useUserStore();
	const user = useSupabaseUser();

	let contactName = ref(null);
	let address = ref(null);
	let zipCode = ref(null);
	let city = ref(null);
	let country = ref(null);

	let currentAddress = ref(null);
	let isUpdate = ref(false);
	let isWorking = ref(false);
	let error = ref(null);

	watchEffect(async () => {
		currentAddress.value = await useFetch(
			`/api/prisma/get-address-by-user/${user.value.id}`
		);

		if (currentAddress.value.data) {
			contactName.value = currentAddress.value.data.name;
			address.value = currentAddress.value.data.address;
			zipCode.value = currentAddress.value.data.zipcode;
			city.value = currentAddress.value.data.city;
			country.value = currentAddress.value.data.country;

			isUpdate.value = true;
		}
		userStore.isLoading = false;
	});

	const submit = async () => {
		isWorking.value = true;
		error.value = null;

		if (!contactName.value) {
			error.value = {
				type: 'contactName',
				message: 'Por favor, informe o nome do contato',
			};
		} else if (!zipCode.value) {
			error.value = {
				type: 'zipCode',
				message: 'Por favor, informe o CEP',
			};
		} else if (!address.value) {
			error.value = {
				type: 'address',
				message: 'Por favor, informe o endereço',
			};
		} else if (!city.value) {
			error.value = {
				type: 'city',
				message: 'Por favor, informe a cidade',
			};
		} else if (!country.value) {
			error.value = {
				type: 'country',
				message: 'Por favor, informe o estado',
			};
		}

		if (error.value) {
			isWorking.value = false;
			return;
		}

		if (isUpdate.value) {
			await useFetch(
				`/api/prisma/update-address/${currentAddress.value.data.id}`,
				{
					method: 'PATCH',
					body: {
						userId: user.value.id,
						name: contactName.value,
						address: address.value,
						zipCode: zipCode.value,
						city: city.value,
						country: country.value,
					},
				}
			);

			isWorking.value = false;

			return navigateTo('/checkout');
		}

		await useFetch('/api/prisma/add-address/', {
			method: 'POST',
			body: {
				userId: user.value.id,
				name: contactName.value,
				address: address.value,
				zipCode: zipCode.value,
				city: city.value,
				country: country.value,
			},
		});

		isWorking.value = false;

		return navigateTo('/checkout');
	};
</script>
