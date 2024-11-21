<template>
<div class="layout">
    <header><slot name="header"> </slot></header>
    <!--<aside>Ma colone de gauche ici</aside>
    <main>mon main principale ici</main>-->
    <footer><slot name="footer"></slot></footer>
</div>

</template>

<script>




</script>

<style>
.layout{
    display: grid;

}
.layout > * {
    border: none;
}
.layout header,
.layout footer{
    grid-column: 1/-1;
}


</style>