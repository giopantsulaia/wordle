<template>
    <section>
        <nav class="relative top-0 left-0 w-screen border-b-2 border-black h-16 flex justify-center items-center">
            <p class="font-black text-3xl uppercase tracking-widest"> Wordle</p>
        </nav>
        <main class="mt-16">
            <div class="flex flex-wrap w-[400px]  m-auto gap-4 justify-center">
                <div class="w-16 h-16 border-2 border-gray-600" v-for="box in 30"></div>
            </div>
        </main>
    </section>
</template>
<script></script>