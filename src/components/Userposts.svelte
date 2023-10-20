<script>
    import { onMount } from 'svelte';
    import BASE_URL from '../service/baseurl';  
    import { deletePost, singleUserPosts } from '../service/allapi';
    import { navigate } from 'svelte-routing/src/history';
    import { link } from 'svelte-routing';
    


    let userPosts = [];
  
    const userData = async () => {
      const id = localStorage.getItem("userId");
      try {
        const response = await singleUserPosts(id);
        userPosts = response.data;
      } catch (error) {
        console.error("An error occurred:", error);
      }
    };
  
    onMount(() => {
      userData();
    });

    console.log(userPosts);
  
    function formatDateToDaysAgo(dateString) {
      const date = new Date(dateString);
      const currentDate = new Date();
      const timeDifference = currentDate - date;
      const minutesDifference = Math.floor(timeDifference / (1000 * 60));
  
      if (minutesDifference < 60) {
        return `${minutesDifference} minutes ago`;
      } else if (minutesDifference < 1440) { // Less than 24 hours
        const hoursDifference = Math.floor(minutesDifference / 60);
        return `${hoursDifference} hours ago`;
      } else if (minutesDifference < 2880) { // Less than 48 hours
        return "Yesterday";
      } else {
        const daysDifference = Math.floor(timeDifference / (1000 * 60 * 60 * 24));
        return `${daysDifference} days ago`;
      }
    }
  
    const handleDeleteClick = async (id) => {
      const { data } = await deletePost(id);
      alert(data.message);
      navigate("/");
    };
  </script>
  
  {#if userPosts.length > 0}
  {#each userPosts as i (i._id)}
    <div class="home-post-container">
      <div class="post mt-5">
        <img class="postImg" src={`${BASE_URL}/uploads/${i.image}`} alt="" />

        <div class="postInfo">
          <span class="postTitle">
            <div  class="link">{i.title}</div>
          </span>
          <hr />
          <span class="postDate me-5 ms-auto">Posted : {formatDateToDaysAgo(i.date)}</span>
        </div>
        <p class="postDesc">{i.content}</p>

        <div class="button-container ms-auto ">
            <button on:click={(e) => navigate(`/edit/${i._id}`)} class="edit-button user-button text-success">Edit</button>
          <button
            class="delete-button ms-3 user-button text-danger"
            on:click={() => handleDeleteClick(i._id)}
          >
            Delete
          </button>
          <a href={`/view/${i._id}`}>
            <button class="view-button ms-3 user-button text-info">View</button>
          </a>
        </div>
      </div>
    </div>
  {/each}
{:else}
  <div class="userposts-container">
    <div class="userposts-noposts">
      <span class="userposts-headerTitleLg">You don't have any posts yet</span>
    </div>
  </div>
{/if}

<style>
    .userposts-noposts {
      background-image: url("https://i.postimg.cc/XJfGnJMF/9264885.jpg");
      width: 70%;
      height: 40rem;
      background-size: cover;
      background-repeat: no-repeat;
      align-items: center;
      margin-top: 48px;
    }
  
    .userposts-container {
      display: flex;
      justify-content: space-evenly;
    }
  
    .userposts-headerTitleLg {
      position: absolute;
      top: 17%;
      font-size: 40px;
    }
  
    .user-button {
      width: 80px;
      border: none;
      font-size: 20px;
    }

    .post {
        width: 30rem;
        margin: 0px 25px 40px 25px;
        display: flex;
        flex-direction: column;
    }

    .postImg {
        width: 100%;
        height: 280px;
        object-fit: cover;
        border-radius: 7px;
    }

    .postInfo {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .postTitle {
        font-family: "Josefin Sans", Arial, Helvetica, sans-serif;
        font-size: 24px;
        font-weight: 900;
        margin-top: 10px;
        cursor: pointer;
    }

    .postDate {
        font-family: "Lora", serif;
        font-style: italic;
        font-size: 21px;
        font-weight: 400;
        color: #5c5a5a;
    }

    .postDesc {
        font-family: "Varela Round", Arial, Helvetica, sans-serif;
        font-weight: 400;
        font-size: 14px;
        line-height: 24px;
        color: #444444;
        margin-top: 15px;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 4;
        -webkit-box-orient: vertical;
    }

    .home-post-container {
        display: flex;
        justify-content: space-evenly;
        flex-wrap: wrap;
        border-bottom: 1px solid rgb(229, 226, 226);
        width: 100%;
    }

    
   

    img {
        background-size: cover;
    }

  </style>
  
  