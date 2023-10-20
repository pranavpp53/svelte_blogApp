<script>
    import { onMount } from "svelte";
    import { postRegistrationData } from "../service/allapi";
    import { navigate } from "@sveltestack/sveltekit";

    let formData = {
        username: "",
        email: "",
        password: "",
    };

    let responseMessage = "";

    const handleChange = (e) => {
        const { name, value } = e.target;
        formData = { ...formData, [name]: value };
    };

    const handleSubmit = async (e) => {
        e.preventDefault();

        try {
            const response = await postRegistrationData(formData);
            if (response.status <= 200) {
                responseMessage = `${response.data.username} your account created successfully`;
                navigate("/");
            } else {
                console.log(`else ${response}`);
            }
        } catch (error) {
            console.error("API error:", error);
        }
    };
</script>

<div class="register">
    <div class="w-25">
        <div class="registerTitle text-center">Register</div>
        <form class="registerForm" on:submit={handleSubmit}>
            <label for="username">Username</label>
            <input
                type="text"
                class="registerInput"
                placeholder="Enter your username..."
                id="username"
                name="username"
                bind:value={formData.username}
                required
                on:input={handleChange}
            />

            <label for="email">Email</label>
            <input
                type="text"
                class="registerInput"
                placeholder="Enter your email..."
                id="email"
                name="email"
                bind:value={formData.email}
                required
                on:input={handleChange}
            />

            <label for="password">Password</label>
            <input
                type="password"
                class="registerInput"
                placeholder="Enter your password..."
                id="password"
                name="password"
                bind:value={formData.password}
                required
                on:input={handleChange}
            />

            <button class="registerButton" type="submit">Register</button>
        </form>
        {#if responseMessage}
            <p>{responseMessage}</p>
        {/if}
    </div>
</div>

<style>
    .register {
      height: calc(100vh - 50px);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      width: 100%;
      background: linear-gradient(
        rgba(255, 255, 255, 0.5),
        rgba(255, 255, 255, 0.5)
      ),
      url("https://i.postimg.cc/CLJ7ZF9Y/creative-copywriting-commercial-text-seo-editing.jpg");
      background-size: cover;
    }
  
    .registerTitle {
      font-size: 50px;
    }
  
    .registerForm {
      margin-top: 20px;
      display: flex;
      flex-direction: column;
    }
  
    .registerForm > label {
      margin: 10px 0;
    }
  
    .registerInput {
      padding: 10px;
      background-color: white;
      border: none;
      border-radius: 10px;
    }
  
    .registerInput:focus {
      outline: none;
    }
  
    .registerButton {
      margin-top: 20px;
      cursor: pointer;
      background-color: teal;
      color: white;
      padding: 10px;
      border: none;
      border-radius: 10px;
      text-align: center;
    }
  </style>
  
