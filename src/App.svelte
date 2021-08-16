<script>
  import GlobalStyle from './styles/Global.svelte';
  import Search from './components/Search.svelte';
  import Container from './components/Container.svelte';
  import Repositories from './components/Repositories.svelte';
</script>

<GlobalStyle />
<Container>
  <Search />
  <Repositories />
</Container>
