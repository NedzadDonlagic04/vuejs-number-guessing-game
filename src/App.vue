<script setup>
import { ref } from 'vue';

const highScore = ref(0);
const score = ref(0);

const guessingInput = ref(null);
const scoreParagraph = ref(null);

if(localStorage.getItem('highScore') === null) {
    localStorage.setItem('highScore', highScore.value);
} else {
    highScore.value = JSON.parse(localStorage.getItem('highScore'));
}

const getRandomNumber = () => Math.floor(Math.random() * 20 + 1);

const guessingFormSubmitEvent = e => {
    e.preventDefault();

    if(scoreParagraph.value.classList.length !== 1) {
        guessingTransitionEndEvent();
        return;
    }

    const generatedNumber = getRandomNumber();
    const guessedNumber = Number(guessingInput.value.value);

    if(generatedNumber === guessedNumber) {
        score.value++;
        scoreParagraph.value.classList.add('correct');

        if(score.value > highScore.value) {
            highScore.value = score.value;
            localStorage.setItem('highScore', highScore.value);
        }

        return;
    }

    score.value = 0;
    scoreParagraph.value.classList.add('wrong');
}

const guessingTransitionEndEvent = () => scoreParagraph.value.classList = 'score';

</script>

<template>
    <header>
        <h1>Number Guessing Game</h1>
        <p>Guess a number between 1 and 20 (including 1 and 20)</p>
    </header>

    <main>
        <p class="high-score">High Score: {{highScore}}</p>
        <p class="score" ref="scoreParagraph" @transitionend="guessingTransitionEndEvent">Score: {{score}}</p>

        <form class="guessing-form" @submit="guessingFormSubmitEvent">
            <input class="guessing-input" ref="guessingInput" type="number" min="1" max="20" name="user-guess" id="user-guess" required>
            <button class="guessing-button">Guess</button>
        </form>
    </main>
</template>

<style scoped>
    header > h1 {
        color: var(--text-color);
        font-size: 3rem;
        margin-top: 3rem;
        text-align: center;
    }

    main {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-content: center;
    }

    header > p,
    main > p {
        color: var(--text-color);
        text-align: center;
    }

    header > p {
        margin-top: 1rem;
        font-size: 1.2rem;
    }

    .high-score {
        margin-top: 3rem;
        font-size: 2rem;
    }

    .score {
        margin-top: 5rem;
        font-size: 1.5rem;
        transition: all .7s ease;
    }

    .correct {
        color: var(--correct-score-color);
        transform: scale(1.7);
    }

    .wrong {
        color: var(--wrong-score-color);
        transform: translateY(3rem) rotate(30deg);
    }

    .guessing-form {
        margin: 5rem auto;
        width: min(100% - 2rem, 600px);
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-content: center;
    }

    .guessing-input,
    .guessing-button {
        padding: 4rem;
        font-size: 3rem;
        border: none;
        outline: none;
    }

    .guessing-input {
        border-radius: .5rem .5rem 0 0;
        text-align: center;
    }

    .guessing-button {
        border-radius: 0 0 .5rem .5rem;
        cursor: pointer;
    }
</style>