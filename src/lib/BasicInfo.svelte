<script>
    import Icon from '@iconify/svelte';

    export let word = 'fun'
    export let partOfSpeech = 'noun'
    export let phonetic = "/fAn/"
    export let audio = 'src'

    let wordAudio

    const runAudio = () =>{
        wordAudio.play()        
    }

</script>

<div class="container">
    <div>
        <p class="word">{word}</p>
        <p class="phonetic">{partOfSpeech}{phonetic}</p>
    </div>
    <span on:click={runAudio}>
        <Icon icon="fluent:headphones-sound-wave-24-regular" color="#545454" width="24" height="24" />
    </span>
    <audio src="{audio}" controls hidden bind:this={wordAudio}/>
</div>

<style>
    .container{
        display: flex;
        align-items: center;
        justify-content: space-between;
        width: 347px;
        text-align: left;
        margin-bottom: 20px;
    }
    .word{
        font-weight: 500;
        color: #343A40;
    }
    .phonetic{
        font-weight: 500;
        font-size: 14px;
        color: #6C757D;
    }
    span{
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        padding: 5px;
    }
</style>