<script>
    import Login from "./components/login.svelte";
    import Edit from "./components/Edit.svelte";
    import Navbar from "./components/Navbar.svelte";
    import Write from "./components/Write.svelte";
    import Userposts from "./components/Userposts.svelte";
    import { Link, Route, Router } from "svelte-routing";
    import Home from "./components/Home.svelte";
    import Register from "./components/Register.svelte";

</script>

<main>
    <Navbar />
    <Router>
        <Route path="/ " component={Home} />
        <Route path="/register" component={Register} />
        <Route path="/login" component={Login} />
        <Route path="/write" component={Write} />
        <Route path="/userposts" component={Userposts} />
        <Route path="/edit/:id" component={Edit} />
    </Router>
</main>
