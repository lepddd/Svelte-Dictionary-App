<script>
  import BasicInfo from "./lib/BasicInfo.svelte";
  import InputText from "./lib/InputText.svelte";
  import SelectBtn from "./lib/SelectBtn.svelte";
  import Card from "./lib/Card.svelte";
  import SynCard from "./lib/SynCard.svelte";

  let wordData,
    activeItem = 0,
    isEmpty = true,
    value;

  const getWordData = async (word) => {
    const res = await fetch(
      `https://api.dictionaryapi.dev/api/v2/entries/en/${word}`
    );
    const data = await res.json();
    const result = data[0];
    const formatedData = formatData(result);
    return formatedData;
  };

  const getExamples = (data) => {
    const examples = data.meanings.map((example) => {
      if (!example.definitions[0].example) {
        example.definitions[0].example = "No existing examples!";
      }
      return example.definitions[0].example;
    });
    return examples
  }

  const getSynonyms = (data) => {
    const synonyms = data.meanings.map((synonym) => {
      if (synonym.synonyms.length > 5) {
        synonym.synonyms = synonym.synonyms.slice(0, 5);
      } else if (synonym.synonyms.length < 5 && synonym.synonyms.length > 0) {
        synonym.synonyms = synonym.synonyms;
      } else {
        synonym.synonyms = ["No existing synonyms!"];
      }
      return synonym.synonyms;
    });  
    return synonyms
  }

  const getMeanings = (data,synonyms,examples) => {
    const meanings = data.meanings.map((meaning, index) => {
      return {
        definitions: meaning.definitions[0].definition,
        synonyms: synonyms[index],
        example: examples[index],
      };
    });
    return meanings    
  }

  const formatData = (data) => {
    const audio = data.phonetics.filter((el) => el.audio !== "");

    const partOfSpeech = data.meanings.map((speech) => speech.partOfSpeech);

    const examples = getExamples(data)

    const synonyms = getSynonyms(data)

    const meanings = getMeanings(data,synonyms,examples)

    return {
      word: data.word,
      phonetic: data.phonetic,
      audio: audio[0].audio,
      partOfSpeech: partOfSpeech,
      meanings: meanings,
    };
  };

  const handleInputValue = (e) => {
    if (e.detail.length !== 0) {
      value = e.detail;
      isEmpty = false;
      activeItem = 0;
      wordData = getWordData(e.detail);
    } else {
      isEmpty = true;
    }
  };
</script>

<main>
  <h1>English Dictionary</h1>
  <InputText
    on:press={(e) => handleInputValue(e)}
    on:clear={() => (isEmpty = true)}
    {value}
  />
  {#if isEmpty}
    <p class="erro">
      Type a word and press <b>enter</b> to get meaning, exemple, pronunciation,
      and synonyms of that typed word.
    </p>
  {:else}
    {#await wordData}
      <p class="erro">searching results for <b>{value}</b>.</p>
    {:then data}
      <BasicInfo
        word={data.word}
        phonetic={data.phonetic}
        audio={data.audio}
        partOfSpeech={data.partOfSpeech[activeItem]}
      />
      <div class="result-container">
        <Card title={"Meaning"} desc={data.meanings[activeItem].definitions} />
        <Card title={"Example"} desc={data.meanings[activeItem].example} />
        <SynCard
          title={"Synonyms"}
          synonyms={data.meanings[activeItem].synonyms}
          on:searchSyn={(e) => handleInputValue(e)}
        />
      </div>
      <SelectBtn
        {activeItem}
        speechs={data.partOfSpeech}
        on:select={(e) => (activeItem = e.detail)}
      />
    {:catch}
      <p class="erro">
        we couldn't find definitions for the <b>word</b> you were looking for.
      </p>
    {/await}
  {/if}
</main>

<style>
  main {
    position: relative;
    width: 427px;
    background-color: #fff;
    padding: 40px;
    border-radius: 5px 5px 0 0;
  }
  main h1 {
    color: #343a40;
  }
  b {
    color: #343a40;
  }
  .erro {
    text-align: left;
    color: #6c757d;
    font-weight: 500;
    font-size: 12px;
  }
  .result-container {
    height: 181px;
    overflow: auto;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
</style>
