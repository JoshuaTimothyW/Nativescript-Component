<template>
    <StackLayout class="text-center">
        <ScrollView>
            <StackLayout>
                <StackLayout class="m-4 list-group-item active">
                    <Image src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSAtqhLL0Gg1UifMNwFbbF1bl0jZl5w2mEvAAtkyNEO6CgmoRyPYQ"/>
                    <StackLayout backgroundColor="white">
                        <Label class="h1" text="card title"/>
                        <StackLayout class="hr-light m-5" /> 
                        <Label class="h3" text="card body"/>
                        <StackLayout class="hr-light m-5" />       
                    </StackLayout>
                </StackLayout>
                <StackLayout class="m-4 list-group-item active">
                    <Image src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSAtqhLL0Gg1UifMNwFbbF1bl0jZl5w2mEvAAtkyNEO6CgmoRyPYQ"/>
                    <StackLayout backgroundColor="white">
                        <Label class="h1" text="Card Title"/>
                        <StackLayout class="hr-light m-5" />    
                    </StackLayout>
                    <Button class="btn-primary btn-active" text="Click" @tap="showModal()" />
                    <StackLayout class="hr-light m-5" />
                </StackLayout>                            
            </StackLayout>
        </ScrollView>
    </StackLayout>
</template>

<script>
export default {
    
}
</script>