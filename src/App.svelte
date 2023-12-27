<script lang="ts">
  import svelteLogo from './assets/svelte.svg'
  import viteLogo from '/vite.svg'
  import Counter from './lib/Counter.svelte'
  import Product from './pages/Product.svelte'
  import Home from './pages/Home.svelte'
  import { Router, Link, Route } from 'svelte-routing';

</script>




<main>
  <Router>
    <Route path="/">
      <!-- Komponen yang akan ditampilkan jika path sesuai dengan '/' -->
      <Home />
    </Route>
    <Route path="/home">
      <!-- Komponen yang akan ditampilkan jika path sesuai dengan '/about' -->
      <Home />
    </Route>
    <Route path="/products">
      <!-- Komponen yang akan ditampilkan jika path sesuai dengan '/contact' -->
      <Product />
    </Route>
  </Router>
  
</main>


