<script>
  import { repositories } from '../stores';
  import { fly } from 'svelte/transition';

  export let repository;
  function onDelete() {
    repositories.update(prev => prev.filter(repo => repo.id !== repository.id));
  }
</script>

<li
  transition:fly={{
    x: -100,
  }}
>
  <div>
    <img src={repository.owner.avatar_url} alt={repository.full_name} />
    <div>
      <small>{repository.owner.login}</small>
      <strong>{repository.full_name}</strong>
    </div>
  </div>
  <button class="remove" on:click={onDelete}>Delete</button>
</li>

<style lang="scss">
  li {
    display: flex;
    padding: 16px;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    background-color: #222222;
    border-radius: 10px;

    transition: background-color 0.2s;

    &:hover {
      background: linear-gradient(
        135deg,
        #242424,
        #262626,
        #3a0311,
        #74031f,
        #da0037
      );
    }

    &:not(:first-child) {
      margin-top: 8px;
    }

    & > div {
      display: flex;
      align-items: center;
      background: transparent;

      img {
        width: 48px;
        height: 48px;
        background: transparent;
      }

      div {
        margin-left: 8px;
        display: flex;
        flex-direction: column;
        background: transparent;

        small {
          color: var(--pink-500);
          background-color: transparent;
        }

        strong {
          font-weight: bold;
          color: var(--text);
          background-color: transparent;
        }
      }
    }

    .remove {
      color: var(--text);
      background-color: transparent;
      border: none;

      transition: color 0.2s;
      &:hover {
        color: var(--primary);
        font-weight: bold;
      }
    }
  }
</style>
