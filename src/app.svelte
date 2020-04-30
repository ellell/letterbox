<script>
  import { onMount } from 'svelte';
  let currentWord = '';
  let validWords = [];
  let validationError;
  const mainLetter = 'k';
  const letters = ['a','l', 'b', 'o', 'm','s'];
  const allLetters = letters.concat([mainLetter]);

  const addLetter = (letter) => {
    if (validationError) {
      validationError = '';
      currentWord = letter;
      return;
    }

    currentWord = currentWord + letter;
  };

  const handleLetterClick = (letter) => () => addLetter(letter);

  const validateWord = (word) => {
    if (word.length < 3) {
      return 'För kort';
    }

    if (word.indexOf(mainLetter) === -1) {
      return `Saknar bokstaven ${mainLetter.toUpperCase()}`;
    }

    if (validWords.indexOf(word) !== -1) {
      return 'Redan hittat';
    }

    return undefined;
  };

  const handleSubmit = () => {
    validationError = validateWord(currentWord);
    if (!validationError) {
      validWords = validWords.concat([currentWord]);
      currentWord = '';
    }
  };

  onMount(() => {
    document.body.addEventListener('keydown', (e) => {
      if (allLetters.indexOf(e.key.toLowerCase()) !== -1) {
        addLetter(e.key.toLowerCase());
        return;
      }

      if (e.code === 'Backspace') {
        currentWord = currentWord.slice(0, -1);
        return;
      }

      if (e.code === 'Enter') {
        handleSubmit();
        return;
      }
    });
  });
</script>

<style>
  @keyframes shake {
    10%, 90% {
      transform: translate3d(-1px, 0, 0);
    }

    20%, 80% {
      transform: translate3d(2px, 0, 0);
    }

    30%, 50%, 70% {
      transform: translate3d(-4px, 0, 0);
    }

    40%, 60% {
      transform: translate3d(4px, 0, 0);
    }
  }

  .word-box {
    margin: auto;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    text-transform: uppercase;
  }

  .letter-box {
    position: relative;
    text-align: center;
  }

  .letter-box:before {
    content: '';
    display: block;
    width: 150px;
    height: 150px;
    background: #ff0783;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) rotate(45deg);
    border: 3px solid #222;
    border-radius: 30%;
  }

  .current-word {
    height: 40px;
    font-size: 30px;
    text-align: center;
  }

  .current-word--error {
    animation: shake 0.6s linear;
  }

  .validation-error {
    height: 20px;
    text-align: center;
    color: red;
  }

  .letter {
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    cursor: pointer;
    width: 80px;
    height: 80px;
    border: 3px solid #222;
    font-size: 26px;
    font-weight: bold;
    text-align: center;
    line-height: 80px;
    background-color: #ffc107;
    display: inline-block;
    position: relative;
    border-radius: 100%;
  }

  .letter--1 {
    top: -6px;
  }

  .letter--4 {
    bottom: -6px;
  }

  .letter--0,
  .letter--2 {
    bottom: -34px;
  }

  .letter--3,
  .letter--5 {
    bottom: 34px;
  }

  .letter--main {
    background-color: transparent;
    border: none;
    color: #fff;
    display: block;
    margin: auto;
  }

  .button-group, .valid-words {
    text-align: center;
    margin-top: 20px;
  }

  button {
    border: 3px solid #222;
    border-radius: 30px;
    text-transform: uppercase;
    cursor: pointer;
    font-size: 22px;
    outline: none;
    padding: 10px 20px;
    color: #222;
    margin-left: -10px;
  }

  .valid-word {
    display: inline-block;
  }

  .valid-word::after {
    content: "\00B7";
    margin: 10px;
  }

  .valid-word:last-child::after {
    content: '';
  }
</style>

<div class="word-box">
  <div class="validation-error">{validationError || ''}</div>
  <div class="current-word {validationError && 'current-word--error'}">{currentWord}</div>
  <div class="letter-box">
    {#each letters as letter, index}
      <div on:click={handleLetterClick(letter)} class="letter letter--{index}">{letter}</div>
      {#if index === 2}
        <div on:click={handleLetterClick(mainLetter)} class="letter letter--main">{mainLetter}</div>
      {/if}
    {/each}
  </div>
  <div on:click={handleSubmit} class="button-group">
    <button>skicka</button>
  </div>
  <div class="valid-words">
    {#each validWords as word}
      <span class="valid-word">{word}</span>
    {/each}
  </div>
</div>
