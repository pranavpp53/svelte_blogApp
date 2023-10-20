<script>
    import { onMount } from "svelte";
    import { navigate } from "svelte-routing/src/history";

    let user = false;
    let username= "";
    const handleLogout = () => {
        localStorage.removeItem("userId");
        localStorage.removeItem("userName");
        alert("Logout successful");
        location.reload();
        navigate("/");
    };

    onMount(() => {
      const data = localStorage.getItem("userId");
      if (data && !user) {
        user = true;
      }
      username = localStorage.getItem("userName");
    });
</script>

<div>
    <nav class="navbar navbar-expand-lg bg-body-tertiary ps-5 pe-5">
        <div class="container-fluid">
            <button
                class="navbar-brand nav-button-null"
                on:click={(e) => navigate("/")}>Home</button
            >
            <button
                class="navbar-toggler"
                type="button"
                data-bs-toggle="collapse"
                data-bs-target="#navbarText"
                aria-controls="navbarText"
                aria-expanded="false"
                aria-label="Toggle navigation"
            >
                <span class="navbar-toggler-icon" />
            </button>
            <div class="collapse navbar-collapse" id="navbarText">
                
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    {#if user}
                    <li class="nav-item">
                        <button
                            class="nav-link nav-button-null"
                            on:click={(e) => navigate("/write")}>Write</button
                        >
                    </li>
                    <li class="nav-item">
                        <button
                            class="nav-link nav-button-null"
                            on:click={(e) => navigate("/userposts")}
                            >MyPosts</button
                        >
                    </li>
                   
                    {/if}
                </ul>
               
                <span class="navbar-text">
                    <ul class="navbar-nav me-auto mb-2 mb-lg-0 nav-local-bar">
                        {#if user}
                        <li class="nav-item">
                            <button
                                class="nav-link nav-button-val text-danger"
                                on:click={handleLogout}>LogOut</button
                            >
                        </li>
                        {:else}
                        <li class="nav-item me-4">
                            <button
                                class="nav-link nav-button-val "
                                on:click={(e) => navigate("/login")}
                                >SignIn</button
                            >
                        </li>
                        <li class="nav-item ">
                            <button
                                class="nav-link nav-button-val"
                                on:click={(e) => navigate("/register")}
                                >SignUp</button
                            >
                        </li>
                        {/if}
                    </ul>
                </span>
            </div>
        </div>
    </nav>
</div>

<style>
    .nav-button-null {
        border: none;
        border-radius: 10px;
        font-weight: 600;


    }
    .nav-local-bar{
        border:  1px solid rgb(161, 157, 157);
        border-radius: 16px;
        font-weight: 700;
        padding-left: 20px;
        padding-right: 20px;
    }
</style>
