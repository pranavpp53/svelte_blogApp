<script>
    import { onMount } from "svelte";
    import { loginUser } from "../service/allapi";
    import { navigate } from "svelte-routing/src/history";

    let formData = {
        email: "",
        password: "",
    };
    let responseMessage = null;

    const handleInputChange = (event) => {
        const { name, value } = event.target;
        formData = { ...formData, [name]: value };
    };

    const handleSubmit = async (event) => {
        event.preventDefault();

        try {
            const response = await loginUser(formData);

            if (response.status <= 210) {
                localStorage.setItem("userId", response.data.user._id);
                localStorage.setItem("userName", response.data.user.username);
                alert(response.data.message);
                navigate("/");
                window.location.reload();
            } else {
                alert(response.data.message);
            }
        } catch (error) {
            responseMessage =
                "Error: An error occurred while making the API request";
        }
    };
</script>

<div>
    <div class="login">
        <div class="w-25">
            <div class="loginTitle text-center">Login</div>
            <form class="loginForm" on:submit={handleSubmit}>
                <label for="email">Email</label>
                <input
                    class="loginInput"
                    type="text"
                    name="email"
                    placeholder="Enter your email..."
                    required
                    bind:value={formData.email}
                    on:input={handleInputChange}
                />
                <label for="password">Password</label>
                <input
                    class="loginInput"
                    type="password"
                    name="password"
                    placeholder="Enter your password..."
                    required
                    bind:value={formData.password}
                    on:input={handleInputChange}
                />
                <button type="submit" class="loginButton">Login</button>
            </form>
        </div>
    </div>
    {#if responseMessage}
        <div class="responseMessage">{responseMessage}</div>
    {/if}
</div>

<style>
    .login {
      height: calc(100vh - 50px);
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 100%;
      justify-content: center;
      background: linear-gradient(
        rgba(255, 255, 255, 0.5),
        rgba(255, 255, 255, 0.5)
      ),
      url("https://i.postimg.cc/CLJ7ZF9Y/creative-copywriting-commercial-text-seo-editing.jpg");
      background-size: cover;
    }
    
    .loginTitle {
      font-size: 50px;
    }
    
    .loginForm {
      margin-top: 20px;
      display: flex;
      flex-direction: column;
    }
    
    .loginForm > label {
      margin: 10px 0;
    }
    
    .loginInput {
      padding: 10px;
      background-color: white;
      border: none;
      border-radius: 10px;
    }
    
    .loginInput:focus {
      outline: none;
    }
    
    .loginButton {
      margin-top: 45px;
      cursor: pointer;
      background-color: lightcoral;
      color: white;
      padding: 10px;
      border: none;
      border-radius: 10px;
      text-align: center;
    }
    
    .loginButton:disabled {
      cursor: not-allowed;
      background-color: rgb(252, 173, 173);
    }
  
  </style>
  