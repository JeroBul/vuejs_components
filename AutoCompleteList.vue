<template>
    <div class="autocomplete" v-click-outside="exitResult">
        <label>Search</label><input v-model="search" type="text" @keyup.down="onArrowDown(-1)" ref="searchInput" />
        <ul v-show="isOpen" class="autocomplete-results" ref="ulref">
            <li v-for="(result, i) in resultFilter" :key="i" class="autocomplete-result" @click="onSelectField(result)"
                @keyup.enter="onSelectField(result)" @keyup.down="onArrowDown(i)" @keyup.up="onArrowUp(i)" @keyup.tab="exitResult" ref="items"
                tabindex="0" @keyup.esc="exitResult">{{props.compareFunction(result)}}</li>
        </ul>
    </div>

</template>

<script setup>
import { computed, ref, onUnmounted, onMounted, useTemplateRef } from 'vue'
const isOpen = ref(false);
const allValue = defineModel("allValues");
const selectedValues = defineModel("selectedValues");
const search = ref("");
// const emit = defineEmits(['selected']);
const itemRefs = useTemplateRef('items');
const ulref = useTemplateRef('ulref');
const searchInput = useTemplateRef('searchInput');
const selected = ref();
const props = defineProps({
    compareFunction : Function
})


let resultFilter = computed(() => {
    if (search.value.length === 0) {
        isOpen.value = false;
        return [];

    }
    else {
        console.log("search : " + search.value)
    }
    isOpen.value = true;
    if(selectedValues === undefined){
    return allValue.value.filter(r => props.compareFunction(r).toUpperCase().includes(search.value.toUpperCase()))
     }
     else {
         return allValue.value.filter(r=>!selectedValues.value.includes(r)).filter(r => props.compareFunction(r).toUpperCase().includes(search.value.toUpperCase()))
    }
}
)


const onSelectField = (result) => {
    selected.value = result;
    search.value = "";
    // emit("selected",selected);
    selected.value = "";
    searchInput.value.focus();
    selectedValues.value.push(result);
   }

//Handle key

const clickOutsideM = () => {
    console.log("Click outside methods");
}

const exitResult = () => {
    search.value = "";
    // searchInput.value.focus();
}

const exit = () => {
    console.log("Exist methods");
    isOpen.value = false;
    search.value = "";

}

const handleClickOutside = (event) => {
    console.log("Click out : ")
    // if (!this.$el.contains(event.target)) {
    //     arrowCounter.value = -1;
    //     isOpen.value = false;
    // }
}

const onArrowDown = (i) => {
    const nextFocusId = i + 1;
    if (nextFocusId === resultFilter.value.length || nextFocusId > resultFilter) {
        itemRefs.value[0].focus();
    } else {
        itemRefs.value[nextFocusId].focus()
    }


}
const onArrowUp = (i) => {
    const nextFocusId = i - 1;
    console.log("Narrow up next focus :  " + nextFocusId)
    if (nextFocusId === -1) {
        itemRefs.value[resultFilter.value.length - 1].focus();
    } else {
        itemRefs.value[nextFocusId].focus()
    }

}

// see: https://medium.com/@stjepan.crncic/crafting-a-simple-click-outside-directive-in-vue-3-980c55ab1a65
const VClickOutside = {
    mounted(el, binding) {
        el.clickOutsideEvent = function(event) {
            // Check if the clicked element is neither the element
            // to which the directive is applied nor its child
            if (!(el === event.target || el.contains(event.target))) {
                // Invoke the provided method
                binding.value(event);
            }
        };
        document.addEventListener('click', el.clickOutsideEvent);
    },
    unmounted(el) {
        // Remove the event listener when the bound element is unmounted
        document.removeEventListener('click', el.clickOutsideEvent);
    }
};


</script>

<style scoped>
.autocomplete {
    position: relative;
}

.autocomplete-resultFilter {
    padding: 0;
    margin: 0;
    border: 1px solid #eeeeee;
    height: 120px;
    min-height: 1em;
    max-height: 6em;
    overflow: auto;
}

.autocomplete-result {
    list-style: none;
    text-align: rigth;
    padding: 4px 2px;
    cursor: pointer;
    border: none;
}

.autocomplete-result.is-active,
.autocomplete-result:hover {
    background-color: #4AAE9B;
    color: white;
}
</style>