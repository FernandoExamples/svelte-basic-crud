<script lang="ts">
	import { v4 as uuidv4 } from 'uuid';
	let isEditing = false;

	let products = [
		{
			id: uuidv4(),
			name: 'HP Pavillio Notebook',
			category: 'laptops',
			description: 'HP Laptop',
			image: 'https://cdn.pixabay.com/photo/2017/01/22/12/07/imac-1999636_1280.png'
		},
		{
			id: uuidv4(),
			name: 'Mouse Raiser',
			category: 'perifericos',
			description: 'Gamming Mouse',
			image: ''
		}
	];

	let product = {
		id: '',
		name: '',
		description: '',
		category: 'laptops',
		image: 'http://'
	};

	const onSubmit = () => {
		if (!isEditing) {
			const id = uuidv4();
			products = products.concat({ ...product, id });
		} else {
			products = products.map((x) => (x.id == product.id ? product : x));
		}
		isEditing = false;
		cleanForm();
	};

	const cleanForm = () => {
		product = {
			id: '',
			name: '',
			description: '',
			category: 'laptops',
			image: 'http://'
		};
	};

	const deleteProduct = (id: string) => {
		products = products.filter((x) => x.id != id);
	};

	const editProuct = (_product: any) => {
		product = _product;
		isEditing = true;
	};
</script>

<main class="container pt-3">
	<div class="row">
		<div class="col-6">
			{#each products as product}
				<div class="card mb-2 shadow">
					<div class="row align-items-center">
						<div class="col-4">
							{#if product.image}
								<img src={product.image} alt="" class="img-fluid" />
							{:else}
								<img src="no-product.png" alt="" class="img-fluid" />
							{/if}
						</div>
						<div class="col-8 py-3">
							<div class="d-flex gap-2 items-center">
								<h5 class="font-bold m-0">
									{product.name}
								</h5>
								<small class="badge bg-primary">{product.category}</small>
							</div>
							<p class="card-text m-0">{product.description}</p>

							<div class="">
								<button class="btn btn-sm btn-danger" on:click={() => deleteProduct(product.id)}
									>Delete</button
								>
								<button class="btn btn-sm btn-secondary" on:click={() => editProuct(product)}
									>Edit</button
								>
							</div>
						</div>
					</div>
				</div>
			{/each}
		</div>

		<div class="col-6">
			<div class="card">
				<div class="card-body">
					<form on:submit|preventDefault={onSubmit}>
						<div class="form-group mb-3">
							<input
								bind:value={product.name}
								id="name"
								type="text"
								placeholder="Product Name"
								class="form-control"
							/>
						</div>
						<div class="form-group mb-3">
							<textarea
								bind:value={product.description}
								id="description"
								cols="30"
								rows="3"
								class="form-control"
							/>
						</div>
						<div class="form-group mb-3">
							<input
								bind:value={product.image}
								type="url"
								id="img"
								placeholder="https://enlace.com"
								class="form-control"
							/>
						</div>

						<div class="form-group mb-3">
							<select id="category" bind:value={product.category} class="form-control">
								<option value="laptops">Laptops</option>
								<option value="perifericos">Perifericos</option>
								<option value="servers">Servidores</option>
							</select>
						</div>

						<button type="submit" class="btn btn-primary">
							{isEditing ? 'Guardar' : 'Crear'} Producto
						</button>
					</form>
				</div>
			</div>
		</div>
	</div>
</main>
