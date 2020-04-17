<script>
  import { Inertia } from '@inertiajs/inertia'
  import store from './store'

  export let
    initialPage,
    resolveComponent,
    transformProps = props => props
  
  let currentComponent

  Inertia.init({
    initialPage,
    resolveComponent,
    updatePage: (component, props, { preserveState }) => {
      store.update(page => {
        if (page.component !== component && currentComponent) {
          currentComponent.$destroy()
        }
        return {
          component,
          key: preserveState ? page.key : Date.now(),
          props: transformProps(props),
        }
      })
    },
  })
</script>

{#each [$store.key] as key (key)}
<svelte:component this={$store.component} bind:this={currentComponent} {...$store.props} />
{/each}
