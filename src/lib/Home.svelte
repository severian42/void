<script lang="ts">
  import { globalStorage, lastChatId, getChat, started, /* setGlobalSettingValueByKey, */checkStateChange } from './Storage.svelte'
  import Footer from './Footer.svelte'
  import { replace } from 'svelte-spa-router'
  import { afterUpdate, onMount } from 'svelte'
  // import { getPetalsBase, getPetalsWebsocket } from './ApiUtil.svelte'
  import { set as setOpenAI } from './providers/openai/util.svelte'
  import { hasActiveModels } from './Models.svelte'


// let showPetalsSettings = $globalStorage.enablePetals
let pedalsEndpoint = $globalStorage.pedalsEndpoint
let hasModels = hasActiveModels()

onMount(() => {
    if (!$started) {
      $started = true
      // console.log('started', apiKey, $lastChatId, getChat($lastChatId))
      if (hasActiveModels() && getChat($lastChatId)) {
        const chatId = $lastChatId
        $lastChatId = 0
        replace(`/chat/${chatId}`)
      }
    }
    $lastChatId = 0
})

afterUpdate(() => {
    hasModels = hasActiveModels()
    pedalsEndpoint = $globalStorage.pedalsEndpoint
    $checkStateChange++
})

/* const setPetalsEnabled = (event: Event) => {
    const el = (event.target as HTMLInputElement)
    setGlobalSettingValueByKey('enablePetals', !!el.checked)
    showPetalsSettings = $globalStorage.enablePetals
    hasModels = hasActiveModels()
} */

</script>

<section class="section">
  <article class="message">
    <div class="message-body">
    <p class="mb-4">
      <strong>ANIMA Nexus</strong> is an AI assistant that was instructed to help scientists and engineers understand, learn from, and emulate the strategies used by living things to create sustainable designs and technologies.
    </p>
    <p class="mb-4">
      ANIMA can guide users through the Biomimicry Institute’s Design Process, a step-by-step method to propose biomimetic solutions to challenges. This process includes defining the problem, biologizing the challenge, discovering natural models, abstracting design strategies, and emulating nature's lessons.
    </p>

    <!---
    <p>
      As an alternative to OpenAI, you can also use Petals swarm as a free API option for open chat models like Llama 2. 
    </p>
    --->
    </div>
  </article>
  <article class="message" class:is-danger={!hasModels}>
    <div class="message-body">
      Welcome to ANIMA Nexus!

    </div>
  </article>

  <!---
  <article class="message" class:is-danger={!hasModels} class:is-warning={!showPetalsSettings} class:is-info={showPetalsSettings}>
    <div class="message-body">
      <label class="label" for="enablePetals">
        <input 
        type="checkbox"
        class="checkbox" 
        id="enablePetals"
        checked={!!$globalStorage.enablePetals} 
        on:click={setPetalsEnabled}
      >
        Use Petals API and Models (Llama 2)
      </label>
      {#if showPetalsSettings}
        <p>Set Petals API Endpoint:</p>
        <form
          class="field has-addons has-addons-right"
          on:submit|preventDefault={(event) => {
            if (event.target && event.target[0].value) {
              const v = event.target[0].value.trim()
              const v2 = v.replace(/^https:/i, 'wss:').replace(/(^wss:\/\/[^/]+)\/*$/i, '$1' + getPetalsWebsocket())
              setGlobalSettingValueByKey('pedalsEndpoint', v2)
              event.target[0].value = v2
            } else {
              setGlobalSettingValueByKey('pedalsEndpoint', '')
            }
          }}
        >
          <p class="control is-expanded">
            <input
              aria-label="PetalsAPI Endpoint"
              type="text"
              class="input"
              placeholder={getPetalsBase() + getPetalsWebsocket()}
              value={$globalStorage.pedalsEndpoint || ''}
            />
          </p>
          <p class="control">
            <button class="button is-info" type="submit">Save</button>
          </p>

          
        </form>
        
        {#if !pedalsEndpoint}
          <p class="help is-warning">
            Please only use the default public API for testing. It's best to <a target="_blank" href="https://github.com/petals-infra/chat.petals.dev">configure a private endpoint</a> and enter it above for connection to the Petals swarm.
          </p>
        {/if}
        <p class="my-4">
          <a target="_blank" href="https://petals.dev/">Petals</a> lets you run large language models at home by connecting to a public swarm, BitTorrent-style, without hefty GPU requirements.
        </p>
        <p class="mb-4">
          You are encouraged to <a target="_blank" href="https://github.com/bigscience-workshop/petals#connect-your-gpu-and-increase-petals-capacity">set up a Petals server to share your GPU resources</a> with the public swarm. Minimum requirements to contribute Llama 2 completions are a GTX&nbsp;1080&nbsp;8GB, but the larger/faster the better.
        </p>
        <p class="mb-4">
          If you're receiving errors while using Petals, <a target="_blank" href="https://health.petals.dev/">check swarm health</a> and consider <a target="_blank" href="https://github.com/bigscience-workshop/petals#connect-your-gpu-and-increase-petals-capacity">adding your GPU to the swarm</a> to help.
        </p>
        <p class="help is-warning">
          Because Petals uses a public swarm, <b>do not send sensitive information</b> when using Petals.
        </p>
      {/if}
    </div>
  </article>
  --->

    <article class="message is-info">
      <div class="message-body">
        Select an existing chat on the sidebar, or
        <a href={'#/chat/new'}>create a new chat</a>
      </div>
    </article>

</section>
<Footer pin={true} />