<template>
    <div class="transparent flowmo">
        <h2>Title</h2>
        <h1>00:00</h1>
        <div class="btn-con">
            <button>Start</button>
            <button disabled>Reset</button>
        </div>
    </div>
</template>
<style scoped>
.flowmo{
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    gap: 16px;
}
.btn-con{
    margin-top: 8px;
    display: flex;
    justify-content: center;
    align-items: center;
    gap:8px;
}
</style>