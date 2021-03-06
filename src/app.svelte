<script>
  import { flip } from 'svelte/animate';
  import { onMount } from 'svelte';
  let currentWord = '';
  let validWords = [];
  let validationError;
  const mainLetter = 'k';

  let letters = ['a','l', 'b', 'o', 'm','s'];
  const allLetters = letters.concat([mainLetter]);
  let lastActiveLetter;
  const addLetter = (letter) => {
    lastActiveLetter = letter;
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

  const handleScramble = () => {
    letters = letters.sort(() => Math.random() - 0.5);
  }

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

  @keyframes scale {
    0%, 100% {
      transform: scale(1);
    }

    50% {
      transform: scale(1.1);
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
    max-width: 258px;
    margin: 60px auto;
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
    position: relative;
  }

  .letter--active {
    animation: scale 0.4s ease;
  }

  .letter--1 {
    top: -50px;
  }

  .letter--4 {
    bottom: -50px;
  }

  .letter--0,
  .letter--2 {
    bottom: 4px;
  }

  .letter--3,
  .letter--5 {
    bottom: -4px;
  }

  .letter--main {
    background-color: transparent;
    border: none;
    color: #fff;
    display: block;
    margin: auto;
    position: absolute;
    top: calc(50% - 10px);
    left: calc(50% - 10px);
    width: 20px;
    line-height: 20px;
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
    background-color: transparent;
    transition: background-color 0.4s ease;
  }

  button:hover {
    background-color: #ddd;
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
    <div on:click={handleLetterClick(mainLetter)} class="letter letter--main {lastActiveLetter === mainLetter && 'letter--active'}">{mainLetter}</div>
    {#each letters as letter, index (letter)}
      <div
        animate:flip="{{ duration: 800 }}"
        on:click={handleLetterClick(letter)}
        class="letter letter--{index} {lastActiveLetter === letter && 'letter--active'}">
        {letter}
      </div>
    {/each}
  </div>
  <div class="button-group">
    <button on:click={handleScramble}>skramla</button>
    <button on:click={handleSubmit}>skicka</button>
  </div>
  <div class="valid-words">
    {#each validWords as word}
      <span class="valid-word">{word}</span>
    {/each}
  </div>
</div>
