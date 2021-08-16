<script>
  import { onMount } from 'svelte';

  import { fade } from 'svelte/transition';
  import { repositories } from '../stores';
  import Repository from './Repository.svelte';
</script>

<section>
  {#if $repositories.length >= 0}
    <ul>
      {#each $repositories as repository (repository.id)}
        <Repository {repository} />
      {/each}
    </ul>
  {/if}
</section>

<style lang="scss">
  section {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 24px;
    width: 100%;
  }

  ul {
    width: 100%;
    list-style: none;
  }
</style>
