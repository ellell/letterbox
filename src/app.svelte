<script>
  const mainLetter = 'k';
  const letters = ['a','l', 'b', 'o', 'm','s']
</script>

<style>
  .letter-box {
    width: 278px;
    margin: auto;
  }

  .letter {
    width: 80px;
    height: 80px;
    border: 3px solid #222;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    text-transform: uppercase;
    font-size: 22px;
    font-weight: bold;
    text-align: center;
    line-height: 80px;
    background-color: orange;
    display: inline-block;
    margin: 2px 0;
    position: relative;
  }

  .letter--0,
  .letter--2 {
    bottom: -45px;
  }

  .letter--3,
  .letter--5 {
    bottom: 45px;
  }

  .letter--main {
    background-color: purple;
    color: #fff;
    display: block;
    margin-left: 90px;
  }
</style>

<div class="letter-box">
  {#each letters as letter, index}
    <div class="letter letter--{index}">{letter}</div>
    {#if index === 2}
      <div class="letter letter--main">{mainLetter}</div>
    {/if}
  {/each}
</div>
