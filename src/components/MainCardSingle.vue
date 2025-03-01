<script setup>
    import { faker } from '@faker-js/faker'
    const firstname = faker.person.firstName()
    const lastname = faker.person.lastName()
    const jobtitle = faker.person.jobTitle()
    const bio = faker.lorem.lines(2)
    const username = faker.internet.userName({ firstName: firstname, lastName: lastname }).toLowerCase()

    const state = faker.location.state();
    const city = faker.location.city();
    const email = faker.internet.email({ firstName: firstname, lastName: lastname });
    const phone = faker.phone.number();
    </script>

<template>
    <RouterLink :to="{name: 'CardDetail', params: {id: username}}">
        <div class="border border-gray-500 rounded-lg shadow bg-gray-400 h-full">
            <img class="object-fill h-480 w-960" v-bind:src="faker.image.urlLoremFlickr({category: 'watch', height: 480, width: 960})" style="object-fit: cover;" />
            <div class="p-4">
                <p class="mb-2 text-2xl font-bold">{{ firstname }} {{ lastname }}</p>
                <p class="mb-3 font-normal text-gray-800">{{ state }}, {{ city }}</p>
                <p class="mb-3 font-normal text-gray-800">{{ bio }}</p>
                <p class="mb-3 font-normal text-gray-800">{{ email }}</p>
                <p class="mb-3 font-normal text-gray-800">{{ phone }}</p>                
            </div>
       </div>
    </RouterLink>
</template>