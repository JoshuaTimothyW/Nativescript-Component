<template>
	<Page>
		<StackLayout class="p-5" backgroundColor="white">
			<Label class="h2 text-center" text="This is a modal page." />
			<StackLayout class="m-4 list-group-item active">
				<Image src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSAtqhLL0Gg1UifMNwFbbF1bl0jZl5w2mEvAAtkyNEO6CgmoRyPYQ"/>
				<StackLayout backgroundColor="white">
					<Label class="h1" text="Card Title"/>
					<StackLayout class="hr-light m-5" />    
				</StackLayout>
				<Button class="btn-primary btn-active" text="Click" @tap="test" />
				<StackLayout class="hr-light m-5" />
			</StackLayout> 
			<Button class="btn btn-outline" text="Close Modal" @tap="$modal.close()" />
		</StackLayout>
	</Page>
</template>

<script>
	export default {
		data() {
			return{};
		},
	}
</script>