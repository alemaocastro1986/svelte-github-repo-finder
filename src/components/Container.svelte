<div class="container-sm">
  <slot />
</div>

<style lang="scss">
  .container-sm {
    max-width: 600px;
    display: flex;
    flex-direction: column;
    margin: 0 auto;
    height: 100%;
    padding: 1rem;
  }
</style>
