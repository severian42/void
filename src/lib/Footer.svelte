<script lang="ts">
  import Fa from 'svelte-fa/src/fa.svelte'
  import { faLeaf } from '@fortawesome/free-solid-svg-icons'

  let classes = ''
  export { classes as class }
  export let pin: boolean = false
  export let strongMask: boolean = false
</script>

<div class="lower-mask section-footer-mask" class:pin-footer={pin}/>
<div class="lower-mask2" class:strong-mask={strongMask} />
<div class="section-footer {classes}" class:pin-footer={pin}>
  <slot />
  <div class="content has-text-centered credit-footer">
    <p>
      <strong>Nexus</strong>
      <span class="author">by
      <a target="_blank" href="www.biomimicryai.org">ANIMA</a>
      </span>
      <a target="_blank" class="ml-4" href="https://www.biomimicryai.org">
        <span style="position:absolute" class="icon default-text">
          <Fa size="2x" icon={faLeaf}/>
        </span>
      </a>
      <span style="display:inline-block;width:30px;height:20px;"></span>
    </p>
  </div>
</div>
