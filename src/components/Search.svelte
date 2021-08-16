<script>
  import { fly } from 'svelte/transition';
  import { repositories } from '../stores';
  import githubLogo from '../assets/github_logo.svg';
  import { onMount } from 'svelte';
  let repository = '';
  let error = null;
  let input;

  let ready = false;
  onMount(() => (ready = true));

  function bounced(node, { delay, duration }) {
    return {
      delay,
      duration,
      css: t => `opacity: ${t};
      transform: scale(${t})
    `,
    };
  }

  async function handleSubmit(e) {
    e.preventDefault();
    if (repository) {
      await fetch(`https://api.github.com/repos/${repository}`)
        .then(response => {
          if (response.ok) {
            return response.json();
          }
          throw new Error('Repository not found!');
        })
        .then(data => {
          repositories.update(repos => {
            const existsRepo = repos.find(r => r.id === data.id);
            if (!existsRepo) {
              repository = '';
              error = '';
              return [...repos, data];
            }
            return repos;
          });
        })
        .catch(err => {
          console.log('ERROR FETCH', err);
          input.focus();
          error = err.message;
        });
    }
  }
</script>

<header>
  <div>
    {#if ready}
      <img
        in:fly={{ y: -100, duration: 1000, delay: 300 }}
        src={githubLogo}
        alt="github logo"
      />
      <h1 in:bounced={{ duration: 1000, delay: 300 }}>Repo finder</h1>
    {/if}
  </div>
  <form on:submit={handleSubmit} autocomplete="off">
    {#if ready}
      <div in:fly={{ y: 100, duration: 1000 }}>
        <input
          bind:this={input}
          class:error={!!error}
          type="text"
          name="search"
          placeholder="Find repository owner/repo"
          bind:value={repository}
          on:focus={e => {
            error = '';
          }}
          autocomplete="off"
        />
        <button>Buscar</button>
      </div>
      {#if error}
        <span
          transition:fly={{
            x: -50,
          }}>{error}</span
        >
      {/if}
    {/if}
  </form>
</header>

<style lang="scss">
  header {
    display: flex;
    flex-direction: column;
    align-items: center;
    > div {
      display: flex;
      flex-direction: column;
      align-items: center;
      img {
        width: 100px;
        height: 100px;
        margin-bottom: 8px;
      }
      h1 {
        font-size: x-large;
        color: var(--pink-500);
        margin-bottom: 16px;
      }
    }
  }

  form {
    width: 100%;
    display: flex;
    flex-direction: column;
    div {
      display: flex;
      width: 100%;
      input {
        flex: 1;
        padding: 0 16px;
        background: #232323;
        border: 1px solid #232323;
        border-radius: 8px;
        height: 44px;
        color: var(--text);

        &:hover,
        :focus {
          border: 1px solid var(--pink-500);
        }

        &:-webkit-autofill {
          border: 1px solid transparent;
          box-shadow: 0 0 0 1000px #232323 inset;
          -webkit-text-fill-color: var(--pink-500) !important;
          transition: background-color 5000s ease-in-out 0s;
        }
      }

      button {
        padding: 8px 16px;
        margin-left: 8px;
        border: none;
        border-radius: 8px;
        background: var(--pink-500);
        font-weight: bold;
        &:hover {
          filter: brightness(0.8);
        }
      }

      .error {
        border: 1px solid tomato;
      }
    }
    span {
      display: block;
      margin-top: 4px;
      color: tomato;
    }
  }
</style>
