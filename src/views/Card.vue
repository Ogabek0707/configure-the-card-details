<template>
    <div class="w-full h-full flex items-center justify-center gap-4 md:gap-10 flex-wrap py-4">
        <div v-for="(card, index) in cards" :key="index"
            class="w-full max-w-md md:w-96 h-56 mt-4 md:mt-10 bg-red-100 rounded-xl relative text-white shadow-2xl transition-transform transform">
            <img alt="no" class="relative object-cover w-full h-full rounded-xl" src="https://i.imgur.com/kGkSg1v.png" />
            <div class="w-full px-8 absolute top-8">
                <div class="flex justify-between">
                    <div>
                        <p class="font-light">Name</p>
                        <p class="font-medium tracking-widest">{{ card.name }}</p>
                    </div>
                    <img alt="logo" class="w-14 h-14" src="https://i.imgur.com/bbPHJVe.png" />
                </div>
                <div class="pt-1">
                    <p class="font-light">Card Number</p>
                    <p class="font-medium tracking-more-wider">{{ card.number }}</p>
                </div>
                <div class="pt-6 pr-6">
                    <div class="flex justify-between">
                        <div>
                            <p class="font-light text-xs">Valid</p>
                            <p class="font-medium tracking-wider text-sm">{{ card.valid }}</p>
                        </div>
                        <div>
                            <p class="font-light text-xs">Expiry</p>
                            <p class="font-medium tracking-wider text-sm">{{ card.expiry }}</p>
                        </div>
                        <div>
                            <p class="font-light text-xs">CVV</p>
                            <p class="font-bold tracking-more-wider text-sm">{{ card.cvv }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="w-full md:w-auto">
            <vee-form :validation-schema="schema" @submit="send"
                class="w-full grid grid-cols-1 md:grid-cols-2 gap-4 p-6 max-w-lg mx-auto mt-4 md:mt-10">
                <VInput type="text" label="" name="name" placeholder="Card name" class=""></VInput>
                <VInput type="text" label="" name="valid" placeholder="Valid" v-mask="'##/##'" class=""></VInput>
                <VInput type="text" label="" name="expiry" placeholder="Expiry" v-mask="'##/##'" class=""></VInput>
                <VInput type="text" label="" name="cvv" placeholder="CVV" v-mask="'###'"></VInput>
                <VInput type="text" card="card" label="" name="card_number" placeholder="Card Number" v-mask="'#### #### #### ####'"
                    class="col-span-1 md:col-span-2"></VInput>
                <VButton type="submit" btn_type="primary"
                    class="bg-green-600 col-span-1 md:col-span-2 text-[17px] text-center mt-[5px] py-[15px]">Submit</VButton>
            </vee-form>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import VInput from '@/components/form/VInput.vue';
import VButton from '@/components/form/VButton.vue';

const loading = ref(false)

const schema = computed(() => {
    return {
        name: 'required|min:3|max:30',
        card_number: 'required|min:0|max:19',
        valid: 'required|min:0|max:5',
        expiry: 'required|min:0|max:5',
        cvv: 'required|min:0|max:3',
    }
})

const cards = ref([
    {
        name: "",
        number: "",
        valid: "",
        expiry: "",
        cvv: "",
    },
]);
const send = async (values) => {
    loading.value = true;
    cards.value[0] = {
        name: values.name,
        number: values.card_number,
        valid: values.valid,
        expiry: values.expiry,
        cvv: values.cvv,
    };
    localStorage.setItem('cards', JSON.stringify(cards.value));
    location.reload()
    loading.value = false;
};

onMounted(() => {
    const storedCards = localStorage.getItem('cards');
    if (storedCards) {
        cards.value = JSON.parse(storedCards);
    }
});

const btn_title = computed(() => {
    return loading.value ? 'Loading' : 'Submit';
});
</script>

<style lang="scss" scoped>

</style>